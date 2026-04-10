# Getting Started with Syncfusion TypeScript Switch

## Table of Contents
- [Installation & Dependencies](#installation--dependencies)
- [Project Setup](#project-setup)
- [Basic Initialization](#basic-initialization)
- [Text Labels](#text-labels)
- [Themes & CSS](#themes--css)
- [Complete Example](#complete-example)

## Installation & Dependencies

### npm Packages Required

The Switch component requires the `@syncfusion/ej2-buttons` package. Install it using:

```bash
npm install @syncfusion/ej2-buttons
```

### Dependencies

The Switch component has these dependencies:

```
@syncfusion/ej2-buttons
  └── @syncfusion/ej2-base (automatically installed)
```

### TypeScript Setup

Ensure your project has TypeScript configured. If using webpack, the quickstart seed repository is preconfigured:

```bash
git clone https://github.com/SyncfusionExamples/ej2-quickstart-webpack- ej2-quickstart
cd ej2-quickstart
npm install
```

## Project Setup

### HTML Markup

Create an HTML file with a checkbox input element. The Switch component will be initialized on this element:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Syncfusion Switch</title>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    
    <!-- Syncfusion CSS Themes -->
    <link href="https://cdn.syncfusion.com/ej2/ej2-base/styles/material.css" rel="stylesheet" />
    <link href="https://cdn.syncfusion.com/ej2/ej2-buttons/styles/material.css" rel="stylesheet" />
</head>
<body>
    <!-- Switch element -->
    <input id="element" type="checkbox" />
    
    <script src="app.ts"></script>
</body>
</html>
```

### TypeScript File (app.ts)

Import the Switch component and initialize it:

```typescript
import { Switch } from '@syncfusion/ej2-buttons';

// Create a Switch component instance
let switchObj: Switch = new Switch({ checked: true });

// Render it to the #element
switchObj.appendTo('#element');
```

### Folder Structure

After setup, your project structure should look like:

```
project/
├── src/
│   ├── app.ts          (TypeScript file with Switch initialization)
│   ├── index.html      (HTML file)
│   └── styles.css      (Optional CSS)
├── package.json        (npm dependencies)
├── webpack.config.js   (Webpack configuration)
└── tsconfig.json       (TypeScript configuration)
```

## Basic Initialization

### Minimal Example

The simplest way to create a Switch:

```typescript
import { Switch } from '@syncfusion/ej2-buttons';

// Step 1: Create instance
const switchObj = new Switch();

// Step 2: Render to element
switchObj.appendTo('#element');
```

### With Initial State

Set the initial checked state:

```typescript
const switchObj = new Switch({ 
  checked: true  // Switch starts in ON state
});
switchObj.appendTo('#element');
```

### With Event Handling

Respond to state changes:

```typescript
const switchObj = new Switch({ 
  checked: false,
  change: function(args: any) {
    console.log('Switch state changed to:', args.checked);
  }
});
switchObj.appendTo('#element');
```

## Text Labels

### Adding ON/OFF Labels

The Switch can display text labels in both states using `onLabel` and `offLabel` properties:

```typescript
import { Switch } from '@syncfusion/ej2-buttons';

let switchObj: Switch = new Switch({
  onLabel: 'ON',      // Text when switch is checked
  offLabel: 'OFF',    // Text when switch is unchecked
  checked: true
});
switchObj.appendTo('#switch1');
```

### Custom Label Text

Use any text you want in the labels:

```typescript
const switchObj = new Switch({
  onLabel: 'Enable',
  offLabel: 'Disable',
  checked: false
});
switchObj.appendTo('#switch2');
```

### Important Note on Labels

Labels are **NOT supported in Material themes**. If using Material theme, the labels will not display. Consider using:
- Bootstrap 5 theme
- Fabric theme
- Or adding surrounding HTML labels instead

```html
<label for="switch1">Dark Mode</label>
<input id="switch1" type="checkbox" />
```

## Themes & CSS

### Including Theme CSS

Syncfusion provides several built-in themes. Include the appropriate CSS files in your HTML:

```html
<!-- Material Theme (default) -->
<link href="https://cdn.syncfusion.com/ej2/ej2-base/styles/material.css" rel="stylesheet" />
<link href="https://cdn.syncfusion.com/ej2/ej2-buttons/styles/material.css" rel="stylesheet" />

<!-- OR Bootstrap 5 Theme -->
<link href="https://cdn.syncfusion.com/ej2/ej2-base/styles/bootstrap5.css" rel="stylesheet" />
<link href="https://cdn.syncfusion.com/ej2/ej2-buttons/styles/bootstrap5.css" rel="stylesheet" />
```

### Available Themes

| Theme | URL |
|-------|-----|
| Material | `material.css` |
| Material Dark | `material-dark.css` |
| Bootstrap 5 | `bootstrap5.css` |
| Bootstrap 4 | `bootstrap4.css` |
| Fabric | `fabric.css` |
| Tailwind | `tailwind.css` |
| Fluent | `fluent.css` |

**Important:** Always include the base theme CSS before the component-specific CSS.

### Using enableRipple

For ripple effect animation on click:

```typescript
import { Switch } from '@syncfusion/ej2-buttons';
import { enableRipple } from '@syncfusion/ej2-base';

enableRipple(true);  // Enable ripple globally

const switchObj = new Switch({ checked: true });
switchObj.appendTo('#element');
```

## Complete Example

Here's a complete working example combining all concepts:

**HTML (index.html):**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Syncfusion Switch Example</title>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    
    <!-- Theme CSS -->
    <link href="https://cdn.syncfusion.com/ej2/ej2-base/styles/material.css" rel="stylesheet" />
    <link href="https://cdn.syncfusion.com/ej2/ej2-buttons/styles/material.css" rel="stylesheet" />
    
    <style>
        body {
            margin: 20px;
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto;
        }
        .switch-container {
            margin: 20px 0;
        }
    </style>
</head>
<body>
    <h1>Syncfusion Switch Examples</h1>
    
    <div class="switch-container">
        <h3>Basic Switch (Checked)</h3>
        <input id="switch1" type="checkbox" />
    </div>
    
    <div class="switch-container">
        <h3>Basic Switch (Unchecked)</h3>
        <input id="switch2" type="checkbox" />
    </div>
    
    <div class="switch-container">
        <h3>Switch with Status Display</h3>
        <input id="switch3" type="checkbox" />
        <p>Status: <span id="status">OFF</span></p>
    </div>
    
    <script src="app.ts"></script>
</body>
</html>
```

**TypeScript (app.ts):**
```typescript
import { Switch } from '@syncfusion/ej2-buttons';
import { enableRipple } from '@syncfusion/ej2-base';

enableRipple(true);

// Example 1: Basic switch (checked)
let switch1 = new Switch({ checked: true });
switch1.appendTo('#switch1');

// Example 2: Basic switch (unchecked)
let switch2 = new Switch({ checked: false });
switch2.appendTo('#switch2');

// Example 3: Switch with change handler
let switch3 = new Switch({ 
  checked: false,
  change: handleSwitchChange
});
switch3.appendTo('#switch3');

function handleSwitchChange(args: any): void {
  const statusElement = document.getElementById('status');
  if (statusElement) {
    statusElement.textContent = args.checked ? 'ON' : 'OFF';
  }
}
```

### Output

When you run the application, you'll see three switch controls:
1. A checked switch
2. An unchecked switch
3. A switch that displays its current status below it

This example demonstrates:
- Basic switch creation and initialization
- Initial checked state
- Change event handling
- Updating UI based on switch state

## Next Steps

After setting up the basic switch, explore:
- [States and Behavior](states-and-behavior.md) - Learn about disabled state, toggle method, and events
- [Customization](customization.md) - Customize colors, sizes, and styling
- [Form Integration](form-integration.md) - Use switches in HTML forms
- [Accessibility](accessibility.md) - Ensure accessibility compliance
