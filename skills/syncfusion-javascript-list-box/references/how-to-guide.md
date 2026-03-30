# How-To Guide for ListBox

## Table of Contents
- [Add Items](#add-items)
- [Enable or Disable Items](#enable-or-disable-items)
- [Select Items](#select-items)
- [Enable Scrolling](#enable-scrolling)
- [Show Tooltips](#show-tooltips)
- [Filter ListBox Data](#filter-listbox-data)
- [Form Submission](#form-submission)

## Add Items

### Adding Single Item

Add a new item dynamically to the ListBox using the `addItems` method:

```typescript
import { ListBox } from '@syncfusion/ej2-dropdowns';

const carData = [
    { text: 'Hennessey Venom', id: 'car-01' },
    { text: 'Bugatti Chiron', id: 'car-02' },
    { text: 'Koenigsegg CCR', id: 'car-05' }
];

const listObj: ListBox = new ListBox({
    dataSource: carData
});

listObj.appendTo('#listbox');

// Add single item
document.getElementById('addButton').onclick = () => {
    const newItem = { text: 'McLaren F1', id: 'car-06' };
    listObj.addItems([newItem]);
};
```

### Adding Multiple Items

Add multiple items at once:

```typescript
import { ListBox } from '@syncfusion/ej2-dropdowns';

const listObj: ListBox = new ListBox({
    dataSource: carData
});

listObj.appendTo('#listbox');

document.getElementById('addMultipleButton').onclick = () => {
    const newItems = [
        { text: 'Bugatti Veyron Super Sport', id: 'car-03' },
        { text: 'SSC Ultimate Aero', id: 'car-04' }
    ];
    listObj.addItems(newItems);
};
```

### Checking Before Adding

Prevent duplicates by checking if item exists:

```typescript
document.getElementById('addButton').onclick = () => {
    // Check if item already exists
    if (!listObj.getDataByValue('McLaren F1')) {
        listObj.addItems([{ text: 'McLaren F1', id: 'car-06' }]);
    } else {
        console.log('Item already exists');
    }
};
```

### Best Practices

- Use `getDataByValue()` or `getDataByText()` to check existence
- Keep items consistent with existing data structure
- Validate data before adding
- Provide user feedback (toast/alert) for successful addition

## Enable or Disable Items

### Disabling Items on Initialization

Disable specific items when ListBox is created:

```typescript
import { ListBox } from '@syncfusion/ej2-dropdowns';

const listObj: ListBox = new ListBox({
    dataSource: carData,
    created: () => {
        // Disable specific items by text
        listObj.enableItems(['Bugatti Chiron', 'McLaren F1'], false);
    }
});

listObj.appendTo('#listbox');
```

### Enabling Disabled Items

Re-enable previously disabled items:

```typescript
document.getElementById('enableButton').onclick = () => {
    // Enable items that were disabled
    listObj.enableItems(['Bugatti Chiron', 'McLaren F1'], true);
};
```

### Disabling After Initialization

Disable items at runtime:

```typescript
document.getElementById('disableButton').onclick = () => {
    // Disable selected items
    const selectedItems = listObj.getSelectedItems();
    listObj.enableItems(selectedItems.text, false);
};
```

### Visual Behavior

Disabled items:
- Cannot be selected
- Appear grayed out
- Not included in selection operations
- Keyboard navigation skips them

### Practical Example: Role-Based Access

```typescript
const items = [
    { text: 'View', id: 'view', role: 'user' },
    { text: 'Edit', id: 'edit', role: 'admin' },
    { text: 'Delete', id: 'delete', role: 'admin' }
];

const listObj: ListBox = new ListBox({
    dataSource: items,
    fields: { text: 'text', value: 'id' },
    created: () => {
        // Disable admin actions for regular users
        const userRole = 'user';
        if (userRole !== 'admin') {
            const adminItems = items
                .filter(item => item.role === 'admin')
                .map(item => item.text);
            listObj.enableItems(adminItems, false);
        }
    }
});

listObj.appendTo('#listbox');
```

## Select Items

### Programmatically Select Single Item

Select an item by text:

```typescript
import { ListBox } from '@syncfusion/ej2-dropdowns';

const listObj: ListBox = new ListBox({
    dataSource: carData,
    created: () => {
        // Select item by text after creation
        listObj.selectItems(['Bugatti Chiron']);
    }
});

listObj.appendTo('#listbox');
```

### Select Multiple Items

Select multiple items programmatically:

```typescript
document.getElementById('selectMultipleButton').onclick = () => {
    // Select multiple items by text
    listObj.selectItems(['Bugatti Chiron', 'McLaren F1', 'Koenigsegg CCR']);
};
```

### Select by Index

Select items using their index position:

```typescript
document.getElementById('selectByIndexButton').onclick = () => {
    // Select items by index (0-based)
    listObj.selectItemByIndex([0, 2, 4]);
};
```

### Select All Items

Select every item in the ListBox:

```typescript
document.getElementById('selectAllButton').onclick = () => {
    listObj.selectAll();
    const selected = listObj.getSelectedItems();
    console.log(`Selected ${selected.text.length} items`);
};
```

### Clear Selection

Remove all selections:

```typescript
document.getElementById('clearButton').onclick = () => {
    listObj.clearSelection();
};
```

### Conditional Selection

Select items based on criteria:

```typescript
// Select items matching a condition
function selectByCondition(predicate) {
    const itemsToSelect = carData
        .filter(predicate)
        .map(item => item.text);
    listObj.selectItems(itemsToSelect);
}

// Example: Select items containing 'Bugatti'
selectByCondition(item => item.text.includes('Bugatti'));
```

## Enable Scrolling

### Fixed Height

Enable scrolling by setting a fixed height:

```typescript
import { ListBox } from '@syncfusion/ej2-dropdowns';

const listObj: ListBox = new ListBox({
    dataSource: carData,
    height: '300px'  // Fixed height enables scrolling
});

listObj.appendTo('#listbox');
```

### Responsive Height

```typescript
const listObj: ListBox = new ListBox({
    dataSource: carData,
    height: '350px'
});

listObj.appendTo('#listbox');

// Adjust height on window resize
window.addEventListener('resize', () => {
    if (window.innerWidth < 768) {
        listObj.height = '200px';
    } else {
        listObj.height = '350px';
    }
});
```

### Max Height

Limit maximum height while allowing dynamic sizing:

```css
.e-listbox {
    max-height: 400px;
    overflow-y: auto;
    border: 1px solid #ddd;
}

.e-listbox::-webkit-scrollbar {
    width: 8px;
}

.e-listbox::-webkit-scrollbar-thumb {
    background: #888;
    border-radius: 4px;
}

.e-listbox::-webkit-scrollbar-thumb:hover {
    background: #555;
}
```

### No Scroll (Auto Expand)

ListBox expands with content (no scrollbar):

```typescript
// Don't set height property
const listObj: ListBox = new ListBox({
    dataSource: carData
    // No height = auto-expand
});

listObj.appendTo('#listbox');
```

### Scrolling with Large Datasets

```typescript
const listObj: ListBox = new ListBox({
    dataSource: largeDataset,  // 1000+ items
    height: '400px',
    fields: { text: 'name', value: 'id' }
});

listObj.appendTo('#listbox');
```

## Show Tooltips

### Basic Tooltip via Title Attribute

Show tooltip on hover using `beforeItemRender` event:

```typescript
import { ListBox, BeforeItemRenderEventArgs } from '@syncfusion/ej2-dropdowns';

const carData = [
    { text: 'Hennessey Venom', id: 'car-01', description: 'Top speed: 270 mph' },
    { text: 'Bugatti Chiron', id: 'car-02', description: 'Top speed: 261 mph' },
    { text: 'McLaren F1', id: 'car-06', description: 'Top speed: 240 mph' }
];

const listObj: ListBox = new ListBox({
    dataSource: carData,
    beforeItemRender: (args: BeforeItemRenderEventArgs) => {
        // Set title attribute for tooltip
        args.element.setAttribute('title', args.element.textContent);
    }
});

listObj.appendTo('#listbox');
```

### Tooltip with Custom Content

```typescript
const listObj: ListBox = new ListBox({
    dataSource: carData,
    fields: { text: 'text', value: 'id' },
    beforeItemRender: (args: BeforeItemRenderEventArgs) => {
        // Find matching data item
        const dataItem = carData.find(item => item.text === args.element.textContent);
        if (dataItem && dataItem.description) {
            args.element.setAttribute('title', dataItem.description);
        }
    }
});

listObj.appendTo('#listbox');
```

### Advanced Tooltip with HTML

```typescript
const listObj: ListBox = new ListBox({
    dataSource: carData,
    beforeItemRender: (args: BeforeItemRenderEventArgs) => {
        const dataItem = carData.find(item => item.text === args.element.textContent);
        if (dataItem) {
            // Note: Simple title attribute doesn't support HTML
            // For rich HTML tooltips, use Syncfusion Tooltip component
            args.element.setAttribute('title', 
                `${dataItem.text} - ${dataItem.description}`);
        }
    }
});

listObj.appendTo('#listbox');
```

### Using Syncfusion Tooltip Component

For advanced tooltip features:

```typescript
import { ListBox } from '@syncfusion/ej2-dropdowns';
import { Tooltip } from '@syncfusion/ej2-popups';

const listObj: ListBox = new ListBox({
    dataSource: carData,
    height: '300px'
});

listObj.appendTo('#listbox');

// Initialize Tooltip for ListBox items
const tooltip = new Tooltip({
    target: '.e-listbox .e-list-item',
    beforeRender: (args: any) => {
        const text = args.target.textContent;
        const dataItem = carData.find(item => item.text === text);
        args.content = dataItem ? dataItem.description : text;
    }
});

tooltip.appendTo('.e-listbox');
```

## Filter ListBox Data

### Basic Filtering

Filter ListBox items based on user input:

```typescript
import { ListBox } from '@syncfusion/ej2-dropdowns';
import { Query } from '@syncfusion/ej2-data';

const carData = [
    { text: 'Hennessey Venom' },
    { text: 'Bugatti Chiron' },
    { text: 'Bugatti Veyron Super Sport' },
    { text: 'McLaren F1' }
];

const listObj: ListBox = new ListBox({
    dataSource: carData
});

listObj.appendTo('#listbox');

// Filter on TextBox input
const textBox = document.getElementById('filterTextBox');
textBox.addEventListener('input', (e: any) => {
    const filterValue = e.target.value;
    
    // Filter data: contains text (case-insensitive)
    listObj.filter(
        carData,
        new Query().where('text', 'contains', filterValue, true)
    );
});
```

### HTML Template

```html
<div>
    <label for="filterTextBox">Filter Cars:</label>
    <input type="text" id="filterTextBox" placeholder="Type to filter...">
    <div id="listbox"></div>
</div>
```

### Case-Sensitive Filtering

```typescript
textBox.addEventListener('input', (e: any) => {
    const filterValue = e.target.value;
    
    // Case-sensitive filter (don't pass 'true' for ignoreCase)
    listObj.filter(carData, new Query().where('text', 'contains', filterValue));
});
```

### Multiple Field Filtering

```typescript
const employeeData = [
    { name: 'John Smith', dept: 'Engineering' },
    { name: 'Sarah Jones', dept: 'Marketing' },
    { name: 'Mike Brown', dept: 'Engineering' }
];

const listObj: ListBox = new ListBox({
    dataSource: employeeData,
    fields: { text: 'name', value: 'name' }
});

listObj.appendTo('#listbox');

// Filter by department
document.getElementById('filterDept').addEventListener('change', (e: any) => {
    const dept = e.target.value;
    
    if (dept === 'all') {
        listObj.filter(employeeData);
    } else {
        listObj.filter(
            employeeData,
            new Query().where('dept', 'equal', dept)
        );
    }
});
```

### Real-Time Debounced Filtering

```typescript
let debounceTimer;
textBox.addEventListener('input', (e: any) => {
    clearTimeout(debounceTimer);
    
    debounceTimer = setTimeout(() => {
        const filterValue = e.target.value.trim();
        
        if (filterValue.length < 2) {
            listObj.filter(carData);  // Reset to all
        } else {
            listObj.filter(
                carData,
                new Query().where('text', 'contains', filterValue, true)
            );
        }
    }, 300);  // Wait 300ms after user stops typing
});
```

## Form Submission

### Basic Form with ListBox

Submit selected ListBox values in a form:

```html
<form id="selectionForm">
    <label for="listbox">Select cars:</label>
    <div id="listbox"></div>
    <button type="submit">Submit</button>
</form>
```

### TypeScript Implementation

```typescript
import { ListBox } from '@syncfusion/ej2-dropdowns';

const carData = [
    { text: 'Hennessey Venom', id: 'car-01' },
    { text: 'Bugatti Chiron', id: 'car-02' },
    { text: 'McLaren F1', id: 'car-06' }
];

const listObj: ListBox = new ListBox({
    dataSource: carData,
    fields: { text: 'text', value: 'id' },
    selectionSettings: { mode: 'Multiple' }
});

listObj.appendTo('#listbox');

// Handle form submission
const form = document.getElementById('selectionForm') as HTMLFormElement;
form.addEventListener('submit', (e: Event) => {
    e.preventDefault();
    
    // Get selected values
    const selected = listObj.getSelectedItems();
    console.log('Selected cars:', selected.value);
    
    // Submit data to server
    submitFormData(selected.value);
});

function submitFormData(selectedIds: any[]) {
    const formData = new FormData();
    formData.append('cars', JSON.stringify(selectedIds));
    
    fetch('/api/cars/save', {
        method: 'POST',
        body: formData
    })
    .then(response => response.json())
    .then(data => {
        console.log('Success:', data);
        alert('Form submitted successfully!');
    })
    .catch(error => console.error('Error:', error));
}
```

### Hidden Input Field Method

```html
<form id="selectionForm">
    <div id="listbox"></div>
    <input type="hidden" id="selectedCars" name="cars">
    <button type="submit">Submit</button>
</form>
```

```typescript
form.addEventListener('submit', (e: Event) => {
    e.preventDefault();
    
    const selected = listObj.getSelectedItems();
    const hiddenInput = document.getElementById('selectedCars') as HTMLInputElement;
    
    // Store selected values in hidden input
    hiddenInput.value = JSON.stringify(selected.value);
    
    // Now form can be submitted traditionally
    form.submit();
});
```

### Form Validation

```typescript
form.addEventListener('submit', (e: Event) => {
    e.preventDefault();
    
    const selected = listObj.getSelectedItems();
    
    // Validate that at least one item selected
    if (!selected.value || selected.value.length === 0) {
        alert('Please select at least one car');
        return;
    }
    
    // Valid - proceed with submission
    submitFormData(selected.value);
});
```

### Populate ListBox from Form

```typescript
// Load previously selected items
window.addEventListener('load', () => {
    const savedSelection = localStorage.getItem('carSelection');
    if (savedSelection) {
        const saved = JSON.parse(savedSelection);
        listObj.selectItems(saved);
    }
});

// Save selection on change
listObj.change = () => {
    const selected = listObj.getSelectedItems();
    localStorage.setItem('carSelection', JSON.stringify(selected.value));
};
```

## Common Patterns & Best Practices

### Pattern 1: Dynamic Add & Delete

```typescript
let itemCounter = 0;

document.getElementById('addButton').onclick = () => {
    itemCounter++;
    const newItem = { text: `Item ${itemCounter}`, id: `item-${itemCounter}` };
    listObj.addItems([newItem]);
};

document.getElementById('deleteButton').onclick = () => {
    const selected = listObj.getSelectedItems();
    if (selected.value && selected.value.length > 0) {
        // Remove by value
        listObj.remove(selected.value);
    }
};
```

### Pattern 2: Search & Filter with Highlighting

```typescript
const searchInput = document.getElementById('search') as HTMLInputElement;
searchInput.addEventListener('input', (e: any) => {
    const searchText = e.target.value.toLowerCase();
    
    if (searchText) {
        listObj.filter(
            carData,
            new Query().where('text', 'contains', searchText, true)
        );
    } else {
        listObj.filter(carData);
    }
});
```

### Pattern 3: Multi-Step Form

```typescript
// Step 1: Select primary car
const step1List = new ListBox({
    dataSource: carData,
    selectionSettings: { mode: 'Single' }
});

// Step 2: Select additional features (multi-select)
const step2List = new ListBox({
    dataSource: featureData,
    selectionSettings: { mode: 'Multiple', showCheckbox: true }
});

// Submit complete form
function submitMultiStepForm() {
    const step1Selected = step1List.getSelectedItems();
    const step2Selected = step2List.getSelectedItems();
    
    const formData = {
        primaryCar: step1Selected.value[0],
        features: step2Selected.value
    };
    
    // Submit to server
    submitFormData(formData);
}
```

## Next Steps

- **Selection**: See [selection-and-checkboxes.md](selection-and-checkboxes.md) for advanced selection
- **Data Binding**: See [data-binding.md](data-binding.md) for remote data operations
- **Events**: See [accessibility-and-keyboard.md](accessibility-and-keyboard.md) for event handling
