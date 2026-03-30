# Sorting & Grouping in ListBox

## Table of Contents
- [Sorting Overview](#sorting-overview)
- [Sort Order Options](#sort-order-options)
- [Grouping Basics](#grouping-basics)
- [Combined Scenarios](#combined-scenarios)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Sorting Overview

The ListBox sorts available items alphabetically in ascending or descending order using the `sortOrder` property.

### Sort Order Property

| Value | Behavior |
|-------|----------|
| `'None'` | No sorting (default, items display in source order) |
| `'Ascending'` | Alphabetically ascending (A-Z) |
| `'Descending'` | Alphabetically descending (Z-A) |

## Sort Order Options

### Ascending Sort

```typescript
import { ListBox } from '@syncfusion/ej2-dropdowns';

const countryData: { [key: string]: Object }[] = [
    { "Name": "Denmark", "Code": "DK" },
    { "Name": "Australia", "Code": "AU" },
    { "Name": "France", "Code": "FR" },
    { "Name": "Canada", "Code": "CA" }
];

const listObj: ListBox = new ListBox({
    dataSource: countryData,
    fields: { text: 'Name', value: 'Code' },
    sortOrder: 'Ascending'  // A to Z
});

listObj.appendTo('#listbox');
```

**Result**: Lists countries alphabetically: Australia, Canada, Denmark, France

### Descending Sort

```typescript
const listObj: ListBox = new ListBox({
    dataSource: countryData,
    fields: { text: 'Name', value: 'Code' },
    sortOrder: 'Descending'  // Z to A
});

listObj.appendTo('#listbox');
```

**Result**: Lists countries in reverse order: France, Denmark, Canada, Australia

### No Sorting

```typescript
const listObj: ListBox = new ListBox({
    dataSource: countryData,
    fields: { text: 'Name', value: 'Code' },
    sortOrder: 'None'  // Source order maintained
});

listObj.appendTo('#listbox');
```

**Result**: Lists items in original order: Denmark, Australia, France, Canada

### Sort with Object Arrays

```typescript
interface Product {
    id: string;
    name: string;
    category: string;
}

const productData: Product[] = [
    { id: 'p3', name: 'Zebra Striped Item', category: 'Electronics' },
    { id: 'p1', name: 'Apple Watch', category: 'Electronics' },
    { id: 'p4', name: 'Banana Stand', category: 'Furniture' },
    { id: 'p2', name: 'Orange Juice', category: 'Food' }
];

const listObj: ListBox = new ListBox({
    dataSource: productData,
    fields: { text: 'name', value: 'id' },
    sortOrder: 'Ascending'
});

listObj.appendTo('#listbox');

// Result: Apple Watch, Banana Stand, Orange Juice, Zebra Striped Item
```

## Grouping Basics

Organize items into logical groups/categories using the `groupBy` field.

### Basic Grouping

```typescript
import { ListBox } from '@syncfusion/ej2-dropdowns';

interface Vegetable {
    Name: string;
    Category: string;
    Id: string;
}

const vegetableData: Vegetable[] = [
    { "Name": "Cabbage", "Category": "Leafy and Salad", "Id": "item1" },
    { "Name": "Spinach", "Category": "Leafy and Salad", "Id": "item2" },
    { "Name": "Wheat grass", "Category": "Leafy and Salad", "Id": "item3" },
    { "Name": "Chickpea", "Category": "Beans", "Id": "item6" },
    { "Name": "Green bean", "Category": "Beans", "Id": "item7" },
    { "Name": "Horse gram", "Category": "Beans", "Id": "item8" },
    { "Name": "Garlic", "Category": "Bulb and Stem", "Id": "item9" },
    { "Name": "Onion", "Category": "Bulb and Stem", "Id": "item11" }
];

const listObj: ListBox = new ListBox({
    dataSource: vegetableData,
    fields: {
        text: 'Name',
        value: 'Id',
        groupBy: 'Category'
    }
});

listObj.appendTo('#listbox');
```

**Result**: Items displayed in groups:
```
Beans
  - Chickpea
  - Green bean
  - Horse gram

Bulb and Stem
  - Garlic
  - Onion

Leafy and Salad
  - Cabbage
  - Spinach
  - Wheat grass
```

### Group Header Styling

```typescript
// The groupBy field automatically creates group headers
// Headers are rendered as non-selectable elements
// Users can collapse/expand groups (depending on styling)
```

### Multiple Groups

```typescript
interface Employee {
    id: string;
    name: string;
    department: string;
    location: string;
}

const employeeData: Employee[] = [
    { id: 'e1', name: 'John Smith', department: 'Engineering', location: 'New York' },
    { id: 'e2', name: 'Sarah Jones', department: 'Engineering', location: 'New York' },
    { id: 'e3', name: 'Mike Brown', department: 'Marketing', location: 'Boston' },
    { id: 'e4', name: 'Lisa Anderson', department: 'Marketing', location: 'New York' }
];

const listObj: ListBox = new ListBox({
    dataSource: employeeData,
    fields: {
        text: 'name',
        value: 'id',
        groupBy: 'department'  // Group by department
    }
});

listObj.appendTo('#listbox');
```

## HTML Element Grouping

### OPTGROUP Support

Initialize from HTML SELECT with OPTGROUP:

```html
<select id="listbox">
    <optgroup label="Fruits">
        <option value="apple">Apple</option>
        <option value="banana">Banana</option>
        <option value="orange">Orange</option>
    </optgroup>
    <optgroup label="Vegetables">
        <option value="carrot">Carrot</option>
        <option value="broccoli">Broccoli</option>
        <option value="spinach">Spinach</option>
    </optgroup>
</select>
```

```typescript
import { ListBox } from '@syncfusion/ej2-dropdowns';

const listObj: ListBox = new ListBox();
listObj.appendTo('#listbox');

// No field mapping needed - groups from OPTGROUP
```

**Result**: Items grouped by OPTGROUP labels

## Combined Scenarios

### Sorting + Grouping

Combine sorted items with logical grouping:

```typescript
const listObj: ListBox = new ListBox({
    dataSource: vegetableData,
    fields: {
        text: 'Name',
        value: 'Id',
        groupBy: 'Category'
    },
    sortOrder: 'Ascending'  // Sort alphabetically within and across groups
});

listObj.appendTo('#listbox');

// Result: Groups appear alphabetically, items within groups sorted
// Beans (group A-Z), then Bulb and Stem, then Leafy and Salad
// Within each group, items sorted alphabetically
```

### Grouped with Icons

```typescript
interface Task {
    id: string;
    name: string;
    priority: string;
    icon: string;
}

const taskData: Task[] = [
    { id: 't1', name: 'Fix bug', priority: 'High', icon: 'icon-warning' },
    { id: 't2', name: 'Write tests', priority: 'Medium', icon: 'icon-info' },
    { id: 't3', name: 'Update docs', priority: 'Low', icon: 'icon-check' }
];

const listObj: ListBox = new ListBox({
    dataSource: taskData,
    fields: {
        text: 'name',
        value: 'id',
        groupBy: 'priority',
        iconCss: 'icon'
    }
});

listObj.appendTo('#listbox');
```

### Grouped with Selection

```typescript
const listObj: ListBox = new ListBox({
    dataSource: vegetableData,
    fields: {
        text: 'Name',
        value: 'Id',
        groupBy: 'Category'
    },
    selectionSettings: {
        mode: 'Multiple',
        showCheckbox: true
    }
});

listObj.appendTo('#listbox');

// Users can select items within grouped display
```

## Best Practices

### 1. Ensure GroupBy Field Consistency

**Wrong** - inconsistent category values:
```typescript
const data = [
    { name: 'Item 1', category: 'Category A' },
    { name: 'Item 2', category: 'categoryA' },  // Different case
    { name: 'Item 3', category: 'Cat A' }       // Different text
];
```

**Correct** - consistent values:
```typescript
const data = [
    { name: 'Item 1', category: 'Category A' },
    { name: 'Item 2', category: 'Category A' },
    { name: 'Item 3', category: 'Category A' }
];
```

### 2. Choose Logical Grouping

**Good grouping**:
- By category (Electronics, Clothing, Food)
- By department (Engineering, Sales, HR)
- By priority (High, Medium, Low)
- By status (Active, Inactive, Pending)

**Poor grouping**:
- By first letter (creates too many groups)
- By random field (confuses users)

### 3. Consider Performance with Large Datasets

For hundreds of items:
- Group reduces visual clutter
- Sort for quick scanning
- Combine both for optimal organization

```typescript
const largeDataset = /* 500+ items */;

const listObj: ListBox = new ListBox({
    dataSource: largeDataset,
    fields: {
        text: 'name',
        value: 'id',
        groupBy: 'category'  // Organize into groups
    },
    sortOrder: 'Ascending',  // Sort within groups
    height: '400px'  // Scrollable list
});

listObj.appendTo('#listbox');
```

### 4. Combine with Selection and Drag-Drop

```typescript
const listObj: ListBox = new ListBox({
    dataSource: vegetableData,
    fields: {
        text: 'Name',
        value: 'Id',
        groupBy: 'Category'
    },
    sortOrder: 'Ascending',
    selectionSettings: { mode: 'Multiple' },
    allowDragAndDrop: true
});

listObj.appendTo('#listbox');
```

## Troubleshooting

### Issue: Grouping not appearing

**Cause**: `groupBy` field doesn't exist in data or field name misspelled.

**Solution**: Verify field exists and is spelled correctly:
```typescript
// Wrong
fields: { groupBy: 'catgory' }  // Typo

// Correct
fields: { groupBy: 'category' }
```

### Issue: Items appear in wrong groups

**Cause**: GroupBy field values are inconsistent (different cases, extra spaces).

**Solution**: Normalize data before binding:
```typescript
// Clean data
const cleanData = vegetableData.map(item => ({
    ...item,
    Category: item.Category.trim().toLowerCase()
}));

fields: { groupBy: 'Category' }
```

### Issue: Sorting not working within groups

**Cause**: `sortOrder` set but data in source order.

**Solution**: Ensure `sortOrder` is set:
```typescript
sortOrder: 'Ascending'  // or 'Descending'
```

### Issue: Too many groups

**Cause**: Grouping by field with many unique values.

**Solution**: Group by higher-level category:
```typescript
// Too many groups (one per item)
fields: { groupBy: 'id' }

// Better - logical grouping
fields: { groupBy: 'department' }
```

### Issue: Performance slow with large grouped list

**Cause**: Rendering thousands of grouped items.

**Solution**: Implement virtualization or pagination:
```typescript
const listObj: ListBox = new ListBox({
    dataSource: largeDataset,
    fields: { groupBy: 'category' },
    height: '400px',  // Fixed height for scrolling
    sortOrder: 'Ascending'
});

listObj.appendTo('#listbox');
```

## Next Steps

- **Drag-and-Drop**: See [drag-and-drop.md](drag-and-drop.md) for reordering grouped items
- **Selection**: See [selection-and-checkboxes.md](selection-and-checkboxes.md) for selecting grouped items
- **Data Binding**: See [data-binding.md](data-binding.md) for remote data with grouping
