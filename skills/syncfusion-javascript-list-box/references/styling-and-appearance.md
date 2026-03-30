# Styling & Appearance in ListBox

## Table of Contents
- [Icons & Templates](#icons--templates)
- [CSS Customization](#css-customization)
- [Height & Scrolling](#height--scrolling)
- [Theme Integration](#theme-integration)
- [Responsive Design](#responsive-design)
- [Advanced Styling](#advanced-styling)

## Icons & Templates

### Adding Icons to Items

```typescript
import { ListBox } from '@syncfusion/ej2-dropdowns';

interface MenuItem {
    id: string;
    label: string;
    icon: string;
}

const menuData: MenuItem[] = [
    { id: 'home', label: 'Home', icon: 'icon-home' },
    { id: 'settings', label: 'Settings', icon: 'icon-gear' },
    { id: 'help', label: 'Help', icon: 'icon-question' },
    { id: 'logout', label: 'Logout', icon: 'icon-exit' }
];

const listObj: ListBox = new ListBox({
    dataSource: menuData,
    fields: {
        text: 'label',
        value: 'id',
        iconCss: 'icon'  // Map to icon property
    }
});

listObj.appendTo('#listbox');
```

### Icon CSS Classes

Define icon styles using CSS:

```css
/* Font Awesome icons */
.icon-home::before {
    content: '\f015';
    font-family: FontAwesome;
    margin-right: 8px;
}

.icon-settings::before {
    content: '\f013';
    font-family: FontAwesome;
    margin-right: 8px;
}

.icon-help::before {
    content: '\f059';
    font-family: FontAwesome;
    margin-right: 8px;
}

/* Material icons */
.icon-delete::before {
    content: 'delete';
    font-family: Material Icons;
    margin-right: 8px;
}
```

### HTML Template Support

```typescript
interface Product {
    id: string;
    name: string;
    price: string;
    image: string;
}

const productData: Product[] = [
    { id: 'p1', name: 'Laptop', price: '$999', image: 'laptop.jpg' },
    { id: 'p2', name: 'Mouse', price: '$25', image: 'mouse.jpg' },
    { id: 'p3', name: 'Keyboard', price: '$80', image: 'keyboard.jpg' }
];

// Note: Template support varies by version
// Use itemTemplate event for custom rendering
const listObj: ListBox = new ListBox({
    dataSource: productData,
    fields: { text: 'name', value: 'id' }
});

listObj.appendTo('#listbox');
```

### Custom Item Rendering

```typescript
import { ListBox } from '@syncfusion/ej2-dropdowns';

interface Employee {
    empId: string;
    name: string;
    designation: string;
    photo: string;
}

const employeeData: Employee[] = [
    { empId: 'E001', name: 'John Smith', designation: 'Manager', photo: 'john.jpg' },
    { empId: 'E002', name: 'Sarah Jones', designation: 'Developer', photo: 'sarah.jpg' }
];

// Customize rendering after ListBox creation
const listObj: ListBox = new ListBox({
    dataSource: employeeData,
    fields: { text: 'name', value: 'empId' }
});

listObj.appendTo('#listbox');

// Custom rendering in dropdown
const items = listObj.getItems();
items.forEach((item: any, index: number) => {
    const data = employeeData[index];
    item.innerHTML = `
        <div style="display: flex; align-items: center;">
            <img src="${data.photo}" style="width: 30px; height: 30px; border-radius: 50%; margin-right: 10px;">
            <div>
                <div style="font-weight: bold;">${data.name}</div>
                <div style="font-size: 12px; color: #666;">${data.designation}</div>
            </div>
        </div>
    `;
});
```

## CSS Customization

### ListBox Container Styles

```css
/* Container styling */
.e-listbox {
    border: 1px solid #ddd;
    border-radius: 4px;
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

/* List item base */
.e-listbox .e-list-item {
    padding: 12px 15px;
    border-bottom: 1px solid #f0f0f0;
    transition: background-color 0.2s;
}

/* List item hover */
.e-listbox .e-list-item:hover {
    background-color: #f5f5f5;
    cursor: pointer;
}

/* List item selected */
.e-listbox .e-list-item.e-selected {
    background-color: #e3f2fd;
    color: #1976d2;
    font-weight: 500;
}

/* List item focus */
.e-listbox .e-list-item:focus {
    outline: 2px solid #1976d2;
    outline-offset: -2px;
}

/* List item disabled */
.e-listbox .e-list-item.e-disabled {
    opacity: 0.5;
    cursor: not-allowed;
    background-color: #fafafa;
}
```

### Advanced Item Styling

```css
/* Group headers */
.e-listbox .e-list-group-header {
    padding: 8px 15px;
    font-weight: bold;
    background-color: #f9f9f9;
    color: #333;
    border-bottom: 2px solid #ddd;
}

/* Checkbox styling */
.e-listbox .e-checkbox {
    margin-right: 8px;
}

.e-listbox .e-list-item.e-checked .e-checkbox {
    background-color: #1976d2;
    border-color: #1976d2;
}

/* Select All item */
.e-listbox .e-selectall-item {
    padding: 10px 15px;
    border-bottom: 1px solid #ddd;
    font-weight: 500;
    background-color: #fafafa;
}
```

### Responsive Item Sizes

```css
/* Small items */
.e-listbox.small .e-list-item {
    padding: 6px 10px;
    font-size: 12px;
}

/* Large items */
.e-listbox.large .e-list-item {
    padding: 16px 20px;
    font-size: 16px;
    line-height: 1.6;
}

/* Compact layout */
.e-listbox.compact {
    border-width: 1px;
}

.e-listbox.compact .e-list-item {
    padding: 4px 8px;
}
```

## Height & Scrolling

### Fixed Height

```typescript
import { ListBox } from '@syncfusion/ej2-dropdowns';

const listObj: ListBox = new ListBox({
    dataSource: itemData,
    height: '300px'  // Fixed height enables scrolling
});

listObj.appendTo('#listbox');
```

### CSS for Scrolling

```css
/* Scrollbar customization */
.e-listbox {
    height: 300px;
    overflow-y: auto;
}

/* Scrollbar styling (Chrome, Safari) */
.e-listbox::-webkit-scrollbar {
    width: 8px;
}

.e-listbox::-webkit-scrollbar-track {
    background: #f1f1f1;
}

.e-listbox::-webkit-scrollbar-thumb {
    background: #888;
    border-radius: 4px;
}

.e-listbox::-webkit-scrollbar-thumb:hover {
    background: #555;
}

/* Firefox scrollbar */
.e-listbox {
    scrollbar-width: thin;
    scrollbar-color: #888 #f1f1f1;
}
```

### Auto Height

```typescript
// No fixed height - expands with content
const listObj: ListBox = new ListBox({
    dataSource: itemData
    // No height property = auto-expand
});

listObj.appendTo('#listbox');
```

### Max Height with Scrolling

```css
.e-listbox {
    max-height: 400px;
    overflow-y: auto;
}
```

## Theme Integration

### Syncfusion Themes

Import pre-built themes:

```typescript
// Material theme
import '@syncfusion/ej2-dropdowns/styles/material.css';

// Bootstrap theme
import '@syncfusion/ej2-dropdowns/styles/bootstrap.css';

// Fabric theme
import '@syncfusion/ej2-dropdowns/styles/fabric.css';

// Tailwind theme
import '@syncfusion/ej2-dropdowns/styles/tailwind.css';

// Fluent theme
import '@syncfusion/ej2-dropdowns/styles/fluent.css';

// High contrast theme
import '@syncfusion/ej2-dropdowns/styles/highcontrast.css';
```

### Dark Mode

```typescript
// Import dark theme
import '@syncfusion/ej2-dropdowns/styles/material-dark.css';
// or
import '@syncfusion/ej2-dropdowns/styles/bootstrap-dark.css';
```

### Theme Customization

```css
/* Override theme variables */
:root {
    --primary-color: #1976d2;
    --secondary-color: #f57c00;
    --background-color: #fff;
    --text-color: #333;
    --border-color: #ddd;
}

/* Apply custom colors */
.e-listbox .e-list-item.e-selected {
    background-color: var(--primary-color);
    color: white;
}

.e-listbox {
    border-color: var(--border-color);
    background-color: var(--background-color);
    color: var(--text-color);
}
```

## Responsive Design

### Mobile-Friendly Sizing

```typescript
const listObj: ListBox = new ListBox({
    dataSource: itemData,
    height: 'auto',
    selectionSettings: {
        mode: 'Multiple',
        showCheckbox: true
    }
});

listObj.appendTo('#listbox');
```

### Responsive CSS

```css
/* Desktop */
@media (min-width: 1024px) {
    .e-listbox {
        height: 400px;
        width: 300px;
    }
    
    .e-listbox .e-list-item {
        padding: 12px 15px;
    }
}

/* Tablet */
@media (max-width: 1023px) and (min-width: 768px) {
    .e-listbox {
        height: 300px;
        width: 100%;
    }
    
    .e-listbox .e-list-item {
        padding: 10px 12px;
    }
}

/* Mobile */
@media (max-width: 767px) {
    .e-listbox {
        height: 200px;
        width: 100%;
    }
    
    .e-listbox .e-list-item {
        padding: 8px 10px;
        font-size: 14px;
    }
}
```

### Touch-Friendly Interactions

```css
/* Larger tap targets for touch */
@media (hover: none) and (pointer: coarse) {
    .e-listbox .e-list-item {
        padding: 16px 15px;
        min-height: 44px;
        font-size: 16px;
    }
    
    .e-listbox .e-checkbox {
        width: 24px;
        height: 24px;
    }
}
```

## Advanced Styling

### Animated Selections

```css
@keyframes slideIn {
    from {
        transform: translateX(-20px);
        opacity: 0;
    }
    to {
        transform: translateX(0);
        opacity: 1;
    }
}

.e-listbox .e-list-item {
    animation: slideIn 0.3s ease-out;
}
```

### Custom State Indicators

```css
/* Add indicator for disabled items */
.e-listbox .e-list-item.e-disabled::after {
    content: ' (Disabled)';
    color: #999;
    font-size: 12px;
    font-style: italic;
}

/* Mark important items */
.e-listbox .e-list-item.important::before {
    content: '★ ';
    color: #ffc107;
    margin-right: 4px;
}

/* Required items indicator */
.e-listbox .e-list-item.required::after {
    content: '*';
    color: #d32f2f;
    margin-left: 4px;
    font-weight: bold;
}
```

### Multi-Level Visual Hierarchy

```css
/* Priority-based colors */
.e-listbox .e-list-item.priority-high {
    border-left: 4px solid #d32f2f;
    padding-left: 11px;
}

.e-listbox .e-list-item.priority-medium {
    border-left: 4px solid #f57c00;
    padding-left: 11px;
}

.e-listbox .e-list-item.priority-low {
    border-left: 4px solid #fbc02d;
    padding-left: 11px;
}

/* Status badges */
.e-listbox .e-list-item::after {
    float: right;
    padding: 2px 8px;
    border-radius: 12px;
    font-size: 11px;
    font-weight: bold;
}

.e-listbox .e-list-item.active::after {
    content: 'ACTIVE';
    background-color: #4caf50;
    color: white;
}

.e-listbox .e-list-item.inactive::after {
    content: 'INACTIVE';
    background-color: #9e9e9e;
    color: white;
}
```

### Dark Mode Support

```css
/* Detect system preference */
@media (prefers-color-scheme: dark) {
    .e-listbox {
        background-color: #1e1e1e;
        color: #e0e0e0;
        border-color: #404040;
    }
    
    .e-listbox .e-list-item {
        color: #e0e0e0;
        border-bottom-color: #2a2a2a;
    }
    
    .e-listbox .e-list-item:hover {
        background-color: #2a2a2a;
    }
    
    .e-listbox .e-list-item.e-selected {
        background-color: #1976d2;
        color: white;
    }
}
```

### High Contrast Mode

```css
/* High contrast for accessibility */
@media (prefers-contrast: more) {
    .e-listbox {
        border: 2px solid #000;
        background-color: #fff;
    }
    
    .e-listbox .e-list-item {
        border: 1px solid #000;
        color: #000;
    }
    
    .e-listbox .e-list-item.e-selected {
        background-color: #000;
        color: #fff;
        font-weight: bold;
    }
}
```

## Next Steps

- **Data Binding**: See [data-binding.md](data-binding.md) for dynamic data sources
- **Selection**: See [selection-and-checkboxes.md](selection-and-checkboxes.md) for selection styling
- **Accessibility**: See [accessibility-and-keyboard.md](accessibility-and-keyboard.md) for contrast and visibility
