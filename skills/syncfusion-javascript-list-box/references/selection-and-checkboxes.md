# Selection & Checkboxes in ListBox

## Table of Contents
- [Single Selection](#single-selection)
- [Multiple Selection](#multiple-selection)
- [Checkbox Selection](#checkbox-selection)
- [Keyboard Navigation](#keyboard-navigation)
- [Change Events](#change-events)
- [Programmatic Selection](#programmatic-selection)
- [Edge Cases](#edge-cases)

## Single Selection

Allow user to select only one item at a time.

### Basic Single Selection

```typescript
import { ListBox } from '@syncfusion/ej2-dropdowns';

const data: string[] = [
    'Hennessey Venom',
    'Bugatti Chiron',
    'Bugatti Veyron Super Sport',
    'SSC Ultimate Aero',
    'Koenigsegg CCR',
    'McLaren F1'
];

const listObj: ListBox = new ListBox({
    dataSource: data,
    selectionSettings: {
        mode: 'Single'
    }
});

listObj.appendTo('#listbox');
```

**Behavior**: 
- Clicking an item selects it and deselects previous selection
- Only one item highlighted at a time
- Previous selection is cleared automatically

### Single Selection with Objects

```typescript
interface Car {
    id: string;
    name: string;
    price: number;
}

const carData: Car[] = [
    { id: 'car1', name: 'Hennessey Venom', price: 1500000 },
    { id: 'car2', name: 'Bugatti Chiron', price: 2600000 },
    { id: 'car3', name: 'McLaren F1', price: 12750000 }
];

const listObj: ListBox = new ListBox({
    dataSource: carData,
    fields: { text: 'name', value: 'id' },
    selectionSettings: {
        mode: 'Single'
    }
});

listObj.appendTo('#listbox');

// Get selected item
const selected = listObj.getSelectedItems();
console.log('Selected:', selected);  // { text: 'Hennessey Venom', value: 'car1' }
```

## Multiple Selection

Allow user to select multiple items simultaneously.

### Basic Multiple Selection

```typescript
import { ListBox } from '@syncfusion/ej2-dropdowns';

const data: string[] = [
    'Hennessey Venom',
    'Bugatti Chiron',
    'Bugatti Veyron Super Sport',
    'SSC Ultimate Aero',
    'Koenigsegg CCR',
    'McLaren F1',
    'Aston Martin One-77',
    'Jaguar XJ220'
];

const listObj: ListBox = new ListBox({
    dataSource: data,
    selectionSettings: {
        mode: 'Multiple'
    }
});

listObj.appendTo('#listbox');
```

**Behavior**:
- Multiple items can be highlighted simultaneously
- Use SHIFT for range selection
- Use CTRL for individual item selection (toggle)
- Default mode if selectionSettings not specified

### Multiple Selection with Shift/Ctrl

```typescript
// User interaction:
// Click item 1: Select item 1
// Shift + Click item 5: Select items 1-5 (range)
// Ctrl + Click item 7: Toggle item 7 (add to selection)
// Ctrl + Click item 3: Toggle item 3 (remove from selection)

const listObj: ListBox = new ListBox({
    dataSource: carData,
    fields: { text: 'name', value: 'id' },
    selectionSettings: {
        mode: 'Multiple'
    }
});

listObj.appendTo('#listbox');

// Get all selected items
const selected = listObj.getSelectedItems();
console.log('Selected count:', selected.text.length);
console.log('Selected values:', selected.value);
```

## Checkbox Selection

Enable checkboxes for explicit multi-select indication.

### Basic Checkbox Mode

```typescript
import { ListBox, CheckBoxSelection } from '@syncfusion/ej2-dropdowns';

// Inject CheckBoxSelection module
ListBox.Inject(CheckBoxSelection);

const data: string[] = [
    'Hennessey Venom',
    'Bugatti Chiron',
    'Bugatti Veyron Super Sport',
    'SSC Ultimate Aero',
    'Koenigsegg CCR',
    'McLaren F1'
];

const listObj: ListBox = new ListBox({
    dataSource: data,
    selectionSettings: {
        mode: 'Multiple',
        showCheckbox: true
    }
});

listObj.appendTo('#listbox');
```

**Features**:
- Checkbox appears next to each item
- Check/uncheck to select/deselect
- Space key toggles checkbox for focused item
- Visual feedback for checked state

### Checkbox with Select All

```typescript
import { ListBox, CheckBoxSelection } from '@syncfusion/ej2-dropdowns';

ListBox.Inject(CheckBoxSelection);

const listObj: ListBox = new ListBox({
    dataSource: data,
    selectionSettings: {
        mode: 'Multiple',
        showCheckbox: true,
        showSelectAll: true
    }
});

listObj.appendTo('#listbox');
```

**Result**:
- "Select All" checkbox appears at top
- Checking it selects all items
- Unchecking deselects all items
- Individual items can be toggled independently

### Checkbox Selection with Objects

```typescript
import { ListBox, CheckBoxSelection } from '@syncfusion/ej2-dropdowns';

ListBox.Inject(CheckBoxSelection);

const employeeData = [
    { id: 'E001', name: 'John Smith', dept: 'Engineering' },
    { id: 'E002', name: 'Sarah Jones', dept: 'Marketing' },
    { id: 'E003', name: 'Mike Brown', dept: 'Sales' },
    { id: 'E004', name: 'Lisa Anderson', dept: 'HR' }
];

const listObj: ListBox = new ListBox({
    dataSource: employeeData,
    fields: { text: 'name', value: 'id' },
    selectionSettings: {
        mode: 'Multiple',
        showCheckbox: true,
        showSelectAll: true
    }
});

listObj.appendTo('#listbox');

// Get checked items
const selected = listObj.getSelectedItems();
console.log('Selected employees:', selected.text);
console.log('Selected IDs:', selected.value);
```

## Keyboard Navigation

### Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| **↑ Up Arrow** | Move focus to previous item |
| **↓ Down Arrow** | Move focus to next item |
| **Home** | Move focus to first item |
| **End** | Move focus to last item |
| **Space** | Toggle selection of focused item (checkbox mode) |
| **Ctrl + A** | Select all items |
| **Ctrl + Shift + Home** | Select from current to first item |
| **Ctrl + Shift + End** | Select from current to last item |
| **Ctrl + ↑ or Ctrl + ↓** | Select/deselect with multi-select |

### Keyboard Selection Example

```typescript
const listObj: ListBox = new ListBox({
    dataSource: data,
    selectionSettings: {
        mode: 'Multiple',
        showCheckbox: true
    }
});

listObj.appendTo('#listbox');

// User can:
// 1. Tab to focus ListBox
// 2. Use arrow keys to navigate
// 3. Press Space to toggle checkbox
// 4. Use Ctrl+A to select all
// 5. Use Shift+Down to extend selection
```

### Focusing Specific Item

```typescript
const listObj: ListBox = new ListBox({
    dataSource: data,
    selectionSettings: { mode: 'Multiple' }
});

listObj.appendTo('#listbox');

// Move focus programmatically
listObj.focusIn();  // Focus ListBox
listObj.focusNext();  // Focus next item
```

## Change Events

### Handling Selection Changes

```typescript
import { ListBox } from '@syncfusion/ej2-dropdowns';

const listObj: ListBox = new ListBox({
    dataSource: data,
    selectionSettings: { mode: 'Multiple' },
    change: (args: any) => {
        console.log('Selection changed');
        console.log('Selected text:', args.text);
        console.log('Selected value:', args.value);
        console.log('Is single select:', args.isInteracted);
    }
});

listObj.appendTo('#listbox');
```

### Accessing Change Event

```typescript
const listObj: ListBox = new ListBox({
    dataSource: data,
    selectionSettings: { mode: 'Multiple' }
});

listObj.appendTo('#listbox');

// Attach event handler after creation
listObj.change = (args: any) => {
    const selected = listObj.getSelectedItems();
    console.log('Total selected:', selected.text.length);
    
    // Update UI or trigger actions
    updateSummary(selected);
};

function updateSummary(selected: any) {
    const summary = document.getElementById('summary');
    summary.textContent = `Selected ${selected.text.length} items`;
}
```

### Change Event with Filter

```typescript
const listObj: ListBox = new ListBox({
    dataSource: data,
    selectionSettings: { mode: 'Multiple' },
    change: (args: any) => {
        // Only react to user interaction, not programmatic changes
        if (args.isInteracted) {
            console.log('User selected:', args.text);
        }
    }
});

listObj.appendTo('#listbox');
```

## Programmatic Selection

### Select Items by Index

```typescript
const listObj: ListBox = new ListBox({
    dataSource: data,
    selectionSettings: { mode: 'Multiple' }
});

listObj.appendTo('#listbox');

// Select single item by index
listObj.selectItemByIndex(0);  // Select first item

// Select multiple by indices
listObj.selectItemByIndex([0, 2, 4]);  // Select items 1, 3, 5
```

### Select All Items

```typescript
const listObj: ListBox = new ListBox({
    dataSource: data,
    selectionSettings: { mode: 'Multiple' }
});

listObj.appendTo('#listbox');

// Select all
listObj.selectAll();

// Get all selected
const selected = listObj.getSelectedItems();
console.log('Total:', selected.text.length);
```

### Select by Value

```typescript
const carData = [
    { id: 'car1', name: 'Hennessey Venom' },
    { id: 'car2', name: 'Bugatti Chiron' },
    { id: 'car3', name: 'McLaren F1' }
];

const listObj: ListBox = new ListBox({
    dataSource: carData,
    fields: { text: 'name', value: 'id' },
    selectionSettings: { mode: 'Multiple' }
});

listObj.appendTo('#listbox');

// Select by value
listObj.value = ['car1', 'car3'];  // Select specific cars

// Get selected values
const selected = listObj.getSelectedItems();
console.log('Selected IDs:', selected.value);
```

### Clear Selection

```typescript
const listObj: ListBox = new ListBox({
    dataSource: data,
    selectionSettings: { mode: 'Multiple' }
});

listObj.appendTo('#listbox');

// Clear all selections
listObj.clearSelection();

// Verify cleared
const selected = listObj.getSelectedItems();
console.log('Selected count:', selected.text.length);  // 0
```

## Edge Cases

### Issue: Selected Items Undefined

**Cause**: No items actually selected when called.

**Solution**: Check selection before accessing:
```typescript
const selected = listObj.getSelectedItems();
if (selected && selected.text && selected.text.length > 0) {
    console.log('Selected:', selected.text);
} else {
    console.log('No items selected');
}
```

### Issue: Checkbox Not Toggling with Space

**Cause**: Focus not on ListBox or CheckBoxSelection module not injected.

**Solution**: 
```typescript
import { ListBox, CheckBoxSelection } from '@syncfusion/ej2-dropdowns';
ListBox.Inject(CheckBoxSelection);  // Required

const listObj: ListBox = new ListBox({
    dataSource: data,
    selectionSettings: {
        mode: 'Multiple',
        showCheckbox: true
    }
});

listObj.appendTo('#listbox');
listObj.focusIn();  // Ensure focus
```

### Issue: Shift+Click Not Working

**Cause**: Not in multiple selection mode.

**Solution**:
```typescript
// Wrong - single mode doesn't support range selection
selectionSettings: { mode: 'Single' }

// Correct
selectionSettings: { mode: 'Multiple' }
```

### Issue: Select All Checkbox Not Appearing

**Cause**: `showSelectAll` not set or `mode` not 'Multiple'.

**Solution**:
```typescript
import { ListBox, CheckBoxSelection } from '@syncfusion/ej2-dropdowns';
ListBox.Inject(CheckBoxSelection);

selectionSettings: {
    mode: 'Multiple',           // Required
    showCheckbox: true,         // Required
    showSelectAll: true         // Enable Select All checkbox
}
```

## Next Steps

- **Data Binding**: See [data-binding.md](data-binding.md) for complex data structures
- **Drag-and-Drop**: See [drag-and-drop.md](drag-and-drop.md) to reorder selections
- **Accessibility**: See [accessibility-and-keyboard.md](accessibility-and-keyboard.md) for WCAG compliance details
