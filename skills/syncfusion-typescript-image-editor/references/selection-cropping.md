# Selection and Cropping

## Table of Contents
- [Selection Types](#selection-types)
- [Cropping Workflow](#cropping-workflow)
- [Aspect Ratio Constraints](#aspect-ratio-constraints)
- [Selection Manipulation](#selection-manipulation)
- [Advanced Cropping](#advanced-cropping)
- [Common Patterns](#common-patterns)

## Selection Types

### Enable Crop Tool

Activate cropping with selection options:

```typescript
import { ImageEditor } from '@syncfusion/ej2-image-editor';

const imageEditor = new ImageEditor({
    toolbar: ['Crop'],
    allowCropping: true
});

imageEditor.appendTo('#imageeditor');

// User clicks Crop button to start selection
```

### Selection Type Options

Available selection shapes:

| Type | Shape | Use Case |
|------|-------|----------|
| Custom | Free rectangle | Any region |
| Circle | Perfect circle | Round images |
| Square | Perfect square | Equal aspect ratio |
| Ratio | Custom ratio | 16:9, 4:3, 1:1 |

### Custom Selection

Free-form rectangular selection:

```typescript
// User clicks Crop → Selection Type → Custom
// Then draws rectangle on image
// Can resize by dragging edges/corners
```

### Circle Selection

Round selection area:

```typescript
// User clicks Crop → Selection Type → Circle
// Then draws circle on image
// Aspect ratio is 1:1 (perfect circle)
```

### Square Selection

Square selection with equal sides:

```typescript
// User clicks Crop → Selection Type → Square
// Maintains equal width and height
// Useful for icon/avatar cropping
```

### Ratio-Based Selection

Constrain to specific aspect ratios:

```typescript
// Available presets:
// - 16:9 (Widescreen)
// - 4:3 (Standard)
// - 1:1 (Square)
// - 3:2 (Classic photo)
// - Custom ratio input

// User selects ratio from dropdown in crop toolbar
```

## Cropping Workflow

### Basic Crop Process

```typescript
const imageEditor = new ImageEditor({
    allowCropping: true,
    toolbar: ['Crop']
});

// Step 1: User clicks Crop button
// Step 2: Contextual toolbar appears with selection options
// Step 3: User selects selection type (Custom, Circle, Square, Ratio)
// Step 4: User draws selection on image
// Step 5: User can pan image to align crop region
// Step 6: User clicks ✓ (check mark) to apply crop
// OR clicks ✗ (X) to cancel
```

### Crop via Programmatic Selection

```typescript
// Set crop region programmatically
imageEditor.crop({
    x: 100,
    y: 100,
    width: 300,
    height: 300
});

// Then call crop() to apply
imageEditor.crop();
```

### Crop with Specific Dimensions

```typescript
// Crop to 400x300 region starting at (50, 50)
imageEditor.crop({
    x: 50,
    y: 50,
    width: 400,
    height: 300
});

// Image is cropped to this region
```

## Aspect Ratio Constraints

### Preset Aspect Ratios

```typescript
const presetRatios = {
    'Free': null,
    '1:1': 1,
    '4:3': 4/3,
    '16:9': 16/9,
    '3:2': 3/2,
    '2:3': 2/3
};

// User selects from toolbar dropdown
// Selection box maintains chosen ratio
```

### Custom Aspect Ratio

```typescript
// User can input custom ratio
// Example: 5:4 ratio
// Selection maintains 5:4 proportions as user resizes

// Custom ratio entry field in crop toolbar
```

### Apply Aspect Ratio Constraint

```typescript
function cropWithRatio(imageEditor: ImageEditor, ratio: number) {
    // Get selection
    const current = imageEditor.getImageDimension();
    
    // Calculate height based on ratio
    let width = 300;
    let height = width / ratio;
    
    // Ensure doesn't exceed image
    if (height > current.height) {
        height = current.height;
        width = height * ratio;
    }
    
    imageEditor.crop({
        x: 50,
        y: 50,
        width: width,
        height: height
    });
}

cropWithRatio(imageEditor, 16/9);  // Crop to 16:9
```

## Selection Manipulation

### Move Selection

Drag selection to different position:

```typescript
// User can click inside selection and drag to move
// Useful for positioning crop area on specific region
// Works automatically with selection enabled
```

### Resize Selection

Adjust selection dimensions:

```typescript
// User drags edges/corners to resize
// If ratio constraint active, maintains ratio
// Works automatically with selection enabled
```

### Selection Handles

```typescript
// Visual handles appear on selection:
// - 4 corners for diagonal resize
// - 4 edges for horizontal/vertical resize
// - Center for movement
// All are interactive by default
```

## Advanced Cropping

### Pan Image Within Selection

Move image to position crop region:

```typescript
// When crop selection active:
// 1. User can click outside selection to pan image
// 2. OR hold spacebar and drag to pan
// 3. Aligns image to desired crop position
```

### Preview Crop Result

```typescript
// Crop dialog shows:
// - Selection outline on image
// - Area outside selection dimmed
// - Aspect ratio maintained if constrained
// - Real-time preview as user adjusts
```

### Apply Rotation During Crop

```typescript
// In crop contextual toolbar:
// 1. Rotate button for 90° increments
// 2. Flip buttons for mirror operations
// 3. Straighten slider for fine angle adjustment
// 4. All applied before crop finalizes
```

### Straighten While Cropping

```typescript
// Straighten slider appears in crop context menu
// - Adjusts image angle before cropping
// - Typically range: -45° to +45°
// - Click apply to accept straightening
```

## Common Patterns

### Pattern: Crop to Square Avatar

```typescript
function cropToSquareAvatar() {
    const imageEditor = new ImageEditor({
        toolbar: ['Crop'],
        allowCropping: true
    });
    
    // Automatically open crop with square selection
    setTimeout(() => {
        // Simulate user selecting square crop
        const dimension = imageEditor.getImageDimension();
        const minDim = Math.min(dimension.width, dimension.height);
        
        imageEditor.crop({
            x: (dimension.width - minDim) / 2,
            y: (dimension.height - minDim) / 2,
            width: minDim,
            height: minDim
        });
    }, 500);
}
```

### Pattern: Crop to Common Aspect Ratios

```typescript
function setupRatioCropping(imageEditor: ImageEditor) {
    const ratios = {
        '16:9': 16/9,
        '4:3': 4/3,
        '1:1': 1,
        '3:2': 3/2
    };
    
    Object.entries(ratios).forEach(([label, ratio]) => {
        const button = document.createElement('button');
        button.textContent = label;
        button.onclick = () => {
            // Get current selection
            const dim = imageEditor.getImageDimension();
            let width = 300;
            let height = width / ratio;
            
            if (height > dim.height) {
                height = dim.height;
                width = height * ratio;
            }
            
            imageEditor.crop({
                x: (dim.width - width) / 2,
                y: (dim.height - height) / 2,
                width: width,
                height: height
            });
        };
        document.getElementById('ratioControls').appendChild(button);
    });
}
```

### Pattern: Smart Crop with Content Detection

```typescript
function smartCrop(imageEditor: ImageEditor) {
    // Get image dimensions
    const dimension = imageEditor.getImageDimension();
    
    // Example: Center crop to 80% of image
    const cropScale = 0.8;
    const cropWidth = dimension.width * cropScale;
    const cropHeight = dimension.height * cropScale;
    
    const x = (dimension.width - cropWidth) / 2;
    const y = (dimension.height - cropHeight) / 2;
    
    imageEditor.crop({
        x: Math.round(x),
        y: Math.round(y),
        width: Math.round(cropWidth),
        height: Math.round(cropHeight)
    });
}

// Apply smart crop
// document.getElementById('smartCropBtn').onclick = () => smartCrop(imageEditor);
```

### Pattern: Progressive Cropping

```typescript
function setupProgressiveCrop(imageEditor: ImageEditor) {
    let cropHistory = [];
    
    document.getElementById('cropBtn').onclick = () => {
        // Store current crop state
        const dimension = imageEditor.getImageDimension();
        cropHistory.push({
            x: dimension.x,
            y: dimension.y,
            width: dimension.width,
            height: dimension.height
        });
        
        // Apply crop
        imageEditor.crop();
    };
    
    document.getElementById('undoCropBtn').onclick = () => {
        if (cropHistory.length > 0) {
            // Undo last crop by reloading image
            imageEditor.undo();
        }
    };
}
```

### Pattern: Crop with Preview

```typescript
function cropWithPreview(imageEditor: ImageEditor) {
    const previewCanvas = document.createElement('canvas');
    const previewCtx = previewCanvas.getContext('2d');
    
    // Monitor crop changes
    const observer = {
        onCropChange: (args) => {
            // Draw preview
            const { x, y, width, height } = args.cropRegion;
            
            previewCanvas.width = width;
            previewCanvas.height = height;
            
            // You would draw the crop preview here
            console.log(`Crop preview: ${width}x${height}`);
        }
    };
    
    // Apply observer pattern (illustration)
}
```

### Pattern: Batch Crop Multiple Images

```typescript
async function batchCropImages(imageUrls: string[], cropRegion: any) {
    const croppedImages = [];
    
    for (const url of imageUrls) {
        const imageEditor = new ImageEditor({
            width: '600px',
            height: '400px'
        });
        
        await imageEditor.open(url);
        imageEditor.crop(cropRegion);
        
        const croppedData = imageEditor.export('PNG');
        croppedImages.push(croppedData);
        
        imageEditor.destroy();
    }
    
    return croppedImages;
}
```

