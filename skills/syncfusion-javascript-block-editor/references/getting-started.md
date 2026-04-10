# Getting Started with Syncfusion JavaScript BlockEditor

This guide walks you through the process of setting up and initializing the Syncfusion JavaScript BlockEditor control in your application.

All peer dependencies are automatically installed when you install `@syncfusion/ej2-blockeditor`.

## Installation

### Using npm

Install the BlockEditor package from npm:

```bash
npm install @syncfusion/ej2-blockeditor --save
```

## Import CSS Styles

Import the BlockEditor and its dependent control styles in your `styles.css` file. Multiple themes are available:

### Tailwind3 Theme

```css
@import "../../node_modules/@syncfusion/ej2-base/styles/tailwind3.css";
@import "../../node_modules/@syncfusion/ej2-inputs/styles/tailwind3.css";
@import "../../node_modules/@syncfusion/ej2-buttons/styles/tailwind3.css";
@import "../../node_modules/@syncfusion/ej2-navigations/styles/tailwind3.css";
@import "../../node_modules/@syncfusion/ej2-popups/styles/tailwind3.css";
@import "../../node_modules/@syncfusion/ej2-dropdowns/styles/tailwind3.css";
@import "../../node_modules/@syncfusion/ej2-splitbuttons/styles/tailwind3.css";
@import "../../node_modules/@syncfusion/ej2-blockeditor/styles/tailwind3.css";
```

### Other Available Themes

Replace `tailwind3` with any of these theme names:
- `material` - Material Design theme
- `material3` - Material Design 3 theme
- `bootstrap5` - Bootstrap 5 theme
- `bootstrap4` - Bootstrap 4 theme
- `fabric` - Office Fabric theme
- `fluent` - Microsoft Fluent theme
- `highcontrast` - High contrast theme

## Basic Implementation

### Step 1: Add HTML Element

Add a container div with an ID in your `index.html`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>BlockEditor Demo</title>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link href="styles.css" rel="stylesheet" />
</head>
<body>
    <div id="container">
        <div id="blockeditor_default"></div>
    </div>
</body>
</html>
```

### Step 2: Initialize BlockEditor

Import and initialize the BlockEditor in your TypeScript file (`app.ts`):

```typescript
import { BlockEditor } from "@syncfusion/ej2-blockeditor";

// Initialize BlockEditor
const blockEditor: BlockEditor = new BlockEditor({});

// Render the BlockEditor
blockEditor.appendTo('#blockeditor_default');
```

### Step 3: Run the Application

```bash
npm start
```

The BlockEditor will render at `http://localhost:8080` (or the configured port).

## Minimal Working Example

Here's a complete minimal example with sample content:

```typescript
import { BlockEditor, BlockType, ContentType } from "@syncfusion/ej2-blockeditor";

const blockEditor: BlockEditor = new BlockEditor({
    width: '100%',
    height: '600px',
    blocks: [
        {
            id: 'heading-1',
            blockType: BlockType.Heading,
            properties: { level: 1 },
            content: [
                {
                    id: 'h1-content',
                    contentType: ContentType.Text,
                    content: 'BlockEditor Demo'
                }
            ]
        },
        {
            id: 'intro-para',
            blockType: BlockType.Paragraph,
            content: [
                {
                    id: 'intro-text',
                    contentType: ContentType.Text,
                    content: 'The Block Editor enables users to create, format, and organize content using various block types.'
                }
            ]
        }
    ]
});

blockEditor.appendTo('#blockeditor_default');
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>BlockEditor Demo</title>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link href="https://cdn.syncfusion.com/ej2/32.1.19/ej2-blockeditor/styles/tailwind3.css" rel="stylesheet" />
    <style>
        #container {
            margin: 50px auto;
            max-width: 1200px;
        }
    </style>
</head>
<body>
    <div id="container">
        <div id="blockeditor_default"></div>
    </div>
</body>
</html>
```

## Common Initialization Patterns

### Empty Editor (User Creates Content)

```typescript
const editor = new BlockEditor({
    width: '100%',
    height: '80vh'
});
editor.appendTo('#blockeditor');
```

The editor starts empty, ready for users to add content using slash commands (/) or typing.

### With Default Content

```typescript
const editor = new BlockEditor({
    blocks: [
        {
            blockType: BlockType.Paragraph,
            content: [
                {
                    contentType: ContentType.Text,
                    content: 'Start typing here...'
                }
            ]
        }
    ]
});
editor.appendTo('#blockeditor');
```

### Read-Only Mode

```typescript
const editor = new BlockEditor({
    blocks: existingBlocks,
    readOnly: true
});
editor.appendTo('#blockeditor');
```

### With Drag-and-Drop Disabled

```typescript
const editor = new BlockEditor({
    blocks: sampleBlocks,
    enableDragAndDrop: false
});
editor.appendTo('#blockeditor');
```

## Configuration Options at Initialization

You can configure various settings when initializing the BlockEditor:

```typescript
const editor = new BlockEditor({
    // Dimensions
    width: '100%',
    height: '600px',
    
    // Content
    blocks: [],
    
    // Features
    enableDragAndDrop: true,
    enableHtmlSanitizer: true,
    readOnly: false,
    
    // Undo/Redo
    undoRedoStack: 30,
    
    // Styling
    cssClass: 'custom-editor',
    
    // Localization
    locale: 'en-US',
    enableRtl: false,
    
    // Events
    created: () => {
        console.log('BlockEditor created');
    },
    blockChanged: (args) => {
        console.log('Content changed', args);
    }
});

editor.appendTo('#blockeditor');
```

## Troubleshooting

### Styles Not Loading

Ensure all dependent stylesheets are imported in the correct order (base → inputs → buttons → navigations → popups → dropdowns → splitbuttons → blockeditor).

### Editor Not Rendering

Check that:
1. The target element exists in the DOM before calling `appendTo()`
2. The element ID matches the selector (e.g., `#blockeditor_default`)
3. All required packages are installed (`npm install`)
4. CSS files are properly loaded

### Webpack Configuration Issues

Ensure your `webpack.config.js` includes proper loaders for CSS and TypeScript files. The Syncfusion quickstart template includes preconfigured webpack settings.
