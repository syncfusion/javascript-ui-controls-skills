# Dual ListBox Configuration

## Table of Contents
- [Overview](#overview)
- [Basic Setup](#basic-setup)
- [Toolbar Items](#toolbar-items)
- [Move Operations](#move-operations)
- [Scope Binding](#scope-binding)
- [Common Use Cases](#common-use-cases)
- [Troubleshooting](#troubleshooting)

## Overview

A dual ListBox displays two lists side-by-side, allowing users to transfer items between them. The left list typically shows available items, and the right list shows selected items. A toolbar between them provides buttons for moving items.

### Key Features

- **Bidirectional Transfer**: Move items left or right
- **Reordering**: Move items up/down within lists
- **Bulk Operations**: Move all items at once
- **Toolbar Buttons**: Visual controls for all operations
- **Keyboard Support**: Alternative to mouse operations

## Basic Setup

### Two-List Configuration

```typescript
import { ListBox } from '@syncfusion/ej2-dropdowns';

// Available items
const availableCountries: { [key: string]: Object }[] = [
    { "Name": "Australia", "Code": "AU" },
    { "Name": "Bermuda", "Code": "BM" },
    { "Name": "Canada", "Code": "CA" },
    { "Name": "Cameroon", "Code": "CM" },
    { "Name": "Denmark", "Code": "DK" },
    { "Name": "France", "Code": "FR" },
    { "Name": "Finland", "Code": "FI" },
    { "Name": "Germany", "Code": "DE" }
];

// Selected items (initially empty)
const selectedCountries: { [key: string]: Object }[] = [
    { "Name": "India", "Code": "IN" },
    { "Name": "Italy", "Code": "IT" },
    { "Name": "Japan", "Code": "JP" }
];

// Left List - Available
const sourceList: ListBox = new ListBox({
    dataSource: availableCountries,
    fields: { text: 'Name' },
    allowDragAndDrop: true,
    scope: '#destList',
    toolbarSettings: {
        items: [
            'moveUp',
            'moveDown',
            'moveTo',
            'moveFrom',
            'moveAllTo',
            'moveAllFrom'
        ]
    },
    height: '330px'
});

sourceList.appendTo('#sourceList');

// Right List - Selected
const destList: ListBox = new ListBox({
    dataSource: selectedCountries,
    fields: { text: 'Name' },
    allowDragAndDrop: true,
    scope: '#destList',
    height: '330px'
});

destList.appendTo('#destList');
```

### HTML Layout

```html
<div class="dual-listbox-container">
    <div class="list-section">
        <h3>Available Countries</h3>
        <div id="sourceList"></div>
    </div>
    
    <div class="toolbar-section">
        <!-- Toolbar buttons render here -->
    </div>
    
    <div class="list-section">
        <h3>Selected Countries</h3>
        <div id="destList"></div>
    </div>
</div>
```

### CSS Styling

```css
.dual-listbox-container {
    display: flex;
    gap: 20px;
    align-items: flex-start;
}

.list-section {
    flex: 1;
}

.list-section h3 {
    margin-bottom: 10px;
    font-size: 16px;
}

.toolbar-section {
    display: flex;
    flex-direction: column;
    gap: 5px;
    padding-top: 30px;
}

/* Toolbar buttons style */
.e-toolbar-item {
    padding: 8px 12px;
    background-color: #0078d4;
    color: white;
    border-radius: 4px;
    cursor: pointer;
}

.e-toolbar-item:hover {
    background-color: #005a9e;
}
```

## Toolbar Items

### Available Toolbar Items

| Item | Icon | Function | Use Case |
|------|------|----------|----------|
| `moveUp` | ↑ | Move selected item up | Reorder within list |
| `moveDown` | ↓ | Move selected item down | Reorder within list |
| `moveTo` | → | Move to other list | Transfer single item |
| `moveFrom` | ← | Move from other list | Transfer single item |
| `moveAllTo` | ⇒ | Move all to other list | Bulk transfer right |
| `moveAllFrom` | ⇐ | Move all from other list | Bulk transfer left |

### Toolbar Configuration

```typescript
// Configure toolbar with specific items
toolbarSettings: {
    items: [
        'moveUp',      // Move up in current list
        'moveDown',    // Move down in current list
        'moveTo',      // Transfer to other list
        'moveFrom',    // Transfer from other list
        'moveAllTo',   // Transfer all to other list
        'moveAllFrom'  // Transfer all from other list
    ]
}

// Or use subset of items
toolbarSettings: {
    items: ['moveTo', 'moveFrom']  // Only transfer buttons
}
```

## Move Operations

### Move Single Item

```typescript
// User selects one item and clicks "Move To"
// Item moves to other list and selection clears

// Programmatically move selected item to other list
const sourceList: ListBox = new ListBox({
    dataSource: availableCountries,
    fields: { text: 'Name', value: 'Code' },
    scope: '#destList'
});

sourceList.appendTo('#sourceList');

// Get selected item and transfer
const selected = sourceList.getSelectedItems();
if (selected.text && selected.text.length > 0) {
    // Item will be moved by toolbar button
    console.log('Moving:', selected.text[0]);
}
```

### Move Multiple Items

```typescript
const sourceList: ListBox = new ListBox({
    dataSource: availableCountries,
    fields: { text: 'Name', value: 'Code' },
    selectionSettings: { mode: 'Multiple' },
    scope: '#destList',
    toolbarSettings: {
        items: ['moveTo', 'moveFrom', 'moveAllTo', 'moveAllFrom']
    }
});

sourceList.appendTo('#sourceList');

// User selects multiple items (Ctrl+Click)
// Clicking toolbar button moves all selected items
```

### Move All Items

```typescript
toolbarSettings: {
    items: [
        'moveAllTo',    // Transfers all items to destination
        'moveAllFrom'   // Transfers all items to source
    ]
}

// When user clicks "Move All To"
// - All items from source list move to destination
// - Source becomes empty
// - Destination contains all items
```

### Reorder Within List

```typescript
const sourceList: ListBox = new ListBox({
    dataSource: availableCountries,
    fields: { text: 'Name', value: 'Code' },
    allowDragAndDrop: true,
    toolbarSettings: {
        items: [
            'moveUp',    // Move selected up in list
            'moveDown'   // Move selected down in list
        ]
    }
});

sourceList.appendTo('#sourceList');

// User selects item and clicks "Move Up" or "Move Down"
// Item reorders within same list
```

## Scope Binding

### Linking Lists

The `scope` property links two ListBox instances for drag-and-drop and toolbar operations.

```typescript
// Both lists must use same scope value
const sourceList: ListBox = new ListBox({
    scope: '#destList',  // Reference to destination list ID
    allowDragAndDrop: true
});

const destList: ListBox = new ListBox({
    scope: '#destList'   // Must match source
});
```

### Scope Value Format

```typescript
// Using element ID (recommended)
scope: '#destinationListId'

// Or using string identifier (must match in both)
scope: 'shared-scope-name'

// Complete example
const source: ListBox = new ListBox({
    scope: '#targetList',
    allowDragAndDrop: true
});

const target: ListBox = new ListBox({
    scope: '#targetList'
});
```

## Common Use Cases

### Use Case 1: Permission Selection

Select permissions to assign to a user:

```typescript
const availablePermissions = [
    { id: 'read', name: 'Read' },
    { id: 'write', name: 'Write' },
    { id: 'delete', name: 'Delete' },
    { id: 'admin', name: 'Admin' }
];

const currentPermissions = [
    { id: 'read', name: 'Read' }
];

const source: ListBox = new ListBox({
    dataSource: availablePermissions,
    fields: { text: 'name', value: 'id' },
    scope: '#grantedPermissions',
    toolbarSettings: {
        items: ['moveTo', 'moveFrom', 'moveAllTo', 'moveAllFrom']
    },
    height: '300px'
});

source.appendTo('#availablePermissions');

const dest: ListBox = new ListBox({
    dataSource: currentPermissions,
    fields: { text: 'name', value: 'id' },
    scope: '#grantedPermissions',
    height: '300px'
});

dest.appendTo('#grantedPermissions');
```

### Use Case 2: Shopping Cart

Add/remove items from cart:

```typescript
const catalog = [
    { id: '1', name: 'Laptop', price: '$999' },
    { id: '2', name: 'Mouse', price: '$25' },
    { id: '3', name: 'Keyboard', price: '$80' }
];

const cart: any[] = [];

const catalogList: ListBox = new ListBox({
    dataSource: catalog,
    fields: { text: 'name', value: 'id' },
    scope: '#shoppingCart',
    toolbarSettings: { items: ['moveTo'] },
    selectionSettings: { mode: 'Multiple' }
});

catalogList.appendTo('#catalog');

const cartList: ListBox = new ListBox({
    dataSource: cart,
    fields: { text: 'name', value: 'id' },
    scope: '#shoppingCart',
    toolbarSettings: { items: ['moveFrom'] }
});

cartList.appendTo('#shoppingCart');
```

### Use Case 3: Skill Selector

Select skills for a job posting:

```typescript
const availableSkills = [
    { skillId: 'js', name: 'JavaScript' },
    { skillId: 'ts', name: 'TypeScript' },
    { skillId: 'react', name: 'React' },
    { skillId: 'node', name: 'Node.js' },
    { skillId: 'python', name: 'Python' }
];

const requiredSkills = [
    { skillId: 'js', name: 'JavaScript' },
    { skillId: 'react', name: 'React' }
];

const source: ListBox = new ListBox({
    dataSource: availableSkills,
    fields: { text: 'name', value: 'skillId' },
    scope: '#requiredSkills',
    toolbarSettings: {
        items: ['moveUp', 'moveDown', 'moveTo', 'moveFrom', 'moveAllTo']
    },
    selectionSettings: { mode: 'Multiple' }
});

source.appendTo('#availableSkills');

const dest: ListBox = new ListBox({
    dataSource: requiredSkills,
    fields: { text: 'name', value: 'skillId' },
    scope: '#requiredSkills',
    toolbarSettings: {
        items: ['moveUp', 'moveDown', 'moveFrom', 'moveAllFrom']
    },
    selectionSettings: { mode: 'Multiple' }
});

dest.appendTo('#requiredSkills');
```

## Troubleshooting

### Issue: Toolbar buttons not working

**Cause**: `scope` values don't match or not set correctly.

**Solution**: Verify both lists have same scope:
```typescript
// Wrong - different scopes
const list1: ListBox = new ListBox({ scope: '#list2' });
const list2: ListBox = new ListBox({ scope: '#list1' });

// Correct - matching scopes
const list1: ListBox = new ListBox({ scope: '#list2' });
const list2: ListBox = new ListBox({ scope: '#list2' });
```

### Issue: "Move To" button disabled

**Cause**: No item selected in source list.

**Solution**: 
```typescript
// Ensure source list has items selected
// Check selectionSettings is configured
selectionSettings: { mode: 'Multiple' }
```

### Issue: Items duplicated on move

**Cause**: Data object shared between lists.

**Solution**: Clone data when splitting:
```typescript
// Wrong - shared reference
const allItems = [...];
const available = allItems;
const selected = [];

// Correct - separate arrays
const allItems = [...];
const available = allItems.filter(/* condition */);
const selected = allItems.filter(/* other condition */);
```

### Issue: Scope not transferring items

**Cause**: `allowDragAndDrop` not set or ID mismatch.

**Solution**:
```typescript
const source: ListBox = new ListBox({
    allowDragAndDrop: true,
    scope: '#destList',
    toolbarSettings: {
        items: ['moveTo', 'moveFrom', 'moveAllTo', 'moveAllFrom']
    }
});

const dest: ListBox = new ListBox({
    allowDragAndDrop: true,
    scope: '#destList'  // Must match source
});
```

## Next Steps

- **Drag-and-Drop**: See [drag-and-drop.md](drag-and-drop.md) for reordering within lists
- **Selection**: See [selection-and-checkboxes.md](selection-and-checkboxes.md) for multi-select patterns
- **Data Binding**: See [data-binding.md](data-binding.md) for dynamic data sources
