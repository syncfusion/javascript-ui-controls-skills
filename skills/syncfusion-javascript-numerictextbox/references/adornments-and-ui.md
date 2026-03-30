# Adornments and UI Customization

## Table of Contents
- [Overview](#overview)
- [Prepend Template](#prepend-template)
- [Append Template](#append-template)
- [Common Use Cases](#common-use-cases)
- [Integration with Float Labels](#integration-with-float-labels)
- [Styling Adornments](#styling-adornments)
- [Complex Adornment Examples](#complex-adornment-examples)

## Overview

Adornments allow you to add custom HTML elements before or after the NumericTextBox input field without affecting numeric validation or behavior. Use `prependTemplate` for prefix elements (currency symbols, icons) and `appendTemplate` for suffix elements (unit labels, action buttons).

## Prepend Template

The `prependTemplate` property renders HTML before the numeric input field.

### Basic Currency Symbol

Add a currency prefix:

```typescript
const numericTextBox = new NumericTextBox({
  value: 100,
  format: 'n2',
  prependTemplate: '$'
});
numericTextBox.appendTo('#price-input');
```

**HTML Output:**
```html
<div class="e-input-group">
  <span class="e-adornment">$</span>
  <input type="text" class="e-input" />
</div>
```

### Icon Prefix

Add an icon before the input:

```typescript
const numericTextBox = new NumericTextBox({
  value: 50,
  prependTemplate: '<i class="e-icons e-dollar"></i>'
});
numericTextBox.appendTo('#amount');
```

### Percentage Symbol Prefix

```typescript
const numericTextBox = new NumericTextBox({
  value: 0.15,
  format: 'p0',
  prependTemplate: '<span style="color: #666;">Rate: </span>'
});
numericTextBox.appendTo('#interest-rate');
```

## Append Template

The `appendTemplate` property renders HTML after the numeric input field.

### Basic Unit Label

Add a unit suffix:

```typescript
const numericTextBox = new NumericTextBox({
  value: 100,
  format: 'n0',
  appendTemplate: 'kg'
});
numericTextBox.appendTo('#weight-input');
```

**HTML Output:**
```html
<div class="e-input-group">
  <input type="text" class="e-input" />
  <span class="e-adornment">kg</span>
</div>
```

### Unit with Icon

```typescript
const numericTextBox = new NumericTextBox({
  value: 25,
  appendTemplate: '<span>°C</span>'
});
numericTextBox.appendTo('#temperature');
```

### Action Button Suffix

Add a clear or reset button:

```typescript
const numericTextBox = new NumericTextBox({
  value: 100,
  appendTemplate: '<button class="e-btn-icon"><i class="e-icons e-clear"></i></button>'
});
numericTextBox.appendTo('#quantity');
```

## Common Use Cases

### Monetary Input (Currency Symbol + Format)

Display price with currency symbol and formatting:

```typescript
const priceInput = new NumericTextBox({
  value: 49.99,
  format: 'c2',
  locale: 'en-US',
  prependTemplate: '$',
  min: 0,
  max: 99999.99,
  step: 0.01,
  placeholder: 'Enter price'
});
priceInput.appendTo('#product-price');
```

**Result:** `$ 49.99` (formatted price with currency symbol)

### Measurement Input (Value + Unit)

Display measurement with unit label:

```typescript
const distanceInput = new NumericTextBox({
  value: 100,
  format: 'n1',
  appendTemplate: 'km',
  min: 0,
  max: 10000,
  step: 0.1
});
distanceInput.appendTo('#distance');
```

**Result:** `100.0 km`

### Tax Rate (Percentage + Label)

Display tax rate with context:

```typescript
const taxRateInput = new NumericTextBox({
  value: 0.08,
  format: 'p2',
  prependTemplate: 'Tax: ',
  min: 0,
  max: 1,
  step: 0.001
});
taxRateInput.appendTo('#tax-rate');
```

**Result:** `Tax: 8.00%`

### Discount Percentage (Context + Value + Unit)

Multi-context discount field:

```typescript
const discountInput = new NumericTextBox({
  value: 0.15,
  format: 'p0',
  prependTemplate: 'Discount:',
  appendTemplate: '<i class="e-icons e-tag"></i>',
  min: 0,
  max: 1,
  step: 0.05
});
discountInput.appendTo('#discount-field');
```

**Result:** `Discount: 15% [tag-icon]`

### Form Field with Description

Add descriptive text:

```typescript
const quantityInput = new NumericTextBox({
  value: 1,
  min: 1,
  max: 100,
  step: 1,
  prependTemplate: '<label>Qty:</label>',
  appendTemplate: '<span style="font-size: 12px; color: #999;">units</span>'
});
quantityInput.appendTo('#quantity-field');
```

## Integration with Float Labels

Float labels automatically adjust positioning when adornments are present:

```typescript
const numericTextBox = new NumericTextBox({
  value: 100,
  floatLabelType: 'Auto',  // Float label moves above when focused
  format: 'n2',
  prependTemplate: '$',
  label: 'Enter Amount'
});
numericTextBox.appendTo('#amount-with-label');
```

**Behavior:**
- Unfocused: Label floats above the input, adornment visible
- Focused: Raw value shown for editing, adornment remains visible
- Blurred: Formatted value with adornment

## Styling Adornments

### CSS Classes for Adornments

The framework generates these classes for styling:

| Class | Target |
|-------|--------|
| `.e-input-group` | Main wrapper |
| `.e-adornment` | Adornment element |
| `.e-input` | Input field |

### Custom CSS for Adornments

```css
/* Style prepended adornments */
.e-input-group .e-adornment:first-child {
  font-weight: bold;
  color: #007bff;
  background-color: #f0f0f0;
  padding: 0 8px;
}

/* Style appended adornments */
.e-input-group .e-adornment:last-child {
  color: #666;
  font-size: 12px;
  padding: 0 8px;
}

/* Focused state */
.e-input-group.e-input-focus .e-adornment {
  color: #007bff;
  background-color: #fff;
}
```

### Inline Styles in Template

Apply styles directly in the template:

```typescript
const numericTextBox = new NumericTextBox({
  value: 100,
  prependTemplate: '<span style="color: green; font-weight: bold; margin-right: 8px;">$</span>',
  appendTemplate: '<span style="color: #999; font-size: 12px; margin-left: 4px;">USD</span>'
});
numericTextBox.appendTo('#currency-field');
```

## Complex Adornment Examples

### Currency Converter Display

Show value with dual currency indicators:

```typescript
const numericTextBox = new NumericTextBox({
  value: 100,
  format: 'c2',
  locale: 'en-US',
  prependTemplate: '<span class="currency-symbol">$</span><span class="currency-code">(USD)</span>',
  appendTemplate: '<button class="e-btn-icon convert-btn" title="Convert"><i class="e-icons e-refresh"></i></button>',
  change: (args) => {
    // Trigger conversion on value change
  }
});
numericTextBox.appendTo('#amount-usd');
```

### Quantity with Inventory Alert

Display quantity with stock status:

```typescript
const quantityInput = new NumericTextBox({
  value: 5,
  min: 1,
  max: 50,
  prependTemplate: '<i class="e-icons e-product"></i>',
  appendTemplate: '<span id="stock-status" class="stock-warning">Low Stock</span>'
});
quantityInput.appendTo('#order-quantity');

// Update status based on value
quantityInput.change = (args) => {
  const stockStatus = document.getElementById('stock-status');
  if (args.value < 10) {
    stockStatus.textContent = 'Low Stock';
    stockStatus.className = 'stock-warning';
  } else {
    stockStatus.textContent = 'In Stock';
    stockStatus.className = 'stock-available';
  }
};
```

### Percentage with Visual Indicator

```typescript
const percentageInput = new NumericTextBox({
  value: 0.65,
  format: 'p0',
  appendTemplate: '<div class="progress-bar"><div class="progress-fill" style="width: 65%"></div></div>'
});
percentageInput.appendTo('#completion-percentage');
```

**CSS:**
```css
.progress-bar {
  width: 60px;
  height: 4px;
  background-color: #e0e0e0;
  border-radius: 2px;
  margin-left: 8px;
}

.progress-fill {
  height: 100%;
  background-color: #4caf50;
  border-radius: 2px;
  transition: width 0.3s ease;
}
```

## Best Practices

**Keep adornments simple:** Use short text or single icons to avoid cluttering the input.

**Test with different values:** Ensure adornments remain readable with minimal and maximal values.

**Combine with float labels:** Adornments work well with float labels for better UX.

**Consider accessibility:** Ensure sufficient contrast and that adornments don't interfere with keyboard navigation.

**Use templates wisely:** Complex HTML in templates can impact performance with many instances.

## Next Steps

- Apply [Number Formatting](number-formatting.md) alongside adornments
- Ensure [Accessibility](accessibility.md) with proper ARIA labels
- Implement [Styling](styling-and-customization.md) for consistent branding
