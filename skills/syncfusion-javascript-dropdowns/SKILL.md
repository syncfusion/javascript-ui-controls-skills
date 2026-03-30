---
name: syncfusion-javascript-dropdowns
description: Comprehensive guide for implementing Syncfusion React dropdown components including DropDownList and MultiSelect. Use this when building selection interfaces, data binding, filtering, cascading dropdowns, custom templates, and accessible dropdown experiences in TypeScript applications.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
  category: "Dropdowns"
---

# Implementing Syncfusion TypeScript Dropdowns

## DropDownList

The Syncfusion DropDownList component provides a single-select dropdown with support for local/remote data binding, filtering, grouping, custom templates, virtual scrolling, accessibility, and cascading patterns — all in TypeScript.

### Documentation and Navigation Guide

#### Getting Started
📄 **Read:** [references/dropdownlist-getting-started.md](references/dropdownlist-getting-started.md)
- Installation and package dependencies
- Initializing DropDownList on input, select, and UL tags
- Binding a simple data array
- Configuring popup height and width
- First render with `appendTo`

#### Data Binding
📄 **Read:** [references/dropdownlist-data-binding.md](references/dropdownlist-data-binding.md)
- Binding arrays of strings, JSON objects, and complex nested data
- Using `fields` to map text, value, iconCss, groupBy, and disabled columns
- Remote data with `DataManager` (OData, ODataV4 adaptors)
- Using `query` to filter/sort remote data
- Primitive vs. object value binding (`allowObjectBinding`)

#### Filtering
📄 **Read:** [references/dropdownlist-filtering.md](references/dropdownlist-filtering.md)
- Enabling `allowFiltering` and the filter bar
- Handling the `filtering` event with `updateData`
- Filter types: `startsWith`, `endsWith`, `contains`
- Minimum character limit, case-sensitive filtering, diacritics (`ignoreAccent`)
- `debounceDelay` for performance
- Highlighting matched characters with `highlightSearch`
- Limiting search result count

#### Grouping
📄 **Read:** [references/dropdownlist-grouping.md](references/dropdownlist-grouping.md)
- Mapping `groupBy` field for category-based grouping
- Inline and fixed group headers
- HTML `<select>` with `<optgroup>` support
- Customizing group headers with `groupTemplate`
- Disabling the fixed group header via CSS

#### Templates
📄 **Read:** [references/dropdownlist-templates.md](references/dropdownlist-templates.md)
- `itemTemplate` — customize each list item
- `valueTemplate` — customize the selected value display
- `groupTemplate` — customize group headers
- `headerTemplate` and `footerTemplate` — static popup header/footer
- `noRecordsTemplate` and `actionFailureTemplate`

#### Features and Configuration
📄 **Read:** [references/dropdownlist-features.md](references/dropdownlist-features.md)
- Disabled items via `fields.disabled` and `disableItem` method
- Clear selection with `showClearButton` or programmatically
- Popup resize with `allowResize`
- Virtual scrolling with `enableVirtualization` for large datasets
- Incremental search (built-in)
- `floatLabelType`, `enabled`, `readonly`, `sortOrder`
- `htmlAttributes`, `cssClass`, `width`, `zIndex`

#### Events and Methods
📄 **Read:** [references/dropdownlist-events-and-methods.md](references/dropdownlist-events-and-methods.md)
- `change` event with `isInteracted` flag
- `open`, `close`, `beforeOpen`, `select`, `dataBound` events
- `actionBegin`, `actionComplete`, `actionFailure` for remote data
- `filtering`, `focus`, `blur` events
- `showPopup`, `hidePopup`, `addItem`, `getItems`, `getDataByValue`
- `dataBind`, `clear`, `refresh`, `destroy`, `focusIn`, `focusOut`

#### Cascading DropDownList
📄 **Read:** [references/dropdownlist-cascading.md](references/dropdownlist-cascading.md)
- Country → State → City cascading pattern
- Filtering child dropdown data using `change` event and `Query`
- Calling `dataBind` after property changes
- Preselecting items in cascading dropdowns

#### Style and Accessibility
📄 **Read:** [references/dropdownlist-style-and-accessibility.md](references/dropdownlist-style-and-accessibility.md)
- CSS classes for wrapper, icon, focus, hover, popup customization
- Float label with asterisk
- WAI-ARIA roles and attributes
- Full keyboard shortcut reference
- RTL support with `enableRtl`
- Localization with `L10n`

#### API Reference
📄 **Read:** [references/dropdownlist-api.md](references/dropdownlist-api.md)
- Complete property list with types and defaults
- All methods with parameters and return types
- All events with descriptions
- `FieldSettingsModel` reference

---

### Quick Start

```ts
import { DropDownList } from '@syncfusion/ej2-dropdowns';

// Simple string array
let dropDownListObject: DropDownList = new DropDownList({
    dataSource: ['Badminton', 'Cricket', 'Football', 'Golf', 'Tennis'],
    placeholder: 'Select a game'
});
dropDownListObject.appendTo('#ddlelement');
```

```html
<!-- index.html -->
<input type="text" id="ddlelement" />
```

---

### Common Patterns

#### JSON Data with Field Mapping
```ts
import { DropDownList } from '@syncfusion/ej2-dropdowns';

let sportsData: { [key: string]: Object }[] = [
    { Id: 'game1', Game: 'Badminton' },
    { Id: 'game2', Game: 'Football' },
    { Id: 'game3', Game: 'Tennis' }
];

let ddl: DropDownList = new DropDownList({
    dataSource: sportsData,
    fields: { text: 'Game', value: 'Id' },
    placeholder: 'Select a game'
});
ddl.appendTo('#ddlelement');
```

#### Remote Data with OData
```ts
import { DropDownList } from '@syncfusion/ej2-dropdowns';
import { Query, DataManager, ODataV4Adaptor } from '@syncfusion/ej2-data';

let ddl: DropDownList = new DropDownList({
    dataSource: new DataManager({
        url: 'https://your-api-endpoint.example.com/odata/', // Replace with your trusted API endpoint
        adaptor: new ODataV4Adaptor,
        crossDomain: true  // Enable only when the API origin differs from your app; use only with trusted endpoints
    }),
    query: new Query().from('Customers').select(['ContactName', 'CustomerID']).take(6),
    fields: { text: 'ContactName', value: 'CustomerID' },
    placeholder: 'Select a customer',
    sortOrder: 'Ascending'
});
ddl.appendTo('#ddlelement');
```

#### Filtering with Search Box
```ts
import { DropDownList, FilteringEventArgs } from '@syncfusion/ej2-dropdowns';
import { DataManager, Query } from '@syncfusion/ej2-data';

let ddl: DropDownList = new DropDownList({
    dataSource: searchData,
    fields: { text: 'Country', value: 'Index' },
    allowFiltering: true,
    placeholder: 'Select a country',
    filtering: (e: FilteringEventArgs) => {
        let query = new Query();
        query = e.text !== '' ? query.where('Country', 'startswith', e.text, true) : query;
        e.updateData(searchData, query);
    }
});
ddl.appendTo('#ddlelement');
```

#### Handling Value Change
```ts
let ddl: DropDownList = new DropDownList({
    dataSource: sportsData,
    fields: { text: 'Game', value: 'Id' },
    placeholder: 'Select a game',
    change: (args) => {
        if (args.isInteracted) {
            console.log('User selected:', args.value);
        } else {
            console.log('Programmatic change:', args.value);
        }
    }
});
```

---

### Key Properties at a Glance

| Property | Type | Purpose |
|---|---|---|
| `dataSource` | array \| DataManager | Data to populate the list |
| `fields` | FieldSettingsModel | Maps text, value, groupBy, iconCss, disabled |
| `value` | string \| number \| boolean \| object | Selected value |
| `text` | string | Display text of selected item |
| `index` | number | Index of selected item |
| `placeholder` | string | Input placeholder text |
| `allowFiltering` | boolean | Show filter bar |
| `enableVirtualization` | boolean | Virtual scroll for large lists |
| `sortOrder` | 'None' \| 'Ascending' \| 'Descending' | Sort list items |
| `showClearButton` | boolean | Show clear (×) button |
| `enabled` | boolean | Enable/disable the component |
| `readonly` | boolean | Prevent user interaction |
| `popupHeight` | string \| number | Height of popup (default 300px) |
| `popupWidth` | string \| number | Width of popup (default 100%) |

---

## MultiSelect

The Syncfusion TypeScript MultiSelect component is a feature-rich dropdown that allows users to select one or multiple values from a list. It supports data binding, filtering, grouping, cascading, custom templates, and comprehensive accessibility features.

### Documentation Navigation Guide

#### Getting Started
📄 **Read:** [references/multiselect-getting-started.md](references/multiselect-getting-started.md)
- Installation and package setup
- CSS imports and theme configuration
- Element initialization (SELECT, UL, INPUT tags)
- Basic component setup with data

#### Data Binding & Sources
📄 **Read:** [references/multiselect-data-binding.md](references/multiselect-data-binding.md)
- Binding local data arrays (strings and objects)
- Connecting to remote data sources
- DataManager configuration with adapters
- Field mapping and data format specifications

#### Selection Modes & Values
📄 **Read:** [references/multiselect-selection-modes.md](references/multiselect-selection-modes.md)
- Single and multiple selection modes
- Working with the `value` property (array of selected values)
- Getting/setting selected values programmatically
- Selection change events and handlers

#### Checkbox & Multi-Select Features
📄 **Read:** [references/multiselect-checkbox-multiselect.md](references/multiselect-checkbox-multiselect.md)
- Enabling checkbox mode for visual multi-select
- Select All / Unselect All functionality
- Delimiter mode vs visual chip display
- Customizing checkbox appearance

#### Filtering & Search
📄 **Read:** [references/multiselect-filtering-search.md](references/multiselect-filtering-search.md)
- Enabling real-time filtering
- Custom filter logic and expressions
- Minimum character validation before search
- Case-sensitive and case-insensitive filtering

#### Grouping & Sorting
📄 **Read:** [references/multiselect-grouping-sorting.md](references/multiselect-grouping-sorting.md)
- Grouping items by category field
- Sorting in ascending/descending order
- Custom group templates
- Hierarchical data grouping

#### Cascading Dropdowns
📄 **Read:** [references/multiselect-cascading-dropdowns.md](references/multiselect-cascading-dropdowns.md)
- Two-level and multi-level cascading
- Handling parent-child data synchronization
- Remote data loading with cascading
- Resetting child dropdowns when parent changes

#### Customization & Templates
📄 **Read:** [references/multiselect-customization-templates.md](references/multiselect-customization-templates.md)
- Item templates for list items
- Value templates for display area
- Group templates for grouped items
- Header and footer templates
- CSS customization and styling

#### Performance: Virtual Scrolling
📄 **Read:** [references/multiselect-virtual-scrolling.md](references/multiselect-virtual-scrolling.md)
- Enabling virtual scrolling for large datasets
- Performance optimization techniques
- Virtual scroll with filtering and grouping
- Memory and rendering considerations

#### Accessibility & Standards
📄 **Read:** [references/multiselect-accessibility.md](references/multiselect-accessibility.md)
- WCAG 2.2 compliance and standards
- Keyboard navigation (Arrow keys, Tab, Enter, Escape)
- ARIA attributes and roles
- Screen reader support and focus management

#### How-To & Troubleshooting
📄 **Read:** [references/multiselect-how-to-guide.md](references/multiselect-how-to-guide.md)
- Common implementation patterns and recipes
- Edge cases and workarounds
- Performance tips and best practices
- Troubleshooting common issues

#### API Reference
📄 **Read:** [references/multiselect-api.md](references/multiselect-api.md)
- Complete list of all properties with types and defaults
- All public methods with parameter signatures and return types
- All events with their argument types and usage
- `FieldSettingsModel` field mapping reference
- Event argument type definitions (`MultiSelectChangeEventArgs`, `SelectEventArgs`, `RemoveEventArgs`, `TaggingEventArgs`, `FilteringEventArgs`, etc.)
- Module injection guide for `CheckBoxSelection` and `VirtualScroll`

### Quick Start Example

#### Basic Setup

```typescript
import { MultiSelect } from '@syncfusion/ej2-dropdowns';

// Initialize with simple string data
const multiSelect = new MultiSelect({
  dataSource: ['Apple', 'Banana', 'Orange', 'Mango'],
  placeholder: 'Select fruits'
});

multiSelect.appendTo('#select');
```

#### Multi-Select with Checkboxes

```typescript
import { MultiSelect, CheckBoxSelection } from '@syncfusion/ej2-dropdowns';

MultiSelect.Inject(CheckBoxSelection);

const multiSelect = new MultiSelect({
  dataSource: [
    { id: '1', name: 'Apple' },
    { id: '2', name: 'Banana' },
    { id: '3', name: 'Orange' }
  ],
  fields: { text: 'name', value: 'id' },
  mode: 'CheckBox',
  showSelectAll: true,
  placeholder: 'Select fruits'
});

multiSelect.appendTo('#select');
```

#### With Filtering

```typescript
import { MultiSelect, FilteringEventArgs } from '@syncfusion/ej2-dropdowns';
import { Query } from '@syncfusion/ej2-data';

const data = [
  { id: '1', city: 'New York' },
  { id: '2', city: 'Los Angeles' },
  { id: '3', city: 'Chicago' }
];

const multiSelect = new MultiSelect({
  dataSource: data,
  fields: { text: 'city', value: 'id' },
  allowFiltering: true,
  filtering: (e: FilteringEventArgs) => {
    const query = new Query();
    if (e.text !== '') {
      query.where('city', 'startswith', e.text, true);
    }
    e.updateData(data, query);
  },
  placeholder: 'Search cities'
});

multiSelect.appendTo('#select');
```

### Common Patterns

#### Pattern 1: Cascading Dropdowns

```typescript
// Country dropdown
const countrySelect = new MultiSelect({
  dataSource: countries,
  fields: { text: 'name', value: 'id' },
  change: () => {
    const selectedCountry = countrySelect.value;
    // Update state dropdown based on selected country
    stateSelect.dataSource = states.filter(s => s.countryId === selectedCountry);
    stateSelect.refresh();
  }
});

countrySelect.appendTo('#country');
```

#### Pattern 2: Remote Data Binding

```typescript
import { DataManager, ODataV4Adaptor, Query } from '@syncfusion/ej2-data';

const multiSelect = new MultiSelect({
  dataSource: new DataManager({
    url: 'https://your-api-endpoint.example.com/odata/Customers', // Replace with your trusted API endpoint
    adaptor: new ODataV4Adaptor(),
    crossDomain: true  // Enable only when the API origin differs from your app; use only with trusted endpoints
  }),
  query: new Query().select(['ContactName', 'CustomerID']).take(10),
  fields: { text: 'ContactName', value: 'CustomerID' },
  placeholder: 'Select customer'
});

multiSelect.appendTo('#select');
```

#### Pattern 3: Custom Item Template

> ⚠️ **Security note:** Template strings use `${field}` interpolation. If field values originate from user input or remote sources, sanitize/encode them before binding to prevent XSS injection.

```typescript
const multiSelect = new MultiSelect({
  dataSource: employees,
  fields: { text: 'firstName', value: 'id' },
  itemTemplate: '<div><span>${firstName}</span><span>${department}</span></div>',
  valueTemplate: '<span>${firstName} - ${department}</span>',
  placeholder: 'Select employee'
});

multiSelect.appendTo('#select');
```

### Key Props Quick Reference

| Prop | Type | Default | Purpose |
|------|------|---------|---------|
| `dataSource` | `array \| DataManager` | `[]` | Bind data to component |
| `fields` | `FieldSettingsModel` | `{}` | Map data fields (text, value, groupBy, iconCss) |
| `value` | `string[] \| number[] \| boolean[] \| object[]` | `null` | Get/set selected value(s) |
| `mode` | `'Box' \| 'Delimiter' \| 'Default' \| 'CheckBox'` | `'Default'` | Selection display mode |
| `allowFiltering` | `boolean` | `null` | Enable search/filter functionality |
| `enableVirtualization` | `boolean` | `false` | Enable virtual scrolling for performance |
| `itemTemplate` | `string \| Function` | `null` | Custom HTML for list items |
| `valueTemplate` | `string \| Function` | `null` | Custom HTML for selected display |
| `placeholder` | `string` | `null` | Input placeholder text |
| `showSelectAll` | `boolean` | `false` | Show Select All option in CheckBox mode |
| `enableGroupCheckBox` | `boolean` | `false` | Enable group header checkboxes in CheckBox mode |
| `maximumSelectionLength` | `number` | `1000` | Limit number of selectable items |
| `allowCustomValue` | `boolean` | `false` | Allow entering values not in the list |
| `closePopupOnSelect` | `boolean` | `true` | Keep popup open after each selection |
| `hideSelectedItem` | `boolean` | `true` | Hide selected items from dropdown list |
| `delimiterChar` | `string` | `','` | Separator character in Delimiter mode |
| `showClearButton` | `boolean` | `true` | Show remove icon on each chip |
| `showDropDownIcon` | `boolean` | `false` | Show dropdown arrow button |
| `enabled` | `boolean` | `true` | Enable/disable component |
| `readonly` | `boolean` | `false` | Make component read-only |
| `zIndex` | `number` | `1000` | z-index of the popup element |

> 📘 For the complete API including all properties, methods, and events, see [references/multiselect-api.md](references/multiselect-api.md).

### Related Features

- **Data Formats:** JSON, XML, OData, Web API
- **Themes:** Material, Bootstrap, Fabric, Tailwind
- **Grouping:** Group items by category with custom templates
- **Sorting:** Ascending, Descending sort orders
- **Localization:** Multi-language support
- **RTL Support:** Right-to-left language support
- **Keyboard Navigation:** Full keyboard accessibility
