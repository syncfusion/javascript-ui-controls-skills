# Special Dates & Visual Enhancement

Highlight important dates in the calendar with custom styling, icons, and tooltips.

## Overview

Special dates allow you to mark specific dates with visual indicators (custom CSS classes) and contextual information (tooltips). Common use cases include:
- **Birthdays** - Friends' and family's birth dates
- **Holidays** - National and religious holidays
- **Events** - Important meetings, deadlines, anniversaries
- **Availability** - Available appointment slots
- **Milestones** - Project deadlines, product launches

## Special Dates Array Structure

Define special dates as an array of objects:

```typescript
const specialDates = [
  {
    specialdate: '25/12/2025',    // Date in matching format
    customClass: 'holiday',        // CSS class for styling
    tooltip: 'Christmas Day'       // Hover tooltip text
  },
  {
    specialdate: '28/06/2025',
    customClass: 'birthday',
    tooltip: 'John\'s Birthday'
  }
];
```

### Data Structure Requirements

| Field | Type | Required | Purpose |
|-------|------|----------|---------|
| `specialdate` | String | Yes | Date matching DatePicker format (e.g., "dd/MM/yyyy") |
| `customClass` | String | Yes | CSS class applied to date element |
| `tooltip` | String | No | Text shown on hover |

## Field Mapping Configuration

Map your data structure to DatePicker's expected field names using the `fields` property:

```typescript
const specialDates = [
  { mydate: '25/12/2025', myclass: 'holiday', mytooltip: 'Christmas' },
  { mydate: '01/01/2026', myclass: 'holiday', mytooltip: 'New Year' }
];

const datePicker = new DatePicker({
  specialDates: specialDates,
  fields: {
    date: 'mydate',      // Map 'mydate' field to date
    iconCSS: 'myclass',  // Map 'myclass' field to CSS class
    tooltip: 'mytooltip' // Map 'mytooltip' field to tooltip
  }
});

datePicker.appendTo('#datepicker');
```

## Icon CSS Classes

Apply custom CSS classes to style special dates:

```typescript
const specialDates = [
  { specialdate: '28/06/2025', customClass: 'birthday', tooltip: 'Birthday' },
  { specialdate: '25/12/2025', customClass: 'holiday', tooltip: 'Holiday' },
  { specialdate: '31/12/2025', customClass: 'event', tooltip: 'NYE Event' }
];

const datePicker = new DatePicker({
  specialDates: specialDates,
  fields: { date: 'specialdate', iconCSS: 'customClass', tooltip: 'tooltip' }
});

datePicker.appendTo('#datepicker');
```

### CSS Styling for Classes

```css
/* Highlight birthday dates with cake icon color */
.e-datepicker .birthday::before {
  content: '🎂';
  display: inline-block;
  margin-right: 2px;
  color: #ff69b4;
}

/* Highlight holidays with star */
.e-datepicker .holiday {
  background-color: #fff3cd !important;  /* Light yellow background */
  font-weight: bold;
  color: #ff6b6b;
}

.e-datepicker .holiday::before {
  content: '⭐';
  display: inline-block;
  margin-right: 2px;
}

/* Event highlight */
.e-datepicker .event {
  background-color: #e7f3ff !important;  /* Light blue */
  border-radius: 50%;
}

.e-datepicker .event::before {
  content: '📌';
  display: inline-block;
  margin-right: 2px;
}
```

## Custom Tooltips

Show contextual information when users hover over special dates:

```typescript
const specialDates = [
  {
    specialdate: '28/06/2025',
    customClass: 'birthday',
    tooltip: 'John Doe - 28 years old'  // Rich tooltip text
  },
  {
    specialdate: '25/12/2025',
    customClass: 'holiday',
    tooltip: 'Christmas (Public Holiday) - Office Closed'
  }
];

const datePicker = new DatePicker({
  specialDates: specialDates,
  fields: { date: 'specialdate', iconCSS: 'customClass', tooltip: 'tooltip' },
  showTooltip: true  // IMPORTANT: Enable tooltips
});

datePicker.appendTo('#datepicker');
```

**Important:** Set `showTooltip: true` to display special date tooltips.

## Birthday Highlights Example

```typescript
const birthdays = [
  { date: '15/03/1990', name: 'Alice', cssClass: 'birthday' },
  { date: '22/07/1985', name: 'Bob', cssClass: 'birthday' },
  { date: '10/11/1992', name: 'Charlie', cssClass: 'birthday' }
];

// Transform to special dates format
const specialDates = birthdays.map(b => ({
  specialdate: b.date,
  customClass: b.cssClass,
  tooltip: `${b.name}'s Birthday`
}));

const datePicker = new DatePicker({
  value: new Date(),
  specialDates: specialDates,
  fields: { date: 'specialdate', iconCSS: 'customClass', tooltip: 'tooltip' },
  showTooltip: true
});

datePicker.appendTo('#datepicker');
```

### CSS for Birthdays

```css
.e-datepicker .birthday {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  border-radius: 50%;
  font-weight: bold;
}

.e-datepicker .birthday::before {
  content: '🎂';
  margin-right: 4px;
}

/* Tooltip styling */
.e-tooltip .e-tooltip-text {
  font-size: 12px;
  padding: 8px;
  background-color: #333;
  color: white;
  border-radius: 4px;
}
```

## Holiday Markers Example

```typescript
const holidays2025 = [
  { date: '01/01/2025', name: 'New Year', type: 'national' },
  { date: '14/02/2025', name: 'Valentine\'s Day', type: 'observance' },
  { date: '25/12/2025', name: 'Christmas', type: 'national' },
  { date: '31/12/2025', name: 'New Year\'s Eve', type: 'observance' }
];

const specialDates = holidays2025.map(h => ({
  specialdate: h.date,
  customClass: h.type === 'national' ? 'national-holiday' : 'observance-holiday',
  tooltip: h.name
}));

const datePicker = new DatePicker({
  specialDates: specialDates,
  fields: { date: 'specialdate', iconCSS: 'customClass', tooltip: 'tooltip' },
  showTooltip: true,
  format: 'MMM dd, yyyy'
});

datePicker.appendTo('#holiday-picker');
```

### CSS for Holidays

```css
.e-datepicker .national-holiday {
  background-color: #ff6b6b;
  color: white;
  font-weight: bold;
}

.e-datepicker .national-holiday::before {
  content: '🚩';
  margin-right: 4px;
}

.e-datepicker .observance-holiday {
  background-color: #ffd43b;
  color: #333;
}

.e-datepicker .observance-holiday::before {
  content: '📌';
  margin-right: 4px;
}
```

## Event/Deadline Markers

Mark project deadlines and important events:

```typescript
const projectEvents = [
  { date: '15/04/2025', event: 'Design Review', status: 'critical' },
  { date: '20/04/2025', event: 'Client Presentation', status: 'warning' },
  { date: '30/04/2025', event: 'Project Launch', status: 'success' }
];

const specialDates = projectEvents.map(e => ({
  specialdate: e.date,
  customClass: `event-${e.status}`,
  tooltip: e.event
}));

const datePicker = new DatePicker({
  specialDates: specialDates,
  fields: { date: 'specialdate', iconCSS: 'customClass', tooltip: 'tooltip' },
  showTooltip: true
});

datePicker.appendTo('#project-timeline');
```

### CSS for Event Markers

```css
.e-datepicker .event-critical {
  background-color: #ff4757;
  color: white;
  border: 2px solid #ff3838;
  border-radius: 3px;
}

.e-datepicker .event-critical::before {
  content: '⚠️';
  margin-right: 4px;
}

.e-datepicker .event-warning {
  background-color: #ffa502;
  color: white;
  border: 1px solid #ff8500;
  border-radius: 3px;
}

.e-datepicker .event-warning::before {
  content: '⏰';
  margin-right: 4px;
}

.e-datepicker .event-success {
  background-color: #2ed573;
  color: white;
  border: 1px solid #1fb85c;
  border-radius: 50%;
}

.e-datepicker .event-success::before {
  content: '✅';
  margin-right: 4px;
}
```

## Availability Calendar

Highlight available appointment slots:

```typescript
// Available dates from backend API
const availableDates = [
  '2025-04-10',
  '2025-04-12',
  '2025-04-14',
  '2025-04-17',
  '2025-04-19',
  '2025-04-21'
];

const specialDates = availableDates.map(d => {
  const [year, month, day] = d.split('-');
  return {
    specialdate: `${day}/${month}/${year}`,
    customClass: 'available-slot',
    tooltip: 'Available for booking'
  };
});

const datePicker = new DatePicker({
  specialDates: specialDates,
  fields: { date: 'specialdate', iconCSS: 'customClass', tooltip: 'tooltip' },
  showTooltip: true,
  change: (args) => {
    if (args.value) {
      const isAvailable = availableDates.includes(args.value.toISOString().split('T')[0]);
      console.log('Booking slot:', isAvailable);
    }
  }
});

datePicker.appendTo('#booking-calendar');
```

### CSS for Availability

```css
.e-datepicker .available-slot {
  background: linear-gradient(135deg, #11998e 0%, #38ef7d 100%);
  color: white;
  border-radius: 50%;
  font-weight: bold;
  box-shadow: 0 2px 4px rgba(0,0,0,0.2);
}

.e-datepicker .available-slot::before {
  content: '✓';
  margin-right: 4px;
  font-size: 14px;
}

.e-datepicker .available-slot:hover {
  box-shadow: 0 4px 8px rgba(0,0,0,0.3);
  cursor: pointer;
}
```

## Color-Coded Categories

Use multiple CSS classes for complex scenarios:

```typescript
const categorizedDates = [
  // Personal
  { date: '15/03/2025', category: 'personal', event: 'Birthday' },
  { date: '25/05/2025', category: 'personal', event: 'Anniversary' },
  
  // Work
  { date: '10/04/2025', category: 'work', event: 'Project Deadline' },
  { date: '20/04/2025', category: 'work', event: 'Team Meeting' },
  
  // Holiday
  { date: '01/01/2026', category: 'holiday', event: 'New Year' },
  { date: '25/12/2025', category: 'holiday', event: 'Christmas' }
];

const specialDates = categorizedDates.map(d => ({
  specialdate: d.date,
  customClass: d.category,
  tooltip: d.event
}));

const datePicker = new DatePicker({
  specialDates: specialDates,
  fields: { date: 'specialdate', iconCSS: 'customClass', tooltip: 'tooltip' },
  showTooltip: true
});

datePicker.appendTo('#calendar');
```

### Multi-Category CSS

```css
/* Personal events */
.e-datepicker .personal {
  background-color: #667eea;
  color: white;
  border-radius: 50%;
}

/* Work events */
.e-datepicker .work {
  background-color: #f093fb;
  color: white;
  border-radius: 3px;
}

/* Holidays */
.e-datepicker .holiday {
  background-color: #fa7921;
  color: white;
  border-radius: 3px;
  font-weight: bold;
}
```

## Dynamic Special Dates

Update special dates based on user interactions:

```typescript
const datePicker = new DatePicker({
  value: new Date(),
  specialDates: [],
  fields: { date: 'specialdate', iconCSS: 'customClass', tooltip: 'tooltip' },
  showTooltip: true
});

datePicker.appendTo('#datepicker');

// Load special dates for selected month
datePicker.change = (args) => {
  const selectedMonth = args.value.getMonth();
  const selectedYear = args.value.getFullYear();
  
  // Fetch dates from backend
  fetch(`/api/special-dates/${selectedYear}/${selectedMonth}`)
    .then(res => res.json())
    .then(data => {
      datePicker.specialDates = data;
      datePicker.dataBind();
    });
};
```

## Common Issues

| Issue | Cause | Solution |
|-------|-------|----------|
| Special dates not showing | Incorrect date format | Ensure format matches DatePicker's `dateFormat` |
| Field mapping wrong | Typo in field names | Verify `fields` property matches your data |
| Tooltips not appearing | `showTooltip` false | Set `showTooltip: true` |
| Styling not applied | CSS selector incorrect | Use `.e-datepicker .class-name` selector |
| Too many special dates | Performance issue | Limit special dates to current/next month |
| Date format mismatch | Different locales | Use consistent format across array and DatePicker |

## Best Practices

### 1. Validate Date Format

```typescript
// ✓ Good: Consistent format
const specialDates = [
  { specialdate: '25/12/2025', customClass: 'holiday', tooltip: 'Christmas' }
];

const datePicker = new DatePicker({
  format: 'dd/MM/yyyy',  // Matches special dates format
  specialDates: specialDates,
  fields: { date: 'specialdate', iconCSS: 'customClass', tooltip: 'tooltip' }
});
```

### 2. Load from API

```typescript
// ✓ Good: Fetch special dates from server
async function loadSpecialDates() {
  const response = await fetch('/api/special-dates');
  const data = await response.json();
  
  const specialDates = data.map(item => ({
    specialdate: item.date,
    customClass: item.category,
    tooltip: item.description
  }));
  
  const datePicker = new DatePicker({
    specialDates: specialDates,
    fields: { date: 'specialdate', iconCSS: 'customClass', tooltip: 'tooltip' }
  });
  
  datePicker.appendTo('#datepicker');
}

loadSpecialDates();
```

### 3. Combine with Min/Max

```typescript
// ✓ Good: Special dates within allowed range
new DatePicker({
  min: new Date(2025, 0, 1),
  max: new Date(2025, 11, 31),
  specialDates: specialDates,  // Only dates in range
  fields: { date: 'specialdate', iconCSS: 'customClass', tooltip: 'tooltip' }
});
```
