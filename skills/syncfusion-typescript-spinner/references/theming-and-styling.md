# Theming & Styling

## Table of Contents
- [Built-in Themes](#built-in-themes)
- [Importing Themes](#importing-themes)
- [Theme Selection Guide](#theme-selection-guide)
- [Custom Styling](#custom-styling)
- [Size Variations](#size-variations)
- [Color Customization](#color-customization)
- [RTL Support](#rtl-support)

---

## Built-in Themes

Syncfusion provides multiple pre-built themes for spinners. All themes are production-ready and follow design guidelines.

### Available Themes

| Theme | Style | Best For | File |
|-------|-------|----------|------|
| **Material** | Modern, flat design | Default choice, clean UI | `material.css` |
| **Bootstrap** | Bootstrap 4 style | Bootstrap projects | `bootstrap.css` |
| **Bootstrap5** | Bootstrap 5 style | Bootstrap 5 projects | `bootstrap5.css` |
| **Fabric** | Microsoft design | Enterprise apps | `fabric.css` |
| **Tailwind** | Tailwind CSS style | Tailwind projects | `tailwind.css` |
| **Material-Dark** | Dark variant | Dark mode apps | `material-dark.css` |
| **Bootstrap-Dark** | Dark variant | Dark mode Bootstrap | `bootstrap-dark.css` |

---

## Importing Themes

### Method 1: CSS File Import (Recommended)

Import the theme CSS file in your main stylesheet:

```css
/* src/styles/styles.css */

/* Import Material theme */
@import '@syncfusion/ej2-popups/styles/material.css';

/* Your other styles */
body {
  font-family: Arial, sans-serif;
}
```

### Method 2: Direct Link Tag

Import via HTML link tag:

```html
<head>
  <!-- Material theme -->
  <link rel="stylesheet" href="node_modules/@syncfusion/ej2-popups/styles/material.css">
</head>
```

### Method 3: JavaScript Import

Import theme in TypeScript/JavaScript:

```typescript
import '@syncfusion/ej2-popups/styles/material.css';
```

### Method 4: CDN Link

Use CDN for quick prototyping (not recommended for production):

```html
<link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/ej2-popups/styles/material.css">
```

---

## Theme Selection Guide

### Material Theme

Modern, flat design with smooth animations. **Default and recommended**.

```css
@import '@syncfusion/ej2-popups/styles/material.css';
```

**When to use:**
- Modern web applications
- Default choice when unsure
- Material Design projects
- Most compatibility

**Appearance:**
- Clean, minimalist spinner
- Smooth rotation animation
- 4px border thickness
- Neutral colors

### Bootstrap Theme

Matches Bootstrap 4 design system.

```css
@import '@syncfusion/ej2-popups/styles/bootstrap.css';
```

**When to use:**
- Bootstrap 4 projects
- Consistent with Bootstrap components
- Enterprise Bootstrap apps

### Bootstrap5 Theme

Updated for Bootstrap 5 design system.

```css
@import '@syncfusion/ej2-popups/styles/bootstrap5.css';
```

**When to use:**
- Bootstrap 5+ projects
- Modern Bootstrap applications

### Fabric Theme

Microsoft's Fluent Design System.

```css
@import '@syncfusion/ej2-popups/styles/fabric.css';
```

**When to use:**
- Enterprise applications
- Microsoft-aligned design
- Windows-style interfaces

### Tailwind CSS Theme

Tailwind-inspired design system.

```css
@import '@syncfusion/ej2-popups/styles/tailwind.css';
```

**When to use:**
- Tailwind CSS projects
- Utility-first CSS frameworks

### Dark Themes

Dark mode variants for low-light environments.

```css
/* Material Dark */
@import '@syncfusion/ej2-popups/styles/material-dark.css';

/* Bootstrap Dark */
@import '@syncfusion/ej2-popups/styles/bootstrap-dark.css';
```

**When to use:**
- Dark mode applications
- Night mode support
- Reduced eye strain interfaces

---

## Custom Styling

### Override Theme Colors

Customize spinner appearance using CSS variables or class overrides.

### CSS Variables

Material theme supports CSS custom properties:

```css
@import '@syncfusion/ej2-popups/styles/material.css';

:root {
  --ej2-spinner-background: #0078d4;  /* Spinner color */
}
```

### Custom CSS Classes

Add custom classes to your container:

```html
<div id="spinner-container" class="custom-spinner"></div>
```

```css
.custom-spinner {
  background-color: rgba(0, 0, 0, 0.1) !important;
}

/* Override spinner animation */
.custom-spinner .e-spinner-pane {
  background: rgba(255, 255, 255, 0.9);
}
```

### Complete Styling Example

```css
@import '@syncfusion/ej2-popups/styles/material.css';

/* Custom spinner container */
.my-spinner-container {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 300px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border-radius: 8px;
}

/* Customize spinner appearance */
.my-spinner-container .e-spinner-pane {
  background: rgba(255, 255, 255, 0.1);
}

.my-spinner-container .e-spinner {
  border-color: white;
  border-top-color: #667eea;
}

.my-spinner-container .e-spinner-label {
  color: white;
  font-weight: 500;
}
```

### Apply Custom Styling

```html
<div id="spinner-container" class="my-spinner-container"></div>
```

```typescript
import { createSpinner, showSpinner } from '@syncfusion/ej2-popups';

const container = document.getElementById('spinner-container');

createSpinner({
  target: container,
  label: 'Processing...'
});

showSpinner(container);
```

---

## Size Variations

### Default Size

Standard spinner size for most use cases.

```typescript
createSpinner({
  target: document.getElementById('spinner'),
  // Default size (no size property)
});
```

### Small Spinner

Compact spinner for inline indicators.

```css
/* Make spinner smaller via CSS */
.my-spinner-container .e-spinner {
  width: 30px;
  height: 30px;
  border-width: 2px;
}
```

### Large Spinner

Prominent spinner for full-page loading.

```css
.my-spinner-container .e-spinner {
  width: 50px;
  height: 50px;
  border-width: 5px;
}
```

### Example: Multiple Sizes

```html
<div class="spinner-demo">
  <div id="small-spinner" class="spinner small"></div>
  <div id="medium-spinner" class="spinner medium"></div>
  <div id="large-spinner" class="spinner large"></div>
</div>
```

```css
@import '@syncfusion/ej2-popups/styles/material.css';

.spinner {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 200px;
  margin: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

/* Small size */
.spinner.small .e-spinner {
  width: 25px;
  height: 25px;
  border-width: 2px;
}

/* Medium size (default) */
.spinner.medium .e-spinner {
  width: 40px;
  height: 40px;
  border-width: 3px;
}

/* Large size */
.spinner.large .e-spinner {
  width: 60px;
  height: 60px;
  border-width: 4px;
}
```

```typescript
import { createSpinner, showSpinner } from '@syncfusion/ej2-popups';

const containers = {
  small: document.getElementById('small-spinner'),
  medium: document.getElementById('medium-spinner'),
  large: document.getElementById('large-spinner')
};

Object.values(containers).forEach(container => {
  createSpinner({
    target: container,
    label: 'Loading'
  });
  showSpinner(container);
});
```

---

## Color Customization

### Change Spinner Color

Override the primary spinner color:

```css
@import '@syncfusion/ej2-popups/styles/material.css';

.my-spinner .e-spinner {
  border-color: #e0e0e0;
  border-top-color: #0078d4;  /* Primary color */
}
```

### Multiple Color Schemes

Create color variants for different contexts:

```css
@import '@syncfusion/ej2-popups/styles/material.css';

/* Primary (blue) spinner */
.spinner-primary .e-spinner {
  border-color: #e3e3e3;
  border-top-color: #0078d4;
}

/* Success (green) spinner */
.spinner-success .e-spinner {
  border-color: #e3e3e3;
  border-top-color: #107c10;
}

/* Warning (orange) spinner */
.spinner-warning .e-spinner {
  border-color: #e3e3e3;
  border-top-color: #ff8c00;
}

/* Error (red) spinner */
.spinner-error .e-spinner {
  border-color: #e3e3e3;
  border-top-color: #e81123;
}

/* Spinner label styling */
.spinner-primary .e-spinner-label {
  color: #0078d4;
}

.spinner-success .e-spinner-label {
  color: #107c10;
}

.spinner-warning .e-spinner-label {
  color: #ff8c00;
}

.spinner-error .e-spinner-label {
  color: #e81123;
}
```

### Usage Example

```html
<div id="primary-spinner" class="spinner-primary"></div>
<div id="success-spinner" class="spinner-success"></div>
<div id="warning-spinner" class="spinner-warning"></div>
<div id="error-spinner" class="spinner-error"></div>
```

```typescript
import { createSpinner, showSpinner } from '@syncfusion/ej2-popups';

type SpinnerType = 'primary' | 'success' | 'warning' | 'error';

function createColoredSpinner(id: string, type: SpinnerType, label: string) {
  const container = document.getElementById(id);
  
  createSpinner({
    target: container,
    label: label
  });
  
  container?.classList.add(`spinner-${type}`);
  showSpinner(container);
}

// Create spinners with different colors
createColoredSpinner('primary-spinner', 'primary', 'Loading');
createColoredSpinner('success-spinner', 'success', 'Syncing');
createColoredSpinner('warning-spinner', 'warning', 'Processing');
createColoredSpinner('error-spinner', 'error', 'Retrying');
```

---

## RTL Support

### Enable RTL (Right-to-Left)

For Arabic, Hebrew, and other RTL languages:

```html
<html dir="rtl">
  <head>
    <!-- RTL-aware CSS import -->
    <link rel="stylesheet" href="node_modules/@syncfusion/ej2-popups/styles/material.css">
  </head>
  <body>
    <div id="spinner-container"></div>
  </body>
</html>
```

### CSS RTL Classes

Apply RTL-specific styling:

```css
@import '@syncfusion/ej2-popups/styles/material.css';

html[dir="rtl"] .my-spinner {
  text-align: right;
}

html[dir="rtl"] .e-spinner-label {
  margin-right: 10px;
}
```

### TypeScript RTL Handling

```typescript
import { createSpinner, showSpinner } from '@syncfusion/ej2-popups';

function createRTLSpinner(containerId: string) {
  const container = document.getElementById(containerId);
  const isRTL = document.documentElement.dir === 'rtl';
  
  createSpinner({
    target: container,
    label: isRTL ? 'جاري التحميل...' : 'Loading...'
  });
  
  showSpinner(container);
}

// Usage
createRTLSpinner('spinner-container');
```

### RTL Example

```html
<!DOCTYPE html>
<html dir="rtl">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>RTL Spinner - العربية</title>
  
  <link rel="stylesheet" href="src/styles/styles.css" />
</head>
<body>
  <div class="container">
    <h1>محمل البيانات</h1>
    <div id="spinner-container"></div>
  </div>
  
  <script src="src/app.ts"></script>
</body>
</html>
```

```css
/* src/styles/styles.css */
@import '@syncfusion/ej2-popups/styles/material.css';

body {
  font-family: 'Segoe UI', 'Arial', sans-serif;
  padding: 20px;
  background: #f5f5f5;
}

.container {
  max-width: 600px;
  margin: 0 auto;
  background: white;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

h1 {
  color: #333;
  text-align: center;
}

#spinner-container {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 300px;
}
```

```typescript
import { createSpinner, showSpinner } from '@syncfusion/ej2-popups';

const container = document.getElementById('spinner-container');

createSpinner({
  target: container,
  label: 'جاري تحميل البيانات...'
});

showSpinner(container);

// Simulate operation
setTimeout(() => {
  container.innerHTML = '<p>تم التحميل بنجاح!</p>';
}, 3000);
```

---

## Theme Switching Example

Allow users to switch themes dynamically:

```typescript
import { createSpinner, showSpinner, hideSpinner } from '@syncfusion/ej2-popups';

class ThemeSwitcher {
  private container: HTMLElement;
  private themeLink: HTMLLinkElement;
  
  constructor(containerId: string) {
    this.container = document.getElementById(containerId)!;
    this.themeLink = document.querySelector('link[rel="stylesheet"]')!;
    
    createSpinner({ target: this.container });
    this.setupThemeSwitcher();
  }
  
  private setupThemeSwitcher() {
    const themeSelect = document.getElementById('theme-select') as HTMLSelectElement;
    
    if (themeSelect) {
      themeSelect.addEventListener('change', (e) => {
        const theme = (e.target as HTMLSelectElement).value;
        this.switchTheme(theme);
      });
    }
  }
  
  private switchTheme(theme: string) {
    const themePath = `node_modules/@syncfusion/ej2-popups/styles/${theme}.css`;
    
    showSpinner(this.container);
    
    // Simulate theme loading delay
    setTimeout(() => {
      this.themeLink.href = themePath;
      hideSpinner(this.container);
      console.log(`Theme switched to: ${theme}`);
    }, 500);
  }
}

// Initialize when DOM is ready
document.addEventListener('DOMContentLoaded', () => {
  new ThemeSwitcher('spinner-container');
});
```

---

## Troubleshooting

### Spinner Colors Not Applying?

**Issue:** Custom color CSS not working.

**Solutions:**
1. Verify CSS specificity:
   ```css
   /* Higher specificity */
   #spinner-container .e-spinner {
     border-top-color: #0078d4 !important;
   }
   ```

2. Check CSS is imported after theme:
   ```css
   /* Correct order */
   @import '@syncfusion/ej2-popups/styles/material.css';
   @import 'custom-styles.css'; /* After theme */
   ```

### Theme Not Loading?

**Issue:** Spinner appears unstyled (just a div).

**Solutions:**
1. Verify theme CSS file exists in node_modules
2. Check import path is correct:
   ```css
   @import '@syncfusion/ej2-popups/styles/material.css'; /* Correct */
   @import 'material.css'; /* Wrong - file not found */
   ```

3. Try CDN version for testing:
   ```css
   @import url('https://cdn.syncfusion.com/ej2/ej2-popups/styles/material.css');
   ```

### RTL Not Working?

**Issue:** Spinner doesn't respect RTL direction.

**Solution:** Ensure `dir="rtl"` is on `<html>` element:
```html
<!-- CORRECT -->
<html dir="rtl">

<!-- WRONG -->
<div dir="rtl">
```

---

## Next Steps

- See [spinner-methods.md](spinner-methods.md) for control methods
- Check [advanced-usage.md](advanced-usage.md) for integration patterns
