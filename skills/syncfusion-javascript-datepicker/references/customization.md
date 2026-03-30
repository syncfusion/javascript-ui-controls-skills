# Customization & Styling

Customize DatePicker dimensions, appearance, and theme to match your application's design.

## Setting Dimensions

### Fixed Height and Width

```typescript
const datePicker = new DatePicker({
  value: new Date(),
  height: '50px',   // Set height
  width: '300px'    // Set width
});

datePicker.appendTo('#datepicker');
```

### Responsive Width (Mobile-Friendly)

```typescript
// ✓ Good: 100% width for responsive layout
const datePicker = new DatePicker({
  value: new Date(),
  width: '100%'
});

datePicker.appendTo('#datepicker');
```

**Result:** DatePicker stretches to fill parent container on all screen sizes.

### Common Dimension Patterns

```typescript
// Desktop: Fixed width with padding
new DatePicker({
  width: '250px',
  height: '40px'
});

// Mobile: Full width responsive
new DatePicker({
  width: '100%',
  height: '48px'  // Touch-friendly size
});

// Inline calendar: Square aspect
new DatePicker({
  displayInline: true,
  width: '400px',
  height: '400px'
});

// Compact input: Small size
new DatePicker({
  width: '120px',
  height: '30px'
});
```

### Media Query Responsive Design

```typescript
// In your TypeScript
const datePicker = new DatePicker({
  value: new Date()
});

datePicker.appendTo('#datepicker');

// Use CSS media queries for responsive sizing
// CSS file:
@media (max-width: 768px) {
  #datepicker {
    width: 100% !important;
    height: 48px !important;
  }
}

@media (min-width: 769px) {
  #datepicker {
    width: 250px !important;
    height: 40px !important;
  }
}
```

## Footer Display

The footer contains a "Today" button for quick navigation to current date.

### Show/Hide Footer

```typescript
// Show footer (default)
new DatePicker({
  value: new Date(),
  showFooter: true   // "Today" button visible
});

// Hide footer
new DatePicker({
  value: new Date(),
  showFooter: false  // Only calendar, no "Today" button
});
```

### Use Cases

```typescript
// Always show "Today" for easy reset
new DatePicker({
  showFooter: true,
  value: new Date()
});

// Minimal interface: Hide footer
new DatePicker({
  showFooter: false,
  displayInline: true
});

// With manual "Today" button
new DatePicker({
  showFooter: false
});

// Add custom "Today" button
document.getElementById('today-btn').addEventListener('click', () => {
  datePicker.value = new Date();
  datePicker.dataBind();
});
```

## Popup Button Visibility

Control whether the calendar toggle button appears next to the input.

### Show/Hide Popup Button

```typescript
// Show button (default)
new DatePicker({
  value: new Date(),
  showPopupButton: true  // Calendar icon visible
});

// Hide button: Focus input to open
new DatePicker({
  value: new Date(),
  showPopupButton: false  // Click input to open calendar
});
```

### Use Cases

```typescript
// Standard: Show button for clarity
new DatePicker({
  placeholder: 'Select date',
  showPopupButton: true
});

// Minimal: Button hidden, focus to open
new DatePicker({
  placeholder: 'Click to select',
  showPopupButton: false
});

// Mobile: Large touch target
new DatePicker({
  height: '48px',
  showPopupButton: true
});
```

## Show/Hide Other Month Days

Display or hide days from adjacent months in the calendar grid.

### Other Months Toggle

```typescript
// Show days from previous/next months (default)
new DatePicker({
  value: new Date(),
  showOtherMonths: true   // Grayed out adjacent month days
});

// Hide: Only current month days
new DatePicker({
  value: new Date(),
  showOtherMonths: false  // Clean, focused calendar
});
```

### Visual Comparison

```typescript
// Show other months (more context)
new DatePicker({
  showOtherMonths: true,
  format: 'dd/MM/yyyy'
});
// Calendar shows:
// [28] [29] [30] [31] [1]  [2]  [3]    ← March 31 + April 1-3
// [4]  [5]  [6]  [7]  [8]  [9] [10]    ← April days

// Hide other months (focused)
new DatePicker({
  showOtherMonths: false,
  format: 'dd/MM/yyyy'
});
// Calendar shows:
// [1]  [2]  [3]  [4]  [5]  [6]  [7]    ← April days only
// [8]  [9] [10] [11] [12] [13] [14]
```

## CSS Class Customization

Apply custom CSS classes to DatePicker for advanced styling.

### cssClass Property

```typescript
const datePicker = new DatePicker({
  value: new Date(),
  cssClass: 'custom-date-input'  // Your custom class
});

datePicker.appendTo('#datepicker');
```

### Custom Styling Example

```typescript
// In TypeScript
const datePicker = new DatePicker({
  value: new Date(),
  cssClass: 'success-input',
  placeholder: 'Select approval date'
});

datePicker.appendTo('#datepicker');
```

```css
/* In CSS file */
.success-input .e-input {
  border: 2px solid #2ed573;
  background-color: #f0fff0;
}

.success-input .e-input-focus {
  border-color: #1fb85c;
  box-shadow: 0 0 0 3px rgba(46, 213, 115, 0.1);
}
```

### Multiple CSS Classes

```typescript
const datePicker = new DatePicker({
  value: new Date(),
  cssClass: 'e-small e-outline e-success',  // Multiple classes
  placeholder: 'Compact success input'
});

datePicker.appendTo('#datepicker');
```

### Built-in Syncfusion Classes

```typescript
// Small size
new DatePicker({
  cssClass: 'e-small'
});

// Outline style (no fill)
new DatePicker({
  cssClass: 'e-outline'
});

// Success/Primary states
new DatePicker({
  cssClass: 'e-success'
});

new DatePicker({
  cssClass: 'e-warning'
});

new DatePicker({
  cssClass: 'e-danger'
});

// Combined
new DatePicker({
  cssClass: 'e-small e-outline e-success'
});
```

## Theme Switching

Change the visual theme for DatePicker.

### Available Themes

```typescript
// Material (default)
import '@syncfusion/ej2-base/styles/material.css';
import '@syncfusion/ej2-calendars/styles/material.css';

// Fabric
import '@syncfusion/ej2-base/styles/fabric.css';
import '@syncfusion/ej2-calendars/styles/fabric.css';

// Bootstrap (v3)
import '@syncfusion/ej2-base/styles/bootstrap.css';
import '@syncfusion/ej2-calendars/styles/bootstrap.css';

// Bootstrap 4
import '@syncfusion/ej2-base/styles/bootstrap4.css';
import '@syncfusion/ej2-calendars/styles/bootstrap4.css';

// Bootstrap 5
import '@syncfusion/ej2-base/styles/bootstrap5.css';
import '@syncfusion/ej2-calendars/styles/bootstrap5.css';

// High Contrast
import '@syncfusion/ej2-base/styles/highcontrast.css';
import '@syncfusion/ej2-calendars/styles/highcontrast.css';

// High Contrast Light
import '@syncfusion/ej2-base/styles/highcontrast-light.css';
import '@syncfusion/ej2-calendars/styles/highcontrast-light.css';

// Tailwind
import '@syncfusion/ej2-base/styles/tailwind.css';
import '@syncfusion/ej2-calendars/styles/tailwind.css';

// Fluent
import '@syncfusion/ej2-base/styles/fluent.css';
import '@syncfusion/ej2-calendars/styles/fluent.css';
```

### Switching Themes

```typescript
// main.ts - Import one theme

// Material (default)
import '@syncfusion/ej2-base/styles/material.css';
import '@syncfusion/ej2-calendars/styles/material.css';

// All DatePickers use Material theme throughout app
const datePicker1 = new DatePicker({ value: new Date() });
const datePicker2 = new DatePicker({ value: new Date() });
```

### Dynamic Theme Switching

```typescript
// Change theme on user preference
function switchTheme(themeName) {
  const baseLink = document.getElementById('base-theme');
  const calendarLink = document.getElementById('calendar-theme');
  
  baseLink.href = `/styles/${themeName}-base.css`;
  calendarLink.href = `/styles/${themeName}-calendar.css`;
}

// Usage
switchTheme('fabric');  // Switch to Fabric theme
switchTheme('bootstrap4');  // Switch to Bootstrap 4
```

## Theme Studio Customization

Create custom themes using Syncfusion Theme Studio.

### Steps to Create Custom Theme

1. Visit [Syncfusion Theme Studio](https://ej2.syncfusion.com/themestudio/)
2. Select DatePicker component
3. Customize colors, fonts, sizing
4. Export CSS file
5. Import in your application

### Custom Theme Example

```typescript
// main.ts - Import custom theme
import '@syncfusion/ej2-base/styles/material.css';
import './custom-theme.css';  // Your custom theme

const datePicker = new DatePicker({
  value: new Date()
});

datePicker.appendTo('#datepicker');
```

```css
/* custom-theme.css - Your custom colors */
.e-input {
  background-color: #f5f5f5;
  border: 1px solid #ddd;
  color: #333;
}

.e-popup .e-datepicker .e-calendar .e-header {
  background-color: #667eea;
  color: white;
}

.e-popup .e-datepicker .e-calendar .e-day.e-selected {
  background-color: #667eea;
  color: white;
}
```

## Advanced Styling Examples

### Brand Color Integration

```typescript
// main.ts
import '@syncfusion/ej2-base/styles/material.css';
import '@syncfusion/ej2-calendars/styles/material.css';

const datePicker = new DatePicker({
  value: new Date(),
  cssClass: 'brand-primary'
});

datePicker.appendTo('#datepicker');
```

```css
/* Override for brand colors */
.brand-primary .e-input {
  border: 2px solid #your-brand-color;
  background: linear-gradient(to right, #brand-light, #brand-lighter);
}

.brand-primary .e-input-focus {
  border-color: #your-brand-color;
  box-shadow: 0 0 0 4px rgba(your-brand-color, 0.1);
}

.brand-primary .e-popup .e-datepicker .e-calendar .e-day.e-selected {
  background-color: #your-brand-color;
  color: white;
}
```

### Flat Design

```typescript
const datePicker = new DatePicker({
  value: new Date(),
  cssClass: 'flat-design'
});

datePicker.appendTo('#datepicker');
```

```css
.flat-design .e-input {
  border: none;
  border-bottom: 2px solid #ddd;
  background-color: transparent;
  border-radius: 0;
  font-size: 16px;
  padding: 8px 0;
}

.flat-design .e-input:focus {
  border-bottom-color: #667eea;
  background-color: #f9f9f9;
}

.flat-design .e-popup .e-datepicker .e-calendar {
  border-radius: 8px;
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.1);
}
```

### Modern Neumorphic Design

```typescript
const datePicker = new DatePicker({
  value: new Date(),
  cssClass: 'neumorphic'
});

datePicker.appendTo('#datepicker');
```

```css
.neumorphic .e-input {
  background: linear-gradient(135deg, #f5f5f5, #ffffff);
  border: none;
  box-shadow: 5px 5px 10px #b8b9be, -5px -5px 10px #ffffff;
  border-radius: 10px;
  padding: 10px 15px;
}

.neumorphic .e-input:focus {
  box-shadow: inset 5px 5px 10px #b8b9be, inset -5px -5px 10px #ffffff;
}
```

### Dark Mode

```typescript
const datePicker = new DatePicker({
  value: new Date(),
  cssClass: 'dark-mode'
});

datePicker.appendTo('#datepicker');
```

```css
.dark-mode .e-input {
  background-color: #2d2d2d;
  color: #e0e0e0;
  border: 1px solid #404040;
}

.dark-mode .e-popup .e-datepicker .e-calendar {
  background-color: #1e1e1e;
  color: #e0e0e0;
}

.dark-mode .e-popup .e-datepicker .e-calendar .e-header {
  background-color: #404040;
  color: #e0e0e0;
}

.dark-mode .e-popup .e-datepicker .e-calendar .e-day {
  color: #e0e0e0;
}

.dark-mode .e-popup .e-datepicker .e-calendar .e-day.e-selected {
  background-color: #667eea;
  color: white;
}
```

## Responsive Customization

```typescript
// Base DatePicker
const datePicker = new DatePicker({
  value: new Date(),
  width: '100%'
});

datePicker.appendTo('#datepicker');
```

```css
/* Desktop */
@media (min-width: 1024px) {
  #datepicker {
    max-width: 300px;
  }
}

/* Tablet */
@media (max-width: 1023px) and (min-width: 768px) {
  #datepicker {
    max-width: 100%;
    height: 44px;
  }
}

/* Mobile */
@media (max-width: 767px) {
  #datepicker {
    width: 100%;
    height: 48px;
    font-size: 16px;  /* Prevent zoom on iOS */
  }
  
  .e-popup .e-datepicker .e-calendar {
    width: 100vw;
    max-width: 100%;
  }
}
```

## Best Practices

### 1. Import CSS Once

```typescript
// ✓ Good: Import in main.ts
import '@syncfusion/ej2-base/styles/material.css';
import '@syncfusion/ej2-calendars/styles/material.css';

// Use throughout app
const datePicker1 = new DatePicker({...});
const datePicker2 = new DatePicker({...});
```

### 2. Use Responsive Width

```typescript
// ✓ Good: Mobile-friendly
new DatePicker({
  width: '100%',
  height: '40px'
});
```

### 3. Test Theme Changes

```typescript
// ✓ Good: Verify theme works
before(() => {
  datePicker = new DatePicker({
    value: new Date(),
    cssClass: 'custom-theme'
  });
  datePicker.appendTo('#datepicker');
});

after(() => {
  datePicker.destroy();
});
```

### 4. Override Selectively

```css
/* ✓ Good: Specific selectors */
.custom-datepicker .e-input {
  border-color: #667eea;
}

/* ✗ Avoid: Overly broad */
.e-input {
  border-color: #667eea;
}
```
