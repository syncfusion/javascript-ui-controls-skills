# Advanced Features in ComboBox

## Table of Contents
- [Virtual Scrolling](#virtual-scrolling)
- [Custom Values](#custom-values)
- [Disabled Items](#disabled-items)
- [Read-Only Mode](#read-only-mode)
- [Popup Positioning](#popup-positioning)
- [Event Handling](#event-handling)
- [RTL Support](#rtl-support)
- [Keyboard Navigation](#keyboard-navigation)
- [Accessibility & WCAG](#accessibility--wcag)

## Virtual Scrolling

### Handling Large Datasets

Virtual scrolling renders only visible items for performance with 10,000+ items:

```typescript
import { ComboBox } from '@syncfusion/ej2-dropdowns';

// Create 10,000 items
const largeDataset: { [key: string]: Object }[] = Array.from({ length: 10000 }, (_, i) => ({
    id: i + 1,
    name: `Item ${i + 1}`,
    category: `Category ${(i % 10) + 1}`
}));

const comboBox: ComboBox = new ComboBox({
    dataSource: largeDataset,
    fields: { text: 'name', value: 'id', groupBy: 'category' },
    
    // Enable virtual scrolling
    enableVirtualization: true,
    
    // Height of dropdown container (important for virtual scroll)
    popupHeight: '300px',
    
    placeholder: "Select from 10,000 items..."
});

comboBox.appendTo('#comboelement');

// Result: Smooth scrolling even with 10,000 items
// Only ~20 items rendered at a time based on scroll position
```

**Performance improvement:**
- Without virtual scroll: 10,000 DOM elements = slow
- With virtual scroll: ~20 DOM elements = fast

---

### Virtual Scroll Configuration

```typescript
const comboBox: ComboBox = new ComboBox({
    dataSource: largeDataset,
    fields: { text: 'name', value: 'id' },
    
    enableVirtualization: true,
    
    // Item height (in pixels) for calculation
    itemHeight: 35,      // Height of each item
    
    popupHeight: '300px', // Visible height of dropdown
    
    placeholder: "Select..."
});

comboBox.appendTo('#comboelement');

// Virtual scroll calculates: 300px / 35px = ~8 items visible
// Plus buffers above/below for smooth scrolling
```

**When to use:**
- ✅ 1,000+ items
- ✅ Remote data with pagination
- ✅ Dynamic/loaded data

**When NOT needed:**
- ❌ <100 items
- ❌ Small lists with templates

---

## Custom Values

### Allow User-Entered Values

Let users enter values not in the predefined list:

```typescript
import { ComboBox } from '@syncfusion/ej2-dropdowns';

const predefinedTags = ['JavaScript', 'TypeScript', 'Python', 'Java'];

const comboBox: ComboBox = new ComboBox({
    dataSource: predefinedTags,
    
    allowCustom: true,  // Allow typing custom values
    
    placeholder: "Enter or select tag",
    
    change: (args: any) => {
        console.log('Selected/Entered value:', args.value);
        // "JavaScript" (from list) or "Go" (custom typed)
    }
});

comboBox.appendTo('#comboelement');

// User scenarios:
// 1. Clicks dropdown → selects "JavaScript"
// 2. Types "Go" (not in list) → "Go" becomes selected value
```

---

### Filtering with Custom Values

Combine filtering with custom value entry:

```typescript
const comboBox: ComboBox = new ComboBox({
    dataSource: ['Apple', 'Apricot', 'Avocado', 'Banana', 'Blueberry'],
    
    allowCustom: true,
    allowFiltering: true,
    
    placeholder: "Search or type a fruit..."
});

comboBox.appendTo('#comboelement');

// User types "app" → sees "Apple", "Apricot", "Avocado"
// User types "grape" (not in list) → "grape" becomes custom value
// User types "Xylophone" (not fruit) → still accepts as custom value
```

**Use case:** Tagging systems, open-ended inputs, or "other" options.

---

## Disabled Items

### Disable Entire ComboBox

```typescript
const comboBox: ComboBox = new ComboBox({
    dataSource: ['Option A', 'Option B', 'Option C'],
    enabled: false,  // Disabled state
    placeholder: "Disabled"
});

comboBox.appendTo('#comboelement');

// Result: ComboBox appears grayed out, cannot interact
```

---

### Toggle Enabled State

```typescript
const comboBox: ComboBox = new ComboBox({
    dataSource: ['Option A', 'Option B', 'Option C'],
    placeholder: "Select option"
});

comboBox.appendTo('#comboelement');

// Enable/disable based on condition
function toggleComboBox(shouldDisable: boolean) {
    comboBox.enabled = !shouldDisable;
}

// Disable ComboBox
toggleComboBox(true);

// Enable ComboBox later
toggleComboBox(false);
```

---

### Disable Specific Items

Some libraries don't support item-level disable, but you can workaround:

```typescript
const itemData: { [key: string]: Object }[] = [
    { id: 1, name: 'Active Item', disabled: false },
    { id: 2, name: 'Inactive Item', disabled: true },
    { id: 3, name: 'Active Item 2', disabled: false }
];

const comboBox: ComboBox = new ComboBox({
    dataSource: itemData,
    fields: { text: 'name', value: 'id' },
    
    // Filter out disabled items
    dataSource: itemData.filter(item => !item.disabled),
    
    placeholder: "Select active item"
});

comboBox.appendTo('#comboelement');
```

---

## Read-Only Mode

### Display-Only ComboBox

Show value but prevent changes:

```typescript
const comboBox: ComboBox = new ComboBox({
    dataSource: ['Option A', 'Option B', 'Option C'],
    
    readonly: true,  // Cannot modify value
    value: 'Option A',  // Pre-set value
    
    placeholder: "Read-only"
});

comboBox.appendTo('#comboelement');

// User sees "Option A" but cannot open dropdown or change it
```

**Use cases:**
- Display-only forms
- Locked selections
- Status indicators

---

### Toggle Read-Only

```typescript
const comboBox: ComboBox = new ComboBox({
    dataSource: ['Option A', 'Option B', 'Option C'],
    placeholder: "Select..."
});

comboBox.appendTo('#comboelement');

// Lock selection
function lockSelection() {
    comboBox.readonly = true;
}

// Allow editing again
function unlockSelection() {
    comboBox.readonly = false;
}
```

---

## Popup Positioning

### Configure Popup Height and Width

```typescript
const comboBox: ComboBox = new ComboBox({
    dataSource: ['Item A', 'Item B', 'Item C', 'Item D', 'Item E'],
    
    popupHeight: '250px',  // Height of dropdown list
    popupWidth: '400px',   // Width of dropdown list
    
    placeholder: "Select..."
});

comboBox.appendTo('#comboelement');

// Default: height 300px, width auto (matches input width)
```

---

### Auto-Adjust Popup

```typescript
const comboBox: ComboBox = new ComboBox({
    dataSource: largeDataset,  // Many items
    
    popupHeight: 'auto',  // Auto-calculate height based on items
    // or specific height
    popupHeight: '300px',
    
    placeholder: "..."
});

comboBox.appendTo('#comboelement');
```

---

### Resizable Popup

Allow users to resize popup:

```typescript
const comboBox: ComboBox = new ComboBox({
    dataSource: ['Item A', 'Item B', 'Item C'],
    
    allowResize: true,  // Enable resize handle
    popupHeight: '200px',
    
    placeholder: "Select (resizable)..."
});

comboBox.appendTo('#comboelement');

// User can drag bottom-right corner to resize popup
```

---

## Event Handling

### Select Event

Fires when user selects an item:

```typescript
import { ComboBox, SelectEventArgs } from '@syncfusion/ej2-dropdowns';

const comboBox: ComboBox = new ComboBox({
    dataSource: ['JavaScript', 'TypeScript', 'Python'],
    
    select: (args: SelectEventArgs) => {
        console.log('Item selected:', args.item);      // { text: 'JavaScript', value: 'JavaScript' }
        console.log('Selected value:', args.value);    // 'JavaScript'
        console.log('Item index:', args.index);        // 0
    },
    
    placeholder: "Select language"
});

comboBox.appendTo('#comboelement');
```

---

### Change Event

Fires when value changes (selection or typing):

```typescript
const comboBox: ComboBox = new ComboBox({
    dataSource: ['Item A', 'Item B'],
    
    change: (args: any) => {
        console.log('Previous value:', args.oldValue);  // Old selected value
        console.log('New value:', args.value);         // New selected value
        console.log('Prevented:', args.isPreventDefault); // Can cancel change
    },
    
    placeholder: "..."
});

comboBox.appendTo('#comboelement');

// Fires on: selection, typing, programmatic value change
```

---

### Filtering Event

Fires when user types to filter:

```typescript
import { ComboBox, FilteringEventArgs } from '@syncfusion/ej2-dropdowns';

const comboBox: ComboBox = new ComboBox({
    dataSource: ['Apple', 'Apricot', 'Avocado', 'Banana'],
    allowFiltering: true,
    
    filtering: (args: FilteringEventArgs) => {
        console.log('Search text:', args.text);  // User typed text
        // Custom filter logic
        if (args.text === '') {
            args.updateData(['Apple', 'Apricot', 'Avocado', 'Banana']);
        } else {
            const filtered = ['Apple', 'Apricot', 'Avocado', 'Banana']
                .filter(item => item.toLowerCase().startsWith(args.text.toLowerCase()));
            args.updateData(filtered);
        }
    },
    
    placeholder: "Search..."
});

comboBox.appendTo('#comboelement');
```

---

### Blur and Focus Events

```typescript
const comboBox: ComboBox = new ComboBox({
    dataSource: ['Item A', 'Item B'],
    
    focus: () => {
        console.log('ComboBox focused');
    },
    
    blur: () => {
        console.log('ComboBox lost focus');
    },
    
    placeholder: "..."
});

comboBox.appendTo('#comboelement');
```

---

### Action Failure Event

Fires when remote data fetch fails:

```typescript
const comboBox: ComboBox = new ComboBox({
    dataSource: new DataManager({
        url: 'https://invalid-api.com/data',  // Bad URL
        adaptor: new ODataV4Adaptor
    }),
    
    actionFailure: (args: any) => {
        console.log('Error loading data:', args.error);
        // Show user-friendly error message
    },
    
    placeholder: "..."
});

comboBox.appendTo('#comboelement');
```

---

## RTL Support

### Right-to-Left Layout

For Arabic, Hebrew, and other RTL languages:

```typescript
const comboBox: ComboBox = new ComboBox({
    dataSource: ['عربي', 'العربية', 'لغة عربية'],
    
    enableRtl: true,  // Enable RTL layout
    
    placeholder: "اختر..."  // RTL placeholder text
});

comboBox.appendTo('#comboelement');

// Result: Layout flipped horizontally
// - Input field on right
// - Dropdown appears on right
// - Text right-aligned
```

---

### RTL HTML Setup

```html
<!DOCTYPE html>
<html dir="rtl">  <!-- Set dir="rtl" for entire page -->
<head>
    <meta charset="utf-8" />
    <link rel="stylesheet" href="node_modules/@syncfusion/ej2-base/styles/material.css" />
    <link rel="stylesheet" href="node_modules/@syncfusion/ej2-dropdowns/styles/material.css" />
</head>
<body>
    <input type="text" id="comboelement" />
</body>
</html>
```

**TypeScript:**
```typescript
const comboBox: ComboBox = new ComboBox({
    dataSource: arabicData,
    enableRtl: true  // Syncfusion automatically respects HTML dir="rtl"
});

comboBox.appendTo('#comboelement');
```

---

## Keyboard Navigation

### Keyboard Shortcuts

| Key | Action |
|-----|--------|
| <kbd>↓ Down Arrow</kbd> | Move to next item / Open popup |
| <kbd>↑ Up Arrow</kbd> | Move to previous item |
| <kbd>Home</kbd> | Move cursor to start of text |
| <kbd>End</kbd> | Move cursor to end of text |
| <kbd>Enter</kbd> | Select focused item |
| <kbd>Escape</kbd> | Close popup |
| <kbd>Tab</kbd> | Move to next element (close popup) |
| <kbd>Shift+Tab</kbd> | Move to previous element |
| <kbd>Alt+↓</kbd> | Open popup |
| <kbd>Alt+↑</kbd> | Close popup |
| <kbd>Page Down</kbd> | Scroll down (when open) |
| <kbd>Page Up</kbd> | Scroll up (when open) |

**These work automatically—no configuration needed.**

---

### Custom Keyboard Handling

```typescript
import { ComboBox } from '@syncfusion/ej2-dropdowns';

const comboBox: ComboBox = new ComboBox({
    dataSource: ['Item A', 'Item B', 'Item C'],
    placeholder: "..."
});

comboBox.appendTo('#comboelement');

// Listen to keyboard events
document.addEventListener('keydown', (e: KeyboardEvent) => {
    if (e.altKey && e.keyCode === 67) {  // Alt+C
        comboBox.focus();
    }
});
```

---

## Accessibility & WCAG

### WCAG 2.2 Compliance

The ComboBox meets WCAG 2.2 Level AA standards:

✅ **ARIA Attributes:**
- `role="combobox"` - Identifies component
- `aria-expanded="true/false"` - Popup open/close state
- `aria-selected="true/false"` - Selected item
- `aria-disabled="true/false"` - Disabled state
- `aria-readonly="true/false"` - Read-only state
- `aria-owns` - Relationship between input and popup
- `aria-autocomplete="both"` - Filtering capability

---

### Screen Reader Support

ComboBox announces:
- Component type and value
- Popup open/close
- Selected item
- Filtering results count
- Keyboard shortcuts

```typescript
const comboBox: ComboBox = new ComboBox({
    dataSource: ['Option A', 'Option B'],
    
    // Provide descriptive labels
    placeholder: "Select an option",
    
    // Assistive text
    tooltipText: "Use arrow keys to navigate"
});

comboBox.appendTo('#comboelement');

// Screen reader announces:
// "ComboBox, select an option, collapsed"
// Then when opened: "expanded, 2 options available"
```

---

### Focus Management

Visual focus indicator for keyboard navigation:

```typescript
// In CSS
#comboelement:focus {
    outline: 2px solid #0066cc;
    outline-offset: 2px;
}
```

```typescript
// TypeScript
const comboBox: ComboBox = new ComboBox({
    dataSource: ['Item A', 'Item B'],
    
    focus: () => {
        // Input focused
        console.log('Focus entered');
    },
    
    blur: () => {
        // Input blurred
        console.log('Focus left');
    }
});

comboBox.appendTo('#comboelement');
```

---

### Color Contrast

Syncfusion themes meet WCAG AA color contrast:
- Text vs background: 4.5:1 ratio
- Component vs background: 3:1 ratio

Use official themes (Material, Bootstrap, Fabric) for compliance.

---

### Testing Accessibility

Use accessibility validators:
- [Axe DevTools](https://www.deque.com/axe/devtools/)
- [WAVE Browser Extension](https://wave.webaim.org/extension/)
- [Lighthouse (in Chrome DevTools)](https://developer.chrome.com/docs/lighthouse/)

---

## Common Patterns

### Pattern: Search with Recent Selections

```typescript
import { ComboBox } from '@syncfusion/ej2-dropdowns';

let recentSelections: string[] = [];
const allOptions = ['Option A', 'Option B', 'Option C', 'Option D'];

const comboBox: ComboBox = new ComboBox({
    dataSource: allOptions,
    allowFiltering: true,
    
    select: (args: any) => {
        // Add to recent
        if (!recentSelections.includes(args.value)) {
            recentSelections.unshift(args.value);
            if (recentSelections.length > 5) recentSelections.pop();
        }
        
        // Update dataSource to show recent first
        const combined = [...new Set([...recentSelections, ...allOptions])];
        comboBox.dataSource = combined;
        comboBox.refresh();
    },
    
    placeholder: "Search or select recent..."
});

comboBox.appendTo('#comboelement');
```

---

### Pattern: Autofill Search

```typescript
const comboBox: ComboBox = new ComboBox({
    dataSource: ['JavaScript', 'TypeScript', 'Java', 'Python'],
    allowFiltering: true,
    autofill: true,  // Auto-complete typed text
    placeholder: "Type to auto-complete..."
});

comboBox.appendTo('#comboelement');

// User types "ja" → auto-fills "JavaScript"
// User can accept with Tab/Enter or continue typing
```

---

## Next Steps

- ✅ **Done:** Advanced features covered
- 📖 **Review:** [SKILL.md](../SKILL.md) - Overview and quick start
- 🔗 **Reference:** Check other reference files as needed

---

## See Also

- [API Reference](https://ej2.syncfusion.com/documentation/api/combo-box/)
- [WCAG Guidelines](https://www.w3.org/WAI/WCAG21/quickref/)
- [Accessibility Best Practices](https://www.a11y-101.com/)
