# Customization

## Table of Contents
- [Toolbar Customization](#toolbar-customization)
- [Quick Access Configuration](#quick-access-configuration)
- [Theme Styling](#theme-styling)
- [Event Handlers](#event-handlers)
- [Configuration Options](#configuration-options)

## Toolbar Customization

### Toolbar Items

Control which tools appear in the toolbar:

```typescript
import { ImageEditor } from '@syncfusion/ej2-image-editor';

const imageEditor = new ImageEditor({
    toolbar: [
        'Open',              // File open
        'Save',              // File save
        'ZoomIn',            // Zoom in
        'ZoomOut',           // Zoom out
        'ZoomReset',         // Reset zoom to 100%
        'FitToWidth',        // Fit image to width
        'FitToHeight',       // Fit image to height
        'Crop',              // Crop tool
        'Rotate',            // Rotate options
        'Flip',              // Flip options
        'Undo',              // Undo action
        'Redo',              // Redo action
        'Reset',             // Reset to original
        'Annotation',        // Text annotation
        'ShapeAnnotation',   // Shape tools
        'FreehandDraw',      // Freehand drawing
        'Redact',            // Redaction tool
        'Frame',             // Frame insertion
        'Filter',            // Filter effects
        'FineTune'           // Fine-tuning adjustments
    ]
});

imageEditor.appendTo('#imageeditor');
```

### Minimal Toolbar

```typescript
// Simplified toolbar for basic editing
const imageEditor = new ImageEditor({
    toolbar: ['Open', 'Crop', 'Rotate', 'Save', 'Undo', 'Redo']
});
```

### Hide Toolbar Entirely

```typescript
const imageEditor = new ImageEditor({
    toolbar: []  // Empty array hides toolbar
});
```

### Custom Toolbar with Separator

```typescript
const imageEditor = new ImageEditor({
    toolbar: [
        'Open', 'Save',
        '|',                 // Separator/divider
        'Crop', 'Rotate',
        '|',
        'Annotation', 'FreehandDraw',
        '|',
        'Filter', 'FineTune'
    ]
});
```

### Toolbar Position

```typescript
// Toolbar appears at top (default)
const imageEditor = new ImageEditor({
    toolbarPosition: 'top'
});

// Note: Position customization depends on Syncfusion version
```

## Quick Access Configuration

### Enable Quick Access Toolbar

```typescript
const imageEditor = new ImageEditor({
    width: '550px',
    height: '330px',
    quickAccessToolbar: [
        'Open', 'Save', 'Undo', 'Redo'
    ]
});
```

### Quick Access with Dropdown

```typescript
const imageEditor = new ImageEditor({
    quickAccessToolbar: [
        {
            name: 'Selection',
            items: ['Rectangle', 'Circle', 'Square', 'Ratio']
        },
        {
            name: 'Shapes',
            items: ['Rectangle', 'Ellipse', 'Arrow', 'Line']
        }
    ]
});
```

### Contextual Quick Access

```typescript
// Shows relevant tools based on context
const imageEditor = new ImageEditor({
    created: () => {
        // Auto-suggest tools based on image state
    }
});
```

## Theme Styling

### Built-in Themes

```typescript
const imageEditor = new ImageEditor({
    theme: 'material'      // Material theme (default)
});

// Available themes:
// - material / material-dark
// - bootstrap5 / bootstrap5-dark
// - bootstrap4 / bootstrap4-dark
// - fabric / fabric-dark
// - tailwind / tailwind-dark
// - fluent / fluent-dark
```

### CSS Theme Import

```css
/* Import theme CSS */
@import "@syncfusion/ej2-image-editor/styles/material.css";

/* Dark theme variant */
@import "@syncfusion/ej2-image-editor/styles/material-dark.css";
```

### CSS Variables

Customize theme using CSS variables:

```css
:root {
    /* Primary colors */
    --primary-color: #0078d4;
    --primary-hover: #005a9e;
    
    /* Background colors */
    --background-color: #ffffff;
    --surface-color: #f3f2f1;
    
    /* Text colors */
    --text-color: #323130;
    --text-secondary: #605e5c;
    
    /* Border and shadows */
    --border-color: #e1dfdd;
    --shadow: 0 1px 3px rgba(0,0,0,0.12);
}

.e-image-editor {
    --primary-color: var(--primary-color);
    --background-color: var(--background-color);
}
```

### Custom Theme

```typescript
// Create custom theme stylesheet
const customTheme = `
    .e-image-editor .e-toolbar {
        background: linear-gradient(to right, #667eea 0%, #764ba2 100%);
    }
    
    .e-image-editor .e-toolbar-item {
        color: white;
    }
    
    .e-image-editor .e-toolbar-item:hover {
        background: rgba(255, 255, 255, 0.2);
    }
    
    .e-image-editor .e-toolbar-item.e-active {
        background: rgba(0, 0, 0, 0.3);
    }
`;

const style = document.createElement('style');
style.textContent = customTheme;
document.head.appendChild(style);
```

### Theme Studio

Create custom themes using Syncfusion Theme Studio:

```
https://ej2.syncfusion.com/themestudio/
```

## Event Handlers

### Creation Events

```typescript
const imageEditor = new ImageEditor({
    created: () => {
        console.log('Image Editor initialized');
        // Load default image
        imageEditor.open('default.jpg');
    },
    
    destroyed: () => {
        console.log('Image Editor destroyed');
        // Cleanup resources
    }
});
```

### Image Events

```typescript
const imageEditor = new ImageEditor({
    imageLoaded: () => {
        console.log('Image loaded and ready');
        const dimension = imageEditor.getImageDimension();
        console.log(`Image size: ${dimension.width}x${dimension.height}`);
    }
});
```

### Save Events

```typescript
const imageEditor = new ImageEditor({
    beforeSave: (args) => {
        console.log('Before save', args);
        // Validate before saving
        // Prevent default save if needed
        // args.cancel = true;
    },
    
    saved: (args) => {
        console.log('After save', args);
    }
});
```

### Annotation Events

```typescript
const imageEditor = new ImageEditor({
    annotationAdded: (args) => {
        console.log('Annotation added:', args.annotation);
    },
    
    annotationRemoved: (args) => {
        console.log('Annotation removed:', args.annotation);
    }
});
```

### Selection Events

```typescript
const imageEditor = new ImageEditor({
    selectionChanged: (args) => {
        console.log('Selection changed:', args.selection);
    }
});
```

## Configuration Options

### Size Configuration

```typescript
const imageEditor = new ImageEditor({
    width: '600px',        // Fixed width
    height: '400px',       // Fixed height
    
    // OR use percentages
    width: '100%',
    height: '80vh'
});
```

### Constraints

```typescript
const imageEditor = new ImageEditor({
    allowRotation: true,    // Enable rotation
    enableZoom: true,       // Enable zoom controls
    allowCropping: true,    // Enable cropping
    autoRender: true        // Auto-render changes
});
```

### Localization

```typescript
import { L10n } from '@syncfusion/ej2-base';

// Register translations
L10n.load({
    'en-US': {
        'image-editor': {
            'open': 'Open Image',
            'save': 'Save Image',
            'crop': 'Crop',
            'rotate': 'Rotate'
        }
    }
});

const imageEditor = new ImageEditor({
    locale: 'en-US'
});
```

### Canvas Configuration

```typescript
const imageEditor = new ImageEditor({
    // Canvas background color
    canvasColor: '#f0f0f0',
    
    // Canvas border
    canvasBorder: 'solid 1px #ccc'
});
```

### Toolbar Configuration Object

```typescript
const imageEditor = new ImageEditor({
    toolbarConfig: {
        position: 'top',           // 'top' or 'bottom'
        height: '50px',
        backgroundColor: '#f5f5f5',
        items: ['Open', 'Save', 'Crop']
    }
});
```

## Common Patterns

### Pattern: Context-Based Toolbar

```typescript
function setupContextToolbar(imageEditor: ImageEditor) {
    imageEditor.selectionChanged = () => {
        if (imageEditor.selectedShape) {
            // Show annotation-specific tools
            imageEditor.toolbar = [
                'Delete', 'Duplicate', 'BringForward',
                'SendBackward', 'Properties'
            ];
        } else {
            // Show general tools
            imageEditor.toolbar = [
                'Open', 'Save', 'Crop', 'Rotate',
                'Filter', 'FineTune'
            ];
        }
    };
}
```

### Pattern: Progressive Enhancement

```typescript
function setupProgressiveToolbar(imageEditor: ImageEditor) {
    let complexity = 'basic';  // basic, intermediate, advanced
    
    const toolbars = {
        basic: ['Open', 'Crop', 'Rotate', 'Save'],
        intermediate: ['Open', 'Crop', 'Rotate', 'Filter', 'Annotation', 'Save', 'Undo', 'Redo'],
        advanced: [
            'Open', 'Save', 'ZoomIn', 'ZoomOut', 'Crop', 'Rotate', 'Flip',
            'Undo', 'Redo', 'Reset', 'Annotation', 'ShapeAnnotation',
            'FreehandDraw', 'Redact', 'Filter', 'FineTune'
        ]
    };
    
    document.getElementById('basicBtn').onclick = () => {
        complexity = 'basic';
        imageEditor.toolbar = toolbars.basic;
    };
    
    document.getElementById('advancedBtn').onclick = () => {
        complexity = 'advanced';
        imageEditor.toolbar = toolbars.advanced;
    };
}
```

