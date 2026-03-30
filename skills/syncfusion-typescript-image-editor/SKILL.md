---
name: syncfusion-typescript-image-editor
description: Implement the Syncfusion TypeScript Image Editor control for image editing, annotation, and manipulation. Use this when the user needs to add image editing capabilities, work with filters, transformations, cropping, or drawing features in TypeScript applications. Covers setup, image operations, annotations, filters, accessibility, and customization patterns.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
  platform: "TypeScript"
---

# Implementing Syncfusion TypeScript Image Editor

A comprehensive skill for implementing the Syncfusion Image Editor control in TypeScript applications. This skill covers initialization, image loading/saving, annotation tools, image manipulation (zoom, crop, rotate, transform), filtering, fine-tuning, accessibility, and customization patterns.

## When to Use This Skill

Use this skill when the user needs to:

- **Add image editing capabilities** to TypeScript applications
- **Enable image annotations** (text, freehand drawing, shapes, redaction)
- **Implement image transformations** (crop, rotate, flip, straighten, resize)
- **Apply filters and fine-tuning** (brightness, contrast, saturation, blur, sharpen)
- **Set up image import/export workflows** (open, save, download)
- **Configure toolbars and quick access** settings
- **Implement layer management** and z-order operations
- **Add keyboard shortcuts** and accessibility features
- **Customize themes** and styling for the image editor
- **Integrate with dialogs** and modal workflows
- **Handle undo/redo** operations and history management

**Trigger Keywords:** Image editing, image editor, annotation tools, image filters, crop image, redact image, image transformation, draw on image, ej2-image-editor, Syncfusion image editor, image processing, TypeScript editor

## Overview

The Syncfusion Image Editor is a powerful, feature-rich component for professional image editing in web applications. Key capabilities include:

- **Image Operations** - Open, save, export in JPEG, PNG, SVG, WebP, BMP formats
- **Zoom & Pan** - Multiple zoom methods (toolbar, mouse wheel, pinch, keyboard)
- **Transformations** - Crop, rotate, flip, straighten, resize with preview
- **Annotations** - Text, freehand drawing, shapes (rectangles, circles, arrows, lines), redaction, frames
- **Filters & Effects** - 20+ filter effects plus fine-tuning controls
- **Layer Management** - Z-order operations, multiple annotations
- **History** - Full undo/redo support with action tracking
- **Accessibility** - WCAG 2.2 compliant, keyboard navigation, screen reader support
- **Customization** - Toolbar, themes, event handlers, quick access
- **TypeScript Support** - Full type definitions and webpack configuration

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- TypeScript project setup with webpack
- NPM package installation (@syncfusion/ej2-image-editor)
- Dependency management
- CSS theme imports and configuration
- Basic component initialization
- HTML markup setup

### Core Operations
📄 **Read:** [references/core-operations.md](references/core-operations.md)
- Opening and loading images programmatically
- Supported file formats (JPEG, PNG, SVG, WebP, BMP)
- Saving and exporting images
- File input validation and restrictions
- File type constraints and size limits
- Download vs. save operations

### Image Manipulation
📄 **Read:** [references/image-manipulation.md](references/image-manipulation.md)
- Zooming methods (toolbar, mouse wheel, pinch, keyboard, fit-to-width/height)
- Panning and image movement
- Image resizing and scaling operations
- Image rotation and flip operations
- Straightening with sliders
- Image dimension queries
- Canvas and viewport management

### Annotation Tools
📄 **Read:** [references/annotation-tools.md](references/annotation-tools.md)
- Text annotations with font customization (family, size, style, color)
- Freehand drawing tools and brush settings
- Shape annotations (rectangles, ellipses, arrows, lines, paths)
- Redaction capabilities and masking
- Frame insertion and styling
- Annotation styling (fill color, stroke color, stroke width)
- Annotation positioning and transformation

### Selection and Cropping
📄 **Read:** [references/selection-cropping.md](references/selection-cropping.md)
- Selection types (custom, circle, square, ratio-based)
- Cropping workflow and execution
- Aspect ratio constraints and custom ratios
- Selection manipulation and repositioning
- Crop finalization and preview
- Region management

### Filters and Effects
📄 **Read:** [references/filters-effects.md](references/filters-effects.md)
- Applying filter effects programmatically
- Fine-tuning adjustments (20+ effect types)
- Brightness, contrast, saturation controls
- Blur, sharpen, hue, saturation effects
- Image processing capabilities
- Effect preview and application

### Undo and Redo
📄 **Read:** [references/undo-redo.md](references/undo-redo.md)
- Undo/redo keyboard shortcuts (Ctrl+Z, Ctrl+Y)
- Undo/redo method calls
- Operation history tracking
- History state management
- Action reversal patterns

### Layer Management
📄 **Read:** [references/layer-management.md](references/layer-management.md)
- Z-order operations and layer stacking
- Bringing layers forward/backward
- Layer arrangement and management
- Working with multiple annotations
- Overlap management

### Accessibility
📄 **Read:** [references/accessibility.md](references/accessibility.md)
- Complete keyboard shortcut reference (Ctrl+Z, Ctrl+Y, Ctrl+S, Delete, Escape, etc.)
- WCAG 2.2, Section 508, ADA compliance
- Screen reader support and ARIA attributes
- Right-to-Left (RTL) support
- Keyboard navigation patterns
- Color contrast and accessibility best practices
- Mobile device support

### Customization
📄 **Read:** [references/customization.md](references/customization.md)
- Toolbar configuration and customization
- Quick access toolbar setup
- Theme styling and CSS customization
- Theme Studio integration
- CSS variables and custom themes
- Event binding and callbacks
- Configuration options reference

### Localization
📄 **Read:** [references/localization.md](references/localization.md)
- Multi-language support setup
- Language-specific configurations
- Locale settings and resource strings
- RTL (Right-to-Left) support configuration
- Region-specific customizations

### How-To Guide
📄 **Read:** [references/how-to-guide.md](references/how-to-guide.md)
- Rendering image editor in dialog/modal windows
- Clearing image before reopening dialogs
- Resetting images to original state
- Fitting images to editor width or height
- Dialog state management
- Common integration patterns

### Advanced Patterns
📄 **Read:** [references/advanced-patterns.md](references/advanced-patterns.md)
- Event handling patterns and event system
- Custom workflow examples
- Complex integration scenarios
- Performance optimization tips
- TypeScript type definitions and interfaces
- Memory management

## Quick Start

### Installation

```bash
npm install @syncfusion/ej2-image-editor
```

### Basic Initialization

```typescript
import { ImageEditor } from '@syncfusion/ej2-image-editor';

// Create a new instance
const imageEditor = new ImageEditor({
    width: '550px',
    height: '330px',
    toolbar: ['Open', 'Save', 'ZoomIn', 'ZoomOut', 'Crop', 'Rotate', 'Flip']
});

// Append to a container
imageEditor.appendTo('#imageeditor');

// Open an image
imageEditor.open('path/to/image.jpg');
```

### HTML Setup

```html
<div id="imageeditor"></div>
```

### CSS Themes

```css
@import "@syncfusion/ej2-image-editor/styles/material.css";
```

## Common Patterns

### Pattern 1: Basic Image Editor with Full Toolbar

```typescript
import { ImageEditor } from '@syncfusion/ej2-image-editor';

const imageEditor = new ImageEditor({
    width: '600px',
    height: '400px',
    toolbar: ['Open', 'Save', 'ZoomIn', 'ZoomOut', 'Crop', 
              'Rotate', 'Flip', 'Undo', 'Redo', 'Reset'],
    created: () => {
        imageEditor.open('sample-image.jpg');
    }
});

imageEditor.appendTo('#imageeditor');
```

### Pattern 2: Adding Text Annotation

```typescript
const dimension = imageEditor.getImageDimension();
imageEditor.drawText(
    dimension.x + 50, 
    dimension.y + 50, 
    'Annotation Text',
    'Arial',      // font family
    30,           // font size
    false,        // bold
    false,        // italic
    'blue',       // text color
    false,        // selected
    null,         // rotation
    '#FFFFCC',    // fill color
    'black'       // stroke color
);
```

### Pattern 3: Applying Filters

```typescript
// Apply brightness adjustment
imageEditor.finetuneImage('brightness', 30);

// Apply blur effect
imageEditor.filter('Blur', 5);

// Apply multiple effects
imageEditor.finetuneImage('contrast', 20);
imageEditor.finetuneImage('saturation', 15);
```

### Pattern 4: Cropping an Image

```typescript
// Enable crop selection
imageEditor.drawImage('crop');

// When user completes selection
imageEditor.crop();

// Or programmatically set crop area
imageEditor.crop({
    x: 0,
    y: 0,
    width: 500,
    height: 300
});
```

### Pattern 5: Dialog Integration

```typescript
import { Dialog } from '@syncfusion/ej2-popups';

const dialog = new Dialog({
    width: '600px',
    height: '450px',
    isModal: true,
    visible: false
});
dialog.appendTo('#dialog');

const imageEditor = new ImageEditor({
    width: '100%',
    height: '100%'
});
imageEditor.appendTo('#imageeditor');

document.getElementById('openBtn').onclick = () => {
    dialog.show();
    imageEditor.open('image.jpg');
};
```

## Key APIs

### Essential Methods

| Method | Purpose |
|--------|---------|
| `open(url)` | Load an image from URL or file path |
| `save()` | Save the edited image |
| `export()` | Export image in specified format |
| `crop()` | Execute crop operation |
| `rotate(degrees)` | Rotate image by specified degrees |
| `flip(direction)` | Flip image (horizontal or vertical) |
| `zoom(level)` | Set zoom level |
| `drawText(...)` | Add text annotation |
| `drawImage(type)` | Draw shapes or other objects |
| `undo()` | Undo last action |
| `redo()` | Redo last undone action |
| `reset()` | Reset image to original state |
| `clearImage()` | Clear current image |
| `filter(filterName, value)` | Apply filter effects |
| `finetuneImage(property, value)` | Apply fine-tuning adjustments |

### Key Properties

| Property | Type | Purpose |
|----------|------|---------|
| `width` | string | Editor container width |
| `height` | string | Editor container height |
| `toolbar` | string[] | Toolbar items to display |
| `allowRotation` | boolean | Enable/disable rotation |
| `enableZoom` | boolean | Enable/disable zoom |
| `allowCropping` | boolean | Enable/disable cropping |
| `theme` | string | Color theme for UI |

### Events

| Event | When Triggered |
|-------|----------------|
| `created` | After component initialization |
| `imageLoaded` | After image is loaded |
| `beforeSave` | Before saving image |
| `annotationAdded` | After annotation added |
| `selectionChanged` | When selection changes |
| `transforming` | During transformation |

## Common Use Cases

**Professional Image Editing Suite** - Full-featured editor with all tools
**Annotation Workflow** - Markup and redaction for documents
**Quick Crop Tool** - Simplified UI focused on cropping
**Social Media Tool** - Preset aspect ratios for platforms
**Screenshot Markup** - Quick annotation on screenshots
**Document Processing** - Straighten, crop, and export scans
**Dialog Integration** - Modal image editing workflows

## Related Skills

- `implementing-rich-text-editor` - For text document editing
- `implementing-buttons` - For control customization
- `implementing-dialogs` - For modal workflow integration

