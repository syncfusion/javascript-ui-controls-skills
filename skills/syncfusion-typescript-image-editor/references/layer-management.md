# Layer Management and Z-Order

## Table of Contents
- [Z-Order Operations](#z-order-operations)
- [Layer Stacking](#layer-stacking)
- [Managing Multiple Annotations](#managing-multiple-annotations)
- [Common Patterns](#common-patterns)

## Z-Order Operations

### Understanding Z-Order

Z-order determines the stacking order of annotations on the image (which appears in front/back):

```typescript
import { ImageEditor } from '@syncfusion/ej2-image-editor';

const imageEditor = new ImageEditor({ width: '550px', height: '330px' });
imageEditor.appendTo('#imageeditor');

// Lower z-index = behind
// Higher z-index = in front
```

### Get Current Z-Order

```typescript
// Get z-index of selected annotation
const selectedShape = imageEditor.selectedShape;
if (selectedShape) {
    console.log(`Current z-index: ${selectedShape.zIndex}`);
}
```

### Bring Forward

Move annotation up in z-order (closer to front):

```typescript
// Bring selected annotation forward by one level
imageEditor.bringForward();

// Bring to absolute front (topmost)
imageEditor.bringToFront();
```

### Send Backward

Move annotation down in z-order (further back):

```typescript
// Send selected annotation backward by one level
imageEditor.sendBackward();

// Send to absolute back (bottom-most)
imageEditor.sendToBack();
```

## Layer Stacking

### Understanding Layers

Each annotation is a layer that can be arranged:

```
Layer 3 (Front) ──┐
Layer 2          │ Z-Order (stacking)
Layer 1          │
Layer 0 (Back)  ─┘
```

### Select Annotation for Reordering

```typescript
// Click on annotation to select it
// Then use bring forward/backward operations

// Programmatic selection
imageEditor.selectShape(annotationElement);
```

### Reorder via Context Menu

```typescript
// Right-click on annotation shows menu:
// - Bring Forward
// - Bring to Front
// - Send Backward
// - Send to Back
```

### View Stacking Order

```typescript
function printZOrderStack(imageEditor: ImageEditor) {
    // Get all shapes/annotations
    const shapes = imageEditor.shapeCollection || [];
    
    shapes.forEach((shape, index) => {
        console.log(`Level ${index}: ${shape.shapeType} (z-index: ${shape.zIndex})`);
    });
}
```

## Managing Multiple Annotations

### Add Multiple Annotations

```typescript
const imageEditor = new ImageEditor({ width: '550px', height: '330px' });
imageEditor.appendTo('#imageeditor');

const dimension = imageEditor.getImageDimension();

// Add first annotation (will be at back initially)
imageEditor.drawText(
    dimension.x + 50,
    dimension.y + 50,
    'First Annotation',
    'Arial', 16, false, false, 'black'
);

// Add second annotation
imageEditor.drawImage('rectangle', {
    x: dimension.x + 100,
    y: dimension.y + 100,
    width: 150,
    height: 100,
    strokeColor: '#0000FF'
});

// Add third annotation (will be at front)
imageEditor.drawImage('arrow', {
    startX: dimension.x + 50,
    startY: dimension.y + 50,
    endX: dimension.x + 200,
    endY: dimension.y + 150,
    strokeColor: '#FF0000'
});
```

### Delete Annotation

Remove a specific annotation:

```typescript
// Select annotation then delete
imageEditor.deleteShape();

// Or use keyboard shortcut: Delete key
```

### Select Specific Annotation

```typescript
// Click on annotation in editor
// Or programmatically:

const shapes = imageEditor.shapeCollection;
if (shapes && shapes.length > 0) {
    imageEditor.selectShape(shapes[0]);  // Select first annotation
}
```

### List All Annotations

```typescript
function getAnnotationsList(imageEditor: ImageEditor) {
    const shapes = imageEditor.shapeCollection || [];
    const list = [];
    
    shapes.forEach((shape, index) => {
        list.push({
            index: index,
            type: shape.shapeType,
            x: shape.x,
            y: shape.y,
            text: shape.text || '',
            zIndex: shape.zIndex
        });
    });
    
    return list;
}

console.log(getAnnotationsList(imageEditor));
```

## Common Patterns

### Pattern: Organize by Type

```typescript
function organizeAnnotationsByType(imageEditor: ImageEditor) {
    const shapes = imageEditor.shapeCollection || [];
    
    // Group by type
    const textAnnotations = shapes.filter(s => s.shapeType === 'text');
    const shapeAnnotations = shapes.filter(s => ['rectangle', 'ellipse', 'arrow'].includes(s.shapeType));
    const drawingAnnotations = shapes.filter(s => s.shapeType === 'freehand');
    
    console.log(`Text annotations: ${textAnnotations.length}`);
    console.log(`Shape annotations: ${shapeAnnotations.length}`);
    console.log(`Drawing annotations: ${drawingAnnotations.length}`);
}
```

### Pattern: Set Drawing Order

```typescript
function setDrawingOrder(imageEditor: ImageEditor, order: string[]) {
    const shapes = imageEditor.shapeCollection || [];
    
    // order example: ['rectangles', 'text', 'arrows']
    
    // Get shapes by type
    const groups = {
        rectangles: shapes.filter(s => s.shapeType === 'rectangle'),
        text: shapes.filter(s => s.shapeType === 'text'),
        arrows: shapes.filter(s => s.shapeType === 'arrow')
    };
    
    // Reorder according to specification
    let zIndex = 0;
    order.forEach(type => {
        (groups[type] || []).forEach(shape => {
            shape.zIndex = zIndex++;
        });
    });
}
```

### Pattern: Highlight Selected Annotation

```typescript
function highlightSelectedAnnotation(imageEditor: ImageEditor) {
    // When annotation selected, bring it forward
    if (imageEditor.selectedShape) {
        imageEditor.bringToFront();
        
        // Optional: Add visual indicator
        console.log(`Selected: ${imageEditor.selectedShape.shapeType}`);
    }
}

// Listen for selection changes
imageEditor.addEventListener('selectionChanged', () => {
    highlightSelectedAnnotation(imageEditor);
});
```

### Pattern: Lock/Unlock Layers

```typescript
class LayerLock {
    private lockedLayers: Set<any> = new Set();
    
    constructor(private imageEditor: ImageEditor) {}
    
    lock(shape: any) {
        this.lockedLayers.add(shape);
    }
    
    unlock(shape: any) {
        this.lockedLayers.delete(shape);
    }
    
    isLocked(shape: any) {
        return this.lockedLayers.has(shape);
    }
    
    getLocked() {
        return Array.from(this.lockedLayers);
    }
    
    deleteAllUnlocked() {
        const shapes = this.imageEditor.shapeCollection || [];
        shapes.forEach(shape => {
            if (!this.isLocked(shape)) {
                this.imageEditor.selectShape(shape);
                this.imageEditor.deleteShape();
            }
        });
    }
}
```

### Pattern: Auto-Arrange Annotations

```typescript
function autoArrangeAnnotations(imageEditor: ImageEditor) {
    const shapes = imageEditor.shapeCollection || [];
    const dimension = imageEditor.getImageDimension();
    
    // Sort by position (left to right, top to bottom)
    const sorted = shapes.sort((a, b) => {
        if (a.y !== b.y) return a.y - b.y;
        return a.x - b.x;
    });
    
    // Apply z-order based on sorted position
    sorted.forEach((shape, index) => {
        shape.zIndex = index;
    });
}
```

### Pattern: Stacking Context

```typescript
class StackingContext {
    private layers: Map<string, any[]> = new Map();
    
    constructor(private imageEditor: ImageEditor) {}
    
    // Create named layer group
    createLayer(name: string) {
        if (!this.layers.has(name)) {
            this.layers.set(name, []);
        }
    }
    
    // Add shape to layer
    addToLayer(name: string, shape: any) {
        if (!this.layers.has(name)) {
            this.createLayer(name);
        }
        this.layers.get(name).push(shape);
    }
    
    // Get layer z-index range
    getLayerRange(name: string) {
        const layer = this.layers.get(name) || [];
        if (layer.length === 0) return null;
        
        const indices = layer.map(s => s.zIndex);
        return {
            min: Math.min(...indices),
            max: Math.max(...indices)
        };
    }
    
    // Bring entire layer forward
    bringLayerForward(name: string) {
        const layer = this.layers.get(name) || [];
        layer.forEach(shape => {
            shape.zIndex += 100;
        });
    }
    
    // List all layers
    getLayers() {
        return Array.from(this.layers.keys());
    }
}
```

### Pattern: Visibility Toggle

```typescript
class LayerVisibility {
    private hiddenLayers: Set<any> = new Set();
    
    hide(shape: any) {
        this.hiddenLayers.add(shape);
        shape.opacity = 0;  // Make transparent
    }
    
    show(shape: any) {
        this.hiddenLayers.delete(shape);
        shape.opacity = 1;  // Make visible
    }
    
    toggle(shape: any) {
        if (this.hiddenLayers.has(shape)) {
            this.show(shape);
        } else {
            this.hide(shape);
        }
    }
    
    isHidden(shape: any) {
        return this.hiddenLayers.has(shape);
    }
}
```

