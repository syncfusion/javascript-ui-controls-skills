# Upload Modes & Features

Explore different upload strategies and advanced file handling features.

## Table of Contents
- [Async Upload](#async-upload)
- [Chunked Upload](#chunked-upload)
- [Drag-and-Drop Upload](#drag-and-drop-upload)
- [Form Submission Upload](#form-submission-upload)
- [File Source Options](#file-source-options)
- [Event Handling](#event-handling)

---

## Async Upload

### Standard Async Upload

Asynchronous upload sends files to the server without page reload:

```typescript
import { Uploader } from '@syncfusion/ej2-inputs';

const uploader = new Uploader({
  asyncSettings: {
    saveUrl: 'https://api.example.com/upload',
    removeUrl: 'https://api.example.com/remove'
  },
  autoUpload: true  // Automatically upload on selection
});
uploader.appendTo('#fileupload');
```

### Manual Upload Control

Trigger upload programmatically:

```typescript
const uploader = new Uploader({
  asyncSettings: {
    saveUrl: 'https://api.example.com/upload',
    removeUrl: 'https://api.example.com/remove'
  },
  autoUpload: false  // Don't auto-upload
});
uploader.appendTo('#fileupload');

// Trigger upload from button click
document.getElementById('uploadBtn')?.addEventListener('click', () => {
  uploader.upload(uploader.getFilesData());
});

// Trigger upload for specific file
document.getElementById('uploadSingleBtn')?.addEventListener('click', () => {
  const firstFile = uploader.getFilesData()[0];
  uploader.upload([firstFile]);
});
```

### Additional Parameters

Send extra data with upload:

```typescript
const uploader = new Uploader({
  asyncSettings: {
    saveUrl: 'https://api.example.com/upload',
    removeUrl: 'https://api.example.com/remove',
    additionalData: {
      userId: '12345',
      projectId: 'proj-abc',
      timestamp: new Date().toISOString()
    }
  }
});
uploader.appendTo('#fileupload');
```

On the server, these parameters are included in the request body or query string.

---

## Chunked Upload

### Resumable Chunked Upload

Upload large files in chunks, with resume capability:

```typescript
import { Uploader, UploaderEventArgs } from '@syncfusion/ej2-inputs';

const uploader = new Uploader({
  asyncSettings: {
    saveUrl: 'https://api.example.com/upload-chunk',
    removeUrl: 'https://api.example.com/remove',
    chunkSize: 5242880  // 5 MB chunks
  },
  minFileSize: 26214400,  // Min 25 MB (trigger chunking)
  allowedExtensions: '.iso,.zip,.rar,.exe'
});
uploader.appendTo('#fileupload');

// Track chunk uploads
uploader.chunkSuccess = (args: any) => {
  const progress = (args.chunkIndex / args.totalChunk) * 100;
  console.log(`Chunk ${args.chunkIndex + 1} of ${args.totalChunk} uploaded (${progress.toFixed(0)}%)`);
};

// Handle chunk failure (auto-retry)
uploader.chunkFailure = (args: any) => {
  console.warn(`Chunk ${args.chunkIndex} failed, retrying...`);
  // The component handles retry automatically
};

// Track upload progress
uploader.progress = (args: any) => {
  console.log(`Overall progress: ${args.percentComplete}%`);
};
```

### Chunk Configuration

```typescript
// Common chunk sizes
const configs = {
  // Small chunks (better for poor connections)
  smallChunks: {
    chunkSize: 1048576  // 1 MB
  },
  
  // Medium chunks (balanced)
  mediumChunks: {
    chunkSize: 5242880  // 5 MB
  },
  
  // Large chunks (faster for good connections)
  largeChunks: {
    chunkSize: 10485760 // 10 MB
  }
};
```

### Server-Side Chunk Handling

Server receives multiple requests for the same file:

```typescript
// Typical request structure:
// POST /upload-chunk
// Body contains:
// - name: filename
// - chunkIndex: current chunk number (0-based)
// - totalChunk: total number of chunks
// - file: binary chunk data

// Server combines chunks:
// 1. Store chunk in temp location
// 2. When chunkIndex == totalChunk - 1, combine all chunks
// 3. Return success with combined file location
```

---

## Drag-and-Drop Upload

### Enable Drag-and-Drop

Create drop zone for files:

```typescript
const uploader = new Uploader({
  asyncSettings: {
    saveUrl: 'https://api.example.com/upload',
    removeUrl: 'https://api.example.com/remove'
  },
  dropEffect: 'Copy',  // Visual effect during drag
  multiple: true
});
uploader.appendTo('#fileupload');
```

### Drop Area Events

```typescript
uploader.dropAreaLeave = () => {
  console.log('Files dragged away from drop zone');
  // Remove visual feedback
};

uploader.drop = (args: any) => {
  console.log('Files dropped:', args.filesData);
  // Add visual feedback
};

uploader.dropAreaEnter = () => {
  console.log('Files dragged over drop zone');
  // Highlight drop zone
};
```

### Custom Drop Zone

Create custom drop area outside the input:

```typescript
const uploader = new Uploader({
  asyncSettings: {
    saveUrl: 'https://api.example.com/upload',
    removeUrl: 'https://api.example.com/remove'
  }
});
uploader.appendTo('#fileupload');

// Reference external drop zone
const dropZone = document.getElementById('dropZone') as HTMLElement;

if (dropZone) {
  dropZone.addEventListener('dragover', (e: DragEvent) => {
    e.preventDefault();
    dropZone.classList.add('drag-over');
  });

  dropZone.addEventListener('dragleave', () => {
    dropZone.classList.remove('drag-over');
  });

  dropZone.addEventListener('drop', (e: DragEvent) => {
    e.preventDefault();
    dropZone.classList.remove('drag-over');

    if (e.dataTransfer?.files) {
      // Set files to uploader
      uploader.upload(Array.from(e.dataTransfer.files) as any);
    }
  });
}
```

HTML:
```html
<div id="dropZone" style="border: 2px dashed #ccc; padding: 20px;">
  Drop files here
</div>
<input type="file" id="fileupload" name="UploadFiles" />
```

CSS:
```css
#dropZone.drag-over {
  border-color: #0078d4;
  background-color: #f0f8ff;
}
```

---

## Form Submission Upload

### Upload with Form Data

Combine file upload with form submission:

```typescript
import { Uploader, UploadingEventArgs } from '@syncfusion/ej2-inputs';

const uploader = new Uploader({
  asyncSettings: {
    saveUrl: 'https://api.example.com/upload',
    removeUrl: 'https://api.example.com/remove'
  }
});
uploader.appendTo('#fileupload');

// Handle form submission
document.getElementById('submitBtn')?.addEventListener('click', (e: Event) => {
  e.preventDefault();

  // Get form data
  const form = document.getElementById('myForm') as HTMLFormElement;
  const formData = new FormData(form);

  // Get uploaded files
  const files = uploader.getFilesData();
  
  // Add files to form data
  files.forEach((file: any, index: number) => {
    formData.append(`files[${index}]`, file.rawFile);
  });

  // Submit to server
  fetch('https://api.example.com/submit', {
    method: 'POST',
    body: formData
  })
  .then(response => response.json())
  .then(data => {
    console.log('Form submitted successfully:', data);
  })
  .catch(error => console.error('Submission error:', error));
});
```

HTML:
```html
<form id="myForm">
  <input type="text" name="userName" placeholder="User name" required />
  <input type="email" name="email" placeholder="Email" required />
  
  <input type="file" id="fileupload" name="UploadFiles" multiple />
  
  <button type="button" id="submitBtn">Submit</button>
</form>
```

---

## File Source Options

### Getting File Information

Access file data programmatically:

```typescript
const uploader = new Uploader({
  asyncSettings: {
    saveUrl: 'https://api.example.com/upload',
    removeUrl: 'https://api.example.com/remove'
  }
});
uploader.appendTo('#fileupload');

// Get all files
const allFiles = uploader.getFilesData();
console.log('Total files:', allFiles.length);

allFiles.forEach((file: any) => {
  console.log({
    name: file.name,
    size: file.size,
    type: file.type,
    lastModified: file.lastModified,
    rawFile: file.rawFile
  });
});
```

### Reading File Content

Access file content before upload:

```typescript
const uploader = new Uploader({
  asyncSettings: {
    saveUrl: 'https://api.example.com/upload',
    removeUrl: 'https://api.example.com/remove'
  }
});
uploader.appendTo('#fileupload');

uploader.selected = (args: any) => {
  args.filesData.forEach((file: any) => {
    const reader = new FileReader();

    // For text files
    if (file.type.includes('text') || file.name.endsWith('.json')) {
      reader.onload = (e: any) => {
        const content = e.target.result;
        console.log(`${file.name} content:`, content);
      };
      reader.readAsText(file.rawFile);
    }

    // For images
    if (file.type.includes('image')) {
      reader.onload = (e: any) => {
        const dataUrl = e.target.result;
        const img = document.createElement('img');
        img.src = dataUrl;
        img.style.maxWidth = '100px';
        document.getElementById('preview')?.appendChild(img);
      };
      reader.readAsDataURL(file.rawFile);
    }

    // For binary files (checksums)
    reader.onload = (e: any) => {
      const arrayBuffer = e.target.result;
      console.log(`${file.name} size: ${arrayBuffer.byteLength} bytes`);
    };
    reader.readAsArrayBuffer(file.rawFile);
  });
};
```

---

## Event Handling

### Complete Event Lifecycle

```typescript
const uploader = new Uploader({
  asyncSettings: {
    saveUrl: 'https://api.example.com/upload',
    removeUrl: 'https://api.example.com/remove'
  }
});
uploader.appendTo('#fileupload');

// 1. Files selected
uploader.selected = (args: any) => {
  console.log('1. Files selected:', args.filesData.length);
};

// 2. File upload starts
uploader.uploading = (args: any) => {
  console.log('2. Uploading:', args.file.name);
  // Can cancel: args.cancel = true;
};

// 3. Upload progress
uploader.progress = (args: any) => {
  console.log(`3. Progress: ${args.percentComplete}%`);
};

// 4. Upload succeeds
uploader.success = (args: any) => {
  console.log('4. Success:', args.responseText);
};

// 5. Upload fails
uploader.failure = (args: any) => {
  console.log('5. Failure:', args.error);
};

// 6. File removed
uploader.removed = (args: any) => {
  console.log('6. Removed:', args.removedFilesData);
};

// 7. All uploads complete
uploader.actionComplete = (args: any) => {
  console.log('7. All uploads complete');
};
```

### Pausing/Resuming Uploads

```typescript
const uploader = new Uploader({
  asyncSettings: {
    saveUrl: 'https://api.example.com/upload',
    removeUrl: 'https://api.example.com/remove'
  },
  autoUpload: false
});
uploader.appendTo('#fileupload');

// Start upload
document.getElementById('startBtn')?.addEventListener('click', () => {
  uploader.upload(uploader.getFilesData());
});

// Pause upload
document.getElementById('pauseBtn')?.addEventListener('click', () => {
  uploader.pause(uploader.getFilesData());
});

// Resume upload
document.getElementById('resumeBtn')?.addEventListener('click', () => {
  uploader.resume(uploader.getFilesData());
});

// Cancel upload
document.getElementById('cancelBtn')?.addEventListener('click', () => {
  uploader.cancel(uploader.getFilesData());
});
```

---

## Next Steps

- **For validation**: Read [file-validation.md](file-validation.md)
- **For customization**: Read [customization-and-styling.md](customization-and-styling.md)
- **For advanced features**: Read [advanced-features.md](advanced-features.md)
