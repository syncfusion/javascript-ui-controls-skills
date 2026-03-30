# Data Binding in ComboBox

## Table of Contents
- [Local Data Arrays](#local-data-arrays)
- [Object Data with Field Mapping](#object-data-with-field-mapping)
- [Nested Object Fields](#nested-object-fields)
- [Remote Data with OData](#remote-data-with-odata)
- [DataManager for Advanced Queries](#datamanager-for-advanced-queries)
- [Dynamic Data Updates](#dynamic-data-updates)
- [Primitive vs Object Values](#primitive-vs-object-values)
- [Common Patterns & Edge Cases](#common-patterns--edge-cases)

## Local Data Arrays

### Simple String Array

Simplest data binding—ComboBox displays and returns string values:

```typescript
import { ComboBox } from '@syncfusion/ej2-dropdowns';

const languageData: string[] = ['JavaScript', 'TypeScript', 'Python', 'C#', 'Java'];

const comboBox: ComboBox = new ComboBox({
    dataSource: languageData,
    placeholder: "Select a language"
});

comboBox.appendTo('#comboelement');

// Get selected value
console.log(comboBox.value);  // Output: "TypeScript" (string)
```

**When to use:** Simple lists with no additional metadata needed.

---

### Number Array

For numeric data:

```typescript
const years: number[] = [2020, 2021, 2022, 2023, 2024, 2025];

const comboBox: ComboBox = new ComboBox({
    dataSource: years,
    placeholder: "Select year",
    value: 2024  // Pre-select 2024
});

comboBox.appendTo('#comboelement');

console.log(comboBox.value);  // Output: 2024 (number)
```

---

## Object Data with Field Mapping

### Basic Object Array

When data contains objects with properties:

```typescript
const employeeData: { [key: string]: Object }[] = [
    { EmpID: 'E001', Name: 'Alice Johnson', Department: 'Engineering' },
    { EmpID: 'E002', Name: 'Bob Smith', Department: 'Sales' },
    { EmpID: 'E003', Name: 'Carol White', Department: 'Marketing' }
];

const comboBox: ComboBox = new ComboBox({
    dataSource: employeeData,
    fields: { text: 'Name', value: 'EmpID' },  // Display Name, return EmpID
    placeholder: "Select employee"
});

comboBox.appendTo('#comboelement');

// User sees: "Alice Johnson", "Bob Smith", "Carol White"
// comboBox.value returns: "E001", "E002", etc.
```

**Field Mapping Rules:**
- `text`: Property to display in the list
- `value`: Property to return as selected value
- Don't need to match—show full name, store ID

---

### Multiple Display Fields

Combine multiple fields for display:

```typescript
const employeeData: { [key: string]: Object }[] = [
    { 
        id: 1,
        firstName: 'Alice',
        lastName: 'Johnson',
        email: 'alice@company.com'
    },
    { 
        id: 2,
        firstName: 'Bob',
        lastName: 'Smith',
        email: 'bob@company.com'
    }
];

const comboBox: ComboBox = new ComboBox({
    dataSource: employeeData,
    fields: { 
        text: 'firstName',  // Display firstName
        value: 'id'         // Return id
    },
    placeholder: "Select employee"
});

comboBox.appendTo('#comboelement');

// For template-based display of multiple fields, see Templates reference
```

---

## Nested Object Fields

### Accessing Nested Properties

Use dot notation to access properties in nested objects:

```typescript
const countryData: { [key: string]: Object }[] = [
    {
        id: 1,
        info: {
            country: 'United States',
            code: 'USA',
            capital: 'Washington DC'
        }
    },
    {
        id: 2,
        info: {
            country: 'India',
            code: 'IND',
            capital: 'New Delhi'
        }
    },
    {
        id: 3,
        info: {
            country: 'United Kingdom',
            code: 'GBR',
            capital: 'London'
        }
    }
];

const comboBox: ComboBox = new ComboBox({
    dataSource: countryData,
    fields: { 
        text: 'info.country',    // Access nested: object.property.property
        value: 'info.code'       // Same syntax for value
    },
    placeholder: "Select country"
});

comboBox.appendTo('#comboelement');

// User sees: "United States", "India", "United Kingdom"
// comboBox.value returns: "USA", "IND", "GBR"
```

**Nested Field Syntax:**
```
'parent.child'              ✅ One level deep
'parent.child.grandchild'   ✅ Multiple levels
'parent[0].name'            ✅ Array access
```

---

### Complex Nested Structures

```typescript
const teamData: { [key: string]: Object }[] = [
    {
        teamId: 'T1',
        team: {
            name: 'Engineering',
            manager: {
                id: 'M1',
                name: 'Alice',
                contact: { email: 'alice@company.com' }
            }
        }
    }
];

const comboBox: ComboBox = new ComboBox({
    dataSource: teamData,
    fields: { 
        text: 'team.manager.name',     // Three levels deep
        value: 'team.manager.id'
    },
    placeholder: "Select team lead"
});

comboBox.appendTo('#comboelement');
```

**Tip:** Keep nesting to 2-3 levels max for performance and maintainability.

---

## Remote Data with OData

### Fetching from OData Service

Use DataManager with ODataV4Adaptor to fetch remote data:

```typescript
import { ComboBox, FilteringEventArgs } from '@syncfusion/ej2-dropdowns';
import { DataManager, Query, ODataV4Adaptor } from '@syncfusion/ej2-data';

const comboBox: ComboBox = new ComboBox({
    dataSource: new DataManager({
        url: 'https://services.odata.org/V4/Northwind/Northwind.svc/Customers',
        adaptor: new ODataV4Adaptor,
        crossDomain: true
    }),
    query: new Query().select(['ContactName', 'CustomerID']).take(10),
    fields: { text: 'ContactName', value: 'CustomerID' },
    placeholder: "Select customer"
});

comboBox.appendTo('#comboelement');
```

**What's happening:**
1. `DataManager` creates connection to OData service
2. `ODataV4Adaptor` handles OData protocol
3. `Query().select()` specifies which columns to fetch
4. `.take(10)` limits to first 10 records
5. `fields` maps remote data to ComboBox

---

### Filtering Remote Data

Implement custom filtering logic when allowFiltering is enabled:

```typescript
import { ComboBox, FilteringEventArgs } from '@syncfusion/ej2-dropdowns';
import { DataManager, Query, ODataV4Adaptor } from '@syncfusion/ej2-data';

const comboBox: ComboBox = new ComboBox({
    dataSource: new DataManager({
        url: 'https://services.odata.org/V4/Northwind/Northwind.svc/Customers',
        adaptor: new ODataV4Adaptor,
        crossDomain: true
    }),
    query: new Query().select(['ContactName', 'CustomerID']).take(10),
    fields: { text: 'ContactName', value: 'CustomerID' },
    allowFiltering: true,
    placeholder: "Search customer...",
    
    // Custom filtering logic
    filtering: (args: FilteringEventArgs) => {
        if (args.text === '') {
            // No search text—show all
            args.updateData(this.dataSource);
        } else {
            // Search text entered—filter server-side
            const query = new Query().select(['ContactName', 'CustomerID'])
                .where('ContactName', 'startswith', args.text, true)
                .take(10);
            
            args.updateData(this.dataSource, query);
        }
    }
});

comboBox.appendTo('#comboelement');
```

**Benefits of server-side filtering:**
- ✅ Scales to millions of records
- ✅ Network bandwidth optimized
- ✅ User sees relevant results quickly

---

## DataManager for Advanced Queries

### Query with Multiple Conditions

Build complex queries using DataManager Query API:

```typescript
import { ComboBox } from '@syncfusion/ej2-dropdowns';
import { DataManager, Query, ODataV4Adaptor, Predicate } from '@syncfusion/ej2-data';

const comboBox: ComboBox = new ComboBox({
    dataSource: new DataManager({
        url: 'https://services.odata.org/V4/Northwind/Northwind.svc/Employees',
        adaptor: new ODataV4Adaptor,
        crossDomain: true
    }),
    
    // Complex query: salary > 50000 AND title = 'Sales Representative'
    query: new Query()
        .select(['FirstName', 'Title', 'EmployeeID'])
        .where('Title', 'equal', 'Sales Representative')
        .where('Salary', 'greaterThan', 50000)
        .orderBy('FirstName'),
    
    fields: { text: 'FirstName', value: 'EmployeeID' },
    placeholder: "Select employee"
});

comboBox.appendTo('#comboelement');
```

**Query Methods:**
- `.select(columns)` - Choose fields to fetch
- `.where(field, operator, value)` - Filter condition
- `.take(n)` - Limit results to n records
- `.skip(n)` - Skip first n records (for pagination)
- `.orderBy(field)` - Sort ascending
- `.orderByDescending(field)` - Sort descending

---

### Grouping Remote Data

Group results by a field:

```typescript
const comboBox: ComboBox = new ComboBox({
    dataSource: new DataManager({
        url: 'https://services.odata.org/V4/Northwind/Northwind.svc/Employees',
        adaptor: new ODataV4Adaptor
    }),
    query: new Query().select(['FirstName', 'City', 'EmployeeID']),
    fields: { 
        text: 'FirstName', 
        value: 'EmployeeID',
        groupBy: 'City'  // Group by city
    },
    placeholder: "Select employee"
});

comboBox.appendTo('#comboelement');

// Result: Employees grouped by city (London, Seattle, etc.)
```

---

## Dynamic Data Updates

### Update Data After Initialization

Change data source after ComboBox is rendered:

```typescript
const comboBox: ComboBox = new ComboBox({
    dataSource: [],  // Start empty
    placeholder: "Select department"
});

comboBox.appendTo('#comboelement');

// Later, fetch and update data
function loadDepartments() {
    const departments = ['Engineering', 'Sales', 'Marketing', 'HR'];
    
    comboBox.dataSource = departments;
    comboBox.refresh();  // Re-render with new data
}

// Call when needed
setTimeout(() => loadDepartments(), 2000);
```

**Important:** Always call `.refresh()` after updating dataSource.

---

### Cascade Updates (Dependent Dropdowns)

Update second ComboBox based on first ComboBox selection:

```typescript
const countryData = [
    { id: 'USA', name: 'United States' },
    { id: 'IND', name: 'India' }
];

const stateData: { [key: string]: string[] } = {
    'USA': ['California', 'Texas', 'New York'],
    'IND': ['Tamil Nadu', 'Karnataka', 'Maharashtra']
};

// First ComboBox - Country
const countryCombo: ComboBox = new ComboBox({
    dataSource: countryData,
    fields: { text: 'name', value: 'id' },
    change: (args: any) => {
        const selectedCountry = args.value;
        // Update state ComboBox
        stateCombo.dataSource = stateData[selectedCountry] || [];
        stateCombo.refresh();
        stateCombo.value = '';  // Clear previous selection
    },
    placeholder: "Select country"
});

countryCombo.appendTo('#country');

// Second ComboBox - State (initially empty)
const stateCombo: ComboBox = new ComboBox({
    dataSource: [],
    placeholder: "Select state"
});

stateCombo.appendTo('#state');
```

**Workflow:**
1. User selects country from first ComboBox
2. `change` event fires
3. Second ComboBox updates with states for that country
4. `.refresh()` re-renders dropdown
5. `.value = ''` clears previous selection

---

## Primitive vs Object Values

### Primitive Values (Strings, Numbers)

```typescript
// Selected value is a primitive
const comboBox: ComboBox = new ComboBox({
    dataSource: ['Item1', 'Item2', 'Item3'],
    value: 'Item1'
});

comboBox.appendTo('#element');

console.log(typeof comboBox.value);  // string
console.log(comboBox.value);         // "Item1"
```

**Use when:** Simple data, IDs, or display text are all you need.

---

### Object Values

```typescript
interface Employee {
    id: number;
    name: string;
}

const employeeData: Employee[] = [
    { id: 1, name: 'Alice' },
    { id: 2, name: 'Bob' }
];

const comboBox: ComboBox = new ComboBox({
    dataSource: employeeData,
    fields: { text: 'name', value: 'id' },
    allowObjectBinding: true,  // Enable object value binding
    value: { id: 1, name: 'Alice' }  // Pre-select object
});

comboBox.appendTo('#element');

console.log(typeof comboBox.value);  // object
console.log(comboBox.value.id);      // 1
console.log(comboBox.value.name);    // "Alice"
```

**When to use:** Need full object data for form submission or complex logic.

---

## Common Patterns & Edge Cases

### Pattern: Pre-select with Dynamic Data

Problem: Data loads asynchronously, but you want to pre-select an item.

```typescript
const comboBox: ComboBox = new ComboBox({
    placeholder: "Select option"
});

comboBox.appendTo('#element');

// Fetch data asynchronously
async function loadData() {
    const data = await fetch('/api/items').then(r => r.json());
    
    comboBox.dataSource = data;
    comboBox.refresh();
    
    // Pre-select after data loads
    comboBox.value = data[0].id;
}

loadData();
```

---

### Issue: Empty Data Shows "No Records"

When dataSource is empty and user opens dropdown:

```typescript
const comboBox: ComboBox = new ComboBox({
    dataSource: [],
    noRecordsTemplate: '<span>No items available</span>',
    placeholder: "Select item"
});

comboBox.appendTo('#element');
```

---

### Issue: Null or Undefined Values

Handle missing data gracefully:

```typescript
const data = [
    { id: 1, name: 'Alice' },
    { id: 2, name: null },      // Missing name
    { id: 3, name: undefined }   // Undefined name
];

const comboBox: ComboBox = new ComboBox({
    dataSource: data,
    fields: { text: 'name', value: 'id' },
    placeholder: "Select"
});

// Use itemTemplate to handle nulls
// See Templates & Grouping reference
```

---

## Next Steps

- ✅ **Done:** Data binding with local and remote sources
- 📖 **Next:** [Filtering & Search](../filtering-and-search.md) - Add search to your data
- 🎨 **Then:** [Templates & Grouping](../templates-and-grouping.md) - Format data display
- ⚙️ **Advanced:** [Advanced Features](../advanced-features.md) - Virtual scroll, events

---

## See Also

- [Query API Documentation](https://ej2.syncfusion.com/documentation/data/getting-started/)
- [DataManager Overview](https://ej2.syncfusion.com/documentation/data/data-binding/)
- [API Reference: Fields](https://ej2.syncfusion.com/documentation/api/combo-box/#fields)
