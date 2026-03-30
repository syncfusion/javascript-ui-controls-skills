# Accessibility & Keyboard Navigation

Implement full keyboard accessibility and WCAG 2.2 compliance for DatePicker.

## Keyboard Navigation Overview

DatePicker supports comprehensive keyboard shortcuts for hands-free operation. Users can navigate the calendar, select dates, and control popups entirely without mouse/touch.

### Keyboard Support

Full keyboard support is enabled in DatePicker. Users can:
- Open/close calendar with keyboard
- Navigate dates with arrow keys
- Jump between views (month → year → decade)
- Select dates with Enter
- Close with Escape

## Keyboard Shortcut Keys

### Opening and Closing

| Shortcut | Action | Context |
|----------|--------|---------|
| **Alt + Down** | Open calendar popup | Input focused |
| **Esc** | Close calendar popup | Calendar open |
| **Enter** | Select focused date | Date focused in calendar |

### Date Navigation

| Shortcut | Action | View |
|----------|--------|------|
| **Left Arrow** | Move to previous date | Month view |
| **Right Arrow** | Move to next date | Month view |
| **Up Arrow** | Move one week up | Month view |
| **Down Arrow** | Move one week down | Month view |

### View Switching

| Shortcut | Action | Result |
|----------|--------|--------|
| **Ctrl + Up** | Drill up to parent view | Month → Year → Decade → Century |
| **Ctrl + Down** | Drill down to child view | Century → Decade → Year → Month |
| **Ctrl + Left** | Previous month/period | Depends on current view |
| **Ctrl + Right** | Next month/period | Depends on current view |

### Complete Keyboard Reference

```
OPENING/CLOSING
├── Alt + Down     → Open calendar
├── Esc            → Close calendar
└── Enter          → Select focused date

MONTH VIEW NAVIGATION
├── Left Arrow     → Previous day
├── Right Arrow    → Next day
├── Up Arrow       → Same day, previous week
├── Down Arrow     → Same day, next week
└── Ctrl + [arrow] → Month/view changes

VIEW DRILLING
├── Ctrl + Up      → Zoom out (month → year → decade)
├── Ctrl + Down    → Zoom in (decade → year → month)
└── Ctrl + Left/Right → Navigate in current view
```

## Enabling Keyboard Navigation

```typescript
import { DatePicker } from '@syncfusion/ej2-calendars';

// Keyboard navigation is enabled by default
const datePicker = new DatePicker({
  value: new Date(),
  allowKeyboardNavigation: true  // Explicitly enabled
});

datePicker.appendTo('#datepicker');
```

**Note:** `allowKeyboardNavigation` defaults to `true` - no action needed unless you want to disable it.

### Disabling Keyboard Navigation

```typescript
// Disable keyboard if you want mouse-only mode
const datePicker = new DatePicker({
  value: new Date(),
  allowKeyboardNavigation: false  // Disable keyboard
});

datePicker.appendTo('#datepicker');
```

## Keyboard Interaction Examples

### Example 1: Navigate and Select a Date

```
1. User presses Alt+Down
   → Calendar popup opens, focuses today

2. User presses Right Arrow 3 times
   → Focus moves to 3 days ahead

3. User presses Enter
   → Selected date becomes focused date
   → Calendar closes
```

### Example 2: Jump Between Months

```
1. Calendar open in month view

2. User presses Ctrl+Right
   → Calendar advances to next month
   → Calendar header updates

3. User presses Left Arrow multiple times
   → Moves backward within new month

4. User presses Enter
   → Selects date, calendar closes
```

### Example 3: View Switching

```
1. Calendar open in month view (April 2025)

2. User presses Ctrl+Up
   → Switches to year view (showing months of 2025)

3. User presses Right Arrow twice
   → Focuses June (2 months to the right)

4. User presses Ctrl+Down
   → Returns to month view (June 2025)

5. User presses Enter
   → Selects June 1st, calendar closes
```

## Screen Reader Support

DatePicker provides full ARIA annotations for screen readers.

### ARIA Attributes

```html
<!-- DatePicker input with ARIA -->
<input 
  id="datepicker" 
  role="combobox"
  aria-label="Select appointment date"
  aria-controls="datepicker-calendar"
  aria-expanded="false"
  aria-haspopup="dialog"
/>

<!-- Calendar popup with ARIA -->
<div id="datepicker-calendar" role="dialog" aria-label="Calendar">
  <!-- Calendar content -->
</div>
```

### Screen Reader Announcements

When using a screen reader:
- **On focus:** "Combobox, Select appointment date, collapsed"
- **On Alt+Down:** "Calendar dialog opened, April 2025"
- **On arrow key:** "Tuesday, April 15, 2025"
- **On Enter:** "April 15, 2025 selected"

### Testing with Screen Readers

```typescript
// Add descriptive labels for screen readers
const datePicker = new DatePicker({
  value: new Date(),
  placeholder: 'Select appointment date',
  change: (args) => {
    // Announce selection to screen reader
    const announcement = document.createElement('div');
    announcement.setAttribute('role', 'status');
    announcement.setAttribute('aria-live', 'polite');
    announcement.textContent = `Date selected: ${args.value}`;
    document.body.appendChild(announcement);
    
    setTimeout(() => announcement.remove(), 1000);
  }
});

datePicker.appendTo('#datepicker');
```

## Focus Management

### Initial Focus

```typescript
const datePicker = new DatePicker({
  value: new Date(),
  created: () => {
    // Calendar focuses today on opening
    console.log('DatePicker initialized');
  }
});

datePicker.appendTo('#datepicker');

// Focus input for keyboard navigation
document.getElementById('datepicker').focus();
```

### Focus Indicators

```css
/* Ensure visible focus indicators */
.e-datepicker .e-input:focus {
  outline: 2px solid #667eea;
  outline-offset: 2px;
}

.e-popup .e-datepicker .e-day:focus {
  outline: 2px solid #667eea;
  border-radius: 4px;
}

.e-popup .e-datepicker .e-day.e-focused {
  background-color: #e3f2fd;
  color: #1976d2;
}
```

### Tab Order

```html
<!-- Good: Logical tab order -->
<input id="start-date" />    <!-- Tab 1 -->
<input id="end-date" />      <!-- Tab 2 -->
<button id="submit">Submit</button> <!-- Tab 3 -->
```

```typescript
// Ensure DatePickers are in form tab order
const startDate = new DatePicker({ value: new Date() });
const endDate = new DatePicker({ value: new Date() });

startDate.appendTo('#start-date');
endDate.appendTo('#end-date');

// Both are in natural tab order
```

## WCAG 2.2 Compliance

DatePicker meets WCAG 2.2 Level AA standards:

### Success Criteria Met

| Criterion | Status | Details |
|-----------|--------|---------|
| **1.4.3 Contrast (Minimum)** | ✓ Pass | 4.5:1 text/background contrast |
| **2.1.1 Keyboard** | ✓ Pass | All functions available by keyboard |
| **2.1.2 No Keyboard Trap** | ✓ Pass | Focus can leave DatePicker |
| **2.4.3 Focus Order** | ✓ Pass | Logical, predictable focus order |
| **2.4.7 Focus Visible** | ✓ Pass | Clear focus indicators |
| **3.2.4 Consistent Identification** | ✓ Pass | Consistent UI patterns |
| **4.1.2 Name, Role, Value** | ✓ Pass | Proper ARIA attributes |
| **4.1.3 Status Messages** | ✓ Pass | Announcements for screen readers |

### Testing for WCAG Compliance

```typescript
// Test keyboard accessibility
function testKeyboardAccess() {
  const datePicker = new DatePicker({
    value: new Date(),
    allowKeyboardNavigation: true
  });
  
  datePicker.appendTo('#datepicker');
  
  // All keyboard shortcuts should work
  // ✓ Alt+Down opens calendar
  // ✓ Arrow keys navigate
  // ✓ Ctrl+Up/Down switch views
  // ✓ Enter selects date
  // ✓ Esc closes calendar
}

// Test contrast ratios
function testContrast() {
  const input = document.querySelector('.e-input') as HTMLElement;
  const style = window.getComputedStyle(input);
  
  // Verify 4.5:1 contrast ratio
  console.log('Background:', style.backgroundColor);
  console.log('Color:', style.color);
}
```

## Accessible Example

```typescript
import { DatePicker } from '@syncfusion/ej2-calendars';

class AccessibleDatePicker {
  private datePicker: DatePicker;
  
  constructor(elementId: string, label: string) {
    this.createAccessibleDatePicker(elementId, label);
  }
  
  private createAccessibleDatePicker(elementId: string, label: string) {
    // Create label element
    const labelElement = document.createElement('label');
    labelElement.htmlFor = elementId;
    labelElement.textContent = label;
    
    // Create DatePicker
    this.datePicker = new DatePicker({
      value: new Date(),
      allowKeyboardNavigation: true,
      placeholder: `${label} (MM/DD/YYYY)`,
      change: (args) => {
        this.announceSelection(args.value);
      },
      created: () => {
        // Ensure focus management
        const input = document.getElementById(elementId);
        if (input) {
          input.addEventListener('focus', () => {
            console.log('DatePicker focused');
          });
        }
      }
    });
    
    // Add ARIA attributes
    const element = document.getElementById(elementId);
    if (element) {
      element.setAttribute('aria-label', label);
      element.setAttribute('aria-describedby', `${elementId}-hint`);
    }
    
    // Append label and DatePicker
    const container = document.getElementById(elementId)?.parentElement;
    if (container) {
      container.insertBefore(labelElement, container.firstChild);
    }
    
    this.datePicker.appendTo(`#${elementId}`);
    
    // Add hint for screen readers
    const hint = document.createElement('div');
    hint.id = `${elementId}-hint`;
    hint.className = 'sr-only';  // Visually hidden
    hint.textContent = 'Use keyboard: Alt+Down to open, arrow keys to navigate, Enter to select, Esc to close';
    
    document.getElementById(elementId)?.parentElement?.appendChild(hint);
  }
  
  private announceSelection(date: Date | null) {
    if (!date) return;
    
    // Create live region for announcement
    const announcement = document.createElement('div');
    announcement.setAttribute('role', 'status');
    announcement.setAttribute('aria-live', 'polite');
    announcement.setAttribute('aria-atomic', 'true');
    announcement.className = 'sr-only';
    announcement.textContent = `Selected ${date.toLocaleDateString()}`;
    
    document.body.appendChild(announcement);
    setTimeout(() => announcement.remove(), 3000);
  }
}

// Usage
new AccessibleDatePicker('appointment-date', 'Appointment Date');
```

### Supporting CSS

```css
/* Screen reader only (visually hidden) */
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}

/* Focus indicators */
.e-datepicker .e-input:focus {
  outline: 2px solid #667eea;
  outline-offset: 2px;
}

/* High contrast mode support */
@media (prefers-contrast: more) {
  .e-datepicker .e-input {
    border-width: 2px;
  }
  
  .e-datepicker .e-input:focus {
    outline-width: 3px;
  }
}

/* Reduced motion support */
@media (prefers-reduced-motion: reduce) {
  .e-popup {
    animation: none;
  }
}
```

## Keyboard Testing Checklist

- [ ] **Opening:** Alt+Down opens calendar from input
- [ ] **Closing:** Esc closes calendar
- [ ] **Navigation:** Arrow keys move through dates
- [ ] **View Change:** Ctrl+Up/Down switch between month/year/decade
- [ ] **Selection:** Enter selects focused date
- [ ] **Focus Visible:** Clear focus indicator on all interactive elements
- [ ] **No Trap:** Can Tab out of DatePicker to next element
- [ ] **Label Present:** Input has associated label
- [ ] **Screen Reader:** Calendar announces current month/year
- [ ] **Contrast:** Text meets 4.5:1 contrast ratio
- [ ] **Tab Order:** Logical tab order through page elements
- [ ] **Error States:** Any validation errors announced to screen readers

## Browser Support

| Browser | Keyboard Support | Screen Reader | ARIA Support |
|---------|-----------------|---------------|--------------|
| Chrome | ✓ Full | ✓ Full | ✓ Full |
| Firefox | ✓ Full | ✓ Full | ✓ Full |
| Safari | ✓ Full | ✓ Full | ✓ Full |
| Edge | ✓ Full | ✓ Full | ✓ Full |

## Troubleshooting

| Issue | Cause | Solution |
|-------|-------|----------|
| Keyboard shortcuts not working | `allowKeyboardNavigation` false | Set to `true` or remove property |
| Focus trap in calendar | Tab order not configured | Ensure DatePicker is in natural tab flow |
| Screen reader not announcing | No ARIA labels | Add `aria-label` to input |
| Low contrast text | Theme not optimized | Use high contrast theme or custom CSS |
| Focus indicator invisible | CSS override | Add focus styles with `outline` |

## Resources

- [WCAG 2.2 Guidelines](https://www.w3.org/WAI/WCAG22/quickref/)
- [ARIA Authoring Practices](https://www.w3.org/WAI/ARIA/apg/)
- [Screen Reader Testing](https://www.nvaccess.org/)
- [WebAIM Keyboard Accessibility](https://webaim.org/articles/keyboard/)
