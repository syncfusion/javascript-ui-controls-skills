# Getting Started with NumericTextBox

## Installation and Setup

The NumericTextBox component is part of the Syncfusion Essential JS 2 library. Install the required packages:

```bash
npm install @syncfusion/ej2-inputs
npm install @syncfusion/ej2-base
```

## CSS Imports

Import the default theme styles in your application:

```typescript
// Import default theme
import '@syncfusion/ej2-inputs/styles/material.css';

// Or import other themes:
// import '@syncfusion/ej2-inputs/styles/bootstrap.css';
// import '@syncfusion/ej2-inputs/styles/bootstrap4.css';
// import '@syncfusion/ej2-inputs/styles/fabric.css';
// import '@syncfusion/ej2-inputs/styles/highcontrast.css';
```

## Basic Implementation

### HTML Structure

Create a simple input element:

```html
<input id="numerictextbox" type="text" />
```

### TypeScript Initialization

Import and initialize the NumericTextBox:

```typescript
import { NumericTextBox } from '@syncfusion/ej2-inputs';

const element = document.getElementById('numerictextbox');
const numericTextBox = new NumericTextBox();
numericTextBox.appendTo(element);
```

## Core Properties

### Value Property

Set and manage the numeric value:

```typescript
const numericTextBox = new NumericTextBox({
  value: 100
});
numericTextBox.appendTo('#numerictextbox');
```

### Min and Max Range

Constrain the input to a range:

```typescript
const numericTextBox = new NumericTextBox({
  value: 50,
  min: 0,
  max: 100
});
numericTextBox.appendTo('#numerictextbox');
```

### Step Value

Configure increment/decrement step for arrow keys and spin buttons:

```typescript
const numericTextBox = new NumericTextBox({
  value: 10,
  step: 5,  // Increment by 5 when using arrow keys or spin buttons
  min: 0,
  max: 100
});
numericTextBox.appendTo('#numerictextbox');
```

### Decimals

Control decimal places displayed:

```typescript
const numericTextBox = new NumericTextBox({
  value: 123.456,
  decimals: 2  // Display as 123.46
});
numericTextBox.appendTo('#numerictextbox');
```

## Event Handling

### Change Event

Listen for value changes:

```typescript
const numericTextBox = new NumericTextBox({
  value: 0,
  change: (args) => {
    console.log('Previous value:', args.previousValue);
    console.log('Current value:', args.value);
  }
});
numericTextBox.appendTo('#numerictextbox');
```

### Focus and Blur Events

Handle focus and blur:

```typescript
const numericTextBox = new NumericTextBox({
  value: 100,
  focus: (args) => {
    console.log('NumericTextBox focused');
  },
  blur: (args) => {
    console.log('NumericTextBox blurred, formatted value:', args.value);
  }
});
numericTextBox.appendTo('#numerictextbox');
```

## State Management

### Enabled/Disabled State

Control whether the field is interactive:

```typescript
const numericTextBox = new NumericTextBox({
  value: 100,
  enabled: false  // Disabled by default
});
numericTextBox.appendTo('#numerictextbox');

// Enable later
numericTextBox.enabled = true;
```

### Read-only Mode

Prevent editing while displaying formatted value:

```typescript
const numericTextBox = new NumericTextBox({
  value: 1000,
  readonly: true  // Display only, no editing
});
numericTextBox.appendTo('#numerictextbox');
```

## Spin Buttons

Control the increment/decrement buttons:

```typescript
const numericTextBox = new NumericTextBox({
  value: 10,
  step: 1,
  showSpinButton: true,  // Show by default (default: true)
  min: 0,
  max: 100
});
numericTextBox.appendTo('#numerictextbox');
```

## RTL Support

Enable right-to-left layout for Arabic and Hebrew:

```typescript
const numericTextBox = new NumericTextBox({
  value: 100,
  enableRtl: true
});
numericTextBox.appendTo('#numerictextbox');
```

## Complete Quick Start Example

```typescript
import { NumericTextBox } from '@syncfusion/ej2-inputs';
import '@syncfusion/ej2-inputs/styles/material.css';

// Initialize with common properties
const priceInput = new NumericTextBox({
  value: 50,
  min: 0,
  max: 1000,
  step: 10,
  decimals: 2,
  enabled: true,
  readonly: false,
  showSpinButton: true,
  change: (args) => {
    console.log('Price changed to:', args.value);
  }
});

priceInput.appendTo('#price-input');
```

## Common Issues

**No value displayed:** Ensure you've imported the CSS theme file.

**Spin buttons not working:** Verify `showSpinButton: true` and check that min/max/step are properly configured.

**Format not applied:** The format is applied on blur. Use the `format` property from number-formatting.md for specific format patterns.

## Next Steps

- Learn [Number Formatting](number-formatting.md) to apply currency or percentage formats
- Add [Adornments](adornments-and-ui.md) for currency symbols or unit labels
- Implement [Validation](advanced-patterns.md) for business rules
- Ensure [Accessibility](accessibility.md) compliance
