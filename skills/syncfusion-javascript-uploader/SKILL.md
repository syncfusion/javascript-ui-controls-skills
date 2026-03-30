---
name: syncfusion-javascript-uploader
description: Implement the Syncfusion Uploader component in TypeScript. This skill provides comprehensive guidance for file upload functionality, from basic setup to advanced features like validation, chunked uploads, and custom styling. Use this skill when adding file upload capability to TypeScript applications, handling file validation, implementing drag-and-drop uploads, or customizing the uploader appearance.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing the Uploader Component (JavaScript)

The Syncfusion Uploader component provides a robust, feature-rich file upload solution for TypeScript applications. It supports multiple upload modes, file validation, drag-and-drop, chunked uploads for large files, and extensive customization options.

## When to Use This Skill

- **Basic file uploads**: When you need to add simple file upload functionality to forms or applications
- **File validation**: When you need to validate file types, sizes, or custom conditions before upload
- **Large file handling**: When users need to upload large files using chunked or resumable uploads
- **User experience**: When you want to customize the uploader's appearance, themes, or provide file preview/resize capabilities
- **Advanced scenarios**: When you need JWT authentication, localization, form data binding, or complex upload workflows
- **Drag-and-drop**: When implementing modern UX patterns with drag-and-drop file uploads

## Component Overview

The Uploader component offers:
- **Multiple upload modes**: Async, chunked, and form-based uploads
- **Comprehensive validation**: File type, size, and count restrictions
- **Rich UI**: Customizable templates, themes, and styling options
- **Advanced features**: JWT authentication, localization, image preview/resize, progress tracking
- **Developer-friendly**: Full TypeScript support with type definitions, events, and methods

## Documentation Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation and package setup
- TypeScript module imports
- Basic component initialization
- HTML structure and CSS imports
- Your first upload example
- Key configuration options

### File Validation
📄 **Read:** [references/file-validation.md](references/file-validation.md)
- File type validation with allowedExtensions
- File size restrictions (minFileSize, maxFileSize)
- Maximum files count limiting
- Pre-upload validation strategies
- Custom validation logic
- Validation event handling

### Upload Modes & Features
📄 **Read:** [references/upload-modes.md](references/upload-modes.md)
- Async upload configuration
- Chunked upload for large files
- Drag-and-drop file handling
- Form submission with file upload
- File source options and events
- Event handling and callbacks

### Customization & Styling
📄 **Read:** [references/customization-and-styling.md](references/customization-and-styling.md)
- CSS customization and themes
- Button template customization
- Progress bar styling
- Appearance modifications
- RTL (Right-to-Left) support
- Responsive design patterns

### Advanced Features
📄 **Read:** [references/advanced-features.md](references/advanced-features.md)
- JWT authentication for secure uploads
- Localization and multi-language support
- File preview functionality
- Image resizing before upload
- Custom template development
- Form data binding
- File removal and cleanup handlers

### How-To Guides & Patterns
📄 **Read:** [references/how-to-guides.md](references/how-to-guides.md)
- Programmatic upload triggers
- Invisible/hidden uploader UI
- Additional data with uploads
- Confirmation dialogs for removal
- Custom button elements
- MIME type validation
- Convert images to binary format
- File size calculations
- Hide drop areas
- Open/edit uploaded files
- Sort selected files
- External button triggers
- Image validation on drop
- Accessibility (WAI-ARIA) support

### Troubleshooting & Patterns
📄 **Read:** [references/troubleshooting-and-examples.md](references/troubleshooting-and-examples.md)
- Common issues and solutions
- Browser compatibility notes
- Performance optimization techniques
- Edge cases and gotchas
- Real-world implementation patterns
- Debugging tips and techniques

---

## Quick Start Example

Here's a minimal TypeScript example to get started:

```typescript
import { Uploader } from '@syncfusion/ej2-inputs';

// Initialize Uploader component
const uploaderObj = new Uploader({
  asyncSettings: {
    saveUrl: 'https://services.syncfusion.com/js/production/api/FileUploader/Save',
    removeUrl: 'https://services.syncfusion.com/js/production/api/FileUploader/Remove'
  },
  allowedExtensions: '.jpg,.png,.pdf'
});

// Render to DOM element with id 'fileupload'
uploaderObj.appendTo('#fileupload');

// Handle file selection
uploaderObj.selected = (args: any) => {
  console.log('Files selected:', args.filesData);
};

// Handle upload completion
uploaderObj.success = (args: any) => {
  console.log('Upload successful:', args.responseText);
};
```

HTML element:
```html
<input type="file" id="fileupload" name="UploadFiles" />
```

---

## Common Patterns

### Pattern 1: Validated File Upload
Restrict uploads to specific file types and sizes:

```typescript
const uploader = new Uploader({
  asyncSettings: {
    saveUrl: 'https://api.example.com/upload',
    removeUrl: 'https://api.example.com/remove'
  },
  allowedExtensions: '.jpg,.jpeg,.png,.gif',
  minFileSize: 5000,      // 5 KB
  maxFileSize: 5242880,   // 5 MB
  maxFileCount: 5
});
uploader.appendTo('#fileupload');
```

### Pattern 2: Drag-and-Drop Upload
Enable modern file upload UX:

```typescript
const uploader = new Uploader({
  asyncSettings: {
    saveUrl: 'https://api.example.com/upload',
    removeUrl: 'https://api.example.com/remove'
  },
  dropEffect: 'Copy',
  multiple: true,
  allowedExtensions: '.pdf,.doc,.docx'
});
uploader.appendTo('#fileupload');

uploader.dropAreaLeave = () => {
  console.log('File dragged out');
};

uploader.drop = (args: any) => {
  console.log('Files dropped:', args.filesData);
};
```

### Pattern 3: Large File Chunked Upload
Handle large files with resumable chunks:

```typescript
const uploader = new Uploader({
  asyncSettings: {
    saveUrl: 'https://api.example.com/upload-chunk',
    removeUrl: 'https://api.example.com/remove',
    chunkSize: 5242880 // 5 MB chunks
  },
  allowedExtensions: '.iso,.zip,.rar',
  minFileSize: 26214400  // 25 MB minimum
});
uploader.appendTo('#fileupload');

uploader.chunkSuccess = (args: any) => {
  console.log('Chunk uploaded:', args.chunkIndex);
};

uploader.chunkFailure = (args: any) => {
  console.log('Chunk failed, retrying...');
};
```

### Pattern 4: File Preview Before Upload
Display images and details before uploading:

```typescript
const uploader = new Uploader({
  asyncSettings: {
    saveUrl: 'https://api.example.com/upload',
    removeUrl: 'https://api.example.com/remove'
  },
  allowedExtensions: '.jpg,.jpeg,.png'
});
uploader.appendTo('#fileupload');

uploader.selected = (args: any) => {
  // Render preview for each file
  args.filesData.forEach((file: any) => {
    const reader = new FileReader();
    reader.onload = (e: any) => {
      const preview = document.createElement('img');
      preview.src = e.target.result;
      preview.style.maxWidth = '100px';
      document.getElementById('preview')?.appendChild(preview);
    };
    reader.readAsDataURL(file.rawFile);
  });
};
```

---

## Key Properties & Events

### Essential Properties
| Property | Type | Purpose |
|----------|------|---------|
| `asyncSettings` | AsyncSettings | Configure upload endpoint and behavior |
| `allowedExtensions` | string | Comma-separated file extensions to allow |
| `minFileSize` | number | Minimum file size in bytes |
| `maxFileSize` | number | Maximum file size in bytes |
| `maxFileCount` | number | Maximum number of files to upload |
| `multiple` | boolean | Allow multiple file selection |
| `autoUpload` | boolean | Auto-upload on file selection |

### Key Events
| Event | Triggered | Use Case |
|-------|-----------|----------|
| `selected` | File(s) selected | Validate, preview, or modify files |
| `uploading` | Before upload starts | Log, add metadata, show progress |
| `success` | Upload completes successfully | Handle server response |
| `failure` | Upload fails | Retry logic, error handling |
| `remove` | File removed | Cleanup references |
| `dropAreaLeave` | Files dragged away | Visual feedback |
| `chunkSuccess` | Chunk uploaded (chunked mode) | Progress tracking |

---

## Common Use Cases

**Use Case 1: Product Image Upload**  
Validate images, resize before upload, show preview → See `advanced-features.md`

**Use Case 2: Document Management**  
Multiple file types, large file support, form integration → See `upload-modes.md` and `advanced-features.md`

**Use Case 3: User Avatar Upload**  
Single image, size validation, custom styling → See `getting-started.md` and `customization-and-styling.md`

**Use Case 4: Secure File Uploads**  
JWT authentication, server validation, error handling → See `advanced-features.md` and `troubleshooting-and-examples.md`

---

## Related Skills
- Implementing Form Validation
- Handling Async Operations in TypeScript
- Custom Event Handling

---

**Next Step:** Start with `references/getting-started.md` to learn installation and basic setup, or jump directly to the reference matching your use case.
