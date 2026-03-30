# Getting Started with DatePicker

This guide covers installation, basic setup, and creating your first DatePicker in a TypeScript application.

## Installation

Install the required Syncfusion EJ2 packages:

```bash
# Install base package (required)
npm install @syncfusion/ej2-base

# Install calendars package containing DatePicker
npm install @syncfusion/ej2-calendars
```

## CSS Theme Setup

Import the theme CSS in your main application file:

```typescript
// main.ts or app.ts

// Material theme (default)
import '@syncfusion/ej2-base/styles/material.css';
import '@syncfusion/ej2-calendars/styles/material.css';

// Alternative themes (uncomment as needed)
// import '@syncfusion/ej2-base/styles/fabric.css';
// import '@syncfusion/ej2-calendars/styles/fabric.css';

// import '@syncfusion/ej2-base/styles/bootstrap.css';
// import '@syncfusion/ej2-calendars/styles/bootstrap.css';

// import '@syncfusion/ej2-base/styles/bootstrap4.css';
// import '@syncfusion/ej2-calendars/styles/bootstrap4.css';

// High contrast theme
// import '@syncfusion/ej2-base/styles/highcontrast.css';
// import '@syncfusion/ej2-calendars/styles/highcontrast.css';
```

**Important:** Import CSS only once in your main file, not in every component using DatePicker.

## Basic HTML Setup

Create an input element with an ID to mount the DatePicker:

```html
<!DOCTYPE html>
<html>
<head>
  <title>DatePicker Example</title>
</head>
<body>
  <!-- Container for DatePicker -->
  <input id="datepicker" />
</body>
</html>
```

## Creating Your First DatePicker

In your TypeScript application file:

```typescript
import { DatePicker } from '@syncfusion/ej2-calendars';

// Initialize DatePicker
const datePicker = new DatePicker({
  value: new Date()  // Set current date as default
});

// Render to the input element
datePicker.appendTo('#datepicker');
```

**Result:** A DatePicker appears with today's date pre-selected.

## Adding a Placeholder

```typescript
const datePicker = new DatePicker({
  value: new Date(),
  placeholder: 'Select a date'
});

datePicker.appendTo('#datepicker');
```

**Result:** Input shows "Select a date" when empty, replaced when user selects.

## Setting Initial Date

```typescript
// Set to specific date
const datePicker = new DatePicker({
  value: new Date(2025, 5, 15)  // June 15, 2025
});

datePicker.appendTo('#datepicker');
```

**Note:** Month is 0-indexed (0 = January, 11 = December).

## Custom Date Format

```typescript
const datePicker = new DatePicker({
  value: new Date(),
  format: 'dd/MM/yyyy'  // Display format
});

datePicker.appendTo('#datepicker');
```

**Common formats:**
- `M/d/yyyy` - 3/15/2025 (US default)
- `dd/MM/yyyy` - 15/03/2025 (European)
- `yyyy-MM-dd` - 2025-03-15 (ISO)
- `MMM dd, yyyy` - Mar 15, 2025 (Full month)

## Handling User Input

### Listen for Date Changes

```typescript
const datePicker = new DatePicker({
  value: new Date(),
  change: (args) => {
    console.log('Selected date:', args.value);
    console.log('Previous date:', args.prevDate);
  }
});

datePicker.appendTo('#datepicker');
```

**args object contains:**
- `args.value` - Newly selected date
- `args.prevDate` - Previously selected date
- `args.currentDate` - Currently focused date in calendar

### Get Value Programmatically

```typescript
const datePicker = new DatePicker({
  value: new Date()
});

datePicker.appendTo('#datepicker');

// Get current value
const selectedDate = datePicker.value;
console.log('Selected:', selectedDate);

// Set value programmatically
datePicker.value = new Date(2025, 11, 25);  // Dec 25, 2025
datePicker.dataBind();  // Update DOM
```

## Complete Example

```typescript
import { DatePicker } from '@syncfusion/ej2-calendars';
import '@syncfusion/ej2-base/styles/material.css';
import '@syncfusion/ej2-calendars/styles/material.css';

// Create DatePicker
const datePicker = new DatePicker({
  value: new Date(),
  placeholder: 'Select your date of birth',
  format: 'MMM dd, yyyy',
  change: (args) => {
    console.log('Date selected:', args.value);
    // Perform validation or update dependent controls
  }
});

// Mount to DOM
datePicker.appendTo('#datepicker');

// Optional: Add validation button
document.getElementById('submit').addEventListener('click', () => {
  if (datePicker.value) {
    console.log('Submitting date:', datePicker.value);
  } else {
    console.log('Please select a date');
  }
});
```

**Corresponding HTML:**
```html
<div>
  <label for="datepicker">Date of Birth:</label>
  <input id="datepicker" />
</div>
<button id="submit">Submit</button>
```

## TypeScript Model (Type-Safe)

Use the DatePickerModel interface for IDE autocomplete:

```typescript
import { DatePicker, DatePickerModel } from '@syncfusion/ej2-calendars';

// Type-safe configuration
const config: DatePickerModel = {
  value: new Date(),
  placeholder: 'Select date',
  format: 'dd/MM/yyyy',
  change: (args: any) => {
    console.log(args.value);
  }
};

const datePicker = new DatePicker(config);
datePicker.appendTo('#datepicker');
```

**Benefits:**
- IDE autocomplete for all properties
- Compile-time type checking
- Self-documenting code

## Common Setup Issues

| Problem | Solution |
|---------|----------|
| DatePicker not visible | Verify CSS imported and element ID matches |
| Clicking doesn't open calendar | Check if JavaScript loaded after DOM |
| Format ignored | Ensure `format` property is lowercase |
| Events not firing | Use `change` (not `onChange`), pass function reference |
| Multiple instances conflict | Give each input unique ID |

## Next Steps

1. **Add date constraints** - Read behavior-settings.md for min/max dates
2. **Format dates globally** - Read date-formats.md for locale support
3. **Highlight special dates** - Read special-dates.md for event markers
4. **Persist selections** - Read state-persistence.md for localStorage
5. **Keyboard navigation** - Read accessibility.md for keyboard shortcuts

## Troubleshooting

**Q: CSS theme not applying?**
A: Verify import path is exact. CSS must be imported before DatePicker initialization.

**Q: Change event not firing?**
A: Ensure `change` property is set in configuration (not as separate event binding).

**Q: Date showing wrong format?**
A: Set explicit `format` property. Don't rely on browser locale.

**Q: Can't set value after creation?**
A: Call `dataBind()` after updating `datePicker.value` to refresh DOM.
