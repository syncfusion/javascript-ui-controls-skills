# Filtering & Search in ComboBox

## Table of Contents
- [Enable Filtering](#enable-filtering)
- [Filter Types](#filter-types)
- [Case Sensitivity](#case-sensitivity)
- [Diacritics Filtering](#diacritics-filtering)
- [Debounce Delay](#debounce-delay)
- [Minimum Character Requirement](#minimum-character-requirement)
- [Custom Filtering Logic](#custom-filtering-logic)
- [Remote Filtering](#remote-filtering)
- [Troubleshooting](#troubleshooting)

## Enable Filtering

### Basic Filtering Setup

Enable the search box by setting `allowFiltering: true`:

```typescript
import { ComboBox } from '@syncfusion/ej2-dropdowns';

const comboBox: ComboBox = new ComboBox({
    dataSource: ['California', 'Florida', 'Georgia', 'Illinois', 'New York', 'Texas'],
    allowFiltering: true,          // Enable search box
    placeholder: "Search states..."
});

comboBox.appendTo('#comboelement');
```

**Result:** ComboBox now shows a search input. User types to filter the list in real-time.

**Default Behavior:**
- Filter type: `startswith` (matches from beginning)
- Case-insensitive search
- Filters local data on the client

---

## Filter Types

### Available Filter Strategies

| Type | Behavior | Example |
|------|----------|---------|
| `startswith` | Match from beginning | Type "cal" → matches "California" |
| `endswith` | Match from end | Type "ia" → matches "California" |
| `contains` | Match anywhere | Type "for" → matches "California", "Florida" |
| `equal` | Exact match | Type "Texas" → matches "Texas" only |

### Using Different Filter Types

```typescript
import { ComboBox, FilteringEventArgs } from '@syncfusion/ej2-dropdowns';
import { Query, DataManager } from '@syncfusion/ej2-data';

const stateData = ['Alabama', 'Arizona', 'California', 'Colorado', 'Connecticut'];

const comboBox: ComboBox = new ComboBox({
    dataSource: stateData,
    allowFiltering: true,
    
    filtering: (args: FilteringEventArgs) => {
        // Get filtered data with custom filter type
        if (args.text !== '') {
            // Filter: endswith "ia"
            const query = new Query().where('', 'endswith', args.text, true);
            const filteredData = new DataManager(stateData).executeLocal(query);
            args.updateData(filteredData);
        } else {
            args.updateData(stateData);
        }
    },
    placeholder: "Search state..."
});

comboBox.appendTo('#comboelement');
```

---

### startsWith (Default)

```typescript
const comboBox: ComboBox = new ComboBox({
    dataSource: ['Badminton', 'Basketball', 'Cricket', 'Football'],
    allowFiltering: true,
    placeholder: "Search sport..."
    // Default is startswith - no need to configure
});

comboBox.appendTo('#comboelement');

// User types: "ba"
// Shows: "Badminton", "Basketball"
```

---

### contains (Anywhere Match)

```typescript
import { ComboBox, FilteringEventArgs } from '@syncfusion/ej2-dropdowns';
import { Query, DataManager } from '@syncfusion/ej2-data';

const comboBox: ComboBox = new ComboBox({
    dataSource: ['Badminton', 'Basketball', 'Cricket', 'Football'],
    allowFiltering: true,
    
    filtering: (args: FilteringEventArgs) => {
        if (args.text === '') {
            args.updateData(['Badminton', 'Basketball', 'Cricket', 'Football']);
        } else {
            const query = new Query().where('', 'contains', args.text, true);
            const result = new DataManager(['Badminton', 'Basketball', 'Cricket', 'Football'])
                .executeLocal(query);
            args.updateData(result);
        }
    },
    placeholder: "Search sport..."
});

comboBox.appendTo('#comboelement');

// User types: "ball"
// Shows: "Basketball", "Football" (matches contain "ball")
```

---

## Case Sensitivity

### Case-Insensitive (Default)

By default, filtering ignores letter case:

```typescript
const comboBox: ComboBox = new ComboBox({
    dataSource: ['JavaScript', 'TypeScript', 'Python'],
    allowFiltering: true,
    placeholder: "Search..."
});

comboBox.appendTo('#comboelement');

// User types: "java"
// Shows: "JavaScript" (case-insensitive)

// User types: "JAVA"
// Shows: "JavaScript" (still matches)
```

---

### Case-Sensitive Filtering

For strict case matching:

```typescript
import { ComboBox, FilteringEventArgs } from '@syncfusion/ej2-dropdowns';
import { Query, DataManager } from '@syncfusion/ej2-data';

const languages = ['JavaScript', 'TypeScript', 'Python', 'Ruby', 'Rust'];

const comboBox: ComboBox = new ComboBox({
    dataSource: languages,
    allowFiltering: true,
    
    filtering: (args: FilteringEventArgs) => {
        const query = new Query().where('', 'startswith', args.text, false);  // false = case-sensitive
        const result = new DataManager(languages).executeLocal(query);
        args.updateData(result);
    },
    placeholder: "Search (case-sensitive)..."
});

comboBox.appendTo('#comboelement');

// User types: "java"
// Shows: Nothing (no match—requires "Java" or "JavaScript")

// User types: "Java"
// Shows: "JavaScript"
```

**The 4th parameter in `.where()` controls case sensitivity:**
- `true` = case-insensitive (default)
- `false` = case-sensitive

---

## Diacritics Filtering

### Accent-Insensitive Search

For international text with accents (é, à, ü, ñ, etc.):

```typescript
import { ComboBox } from '@syncfusion/ej2-dropdowns';

const sportList: string[] = [
    'Aeróbics',
    'Aeróbics en Agua',
    'Aerografía',
    'Aeromodelaje',
    'Águilas',
    'Ajedrez',
    'Ala Delta',
    'Álbumes de Música'
];

const comboBox: ComboBox = new ComboBox({
    dataSource: sportList,
    allowFiltering: true,
    ignoreAccent: true,  // Ignore accents when filtering
    placeholder: "Search sport... (accents ignored)"
});

comboBox.appendTo('#comboelement');

// User types: "aero"
// Shows: "Aeróbics", "Aeróbics en Agua", "Aerografía", "Aeromodelaje"
// (Accented characters treated as regular characters)

// User types: "aguila"
// Shows: "Águilas" (accent ignored in comparison)
```

**When to use:** International applications with accented characters (Spanish, French, Portuguese, etc.).

---

## Debounce Delay

### Optimize Remote Requests

When filtering remote data, add delay to reduce server calls:

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
    debounceDelay: 300,  // Wait 300ms before fetching
    placeholder: "Search customer..."
});

comboBox.appendTo('#comboelement');
```

**How it works:**
1. User types "a" → wait 300ms
2. User types "al" → still waiting
3. User types "ali" → still waiting  
4. User stops typing (pauses >300ms) → server request sent
5. User types "aliceXYZ" → cancel previous, wait 300ms again

**Benefits:**
- ✅ Reduces network traffic
- ✅ Improves server performance
- ✅ Better user experience (less lag)

**Default:** 300ms (can be 0 to disable)

---

## Minimum Character Requirement

### Require Minimum Characters Before Filtering

Don't search until user types at least N characters:

```typescript
import { ComboBox, FilteringEventArgs } from '@syncfusion/ej2-dropdowns';
import { DataManager, Query, ODataV4Adaptor } from '@syncfusion/ej2-data';

const customerData = new DataManager({
    url: 'https://services.odata.org/V4/Northwind/Northwind.svc/Customers',
    adaptor: new ODataV4Adaptor,
    crossDomain: true
});

const comboBox: ComboBox = new ComboBox({
    dataSource: customerData,
    query: new Query().select(['ContactName', 'CustomerID']),
    fields: { text: 'ContactName', value: 'CustomerID' },
    allowFiltering: true,
    debounceDelay: 300,
    
    filtering: (args: FilteringEventArgs) => {
        // Require at least 3 characters
        if (args.text.length < 3) {
            return;  // Don't filter yet
        }
        
        // Search only after 3+ characters
        if (args.text === '') {
            args.updateData(customerData);
        } else {
            const query = new Query()
                .select(['ContactName', 'CustomerID'])
                .where('ContactName', 'startswith', args.text, true)
                .take(10);
            args.updateData(customerData, query);
        }
    },
    placeholder: "Type at least 3 characters..."
});

comboBox.appendTo('#comboelement');
```

**Workflow:**
1. User types "a" → nothing happens
2. User types "al" → still nothing
3. User types "ali" → server request sent, results show
4. User types "alice" → another request, updated results

**Benefits:**
- ✅ Reduces initial server load
- ✅ Avoids "too many results" when search is broad
- ✅ Forces users to be more specific

---

## Custom Filtering Logic

### Filter by Multiple Fields

Search in multiple data fields:

```typescript
import { ComboBox, FilteringEventArgs } from '@syncfusion/ej2-dropdowns';
import { Query, DataManager } from '@syncfusion/ej2-data';

const employeeData: { [key: string]: Object }[] = [
    { id: 1, firstName: 'Alice', lastName: 'Johnson' },
    { id: 2, firstName: 'Bob', lastName: 'Smith' },
    { id: 3, firstName: 'Carol', lastName: 'White' }
];

const comboBox: ComboBox = new ComboBox({
    dataSource: employeeData,
    fields: { text: 'firstName', value: 'id' },
    allowFiltering: true,
    
    filtering: (args: FilteringEventArgs) => {
        const searchText = args.text.toLowerCase();
        
        const filtered = employeeData.filter(emp => {
            const firstName = (emp.firstName as string).toLowerCase();
            const lastName = (emp.lastName as string).toLowerCase();
            
            // Match either first or last name
            return firstName.startsWith(searchText) || lastName.startsWith(searchText);
        });
        
        args.updateData(filtered);
    },
    placeholder: "Search by first or last name..."
});

comboBox.appendTo('#comboelement');

// User types: "john"
// Shows: nothing (no match)

// User types: "john" → "alice"
// Shows: "Alice Johnson" (matches last name)

// User types: "bob"
// Shows: "Bob Smith" (matches first name)
```

---

### Filter with Regular Expressions

Complex pattern matching:

```typescript
import { ComboBox, FilteringEventArgs } from '@syncfusion/ej2-dropdowns';

const emailList: string[] = [
    'alice@company.com',
    'bob@company.com',
    'admin@company.com',
    'support@company.com',
    'john@external.com'
];

const comboBox: ComboBox = new ComboBox({
    dataSource: emailList,
    allowFiltering: true,
    
    filtering: (args: FilteringEventArgs) => {
        if (args.text === '') {
            args.updateData(emailList);
        } else {
            // Regex: emails containing search text
            const pattern = new RegExp(args.text, 'i');  // case-insensitive
            const filtered = emailList.filter(email => pattern.test(email));
            args.updateData(filtered);
        }
    },
    placeholder: "Search email..."
});

comboBox.appendTo('#comboelement');

// User types: "@company"
// Shows: alice@company.com, bob@company.com, admin@company.com, support@company.com

// User types: "admin"
// Shows: admin@company.com
```

---

## Remote Filtering

### Server-Side Search with DataManager

```typescript
import { ComboBox, FilteringEventArgs } from '@syncfusion/ej2-dropdowns';
import { DataManager, Query, ODataV4Adaptor } from '@syncfusion/ej2-data';

const customerDataManager = new DataManager({
    url: 'https://services.odata.org/V4/Northwind/Northwind.svc/Customers',
    adaptor: new ODataV4Adaptor,
    crossDomain: true
});

const comboBox: ComboBox = new ComboBox({
    dataSource: customerDataManager,
    query: new Query().select(['ContactName', 'CustomerID']).take(10),
    fields: { text: 'ContactName', value: 'CustomerID' },
    allowFiltering: true,
    debounceDelay: 300,
    
    filtering: (args: FilteringEventArgs) => {
        let query = new Query()
            .select(['ContactName', 'CustomerID'])
            .take(10);
        
        // Add search condition only if text entered
        if (args.text) {
            query = query.where('ContactName', 'startswith', args.text, true);
        }
        
        // Send filtered query to server
        args.updateData(customerDataManager, query);
    },
    placeholder: "Search customer..."
});

comboBox.appendTo('#comboelement');
```

**Advantages:**
- ✅ Handles large datasets (millions of records)
- ✅ Server-side indexing and optimization
- ✅ Only relevant results transmitted

**Performance tips:**
- Use indexes on searchable fields
- Limit results with `.take()`
- Add minimum character requirement
- Use debounce delay

---

## Troubleshooting

### Issue: Filtering Doesn't Work

**Cause 1:** `allowFiltering` not set
```typescript
// ❌ Wrong
const comboBox = new ComboBox({
    dataSource: ['a', 'b', 'c']
    // allowFiltering missing
});

// ✅ Correct
const comboBox = new ComboBox({
    dataSource: ['a', 'b', 'c'],
    allowFiltering: true  // Required
});
```

**Cause 2:** Filtering event returns no data
```typescript
// ❌ Wrong
filtering: (args: FilteringEventArgs) => {
    // No updateData call
    const filtered = data.filter(x => x.includes(args.text));
}

// ✅ Correct
filtering: (args: FilteringEventArgs) => {
    const filtered = data.filter(x => x.includes(args.text));
    args.updateData(filtered);  // Must call updateData
}
```

---

### Issue: Remote Filtering Slow

**Solution:** Add debounce and minimum character requirement
```typescript
const comboBox = new ComboBox({
    allowFiltering: true,
    debounceDelay: 500,  // Increase delay
    
    filtering: (args: FilteringEventArgs) => {
        if (args.text.length < 3) return;  // Minimum 3 chars
        // Rest of filtering...
    }
});
```

---

### Issue: Case Sensitivity Not Working

**Cause:** 4th parameter in `.where()` not set correctly
```typescript
// Case-insensitive
query.where('Name', 'startswith', 'abc', true);   // true = ignore case

// Case-sensitive  
query.where('Name', 'startswith', 'abc', false);  // false = match case
```

---

## Next Steps

- ✅ **Done:** Filtering and search implementation
- 📖 **Next:** [Templates & Grouping](../templates-and-grouping.md) - Format filtered results
- ⚙️ **Then:** [Advanced Features](../advanced-features.md) - Virtual scroll with filtering

---

## See Also

- [Query Methods](https://ej2.syncfusion.com/documentation/data/getting-started/)
- [API: allowFiltering](https://ej2.syncfusion.com/documentation/api/combo-box/#allowfiltering)
- [API: debounceDelay](https://ej2.syncfusion.com/documentation/api/combo-box/#debouncedelay)
- [Filtering Event](https://ej2.syncfusion.com/documentation/api/combo-box/#filtering)
