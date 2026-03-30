# Data Binding in ListBox

## Table of Contents
- [Overview](#overview)
- [Local Data Sources](#local-data-sources)
- [Remote Data Sources](#remote-data-sources)
- [HTML Element Initialization](#html-element-initialization)
- [Field Mapping](#field-mapping)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

The ListBox binds data via the `dataSource` property, which accepts:
- **Array of primitives** (strings, numbers)
- **Array of objects** (with field mapping)
- **DataManager instance** (for remote data)
- **HTML elements** (SELECT, UL)

The `fields` property maps object properties to ListBox display and value fields.

## Local Data Sources

### Array of Strings

Simplest data binding for primitive values:

```typescript
import { ListBox } from '@syncfusion/ej2-dropdowns';

const sportsData: string[] = [
    'Badminton',
    'Cricket',
    'Football',
    'Golf',
    'Tennis',
    'Basketball'
];

const listObj: ListBox = new ListBox({
    dataSource: sportsData
});

listObj.appendTo('#listbox');
```

**Result**: Both `text` (displayed) and `value` (internal) use the string directly. No field mapping needed.

### Array of Objects

Bind object arrays by mapping properties to fields:

```typescript
import { ListBox } from '@syncfusion/ej2-dropdowns';

interface Sport {
    id: string;
    name: string;
}

const sportsData: Sport[] = [
    { id: 'game1', name: 'Badminton' },
    { id: 'game2', name: 'Cricket' },
    { id: 'game3', name: 'Football' },
    { id: 'game4', name: 'Golf' },
    { id: 'game5', name: 'Tennis' }
];

const listObj: ListBox = new ListBox({
    dataSource: sportsData,
    fields: {
        text: 'name',      // Display field
        value: 'id'        // Hidden value field
    }
});

listObj.appendTo('#listbox');
```

**Result**: Lists show "Badminton", "Cricket", etc., with corresponding ID values stored internally.

### Complex Nested Objects

Access nested properties using dot notation:

```typescript
import { ListBox } from '@syncfusion/ej2-dropdowns';

interface Employee {
    empId: string;
    details: {
        name: string;
        department: string;
    };
}

const employeeData: Employee[] = [
    { empId: 'E001', details: { name: 'John Smith', department: 'Engineering' } },
    { empId: 'E002', details: { name: 'Sarah Jones', department: 'Marketing' } },
    { empId: 'E003', details: { name: 'Mike Brown', department: 'Sales' } }
];

const listObj: ListBox = new ListBox({
    dataSource: employeeData,
    fields: {
        text: 'details.name',           // Nested property
        value: 'empId'
    }
});

listObj.appendTo('#listbox');
```

**Result**: Shows "John Smith", "Sarah Jones", etc., with employee IDs as values.

**Important**: When binding complex data, map fields correctly. Otherwise selected items may return undefined.

## Remote Data Sources

### DataManager with OData V4

Fetch data from a remote API:

```typescript
import { ListBox } from '@syncfusion/ej2-dropdowns';
import { Query, DataManager, ODataV4Adaptor } from '@syncfusion/ej2-data';

const listObj: ListBox = new ListBox({
    dataSource: new DataManager({
        url: 'https://services.odata.org/V4/Northwind/Northwind.svc/',
        adaptor: new ODataV4Adaptor()
    }),
    query: new Query()
        .from('Products')
        .select('ProductID, ProductName')
        .take(10),
    fields: {
        text: 'ProductName',
        value: 'ProductID'
    }
});

listObj.appendTo('#listbox');
```

**Result**: Loads first 10 products from Northwind API.

### DataManager with Custom Web API

Connect to your own API endpoint:

```typescript
import { ListBox } from '@syncfusion/ej2-dropdowns';
import { Query, DataManager } from '@syncfusion/ej2-data';

const listObj: ListBox = new ListBox({
    dataSource: new DataManager({
        url: 'https://api.example.com/api/items',
        adaptor: new ODataV4Adaptor()
    }),
    query: new Query()
        .select(['id', 'label', 'category'])
        .where('active', 'equal', true),
    fields: {
        text: 'label',
        value: 'id',
        groupBy: 'category'
    }
});

listObj.appendTo('#listbox');
```

**Result**: Fetches active items from API, grouped by category.

### DataManager with URL Encoding Adaptor

For simple REST APIs without advanced querying:

```typescript
import { ListBox } from '@syncfusion/ej2-dropdowns';
import { Query, DataManager, UrlAdaptor } from '@syncfusion/ej2-data';

const listObj: ListBox = new ListBox({
    dataSource: new DataManager({
        url: 'https://api.example.com/api/countries',
        adaptor: new UrlAdaptor()
    }),
    fields: {
        text: 'countryName',
        value: 'countryCode'
    }
});

listObj.appendTo('#listbox');
```

## HTML Element Initialization

### SELECT Element

Initialize from HTML SELECT with OPTION tags:

```html
<select id="listbox">
    <option value="val1">Item 1</option>
    <option value="val2">Item 2</option>
    <option value="val3">Item 3</option>
</select>
```

```typescript
import { ListBox } from '@syncfusion/ej2-dropdowns';

const listObj: ListBox = new ListBox();
listObj.appendTo('#listbox');
```

**Result**: Displays items from SELECT element. No dataSource needed.

### UL Element

Initialize from unordered list:

```html
<ul id="listbox">
    <li>Badminton</li>
    <li>Cricket</li>
    <li>Football</li>
    <li>Golf</li>
    <li>Tennis</li>
</ul>
```

```typescript
import { ListBox } from '@syncfusion/ej2-dropdowns';

const listObj: ListBox = new ListBox();
listObj.appendTo('#listbox');
```

**Result**: Converts UL/LI into ListBox. Inner text acts as both display and value.

### SELECT with OPTGROUP

Group related items using optgroup:

```html
<select id="listbox">
    <optgroup label="Fruits">
        <option value="apple">Apple</option>
        <option value="banana">Banana</option>
    </optgroup>
    <optgroup label="Vegetables">
        <option value="carrot">Carrot</option>
        <option value="broccoli">Broccoli</option>
    </optgroup>
</select>
```

```typescript
import { ListBox } from '@syncfusion/ej2-dropdowns';

const listObj: ListBox = new ListBox();
listObj.appendTo('#listbox');
```

**Result**: Items displayed in groups.

## Field Mapping

### FieldSettings Properties

| Property | Type | Description | Example |
|----------|------|-------------|---------|
| `text` | string | Display property | `'name'` |
| `value` | string | Hidden value property | `'id'` |
| `groupBy` | string | Grouping property | `'department'` |
| `iconCss` | string | CSS class for icons | `'iconClass'` |
| `htmlAttributes` | string | HTML attributes property | `'attributes'` |

### Basic Field Mapping

```typescript
fields: {
    text: 'displayName',
    value: 'uniqueId'
}
```

### With Grouping and Icons

```typescript
fields: {
    text: 'itemName',
    value: 'itemId',
    groupBy: 'category',
    iconCss: 'itemIcon'
}
```

### Example with Icons

```typescript
const itemsData = [
    { id: '1', name: 'Item 1', icon: 'icon-star', category: 'A' },
    { id: '2', name: 'Item 2', icon: 'icon-heart', category: 'B' }
];

const listObj: ListBox = new ListBox({
    dataSource: itemsData,
    fields: {
        text: 'name',
        value: 'id',
        iconCss: 'icon',
        groupBy: 'category'
    }
});

listObj.appendTo('#listbox');
```

## Best Practices

### 1. Always Map Fields for Objects

**Correct**:
```typescript
fields: { text: 'name', value: 'id' }
```

**Incorrect** (will show "[object Object]"):
```typescript
// Missing field mapping
```

### 2. Use Consistent Data Types

Ensure value field contains consistent data types:

```typescript
// Good - all IDs are strings
const data = [
    { id: '1', name: 'Item 1' },
    { id: '2', name: 'Item 2' }
];

// Risky - mixed types
const data = [
    { id: 1, name: 'Item 1' },
    { id: '2', name: 'Item 2' }
];
```

### 3. Handle Async Data Loading

```typescript
// Load data asynchronously
async function loadItems() {
    const response = await fetch('/api/items');
    const data = await response.json();
    
    const listObj: ListBox = new ListBox({
        dataSource: data,
        fields: { text: 'name', value: 'id' }
    });
    
    listObj.appendTo('#listbox');
}

loadItems();
```

### 4. Validate Remote Data Structure

Verify API response matches expected field names:

```typescript
// API returns: { ProductID, ProductName, ... }
fields: {
    text: 'ProductName',    // Must match API response
    value: 'ProductID'
}
```

## Troubleshooting

### Issue: Selected item returns undefined

**Cause**: Field mapping incorrect or missing.

**Solution**: Verify `value` field maps to actual property:
```typescript
// Wrong
fields: { text: 'name', value: 'invalidField' }

// Correct
fields: { text: 'name', value: 'id' }
```

### Issue: Remote data not loading

**Cause**: API error, CORS issue, or network timeout.

**Solution**:
1. Check API endpoint is accessible
2. Verify CORS headers if cross-origin
3. Check DataManager URL syntax
4. Use browser DevTools to inspect network requests

### Issue: "[object Object]" displayed in list

**Cause**: Object data without field mapping or text field mapping to object.

**Solution**: Ensure fields map to primitive properties:
```typescript
// Wrong
fields: { text: 'nestedObject' }

// Correct
fields: { text: 'nestedObject.name' }
```

### Issue: Grouping shows all items in one group

**Cause**: GroupBy field values are inconsistent or missing.

**Solution**: Ensure groupBy property exists and has consistent values:
```typescript
const data = [
    { name: 'Item 1', category: 'A' },
    { name: 'Item 2', category: 'A' },
    { name: 'Item 3', category: 'B' }
];

fields: { groupBy: 'category' }
```

## Next Steps

- **Selection**: See [selection-and-checkboxes.md](selection-and-checkboxes.md) to handle user selections
- **Sorting & Grouping**: See [sorting-and-grouping.md](sorting-and-grouping.md) for advanced organization
- **Accessibility**: See [accessibility-and-keyboard.md](accessibility-and-keyboard.md) for WCAG compliance
