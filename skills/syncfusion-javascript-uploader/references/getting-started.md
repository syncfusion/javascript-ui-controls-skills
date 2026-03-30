# Getting Started with the Uploader Component

This guide walks you through installation, setup, and creating your first file upload in TypeScript.

## Table of Contents
- [Installation](#installation)
- [Module Import](#module-import)
- [Basic HTML Structure](#basic-html-structure)
- [CSS Imports & Themes](#css-imports--themes)
- [Component Initialization](#component-initialization)
- [First Upload Example](#first-upload-example)
- [Configuration Options](#configuration-options)

---

## Installation

### Via npm Package

Install the Syncfusion inputs package (which includes the Uploader):

```bash
npm install @syncfusion/ej2-inputs --save
```

This installs the complete package with TypeScript definitions.

### Via CDN (Global Script)

Alternatively, use a CDN link in your HTML:

```html
<!-- Syncfusion Material Theme -->
<link href="https://cdn.syncfusion.com/ej2/material.css" rel="stylesheet" type="text/css">

<!-- Syncfusion Inputs Global Script -->
<script src="https://cdn.syncfusion.com/ej2/dist/ej2.min.js" type="text/javascript"></script>
```

---

## Module Import

### TypeScript/ES6 Import

For module-based projects (recommended for TypeScript):

```typescript
import { Uploader } from '@syncfusion/ej2-inputs';
```

If using a framework:

```typescript
// React
import { UploaderComponent } from '@syncfusion/ej2-react-inputs';

// Angular
import { UploaderModule } from '@syncfusion/ej2-angular-inputs';

// Vue
import { UploaderComponent } from '@syncfusion/ej2-vue-inputs';
```

### CDN Global Access

When using CDN, access via global namespace:

```typescript
const Uploader = (window as any).ej.inputs.Uploader;
```

---

## Basic HTML Structure

### Input Element

Create a file input element with an ID for the Uploader to target:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Syncfusion Uploader Example</title>
    <link href="styles/material.css" rel="stylesheet" type="text/css">
</head>
<body>
    <!-- File input for Uploader -->
    <input type="file" id="fileupload" name="UploadFiles" />
    
    <script src="app.ts"></script>
</body>
</html>
```

### Key Attributes
| Attribute | Purpose |
|-----------|---------|
| `id` | Target selector for Uploader initialization |
| `type="file"` | HTML5 file input (required) |
| `name` | Form submission name parameter |
| `multiple` | Allow multiple file selection (optional) |

---

## CSS Imports & Themes

### TypeScript Project (npm)

In your TypeScript main file or CSS import:

```typescript
// Import Syncfusion theme
import '@syncfusion/ej2-inputs/styles/material.css';

// Or other themes:
// import '@syncfusion/ej2-inputs/styles/bootstrap.css';
// import '@syncfusion/ej2-inputs/styles/bootstrap4.css';
// import '@syncfusion/ej2-inputs/styles/highcontrast.css';
// import '@syncfusion/ej2-inputs/styles/fabric.css';
```

### Available Themes
- `material.css` - Material Design (recommended)
- `bootstrap.css` - Bootstrap 3 styling
- `bootstrap4.css` - Bootstrap 4 styling
- `fabric.css` - Office Fabric design
- `highcontrast.css` - Accessibility-focused

---

## Component Initialization

### Basic Initialization

```typescript
import { Uploader } from '@syncfusion/ej2-inputs';
import '@syncfusion/ej2-inputs/styles/material.css';

// Create Uploader instance
const uploaderObj = new Uploader({
  asyncSettings: {
    saveUrl: 'https://services.syncfusion.com/js/production/api/FileUploader/Save',
    removeUrl: 'https://services.syncfusion.com/js/production/api/FileUploader/Remove'
  }
});

// Render to DOM element
uploaderObj.appendTo('#fileupload');
```

### asyncSettings Configuration

The `asyncSettings` object defines the server endpoints:

```typescript
asyncSettings: {
  // URL to handle file save/upload
  saveUrl: 'https://your-api.com/upload',
  
  // URL to handle file removal
  removeUrl: 'https://your-api.com/remove',
  
  // Additional parameters sent to server
  additionalData: {
    userId: '123',
    projectId: 'proj-456'
  }
}
```

### Server Response Format

The server should return JSON responses:

```json
// Save response
{
  "name": "document.pdf",
  "size": 1024,
  "status": "File uploaded successfully"
}

// Remove response
{
  "status": "File removed successfully"
}
```

---

## First Upload Example

Complete working example:

```typescript
import { Uploader, SelectedEventArgs, UploadingEventArgs } from '@syncfusion/ej2-inputs';
import '@syncfusion/ej2-inputs/styles/material.css';

class FileUploadApp {
  uploaderObj: Uploader;

  constructor() {
    this.uploaderObj = new Uploader({
      asyncSettings: {
        saveUrl: 'https://services.syncfusion.com/js/production/api/FileUploader/Save',
        removeUrl: 'https://services.syncfusion.com/js/production/api/FileUploader/Remove'
      },
      allowedExtensions: '.jpg,.jpeg,.png,.gif',
      maxFileSize: 5242880, // 5 MB
      multiple: true
    });

    this.uploaderObj.appendTo('#fileupload');
    this.attachEventHandlers();
  }

  private attachEventHandlers(): void {
    // When files are selected
    this.uploaderObj.selected = (args: SelectedEventArgs) => {
      console.log('Files selected:', args.filesData);
      
      args.filesData.forEach((file: any) => {
        console.log(`File: ${file.name}, Size: ${file.size} bytes`);
      });
    };

    // Before upload starts
    this.uploaderObj.uploading = (args: UploadingEventArgs) => {
      console.log(`Uploading: ${args.file.name}`);
    };

    // On successful upload
    this.uploaderObj.success = (args: any) => {
      console.log('Upload successful:', args.responseText);
    };

    // On upload failure
    this.uploaderObj.failure = (args: any) => {
      console.error('Upload failed:', args.error);
    };

    // When file is removed
    this.uploaderObj.removed = (args: any) => {
      console.log('File removed:', args.removedFilesData);
    };
  }
}

// Initialize when DOM is ready
window.addEventListener('DOMContentLoaded', () => {
  new FileUploadApp();
});
```

HTML:
```html
<!DOCTYPE html>
<html>
<head>
    <title>File Upload Example</title>
    <link href="styles/material.css" rel="stylesheet">
</head>
<body>
    <h2>Upload Files</h2>
    <input type="file" id="fileupload" name="UploadFiles" multiple />
    
    <script src="dist/app.js"></script>
</body>
</html>
```

---

## Configuration Options

### Frequently Used Properties

```typescript
new Uploader({
  // Async upload configuration (required)
  asyncSettings: {
    saveUrl: 'string',
    removeUrl: 'string'
  },
  
  // File type restrictions
  allowedExtensions: '.jpg,.png,.pdf',
  
  // Size limits
  minFileSize: 1000,          // 1 KB
  maxFileSize: 52428800,      // 50 MB
  
  // Selection limits
  maxFileCount: 10,           // Max 10 files
  multiple: true,             // Multiple file selection
  
  // Upload behavior
  autoUpload: true,           // Upload on selection
  dropEffect: 'Copy',         // Drag-drop visual effect
  
  // Template customization
  template: '<span>${name}</span>',
  
  // Localization
  locale: 'en-US'
});
```

### Event Handlers

Key events to attach handlers:

```typescript
uploader.selected = (args: SelectedEventArgs) => { };
uploader.uploading = (args: UploadingEventArgs) => { };
uploader.success = (args: any) => { };
uploader.failure = (args: any) => { };
uploader.removed = (args: any) => { };
uploader.onCleared = (args: any) => { };
```

---

## Next Steps

- **To validate files**: Read [file-validation.md](file-validation.md)
- **For advanced uploads**: Read [upload-modes.md](upload-modes.md)
- **To customize appearance**: Read [customization-and-styling.md](customization-and-styling.md)
- **For security/auth**: Read [advanced-features.md](advanced-features.md)
