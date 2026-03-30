# Image Manipulation

## Table of Contents
- [Zooming](#zooming)
- [Panning](#panning)
- [Resizing Images](#resizing-images)
- [Rotation](#rotation)
- [Flipping](#flipping)
- [Straightening](#straightening)
- [Image Dimensions](#image-dimensions)
- [Common Patterns](#common-patterns)

## Zooming

### Zoom Levels and Percentages

Control the magnification level of the image:

```typescript
import { ImageEditor } from '@syncfusion/ej2-image-editor';

const imageEditor = new ImageEditor({ width: '550px', height: '330px' });
imageEditor.appendTo('#imageeditor');

// Set zoom level (1.0 = 100%, 2.0 = 200%, 0.5 = 50%)
imageEditor.zoom(1.5);       // Zoom to 150%
imageEditor.zoom(2);         // Zoom to 200%
imageEditor.zoom(1);         // Reset to 100% (fit image)
imageEditor.zoom(0.5);       // Zoom out to 50%
```

### Zoom In/Out Methods

Increment or decrement zoom level:

```typescript
// Get current zoom level
const currentZoom = imageEditor.zoomLevel;
console.log(`Current zoom: ${currentZoom}%`);

// Zoom in (increase zoom)
imageEditor.zoom(currentZoom + 1);

// Zoom out (decrease zoom)
imageEditor.zoom(Math.max(0.5, currentZoom - 1));
```

### Toolbar Zoom Buttons

Enable zoom controls in toolbar:

```typescript
const imageEditor = new ImageEditor({
    toolbar: ['ZoomIn', 'ZoomOut', 'ZoomReset', 'FitToWidth', 'FitToHeight'],
    enableZoom: true
});
```

### Mouse Wheel Zooming

Users can zoom using Ctrl + mouse wheel:

```typescript
// Mouse wheel zooming is enabled by default
// Ctrl + scroll wheel: zoom in/out
// This works automatically with zoom enabled
```

### Keyboard Shortcuts for Zoom

Users can zoom with keyboard:

```
Ctrl + '+' key  : Zoom in
Ctrl + '-' key  : Zoom out
```

## Panning

### Pan an Image

Move the image within the editor canvas:

```typescript
// Panning is enabled when:
// 1. Image is larger than editor canvas (zoomed in)
// 2. Selection is applied for cropping

// Users pan by clicking and dragging the image
// This works automatically after zooming
```

### When Panning Is Available

Panning activates in these scenarios:

```typescript
const imageEditor = new ImageEditor({ width: '550px', height: '330px' });
imageEditor.appendTo('#imageeditor');

// Scenario 1: Image larger than canvas (zoom in first)
imageEditor.zoom(2); // Now user can pan

// Scenario 2: During crop selection
imageEditor.drawImage('crop');
// Now user can pan to move image within crop region
```

### Programmatic Panning

```typescript
// Pan by coordinates (not directly supported)
// Instead, users pan manually by dragging
// Or use zoom + canvas scroll

// Alternative: Use scroll on canvas
const canvas = imageEditor.upperCanvas;
canvas.scrollLeft = 100;  // Scroll horizontally
canvas.scrollTop = 50;    // Scroll vertically
```

## Resizing Images

### Scale Image Dimensions

Resize the image to specific dimensions:

```typescript
// Resize using resize method
imageEditor.resize(800, 600);  // Resize to 800x600 pixels

// Resize with aspect ratio preservation
const originalDimension = imageEditor.getImageDimension();
const newWidth = 600;
const aspectRatio = originalDimension.width / originalDimension.height;
const newHeight = newWidth / aspectRatio;

imageEditor.resize(newWidth, newHeight);
```

### Percentage-Based Resizing

Scale image by percentage:

```typescript
function resizeByPercentage(percent: number) {
    const dimension = imageEditor.getImageDimension();
    const newWidth = dimension.width * (percent / 100);
    const newHeight = dimension.height * (percent / 100);
    
    imageEditor.resize(newWidth, newHeight);
}

resizeByPercentage(50);   // Resize to 50%
resizeByPercentage(150);  // Resize to 150%
```

### Constraint Resizing

Maintain aspect ratio when resizing:

```typescript
function resizeWithConstraint(maxWidth: number, maxHeight: number) {
    const dimension = imageEditor.getImageDimension();
    let newWidth = dimension.width;
    let newHeight = dimension.height;
    
    // Scale down if larger than max
    if (newWidth > maxWidth) {
        newWidth = maxWidth;
        newHeight = (dimension.height / dimension.width) * newWidth;
    }
    
    if (newHeight > maxHeight) {
        newHeight = maxHeight;
        newWidth = (dimension.width / dimension.height) * newHeight;
    }
    
    imageEditor.resize(newWidth, newHeight);
}

// Resize to fit within 1000x1000
resizeWithConstraint(1000, 1000);
```

## Rotation

### Rotate Image

Rotate image by 90-degree increments or custom angles:

```typescript
// Rotate 90 degrees clockwise
imageEditor.rotate(90);

// Rotate 180 degrees
imageEditor.rotate(180);

// Rotate 270 degrees (or -90)
imageEditor.rotate(270);

// Rotate back to 0 degrees
imageEditor.rotate(0);

// Rotate by custom angle
imageEditor.rotate(45);
```

### Using Toolbar Rotation

Enable rotation controls:

```typescript
const imageEditor = new ImageEditor({
    toolbar: ['Rotate'],
    allowRotation: true
});
```

### Accessing Rotation Methods

```typescript
// Get current rotation angle
const rotation = imageEditor.rotation;
console.log(`Current rotation: ${rotation}°`);

// Rotate left (counter-clockwise)
imageEditor.rotate((imageEditor.rotation - 90) % 360);

// Rotate right (clockwise)
imageEditor.rotate((imageEditor.rotation + 90) % 360);
```

### Rotate with Context Menu

Users can right-click for rotation options:

```typescript
// This is automatic with toolbar enabled
// Right-click menu shows: Rotate, Flip Left, Flip Right options
```

## Flipping

### Flip Horizontally

Mirror image left-to-right:

```typescript
// Flip horizontally (mirror image)
imageEditor.flip('Horizontal');

// Flip again to restore original orientation
imageEditor.flip('Horizontal');
```

### Flip Vertically

Mirror image top-to-bottom:

```typescript
// Flip vertically (upside down)
imageEditor.flip('Vertical');

// Flip again to restore original orientation
imageEditor.flip('Vertical');
```

### Toolbar Flip Options

Enable flip controls:

```typescript
const imageEditor = new ImageEditor({
    toolbar: ['Flip'],
    // Users see options: Flip Left, Flip Right, Flip Vertical
});
```

### Combined Transformations

Combine rotation and flipping:

```typescript
// Rotate 90 degrees then flip
imageEditor.rotate(90);
imageEditor.flip('Horizontal');

// Result: Image rotated and mirrored
```

## Straightening

### Straighten Crooked Images

Use straighten slider to correct image tilt:

```typescript
// Straightening is accessed via toolbar
// Shows a slider for angle adjustment: typically -45° to +45°

const imageEditor = new ImageEditor({
    toolbar: ['Straighten'] // Enable straighten tool
});
```

### Straighten with Context Menu

```typescript
// Via toolbar:
// 1. User clicks Crop button
// 2. In crop context toolbar, clicks Straighten
// 3. Slider appears to adjust tilt angle
// 4. Applies straightening to image
```

### Programmatic Straightening

```typescript
// Note: Straightening is primarily UI-driven
// Manual implementation requires canvas manipulation
// Recommended to use toolbar straightening feature

// Get straighten angle from UI
const straightenAngle = imageEditor.straightenAngle || 0;
```

## Image Dimensions

### Get Current Image Size

Retrieve image dimensions and position:

```typescript
const dimension = imageEditor.getImageDimension();

console.log(`Image width: ${dimension.width}px`);
console.log(`Image height: ${dimension.height}px`);
console.log(`Left position (X): ${dimension.x}px`);
console.log(`Top position (Y): ${dimension.y}px`);
console.log(`Canvas width: ${dimension.clientWidth}px`);
console.log(`Canvas height: ${dimension.clientHeight}px`);
```

### Query Canvas Dimensions

```typescript
// Get editor canvas properties
const upperCanvas = imageEditor.upperCanvas;
const canvasWidth = upperCanvas.width;
const canvasHeight = upperCanvas.height;

// Get viewport dimensions
const viewportWidth = imageEditor.upperCanvas.offsetWidth;
const viewportHeight = imageEditor.upperCanvas.offsetHeight;
```

### Responsive Sizing

Auto-adjust editor based on container:

```typescript
function makeResponsive() {
    const container = document.getElementById('imageeditor-container');
    
    // Update editor size on window resize
    window.addEventListener('resize', () => {
        imageEditor.width = `${container.offsetWidth}px`;
        imageEditor.height = `${container.offsetHeight}px`;
        imageEditor.refresh();
    });
}

makeResponsive();
```

## Common Patterns

### Pattern: Fit Image to Width

```typescript
function fitToWidth() {
    const dimension = imageEditor.getImageDimension();
    const containerWidth = imageEditor.upperCanvas.width;
    
    // Calculate zoom to fit width
    const zoom = containerWidth / dimension.width;
    imageEditor.zoom(zoom);
}

document.getElementById('fitWidthBtn').onclick = fitToWidth;
```

### Pattern: Fit Image to Height

```typescript
function fitToHeight() {
    const dimension = imageEditor.getImageDimension();
    const containerHeight = imageEditor.upperCanvas.height;
    
    // Calculate zoom to fit height
    const zoom = containerHeight / dimension.height;
    imageEditor.zoom(zoom);
}

document.getElementById('fitHeightBtn').onclick = fitToHeight;
```

### Pattern: Rotate Gallery

```typescript
function createRotateGallery() {
    const rotateBtn = document.getElementById('rotateBtn');
    const rotations = [0, 90, 180, 270];
    let currentIndex = 0;
    
    rotateBtn.onclick = () => {
        currentIndex = (currentIndex + 1) % rotations.length;
        imageEditor.rotate(rotations[currentIndex]);
    };
}

createRotateGallery();
```

### Pattern: Zoom with Buttons

```typescript
function setupZoomControls() {
    document.getElementById('zoomInBtn').onclick = () => {
        const current = imageEditor.zoomLevel || 1;
        imageEditor.zoom(current + 0.1);
    };
    
    document.getElementById('zoomOutBtn').onclick = () => {
        const current = imageEditor.zoomLevel || 1;
        imageEditor.zoom(Math.max(0.1, current - 0.1));
    };
    
    document.getElementById('zoomResetBtn').onclick = () => {
        imageEditor.zoom(1);
    };
}

setupZoomControls();
```

### Pattern: Mirror Image Horizontally then Rotate

```typescript
function mirrorThenRotate() {
    // Mirror left-right
    imageEditor.flip('Horizontal');
    
    // Then rotate 90 degrees
    imageEditor.rotate(90);
}

document.getElementById('transformBtn').onclick = mirrorThenRotate;
```

