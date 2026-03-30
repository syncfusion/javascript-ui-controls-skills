# Annotation Tools

## Table of Contents
- [Text Annotations](#text-annotations)
- [Freehand Drawing](#freehand-drawing)
- [Shape Annotations](#shape-annotations)
- [Redaction Tools](#redaction-tools)
- [Frames](#frames)
- [Annotation Styling](#annotation-styling)
- [Managing Annotations](#managing-annotations)
- [Common Patterns](#common-patterns)

## Text Annotations

### Add Text to Image

Insert text annotations with full customization:

```typescript
import { ImageEditor } from '@syncfusion/ej2-image-editor';

const imageEditor = new ImageEditor({ width: '550px', height: '330px' });
imageEditor.appendTo('#imageeditor');

// Get image dimension for positioning
const dimension = imageEditor.getImageDimension();

// Add basic text
imageEditor.drawText(
    dimension.x + 50,           // X coordinate
    dimension.y + 50,           // Y coordinate
    'Hello Syncfusion'          // Text content
);
```

### Text Customization

Control font, size, style, and colors:

```typescript
const dimension = imageEditor.getImageDimension();

// Full customization
imageEditor.drawText(
    dimension.x + 100,          // x position
    dimension.y + 100,          // y position
    'Custom Text',              // text content
    'Arial',                    // font family
    30,                         // font size (pixels)
    true,                       // bold
    false,                      // italic
    'darkblue',                 // text color
    false,                      // isSelected
    0,                          // rotation (degrees)
    '#FFFFCC',                  // fill (background) color
    'black',                    // stroke (outline) color
    2                           // stroke width
);
```

### Text Parameters Reference

| Parameter | Type | Purpose |
|-----------|------|---------|
| x | number | Horizontal position from left |
| y | number | Vertical position from top |
| text | string | Text content to display |
| fontFamily | string | Font family (Arial, Times New Roman, etc.) |
| fontSize | number | Font size in pixels |
| bold | boolean | Enable bold text |
| italic | boolean | Enable italic text |
| color | string | Text color (hex or name) |
| isSelected | boolean | Show selection handles |
| degree | number | Rotation angle in degrees |
| fillColor | string | Background fill color |
| strokeColor | string | Outline stroke color |
| strokeWidth | number | Outline width in pixels |

### Font Families

Available font options:

```typescript
const fonts = [
    'Arial',
    'Courier New',
    'Georgia',
    'Impact',
    'Palatino Linotype',
    'Segoe UI',
    'Tahoma',
    'Times New Roman',
    'Trebuchet MS',
    'Verdana'
];

// Use in drawText
imageEditor.drawText(x, y, 'Text', 'Georgia', 24, false, false, 'black');
```

### Text Styling Combinations

```typescript
// Bold only
imageEditor.drawText(x, y, 'Bold Text', 'Arial', 20, true, false, 'black');

// Italic only
imageEditor.drawText(x, y, 'Italic Text', 'Arial', 20, false, true, 'blue');

// Bold + Italic
imageEditor.drawText(x, y, 'Bold Italic', 'Arial', 20, true, true, 'red');
```

### Underline and Strikethrough

Add text decoration:

```typescript
const dimension = imageEditor.getImageDimension();

imageEditor.drawText(
    dimension.x + 50,
    dimension.y + 50,
    'Decorated Text',
    'Arial',
    24,
    false,
    false,
    'black',
    false,
    0,
    '',
    '',
    0,
    null,                       // transformCollection
    true,                       // underline
    false                       // strikethrough
);
```

## Freehand Drawing

### Enable Freehand Drawing Tool

Allow users to draw on image:

```typescript
const imageEditor = new ImageEditor({
    toolbar: ['FreehandDraw'],
    // Users can now draw freehand on image
});
```

### Draw via Toolbar

```typescript
// User clicks toolbar: Annotation → Freehand Draw
// Then draws on image by clicking and dragging
// Tool is UI-driven, no direct draw method
```

### Freehand Drawing Settings

Configure pen behavior:

```typescript
const imageEditor = new ImageEditor({
    width: '550px',
    height: '330px',
    toolbar: ['FreehandDraw', 'Annotation']
});

// Settings are configured via UI toolbar
// User selects: stroke color, stroke width
```

### Programmatic Freehand (Path Drawing)

Draw complex paths:

```typescript
// Freehand drawing via points
imageEditor.drawImage('freehand', {
    points: [
        { x: 50, y: 50 },
        { x: 100, y: 60 },
        { x: 150, y: 50 },
        { x: 200, y: 100 }
    ],
    strokeColor: '#FF0000',
    strokeWidth: 3
});
```

## Shape Annotations

### Rectangle

Draw rectangular shapes:

```typescript
const dimension = imageEditor.getImageDimension();

imageEditor.drawImage('rectangle', {
    x: dimension.x + 50,
    y: dimension.y + 50,
    width: 200,
    height: 150,
    strokeColor: '#0000FF',
    strokeWidth: 2,
    fillColor: '#FFFFCC'
});
```

### Ellipse/Circle

Draw circular shapes:

```typescript
imageEditor.drawImage('ellipse', {
    cx: dimension.x + 150,      // Center X
    cy: dimension.y + 100,      // Center Y
    rx: 75,                     // Radius X
    ry: 50,                     // Radius Y
    strokeColor: '#00FF00',
    strokeWidth: 2,
    fillColor: 'transparent'
});

// Perfect circle (rx == ry)
imageEditor.drawImage('ellipse', {
    cx: dimension.x + 150,
    cy: dimension.y + 100,
    rx: 60,
    ry: 60,                     // Same as rx for circle
    strokeColor: '#FF0000'
});
```

### Arrow

Draw arrow annotations:

```typescript
imageEditor.drawImage('arrow', {
    startX: dimension.x + 50,
    startY: dimension.y + 50,
    endX: dimension.x + 250,
    endY: dimension.y + 150,
    strokeColor: '#FF0000',
    strokeWidth: 3,
    arrowType: 'both'           // 'start', 'end', 'both'
});
```

### Line

Draw straight lines:

```typescript
imageEditor.drawImage('line', {
    x1: dimension.x + 50,
    y1: dimension.y + 50,
    x2: dimension.x + 300,
    y2: dimension.y + 200,
    strokeColor: '#0000FF',
    strokeWidth: 2
});
```

### Path

Draw multi-point paths:

```typescript
imageEditor.drawImage('path', {
    points: [
        { x: dimension.x + 50, y: dimension.y + 50 },
        { x: dimension.x + 100, y: dimension.y + 100 },
        { x: dimension.x + 150, y: dimension.y + 50 },
        { x: dimension.x + 200, y: dimension.y + 100 }
    ],
    strokeColor: '#FF00FF',
    strokeWidth: 2,
    fillColor: 'transparent'
});
```

## Redaction Tools

### Redact/Mask Areas

Hide sensitive information:

```typescript
// Redaction via toolbar
const imageEditor = new ImageEditor({
    toolbar: ['Redact'],
    // User draws rectangle to redact information
});

// Toolbar approach:
// 1. User clicks Redact tool
// 2. Draws rectangle over sensitive area
// 3. Area is masked/blacked out
```

### Programmatic Redaction

```typescript
// Redact via filled rectangle
imageEditor.drawImage('rectangle', {
    x: dimension.x + 100,
    y: dimension.y + 100,
    width: 150,
    height: 50,
    strokeColor: '#000000',
    strokeWidth: 1,
    fillColor: '#000000'       // Black fill for redaction
});
```

## Frames

### Add Frames to Image

Insert decorative borders/frames:

```typescript
const imageEditor = new ImageEditor({
    toolbar: ['Frame'],
    // User selects frame from options
});

// Available frame types accessed via toolbar
```

### Frame Selection via Toolbar

```typescript
// User interaction:
// 1. Click Frame button in toolbar
// 2. Select frame style from dropdown
// 3. Frame applied to image with adjustable border thickness
```

### Custom Frame Implementation

```typescript
// Add border frame programmatically
function addCustomFrame(thickness: number, color: string) {
    const dimension = imageEditor.getImageDimension();
    
    // Draw outer rectangle as frame
    imageEditor.drawImage('rectangle', {
        x: dimension.x - thickness,
        y: dimension.y - thickness,
        width: dimension.width + (thickness * 2),
        height: dimension.height + (thickness * 2),
        strokeColor: color,
        strokeWidth: thickness,
        fillColor: 'transparent'
    });
}

addCustomFrame(10, '#8B4513');  // Brown frame
```

## Annotation Styling

### Color Selection

Set annotation colors:

```typescript
// Text color
imageEditor.drawText(x, y, 'Red Text', 'Arial', 20, false, false, '#FF0000');

// Shape colors
imageEditor.drawImage('rectangle', {
    x: x, y: y, width: 100, height: 100,
    strokeColor: '#0000FF',    // Blue outline
    fillColor: '#00FF00'       // Green fill
});
```

### Stroke Width/Thickness

Control line thickness:

```typescript
// Thin line
imageEditor.drawImage('line', {
    x1: 50, y1: 50, x2: 200, y2: 100,
    strokeColor: '#000000',
    strokeWidth: 1             // 1 pixel
});

// Thick line
imageEditor.drawImage('line', {
    x1: 50, y1: 150, x2: 200, y2: 200,
    strokeColor: '#000000',
    strokeWidth: 5             // 5 pixels
});
```

### Transparency/Opacity

Use rgba for transparency:

```typescript
// Semi-transparent fill
imageEditor.drawImage('rectangle', {
    x: 50, y: 50, width: 200, height: 150,
    strokeColor: 'rgba(0, 0, 255, 1)',      // Solid blue outline
    fillColor: 'rgba(255, 255, 0, 0.5)'     // 50% transparent yellow
});
```

## Managing Annotations

### Select Annotation

Make annotation selected/editable:

```typescript
// Annotation becomes selected with handles
imageEditor.drawText(x, y, 'Selectable', 'Arial', 20, false, false, 'black', true);
//                                                                     ^^^^
//                                                          isSelected = true
```

### Delete Annotation

Remove annotations:

```typescript
// Delete last annotation
imageEditor.deleteShape();

// Keyboard shortcut (automatic)
// User selects annotation and presses Delete key
```

### Rotate Annotation

Rotate individual annotations:

```typescript
// Rotate text by 45 degrees
imageEditor.drawText(x, y, 'Rotated', 'Arial', 20, false, false, 'black', false, 45);
//                                                                                 ^^
//                                                                          degree = 45
```

### Transform Collection

Advanced annotation positioning:

```typescript
const transformCollection = {
    x: 100,
    y: 100,
    width: 200,
    height: 150,
    angle: 0,
    scaleX: 1,
    scaleY: 1,
    flipX: false,
    flipY: false
};

imageEditor.drawText(
    100, 100, 'Transformed Text', 'Arial', 24,
    false, false, 'black', false, 0, '', '', 0,
    transformCollection
);
```

## Common Patterns

### Pattern: Watermark Text

```typescript
function addWatermark(text: string) {
    const dimension = imageEditor.getImageDimension();
    
    // Center position
    const x = dimension.x + (dimension.width / 2) - 100;
    const y = dimension.y + (dimension.height / 2);
    
    // Large, semi-transparent text
    imageEditor.drawText(
        x, y, text, 'Arial', 48, false, true,
        'rgba(255, 255, 255, 0.3)',  // White, 30% opacity
        false, 45                     // 45 degree rotation
    );
}

addWatermark('© 2024 Company');
```

### Pattern: Multiple Shapes for Emphasis

```typescript
function highlightArea(x: number, y: number, width: number, height: number) {
    // Draw rectangle with thick border
    imageEditor.drawImage('rectangle', {
        x, y, width, height,
        strokeColor: '#FF0000',
        strokeWidth: 3,
        fillColor: 'transparent'
    });
    
    // Add corner markers
    const cornerSize = 10;
    imageEditor.drawImage('rectangle', {
        x: x - cornerSize/2, y: y - cornerSize/2,
        width: cornerSize, height: cornerSize,
        strokeColor: '#FF0000', strokeWidth: 1,
        fillColor: '#FF0000'
    });
}

highlightArea(100, 100, 200, 150);
```

### Pattern: Annotation with Label

```typescript
function annotateWithLabel(x: number, y: number, label: string, note: string) {
    // Add arrow pointing to area
    imageEditor.drawImage('arrow', {
        startX: x + 50, startY: y + 50,
        endX: x, endY: y,
        strokeColor: '#0000FF', strokeWidth: 2
    });
    
    // Add label text
    imageEditor.drawText(
        x + 60, y + 40,
        label, 'Arial', 16, true, false, '#0000FF'
    );
    
    // Add note in smaller text
    imageEditor.drawText(
        x + 60, y + 60,
        note, 'Arial', 12, false, false, '#666666'
    );
}

annotateWithLabel(200, 150, 'Important', 'Review this section');
```

### Pattern: Batch Annotations

```typescript
function addAnnotations(annotations: any[]) {
    annotations.forEach(annotation => {
        switch (annotation.type) {
            case 'text':
                imageEditor.drawText(
                    annotation.x, annotation.y, annotation.text,
                    annotation.font, annotation.size, false, false,
                    annotation.color
                );
                break;
            case 'rectangle':
                imageEditor.drawImage('rectangle', annotation.options);
                break;
            case 'arrow':
                imageEditor.drawImage('arrow', annotation.options);
                break;
        }
    });
}

addAnnotations([
    { type: 'rectangle', options: { x: 50, y: 50, width: 100, height: 100, strokeColor: '#FF0000' } },
    { type: 'text', x: 200, y: 200, text: 'Note', font: 'Arial', size: 20, color: '#000000' },
    { type: 'arrow', options: { startX: 100, startY: 100, endX: 200, endY: 200, strokeColor: '#0000FF' } }
]);
```

