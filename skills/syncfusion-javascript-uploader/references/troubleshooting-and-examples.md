# Troubleshooting & Real-World Examples

Common issues, solutions, browser compatibility, and practical implementation patterns.

## Table of Contents
- [Common Issues & Solutions](#common-issues--solutions)
- [Browser Compatibility](#browser-compatibility)
- [Performance Optimization](#performance-optimization)
- [Edge Cases & Gotchas](#edge-cases--gotchas)
- [Real-World Patterns](#real-world-patterns)
- [Debugging Tips](#debugging-tips)

---

## Common Issues & Solutions

### Issue 1: Upload Button Not Appearing

**Problem:** The uploader component renders but the upload button is missing or inactive.

**Solution:**
```typescript
// Ensure asyncSettings is properly configured
const uploader = new Uploader({
  asyncSettings: {
    saveUrl: 'https://api.example.com/upload',      // Must be valid URL
    removeUrl: 'https://api.example.com/remove'     // Both are required
  }
});
uploader.appendTo('#fileupload');

// Verify HTML element exists
if (!document.getElementById('fileupload')) {
  console.error('Input element with id "fileupload" not found');
}

// Check browser console for errors
console.log('Uploader initialized:', uploader);
```

### Issue 2: Files Not Uploading

**Problem:** Files are selected but upload doesn't start.

**Solution:**
```typescript
// Check if autoUpload is disabled
const uploader = new Uploader({
  asyncSettings: {
    saveUrl: 'https://api.example.com/upload',
    removeUrl: 'https://api.example.com/remove'
  },
  autoUpload: true  // Explicitly set to true
});
uploader.appendTo('#fileupload');

// Monitor upload events for errors
uploader.uploading = (args: any) => {
  console.log('Uploading:', args.file.name);
};

uploader.failure = (args: any) => {
  console.error('Upload failed:', {
    statusCode: args.statusCode,
    error: args.error,
    responseText: args.responseText
  });
};

// For manual upload, ensure trigger exists
document.getElementById('uploadBtn')?.addEventListener('click', () => {
  console.log('Upload button clicked');
  uploader.upload(uploader.getFilesData());
});
```

### Issue 3: CORS Errors

**Problem:** "Access to XMLHttpRequest has been blocked by CORS policy"

**Solution:**

Server-side (Node.js/Express):
```typescript
import cors from 'cors';

app.use(cors({
  origin: 'https://your-app-domain.com',
  credentials: true,
  methods: ['GET', 'POST', 'DELETE'],
  allowedHeaders: ['Content-Type', 'Authorization']
}));

app.post('/api/FileUploader/Save', (req, res) => {
  // Handle upload
  res.json({ status: 'success' });
});
```

Client-side workaround:
```typescript
const uploader = new Uploader({
  asyncSettings: {
    saveUrl: 'https://same-origin-api.com/upload',  // Use same origin
    removeUrl: 'https://same-origin-api.com/remove',
    withCredentials: true
  }
});
uploader.appendTo('#fileupload');
```

### Issue 4: Validation Not Working

**Problem:** Files that should be rejected are still being accepted.

**Solution:**
```typescript
const uploader = new Uploader({
  asyncSettings: {
    saveUrl: 'https://api.example.com/upload',
    removeUrl: 'https://api.example.com/remove'
  },
  // Use correct format: extensions separated by comma, starting with dot
  allowedExtensions: '.jpg,.jpeg,.png,.pdf',  // ✓ Correct
  // allowedExtensions: 'jpg,jpeg,png,pdf',   // ✗ Wrong (missing dots)
  
  maxFileSize: 5242880,  // 5 MB in bytes, not MB
  minFileSize: 1024      // 1 KB in bytes
});
uploader.appendTo('#fileupload');

// Verify validation in events
uploader.selected = (args: any) => {
  console.log('Valid files:', args.filesData.length);
  console.log('Invalid files:', args.invalidFiles?.length || 0);
};
```

### Issue 5: Multiple Uploader Instances Conflict

**Problem:** Multiple uploaders on same page interfere with each other.

**Solution:**
```typescript
// Create separate uploader instances with unique IDs
class MultiUploader {
  private uploadersMap = new Map();

  constructor() {
    this.initializeUploaders();
  }

  private initializeUploaders(): void {
    const configs = [
      { id: 'profileImageUpload', title: 'Profile Picture' },
      { id: 'documentUpload', title: 'Documents' },
      { id: 'galleryUpload', title: 'Gallery' }
    ];

    configs.forEach(config => {
      const uploader = new Uploader({
        asyncSettings: {
          saveUrl: `https://api.example.com/upload/${config.id}`,
          removeUrl: `https://api.example.com/remove/${config.id}`,
          additionalData: {
            uploadType: config.id
          }
        }
      });

      uploader.appendTo(`#${config.id}`);
      this.uploadersMap.set(config.id, uploader);
    });
  }

  public getUploader(id: string): Uploader {
    return this.uploadersMap.get(id);
  }
}

const multiUploader = new MultiUploader();
```

---

## Browser Compatibility

### Supported Browsers

| Browser | Version | Status | Notes |
|---------|---------|--------|-------|
| Chrome | 90+ | ✓ Full support | Recommended |
| Firefox | 88+ | ✓ Full support | Fully compatible |
| Safari | 14+ | ✓ Full support | iOS support included |
| Edge | 90+ | ✓ Full support | Chromium-based |
| IE 11 | Any | ✗ Not supported | Use polyfills if needed |

### Browser Feature Detection

```typescript
// Check for File API support
const hasFileApi = typeof File !== 'undefined' && typeof FileReader !== 'undefined';

// Check for Drag & Drop support
const hasDragDrop = 'draggable' in document.createElement('div');

// Check for FormData support
const hasFormData = typeof FormData !== 'undefined';

// Initialize uploader based on capabilities
if (hasFileApi && hasFormData) {
  new Uploader({
    asyncSettings: {
      saveUrl: 'https://api.example.com/upload',
      removeUrl: 'https://api.example.com/remove'
    }
  }).appendTo('#fileupload');
} else {
  console.warn('File upload not supported in this browser');
  // Fallback: show form-based upload
}
```

### Mobile Browser Quirks

```typescript
// Mobile-specific handling
const isMobile = /iPhone|iPad|Android|Windows Phone/.test(navigator.userAgent);

if (isMobile) {
  const uploader = new Uploader({
    asyncSettings: {
      saveUrl: 'https://api.example.com/upload',
      removeUrl: 'https://api.example.com/remove'
    },
    multiple: false,  // Single file on mobile
    autoUpload: true, // Auto-upload for better UX
    maxFileCount: 1
  });
  uploader.appendTo('#fileupload');
}
```

---

## Performance Optimization

### Large File Handling

```typescript
// For files >50MB, use chunked uploads
const uploader = new Uploader({
  asyncSettings: {
    saveUrl: 'https://api.example.com/upload-chunk',
    removeUrl: 'https://api.example.com/remove',
    chunkSize: 5242880  // 5 MB chunks
  },
  minFileSize: 52428800  // Trigger chunking for files >50 MB
});
uploader.appendTo('#fileupload');

// Monitor chunk progress
let totalBytes = 0;
let uploadedBytes = 0;

uploader.chunkSuccess = (args: any) => {
  uploadedBytes += args.chunkSize;
  const progress = (uploadedBytes / totalBytes) * 100;
  console.log(`Upload progress: ${progress.toFixed(2)}%`);
};
```

### Memory Optimization

```typescript
// Avoid loading all files into memory at once
const uploader = new Uploader({
  asyncSettings: {
    saveUrl: 'https://api.example.com/upload',
    removeUrl: 'https://api.example.com/remove'
  },
  maxFileCount: 10  // Limit concurrent uploads
});
uploader.appendTo('#fileupload');

// Clear completed uploads from memory
uploader.success = (args: any) => {
  console.log('Upload complete:', args.file.name);
  // Remove from file list to free memory
  uploader.remove([args.file]);
};

// Garbage collection hint
uploader.actionComplete = () => {
  // Suggested for cleanup
  if ('gc' in window) {
    (window as any).gc();
  }
};
```

### Network Optimization

```typescript
// Compress before upload
class CompressedUploader {
  private uploader: Uploader;

  constructor() {
    this.uploader = new Uploader({
      asyncSettings: {
        saveUrl: 'https://api.example.com/upload',
        removeUrl: 'https://api.example.com/remove'
      }
    });
    this.uploader.appendTo('#fileupload');
    this.attachCompressionHandler();
  }

  private attachCompressionHandler(): void {
    this.uploader.uploading = async (args: any) => {
      if (args.file.type.includes('image')) {
        // Compress image before upload
        args.cancel = true;  // Cancel default upload
        
        const compressedBlob = await this.compressImage(args.file.rawFile);
        const compressedFile = new File([compressedBlob], args.file.name);
        
        // Resume with compressed file
        this.uploader.upload([{ rawFile: compressedFile, name: args.file.name }]);
      }
    };
  }

  private async compressImage(file: File): Promise<Blob> {
    return new Promise((resolve) => {
      const reader = new FileReader();
      reader.onload = (e: any) => {
        const img = new Image();
        img.onload = () => {
          const canvas = document.createElement('canvas');
          canvas.width = img.width * 0.8;
          canvas.height = img.height * 0.8;
          canvas.getContext('2d')?.drawImage(img, 0, 0, canvas.width, canvas.height);
          canvas.toBlob(resolve, file.type, 0.7);
        };
        img.src = e.target.result;
      };
      reader.readAsDataURL(file);
    });
  }
}
```

---

## Edge Cases & Gotchas

### Case 1: Empty File Upload

```typescript
uploader.selected = (args: any) => {
  // Filter out empty files
  args.filesData = args.filesData.filter((file: any) => file.size > 0);
  args.modifiedFilesData = args.filesData;

  if (args.modifiedFilesData.length === 0) {
    console.warn('All selected files are empty');
  }
};
```

### Case 2: Special Characters in Filenames

```typescript
uploader.selected = (args: any) => {
  args.filesData = args.filesData.map((file: any) => {
    // Sanitize filename
    const sanitized = file.name.replace(/[<>:"/\\|?*]/g, '_');
    return {
      ...file,
      name: sanitized
    };
  });
  args.modifiedFilesData = args.filesData;
};
```

### Case 3: Duplicate Files

```typescript
const uploadedFiles = new Set<string>();

uploader.success = (args: any) => {
  uploadedFiles.add(args.file.name);
};

uploader.selected = (args: any) => {
  // Prevent duplicate uploads
  args.filesData = args.filesData.filter((file: any) => 
    !uploadedFiles.has(file.name)
  );
  
  if (args.filesData.length < args.filesData.length) {
    console.warn('Duplicate files removed');
  }
  
  args.modifiedFilesData = args.filesData;
};
```

### Case 4: Network Interruption

```typescript
const maxRetries = 3;
const retryMap = new Map<string, number>();

uploader.failure = (args: any) => {
  const fileKey = args.file.name;
  const retries = retryMap.get(fileKey) || 0;

  if (retries < maxRetries) {
    console.log(`Retrying ${fileKey} (${retries + 1}/${maxRetries})`);
    retryMap.set(fileKey, retries + 1);
    
    // Wait 2 seconds, then retry
    setTimeout(() => {
      uploader.upload([args.file]);
    }, 2000);
  } else {
    console.error(`Upload failed after ${maxRetries} retries: ${fileKey}`);
    retryMap.delete(fileKey);
  }
};
```

---

## Real-World Patterns

### Pattern 1: User Avatar Upload with Crop

```typescript
class AvatarUploader {
  private uploader: Uploader;
  private croppedCanvas: HTMLCanvasElement | null = null;

  constructor() {
    this.uploader = new Uploader({
      asyncSettings: {
        saveUrl: 'https://api.example.com/upload-avatar',
        removeUrl: 'https://api.example.com/remove-avatar'
      },
      allowedExtensions: '.jpg,.jpeg,.png',
      maxFileSize: 5242880,
      autoUpload: false
    });
    this.uploader.appendTo('#avatarUpload');
    this.setupWorkflow();
  }

  private setupWorkflow(): void {
    // Step 1: Select image
    this.uploader.selected = (args: any) => {
      this.showCropDialog(args.filesData[0]);
    };

    // Step 3: Upload cropped image
    document.getElementById('confirmCropBtn')?.addEventListener('click', () => {
      if (this.croppedCanvas) {
        this.croppedCanvas.toBlob((blob: Blob | null) => {
          if (blob) {
            const croppedFile = new File([blob], 'avatar.jpg', { type: 'image/jpeg' });
            this.uploader.upload([{ rawFile: croppedFile, name: 'avatar.jpg' }]);
          }
        });
      }
    });
  }

  private showCropDialog(file: any): void {
    // Step 2: Show image and crop UI (implementation details)
    const reader = new FileReader();
    reader.onload = (e: any) => {
      const img = new Image();
      img.src = e.target.result;
      // Show crop interface
      document.getElementById('cropModal')?.style.display = 'block';
    };
    reader.readAsDataURL(file.rawFile);
  }
}

new AvatarUploader();
```

### Pattern 2: Batch Document Processing

```typescript
class BatchDocumentProcessor {
  private uploader: Uploader;
  private processQueue: any[] = [];

  constructor() {
    this.uploader = new Uploader({
      asyncSettings: {
        saveUrl: 'https://api.example.com/process-document',
        removeUrl: 'https://api.example.com/remove'
      },
      allowedExtensions: '.pdf,.docx,.xlsx',
      autoUpload: false,
      multiple: true
    });
    this.uploader.appendTo('#documentUpload');
    this.setupProcessing();
  }

  private setupProcessing(): void {
    document.getElementById('processBtn')?.addEventListener('click', () => {
      this.processQueue = this.uploader.getFilesData();
      this.processNext();
    });
  }

  private processNext(): void {
    if (this.processQueue.length === 0) {
      console.log('All documents processed');
      return;
    }

    const file = this.processQueue.shift();
    this.uploader.upload([file]);
  }

  private setupEventHandlers(): void {
    this.uploader.success = () => {
      this.processNext();  // Process next file
    };

    this.uploader.failure = () => {
      this.processNext();  // Skip failed file and continue
    };
  }
}

new BatchDocumentProcessor();
```

---

## Debugging Tips

### Enable Console Logging

```typescript
const uploader = new Uploader({
  asyncSettings: {
    saveUrl: 'https://api.example.com/upload',
    removeUrl: 'https://api.example.com/remove'
  }
});
uploader.appendTo('#fileupload');

// Log all events
uploader.selected = (args: any) => {
  console.log('[SELECTED]', {
    filesData: args.filesData,
    invalidFiles: args.invalidFiles
  });
};

uploader.uploading = (args: any) => {
  console.log('[UPLOADING]', {
    fileName: args.file.name,
    fileSize: args.file.size
  });
};

uploader.progress = (args: any) => {
  console.log('[PROGRESS]', `${args.percentComplete}%`);
};

uploader.success = (args: any) => {
  console.log('[SUCCESS]', args.responseText);
};

uploader.failure = (args: any) => {
  console.log('[FAILURE]', {
    statusCode: args.statusCode,
    error: args.error
  });
};
```

### Network Request Inspection

In browser DevTools:
1. Open **Network** tab
2. Select upload file
3. Look for POST request to `saveUrl`
4. Check:
   - Request headers (Authorization, Content-Type)
   - Request body (form data)
   - Response status and body
   - Timing information

### Browser Compatibility Testing

```bash
# Use BrowserStack or similar for cross-browser testing
# Or use local VMs/containers

# Test on various browsers and devices:
- Chrome (latest)
- Firefox (latest)
- Safari (latest)
- Edge (latest)
- iOS Safari
- Android Chrome
```

---

## Next Steps

- **For getting started**: Read [getting-started.md](getting-started.md)
- **For validation**: Read [file-validation.md](file-validation.md)
- **For advanced features**: Read [advanced-features.md](advanced-features.md)
