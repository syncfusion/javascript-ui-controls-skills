# File Validation in the Uploader Component

Comprehensive guide to validating files by type, size, and count before upload.

## Table of Contents
- [File Type Validation](#file-type-validation)
- [File Size Validation](#file-size-validation)
- [File Count Restrictions](#file-count-restrictions)
- [Validation Before Upload](#validation-before-upload)
- [Custom Validation Logic](#custom-validation-logic)
- [Validation Events](#validation-events)
- [Error Handling](#error-handling)

---

## File Type Validation

### allowedExtensions Property

Restrict upload to specific file types using comma-separated extensions:

```typescript
import { Uploader } from '@syncfusion/ej2-inputs';

const uploader = new Uploader({
  asyncSettings: {
    saveUrl: 'https://api.example.com/upload',
    removeUrl: 'https://api.example.com/remove'
  },
  allowedExtensions: '.jpg,.jpeg,.png,.gif,.bmp'
});
uploader.appendTo('#fileupload');
```

### Common File Type Patterns

```typescript
// Images only
allowedExtensions: '.jpg,.jpeg,.png,.gif,.webp,.bmp'

// Documents
allowedExtensions: '.pdf,.doc,.docx,.xls,.xlsx,.ppt,.pptx'

// Archives
allowedExtensions: '.zip,.rar,.7z,.tar,.gz'

// Media
allowedExtensions: '.mp4,.avi,.mov,.mkv,.webm,.mp3,.wav'

// Mixed types
allowedExtensions: '.jpg,.png,.pdf,.doc,.docx'
```

### How File Type Validation Works

1. **Extension matching**: Compares file extension against allowedExtensions
2. **Case-insensitive**: `.JPG` and `.jpg` both match
3. **Applied on select**: Validation occurs when files are selected
4. **Applied on drop**: Validation also occurs during drag-and-drop
5. **HTML5 accept attribute**: Optionally add to HTML input:

```html
<input type="file" 
       id="fileupload" 
       name="UploadFiles"
       accept=".jpg,.jpeg,.png,.gif" />
```

### Validation Result

Invalid file types are:
- Removed from the selected files list
- Not uploaded to the server
- Logged in the validation event

```typescript
uploader.selected = (args: any) => {
  console.log('Valid files:', args.filesData);
  console.log('Invalid files:', args.invalidFiles);
};
```

---

## File Size Validation

### Size Validation Properties

```typescript
const uploader = new Uploader({
  asyncSettings: {
    saveUrl: 'https://api.example.com/upload',
    removeUrl: 'https://api.example.com/remove'
  },
  // Minimum file size (bytes)
  minFileSize: 5000,      // 5 KB
  
  // Maximum file size (bytes)
  maxFileSize: 5242880    // 5 MB
});
uploader.appendTo('#fileupload');
```

### Byte Size Reference

| Size | Bytes |
|------|-------|
| 1 KB | 1024 |
| 1 MB | 1048576 |
| 5 MB | 5242880 |
| 10 MB | 10485760 |
| 25 MB | 26214400 |
| 50 MB | 52428800 |
| 100 MB | 104857600 |

### Common Size Patterns

```typescript
// Images: 100 KB - 10 MB
minFileSize: 102400,
maxFileSize: 10485760

// Documents: 50 KB - 25 MB
minFileSize: 51200,
maxFileSize: 26214400

// Videos: 1 MB - 500 MB
minFileSize: 1048576,
maxFileSize: 524288000

// No minimum, 5 MB maximum
minFileSize: 0,
maxFileSize: 5242880
```

### Size Validation in Events

Access file size information:

```typescript
uploader.selected = (args: any) => {
  args.filesData.forEach((file: any) => {
    const sizeInKB = (file.size / 1024).toFixed(2);
    const sizeInMB = (file.size / 1048576).toFixed(2);
    
    console.log(`${file.name}: ${sizeInKB} KB (${sizeInMB} MB)`);
  });
};
```

---

## File Count Restrictions

### maxFileCount Property

Limit the total number of files that can be uploaded:

```typescript
const uploader = new Uploader({
  asyncSettings: {
    saveUrl: 'https://api.example.com/upload',
    removeUrl: 'https://api.example.com/remove'
  },
  maxFileCount: 5,    // Maximum 5 files
  multiple: true      // Allow multiple selection
});
uploader.appendTo('#fileupload');
```

### Enforcing Count in Events

To have more control over file count validation:

```typescript
const uploader = new Uploader({
  asyncSettings: {
    saveUrl: 'https://api.example.com/upload',
    removeUrl: 'https://api.example.com/remove'
  },
  multiple: true
});
uploader.appendTo('#fileupload');

uploader.selected = (args: any) => {
  const maxAllowed = 3;
  
  if (args.filesData.length > maxAllowed) {
    // Keep only first N files
    args.filesData = args.filesData.slice(0, maxAllowed);
    args.modifiedFilesData = args.filesData;
    
    console.warn(`Only ${maxAllowed} files allowed. Excess files removed.`);
  }
};
```

---

## Validation Before Upload

### Pre-Upload Validation Strategy

Validate files before sending to server:

```typescript
class ValidatedUploader {
  private uploader: Uploader;
  private allowedExtensions = ['.jpg', '.png', '.pdf'];
  private maxSize = 5242880; // 5 MB
  private maxFiles = 10;

  constructor() {
    this.uploader = new Uploader({
      asyncSettings: {
        saveUrl: 'https://api.example.com/upload',
        removeUrl: 'https://api.example.com/remove'
      },
      allowedExtensions: this.allowedExtensions.join(','),
      maxFileSize: this.maxSize,
      maxFileCount: this.maxFiles,
      autoUpload: false  // Manual upload control
    });

    this.uploader.appendTo('#fileupload');
    this.attachValidation();
  }

  private attachValidation(): void {
    this.uploader.selected = (args: any) => {
      const validationResults = this.validateFiles(args.filesData);
      
      if (!validationResults.isValid) {
        console.error('Validation failed:', validationResults.errors);
        // Prevent upload
        args.filesData = [];
      } else {
        console.log('All files valid, ready to upload');
      }
    };
  }

  private validateFiles(files: any[]): any {
    const errors: string[] = [];
    let isValid = true;

    files.forEach((file: any) => {
      // Check extension
      const extension = this.getFileExtension(file.name);
      if (!this.allowedExtensions.includes(extension.toLowerCase())) {
        errors.push(`${file.name}: Invalid file type`);
        isValid = false;
      }

      // Check size
      if (file.size > this.maxSize) {
        const sizeInMB = (file.size / 1048576).toFixed(2);
        errors.push(`${file.name}: Exceeds max size (${sizeInMB} MB)`);
        isValid = false;
      }

      // Check minimum size (prevent empty files)
      if (file.size === 0) {
        errors.push(`${file.name}: Empty file not allowed`);
        isValid = false;
      }
    });

    // Check total file count
    if (files.length > this.maxFiles) {
      errors.push(`Exceeds max file count (${this.maxFiles})`);
      isValid = false;
    }

    return { isValid, errors };
  }

  private getFileExtension(filename: string): string {
    return filename.substring(filename.lastIndexOf('.'));
  }

  public upload(): void {
    // Manually trigger upload after validation
    this.uploader.upload(this.uploader.getFilesData());
  }
}

// Usage
const app = new ValidatedUploader();
```

---

## Custom Validation Logic

### Custom Validation in Selected Event

Implement custom validation rules:

```typescript
const uploader = new Uploader({
  asyncSettings: {
    saveUrl: 'https://api.example.com/upload',
    removeUrl: 'https://api.example.com/remove'
  }
});
uploader.appendTo('#fileupload');

// Custom validation rules
uploader.selected = (args: any) => {
  args.filesData = args.filesData.filter((file: any) => {
    // Rule 1: No files starting with specific character
    if (file.name.startsWith('_')) {
      console.warn(`Rejected: ${file.name} (starts with underscore)`);
      return false;
    }

    // Rule 2: No spaces in filename
    if (file.name.includes(' ')) {
      console.warn(`Rejected: ${file.name} (contains spaces)`);
      return false;
    }

    // Rule 3: Filename length limit
    if (file.name.length > 50) {
      console.warn(`Rejected: ${file.name} (filename too long)`);
      return false;
    }

    return true;
  });

  args.modifiedFilesData = args.filesData;
};
```

### Async Validation (Server-Side Check)

Validate against server before upload:

```typescript
uploader.uploading = async (args: any) => {
  // Check with server before allowing upload
  try {
    const response = await fetch('https://api.example.com/validate-file', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        fileName: args.file.name,
        fileSize: args.file.size,
        fileType: args.file.type
      })
    });

    const result = await response.json();
    
    if (!result.isAllowed) {
      // Cancel upload
      args.cancel = true;
      console.error(`Upload blocked: ${result.reason}`);
    }
  } catch (error) {
    console.error('Validation error:', error);
  }
};
```

---

## Validation Events

### Available Validation Events

```typescript
// When files are selected/dropped
uploader.selected = (args: any) => {
  console.log('Selected files:', args.filesData);
  console.log('Invalid files:', args.invalidFiles);
};

// Before upload starts
uploader.uploading = (args: any) => {
  console.log('File about to upload:', args.file);
  // Can cancel: args.cancel = true;
};

// Upload progress
uploader.progress = (args: any) => {
  console.log('Upload progress:', args.percentComplete + '%');
};

// Upload success
uploader.success = (args: any) => {
  console.log('Upload successful:', args.responseText);
};

// Upload failure
uploader.failure = (args: any) => {
  console.error('Upload failed:', args.error);
};

// When file is removed
uploader.removed = (args: any) => {
  console.log('Files removed:', args.removedFilesData);
};
```

---

## Error Handling

### Common Validation Errors

```typescript
uploader.selected = (args: any) => {
  // Check for invalid files
  if (args.invalidFiles && args.invalidFiles.length > 0) {
    args.invalidFiles.forEach((file: any) => {
      if (file.error === 'File type not allowed') {
        console.error(`${file.name}: Invalid file type`);
      } else if (file.error === 'File size is too large') {
        console.error(`${file.name}: File too large`);
      } else if (file.error === 'File size is too small') {
        console.error(`${file.name}: File too small`);
      }
    });
  }
};
```

### Handling Upload Errors

```typescript
uploader.failure = (args: any) => {
  const errorCode = args.statusCode;
  const errorMessage = args.error;

  switch (errorCode) {
    case 400:
      console.error('Bad request:', errorMessage);
      break;
    case 403:
      console.error('Access forbidden:', errorMessage);
      break;
    case 413:
      console.error('File too large:', errorMessage);
      break;
    case 415:
      console.error('Unsupported file type:', errorMessage);
      break;
    case 500:
      console.error('Server error:', errorMessage);
      break;
    default:
      console.error(`Upload failed (${errorCode}):`, errorMessage);
  }
};
```

---

## Next Steps

- **For upload modes**: Read [upload-modes.md](upload-modes.md)
- **For security**: Read [advanced-features.md](advanced-features.md)
- **For customization**: Read [customization-and-styling.md](customization-and-styling.md)
