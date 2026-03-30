# Getting Started with ComboBox

## Table of Contents
- [Installation](#installation)
- [Project Setup](#project-setup)
- [Import CSS Theme](#import-css-theme)
- [Initialize ComboBox](#initialize-combobox)
- [Bind Local Data](#bind-local-data)
- [Configure Properties](#configure-properties)
- [Run Application](#run-application)
- [Troubleshooting](#troubleshooting)

## Installation

### Step 1: Install Syncfusion Packages

The ComboBox component is part of the `@syncfusion/ej2-dropdowns` package. Install it using npm:

```bash
npm install @syncfusion/ej2-dropdowns
```

### Required Dependencies

The ComboBox package automatically includes these dependencies:

```
@syncfusion/ej2-dropdowns
├── @syncfusion/ej2-base
├── @syncfusion/ej2-data
├── @syncfusion/ej2-lists
├── @syncfusion/ej2-inputs
├── @syncfusion/ej2-buttons
├── @syncfusion/ej2-popups
└── @syncfusion/ej2-navigations
```

You don't need to install these separately—they're bundled with the dropdown package.

## Project Setup

### Using Webpack (Recommended)

Clone the Syncfusion quickstart repository for a pre-configured setup:

```bash
git clone https://github.com/SyncfusionExamples/ej2-quickstart-webpack ej2-quickstart
cd ej2-quickstart
npm install
```

This includes:
- Webpack configuration for TypeScript
- Dev server setup
- CSS bundling
- Hot module reloading

### Manual Setup

If you have an existing TypeScript project, ensure you have:

```bash
npm install --save-dev webpack webpack-cli typescript ts-loader
```

**webpack.config.js:**
```javascript
module.exports = {
    mode: 'development',
    entry: './src/app.ts',
    output: {
        filename: 'app.js',
        path: __dirname + '/dist'
    },
    module: {
        rules: [
            {
                test: /\.ts$/,
                use: 'ts-loader',
                exclude: /node_modules/
            },
            {
                test: /\.css$/,
                use: ['style-loader', 'css-loader']
            }
        ]
    }
};
```

## Import CSS Theme

### Required Imports

Add CSS imports to your TypeScript file or index.html. Syncfusion provides multiple built-in themes:

**In HTML (index.html):**
```html
<!DOCTYPE html>
<html>
<head>
    <!-- Material Theme -->
    <link rel="stylesheet" href="node_modules/@syncfusion/ej2-base/styles/material.css" />
    <link rel="stylesheet" href="node_modules/@syncfusion/ej2-dropdowns/styles/material.css" />
    <link rel="stylesheet" href="node_modules/@syncfusion/ej2-inputs/styles/material.css" />
    <link rel="stylesheet" href="node_modules/@syncfusion/ej2-popups/styles/material.css" />
</head>
<body>
    <input type="text" id="comboelement" />
</body>
</html>
```

**Or in TypeScript (app.ts):**
```typescript
import '@syncfusion/ej2-base/styles/material.css';
import '@syncfusion/ej2-dropdowns/styles/material.css';
import '@syncfusion/ej2-inputs/styles/material.css';
import '@syncfusion/ej2-popups/styles/material.css';
```

### Available Themes

| Theme | CSS File |
|-------|----------|
| Material | `material.css` |
| Bootstrap | `bootstrap.css` |
| Fabric | `fabric.css` |
| Tailwind | `tailwind.css` |
| Bootstrap 5 | `bootstrap5.css` |

Choose one theme that matches your application design.

## Initialize ComboBox

### Step 1: Create HTML Element

Add an input element that will become the ComboBox:

```html
<input type="text" id="comboelement" />
```

### Step 2: Import and Initialize

```typescript
import { ComboBox } from '@syncfusion/ej2-dropdowns';

// Initialize ComboBox component
const comboBoxObject: ComboBox = new ComboBox({
    placeholder: "Select an item"
});

// Render to the DOM
comboBoxObject.appendTo('#comboelement');
```

**What happens:**
1. `new ComboBox({...})` creates a ComboBox instance
2. Configuration object specifies properties
3. `appendTo('#comboelement')` renders the component to the HTML element

### Result

An empty ComboBox appears in the browser with "Select an item" as placeholder text.

---

## Bind Local Data

### Array of Strings

Simplest data source—array of string values:

```typescript
import { ComboBox } from '@syncfusion/ej2-dropdowns';

const sportsData: string[] = ['Badminton', 'Cricket', 'Football', 'Golf', 'Tennis'];

const comboBox: ComboBox = new ComboBox({
    dataSource: sportsData,
    placeholder: "Select a sport",
    popupHeight: '200px'
});

comboBox.appendTo('#comboelement');
```

**User sees:** "Badminton", "Cricket", "Football", etc. in dropdown list
**Selected value:** Same as displayed text

---

### Array of JSON Objects

For structured data with separate display and value fields:

```typescript
import { ComboBox } from '@syncfusion/ej2-dropdowns';

const employeeData: { [key: string]: Object }[] = [
    { EmployeeID: 1, Name: 'Alice Johnson' },
    { EmployeeID: 2, Name: 'Bob Smith' },
    { EmployeeID: 3, Name: 'Carol White' }
];

const comboBox: ComboBox = new ComboBox({
    dataSource: employeeData,
    fields: { text: 'Name', value: 'EmployeeID' },
    placeholder: "Select employee"
});

comboBox.appendTo('#comboelement');
```

**User sees:** Employee names ("Alice Johnson", "Bob Smith", etc.)
**Selected value:** Employee IDs (1, 2, 3, etc.)

**When to use:** Database records, API responses, or any structured data.

---

### Nested Object Fields

For deeply nested data structures:

```typescript
const locationData: { [key: string]: Object }[] = [
    { 
        Country: { Name: 'USA', Code: 'US' },
        Capital: 'Washington DC'
    },
    { 
        Country: { Name: 'India', Code: 'IN' },
        Capital: 'New Delhi'
    }
];

const comboBox: ComboBox = new ComboBox({
    dataSource: locationData,
    fields: { text: 'Country.Name', value: 'Country.Code' },
    placeholder: "Select country"
});

comboBox.appendTo('#comboelement');
```

**Dot notation:** Use `'Country.Name'` to access nested properties
**Result:** Shows country names, stores country codes

---

## Configure Properties

### Common Configuration Properties

```typescript
const comboBox: ComboBox = new ComboBox({
    // Data
    dataSource: ['Item1', 'Item2', 'Item3'],
    fields: { text: 'text', value: 'value' },
    
    // Display
    placeholder: "Select an item",
    popupHeight: '200px',              // Height of dropdown list
    popupWidth: '250px',               // Width of dropdown list
    
    // Behavior
    allowFiltering: false,             // Enable search box
    allowCustom: false,                // Allow typing custom values
    
    // Selection
    value: 'Item1',                    // Pre-select value
    index: 0,                          // Pre-select by index
    
    // State
    enabled: true,                     // Enable/disable component
    readonly: false                    // Read-only mode
});

comboBox.appendTo('#comboelement');
```

### Essential Properties Explained

| Property | Type | Default | Purpose |
|----------|------|---------|---------|
| `dataSource` | array | `[]` | Array of data items |
| `fields` | object | `{ text: 'text', value: 'value' }` | Map data fields |
| `placeholder` | string | `''` | Hint text in input |
| `popupHeight` | string | `'300px'` | Height of dropdown |
| `allowFiltering` | boolean | `false` | Enable search |
| `allowCustom` | boolean | `false` | Allow custom values |
| `value` | string | `''` | Currently selected value |
| `enabled` | boolean | `true` | Component enabled state |

---

## Run Application

### Using Webpack Dev Server

```bash
npm start
```

Opens http://localhost:8080 in your browser with hot reload enabled.

### Build for Production

```bash
npm run build
```

Creates optimized bundle in `dist/` folder.

---

## Complete Example

**index.html:**
```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title>ComboBox Getting Started</title>
    <link rel="stylesheet" href="node_modules/@syncfusion/ej2-base/styles/material.css" />
    <link rel="stylesheet" href="node_modules/@syncfusion/ej2-dropdowns/styles/material.css" />
    <link rel="stylesheet" href="node_modules/@syncfusion/ej2-inputs/styles/material.css" />
    <link rel="stylesheet" href="node_modules/@syncfusion/ej2-popups/styles/material.css" />
</head>
<body>
    <div style="margin: 50px;">
        <h3>Select Your Favorite Sport</h3>
        <input type="text" id="comboelement" />
    </div>
</body>
</html>
```

**app.ts:**
```typescript
import { ComboBox } from '@syncfusion/ej2-dropdowns';
import '@syncfusion/ej2-base/styles/material.css';
import '@syncfusion/ej2-dropdowns/styles/material.css';
import '@syncfusion/ej2-inputs/styles/material.css';
import '@syncfusion/ej2-popups/styles/material.css';

// Define sport data
const sportsData: string[] = [
    'Badminton',
    'Basketball',
    'Cricket',
    'Football',
    'Golf',
    'Hockey',
    'Tennis',
    'Volleyball'
];

// Initialize ComboBox
const comboBox: ComboBox = new ComboBox({
    dataSource: sportsData,
    placeholder: "Select your favorite sport",
    popupHeight: '200px',
    change: (args: any) => {
        console.log('Selected sport:', args.value);
    }
});

// Render to page
comboBox.appendTo('#comboelement');
```

**Result:** Running application shows ComboBox with sports list, logging selected value on change.

---

## Troubleshooting

### Issue: "ComboBox is not defined"

**Cause:** Package not installed or import missing
**Solution:**
```bash
npm install @syncfusion/ej2-dropdowns
```

```typescript
import { ComboBox } from '@syncfusion/ej2-dropdowns';  // Verify this line
```

---

### Issue: Styles not loading (unstyled component)

**Cause:** CSS imports missing
**Solution:** Add all required CSS imports:
```html
<link rel="stylesheet" href="node_modules/@syncfusion/ej2-base/styles/material.css" />
<link rel="stylesheet" href="node_modules/@syncfusion/ej2-dropdowns/styles/material.css" />
<link rel="stylesheet" href="node_modules/@syncfusion/ej2-inputs/styles/material.css" />
<link rel="stylesheet" href="node_modules/@syncfusion/ej2-popups/styles/material.css" />
```

---

### Issue: HTML element not found

**Cause:** ComboBox initializing before DOM is ready
**Solution:** Ensure HTML element exists before calling `appendTo()`:

```typescript
// ✅ Correct: Element exists first
document.addEventListener('DOMContentLoaded', () => {
    const comboBox = new ComboBox({ dataSource: ['a', 'b'] });
    comboBox.appendTo('#comboelement');
});
```

---

### Issue: Data not showing in dropdown

**Cause:** Field mapping incorrect for object arrays
**Solution:** Verify field names match your data:

```typescript
// Your data structure
const data = [
    { id: 1, label: 'Option 1' },
    { id: 2, label: 'Option 2' }
];

// Correct mapping
const comboBox = new ComboBox({
    dataSource: data,
    fields: { text: 'label', value: 'id' }  // Match property names
});
```

---

## Next Steps

- ✅ **Done:** Basic ComboBox setup with local data
- 📖 **Next:** [Data Binding](../data-binding.md) - Remote data, OData, DataManager
- 🔍 **Then:** [Filtering & Search](../filtering-and-search.md) - Add search functionality
- 🎨 **Advanced:** [Templates & Grouping](../templates-and-grouping.md) - Customize list display

---

## See Also

- [Data Binding Guide](../data-binding.md)
- [API Reference: ComboBox](https://ej2.syncfusion.com/documentation/api/combo-box/)
- [Sample Applications](https://github.com/SyncfusionExamples/ej2-combobox-typescript-samples)
