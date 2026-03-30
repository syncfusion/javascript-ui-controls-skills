# Date Formatting & Display

## Table of Contents
- [Format Pattern Syntax](#format-pattern-syntax)
- [Standard Date Formats](#standard-date-formats)
- [Date Format Examples](#date-format-examples)
- [Header Format Customization](#header-format-customization)
- [Day Header Format](#day-header-format)
- [Tooltip Formatting](#tooltip-formatting)
- [Locale-Specific Formats](#locale-specific-formats)
- [Format Best Practices](#format-best-practices)

---

## Format Pattern Syntax

DatePicker uses pattern characters to define how dates display:

| Pattern | Description | Example Result |
|---------|-------------|-----------------|
| **d** | Day of month (1-31) | `1` to `31` |
| **dd** | Day with leading zero (01-31) | `01` to `31` |
| **ddd** | Abbreviated day name | `Mon`, `Tue`, `Wed` |
| **dddd** | Full day name | `Monday`, `Tuesday`, `Wednesday` |
| **M** | Month (1-12) | `1` to `12` |
| **MM** | Month with leading zero (01-12) | `01` to `12` |
| **MMM** | Abbreviated month name | `Jan`, `Feb`, `Mar` |
| **MMMM** | Full month name | `January`, `February`, `March` |
| **yy** | Two-digit year | `25`, `26` (represents 2025, 2026) |
| **yyyy** | Four-digit year | `2025`, `2026` |

### Pattern Combinations

```typescript
const datePicker = new DatePicker({
  value: new Date(2025, 2, 15),  // March 15, 2025
  format: 'dddd, MMMM d, yyyy'   // "Saturday, March 15, 2025"
});

datePicker.appendTo('#datepicker');
```

---

## Standard Date Formats

Common pre-defined formats for quick setup:

| Format Name | Pattern | Example (March 15, 2025) |
|-------------|---------|--------------------------|
| **default** | M/d/yyyy | 3/15/2025 |
| **short** | d M, y | 15 3, 25 |
| **medium** | d MM, y | 15 03, 25 |
| **full** | dddd, d MMMM, yy | Saturday, 15 March, 25 |
| **UTC** | yyyy-MM-dd | 2025-03-15 |

### Using Standard Formats

```typescript
// Default (US style)
new DatePicker({ format: 'M/d/yyyy' });

// Short (European style)
new DatePicker({ format: 'd M, y' });

// Full (Readable)
new DatePicker({ format: 'dddd, d MMMM, yyyy' });

// ISO standard
new DatePicker({ format: 'yyyy-MM-dd' });
```

---

## Date Format Examples

### US Format
```typescript
const datePicker = new DatePicker({
  value: new Date(2025, 2, 15),
  format: 'M/d/yyyy',
  locale: 'en-US'
});

datePicker.appendTo('#datepicker');
// Displays: 3/15/2025
```

### European Format
```typescript
const datePicker = new DatePicker({
  value: new Date(2025, 2, 15),
  format: 'dd/MM/yyyy',
  locale: 'de-DE'
});

datePicker.appendTo('#datepicker');
// Displays: 15/03/2025
```

### ISO Format (Database)
```typescript
const datePicker = new DatePicker({
  value: new Date(2025, 2, 15),
  format: 'yyyy-MM-dd'
});

datePicker.appendTo('#datepicker');
// Displays: 2025-03-15
```

### Full Readable Format
```typescript
const datePicker = new DatePicker({
  value: new Date(2025, 2, 15),
  format: 'dddd, MMMM d, yyyy'
});

datePicker.appendTo('#datepicker');
// Displays: Saturday, March 15, 2025
```

### Month & Year Only
```typescript
const datePicker = new DatePicker({
  value: new Date(2025, 2, 15),
  format: 'MMMM yyyy',
  startLevel: 'year',
  depthLevel: 'year'
});

datePicker.appendTo('#datepicker');
// Displays: March 2025
```

---

## Header Format Customization

The header appears at the top of the calendar popup and can be customized:

```typescript
const datePicker = new DatePicker({
  value: new Date(2025, 2, 15),
  headerFormat: 'MMMM yyyy',  // Shows "March 2025" in header
  format: 'dd/MM/yyyy'
});

datePicker.appendTo('#datepicker');
```

### Header Format Patterns

| Format | Display | Use Case |
|--------|---------|----------|
| `MMMM yyyy` | March 2025 | Monthly view |
| `yyyy` | 2025 | Year view |
| `MMMM d, yyyy` | March 15, 2025 | Selected date |
| `MMMM d - dddd` | March 15 - Saturday | Date with day |

### Example: Dynamic Header

```typescript
const datePicker = new DatePicker({
  value: new Date(2025, 2, 15),
  headerFormat: 'MMMM d, yyyy',
  dateFormat: 'dd/MM/yyyy'
});

datePicker.appendTo('#datepicker');
// Header shows: March 15, 2025
// Input shows: 15/03/2025
```

---

## Day Header Format

The calendar day names (Mon, Tue, Wed) can be shortened or expanded:

```typescript
const datePicker = new DatePicker({
  value: new Date(),
  dayHeaderFormat: 'Long'    // Monday, Tuesday, Wednesday
});

datePicker.appendTo('#datepicker');
```

### Day Header Options

| Option | Display |
|--------|---------|
| **Short** | M, T, W, T, F, S, S |
| **Medium** (default) | Mon, Tue, Wed, Thu, Fri, Sat, Sun |
| **Long** | Monday, Tuesday, Wednesday, etc. |

### Use Cases

```typescript
// Compact calendar
new DatePicker({ dayHeaderFormat: 'Short' });

// Standard calendar (default)
new DatePicker({ dayHeaderFormat: 'Medium' });

// Accessible calendar
new DatePicker({ dayHeaderFormat: 'Long' });
```

---

## Tooltip Formatting

Show formatted dates when hovering over calendar dates:

```typescript
const datePicker = new DatePicker({
  value: new Date(),
  showTooltip: true,          // Enable tooltip
  tooltipFormat: 'dd/MM/yyyy' // Tooltip date format
});

datePicker.appendTo('#datepicker');
```

### Tooltip Examples

```typescript
// Tooltip in US format
new DatePicker({
  showTooltip: true,
  tooltipFormat: 'M/d/yyyy'
});

// Tooltip with day name
new DatePicker({
  showTooltip: true,
  tooltipFormat: 'dddd, MMM d'
});

// Disable tooltips
new DatePicker({
  showTooltip: false
});
```

**Behavior:** When user hovers over any date in calendar, tooltip appears with formatted date.

---

## Locale-Specific Formats

Different locales have different date conventions:

```typescript
// German format (Europe)
const germanDatePicker = new DatePicker({
  locale: 'de-DE',
  value: new Date(2025, 2, 15),
  dateFormat: 'dd.MM.yyyy'
});

germanDatePicker.appendTo('#de-datepicker');
// Displays: 15.03.2025

// Japanese format (Asia)
const japaneseDate = new DatePicker({
  locale: 'ja-JP',
  value: new Date(2025, 2, 15),
  dateFormat: 'yyyy/MM/dd'
});

japaneseDate.appendTo('#ja-datepicker');
// Displays: 2025/03/15

// French format (Europe)
const frenchDate = new DatePicker({
  locale: 'fr-FR',
  value: new Date(2025, 2, 15),
  dateFormat: 'dd/MM/yyyy'
});

frenchDate.appendTo('#fr-datepicker');
// Displays: 15/03/2025
```

### Common Locale Formats

| Locale | Format | Example |
|--------|--------|---------|
| en-US | M/d/yyyy | 3/15/2025 |
| en-GB | dd/MM/yyyy | 15/03/2025 |
| de-DE | dd.MM.yyyy | 15.03.2025 |
| fr-FR | dd/MM/yyyy | 15/03/2025 |
| ja-JP | yyyy/MM/dd | 2025/03/15 |
| es-ES | dd/MM/yyyy | 15/03/2025 |
| it-IT | dd/MM/yyyy | 15/03/2025 |

---

## Format Best Practices

### 1. Always Set Format Explicitly

```typescript
// ✓ Good: Explicit format
const datePicker = new DatePicker({
  locale: 'de-DE',
  format: 'dd.MM.yyyy'
});

// ✗ Avoid: Relying on default
const datePicker = new DatePicker({
  locale: 'de-DE'
});
```

**Why:** Prevents confusion across browsers and ensures consistency.

### 2. Use ISO Format for APIs

```typescript
// ✓ Good: ISO format for backend
const datePicker = new DatePicker({
  format: 'yyyy-MM-dd'  // Send to API
});

datePicker.change = (args) => {
  const isoDate = datePicker.value;  // Native JavaScript Date
  fetch('/api/save-date', {
    method: 'POST',
    body: JSON.stringify({ date: isoDate.toISOString() })
  });
};
```

### 3. Match User Locale

```typescript
// ✓ Good: Match user's locale
const userLocale = navigator.language;  // "de-DE"
const datePicker = new DatePicker({
  locale: userLocale,
  format: getFormatForLocale(userLocale)
});
```

### 4. Use Readable Formats for Display

```typescript
// ✓ Good: Human-readable
new DatePicker({
  format: 'dddd, MMMM d, yyyy'  // Saturday, March 15, 2025
});

// ✗ Avoid: Confusing abbreviations
new DatePicker({
  format: 'd/M/yy'  // 15/3/25 (unclear)
});
```

### 5. Format Header for Navigation Context

```typescript
// ✓ Good: Header shows month/year context
const datePicker = new DatePicker({
  headerFormat: 'MMMM yyyy',
  format: 'dd/MM/yyyy'
});

// Month view header shows "March 2025"
// Input shows date as "15/03/2025"
```

---

## Formatting with Globalization

Import localization files for proper month/day name translation:

```typescript
import { DatePicker } from '@syncfusion/ej2-calendars';
import '@syncfusion/ej2-base/styles/material.css';
import '@syncfusion/ej2-calendars/styles/material.css';

// Import localization (enables translated month/day names)
// Localization typically handled by your build system or CDN

const datePicker = new DatePicker({
  locale: 'de-DE',
  value: new Date(2025, 2, 15),
  format: 'dddd, d. MMMM yyyy'
});

datePicker.appendTo('#datepicker');
// Displays with German day/month names:
// "Samstag, 15. März 2025"
```

---

## Getting and Setting Formatted Dates

### Get Formatted Value

```typescript
const datePicker = new DatePicker({
  value: new Date(2025, 2, 15),
  format: 'dd/MM/yyyy'
});

datePicker.appendTo('#datepicker');

// Get native JavaScript Date object
const dateObj = datePicker.value;
console.log(dateObj);  // Date object

// Get formatted string
const formatted = datePicker.formattedValue();
console.log(formatted);  // "15/03/2025"
```

### Set Date Programmatically

```typescript
const datePicker = new DatePicker({
  format: 'dd/MM/yyyy'
});

datePicker.appendTo('#datepicker');

// Set from Date object
datePicker.value = new Date(2025, 11, 25);
datePicker.dataBind();

// Display updates to "25/12/2025"
```

---

## Common Format Issues

| Problem | Solution |
|---------|----------|
| Month shows as 1-12 instead of 01-12 | Use `MM` instead of `M` |
| Year shows as 25 instead of 2025 | Use `yyyy` instead of `yy` |
| Day names not translated | Import localization files for locale |
| Format ignored | Ensure property is `dateFormat` or `format` (lowercase) |
| Tooltip doesn't appear | Set `showTooltip: true` AND `tooltipFormat` |
| Header format wrong | Double-check pattern characters (d/M/y case-sensitive) |

---

## Advanced Formatting Patterns

```typescript
// US format with day
new DatePicker({ format: 'ddd, MMM d, yyyy' });

// Time-like format (month/day only)
new DatePicker({ format: 'MM/dd' });

// Database format with leading zeros
new DatePicker({ format: 'yyyy-MM-dd' });

// European format with dot separator
new DatePicker({ format: 'dd.MM.yyyy' });

// Asian format with slash separator
new DatePicker({ format: 'yyyy/MM/dd' });
```

Choose the format that best matches your application's conventions and user expectations.
