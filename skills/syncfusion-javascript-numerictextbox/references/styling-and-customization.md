# Styling and Customization

## Table of Contents
- [Overview](#overview)
- [CSS Class Structure](#css-class-structure)
- [Wrapper Customization](#wrapper-customization)
- [Spin Button Styling](#spin-button-styling)
- [Input Styling](#input-styling)
- [Theme Integration](#theme-integration)
- [CSS Variables](#css-variables)
- [Practical Styling Examples](#practical-styling-examples)
- [Responsive Design](#responsive-design)

## Overview

The NumericTextBox uses a consistent CSS class structure that allows deep customization. You can modify appearance at multiple levels: the wrapper container, the input field, or individual spin buttons.

## CSS Class Structure

### DOM Structure

The NumericTextBox renders with this HTML structure:

```html
<div class="e-input-group e-numeric">
  <!-- Prepended adornments -->
  <span class="e-adornment">$</span>
  
  <!-- Main input -->
  <input type="text" class="e-input" />
  
  <!-- Spin buttons -->
  <div class="e-input-group-icon e-numeric-up-icon"></div>
  <div class="e-input-group-icon e-numeric-down-icon"></div>
  
  <!-- Appended adornments -->
  <span class="e-adornment">kg</span>
</div>
```

### Main Classes

| Class | Target | Purpose |
|-------|--------|---------|
| `.e-input-group` | Wrapper | Container for all elements |
| `.e-numeric` | Wrapper | Numeric-specific styling |
| `.e-input` | Input field | Main input element |
| `.e-input-group-icon` | Spin button | Increment/decrement button |
| `.e-numeric-up-icon` | Spin up | Increment button icon |
| `.e-numeric-down-icon` | Spin down | Decrement button icon |
| `.e-adornment` | Prefix/Suffix | Template elements |
| `.e-input-focus` | Wrapper (focused) | Focus state styling |
| `.e-input-disabled` | Wrapper (disabled) | Disabled state styling |
| `.e-input-readonly` | Wrapper (readonly) | Read-only state styling |

## Wrapper Customization

### Basic Wrapper Styling

Customize the outer container:

```css
/* Set height and padding */
.e-input-group {
  height: 40px;
  padding: 8px 12px;
  border: 1px solid #ddd;
  border-radius: 4px;
  background-color: #fff;
}

/* Focused state */
.e-input-group.e-input-focus {
  border-color: #007bff;
  box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.25);
}

/* Disabled state */
.e-input-group.e-input-disabled {
  background-color: #f5f5f5;
  border-color: #ccc;
}

/* Read-only state */
.e-input-group.e-input-readonly {
  background-color: #f9f9f9;
  border-color: #ddd;
}
```

### Hover Effects

Add visual feedback on hover:

```css
.e-input-group:hover:not(.e-input-disabled) {
  border-color: #bbb;
  background-color: #fafafa;
}

.e-input-group:hover:not(.e-input-disabled) .e-input-group-icon {
  visibility: visible;
  opacity: 1;
}
```

## Spin Button Styling

### Icon Size and Color

Customize the increment/decrement button icons:

```css
/* Up and down button styling */
.e-input-group-icon.e-numeric-up-icon,
.e-input-group-icon.e-numeric-down-icon {
  font-size: 20px;
  color: #666;
  cursor: pointer;
  transition: color 0.2s ease;
}

/* Hover state */
.e-input-group-icon:hover {
  color: #007bff;
  background-color: #e7f3ff;
}

/* Active state */
.e-input-group-icon:active {
  color: #0056b3;
  background-color: #cce5ff;
}
```

### Hide Spin Buttons

Completely hide increment/decrement buttons:

```css
/* Hide both buttons */
.e-input-group-icon.e-numeric-up-icon,
.e-input-group-icon.e-numeric-down-icon {
  display: none;
}

/* Or hide via TypeScript */
const numericTextBox = new NumericTextBox({
  value: 100,
  showSpinButton: false  // Disables buttons
});
```

### Customize Individual Buttons

Style up and down buttons differently:

```css
/* Larger up button */
.e-input-group-icon.e-numeric-up-icon {
  font-size: 24px;
  color: #28a745;
}

/* Smaller down button */
.e-input-group-icon.e-numeric-down-icon {
  font-size: 16px;
  color: #dc3545;
}
```

### Button Background

Add background color to spin buttons:

```css
.e-input-group-icon {
  background-color: #f0f0f0;
  border-left: 1px solid #ddd;
  border-radius: 0 4px 4px 0;
}

.e-input-group-icon:hover {
  background-color: #e0e0e0;
}
```

## Input Styling

### Font and Size

Customize the input field text:

```css
.e-input-group input.e-input {
  font-size: 16px;
  font-family: 'Segoe UI', sans-serif;
  font-weight: 500;
  height: 40px;
  padding: 0 12px;
  color: #333;
  background-color: #fff;
}
```

### Text Alignment

Align numeric values:

```css
/* Right-align for currency/numbers */
.e-input-group input.e-input {
  text-align: right;
}

/* Center alignment */
.e-input-group input.e-input {
  text-align: center;
}
```

### Placeholder Styling

Style the placeholder text:

```css
.e-input-group input.e-input::placeholder {
  color: #aaa;
  font-style: italic;
}

.e-input-group input.e-input:focus::placeholder {
  color: #ddd;
}
```

### Focus Ring

Customize the focus indicator:

```css
.e-input-group input.e-input:focus {
  outline: 2px solid #007bff;
  outline-offset: 2px;
}

/* Or use box-shadow */
.e-input-group.e-input-focus input.e-input {
  box-shadow: inset 0 0 0 2px #007bff;
}
```

## Theme Integration

### Available Themes

Import the theme of your choice:

```typescript
// Material Design (default)
import '@syncfusion/ej2-inputs/styles/material.css';

// Bootstrap 5
import '@syncfusion/ej2-inputs/styles/bootstrap5.css';

// Bootstrap 4
import '@syncfusion/ej2-inputs/styles/bootstrap4.css';

// Fabric
import '@syncfusion/ej2-inputs/styles/fabric.css';

// High Contrast
import '@syncfusion/ej2-inputs/styles/highcontrast.css';

// Tailwind CSS
import '@syncfusion/ej2-inputs/styles/tailwind.css';
```

### Theme-Specific Overrides

Override theme variables after importing:

```css
/* Override Material theme */
:root {
  --primary-color: #2196F3;
  --primary-hover: #1976D2;
  --border-color: #bdbdbd;
  --focus-color: #1565C0;
}

/* Bootstrap theme overrides */
:root {
  --bs-primary: #0d6efd;
  --bs-border-color: #dee2e6;
}
```

## CSS Variables

### Root CSS Variables

Use CSS custom properties for dynamic theming:

```css
:root {
  /* Colors */
  --numeric-text-color: #333;
  --numeric-bg-color: #fff;
  --numeric-border-color: #ddd;
  --numeric-focus-color: #007bff;
  --numeric-error-color: #dc3545;
  
  /* Spacing */
  --numeric-padding: 8px 12px;
  --numeric-border-radius: 4px;
  
  /* Typography */
  --numeric-font-size: 16px;
  --numeric-font-family: 'Segoe UI', sans-serif;
}

.e-input-group {
  color: var(--numeric-text-color);
  background-color: var(--numeric-bg-color);
  border: 1px solid var(--numeric-border-color);
  border-radius: var(--numeric-border-radius);
  padding: var(--numeric-padding);
  font-family: var(--numeric-font-family);
  font-size: var(--numeric-font-size);
}

.e-input-group.e-input-focus {
  border-color: var(--numeric-focus-color);
}

.e-input-group input.e-input.e-invalid {
  border-color: var(--numeric-error-color);
}
```

### Scoped Variables

Apply theme to specific instances:

```html
<input id="price-input" class="premium-style" />
<input id="discount-input" class="minimal-style" />
```

```css
/* Premium style */
.premium-style {
  --numeric-border-radius: 8px;
  --numeric-font-size: 18px;
  --numeric-focus-color: #ff6b6b;
}

/* Minimal style */
.minimal-style {
  --numeric-border-radius: 0;
  --numeric-border-color: transparent;
  --numeric-focus-color: #333;
}
```

## Practical Styling Examples

### Monetary Input Styling

Style for currency input:

```css
.monetary-input .e-input-group {
  height: 48px;
  padding: 12px 16px;
  border: 2px solid #ddd;
  border-radius: 6px;
  font-size: 18px;
}

.monetary-input .e-input-group.e-input-focus {
  border-color: #28a745;
  box-shadow: 0 0 0 3px rgba(40, 167, 69, 0.1);
}

.monetary-input .e-input {
  text-align: right;
  font-weight: 600;
  color: #28a745;
}

.monetary-input .e-adornment {
  color: #28a745;
  font-weight: bold;
  font-size: 20px;
}
```

### Error State Styling

Style validation errors:

```css
.e-input-group.input-error {
  border-color: #dc3545;
  background-color: #fff5f5;
}

.e-input-group.input-error input.e-input {
  color: #dc3545;
}

.e-input-group.input-error .e-input-group-icon {
  color: #dc3545;
}

/* Error message */
.error-message {
  color: #dc3545;
  font-size: 12px;
  margin-top: 4px;
  display: block;
}
```

### Compact Input Styling

Minimal, compact design:

```css
.compact-input .e-input-group {
  height: 32px;
  padding: 4px 8px;
  border: 1px solid #ccc;
  border-radius: 2px;
  font-size: 13px;
}

.compact-input .e-input {
  padding: 0 6px;
}

.compact-input .e-input-group-icon {
  font-size: 14px;
}
```

### Large Touch-Friendly Input

Optimized for mobile/touch:

```css
.touch-input .e-input-group {
  height: 56px;
  padding: 12px 16px;
  border: 2px solid #007bff;
  border-radius: 8px;
  font-size: 18px;
}

.touch-input .e-input {
  font-size: 18px;
  min-height: 44px;  /* WCAG minimum touch target */
}

.touch-input .e-input-group-icon {
  font-size: 28px;
  padding: 0 12px;
  min-width: 44px;  /* WCAG minimum touch target */
  min-height: 44px;
}
```

## Responsive Design

### Media Query Styling

Adapt styles for different screen sizes:

```css
/* Desktop */
@media (min-width: 1024px) {
  .e-input-group {
    height: 40px;
    font-size: 16px;
  }
}

/* Tablet */
@media (min-width: 768px) and (max-width: 1023px) {
  .e-input-group {
    height: 44px;
    font-size: 16px;
  }
}

/* Mobile */
@media (max-width: 767px) {
  .e-input-group {
    height: 48px;
    font-size: 16px;
    padding: 12px 14px;
  }

  .e-input-group-icon {
    font-size: 24px;
  }
}
```

### Fluid Spacing

Use relative units for responsive sizing:

```css
.e-input-group {
  height: clamp(32px, 6vw, 56px);
  padding: clamp(4px, 1vw, 12px);
  font-size: clamp(12px, 2vw, 18px);
}
```

### Dark Mode

Support dark theme:

```css
@media (prefers-color-scheme: dark) {
  :root {
    --numeric-text-color: #e0e0e0;
    --numeric-bg-color: #1e1e1e;
    --numeric-border-color: #404040;
    --numeric-focus-color: #66bb6a;
  }

  .e-input-group {
    color: var(--numeric-text-color);
    background-color: var(--numeric-bg-color);
    border-color: var(--numeric-border-color);
  }
}
```

## Best Practices

**Maintain contrast:** Ensure text color vs background has at least 4.5:1 contrast ratio.

**Provide focus indicators:** Always show visible focus state for keyboard accessibility.

**Test responsive:** Verify styling works across breakpoints and devices.

**Use semantic HTML:** Pair with proper `<label>` elements and ARIA attributes.

**Performance:** Avoid expensive CSS operations like transitions on frequently-updated values.

**Consistency:** Use CSS variables for theming to maintain consistency across components.

## Next Steps

- Combine styling with [Adornments](adornments-and-ui.md) for visual context
- Ensure [Accessibility](accessibility.md) with proper contrast and focus indicators
- Implement [Advanced Patterns](advanced-patterns.md) with custom validation styles
