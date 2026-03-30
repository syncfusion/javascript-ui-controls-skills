# Templates & Grouping in ComboBox

## Table of Contents
- [Item Templates](#item-templates)
- [Group Templates](#group-templates)
- [Header and Footer Templates](#header-and-footer-templates)
- [Grouping Data](#grouping-data)
- [No Records Template](#no-records-template)
- [Template Patterns](#template-patterns)
- [Performance Optimization](#performance-optimization)

## Item Templates

### Customizing List Item Display

The `itemTemplate` property allows custom HTML rendering for each list item:

```typescript
import { ComboBox } from '@syncfusion/ej2-dropdowns';

const employeeData: { [key: string]: Object }[] = [
    { id: 1, name: 'Alice Johnson', department: 'Engineering' },
    { id: 2, name: 'Bob Smith', department: 'Sales' },
    { id: 3, name: 'Carol White', department: 'Marketing' }
];

const comboBox: ComboBox = new ComboBox({
    dataSource: employeeData,
    fields: { text: 'name', value: 'id' },
    
    // Template: Display name in bold + department in gray
    itemTemplate: '<div><strong>${name}</strong> <span style="color: #999;">- ${department}</span></div>',
    
    placeholder: "Select employee"
});

comboBox.appendTo('#comboelement');
```

**Template Syntax:**
- `${fieldName}` - Insert field value
- HTML allowed for styling
- Can combine multiple fields

**Result:** Each dropdown item shows:
```
Alice Johnson - Engineering
Bob Smith - Sales
Carol White - Marketing
```

---

### Template with Formatting

Add formatting and conditional styling:

```typescript
const productData: { [key: string]: Object }[] = [
    { id: 1, name: 'Laptop', price: 999.99, inStock: true },
    { id: 2, name: 'Mouse', price: 19.99, inStock: false },
    { id: 3, name: 'Keyboard', price: 79.99, inStock: true }
];

const comboBox: ComboBox = new ComboBox({
    dataSource: productData,
    fields: { text: 'name', value: 'id' },
    
    itemTemplate: `
        <div style="padding: 8px; border-bottom: 1px solid #eee;">
            <div style="font-weight: bold;">${name}</div>
            <div style="font-size: 12px; color: #666;">
                $\${price} 
                <span style="color: ${inStock ? 'green' : 'red'}; margin-left: 10px;">
                    ${inStock ? '✓ In Stock' : '✗ Out of Stock'}
                </span>
            </div>
        </div>
    `,
    
    placeholder: "Select product"
});

comboBox.appendTo('#comboelement');
```

**Result:** Product items display with:
- Product name (bold)
- Price
- Stock status (color-coded green/red)

---

### Complex Item Template with Images

```typescript
const countryData: { [key: string]: Object }[] = [
    { id: 'USA', name: 'United States', flag: '🇺🇸' },
    { id: 'IND', name: 'India', flag: '🇮🇳' },
    { id: 'GBR', name: 'United Kingdom', flag: '🇬🇧' },
    { id: 'JPN', name: 'Japan', flag: '🇯🇵' }
];

const comboBox: ComboBox = new ComboBox({
    dataSource: countryData,
    fields: { text: 'name', value: 'id' },
    
    itemTemplate: '<div style="display: flex; align-items: center; gap: 10px;"><span style="font-size: 20px;">${flag}</span><span>${name}</span></div>',
    
    placeholder: "Select country"
});

comboBox.appendTo('#comboelement');
```

**Result:** Each item shows flag emoji + country name

---

## Group Templates

### Customizing Group Headers

The `groupTemplate` property customizes category headers:

```typescript
import { ComboBox } from '@syncfusion/ej2-dropdowns';

const employeeData: { [key: string]: Object }[] = [
    { name: 'Alice', department: 'Engineering' },
    { name: 'Bob', department: 'Engineering' },
    { name: 'Carol', department: 'Sales' },
    { name: 'Dave', department: 'Sales' },
    { name: 'Eve', department: 'Marketing' }
];

const comboBox: ComboBox = new ComboBox({
    dataSource: employeeData,
    fields: { text: 'name', value: 'name', groupBy: 'department' },
    
    // Style group headers
    groupTemplate: '<div style="background: #f5f5f5; padding: 8px; font-weight: bold; color: #333;">${department}</div>',
    
    placeholder: "Select employee"
});

comboBox.appendTo('#comboelement');

// Result:
// [Engineering]
//   - Alice
//   - Bob
// [Sales]
//   - Carol
//   - Dave
// [Marketing]
//   - Eve
```

---

### Group Template with Count

Show number of items in each group:

```typescript
import { ComboBox, FilteringEventArgs } from '@syncfusion/ej2-dropdowns';

const employeeData: { [key: string]: Object }[] = [
    { name: 'Alice', department: 'Engineering' },
    { name: 'Bob', department: 'Engineering' },
    { name: 'Carol', department: 'Sales' },
    { name: 'Dave', department: 'Sales' },
    { name: 'Eve', department: 'Marketing' }
];

const comboBox: ComboBox = new ComboBox({
    dataSource: employeeData,
    fields: { text: 'name', value: 'name', groupBy: 'department' },
    
    groupTemplate: `<div style="display: flex; justify-content: space-between; background: #f5f5f5; padding: 8px; font-weight: bold;">
        <span>${department}</span>
        <span style="font-size: 12px; color: #999;">(${count} employees)</span>
    </div>`,
    
    placeholder: "Select employee"
});

comboBox.appendTo('#comboelement');
```

**Note:** The `${count}` variable shows group item count.

---

## Header and Footer Templates

### Header Template

Display header above the list (static):

```typescript
const comboBox: ComboBox = new ComboBox({
    dataSource: ['JavaScript', 'TypeScript', 'Python', 'Java'],
    
    headerTemplate: '<div style="padding: 10px; background: #007bff; color: white; font-weight: bold;">Popular Languages</div>',
    
    placeholder: "Select language"
});

comboBox.appendTo('#comboelement');
```

**Result:** "Popular Languages" header appears above the list.

---

### Footer Template

Display footer below the list (static):

```typescript
const languages = ['JavaScript', 'TypeScript', 'Python', 'Java', 'C#', 'Ruby'];

const comboBox: ComboBox = new ComboBox({
    dataSource: languages,
    
    footerTemplate: `<div style="padding: 10px; background: #f5f5f5; text-align: center; border-top: 1px solid #ddd; color: #666; font-size: 12px;">Total: ${languages.length} languages</div>`,
    
    placeholder: "Select language"
});

comboBox.appendTo('#comboelement');
```

**Result:** "Total: 6 languages" footer appears below the list.

---

### Combined Header + Item + Footer

Complete template setup:

```typescript
const comboBox: ComboBox = new ComboBox({
    dataSource: ['Option A', 'Option B', 'Option C'],
    
    headerTemplate: '<div style="padding: 10px; background: #007bff; color: white;">Select an Option</div>',
    
    itemTemplate: '<div style="padding: 8px; border-bottom: 1px solid #eee;"><strong>${}</strong></div>',
    
    footerTemplate: '<div style="padding: 8px; background: #f5f5f5; font-size: 12px; color: #666;">You have 3 choices</div>',
    
    placeholder: "Choose..."
});

comboBox.appendTo('#comboelement');
```

---

## Grouping Data

### Group by Single Field

Use `groupBy` field property to group items:

```typescript
const employeeData: { [key: string]: Object }[] = [
    { name: 'Alice', city: 'New York' },
    { name: 'Bob', city: 'Los Angeles' },
    { name: 'Carol', city: 'New York' },
    { name: 'Dave', city: 'Chicago' },
    { name: 'Eve', city: 'Los Angeles' }
];

const comboBox: ComboBox = new ComboBox({
    dataSource: employeeData,
    fields: { text: 'name', value: 'name', groupBy: 'city' },
    placeholder: "Select employee"
});

comboBox.appendTo('#comboelement');

// Result (automatically grouped):
// [Chicago]
//   - Dave
// [Los Angeles]
//   - Bob
//   - Eve
// [New York]
//   - Alice
//   - Carol
```

**No template needed—grouping is automatic!**

---

### Group with Remote Data

```typescript
import { ComboBox } from '@syncfusion/ej2-dropdowns';
import { DataManager, Query, ODataV4Adaptor } from '@syncfusion/ej2-data';

const comboBox: ComboBox = new ComboBox({
    dataSource: new DataManager({
        url: 'https://services.odata.org/V4/Northwind/Northwind.svc/Employees',
        adaptor: new ODataV4Adaptor
    }),
    query: new Query().select(['FirstName', 'City']),
    fields: { text: 'FirstName', value: 'FirstName', groupBy: 'City' },
    placeholder: "Select employee by city"
});

comboBox.appendTo('#comboelement');

// Groups employees from OData server by city
```

---

## No Records Template

### Empty Data Display

Custom message when no items available:

```typescript
const comboBox: ComboBox = new ComboBox({
    dataSource: [],  // Empty initially
    
    noRecordsTemplate: '<div style="padding: 20px; text-align: center; color: #999;">No items found</div>',
    
    placeholder: "Select item"
});

comboBox.appendTo('#comboelement');

// User sees: "No items found" message
```

---

### Dynamic Empty Message

```typescript
const comboBox: ComboBox = new ComboBox({
    dataSource: [],
    
    noRecordsTemplate: `
        <div style="padding: 20px; text-align: center;">
            <div style="color: #999; margin-bottom: 10px;">No results found</div>
            <button style="padding: 5px 10px; background: #007bff; color: white; border: none; cursor: pointer;">
                Load More
            </button>
        </div>
    `,
    
    placeholder: "Search..."
});

comboBox.appendTo('#comboelement');
```

---

### Action Failure Template

Display when data fetch fails:

```typescript
const comboBox: ComboBox = new ComboBox({
    dataSource: new DataManager({
        url: 'https://invalid-api.example.com/data',  // Intentionally wrong URL
        adaptor: new ODataV4Adaptor
    }),
    
    actionFailureTemplate: '<div style="padding: 20px; text-align: center; color: #dc3545;">Failed to load data. Please try again.</div>',
    
    placeholder: "Select..."
});

comboBox.appendTo('#comboelement');

// If server error occurs, user sees failure message
```

---

## Template Patterns

### Pattern 1: Show Extra Info with Icon

```typescript
const taskData: { [key: string]: Object }[] = [
    { id: 1, title: 'Task A', priority: 'High', icon: '⚠️' },
    { id: 2, title: 'Task B', priority: 'Medium', icon: '📌' },
    { id: 3, title: 'Task C', priority: 'Low', icon: '✓' }
];

const comboBox: ComboBox = new ComboBox({
    dataSource: taskData,
    fields: { text: 'title', value: 'id' },
    
    itemTemplate: '<div style="display: flex; gap: 8px; align-items: center;"><span style="font-size: 18px;">${icon}</span><div><div>${title}</div><div style="font-size: 11px; color: #999;">Priority: ${priority}</div></div></div>',
    
    placeholder: "Select task"
});

comboBox.appendTo('#comboelement');
```

---

### Pattern 2: Color-Coded Status

```typescript
const orderData: { [key: string]: Object }[] = [
    { id: 1, orderNo: 'ORD-001', status: 'Pending', color: '#ffc107' },
    { id: 2, orderNo: 'ORD-002', status: 'Confirmed', color: '#28a745' },
    { id: 3, orderNo: 'ORD-003', status: 'Cancelled', color: '#dc3545' }
];

const comboBox: ComboBox = new ComboBox({
    dataSource: orderData,
    fields: { text: 'orderNo', value: 'id' },
    
    itemTemplate: '<div style="display: flex; justify-content: space-between; align-items: center; padding: 5px;"><span>${orderNo}</span><span style="padding: 3px 8px; background: ${color}; color: white; border-radius: 3px; font-size: 12px;">${status}</span></div>',
    
    placeholder: "Select order"
});

comboBox.appendTo('#comboelement');
```

---

### Pattern 3: Two-Column Layout (Employee + Salary)

```typescript
const employeeData: { [key: string]: Object }[] = [
    { id: 1, name: 'Alice', salary: '$85,000' },
    { id: 2, name: 'Bob', salary: '$92,000' },
    { id: 3, name: 'Carol', salary: '$78,000' }
];

const comboBox: ComboBox = new ComboBox({
    dataSource: employeeData,
    fields: { text: 'name', value: 'id' },
    
    headerTemplate: '<div style="display: flex; justify-content: space-between; padding: 8px; background: #f5f5f5; font-weight: bold; border-bottom: 2px solid #ddd;"><span style="width: 50%;">Employee</span><span style="width: 50%; text-align: right;">Salary</span></div>',
    
    itemTemplate: '<div style="display: flex; justify-content: space-between; padding: 8px; border-bottom: 1px solid #eee;"><span style="width: 50%;">${name}</span><span style="width: 50%; text-align: right; color: #666;">${salary}</span></div>',
    
    placeholder: "Select employee"
});

comboBox.appendTo('#comboelement');
```

---

## Performance Optimization

### Template Best Practices

**✅ DO:**
- Keep templates simple for large lists
- Use CSS classes instead of inline styles when possible
- Cache template functions if complex

**❌ DON'T:**
- Run heavy computations in templates
- Make async calls in templates
- Use complex nesting or too much HTML

---

### Virtual Scrolling with Templates

For large datasets, use virtual scrolling with templates:

```typescript
const largeDataset: { [key: string]: Object }[] = 
    Array.from({ length: 10000 }, (_, i) => ({
        id: i + 1,
        name: `Item ${i + 1}`
    }));

const comboBox: ComboBox = new ComboBox({
    dataSource: largeDataset,
    fields: { text: 'name', value: 'id' },
    
    itemTemplate: '<div style="padding: 5px;">${name}</div>',
    
    enableVirtualization: true,  // Virtual scrolling enabled
    
    placeholder: "Select item (virtual scroll)"
});

comboBox.appendTo('#comboelement');

// Only visible items rendered—performance optimized for 10k items
```

See: [Advanced Features - Virtual Scrolling](../advanced-features.md#virtual-scrolling)

---

## Next Steps

- ✅ **Done:** Templates and grouping
- 📖 **Next:** [Advanced Features](../advanced-features.md) - Virtual scroll, events, RTL

---

## See Also

- [API: itemTemplate](https://ej2.syncfusion.com/documentation/api/combo-box/#itemtemplate)
- [API: groupTemplate](https://ej2.syncfusion.com/documentation/api/combo-box/#grouptemplate)
- [Template Engine Documentation](https://ej2.syncfusion.com/documentation/common/template-engine/)
