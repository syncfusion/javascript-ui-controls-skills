# Number Formatting in NumericTextBox

## Table of Contents
- [Overview](#overview)
- [Standard Format Specifiers](#standard-format-specifiers)
- [Format String Patterns](#format-string-patterns)
- [Currency Formatting](#currency-formatting)
- [Percentage Formatting](#percentage-formatting)
- [Custom Number Formats](#custom-number-formats)
- [Globalization and Locales](#globalization-and-locales)
- [Format Behavior](#format-behavior)
- [Troubleshooting](#troubleshooting)

## Overview

The NumericTextBox uses the `format` property to display numeric values in different representations. The formatted value appears when the control loses focus (blur). While focused, the raw numeric value is displayed for easy editing.

## Standard Format Specifiers

Syncfusion supports standard .NET numeric format specifiers. The most common are:

| Specifier | Name | Example | Result |
|-----------|------|---------|--------|
| `n` | Number | `n2` | 1234.57 |
| `c` | Currency | `c2` | $1,234.57 |
| `p` | Percent | `p2` | 12.35% |

### Number Format (n)

Displays numbers with thousand separators and decimal places:

```typescript
const numericTextBox = new NumericTextBox({
  value: 1234567.89,
  format: 'n2'  // 1,234,567.89
});
numericTextBox.appendTo('#numerictextbox');
```

**Variations:**
- `n0` - No decimal places: 1,234,568
- `n1` - One decimal place: 1,234,567.9
- `n2` - Two decimal places: 1,234,567.89
- `n3` - Three decimal places: 1,234,567.890

## Currency Formatting

Format numbers as currency with locale-specific symbols and separators:

```typescript
const numericTextBox = new NumericTextBox({
  value: 1234.56,
  format: 'c2',  // Locale-dependent, e.g., $1,234.56
  locale: 'en-US'
});
numericTextBox.appendTo('#price-input');
```

### Currency with Different Locales

The currency symbol changes based on locale:

```typescript
// US Dollar
new NumericTextBox({
  value: 1000,
  format: 'c2',
  locale: 'en-US'
}).appendTo('#usd'); // $1,000.00

// Euro (Germany)
new NumericTextBox({
  value: 1000,
  format: 'c2',
  locale: 'de-DE'
}).appendTo('#eur'); // 1.000,00 €

// British Pound
new NumericTextBox({
  value: 1000,
  format: 'c2',
  locale: 'en-GB'
}).appendTo('#gbp'); // £1,000.00

// Japanese Yen
new NumericTextBox({
  value: 1000,
  format: 'c2',
  locale: 'ja-JP'
}).appendTo('#jpy'); // ¥1,000
```

### Combining Currency with Adornments

Use adornments along with currency format for redundant symbols (useful for extra clarity):

```typescript
const numericTextBox = new NumericTextBox({
  value: 99.99,
  format: 'c2',
  prependTemplate: '$'  // Optional, depending on locale
});
numericTextBox.appendTo('#currency-input');
```

## Percentage Formatting

Display numbers as percentages:

```typescript
const numericTextBox = new NumericTextBox({
  value: 0.1234,  // 12.34%
  format: 'p2'    // Display as 12.34%
});
numericTextBox.appendTo('#discount-input');
```

**Variations:**
- `p0` - No decimal places: 12%
- `p1` - One decimal place: 12.3%
- `p2` - Two decimal places: 12.34%

**Note:** The input value is multiplied by 100 for display. Input `0.15` displays as `15%`.

### Percentage Discount Example

```typescript
const discountInput = new NumericTextBox({
  value: 0.15,      // 15% discount
  format: 'p0',
  min: 0,
  max: 1,           // 0-100%
  step: 0.05
});
discountInput.appendTo('#discount-field');
```

## Custom Number Formats

Create custom formats using format string patterns:

### Format String Patterns

| Pattern | Meaning | Example |
|---------|---------|---------|
| `#` | Optional digit (no leading zero) | `###` formats 1 as 1 |
| `0` | Required digit (shows 0 if missing) | `000` formats 1 as 001 |
| `.` | Decimal separator | `#.00` formats 1 as 1.00 |
| `,` | Thousand separator | `#,##0` formats 1000 as 1,000 |

### Custom Format Examples

**Phone Number-like Format:**
```typescript
const numericTextBox = new NumericTextBox({
  value: 5551234567,
  format: '[000] 000-0000'  // Custom pattern
});
numericTextBox.appendTo('#phone');
```

**Custom Decimal Pattern:**
```typescript
// Display number with exactly 3 decimal places
const numericTextBox = new NumericTextBox({
  value: 123.5,
  format: '#,##0.000'  // 123.500
});
numericTextBox.appendTo('#precision');
```

**Currency Without Symbol:**
```typescript
const numericTextBox = new NumericTextBox({
  value: 1234.56,
  format: '#,##0.00'  // 1,234.56 (no symbol)
});
numericTextBox.appendTo('#amount');
```

### Using # vs 0

```typescript
// Using # (optional digits)
new NumericTextBox({
  value: 1,
  format: '###'  // Displays as: 1
}).appendTo('#optional');

// Using 0 (required digits with leading zeros)
new NumericTextBox({
  value: 1,
  format: '000'  // Displays as: 001
}).appendTo('#required');
```

## Globalization and Locales

Format output adapts to language and regional settings:

```typescript
import { setCulture } from '@syncfusion/ej2-base';

// Set global locale
setCulture('fr-FR');

const numericTextBox = new NumericTextBox({
  value: 1234.56,
  format: 'n2'  // Uses French formatting: 1234,56
});
numericTextBox.appendTo('#numeric');
```

### Locale-Specific Separators

Different locales use different separators:

| Locale | Value | Format Result |
|--------|-------|---------------|
| `en-US` | 1234.56 | 1,234.56 |
| `de-DE` | 1234.56 | 1.234,56 |
| `fr-FR` | 1234.56 | 1 234,56 |
| `es-ES` | 1234.56 | 1.234,56 |
| `pt-BR` | 1234.56 | 1.234,56 |

## Format Behavior

### Format on Blur

The NumericTextBox applies formatting when the field loses focus (blur):

```typescript
const numericTextBox = new NumericTextBox({
  value: 1000,
  format: 'n2'
});
numericTextBox.appendTo('#numeric');

// While focused: user sees raw value "1000"
// When blurred: formatted value "1,000.00" appears
```

### Raw Value Access

Access the unformatted numeric value programmatically:

```typescript
const numericTextBox = new NumericTextBox({
  value: 1234.567,
  format: 'n2'
});
numericTextBox.appendTo('#numeric');

// Get raw numeric value (not formatted string)
const rawValue = numericTextBox.value;  // 1234.567

// Listen to change events for value updates
numericTextBox.change = (args) => {
  console.log('Raw value:', args.value);  // Unformatted
};
```

## Practical Examples

### Monetary Input
```typescript
const priceInput = new NumericTextBox({
  value: 99.99,
  format: 'c2',
  locale: 'en-US',
  min: 0,
  max: 999999.99,
  step: 0.01
});
priceInput.appendTo('#price');
```

### Discount Percentage
```typescript
const discountInput = new NumericTextBox({
  value: 0.10,  // 10% off
  format: 'p0',
  min: 0,
  max: 1,
  step: 0.01
});
discountInput.appendTo('#discount');
```

### Measurement with Precision
```typescript
const temperatureInput = new NumericTextBox({
  value: 98.6,
  format: 'n1',  // One decimal place
  min: -50,
  max: 50,
  step: 0.1
});
temperatureInput.appendTo('#temperature');
```

### Tax Rate
```typescript
const taxInput = new NumericTextBox({
  value: 0.08,  // 8%
  format: 'p2',
  min: 0,
  max: 1,
  step: 0.001
});
taxInput.appendTo('#tax-rate');
```

## Troubleshooting

**Format not applying:** Ensure the format string follows .NET patterns (n, c, p, or custom #/0 patterns).

**Currency symbol not showing:** Verify the locale is set correctly. Use `locale: 'en-US'` for US Dollar, etc.

**Percentage not calculating:** Remember that `0.15` displays as `15%`. Input is multiplied by 100.

**Decimal places not consistent:** Use format patterns like `c2` or `n2` to enforce specific decimal places.

## Next Steps

- Combine formatting with [Adornments](adornments-and-ui.md) for enhanced visual context
- Implement [Validation](advanced-patterns.md) to ensure values meet business rules
- Use formatting in [Accessibility](accessibility.md) compliant forms
