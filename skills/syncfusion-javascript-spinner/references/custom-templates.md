# Custom Spinner Templates

## Table of Contents
- [Overview](#overview)
- [setSpinner Method](#setspinner-method)
- [Creating Custom Templates](#creating-custom-templates)
- [Template Design Patterns](#template-design-patterns)
- [Integration with Components](#integration-with-components)
- [Performance Optimization](#performance-optimization)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

---

## Overview

The Spinner component provides a `setSpinner` method that allows you to define custom HTML templates for loading indicators. Instead of using the default spinner animation, you can create branded, themed, or unique loading experiences tailored to your application.

**Key Points:**
- Global template setting (applies to all components created after the call)
- Template must be set **before** component creation
- HTML string format with custom CSS styling
- Completely customizable appearance and animations

---

## setSpinner Method

### Basic Syntax

```typescript
import { setSpinner } from '@syncfusion/ej2-popups';

setSpinner({
  template: '<div class="my-spinner"></div>'
});
```

### Method Signature

```typescript
setSpinner(options: SetSpinnerModel): void

interface SetSpinnerModel {
  template: string;  // Required: HTML string for custom spinner
}
```

### Import and Usage

```typescript
import { setSpinner } from '@syncfusion/ej2-popups';

// Define template as HTML string
const customTemplate = `
  <div style="width:100%;height:100%" class="my-custom-spinner">
    <div class="spinner-ring"></div>
    <p class="spinner-text">Loading...</p>
  </div>
`;

// Set the template
setSpinner({ template: customTemplate });
```

---

## Creating Custom Templates

### Simple Custom Template

Minimal custom spinner template:

```typescript
import { setSpinner } from '@syncfusion/ej2-popups';

// Simple rotating div template
const simpleTemplate = `
  <div style="width:100%;height:100%" class="simple-spinner">
    <div class="spinner-ring"></div>
  </div>
`;

setSpinner({ template: simpleTemplate });
```

### Template with Styling

Template with inline and external CSS:

```typescript
const styledTemplate = `
  <div style="
    width: 100%;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    background: rgba(0, 0, 0, 0.05);
  " class="styled-spinner">
    <div class="spinner-ring"></div>
    <div class="spinner-label">Processing...</div>
  </div>
`;

setSpinner({ template: styledTemplate });
```

### Template with SVG

Using SVG for custom graphics:

```typescript
const svgTemplate = `
  <div style="width:100%;height:100%;display:flex;align-items:center;justify-content:center;">
    <svg class="spinner-svg" viewBox="0 0 50 50" width="50" height="50">
      <circle cx="25" cy="25" r="20" fill="none" stroke="#0078d4" stroke-width="2" />
    </svg>
  </div>
`;

setSpinner({ template: svgTemplate });

// CSS for SVG animation
const svgCSS = `
  .spinner-svg {
    animation: rotate-svg 2s linear infinite;
  }

  @keyframes rotate-svg {
    to { transform: rotate(360deg); }
  }
`;
```

---

## Template Design Patterns

### Pattern 1: Rotating Circle Spinner

**HTML Template:**
```typescript
const rotatingTemplate = `
  <div style="width:100%;height:100%" class="rotating-spinner">
    <div class="ring"></div>
  </div>
`;
```

**CSS Styling:**
```css
.rotating-spinner {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 100%;
}

.rotating-spinner .ring {
  width: 50px;
  height: 50px;
  border: 4px solid #e0e0e0;
  border-top-color: #0078d4;
  border-radius: 50%;
  animation: rotate-ring 1s linear infinite;
}

@keyframes rotate-ring {
  to {
    transform: rotate(360deg);
  }
}
```

### Pattern 2: Pulsing Dot Spinner

**HTML Template:**
```typescript
const pulsingTemplate = `
  <div style="width:100%;height:100%" class="pulsing-spinner">
    <div class="pulse-dot"></div>
  </div>
`;
```

**CSS Styling:**
```css
.pulsing-spinner {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 100%;
}

.pulsing-spinner .pulse-dot {
  width: 40px;
  height: 40px;
  background: #107c10;
  border-radius: 50%;
  animation: pulse-animation 2s ease-in-out infinite;
}

@keyframes pulse-animation {
  0%, 100% {
    opacity: 1;
    transform: scale(1);
  }
  50% {
    opacity: 0.3;
    transform: scale(0.8);
  }
}
```

### Pattern 3: Skeleton Screen Loader

**HTML Template:**
```typescript
const skeletonTemplate = `
  <div style="width:100%;height:100%;padding:20px;" class="skeleton-loader">
    <div class="skeleton-line"></div>
    <div class="skeleton-line"></div>
    <div class="skeleton-line"></div>
  </div>
`;
```

**CSS Styling:**
```css
.skeleton-loader {
  width: 100%;
  height: 100%;
}

.skeleton-line {
  height: 12px;
  background: linear-gradient(
    90deg,
    #f0f0f0 25%,
    #e0e0e0 50%,
    #f0f0f0 75%
  );
  background-size: 200% 100%;
  animation: skeleton-loading 2s infinite;
  margin-bottom: 10px;
  border-radius: 4px;
}

@keyframes skeleton-loading {
  0% {
    background-position: 200% 0;
  }
  100% {
    background-position: -200% 0;
  }
}
```

### Pattern 4: Multiple Rings Spinner

**HTML Template:**
```typescript
const multiRingTemplate = `
  <div style="width:100%;height:100%" class="multi-ring-spinner">
    <div class="outer-ring"></div>
    <div class="inner-ring"></div>
  </div>
`;
```

**CSS Styling:**
```css
.multi-ring-spinner {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 100%;
}

.multi-ring-spinner div {
  position: absolute;
  border: 3px solid transparent;
  border-radius: 50%;
}

.outer-ring {
  width: 60px;
  height: 60px;
  border-top-color: #0078d4;
  animation: rotate-ring 2s linear infinite;
}

.inner-ring {
  width: 40px;
  height: 40px;
  border-top-color: #50e6ff;
  animation: rotate-ring-reverse 2s linear infinite;
}

@keyframes rotate-ring {
  to { transform: rotate(360deg); }
}

@keyframes rotate-ring-reverse {
  to { transform: rotate(-360deg); }
}
```

### Pattern 5: Bouncing Balls Spinner

**HTML Template:**
```typescript
const bouncingTemplate = `
  <div style="width:100%;height:100%" class="bouncing-spinner">
    <div class="bounce-ball ball-1"></div>
    <div class="bounce-ball ball-2"></div>
    <div class="bounce-ball ball-3"></div>
  </div>
`;
```

**CSS Styling:**
```css
.bouncing-spinner {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  width: 100%;
  height: 100%;
}

.bounce-ball {
  width: 12px;
  height: 12px;
  background: #0078d4;
  border-radius: 50%;
  animation: bounce 1.4s infinite ease-in-out both;
}

.ball-1 {
  animation-delay: -0.32s;
}

.ball-2 {
  animation-delay: -0.16s;
}

@keyframes bounce {
  0%, 80%, 100% {
    transform: scale(0);
    opacity: 0.5;
  }
  40% {
    transform: scale(1);
    opacity: 1;
  }
}
```

---

## Integration with Components

### Using Custom Template with Grid

```typescript
import { setSpinner } from '@syncfusion/ej2-popups';
import { Grid } from '@syncfusion/ej2-grids';

// Set custom template BEFORE creating Grid
setSpinner({
  template: `
    <div style="width:100%;height:100%" class="custom-grid-spinner">
      <div class="spinner-ring"></div>
      <p>Loading data...</p>
    </div>
  `
});

// Now create Grid - it will use custom spinner
const grid = new Grid({
  dataSource: remoteData,
  columns: [
    { field: 'OrderID', headerText: 'Order ID', width: 120 },
    { field: 'CustomerID', headerText: 'Customer ID', width: 150 },
  ]
});

grid.appendTo('#GridContainer');
```

### Multiple Components with Same Template

```typescript
import { setSpinner } from '@syncfusion/ej2-popups';
import { Grid } from '@syncfusion/ej2-grids';
import { TreeGrid } from '@syncfusion/ej2-treegrid';

const customTemplate = `
  <div style="width:100%;height:100%" class="app-spinner">
    <div class="loader"></div>
  </div>
`;

// Set template once
setSpinner({ template: customTemplate });

// All components created after this use same template
const grid = new Grid(gridConfig);
const treeGrid = new TreeGrid(treeGridConfig);
```

### Different Templates for Different Components

```typescript
// Component 1 uses template A
setSpinner({ template: templateA });
const grid1 = new Grid(config1);

// Component 2 uses template B (replaces A globally)
setSpinner({ template: templateB });
const grid2 = new Grid(config2);

// Now all NEW components will use template B
```

---

## Performance Optimization

### CSS-Only Animations (Recommended)

Use CSS animations instead of JavaScript for better performance:

```typescript
// ✓ GOOD: CSS animation (GPU accelerated)
const efficientTemplate = `
  <div style="width:100%;height:100%" class="efficient-spinner">
    <div class="spinner"></div>
  </div>
`;

setSpinner({ template: efficientTemplate });
```

```css
.efficient-spinner {
  display: flex;
  align-items: center;
  justify-content: center;
}

.spinner {
  width: 40px;
  height: 40px;
  border: 4px solid #e0e0e0;
  border-top-color: #0078d4;
  border-radius: 50%;
  animation: spin 1s linear infinite;
  will-change: transform;
}

@keyframes spin {
  to { transform: rotate(360deg); }
}
```

### Minimize DOM Elements

```typescript
// ✓ GOOD: Single element
const minimal = `<div class="spinner" style="width:100%;height:100%"></div>`;

// ✗ INEFFICIENT: Multiple nested elements
const complex = `
  <div style="width:100%;height:100%">
    <div><div><div class="spinner"></div></div></div>
  </div>
`;
```

### Use will-change Carefully

```css
/* Use will-change only for animated elements */
.spinner-ring {
  animation: rotate 1s linear infinite;
  will-change: transform;  /* Optimizes this property */
}

/* Don't overuse will-change */
.should-not-use {
  will-change: width, height, background;  /* Too many properties */
}
```

---

## Best Practices

### 1. Set Template Before Component Creation

```typescript
// ✓ CORRECT
setSpinner({ template: myTemplate });
let grid = new Grid(config);

// ✗ WRONG
let grid = new Grid(config);
setSpinner({ template: myTemplate });  // Too late!
```

### 2. Use Hardware Acceleration

```css
/* ✓ GOOD: GPU accelerated */
@keyframes spin {
  to { transform: rotate(360deg); }
}

/* ✗ POOR: CPU intensive */
@keyframes expand {
  0% { width: 0; }
  100% { width: 100px; }
}
```

### 3. Keep Animations Smooth

```css
/* Use consistent timing */
.spinner {
  animation: spin 1s linear infinite;  /* ✓ Consistent */
}

.poor-animation {
  animation: spin 1s ease-in-out infinite;  /* ✗ Jerky in loop */
}
```

### 4. Responsive Templates

```typescript
const responsiveTemplate = `
  <div style="width:100%;height:100%" class="responsive-spinner">
    <div class="spinner"></div>
  </div>
`;

setSpinner({ template: responsiveTemplate });
```

```css
.responsive-spinner {
  display: flex;
  align-items: center;
  justify-content: center;
}

.spinner {
  width: 40px;
  height: 40px;
  border: 4px solid #e0e0e0;
  border-top-color: #0078d4;
  border-radius: 50%;
  animation: spin 1s linear infinite;
}

/* Adjust for mobile */
@media (max-width: 600px) {
  .spinner {
    width: 30px;
    height: 30px;
    border-width: 3px;
  }
}
```

---

## Troubleshooting

### Custom Template Not Showing

**Issue:** Component displays default spinner, not custom template.

**Solutions:**
1. Verify `setSpinner()` called **before** component creation
2. Check template HTML syntax is valid
3. Confirm CSS file is loaded:
   ```typescript
   // Ensure CSS is imported in main file
   import './styles.css';
   
   setSpinner({ template: myTemplate });
   ```

### Animation Not Displaying

**Issue:** Template appears but animation doesn't run.

**Solutions:**
1. Verify `@keyframes` is defined in CSS
2. Check animation property on element:
   ```css
   .spinner {
     animation: my-animation 1s linear infinite;  /* Must specify */
   }
   ```

3. Ensure CSS file is loaded

### Template Looks Different in Different Browsers

**Issue:** Animation or styling appears different in Chrome vs Firefox.

**Solutions:**
1. Use vendor prefixes for older browsers:
   ```css
   @keyframes spin {
     to { -webkit-transform: rotate(360deg); transform: rotate(360deg); }
   }
   ```

2. Test on target browsers

### Performance Issues with Custom Template

**Issue:** Spinner animation is choppy or causes lag.

**Solutions:**
1. Use `will-change` property:
   ```css
   .spinner {
     animation: spin 1s linear infinite;
     will-change: transform;
   }
   ```

2. Minimize DOM elements in template
3. Use CSS animations instead of JavaScript
4. Avoid animating expensive properties (width, height, position)

### Template Shows for Too Long

**Issue:** Spinner remains visible after operation completes.

**Solution:** Verify `hideSpinner()` is called:
```typescript
try {
  // Show spinner and load data
  showSpinner(container);
  const data = await fetchData();
} finally {
  hideSpinner(container);  // Always hide
}
```

---

## Related Resources

- [spinner-methods.md](spinner-methods.md) - Learn about show/hide operations
- [theming-and-styling.md](theming-and-styling.md) - Explore built-in themes
- [advanced-usage.md](advanced-usage.md) - Integration with async operations
- [getting-started.md](getting-started.md) - Basic setup and configuration
