# Drag & Drop in ListBox

## Table of Contents
- [Single ListBox](#single-listbox)
- [Multiple ListBox](#multiple-listbox)
- [Dual ListBox with Toolbar](#dual-listbox-with-toolbar)
- [Drag Events](#drag-events)
- [Common Patterns](#common-patterns)
- [Troubleshooting](#troubleshooting)

## Single ListBox

Enable drag-and-drop within a single ListBox to reorder items.

### Basic Reordering

```typescript
import { ListBox } from '@syncfusion/ej2-dropdowns';

const countryData: { [key: string]: Object }[] = [
    { "Name": "Australia", "Code": "AU" },
    { "Name": "Bermuda", "Code": "BM" },
    { "Name": "Canada", "Code": "CA" },
    { "Name": "Denmark", "Code": "DK" },
    { "Name": "France", "Code": "FR" },
    { "Name": "Germany", "Code": "DE" },
    { "Name": "Hong Kong", "Code": "HK" }
];

const listObj: ListBox = new ListBox({
    dataSource: countryData,
    fields: { text: 'Name' },
    // Enable drag-and-drop
    allowDragAndDrop: true,
    height: '250px'
});

listObj.appendTo('#listbox');
```

**Result**: Users can drag items to reorder them within the list.

### Reordering with Selection

```typescript
import { ListBox } from '@syncfusion/ej2-dropdowns';

const listObj: ListBox = new ListBox({
    dataSource: countryData,
    fields: { text: 'Name', value: 'Code' },
    allowDragAndDrop: true,
    selectionSettings: {
        mode: 'Multiple'  // Allow selecting multiple items to drag together
    },
    height: '300px'
});

listObj.appendTo('#listbox');

// Selected items move together when dragged
```

**Behavior**:
- Single item drag moves that one item
- Multiple selected items drag together
- Drop reorders items in list

## Multiple ListBox

Enable drag-and-drop between two or more ListBox instances.

### Two-List Transfer

```typescript
import { ListBox } from '@syncfusion/ej2-dropdowns';

const groupA: { [key: string]: Object }[] = [
    { "Name": "Australia", "Code": "AU" },
    { "Name": "Bermuda", "Code": "BM" },
    { "Name": "Canada", "Code": "CA" },
    { "Name": "Cameroon", "Code": "CM" }
];

const groupB: { [key: string]: Object }[] = [
    { "Name": "Denmark", "Code": "DK" },
    { "Name": "France", "Code": "FR" },
    { "Name": "Finland", "Code": "FI" },
    { "Name": "Germany", "Code": "DE" }
];

// First ListBox
const listObj1: ListBox = new ListBox({
    dataSource: groupA,
    fields: { text: 'Name' },
    allowDragAndDrop: true,
    scope: 'combined-list',  // Link to second list
    height: '280px'
});

listObj1.appendTo('#listbox1');

// Second ListBox
const listObj2: ListBox = new ListBox({
    dataSource: groupB,
    fields: { text: 'Name' },
    allowDragAndDrop: true,
    scope: 'combined-list',  // Same scope
    height: '280px'
});

listObj2.appendTo('#listbox2');
```

**Key Points**:
- Both lists must have `allowDragAndDrop: true`
- Both must have same `scope` value
- Dragging item from one list moves it to the other
- Items can be reordered within each list

### HTML Template for Multiple Lists

```html
<div style="display: flex; gap: 20px;">
    <div>
        <h3>Available Countries</h3>
        <div id="listbox1"></div>
    </div>
    <div>
        <h3>Selected Countries</h3>
        <div id="listbox2"></div>
    </div>
</div>
```

## Dual ListBox with Toolbar

Combine two ListBox instances with toolbar for controlled item movement.

### Basic Dual ListBox

```typescript
import { ListBox } from '@syncfusion/ej2-dropdowns';

const groupA: { [key: string]: Object }[] = [
    { "Name": "Australia", "Code": "AU" },
    { "Name": "Bermuda", "Code": "BM" },
    { "Name": "Canada", "Code": "CA" },
    { "Name": "Cameroon", "Code": "CM" },
    { "Name": "Denmark", "Code": "DK" }
];

const groupB: { [key: string]: Object }[] = [
    { "Name": "France", "Code": "FR" },
    { "Name": "Finland", "Code": "FI" },
    { "Name": "Germany", "Code": "DE" },
    { "Name": "Hong Kong", "Code": "HK" },
    { "Name": "India", "Code": "IN" }
];

// Source List
const listObj1: ListBox = new ListBox({
    dataSource: groupA,
    fields: { text: 'Name' },
    allowDragAndDrop: true,
    scope: '#listbox2',
    toolbarSettings: {
        items: [
            'moveUp',        // Move item up
            'moveDown',      // Move item down
            'moveTo',        // Move to other list
            'moveFrom',      // Move from other list
            'moveAllTo',     // Move all to other list
            'moveAllFrom'    // Move all from other list
        ]
    },
    height: '330px'
});

listObj1.appendTo('#listbox1');

// Destination List
const listObj2: ListBox = new ListBox({
    dataSource: groupB,
    fields: { text: 'Name' },
    allowDragAndDrop: true,
    scope: '#listbox2',
    height: '330px'
});

listObj2.appendTo('#listbox2');
```

### HTML Layout for Dual ListBox

```html
<div style="display: flex; gap: 20px; align-items: center;">
    <div>
        <h3>Available</h3>
        <div id="listbox1"></div>
        <!-- Toolbar buttons render here automatically -->
    </div>
    <div id="listbox2">
        <h3>Selected</h3>
        <!-- Toolbar between lists -->
    </div>
</div>
```

### Toolbar Items Reference

| Item | Function |
|------|----------|
| `moveUp` | Move selected item up in list |
| `moveDown` | Move selected item down in list |
| `moveTo` | Move selected items to other list |
| `moveFrom` | Move items from other list (if any selected) |
| `moveAllTo` | Move all items to other list |
| `moveAllFrom` | Move all items from other list |

## Drag Events

### dragStart Event

Triggered when drag operation begins:

```typescript
import { ListBox } from '@syncfusion/ej2-dropdowns';

const listObj: ListBox = new ListBox({
    dataSource: countryData,
    fields: { text: 'Name' },
    allowDragAndDrop: true,
    dragStart: (args: any) => {
        console.log('Drag started on:', args.draggedItem.innerText);
        // Can cancel drag here: args.cancel = true;
    }
});

listObj.appendTo('#listbox');
```

### drag Event

Triggered while dragging (multiple times):

```typescript
const listObj: ListBox = new ListBox({
    dataSource: countryData,
    fields: { text: 'Name' },
    allowDragAndDrop: true,
    drag: (args: any) => {
        console.log('Dragging...');
        // Update UI feedback or validate drop target
    }
});

listObj.appendTo('#listbox');
```

### drop Event

Triggered when item is dropped:

```typescript
const listObj: ListBox = new ListBox({
    dataSource: countryData,
    fields: { text: 'Name' },
    allowDragAndDrop: true,
    drop: (args: any) => {
        console.log('Item dropped:', args.draggedItem.innerText);
        // Persist new order to database
        saveListOrder();
    }
});

listObj.appendTo('#listbox');

function saveListOrder() {
    const order = listObj.getItems().map((item: any) => item.getAttribute('data-value'));
    console.log('New order:', order);
    // Send to server
}
```

### Complete Event Handler

```typescript
const listObj: ListBox = new ListBox({
    dataSource: countryData,
    fields: { text: 'Name' },
    allowDragAndDrop: true,
    dragStart: (args: any) => {
        console.log('Drag started:', args.draggedItem.textContent);
    },
    drag: (args: any) => {
        // Could add visual effects here
    },
    drop: (args: any) => {
        console.log('Dropped at position:', args.index);
        updateDatabase(listObj.getItems());
    }
});

listObj.appendTo('#listbox');

function updateDatabase(items: any[]) {
    const newOrder = items.map((item: any) => {
        return {
            text: item.textContent,
            index: Array.from(items).indexOf(item)
        };
    });
    console.log('Saving:', newOrder);
}
```

## Common Patterns

### Pattern 1: Reorderable To-Do List

```typescript
const tasks = [
    { id: '1', title: 'Task 1' },
    { id: '2', title: 'Task 2' },
    { id: '3', title: 'Task 3' }
];

const listObj: ListBox = new ListBox({
    dataSource: tasks,
    fields: { text: 'title', value: 'id' },
    allowDragAndDrop: true,
    drop: (args: any) => {
        const orderedTasks = listObj.getItems().map((item: any, index: number) => ({
            id: item.getAttribute('data-value'),
            position: index
        }));
        // Save priority order to server
        updateTaskOrder(orderedTasks);
    }
});

listObj.appendTo('#tasks');
```

### Pattern 2: Multi-Select Drag Transfer

```typescript
const sourceList: ListBox = new ListBox({
    dataSource: groupA,
    fields: { text: 'Name' },
    allowDragAndDrop: true,
    scope: '#targetList',
    selectionSettings: { mode: 'Multiple' }  // Multiple items drag together
});

sourceList.appendTo('#sourceList');

const targetList: ListBox = new ListBox({
    dataSource: groupB,
    fields: { text: 'Name' },
    allowDragAndDrop: true,
    scope: '#targetList'
});

targetList.appendTo('#targetList');
```

### Pattern 3: Drag with Validation

```typescript
const listObj: ListBox = new ListBox({
    dataSource: countryData,
    fields: { text: 'Name' },
    allowDragAndDrop: true,
    dragStart: (args: any) => {
        const itemName = args.draggedItem.textContent;
        
        // Only allow dragging countries starting with 'A'
        if (!itemName.startsWith('A')) {
            args.cancel = true;
            console.log('Cannot drag this item');
        }
    }
});

listObj.appendTo('#listbox');
```

## Troubleshooting

### Issue: Drag-and-drop not working between lists

**Cause**: `scope` values don't match or `allowDragAndDrop` not set on both.

**Solution**:
```typescript
// Correct - both have matching scope
const list1: ListBox = new ListBox({
    allowDragAndDrop: true,
    scope: 'shared-scope'
});

const list2: ListBox = new ListBox({
    allowDragAndDrop: true,
    scope: 'shared-scope'
});
```

### Issue: Items not staying in dropped position

**Cause**: Drop event handler not handling data properly or data not persisted.

**Solution**:
```typescript
drop: (args: any) => {
    // Ensure dataSource reflects new order
    const items = listObj.getItems();
    const newData = items.map((item: any) => ({
        text: item.textContent,
        value: item.getAttribute('data-value')
    }));
    
    // Update dataSource
    listObj.dataSource = newData;
}
```

### Issue: Toolbar buttons not appearing

**Cause**: `toolbarSettings` not configured.

**Solution**:
```typescript
toolbarSettings: {
    items: ['moveUp', 'moveDown', 'moveTo', 'moveFrom', 'moveAllTo', 'moveAllFrom']
}
```

### Issue: Can't drag selected items

**Cause**: Selection mode not set to Multiple.

**Solution**:
```typescript
selectionSettings: { mode: 'Multiple' },
allowDragAndDrop: true
```

## Next Steps

- **Selection**: See [selection-and-checkboxes.md](selection-and-checkboxes.md) for multi-select patterns
- **Sorting**: See [sorting-and-grouping.md](sorting-and-grouping.md) for organized lists
- **Accessibility**: See [accessibility-and-keyboard.md](accessibility-and-keyboard.md) for keyboard alternatives
