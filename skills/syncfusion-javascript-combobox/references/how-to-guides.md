# How-To Guides for ComboBox

## Table of Contents
- [Autofill Search](#autofill-search)
- [Cascading Dropdowns](#cascading-dropdowns)
- [Icons in List Items](#icons-in-list-items)
- [Form Integration](#form-integration)
- [Custom Styling](#custom-styling)

## Autofill Search

### Enable Auto-Complete

The `autofill` property enables automatic completion of typed text:

```typescript
import { ComboBox } from '@syncfusion/ej2-dropdowns';

const languageData: string[] = ['JavaScript', 'TypeScript', 'Python', 'Java', 'C#', 'Ruby', 'Go', 'Rust'];

const comboBox: ComboBox = new ComboBox({
    dataSource: languageData,
    autofill: true,  // Enable auto-complete
    allowFiltering: true,
    placeholder: "Type to auto-complete..."
});

comboBox.appendTo('#comboelement');

// User interaction:
// Type "ja" → auto-fills "JavaScript"
// Type "py" → auto-fills "Python"
// Type "r" → auto-fills "Ruby"
```

**Behavior:**
- User types first letter(s)
- ComboBox auto-completes with first matching item
- User can press Tab/Enter to accept or continue typing

---

### Autofill with Filtering

Combine autofill with filtering for smart suggestions:

```typescript
const frameworkData: string[] = [
    'Angular',
    'AngularJS',
    'React',
    'React Native',
    'Vue',
    'Vue 3',
    'Svelte',
    'Next.js',
    'Nuxt'
];

const comboBox: ComboBox = new ComboBox({
    dataSource: frameworkData,
    autofill: true,
    allowFiltering: true,
    placeholder: "Search and auto-fill framework..."
});

comboBox.appendTo('#comboelement');

// User types "ang" → shows Angular options, auto-fills "Angular"
// User types "rea" → shows React options, auto-fills "React"
// User types "vue" → shows Vue options, auto-fills "Vue"
```

---

## Cascading Dropdowns

### Three-Level Cascading (Country → State → City)

```typescript
import { ComboBox } from '@syncfusion/ej2-dropdowns';
import { Query } from '@syncfusion/ej2-data';

// Define data for three levels
const countryData: { [key: string]: Object }[] = [
    { id: 'USA', name: 'United States' },
    { id: 'IND', name: 'India' },
    { id: 'AUS', name: 'Australia' }
];

const stateData: { [key: string]: Object }[] = [
    // USA states
    { id: 'CA', name: 'California', countryId: 'USA' },
    { id: 'TX', name: 'Texas', countryId: 'USA' },
    { id: 'NY', name: 'New York', countryId: 'USA' },
    // India states
    { id: 'TN', name: 'Tamil Nadu', countryId: 'IND' },
    { id: 'MH', name: 'Maharashtra', countryId: 'IND' },
    { id: 'KA', name: 'Karnataka', countryId: 'IND' },
    // Australia states
    { id: 'NSW', name: 'New South Wales', countryId: 'AUS' },
    { id: 'VIC', name: 'Victoria', countryId: 'AUS' }
];

const cityData: { [key: string]: Object }[] = [
    // California cities
    { id: 'SF', name: 'San Francisco', stateId: 'CA' },
    { id: 'LA', name: 'Los Angeles', stateId: 'CA' },
    // Texas cities
    { id: 'HOU', name: 'Houston', stateId: 'TX' },
    { id: 'DAL', name: 'Dallas', stateId: 'TX' },
    // Tamil Nadu cities
    { id: 'CHN', name: 'Chennai', stateId: 'TN' },
    { id: 'CBE', name: 'Coimbatore', stateId: 'TN' },
    // And more...
];

// Country ComboBox
const countryCombo: ComboBox = new ComboBox({
    dataSource: countryData,
    fields: { text: 'name', value: 'id' },
    change: () => {
        const selectedCountry = countryCombo.value;
        
        // Update states based on selected country
        const statesForCountry = stateData.filter(s => s.countryId === selectedCountry);
        stateCombo.dataSource = statesForCountry;
        stateCombo.enabled = true;
        stateCombo.refresh();
        stateCombo.value = '';  // Clear previous selection
        
        // Clear city ComboBox
        cityCombo.dataSource = [];
        cityCombo.enabled = false;
        cityCombo.refresh();
        cityCombo.value = '';
    },
    placeholder: "Select country"
});

countryCombo.appendTo('#country');

// State ComboBox
const stateCombo: ComboBox = new ComboBox({
    dataSource: [],
    fields: { text: 'name', value: 'id' },
    enabled: false,  // Disabled until country selected
    change: () => {
        const selectedState = stateCombo.value;
        
        // Update cities based on selected state
        const citiesForState = cityData.filter(c => c.stateId === selectedState);
        cityCombo.dataSource = citiesForState;
        cityCombo.enabled = true;
        cityCombo.refresh();
        cityCombo.value = '';  // Clear previous selection
    },
    placeholder: "Select state"
});

stateCombo.appendTo('#state');

// City ComboBox
const cityCombo: ComboBox = new ComboBox({
    dataSource: [],
    fields: { text: 'name', value: 'id' },
    enabled: false,  // Disabled until state selected
    placeholder: "Select city"
});

cityCombo.appendTo('#city');

// HTML
// <input type="text" id="country" />
// <input type="text" id="state" />
// <input type="text" id="city" />
```

**Workflow:**
1. User selects country → states update
2. User selects state → cities update
3. User selects city → complete

---

### Cascading with Remote Data

```typescript
import { ComboBox } from '@syncfusion/ej2-dropdowns';
import { DataManager, Query, ODataV4Adaptor } from '@syncfusion/ej2-data';

const departmentData = new DataManager({
    url: 'https://api.example.com/departments',
    adaptor: new ODataV4Adaptor
});

const employeeDataBase = new DataManager({
    url: 'https://api.example.com/employees',
    adaptor: new ODataV4Adaptor
});

// Department ComboBox
const deptCombo: ComboBox = new ComboBox({
    dataSource: departmentData,
    query: new Query().select(['id', 'name']),
    fields: { text: 'name', value: 'id' },
    change: () => {
        const selectedDept = deptCombo.value;
        
        // Fetch employees for selected department from server
        const empQuery = new Query()
            .select(['id', 'name', 'email'])
            .where('departmentId', 'equal', selectedDept);
        
        empCombo.dataSource = employeeDataBase;
        empCombo.query = empQuery;
        empCombo.enabled = true;
        empCombo.refresh();
        empCombo.value = '';
    },
    placeholder: "Select department"
});

deptCombo.appendTo('#department');

// Employee ComboBox
const empCombo: ComboBox = new ComboBox({
    dataSource: [],
    fields: { text: 'name', value: 'id' },
    enabled: false,
    placeholder: "Select employee"
});

empCombo.appendTo('#employee');
```

---

## Icons in List Items

### Display Icons with Items

Add icons to list items using `iconCss` field:

```typescript
import { ComboBox } from '@syncfusion/ej2-dropdowns';

const sortFormatData: { [key: string]: Object }[] = [
    { id: 'asc', label: 'Sort A to Z', iconClass: 'e-icons e-sort-ascending' },
    { id: 'desc', label: 'Sort Z to A', iconClass: 'e-icons e-sort-descending' },
    { id: 'filter', label: 'Filter', iconClass: 'e-icons e-filter' },
    { id: 'clear', label: 'Clear', iconClass: 'e-icons e-close' }
];

const comboBox: ComboBox = new ComboBox({
    dataSource: sortFormatData,
    fields: { 
        text: 'label', 
        value: 'id',
        iconCss: 'iconClass'  // Map icon class field
    },
    placeholder: "Select action"
});

comboBox.appendTo('#comboelement');

// Result: Each item shows icon + label
// ⬆️ Sort A to Z
// ⬇️ Sort Z to A
// 🔍 Filter
// ✕ Clear
```

---

### Custom Emoji Icons

```typescript
const statusData: { [key: string]: Object }[] = [
    { id: 'new', name: 'New', emoji: '✨' },
    { id: 'progress', name: 'In Progress', emoji: '🔄' },
    { id: 'review', name: 'Under Review', emoji: '👀' },
    { id: 'done', name: 'Done', emoji: '✅' },
    { id: 'archived', name: 'Archived', emoji: '📦' }
];

const comboBox: ComboBox = new ComboBox({
    dataSource: statusData,
    
    itemTemplate: '<div style="display: flex; gap: 10px; align-items: center;"><span style="font-size: 20px;">${emoji}</span><span>${name}</span></div>',
    
    placeholder: "Select status"
});

comboBox.appendTo('#comboelement');

// Result:
// ✨ New
// 🔄 In Progress
// 👀 Under Review
// ✅ Done
// 📦 Archived
```

---

### Material Design Icons

```typescript
const actionData: { [key: string]: Object }[] = [
    { id: 'edit', label: 'Edit', icon: 'e-icons e-edit' },
    { id: 'delete', label: 'Delete', icon: 'e-icons e-delete' },
    { id: 'download', label: 'Download', icon: 'e-icons e-download' },
    { id: 'share', label: 'Share', icon: 'e-icons e-share' },
    { id: 'settings', label: 'Settings', icon: 'e-icons e-settings' }
];

const comboBox: ComboBox = new ComboBox({
    dataSource: actionData,
    fields: { text: 'label', value: 'id', iconCss: 'icon' },
    placeholder: "Select action"
});

comboBox.appendTo('#comboelement');

// Need to include Syncfusion icon CSS:
// <link rel="stylesheet" href="node_modules/@syncfusion/ej2-icons/styles/material.css" />
```

---

## Form Integration

### Template-Driven Form

```typescript
import { ComboBox } from '@syncfusion/ej2-dropdowns';

const departmentList = [
    'Engineering',
    'Sales',
    'Marketing',
    'Human Resources',
    'Finance'
];

const comboBox: ComboBox = new ComboBox({
    dataSource: departmentList,
    placeholder: "Select department",
    
    change: (args: any) => {
        console.log('Selected department:', args.value);
        // Update form data
        updateFormData({ department: args.value });
    }
});

comboBox.appendTo('#department');

// Form submission
document.getElementById('submitBtn')?.addEventListener('click', () => {
    const selectedValue = comboBox.value;
    console.log('Form submitted with:', selectedValue);
});
```

---

### Validation on Form

```typescript
const comboBox: ComboBox = new ComboBox({
    dataSource: ['Option A', 'Option B', 'Option C'],
    placeholder: "Required field",
    
    change: (args: any) => {
        if (!args.value) {
            // Show error
            addErrorClass();
        } else {
            // Clear error
            removeErrorClass();
        }
    }
});

comboBox.appendTo('#required-field');

function addErrorClass() {
    document.getElementById('required-field')?.classList.add('e-error');
}

function removeErrorClass() {
    document.getElementById('required-field')?.classList.remove('e-error');
}

// CSS for error state
// .e-error { border-color: #dc3545 !important; }
```

---

## Custom Styling

### CSS Customization

```css
/* Customize wrapper appearance */
.e-ddl.e-input-group.e-control-wrapper .e-input {
    font-size: 14px;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    color: #333;
    background: #f9f9f9;
}

/* Customize dropdown icon */
.e-ddl.e-input-group .e-input-group-icon {
    color: #007bff;
    font-size: 16px;
}

/* Customize focus state */
.e-ddl.e-input-group.e-control-wrapper.e-input-focus::before,
.e-ddl.e-input-group.e-control-wrapper.e-input-focus::after {
    background: #007bff;
}

/* Customize list item hover */
.e-dropdownbase .e-list-item.e-hover {
    background-color: #e7f3ff;
    color: #007bff;
}

/* Customize active item */
.e-dropdownbase .e-list-item.e-active {
    background-color: #007bff;
    color: white;
}

/* Customize placeholder text */
.e-ddl.e-input-group input.e-input::placeholder {
    color: #999;
    font-style: italic;
}

/* Customize disabled state */
.e-ddl.e-input-group .e-input[disabled] {
    background-color: #f5f5f5;
    color: #ccc;
    cursor: not-allowed;
}
```

---

### Apply Custom Class

```typescript
const comboBox: ComboBox = new ComboBox({
    dataSource: ['Option A', 'Option B'],
    cssClass: 'custom-combo',  // Add custom class
    placeholder: "Select..."
});

comboBox.appendTo('#comboelement');

// CSS
// .custom-combo .e-input {
//     font-weight: bold;
//     color: #007bff;
// }
```

---

## Next Steps

- ✅ **Done:** How-to guides covered
- 📖 **Review:** Back to [SKILL.md](../SKILL.md) for complete overview

---

## See Also

- [API: autofill](https://ej2.syncfusion.com/documentation/api/combo-box/#autofill)
- [API: change Event](https://ej2.syncfusion.com/documentation/api/combo-box/#change)
- [API: itemTemplate](https://ej2.syncfusion.com/documentation/api/combo-box/#itemtemplate)
- [Syncfusion Icon CSS](https://ej2.syncfusion.com/documentation/icon/)
