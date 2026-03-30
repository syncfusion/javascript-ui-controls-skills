# State Persistence

Save and restore DatePicker selections across browser sessions using localStorage.

## Overview

State persistence enables:
- **Session Continuity** - Date selection remembered after page refresh
- **Better UX** - Users return to previously selected dates
- **Form Recovery** - Preserve form data during accidental navigation
- **Analytics** - Track user preferences over time

## Saving to localStorage

### Basic Save Pattern

```typescript
import { DatePicker } from '@syncfusion/ej2-calendars';

const datePicker = new DatePicker({
  value: new Date(),
  change: (args) => {
    // Save to localStorage when date changes
    if (args.value) {
      localStorage.setItem('selectedDate', args.value.toISOString());
      console.log('Date saved:', args.value);
    }
  }
});

datePicker.appendTo('#datepicker');
```

### Save with Key

```typescript
const datePicker = new DatePicker({
  value: new Date(),
  change: (args) => {
    if (args.value) {
      // Use descriptive key
      localStorage.setItem('appointment_date', args.value.toISOString());
    }
  }
});

datePicker.appendTo('#appointment-picker');
```

## Retrieving Persisted State

### Load on Initialization

```typescript
import { DatePicker } from '@syncfusion/ej2-calendars';

// Retrieve saved date
const savedDate = localStorage.getItem('selectedDate');
const initialValue = savedDate ? new Date(savedDate) : new Date();

const datePicker = new DatePicker({
  value: initialValue,
  change: (args) => {
    if (args.value) {
      localStorage.setItem('selectedDate', args.value.toISOString());
    }
  }
});

datePicker.appendTo('#datepicker');
```

### Automatic Restoration

```typescript
function initializeDatePicker() {
  // Check for saved state
  const savedDate = localStorage.getItem('userBirthday');
  
  const datePicker = new DatePicker({
    value: savedDate ? new Date(savedDate) : null,
    placeholder: 'Select your birthday',
    change: (args) => {
      if (args.value) {
        localStorage.setItem('userBirthday', args.value.toISOString());
      }
    }
  });
  
  datePicker.appendTo('#birthday-picker');
  
  return datePicker;
}

// Call on app load
initializeDatePicker();
```

## Complete Example

```typescript
import { DatePicker } from '@syncfusion/ej2-calendars';
import '@syncfusion/ej2-base/styles/material.css';
import '@syncfusion/ej2-calendars/styles/material.css';

class PersistentDatePicker {
  private datePicker: DatePicker;
  private storageKey: string;
  
  constructor(elementId: string, storageKey: string) {
    this.storageKey = storageKey;
    this.initializeDatePicker(elementId);
  }
  
  private initializeDatePicker(elementId: string) {
    // Load saved value
    const savedDate = this.getSavedDate();
    
    this.datePicker = new DatePicker({
      value: savedDate,
      placeholder: 'Select a date',
      format: 'MMM dd, yyyy',
      change: (args) => this.saveDateToStorage(args.value)
    });
    
    this.datePicker.appendTo(`#${elementId}`);
  }
  
  private getSavedDate(): Date | null {
    const saved = localStorage.getItem(this.storageKey);
    if (!saved) return new Date();  // Default to today
    
    try {
      return new Date(saved);
    } catch {
      console.error('Invalid saved date:', saved);
      return new Date();
    }
  }
  
  private saveDateToStorage(date: Date | null) {
    if (!date) {
      localStorage.removeItem(this.storageKey);
      return;
    }
    
    localStorage.setItem(this.storageKey, date.toISOString());
    console.log(`Saved ${this.storageKey}:`, date);
  }
  
  public getValue(): Date | null {
    return this.datePicker.value;
  }
  
  public setValue(date: Date) {
    this.datePicker.value = date;
    this.datePicker.dataBind();
    this.saveDateToStorage(date);
  }
  
  public clearSavedState() {
    localStorage.removeItem(this.storageKey);
    this.datePicker.value = new Date();
    this.datePicker.dataBind();
  }
}

// Usage
const picker = new PersistentDatePicker('datepicker', 'user_selected_date');
```

## Multi-Page State Sharing

Share selected date across multiple pages:

```typescript
// page1.ts
const datePicker = new DatePicker({
  value: new Date(),
  change: (args) => {
    if (args.value) {
      localStorage.setItem('reportDate', args.value.toISOString());
      // Navigate to report page
      window.location.href = '/report';
    }
  }
});

datePicker.appendTo('#date-selector');
```

```typescript
// page2.ts (report page)
const reportDate = localStorage.getItem('reportDate');

if (reportDate) {
  const date = new Date(reportDate);
  loadReport(date);
  console.log('Report for:', date);
}
```

## Form State Persistence

Save entire form including DatePicker:

```typescript
interface FormData {
  name: string;
  email: string;
  appointmentDate: string;
}

const datePicker = new DatePicker({
  value: new Date(),
  change: (args) => {
    saveFormState();
  }
});

datePicker.appendTo('#appointment');

function saveFormState() {
  const formData: FormData = {
    name: (document.getElementById('name') as HTMLInputElement).value,
    email: (document.getElementById('email') as HTMLInputElement).value,
    appointmentDate: datePicker.value?.toISOString() || ''
  };
  
  localStorage.setItem('formData', JSON.stringify(formData));
  console.log('Form saved:', formData);
}

function restoreFormState() {
  const saved = localStorage.getItem('formData');
  
  if (!saved) return;
  
  try {
    const data: FormData = JSON.parse(saved);
    
    (document.getElementById('name') as HTMLInputElement).value = data.name;
    (document.getElementById('email') as HTMLInputElement).value = data.email;
    
    if (data.appointmentDate) {
      datePicker.value = new Date(data.appointmentDate);
      datePicker.dataBind();
    }
    
    console.log('Form restored:', data);
  } catch (error) {
    console.error('Failed to restore form:', error);
  }
}

// Restore on load
window.addEventListener('DOMContentLoaded', () => {
  restoreFormState();
});
```

## Clearing Saved State

### Manual Clear

```typescript
const datePicker = new DatePicker({
  value: new Date()
});

datePicker.appendTo('#datepicker');

// Clear saved state
function clearSavedDate() {
  localStorage.removeItem('selectedDate');
  datePicker.value = null;
  datePicker.dataBind();
  console.log('Saved state cleared');
}

// Bind to button
document.getElementById('clear-btn').addEventListener('click', clearSavedDate);
```

### Clear on Logout

```typescript
function logout() {
  // Clear DatePicker state
  localStorage.removeItem('userBirthday');
  localStorage.removeItem('appointmentDate');
  
  // Clear other session data
  sessionStorage.clear();
  
  // Redirect to login
  window.location.href = '/login';
}
```

## Advanced Patterns

### Expiring Saved State

```typescript
interface SavedDateWithExpiry {
  value: string;
  timestamp: number;
  expiryHours: number;
}

function saveDateWithExpiry(key: string, date: Date, expiryHours: number = 24) {
  const data: SavedDateWithExpiry = {
    value: date.toISOString(),
    timestamp: Date.now(),
    expiryHours: expiryHours
  };
  
  localStorage.setItem(key, JSON.stringify(data));
}

function getDateIfNotExpired(key: string): Date | null {
  const saved = localStorage.getItem(key);
  
  if (!saved) return null;
  
  try {
    const data: SavedDateWithExpiry = JSON.parse(saved);
    const age = Date.now() - data.timestamp;
    const expiryMs = data.expiryHours * 60 * 60 * 1000;
    
    if (age > expiryMs) {
      localStorage.removeItem(key);  // Expired
      return null;
    }
    
    return new Date(data.value);
  } catch (error) {
    return null;
  }
}

// Usage
const datePicker = new DatePicker({
  value: getDateIfNotExpired('appointmentDate') || new Date(),
  change: (args) => {
    if (args.value) {
      saveDateWithExpiry('appointmentDate', args.value, 7);  // Expire in 7 hours
    }
  }
});

datePicker.appendTo('#datepicker');
```

### Multiple Dates with Tags

```typescript
interface TaggedDate {
  date: string;
  tag: string;
  createdAt: number;
}

function saveMultipleDates(dates: Map<string, Date>) {
  const tagged: TaggedDate[] = Array.from(dates).map(([tag, date]) => ({
    date: date.toISOString(),
    tag: tag,
    createdAt: Date.now()
  }));
  
  localStorage.setItem('savedDates', JSON.stringify(tagged));
}

function loadMultipleDates(): Map<string, Date> {
  const saved = localStorage.getItem('savedDates');
  const map = new Map<string, Date>();
  
  if (!saved) return map;
  
  try {
    const tagged: TaggedDate[] = JSON.parse(saved);
    tagged.forEach(item => {
      map.set(item.tag, new Date(item.date));
    });
  } catch (error) {
    console.error('Failed to load dates:', error);
  }
  
  return map;
}

// Usage
const savedDates = loadMultipleDates();

const startDate = new DatePicker({
  value: savedDates.get('start') || new Date(),
  change: (args) => {
    savedDates.set('start', args.value);
    saveMultipleDates(savedDates);
  }
});

const endDate = new DatePicker({
  value: savedDates.get('end') || new Date(),
  change: (args) => {
    savedDates.set('end', args.value);
    saveMultipleDates(savedDates);
  }
});

startDate.appendTo('#start-date');
endDate.appendTo('#end-date');
```

### Sync Across Tabs

```typescript
// Share DatePicker state between browser tabs/windows
function setupCrosTabSync(storageKey: string, datePicker: DatePicker) {
  window.addEventListener('storage', (event) => {
    if (event.key === storageKey && event.newValue) {
      try {
        const newDate = new Date(event.newValue);
        datePicker.value = newDate;
        datePicker.dataBind();
        console.log('Updated from other tab:', newDate);
      } catch (error) {
        console.error('Invalid date from other tab:', event.newValue);
      }
    }
  });
}

const datePicker = new DatePicker({
  value: new Date(),
  change: (args) => {
    if (args.value) {
      localStorage.setItem('sharedDate', args.value.toISOString());
    }
  }
});

datePicker.appendTo('#datepicker');
setupCrosTabSync('sharedDate', datePicker);
```

## Storage Best Practices

### 1. Handle Storage Quota

```typescript
function saveWithQuotaHandling(key: string, value: string) {
  try {
    localStorage.setItem(key, value);
  } catch (e) {
    if (e instanceof DOMException && e.code === 22) {
      // QuotaExceededError: localStorage full
      console.warn('Storage quota exceeded');
      
      // Clear old data and retry
      const oldestKey = Object.keys(localStorage).reduce((oldest, key) => {
        if (key.startsWith('date_')) {
          return !oldest || 
            parseInt(localStorage.getItem(key + '_time') || '0') <
            parseInt(localStorage.getItem(oldest + '_time') || '0')
            ? key : oldest;
        }
        return oldest;
      }, '');
      
      if (oldestKey) {
        localStorage.removeItem(oldestKey);
        localStorage.removeItem(oldestKey + '_time');
      }
      
      localStorage.setItem(key, value);
      localStorage.setItem(key + '_time', Date.now().toString());
    }
  }
}
```

### 2. Validate Stored Data

```typescript
function loadDateSafely(key: string, fallback: Date = new Date()): Date {
  try {
    const saved = localStorage.getItem(key);
    if (!saved) return fallback;
    
    const date = new Date(saved);
    
    // Validate it's a valid Date
    if (isNaN(date.getTime())) {
      console.warn('Invalid date in storage:', saved);
      return fallback;
    }
    
    return date;
  } catch (error) {
    console.error('Error loading date:', error);
    return fallback;
  }
}
```

### 3. Use sessionStorage for Temporary State

```typescript
// Temporary state (per session)
const datePicker = new DatePicker({
  value: new Date(),
  change: (args) => {
    if (args.value) {
      // Temporary session storage
      sessionStorage.setItem('tempDate', args.value.toISOString());
      
      // Permanent local storage
      localStorage.setItem('lastDate', args.value.toISOString());
    }
  }
});

datePicker.appendTo('#datepicker');
```

## Troubleshooting

| Issue | Cause | Solution |
|-------|-------|----------|
| State not persisting | localStorage disabled | Check browser privacy settings |
| Invalid date on load | Corrupted storage | Validate with `isNaN()` check |
| Storage quota exceeded | Too much data | Implement quota handling or cleanup |
| Multi-tab sync not working | No storage event listener | Add `window.addEventListener('storage', ...)` |
| Date format mismatch | Inconsistent timezone | Always use `toISOString()` for storage |

## Security Considerations

### 1. Don't Store Sensitive Data

```typescript
// ✗ DON'T: Store sensitive dates in localStorage
localStorage.setItem('userPassword', password);
localStorage.setItem('creditCard', cardNumber);

// ✓ DO: Store safe, non-sensitive dates
localStorage.setItem('lastVisitDate', new Date().toISOString());
```

### 2. Sanitize Retrieved Data

```typescript
// ✓ Good: Validate before use
function getSafeDate(key: string): Date | null {
  try {
    const saved = localStorage.getItem(key);
    if (!saved) return null;
    
    const date = new Date(saved);
    if (!isValidDate(date)) return null;
    
    return date;
  } catch {
    return null;
  }
}

function isValidDate(date: Date): boolean {
  return date instanceof Date && !isNaN(date.getTime());
}
```

### 3. Clear on Logout

```typescript
function logout() {
  // Clear all stored user data
  const keysToRemove = ['appointmentDate', 'userBirthday', 'formData'];
  keysToRemove.forEach(key => localStorage.removeItem(key));
}
```
