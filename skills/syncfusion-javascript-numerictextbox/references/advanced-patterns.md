# Advanced Patterns and Customization

## Table of Contents
- [Validation Patterns](#validation-patterns)
- [Nullable Input Handling](#nullable-input-handling)
- [Trailing Zeros](#trailing-zeros)
- [Step Configuration](#step-configuration)
- [Spin Button Customization](#spin-button-customization)
- [Event Handling](#event-handling)
- [Data Binding](#data-binding)
- [Common Edge Cases](#common-edge-cases)
- [Troubleshooting](#troubleshooting)

## Validation Patterns

### Basic Min/Max Validation

Use built-in min/max properties for range validation:

```typescript
const quantityInput = new NumericTextBox({
  value: 10,
  min: 1,
  max: 100,
  step: 1,
  change: (args) => {
    if (args.value < args.minimum || args.value > args.maximum) {
      console.log('Value out of range');
    }
  }
});
quantityInput.appendTo('#quantity');
```

### Form Validator Integration

Combine with Syncfusion form validator for comprehensive validation:

```typescript
import { NumericTextBox } from '@syncfusion/ej2-inputs';
import { FormValidator } from '@syncfusion/ej2-inputs';

const numericTextBox = new NumericTextBox({
  value: 50,
  min: 0,
  max: 100
});
numericTextBox.appendTo('#percentage');

// Create form validator
const formValidator = new FormValidator('#form', {
  rules: {
    percentage: [
      { required: true, message: 'Percentage is required' },
      { min: 0, message: 'Minimum value is 0' },
      { max: 100, message: 'Maximum value is 100' }
    ]
  }
});

// Validate on submit
document.getElementById('submit-btn').addEventListener('click', () => {
  const isValid = formValidator.validate();
  if (isValid) {
    console.log('Form valid, value:', numericTextBox.value);
  }
});
```

### Custom Validation Function

Implement custom validation logic:

```typescript
function validateDiscountInput(value) {
  // Discount must be between 0 and 50%
  if (value < 0 || value > 0.5) {
    return { valid: false, message: 'Discount must be 0-50%' };
  }
  
  // Discount must be a multiple of 0.05 (5%)
  if (value % 0.05 !== 0) {
    return { valid: false, message: 'Discount must be in 5% increments' };
  }
  
  return { valid: true };
}

const discountInput = new NumericTextBox({
  value: 0.15,
  format: 'p0',
  min: 0,
  max: 0.5,
  step: 0.05,
  change: (args) => {
    const validation = validateDiscountInput(args.value);
    if (!validation.valid) {
      discountInput.element.setAttribute('aria-invalid', 'true');
      showErrorMessage(validation.message);
    } else {
      discountInput.element.setAttribute('aria-invalid', 'false');
      clearErrorMessage();
    }
  }
});
discountInput.appendTo('#discount');
```

### Server-Side Validation

Validate on server and display errors:

```typescript
const priceInput = new NumericTextBox({
  value: 99.99,
  format: 'c2',
  locale: 'en-US',
  blur: async (args) => {
    // Send to server for validation
    const response = await fetch('/api/validate-price', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ price: args.value })
    });
    
    const result = await response.json();
    if (!result.valid) {
      priceInput.element.setAttribute('aria-invalid', 'true');
      showValidationError(result.message);
    } else {
      priceInput.element.setAttribute('aria-invalid', 'false');
    }
  }
});
priceInput.appendTo('#product-price');
```

## Nullable Input Handling

### Prevent Null/Empty Values

Ensure a value is always set:

```typescript
const quantityInput = new NumericTextBox({
  value: 1,  // Default value
  min: 1,
  max: 100,
  change: (args) => {
    // Force minimum if empty
    if (args.value === null || args.value === undefined) {
      quantityInput.value = 1;
    }
  }
});
quantityInput.appendTo('#quantity');
```

### Optional Fields with Placeholder

Allow null values but show placeholder guidance:

```typescript
const optionalInput = new NumericTextBox({
  value: null,
  placeholder: 'Optional - leave blank',
  change: (args) => {
    if (args.value === null) {
      console.log('Field is empty');
    } else {
      console.log('Value:', args.value);
    }
  }
});
optionalInput.appendTo('#optional-field');
```

### Nullable with Default

Provide intelligent defaults:

```typescript
const unitPriceInput = new NumericTextBox({
  value: null,
  format: 'c2',
  locale: 'en-US',
  blur: (args) => {
    // Set default on blur if empty
    if (args.value === null || args.value === '') {
      unitPriceInput.value = 0;
    }
  }
});
unitPriceInput.appendTo('#unit-price');
```

## Trailing Zeros

### Maintain Trailing Zeros

By default, trailing zeros disappear on focus. Use format to maintain them:

```typescript
// Standard approach - zeros disappear on focus
const standard = new NumericTextBox({
  value: 10.50,
  format: 'n2'
});
standard.appendTo('#standard');
// Displays: "10.50" (blur) → "10.5" (focus) → "10.50" (blur again)

// Solution: Use fixed format with floatLabelType
const maintained = new NumericTextBox({
  value: 10.50,
  format: 'n2',
  floatLabelType: 'Auto',
  focus: (args) => {
    // Manually format to keep zeros visible
    maintained.element.value = maintained.value.toFixed(2);
  }
});
maintained.appendTo('#maintained');
```

### Custom Trailing Zero Handler

```typescript
function createTrailingZeroInput(elementId, decimals = 2) {
  const numericTextBox = new NumericTextBox({
    value: 0,
    decimals: decimals,
    format: `n${decimals}`,
    focus: (args) => {
      // Display with trailing zeros on focus
      const formatted = args.value.toFixed(decimals);
      numericTextBox.element.value = formatted;
    },
    blur: (args) => {
      // Ensure proper formatting on blur
      numericTextBox.value = parseFloat(args.value);
    }
  });
  
  numericTextBox.appendTo(elementId);
  return numericTextBox;
}

const priceInput = createTrailingZeroInput('#price', 2);
// Maintains format: 10.50 → 10.50 (never becomes 10.5)
```

## Step Configuration

### Basic Step Value

Increment/decrement by fixed amount:

```typescript
const numericTextBox = new NumericTextBox({
  value: 0,
  min: 0,
  max: 100,
  step: 5  // Arrow keys change by 5
});
numericTextBox.appendTo('#step-5');
```

### Dynamic Step Changes

Modify step based on conditions:

```typescript
const amountInput = new NumericTextBox({
  value: 100,
  step: 1,
  change: (args) => {
    // Use different steps based on range
    if (args.value < 100) {
      amountInput.step = 1;
    } else if (args.value < 1000) {
      amountInput.step = 10;
    } else {
      amountInput.step = 100;
    }
  }
});
amountInput.appendTo('#amount');
```

### Fractional Steps

Support decimal increment:

```typescript
const temperatureInput = new NumericTextBox({
  value: 98.6,
  step: 0.1,  // Change by 0.1 degrees
  format: 'n1',
  min: 95,
  max: 104
});
temperatureInput.appendTo('#temperature');
```

### Percentage Steps

Increment percentage values:

```typescript
const discountInput = new NumericTextBox({
  value: 0,
  format: 'p0',
  step: 0.05,  // 5% increments
  min: 0,
  max: 1
});
discountInput.appendTo('#discount');
```

## Spin Button Customization

### Hide Spin Buttons

Remove increment/decrement buttons:

```typescript
const numericTextBox = new NumericTextBox({
  value: 100,
  showSpinButton: false  // No spin buttons
});
numericTextBox.appendTo('#no-buttons');
```

### Custom Spin Button Click Handlers

Create custom increment/decrement logic:

```typescript
const numericTextBox = new NumericTextBox({
  value: 0,
  min: 0,
  max: 100,
  step: 1
});
numericTextBox.appendTo('#custom-buttons');

// Add custom buttons outside NumericTextBox
document.getElementById('decrement-btn').addEventListener('click', () => {
  const newValue = Math.max(numericTextBox.min, numericTextBox.value - numericTextBox.step);
  numericTextBox.value = newValue;
});

document.getElementById('increment-btn').addEventListener('click', () => {
  const newValue = Math.min(numericTextBox.max, numericTextBox.value + numericTextBox.step);
  numericTextBox.value = newValue;
});
```

### Styled Spin Buttons

Add custom styling to buttons (see [styling-and-customization.md](styling-and-customization.md)):

```css
.e-input-group-icon.e-numeric-up-icon {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  font-size: 16px;
}

.e-input-group-icon.e-numeric-down-icon {
  background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
  color: white;
  font-size: 16px;
}
```

## Event Handling

### Change Event

Listen for value changes:

```typescript
const numericTextBox = new NumericTextBox({
  value: 50,
  change: (args) => {
    console.log('Previous:', args.previousValue);
    console.log('Current:', args.value);
    console.log('Is interaction:', args.isInteracted);
    
    // Update UI based on new value
    updateSummary(args.value);
  }
});
numericTextBox.appendTo('#numeric');
```

### Focus Event

Handle focus:

```typescript
const numericTextBox = new NumericTextBox({
  value: 100,
  focus: (args) => {
    console.log('Focused, current value:', args.value);
    
    // Show tooltip or help text
    showHelpText('Enter a value between 0 and 1000');
  }
});
numericTextBox.appendTo('#numeric');
```

### Blur Event

Handle blur (formatting applied here):

```typescript
const numericTextBox = new NumericTextBox({
  value: 1234.5,
  format: 'n2',
  blur: (args) => {
    console.log('Raw value:', args.value);
    console.log('Formatted display:', numericTextBox.element.value);
    
    // Validate and save
    saveValue(args.value);
  }
});
numericTextBox.appendTo('#numeric');
```

### Keyboard Events

```typescript
const numericTextBox = new NumericTextBox({
  value: 0,
  keydown: (args) => {
    // Check for specific keys
    if (args.key === 'Enter') {
      console.log('Enter pressed, value:', numericTextBox.value);
      submitForm();
    }
    
    if (args.key === 'Escape') {
      // Reset to previous value
      numericTextBox.value = numericTextBox.value;
    }
  }
});
numericTextBox.appendTo('#numeric');
```

## Data Binding

### One-Way Binding

Display model value:

```typescript
// TypeScript model
class PricingModel {
  regularPrice: number = 100;
  discountedPrice: number = 75;
}

const model = new PricingModel();

// Bind regular price
const regularInput = new NumericTextBox({
  value: model.regularPrice,
  format: 'c2'
});
regularInput.appendTo('#regular-price');

// Bind discounted price
const discountedInput = new NumericTextBox({
  value: model.discountedPrice,
  format: 'c2'
});
discountedInput.appendTo('#discounted-price');
```

### Two-Way Binding Pattern

Sync model with input:

```typescript
class Order {
  quantity: number = 1;
  unitPrice: number = 100;
  
  get total() {
    return this.quantity * this.unitPrice;
  }
}

const order = new Order();

// Quantity input
const quantityInput = new NumericTextBox({
  value: order.quantity,
  change: (args) => {
    order.quantity = args.value;
    updateTotal();
  }
});
quantityInput.appendTo('#quantity');

// Unit price input
const unitPriceInput = new NumericTextBox({
  value: order.unitPrice,
  format: 'c2',
  change: (args) => {
    order.unitPrice = args.value;
    updateTotal();
  }
});
unitPriceInput.appendTo('#unit-price');

// Total display
function updateTotal() {
  document.getElementById('total').textContent = `$${order.total.toFixed(2)}`;
}

updateTotal();
```

### Form Serialization

Collect values from form:

```typescript
const quantityInput = new NumericTextBox({ value: 1 });
const priceInput = new NumericTextBox({ value: 99.99 });

quantityInput.appendTo('#quantity');
priceInput.appendTo('#price');

// Get all values
function getFormData() {
  return {
    quantity: quantityInput.value,
    price: priceInput.value,
    total: quantityInput.value * priceInput.value
  };
}

// Submit
document.getElementById('submit').addEventListener('click', () => {
  const formData = getFormData();
  console.log('Submitting:', formData);
  // Send to server
});
```

## Common Edge Cases

### Zero vs Null

Distinguish between 0 and empty:

```typescript
const numericTextBox = new NumericTextBox({
  value: 0,
  change: (args) => {
    if (args.value === 0) {
      console.log('Zero value');
    } else if (args.value === null) {
      console.log('Null/empty');
    }
  }
});
```

### Very Large Numbers

Handle large numeric values:

```typescript
const largeNumberInput = new NumericTextBox({
  value: 1000000000,  // 1 billion
  format: 'n0',
  max: Number.MAX_SAFE_INTEGER
});
largeNumberInput.appendTo('#large-number');
```

### Precision Issues

Avoid floating-point errors:

```typescript
// Problem: 0.1 + 0.2 !== 0.3
const problematicValue = 0.1 + 0.2;  // 0.30000000000000004

// Solution: Use fixed decimals
const percentageInput = new NumericTextBox({
  value: 0.1,
  decimals: 2,
  change: (args) => {
    const fixedValue = parseFloat(args.value.toFixed(2));
    console.log('Fixed value:', fixedValue);
  }
});
percentageInput.appendTo('#percentage');
```

### Rapid User Input

Debounce frequent changes:

```typescript
let changeTimeout;

const numericTextBox = new NumericTextBox({
  value: 100,
  change: (args) => {
    // Cancel previous timeout
    clearTimeout(changeTimeout);
    
    // Wait 500ms before processing
    changeTimeout = setTimeout(() => {
      processValue(args.value);
      console.log('Processing value:', args.value);
    }, 500);
  }
});
numericTextBox.appendTo('#numeric');
```

## Troubleshooting

**Value not updating:** Ensure you're using `numericTextBox.value = newValue` not direct element modification.

**Format not applying:** Format applies on blur. Use `blur` event to verify formatting.

**Spin buttons not working:** Check `showSpinButton: true`, verify min/max/step are configured.

**Validation not triggering:** Ensure validator rules match your input constraints.

**Decimal precision errors:** Use `.toFixed()` to handle floating-point arithmetic.

**Performance issues with rapid updates:** Implement debouncing or throttling for frequent changes.

## Next Steps

- Combine patterns with [Accessibility](accessibility.md) best practices
- Review [Styling](styling-and-customization.md) for error states
- Implement comprehensive validation in production forms
