---
name: syncfusion-javascript-combobox
description: Guide for implementing Syncfusion JavaScript ComboBox component for single-item selection with data binding, filtering, grouping, templates, and accessibility. Use this skill when users need ComboBox implementation in TypeScript, dropdown selection components, search/filtering functionality, custom value entry, data binding from local or remote sources, popup customization, or keyboard navigation support in Syncfusion JavaScript projects.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing JavaScript ComboBox Component

## When to Use This Skill

**Use this skill when you need to:**

- Set up a Syncfusion JavaScript ComboBox component in your web application
- Create a dropdown for single-item selection from predefined options
- Implement filtering and search functionality as users type
- Bind ComboBox to local arrays or remote data sources (OData, Web API, DataManager)
- Group and organize list items with custom display templates
- Enable custom value entry when items aren't in the predefined list
- Configure advanced features (virtual scrolling, disabled items, resizing)
- Integrate with forms and handle value selection events
- Ensure keyboard navigation and accessibility compliance (WCAG 2.2, ARIA)
- Customize styling, themes, and support RTL languages

**Skip this skill if:**
- You need multi-item selection (use MultiSelect Dropdown instead)
- You need a simple select without filtering (use regular HTML select)
- You're not using TypeScript (check framework-specific documentation)
- You need a simple button dropdown (use DropdownButton instead)

---

## Component Overview & Architecture

The **ComboBox** is a flexible dropdown component that allows users to:
1. **Select from a list** of predefined options
2. **Enter custom values** when `allowCustom` is enabled
3. **Search/filter** items as they type in real-time
4. **Group items** by category with custom headers
5. **Customize display** with templates for items, groups, headers, and footers

### Key Characteristics

| Aspect | Details |
|--------|---------|
| **Selection Mode** | Single item from predefined list OR custom value |
| **Data Sources** | Local arrays, remote OData, DataManager, Web API, async data |
| **Filtering** | Real-time search with configurable filter strategies |
| **Performance** | Virtual scrolling for large datasets (10,000+ items) |
| **Customization** | Item templates, group headers, custom CSS styling |
| **Accessibility** | WCAG 2.2 compliant, full keyboard navigation, ARIA attributes |
| **Events** | select, filtering, change, blur, focus, scroll, actionFailure |

### When to Use ComboBox vs Alternatives

| Component | Use Case |
|-----------|----------|
| **ComboBox** | Single selection with search/filter enabled |
| **Dropdown List** | Single selection, no search, fixed list |
| **MultiSelect** | Multiple items selection |
| **AutoComplete** | Search with suggestions from large dataset |
| **Mention** | @mentions in text input (Twitter-like) |

---

## Documentation Navigation Guide

### 📄 Getting Started
**Read:** [references/getting-started.md](references/getting-started.md)
- Install `@syncfusion/ej2-dropdowns` package and dependencies
- Configure webpack for TypeScript bundling
- Import CSS themes (Material, Bootstrap, Fabric)
- Create HTML input element and initialize ComboBox
- Bind simple string array data
- Set basic properties (placeholder, value, popupHeight)
- Run the application and verify setup

### 📄 Data Binding & Sources
**Read:** [references/data-binding.md](references/data-binding.md)
- Bind to local arrays of strings
- Bind to arrays of JSON objects with field mapping
- Handle nested object fields (Country.Name, Code.Id)
- Fetch data from remote OData services
- Use DataManager with Query for data filtering
- Handle dynamic data updates
- Common data binding patterns and gotchas

### 📄 Filtering & Search
**Read:** [references/filtering-and-search.md](references/filtering-and-search.md)
- Enable filtering with `allowFiltering` property
- Configure filter types (startsWith, contains, endsWith)
- Case-sensitive vs case-insensitive filtering
- Diacritics filtering for accent-insensitive search
- Debounce delay to optimize performance
- Minimum filter character requirements
- Custom filtering logic with filtering event

### 📄 Templates & Grouping
**Read:** [references/templates-and-grouping.md](references/templates-and-grouping.md)
- Item templates for custom list item rendering
- Group templates for category headers
- Header and footer templates for popup decoration
- Group items by data field with `groupBy`
- Template syntax and interpolation patterns
- Combining multiple templates effectively
- Real-world template examples

### 📄 Disabled Items & Resizable Popup
**Read:** [references/disabled-items-and-resize.md](references/disabled-items-and-resize.md)
- Mark specific items as non-selectable using disabled field
- Dynamic item disabling with disableItem() method
- Disable entire ComboBox component
- Enable user-resizable popups for better visibility
- Configure popup height and width
- Real-world examples (permissions, inventory status)

### 📄 Advanced Features & Configuration
**Read:** [references/advanced-features.md](references/advanced-features.md)
- Virtual scrolling for performance with large lists (10,000+ items)
- Custom values not in the predefined list
- Read-only mode for display-only scenarios
- Event handling (select, filtering, change, blur, actionFailure)
- RTL support for right-to-left languages
- CSS class customization and styling
- Keyboard navigation and accessibility features
- WCAG 2.2 compliance and screen reader support

### 📄 How-To Guides
**Read:** [references/how-to-guides.md](references/how-to-guides.md)
- Auto-complete search functionality
- Cascading dropdowns (Country → State → City)
- Adding icons to list items
- Form integration and validation
- Custom styling and CSS customization

---

## Quick Start Example

### Minimal Setup (5 minutes)

**HTML:**
```html
<!DOCTYPE html>
<html>
<head>
    <link rel="stylesheet" href="node_modules/@syncfusion/ej2-base/styles/material.css" />
    <link rel="stylesheet" href="node_modules/@syncfusion/ej2-dropdowns/styles/material.css" />
</head>
<body>
    <input type="text" id="comboelement" />
</body>
</html>
```

**TypeScript (app.ts):**
```typescript
import { ComboBox } from '@syncfusion/ej2-dropdowns';

// Define simple data array
const sportsData: string[] = ['Badminton', 'Cricket', 'Football', 'Golf', 'Tennis'];

// Initialize ComboBox component
const comboBoxObject: ComboBox = new ComboBox({
    dataSource: sportsData,
    placeholder: "Select a sport",
    popupHeight: '200px'
});

// Render the component to DOM
comboBoxObject.appendTo('#comboelement');
```

**What's happening:**
1. Import `ComboBox` from `@syncfusion/ej2-dropdowns` package
2. Define array of strings as data source
3. Create ComboBox instance with configuration object
4. Set placeholder for empty state hint
5. Render component to HTML element with `appendTo()`

**Result:** A dropdown that allows users to select a sport or type custom values.

---

## Common Patterns & Workflows

### Pattern 1: Data Objects with Field Mapping

**Scenario:** You have an array of objects and need to display specific fields.

```typescript
const employeeData: { [key: string]: Object }[] = [
    { Id: 'emp1', Name: 'Alice Johnson', Department: 'Engineering' },
    { Id: 'emp2', Name: 'Bob Smith', Department: 'Sales' },
    { Id: 'emp3', Name: 'Carol White', Department: 'Marketing' }
];

const comboBox: ComboBox = new ComboBox({
    dataSource: employeeData,
    fields: { text: 'Name', value: 'Id' },  // Map which field to show/store
    placeholder: "Select an employee"
});

comboBox.appendTo('#comboelement');
```

**When to use:** Working with database records, API responses, or structured data.

See: [Data Binding Reference](references/data-binding.md#array-of-json-data)

---

### Pattern 2: Search & Filter

**Scenario:** User types to search/filter the list items dynamically.

```typescript
const comboBox: ComboBox = new ComboBox({
    dataSource: ['California', 'Florida', 'Alaska', 'Georgia'],
    allowFiltering: true,      // Enable search box
    placeholder: "Select a state"
});

comboBox.appendTo('#comboelement');
```

**When to use:** Large lists where users need to quickly find items.

See: [Filtering & Search Reference](references/filtering-and-search.md)

---

### Pattern 3: Remote Data with Debounce

**Scenario:** Fetch data from server as user types, but not on every keystroke.

```typescript
import { ComboBox, FilteringEventArgs } from '@syncfusion/ej2-dropdowns';
import { DataManager, ODataV4Adaptor, Query } from '@syncfusion/ej2-data';

const comboBox: ComboBox = new ComboBox({
    dataSource: new DataManager({
        url: 'https://services.odata.org/V4/Northwind/Northwind.svc/Customers',
        adaptor: new ODataV4Adaptor,
        crossDomain: true
    }),
    query: new Query().select(['ContactName', 'CustomerID']).take(6),
    fields: { text: 'ContactName', value: 'CustomerID' },
    allowFiltering: true,
    debounceDelay: 300,        // Wait 300ms before fetching
    placeholder: "Select a customer"
});

comboBox.appendTo('#comboelement');
```

**When to use:** Server-side search for scalability with large datasets.

See: [Filtering & Search Reference](references/filtering-and-search.md#debounce-delay)

---

### Pattern 4: Cascading Dropdowns

**Scenario:** Second ComboBox depends on first ComboBox selection.

```typescript
// First ComboBox - Countries
const countryData: { [key: string]: Object }[] = [
    { id: 'USA', name: 'United States' },
    { id: 'IND', name: 'India' },
    { id: 'GBR', name: 'United Kingdom' }
];

const countryCombo: ComboBox = new ComboBox({
    dataSource: countryData,
    fields: { text: 'name', value: 'id' },
    change: (args: any) => {
        // Update second ComboBox when selection changes
        const selectedCountry = args.value;
        stateCombo.dataSource = getStatesForCountry(selectedCountry);
        stateCombo.refresh();
    },
    placeholder: "Select country"
});

countryCombo.appendTo('#country');

// Second ComboBox - States (initially empty)
const stateCombo: ComboBox = new ComboBox({
    dataSource: [],
    placeholder: "Select state"
});

stateCombo.appendTo('#state');

function getStatesForCountry(country: string): string[] {
    const stateMap: { [key: string]: string[] } = {
        'USA': ['California', 'Texas', 'New York'],
        'IND': ['Tamil Nadu', 'Kerala', 'Maharashtra'],
        'GBR': ['England', 'Scotland', 'Wales']
    };
    return stateMap[country] || [];
}
```

**When to use:** Multi-level selection where options depend on previous choices.

See: [Advanced Features Reference](references/advanced-features.md#cascading-dropdowns)

---

## Keyboard Navigation

The ComboBox fully supports keyboard navigation for accessibility:

| Key | Action |
|-----|--------|
| <kbd>↓ Arrow Down</kbd> | Select next item or open popup |
| <kbd>↑ Arrow Up</kbd> | Select previous item |
| <kbd>Enter</kbd> | Select focused item |
| <kbd>Escape</kbd> | Close popup |
| <kbd>Tab</kbd> | Move to next element (close popup) |
| <kbd>Alt + ↓</kbd> | Open popup list |
| <kbd>Alt + ↑</kbd> | Close popup list |
| <kbd>Home</kbd> | Move cursor to start of text |
| <kbd>End</kbd> | Move cursor to end of text |

Users can also type to filter items when popup is open.

---

## Accessibility Features

The ComboBox meets WCAG 2.2, Section 508, and ADA compliance standards:

- ✅ **ARIA Attributes:** `role="combobox"`, `aria-expanded`, `aria-selected`, `aria-disabled`
- ✅ **Screen Reader Support:** All elements labeled and announced properly
- ✅ **Keyboard Navigation:** Full control without mouse
- ✅ **Focus Management:** Clear focus indicators and tab order
- ✅ **Color Contrast:** Meets WCAG AA standards
- ✅ **RTL Support:** Automatic layout adjustment for right-to-left languages

See: [Advanced Features Reference](references/advanced-features.md#accessibility-and-wcag-compliance)

---

## Common Issues & Troubleshooting

**Issue: CSS styles not loading?**
- Ensure CSS import statement in HTML: `<link rel="stylesheet" href="node_modules/@syncfusion/ej2-dropdowns/styles/material.css" />`
- Check theme name (material, bootstrap, fabric, tailwind)

**Issue: ComboBox not rendering?**
- Verify HTML element exists with matching ID
- Ensure `appendTo()` called after ComboBox initialization
- Check browser console for JavaScript errors

**Issue: Data not showing?**
- Verify dataSource is properly defined
- For object arrays, check field mapping (text, value match actual data)
- Use browser DevTools to inspect data structure

**Issue: Filtering not working?**
- Set `allowFiltering: true`
- For remote data, implement filtering event handler
- Check debounceDelay isn't too high (default 300ms)

---

## Next Steps

1. **Start with:** [Getting Started](references/getting-started.md) to set up your first ComboBox
2. **Then explore:** [Data Binding](references/data-binding.md) for your specific data source
3. **Add features:** Use [Filtering](references/filtering-and-search.md) or [Templates](references/templates-and-grouping.md) as needed
4. **Advanced:** Check [Advanced Features](references/advanced-features.md) for virtual scrolling, events, and accessibility

---

## Resources

- [Syncfusion ComboBox Official Docs](https://www.syncfusion.com/documentation/typescript/combobox/getting-started/)
- [API Reference](https://ej2.syncfusion.com/documentation/api/combo-box/)
- [GitHub Examples](https://github.com/SyncfusionExamples/ej2-combobox-typescript-samples)
- [Syncfusion Support](https://www.syncfusion.com/support/)

---

## See Also

- **AutoComplete:** For search suggestions and autocomplete behavior
- **MultiSelect:** For multiple item selection dropdown
- **Dropdown List:** For simple selection without filtering
- **Mention:** For @mention functionality in text areas
