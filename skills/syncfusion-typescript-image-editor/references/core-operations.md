# Core Operations

## Table of Contents
- [Opening Images](#opening-images)
- [Supported File Formats](#supported-file-formats)
- [Saving and Exporting](#saving-and-exporting)
- [File Validation](#file-validation)
- [File Size Restrictions](#file-size-restrictions)
- [Common Patterns](#common-patterns)

## Opening Images

### Basic Image Loading

Load an image from a URL or file path:

```typescript
import { ImageEditor } from '@syncfusion/ej2-image-editor';

const imageEditor = new ImageEditor({ width: '550px', height: '330px' });
imageEditor.appendTo('#imageeditor');

// Open image from URL
imageEditor.open('https://example.com/images/photo.jpg');

// Open image from local path
imageEditor.open('assets/sample.png');
```

### Using Open Method

The `open()` method is the primary way to load images:

```typescript
// Simple open
imageEditor.open('image.jpg');

// Open with promise callback
imageEditor.open('image.jpg').then(() => {
    console.log('Image loaded successfully');
    const dimension = imageEditor.getImageDimension();
    console.log(`Image size: ${dimension.width} x ${dimension.height}`);
}).catch((error) => {
    console.error('Failed to load image:', error);
});
```

### Opening from File Input

Allow users to select images from their computer:

```typescript
const fileInput = document.getElementById('fileInput') as HTMLInputElement;
const openBtn = document.getElementById('openBtn') as HTMLElement;

openBtn.onclick = () => {
    fileInput.click();
};

fileInput.addEventListener('change', (event) => {
    const file = (event.target as HTMLInputElement).files[0];
    if (file) {
        const reader = new FileReader();
        reader.onload = (e) => {
            // Read file as data URL
            imageEditor.open(e.target.result as string);
        };
        reader.readAsDataURL(file);
    }
});
```

### Image Loaded Event

Respond to successful image loading:

```typescript
const imageEditor = new ImageEditor({
    width: '550px',
    height: '330px',
    imageLoaded: () => {
        console.log('Image loaded and ready to edit');
        // Perform operations after image loads
        const dimension = imageEditor.getImageDimension();
    }
});
```

### Getting Image Dimensions

Query the current image dimensions:

```typescript
// Get image dimension object
const dimension = imageEditor.getImageDimension();

console.log(`X: ${dimension.x}`);                    // Left position
console.log(`Y: ${dimension.y}`);                    // Top position
console.log(`Width: ${dimension.width}`);            // Image width
console.log(`Height: ${dimension.height}`);          // Image height
console.log(`ClientWidth: ${dimension.clientWidth}`);// Viewport width
console.log(`ClientHeight: ${dimension.clientHeight}`); // Viewport height
```

## Supported File Formats

### Image Format Support

The Image Editor supports the following image file formats for loading:

| Format | Extension | Support | Notes |
|--------|-----------|---------|-------|
| JPEG | `.jpg`, `.jpeg` | ✓ Full support | Commonly used, good compression |
| PNG | `.png` | ✓ Full support | Supports transparency |
| SVG | `.svg` | ✓ Full support | Vector format, scalable |
| WebP | `.webp` | ✓ Full support | Modern format, better compression |
| BMP | `.bmp` | ✓ Full support | Uncompressed bitmap format |

### Format Selection for Export

When saving/exporting, choose based on your needs:

```typescript
// Export as PNG (preserves transparency)
imageEditor.export('PNG');

// Export as JPEG (smaller file size)
imageEditor.export('JPEG');

// Export as WebP (modern compression)
imageEditor.export('WEBP');

// Export as SVG (vector format)
imageEditor.export('SVG');
```

## Saving and Exporting

### Save Method

Save the edited image:

```typescript
// Simple save (triggers browser download)
imageEditor.save();

// Save with specific filename
imageEditor.save('my-edited-image.png');
```

### Export to Different Formats

Export image in various formats:

```typescript
// Export to PNG
imageEditor.export('PNG', 'image.png');

// Export to JPEG with quality settings
imageEditor.export('JPEG', 'image.jpg', {
    quality: 0.95
});

// Export to WebP
imageEditor.export('WEBP', 'image.webp');

// Export to SVG
imageEditor.export('SVG', 'image.svg');
```

### Programmatic Save Handling

Intercept save events to handle data yourself:

```typescript
const imageEditor = new ImageEditor({
    width: '550px',
    height: '330px',
    beforeSave: (args) => {
        // Prevent default save behavior
        args.cancel = true;
        
        // Get image data
        const imageData = imageEditor.getImageData();
        
        // Send to server
        fetch('/api/save-image', {
            method: 'POST',
            body: JSON.stringify(imageData),
            headers: { 'Content-Type': 'application/json' }
        }).then(response => response.json())
          .then(data => console.log('Saved:', data));
    }
});
```

### Download vs Export

**Download:** Triggers browser download dialog  
**Export:** Returns image data in specified format

```typescript
// Download (immediate browser download)
imageEditor.save('output.png');

// Export (get data programmatically)
const imageData = imageEditor.export('PNG');
console.log(imageData);
```

## File Validation

### Input Validation

Validate files before opening:

```typescript
function validateImageFile(file: File): boolean {
    const validFormats = ['image/jpeg', 'image/png', 'image/svg+xml', 'image/webp', 'image/bmp'];
    
    // Check MIME type
    if (!validFormats.includes(file.type)) {
        console.error('Invalid file format. Supported: JPEG, PNG, SVG, WebP, BMP');
        return false;
    }
    
    // Check file size (max 10MB)
    const maxSize = 10 * 1024 * 1024; // 10MB
    if (file.size > maxSize) {
        console.error('File too large. Maximum size: 10MB');
        return false;
    }
    
    return true;
}

// Use in file input handler
fileInput.addEventListener('change', (event) => {
    const file = (event.target as HTMLInputElement).files[0];
    
    if (file && validateImageFile(file)) {
        const reader = new FileReader();
        reader.onload = (e) => {
            imageEditor.open(e.target.result as string);
        };
        reader.readAsDataURL(file);
    }
});
```

### Filename Validation

Ensure valid filenames for export:

```typescript
function sanitizeFilename(filename: string): string {
    // Remove invalid characters
    return filename
        .replace(/[^a-zA-Z0-9._-]/g, '_')  // Replace special chars
        .replace(/^\.+/, '')                // Remove leading dots
        .substring(0, 255);                 // Limit length
}

// Use when exporting
const originalName = 'user@photo (1).jpg';
const safeFilename = sanitizeFilename(originalName); // 'user_photo__1_.jpg'
imageEditor.save(safeFilename);
```

## File Size Restrictions

### Maximum File Sizes

While the Image Editor doesn't enforce strict limits, practical constraints apply:

```typescript
const MAX_FILE_SIZE = 50 * 1024 * 1024; // 50MB as practical limit

async function openImageWithValidation(file: File): Promise<void> {
    // Validate size
    if (file.size > MAX_FILE_SIZE) {
        throw new Error(`File exceeds maximum size of 50MB`);
    }
    
    // Compress if needed
    if (file.size > 5 * 1024 * 1024) { // > 5MB
        console.warn('Large file detected, performance may be affected');
    }
    
    // Proceed with opening
    const reader = new FileReader();
    reader.onload = (e) => {
        imageEditor.open(e.target.result as string);
    };
    reader.readAsDataURL(file);
}
```

### Canvas Size Limitations

Very large images may cause performance issues:

```typescript
// Check image dimensions after loading
const imageEditor = new ImageEditor({
    imageLoaded: () => {
        const dimension = imageEditor.getImageDimension();
        
        // Warn if very large
        const totalPixels = dimension.width * dimension.height;
        if (totalPixels > 50000000) { // > 50 megapixels
            console.warn('Very large image loaded, editing may be slow');
        }
    }
});
```

## Common Patterns

### Pattern: Open Image with Loading Indicator

```typescript
const loadingDiv = document.getElementById('loading');
const imageEditor = new ImageEditor({ width: '550px', height: '330px' });

function openImageWithLoading(url: string) {
    loadingDiv.style.display = 'block';
    
    imageEditor.open(url).then(() => {
        console.log('Image loaded');
        loadingDiv.style.display = 'none';
    }).catch((error) => {
        console.error('Load error:', error);
        loadingDiv.style.display = 'none';
        alert('Failed to load image');
    });
}

openImageWithLoading('assets/sample.jpg');
```

### Pattern: Save Multiple Formats

```typescript
function saveMultipleFormats(baseFilename: string) {
    const name = baseFilename.replace(/\.[^/.]+$/, ''); // Remove extension
    
    // Save as PNG (lossless)
    imageEditor.save(`${name}.png`);
    
    // Save as JPEG (compressed)
    imageEditor.export('JPEG', `${name}.jpg`);
    
    // Save as WebP (modern)
    imageEditor.export('WEBP', `${name}.webp`);
}

document.getElementById('saveAllBtn').onclick = () => {
    saveMultipleFormats('edited-image');
};
```

### Pattern: Upload to Server

```typescript
async function uploadImageToServer(filename: string): Promise<void> {
    // Get image data
    const imageData = imageEditor.export('PNG');
    
    // Create FormData
    const formData = new FormData();
    formData.append('file', imageData);
    formData.append('filename', filename);
    
    // Send to server
    const response = await fetch('/api/upload-image', {
        method: 'POST',
        body: formData
    });
    
    if (response.ok) {
        const result = await response.json();
        console.log('Upload successful:', result);
    } else {
        throw new Error('Upload failed');
    }
}

document.getElementById('uploadBtn').onclick = () => {
    uploadImageToServer('my-image.png');
};
```

### Pattern: Reload Original Image

```typescript
let originalImageUrl: string;

const imageEditor = new ImageEditor({
    imageLoaded: () => {
        // Store original URL
        originalImageUrl = imageEditor.imageSource;
    }
});

function reloadOriginal() {
    if (originalImageUrl) {
        imageEditor.open(originalImageUrl);
    }
}

document.getElementById('reloadBtn').onclick = reloadOriginal;
```

