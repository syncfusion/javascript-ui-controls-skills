# Behavior Settings & Date Range

## Table of Contents
- [Selected Date & Value](#selected-date--value)
- [Date Range Constraints](#date-range-constraints)
- [Strict Mode Validation](#strict-mode-validation)
- [Calendar View Levels](#calendar-view-levels)
- [Inline Calendar Display](#inline-calendar-display)
- [Change Events & Callbacks](#change-events--callbacks)
- [Common Patterns](#common-patterns)

---

## Selected Date & Value

### Setting Initial Value

```typescript
// Set to today
const datePicker = new DatePicker({
  value: new Date()
});

// Set to specific date
const datePicker = new DatePicker({
  value: new Date(2025, 2, 15)  // March 15, 2025
});

// Set to null (no default)
const datePicker = new DatePicker({
  value: null
});

datePicker.appendTo('#datepicker');
```

### Getting Current Value

```typescript
const datePicker = new DatePicker({
  value: new Date(2025, 2, 15)
});

datePicker.appendTo('#datepicker');

// Get value at any time
const selected = datePicker.value;
console.log(selected);  // Date object

// Get formatted string
const formatted = datePicker.formattedValue();
console.log(formatted);  // "3/15/2025" (depending on format)
```

### Setting Value After Initialization

```typescript
const datePicker = new DatePicker({
  value: new Date()
});

datePicker.appendTo('#datepicker');

// Change value programmatically
datePicker.value = new Date(2025, 11, 25);
datePicker.dataBind();  // IMPORTANT: Update DOM

console.log(datePicker.value);  // Dec 25, 2025
```

**Important:** Always call `dataBind()` after changing `value` to update the display.

---

## Date Range Constraints

### Min and Max Dates

Restrict selectable dates within a specific range:

```typescript
const datePicker = new DatePicker({
  value: new Date(2025, 5, 15),
  min: new Date(2025, 0, 1),    // January 1, 2025
  max: new Date(2025, 11, 31),  // December 31, 2025
  format: 'MMM dd, yyyy'
});

datePicker.appendTo('#datepicker');
```

**Result:**
- Dates before Jan 1 are disabled (greyed out)
- Dates after Dec 31 are disabled
- Only dates within range can be selected

### Use Cases for Date Ranges

```typescript
// Booking system: Only future dates available
new DatePicker({
  min: new Date(),  // Today
  max: new Date(new Date().getTime() + 30*24*60*60*1000)  // 30 days ahead
});

// Fiscal year selection: Jan 1 - Dec 31
new DatePicker({
  min: new Date(2025, 0, 1),
  max: new Date(2025, 11, 31)
});

// Membership start date: Min 18 years ago
const eighteenYearsAgo = new Date();
eighteenYearsAgo.setFullYear(eighteenYearsAgo.getFullYear() - 18);
new DatePicker({
  max: eighteenYearsAgo
});

// Historical records: Last 2 years
const twoYearsAgo = new Date();
twoYearsAgo.setFullYear(twoYearsAgo.getFullYear() - 2);
new DatePicker({
  min: twoYearsAgo,
  max: new Date()
});
```

### Changing Range Dynamically

```typescript
const datePicker = new DatePicker({
  value: new Date(),
  min: new Date(2025, 0, 1),
  max: new Date(2025, 11, 31)
});

datePicker.appendTo('#datepicker');

// Update range based on user selection
function updateRange() {
  datePicker.min = new Date(2025, 6, 1);  // July
  datePicker.max = new Date(2025, 8, 30); // September
  datePicker.dataBind();
}
```

---

## Strict Mode Validation

### What is Strict Mode?

**Strict Mode ON (default):** User can type invalid dates, but error styling appears.
**Strict Mode OFF:** DatePicker only accepts valid dates in range; reverts to previous value if invalid.

### Enable/Disable Strict Mode

```typescript
// Strict mode ON (default): Allow invalid input with error styling
const lenient = new DatePicker({
  enableStrictMode: true,
  value: new Date()
});

// Strict mode OFF: Only accept valid dates in range
const strict = new DatePicker({
  enableStrictMode: false,
  value: new Date(),
  min: new Date(2025, 0, 1),
  max: new Date(2025, 11, 31)
});

lenient.appendTo('#lenient-datepicker');
strict.appendTo('#strict-datepicker');
```

### Strict Mode Behavior

#### Strict Mode ON (enableStrictMode: true)

```typescript
const datePicker = new DatePicker({
  enableStrictMode: true,
  min: new Date(2025, 0, 1),
  max: new Date(2025, 11, 31)
});

datePicker.appendTo('#datepicker');

// User types invalid date: "99/99/2025"
// Result: Error class added, input shows "99/99/2025" in red
```

**CSS for error styling:**
```css
.e-error .e-input {
  color: red;
  border-color: red;
}
```

#### Strict Mode OFF (enableStrictMode: false)

```typescript
const datePicker = new DatePicker({
  enableStrictMode: false,
  min: new Date(2025, 0, 1),
  max: new Date(2025, 11, 31)
});

datePicker.appendTo('#datepicker');

// User types invalid date: "99/99/2025"
// Result: Reverts to previous valid date (e.g., "3/15/2025")

// User types out-of-range: "6/1/2024"
// Result: Reverts to min date "1/1/2025"
```

### When to Use Each Mode

| Mode | Use Case |
|------|----------|
| **Strict ON** | Allow user to see/fix their input with error feedback |
| **Strict OFF** | Automatic correction for mobile/touch (no typos to fix) |

---

## Calendar View Levels

DatePicker supports four drill-down/up calendar views:

```
Month View (shows days)
    ↓ Click month header
    ↓
Year View (shows months)
    ↓ Click year header
    ↓
Decade View (shows years 2020-2029)
    ↓ Click decade header
    ↓
Century View (shows decades 2000-2099)
```

### Start and Depth Level Properties

```typescript
// Default: Month view, returns value on day select
new DatePicker({
  startLevel: 'month',  // Begin in month view
  depthLevel: 'month'   // Return value when day selected
});

// Year/Month selection: Start in year, return on month select
new DatePicker({
  startLevel: 'year',
  depthLevel: 'year',
  dateFormat: 'MMMM yyyy'
});

// Year only: Start in decade, return on year select
new DatePicker({
  startLevel: 'decade',
  depthLevel: 'year',
  dateFormat: 'yyyy'
});

// Quick year selection: Start in century
new DatePicker({
  startLevel: 'century',
  depthLevel: 'year',
  dateFormat: 'yyyy'
});
```

### Examples by Use Case

#### Month/Year Billing Period
```typescript
const datePicker = new DatePicker({
  value: new Date(),
  startLevel: 'year',
  depthLevel: 'year',
  dateFormat: 'MMMM yyyy',
  headerFormat: 'yyyy'
});

datePicker.appendTo('#billing-month');
// Opens in year view showing 12 months
// Returns full date when month selected (e.g., March 15, 2025 when March clicked)
```

#### Year-Only Report Selection
```typescript
const datePicker = new DatePicker({
  value: new Date(),
  startLevel: 'decade',
  depthLevel: 'year',
  dateFormat: 'yyyy',
  headerFormat: 'yyyy'
});

datePicker.appendTo('#report-year');
// Opens in decade view showing years 2020-2029
// Returns year only when year selected
```

#### Day-Level Default
```typescript
const datePicker = new DatePicker({
  value: new Date(),
  startLevel: 'month',
  depthLevel: 'month',
  dateFormat: 'MMM dd, yyyy'
});

datePicker.appendTo('#standard-date');
// Opens in month view showing days
// Returns full date when day selected
```

### Navigation Within Views

While in any view:
- **Left/Right arrows:** Move to previous/next period (month → year → decade)
- **Up arrow:** Drill up to parent view (month → year → decade)
- **Down arrow:** Drill down to child view (decade → year → month)
- **Ctrl+Up:** Jump to parent view
- **Ctrl+Down:** Jump to child view

---

## Inline Calendar Display

Display calendar permanently visible instead of in a popup:

```typescript
// Popup mode (default)
const popup = new DatePicker({
  value: new Date(),
  displayInline: false
});

// Inline mode (always visible)
const inline = new DatePicker({
  value: new Date(),
  displayInline: true
});

popup.appendTo('#popup-container');
inline.appendTo('#inline-container');
```

### HTML for Inline Mode

Use `<div>` instead of `<input>` for better visualization:

```html
<!-- Popup mode -->
<input id="popup-datepicker" />

<!-- Inline mode (calendar always visible) -->
<div id="inline-datepicker"></div>
```

### Inline Calendar with Footer

```typescript
const inline = new DatePicker({
  value: new Date(),
  displayInline: true,
  showFooter: true  // Show "Today" button
});

inline.appendTo('#calendar');
```

### Use Cases for Inline

```typescript
// Dashboard showing selected date
new DatePicker({
  displayInline: true,
  value: new Date()
});

// Schedule builder with persistent calendar
new DatePicker({
  displayInline: true,
  showFooter: true,
  showOtherMonths: true
});

// Date range picker with two inline calendars
const startDate = new DatePicker({ displayInline: true });
const endDate = new DatePicker({ displayInline: true });
```

---

## Change Events & Callbacks

### Change Event Handler

```typescript
const datePicker = new DatePicker({
  value: new Date(),
  change: (args) => {
    console.log('Event fired:', args);
  }
});

datePicker.appendTo('#datepicker');
```

### Event Arguments

```typescript
datePicker.change = (args) => {
  // args object contains:
  console.log(args.value);       // Selected date (Date object)
  console.log(args.prevDate);    // Previous date
  console.log(args.currentDate); // Currently focused in calendar
};
```

### Complete Change Handler Example

```typescript
const datePicker = new DatePicker({
  value: new Date(),
  min: new Date(2025, 0, 1),
  max: new Date(2025, 11, 31),
  change: (args) => {
    const selected = args.value;
    
    if (!selected) {
      console.log('Date cleared');
      return;
    }
    
    // Validate date is in range
    if (selected < datePicker.min || selected > datePicker.max) {
      console.log('Out of range!');
      return;
    }
    
    // Format for API
    const isoDate = selected.toISOString().split('T')[0];
    console.log('Submitting:', isoDate);
    
    // Send to server
    fetch('/api/update-date', {
      method: 'POST',
      body: JSON.stringify({ date: isoDate })
    });
  }
});

datePicker.appendTo('#datepicker');
```

### Suppressing Change Events

```typescript
const datePicker = new DatePicker({
  value: new Date()
});

datePicker.appendTo('#datepicker');

// Set value WITHOUT firing change event
datePicker.value = new Date(2025, 5, 15);
datePicker.dataBind();

// This won't trigger the change handler
```

---

## Common Patterns

### Pattern 1: Birthday Picker (Age 18-100)

```typescript
const today = new Date();
const maxAge = new Date(today.getFullYear() - 18, today.getMonth(), today.getDate());
const minAge = new Date(today.getFullYear() - 100, today.getMonth(), today.getDate());

const birthday = new DatePicker({
  value: maxAge,
  min: minAge,
  max: maxAge,
  format: 'MMM dd, yyyy',
  enableStrictMode: false
});

birthday.appendTo('#birthday');
```

### Pattern 2: Flexible Date Range

```typescript
const datePicker = new DatePicker({
  value: new Date(),
  format: 'dd/MM/yyyy'
});

datePicker.appendTo('#datepicker');

// Change range based on dropdown
document.getElementById('range-select').addEventListener('change', (e) => {
  const range = e.target.value;
  const today = new Date();
  
  if (range === 'last-week') {
    datePicker.min = new Date(today.getTime() - 7*24*60*60*1000);
    datePicker.max = today;
  } else if (range === 'this-month') {
    datePicker.min = new Date(today.getFullYear(), today.getMonth(), 1);
    datePicker.max = today;
  }
  
  datePicker.dataBind();
});
```

### Pattern 3: Dependent DatePicker

```typescript
const startDate = new DatePicker({
  value: new Date(2025, 0, 1),
  change: (args) => {
    // Update end date minimum when start changes
    endDate.min = args.value;
    endDate.dataBind();
  }
});

const endDate = new DatePicker({
  value: new Date(2025, 11, 31),
  min: new Date(2025, 0, 1)
});

startDate.appendTo('#start-date');
endDate.appendTo('#end-date');
```

### Pattern 4: Today Button in Inline Mode

```typescript
const inline = new DatePicker({
  displayInline: true,
  showFooter: true,
  value: new Date()
});

inline.appendTo('#calendar');

// Manual "Today" button
document.getElementById('today-btn').addEventListener('click', () => {
  inline.value = new Date();
  inline.dataBind();
});
```

---

## Troubleshooting

| Problem | Cause | Solution |
|---------|-------|----------|
| Value not changing | Forgot `dataBind()` | Call `datePicker.dataBind()` after setting value |
| Min/max not working | Timezone mismatch | Use consistent Date objects or ISO strings |
| Change event not firing | Event handler not in config | Set `change` in constructor, not separately |
| Strict mode too lenient | Expecting validation | Set `enableStrictMode: false` for strict behavior |
| Inline calendar too large | Container too small | Set explicit width/height on parent |
