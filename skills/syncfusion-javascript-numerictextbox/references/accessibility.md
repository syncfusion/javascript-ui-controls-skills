# Accessibility in NumericTextBox

## Table of Contents
- [Overview](#overview)
- [WCAG Compliance](#wcag-compliance)
- [WAI-ARIA Attributes](#wai-aria-attributes)
- [Keyboard Navigation](#keyboard-navigation)
- [Screen Reader Support](#screen-reader-support)
- [Mobile Accessibility](#mobile-accessibility)
- [Color Contrast](#color-contrast)
- [Testing and Validation](#testing-and-validation)
- [Accessible Form Examples](#accessible-form-examples)

## Overview

The NumericTextBox component is built with accessibility as a core principle, supporting WCAG 2.2 AA standards, Section 508 compliance, and comprehensive screen reader support. Keyboard navigation, ARIA attributes, and semantic HTML ensure the control is usable by all users, including those with disabilities.

## WCAG Compliance

The NumericTextBox meets WCAG 2.2 Level AA requirements:

| Standard | Support | Details |
|----------|---------|---------|
| WCAG 2.2 | AA | Web Content Accessibility Guidelines Level AA |
| Section 508 | ✓ | US federal accessibility standard |
| Screen Readers | ✓ | NVDA, JAWS, VoiceOver compatible |
| Keyboard Only | ✓ | Full functionality without mouse |
| Color Contrast | ✓ | WCAG minimum contrast ratios |
| Mobile Devices | ✓ | Touch-friendly and screen reader compatible |

## WAI-ARIA Attributes

The NumericTextBox uses the `spinbutton` role with ARIA properties to communicate state to assistive technology:

### ARIA Role

```html
<input role="spinbutton" aria-valuemin="0" aria-valuemax="100" aria-valuenow="50" />
```

The `spinbutton` role indicates an input field for numeric values with increment/decrement functionality.

### ARIA Properties

| Property | Purpose | Example |
|----------|---------|---------|
| `aria-valuemin` | Minimum allowed value | `aria-valuemin="0"` |
| `aria-valuemax` | Maximum allowed value | `aria-valuemax="100"` |
| `aria-valuenow` | Current value | `aria-valuenow="50"` |
| `aria-disabled` | Disabled state | `aria-disabled="false"` |
| `aria-readonly` | Read-only state | `aria-readonly="false"` |
| `aria-invalid` | Validation error state | `aria-invalid="false"` |
| `aria-label` | Descriptive label | `aria-label="Discount percentage"` |
| `aria-describedby` | Link to help text | `aria-describedby="help-text-id"` |
| `aria-live` | Update announcement | `aria-live="polite"` |

### Implementing ARIA Attributes

```typescript
import { NumericTextBox } from '@syncfusion/ej2-inputs';

const numericTextBox = new NumericTextBox({
  value: 50,
  min: 0,
  max: 100,
  step: 5,
  // aria-label is automatically generated but can be customized
  change: (args) => {
    console.log('Value changed to:', args.value);
  }
});

numericTextBox.appendTo('#quantity-field');

// The component automatically sets:
// - role="spinbutton"
// - aria-valuemin, aria-valuemax, aria-valuenow
// - aria-disabled based on enabled state
// - aria-readonly based on readonly state
```

### aria-label for Context

Provide descriptive labels for screen readers:

```html
<label for="discount">Discount Percentage:</label>
<input id="discount" aria-label="Discount percentage (0-100%)" />
```

```typescript
const discountInput = new NumericTextBox({
  value: 0.15,
  format: 'p0',
  min: 0,
  max: 1,
  // Label connected via HTML <label> element
});
discountInput.appendTo('#discount');
```

### aria-describedby for Help Text

Link to additional help text:

```html
<input id="tax-rate" aria-describedby="tax-help" />
<span id="tax-help">Enter tax rate as a decimal (e.g., 0.08 for 8%)</span>
```

```typescript
const taxInput = new NumericTextBox({
  value: 0.08,
  format: 'p2',
  locale: 'en-US'
});
taxInput.appendTo('#tax-rate');
```

## Keyboard Navigation

The NumericTextBox supports standard keyboard interactions:

### Arrow Keys

| Key | Action | Details |
|-----|--------|---------|
| <kbd>Arrow Up</kbd> | Increment | Increases value by step amount |
| <kbd>Arrow Down</kbd> | Decrement | Decreases value by step amount |
| <kbd>Tab</kbd> | Move focus | Standard tabbing behavior |
| <kbd>Shift+Tab</kbd> | Previous | Move to previous focusable element |
| <kbd>Home</kbd> | Jump to min | Move to minimum value |
| <kbd>End</kbd> | Jump to max | Move to maximum value |

### Keyboard Example

```typescript
const numericTextBox = new NumericTextBox({
  value: 50,
  min: 0,
  max: 100,
  step: 5
});
numericTextBox.appendTo('#keyboard-test');

// User interaction:
// 1. Tab to focus the field
// 2. Arrow Up: 55 (incremented by 5)
// 3. Arrow Down: 50 (decremented by 5)
// 4. Home: 0 (minimum value)
// 5. End: 100 (maximum value)
```

### Custom Keyboard Handling

Listen to keyboard events for additional functionality:

```typescript
const numericTextBox = new NumericTextBox({
  value: 0,
  keydown: (args) => {
    if (args.key === 'Enter') {
      console.log('Enter pressed with value:', numericTextBox.value);
      // Submit form or trigger action
    }
  }
});
numericTextBox.appendTo('#numeric');
```

## Screen Reader Support

NumericTextBox announces changes to screen readers in real-time:

### JAWS / NVDA

When focusing the field, the screen reader announces:
```
"Quantity, edit text, spinbutton, 50, minimum 0, maximum 100"
```

When incrementing with Arrow Up:
```
"55"
```

### VoiceOver (macOS/iOS)

```
"Quantity, spinbutton, value 50"
```

### Screen Reader Testing

To test with screen readers:
1. **Windows:** Use NVDA (free) or JAWS
2. **macOS:** Use VoiceOver (built-in, activate with Cmd+F5)
3. **Mobile:** Use platform screen reader (TalkBack on Android, VoiceOver on iOS)

### aria-live for Dynamic Updates

Use `aria-live` to announce value changes:

```typescript
const numericTextBox = new NumericTextBox({
  value: 0,
  change: (args) => {
    // aria-live region announces: "Value updated to 25"
    console.log('Value changed:', args.value);
  }
});

// The component automatically manages aria-live announcements
numericTextBox.appendTo('#live-region');
```

## Mobile Accessibility

### Touch Input

The NumericTextBox supports touch input on mobile devices:
- Tap to focus and open keyboard
- Spin buttons work with touch gestures
- Zoom and text sizing respect browser settings

### Mobile Screen Readers

Fully compatible with:
- **TalkBack** (Android)
- **VoiceOver** (iOS)
- **Narrator** (Windows Mobile)

### Accessible Mobile Implementation

```typescript
const numericTextBox = new NumericTextBox({
  value: 0,
  min: 0,
  max: 100,
  step: 1,
  showSpinButton: true,  // Spin buttons work with touch
  enableRtl: false,      // RTL support for languages
  readonly: false        // Allow mobile input
});
numericTextBox.appendTo('#mobile-quantity');
```

## Color Contrast

The NumericTextBox meets WCAG color contrast requirements (4.5:1 for normal text):

### Default Theme Contrast

- **Foreground:** #333 on Light themes
- **Background:** #fff on Light themes
- **Focus Indicator:** Blue highlight with 4.5:1 contrast
- **Error State:** Red text with proper contrast

### Custom Contrast

Ensure custom styling maintains minimum contrast:

```css
/* GOOD: High contrast */
.e-input-group input.e-input {
  color: #333;
  background-color: #fff;
  /* Contrast ratio: 12.6:1 ✓ */
}

/* POOR: Low contrast */
.e-input-group input.e-input {
  color: #ccc;
  background-color: #e0e0e0;
  /* Contrast ratio: 1.4:1 ✗ */
}
```

### Focus Indicator Contrast

Ensure visible focus state:

```css
.e-input-group.e-input-focus input.e-input {
  outline: 2px solid #0066cc;
  outline-offset: 2px;
  /* High contrast visible focus */
}
```

## Testing and Validation

### Automated Testing Tools

Use these tools to validate accessibility:

1. **accessibility-checker** (npm package)
   ```bash
   npm install accessibility-checker
   ```

2. **axe-core** (npm package)
   ```bash
   npm install axe-core
   ```

3. **WAVE Browser Extension** (Free online)
   - Analyze pages for accessibility issues

### Manual Testing Checklist

- [ ] Keyboard navigation works (Tab, Arrow keys, Home, End)
- [ ] Screen reader announces all information
- [ ] Focus visible at all times
- [ ] Color contrast meets WCAG AA (4.5:1)
- [ ] Touch targets large enough (44x44 px minimum)
- [ ] Error messages clear and associated with field
- [ ] Works in multiple screen readers (JAWS, NVDA, VoiceOver)
- [ ] RTL layout works if applicable

### Testing Implementation

```typescript
import { NumericTextBox } from '@syncfusion/ej2-inputs';
import axe from 'axe-core';

const numericTextBox = new NumericTextBox({
  value: 100,
  min: 0,
  max: 1000,
  format: 'c2',
  locale: 'en-US'
});
numericTextBox.appendTo('#numeric-test');

// Run accessibility checker
axe.run((results) => {
  if (results.violations.length === 0) {
    console.log('✓ Accessibility test passed');
  } else {
    console.log('✗ Accessibility issues found:', results.violations);
  }
});
```

## Accessible Form Examples

### Complete Accessible Monetary Input

```typescript
import { NumericTextBox } from '@syncfusion/ej2-inputs';

// HTML structure
const html = `
  <form id="payment-form">
    <div class="form-group">
      <label for="amount">Payment Amount (USD):</label>
      <input 
        id="amount" 
        type="text"
        aria-describedby="amount-help"
      />
      <span id="amount-help" class="help-text">
        Enter the amount in US dollars. Minimum: $0.01, Maximum: $999,999.99
      </span>
    </div>
  </form>
`;

// Initialize with accessibility
const amountInput = new NumericTextBox({
  value: 100.00,
  format: 'c2',
  locale: 'en-US',
  min: 0.01,
  max: 999999.99,
  step: 0.01,
  change: (args) => {
    console.log('Amount changed:', args.value);
  }
});

amountInput.appendTo('#amount');
```

### Accessible Percentage Input

```typescript
const discountInput = new NumericTextBox({
  value: 0.15,
  format: 'p0',
  min: 0,
  max: 1,
  step: 0.05,
  change: (args) => {
    updateTotal(args.value);
  }
});

discountInput.appendTo('#discount-percentage');

// HTML
/*
<label for="discount-percentage">Discount Percentage:</label>
<input id="discount-percentage" type="text" />
<span id="discount-help">Valid range: 0% to 100%, increments of 5%</span>
*/
```

### Form Validation with Accessibility

```typescript
const quantityInput = new NumericTextBox({
  value: 1,
  min: 1,
  max: 100,
  step: 1,
  change: (args) => {
    const errorDiv = document.getElementById('quantity-error');
    if (args.value < 1) {
      quantityInput.element.setAttribute('aria-invalid', 'true');
      errorDiv.textContent = 'Quantity must be at least 1';
    } else {
      quantityInput.element.setAttribute('aria-invalid', 'false');
      errorDiv.textContent = '';
    }
  }
});

quantityInput.appendTo('#quantity');

// HTML
/*
<label for="quantity">Order Quantity:</label>
<input 
  id="quantity" 
  type="text"
  aria-describedby="quantity-error"
/>
<div id="quantity-error" role="alert"></div>
*/
```

## Next Steps

- Learn [Styling](styling-and-customization.md) while maintaining contrast
- Implement [Advanced Patterns](advanced-patterns.md) with accessible validation
- Test with real assistive technologies and gather user feedback
