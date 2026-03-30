---
name: syncfusion-javascript-numerictextbox
description: Implement numeric input controls with validation, formatting, and accessibility. Use this skill when the user needs a NumericTextBox component for accepting numeric input, applying number formats (currency, percentage, custom patterns), adding adornments (currency symbols, unit labels), or ensuring accessibility with ARIA attributes. Also refer to this skill for keyboard navigation, validation patterns, or styling customization.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing NumericTextBox

The NumericTextBox is a specialized input control for capturing numeric values with built-in formatting, validation, and accessibility features. It provides a user-friendly way to handle numeric input with support for decimal values, custom formats, and currency symbols.

## When to Use This Skill

Use the NumericTextBox control when you need to:
- Accept numeric input with validation
- Format numbers as currency, percentages, or custom patterns
- Add currency symbols or unit labels (adornments)
- Provide keyboard navigation with arrow keys
- Ensure WCAG-compliant accessibility with ARIA attributes
- Customize the appearance and behavior of numeric inputs
- Handle edge cases like trailing zeros or nullable values

## Control Overview

**NumericTextBox** is an input component for numeric values that:
- Validates numeric input in real-time
- Formats output (currency, percentage, patterns)
- Supports increment/decrement with spin buttons or arrow keys
- Accessible to screen readers and keyboard users
- Customizable with CSS, themes, and templates
- Works with two-way binding and form validation

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation and package setup
- Basic NumericTextBox implementation
- CSS imports and theme selection
- Core properties (value, min, max, step)
- Change events and initialization
- RTL support configuration

### Number Formatting
📄 **Read:** [references/number-formatting.md](references/number-formatting.md)
- Standard format specifiers (n for numbers, c for currency, p for percentage)
- Currency formatting with locale support
- Percentage calculations and display
- Custom numeric format strings
- Format string patterns and specifiers
- Globalization and locale handling
- Format behavior on blur

### Adornments and UI
📄 **Read:** [references/adornments-and-ui.md](references/adornments-and-ui.md)
- Adding prefix elements with prependTemplate (currency symbols, icons)
- Adding suffix elements with appendTemplate (unit labels, action buttons)
- Common use cases (monetary inputs, measurements)
- Integration with float labels
- Styling adornment elements

### Accessibility
📄 **Read:** [references/accessibility.md](references/accessibility.md)
- WCAG 2.2 compliance and Section 508 support
- WAI-ARIA attributes (spinbutton role)
- aria-valuemin, aria-valuemax, aria-valuenow properties
- aria-disabled, aria-readonly, aria-invalid states
- Keyboard shortcuts (Arrow Up/Down for increment/decrement)
- Screen reader support
- Mobile accessibility
- Testing with accessibility tools

### Styling and Customization
📄 **Read:** [references/styling-and-customization.md](references/styling-and-customization.md)
- CSS class structure and wrapper elements
- Customizing the input group wrapper
- Styling spin buttons (up/down icons)
- Height, font-size, and spacing customization
- Border, background color, and shadow effects
- Theme Studio integration
- CSS variables for theming

### Advanced Patterns
📄 **Read:** [references/advanced-patterns.md](references/advanced-patterns.md)
- Custom validation with form validator
- Preventing nullable input
- Maintaining trailing zeros
- Step value configuration and customization
- Hiding or customizing spin buttons
- Event handling (change, focus, blur, keydown)
- Two-way binding and model binding
- Common edge cases and troubleshooting

## Quick Start Example

```typescript
import { NumericTextBox } from '@syncfusion/ej2-inputs';

// Create a container element
const element = document.getElementById('numerictextbox');

// Initialize basic NumericTextBox with formatting
const numericTextBox = new NumericTextBox({
  value: 100,
  min: 0,
  max: 1000,
  step: 5,
  format: 'n2',  // Display with 2 decimal places
  change: (args) => {
    console.log('New value:', args.value);
  }
});

numericTextBox.appendTo(element);
```

## Key Properties

| Property | Type | Purpose |
|----------|------|---------|
| `value` | number | Current numeric value |
| `min` | number | Minimum allowed value |
| `max` | number | Maximum allowed value |
| `step` | number | Increment/decrement step |
| `format` | string | Number format (n, c, p, or custom) |
| `decimals` | number | Decimal places to display |
| `enabled` | boolean | Enable/disable the control |
| `readonly` | boolean | Prevent user editing |
| `showSpinButton` | boolean | Display increment/decrement buttons |
| `prependTemplate` | string | HTML before the input |
| `appendTemplate` | string | HTML after the input |

## Common Use Cases

### Monetary Input
Add currency symbol as adornment and format with currency pattern:
```typescript
new NumericTextBox({
  format: 'c2',  // Currency with 2 decimals
  prependTemplate: '$',
  min: 0
}).appendTo('#price-input');
```

### Percentage Input
Format as percentage and customize step:
```typescript
new NumericTextBox({
  format: 'p0',  // Percentage
  step: 5,
  min: 0,
  max: 100
}).appendTo('#discount-input');
```

### Measurements with Units
Add unit label as suffix:
```typescript
new NumericTextBox({
  format: 'n2',
  appendTemplate: 'kg',
  min: 0
}).appendTo('#weight-input');
```

### Accessible Form Field
Include validation and ARIA labels:
```typescript
new NumericTextBox({
  value: 0,
  format: 'n0',
  min: 0,
  max: 100,
  change: validateInput
}).appendTo('#quantity-input');
```

## Common Patterns

**Decimal Precision:** Use `format: 'n2'` for two decimal places, `n4` for four, etc.

**Currency Support:** Set `format: 'c2'` and locale for automatic currency symbol formatting.

**Validation:** Combine `min`, `max` properties with form validator for comprehensive validation.

**Readonly Fields:** Use `readonly: true` to display formatted values without editing.

**Event Handling:** Listen to `change` event for value updates and `blur` event for format application.

---

**Next Steps:**
- Start with [Getting Started](references/getting-started.md) to install and set up
- Choose your [Number Format](references/number-formatting.md) strategy
- Add [Adornments](references/adornments-and-ui.md) for context
- Ensure [Accessibility](references/accessibility.md) compliance
- Implement [Styling](references/styling-and-customization.md) and branding
- Handle [Advanced Patterns](references/advanced-patterns.md) and edge cases
