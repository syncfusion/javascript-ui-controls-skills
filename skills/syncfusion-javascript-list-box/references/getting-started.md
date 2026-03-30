# Getting Started with ListBox

## Table of Contents
- [Installation](#installation)
- [Basic Setup](#basic-setup)
- [Simple Initialization](#simple-initialization)
- [Minimal Working Example](#minimal-working-example)
- [CSS Imports](#css-imports)
- [Common Setup Errors](#common-setup-errors)

## Installation

### NPM Installation

```bash
npm install @syncfusion/ej2-dropdowns
```

### Required Dependencies

ListBox depends on the core library. Ensure you have:

```bash
npm install @syncfusion/ej2-base
npm install @syncfusion/ej2-data
```

### Module Imports

For TypeScript projects, import the ListBox component:

```typescript
import { ListBox } from '@syncfusion/ej2-dropdowns';
```

If using specific modules (CheckBoxSelection, ToolbarSettings):

```typescript
import { ListBox, CheckBoxSelection } from '@syncfusion/ej2-dropdowns';

// Register the module
ListBox.Inject(CheckBoxSelection);
```

## Basic Setup

### HTML Template

Create a container element with a unique ID:

```html
<div id="listbox"></div>
```

### TypeScript Initialization

```typescript
import { ListBox } from '@syncfusion/ej2-dropdowns';

// Create ListBox instance
const listObj: ListBox = new ListBox({
    // Configuration here
});

// Append to container
listObj.appendTo('#listbox');
```

### JavaScript Initialization

```javascript
// Create with constructor
var listObj = new ej.dropdowns.ListBox({
    // Configuration here
}, '#listbox');
```

## Simple Initialization

### Array of Strings

Simplest approach for displaying a list of text items:

```typescript
import { ListBox } from '@syncfusion/ej2-dropdowns';

const sportsData: string[] = [
    'Badminton',
    'Cricket',
    'Football',
    'Golf',
    'Tennis'
];

const listObj: ListBox = new ListBox({
    dataSource: sportsData
});

listObj.appendTo('#listbox');
```

Both text and value fields use the string directly.

### Array of Objects

For data with IDs or additional properties:

```typescript
import { ListBox } from '@syncfusion/ej2-dropdowns';

const data: { [key: string]: Object }[] = [
    { id: '1', text: 'Item 1' },
    { id: '2', text: 'Item 2' },
    { id: '3', text: 'Item 3' }
];

const listObj: ListBox = new ListBox({
    dataSource: data,
    fields: { text: 'text', value: 'id' }
});

listObj.appendTo('#listbox');
```

## Minimal Working Example

Complete working example with HTML, CSS, and TypeScript:

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8" />
    <title>ListBox Getting Started</title>
    <link rel="stylesheet" href="styles.css" />
</head>
<body>
    <h2>Sports Selection</h2>
    <div id="listbox"></div>

    <script src="config.js"></script>
    <script src="index.js"></script>
</body>
</html>
```

```typescript
// index.ts
import { ListBox } from '@syncfusion/ej2-dropdowns';

const sportsData: string[] = [
    'Badminton',
    'Cricket',
    'Football',
    'Golf',
    'Tennis',
    'Basketball',
    'Baseball',
    'Hockey',
    'Volleyball'
];

const listObj: ListBox = new ListBox({
    dataSource: sportsData
});

listObj.appendTo('#listbox');

// Optional: Handle selection change
listObj.change = function(e: any) {
    console.log('Selected item:', e.text);
};
```

```css
/* styles.css */
body {
    font-family: Segoe UI, Helvetica Neue, sans-serif;
    padding: 20px;
}

#listbox {
    width: 300px;
    border: 1px solid #ccc;
    border-radius: 4px;
}

.e-listbox .e-list-item {
    padding: 12px;
    border-bottom: 1px solid #f0f0f0;
}

.e-listbox .e-list-item:hover {
    background-color: #f0f0f0;
}

.e-listbox .e-list-item.e-selected {
    background-color: #0078d4;
    color: white;
}
```

## CSS Imports

### Material Theme

```typescript
import '@syncfusion/ej2-dropdowns/styles/material.css';
```

### Bootstrap Theme

```typescript
import '@syncfusion/ej2-dropdowns/styles/bootstrap.css';
```

### Fabric Theme

```typescript
import '@syncfusion/ej2-dropdowns/styles/fabric.css';
```

### Bootstrap Dark Theme

```typescript
import '@syncfusion/ej2-dropdowns/styles/bootstrap-dark.css';
```

### High Contrast Theme

```typescript
import '@syncfusion/ej2-dropdowns/styles/highcontrast.css';
```

### Tailwind CSS

```typescript
import '@syncfusion/ej2-dropdowns/styles/tailwind.css';
```

### Fluent Theme

```typescript
import '@syncfusion/ej2-dropdowns/styles/fluent.css';
```

**Best Practice**: Import theme CSS early in your application bundle to ensure consistent styling.

## Common Setup Errors

### Error: "ListBox is not defined"

**Cause**: Module not imported or script not loaded.

**Solution**: Ensure import statement:
```typescript
import { ListBox } from '@syncfusion/ej2-dropdowns';
```

### Error: Container element not found

**Cause**: HTML element with specified ID doesn't exist.

**Solution**: Verify element exists before appending:
```html
<div id="listbox"></div>
```

### Error: Module not found (@syncfusion/ej2-dropdowns)

**Cause**: Package not installed.

**Solution**: Install via npm:
```bash
npm install @syncfusion/ej2-dropdowns
```

### Styling not applied

**Cause**: CSS theme not imported.

**Solution**: Add theme import to your entry point:
```typescript
import '@syncfusion/ej2-dropdowns/styles/material.css';
```

### CheckBoxSelection module error

**Cause**: CheckBoxSelection not injected when using showCheckbox.

**Solution**: Inject the module:
```typescript
import { ListBox, CheckBoxSelection } from '@syncfusion/ej2-dropdowns';
ListBox.Inject(CheckBoxSelection);
```

## Next Steps

- **Data Binding**: See [data-binding.md](data-binding.md) for remote data and complex structures
- **Selection**: See [selection-and-checkboxes.md](selection-and-checkboxes.md) for single/multiple selection
- **Drag-and-Drop**: See [drag-and-drop.md](drag-and-drop.md) for reordering items
- **Accessibility**: See [accessibility-and-keyboard.md](accessibility-and-keyboard.md) for keyboard support
