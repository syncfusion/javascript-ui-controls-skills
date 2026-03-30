# Filtering in Syncfusion TypeScript Pivot View

## Table of Contents
- [Overview](#overview)
- [Member Filtering](#member-filtering)
- [Label Filtering](#label-filtering)
- [Value Filtering](#value-filtering)
- [TypeScript Code Examples](#typescript-code-examples)
- [Configuration Options](#configuration-options)
- [Common Patterns](#common-patterns)
- [Troubleshooting](#troubleshooting)

## Overview

The Pivot Table provides comprehensive filtering capabilities allowing users to filter data based on members, labels, and values. Filtering can be applied at both design-time and runtime, helping users focus on specific data subsets for analysis.

**Key Features:**
- Member filtering (include/exclude specific items)
- Label filtering with operators (Equals, Contains, BeginWith, etc.)
- Value filtering for measures
- Performance optimization options
- Load-on-demand for large datasets

## Member Filtering

Member filtering allows including or excluding specific members from row and column fields.

### Include Members
```typescript
import { PivotView, IDataSet } from '@syncfusion/ej2-pivotview';
import { pivotData } from './datasource.ts';

let pivotTableObj: PivotView = new PivotView({
    dataSourceSettings: {
        dataSource: pivotData as IDataSet[],
        rows: [{ name: 'Country' }],
        columns: [{ name: 'Year' }],
        values: [{ name: 'Sold', caption: 'Units Sold' }],
        filterSettings: [
            { name: 'Country', type: 'Include', items: ['France', 'Germany'] }
        ]
    },
    height: 350
});
pivotTableObj.appendTo('#PivotTable');
```

### Exclude Members
```typescript
filterSettings: [
    { name: 'Country', type: 'Exclude', items: ['United States', 'Canada'] }
]
```

## Label Filtering

Label filtering applies conditions to row and column headers using various operators.

### Available Operators
- **Equals** - Exact match
- **DoesNotEquals** - Not equal to
- **BeginWith** - Starts with
- **DoesNotBeginWith** - Does not start with
- **EndsWith** - Ends with
- **DoesNotEndsWith** - Does not end with
- **Contains** - Contains substring
- **DoesNotContains** - Does not contain substring
- **GreaterThan** - Greater than
- **GreaterThanOrEqualTo** - Greater than or equal
- **LessThan** - Less than
- **LessThanOrEqualTo** - Less than or equal
- **Between** - Between two values
- **NotBetween** - Not between two values

### Example: Label Filtering with Operators
```typescript
import { PivotView, IDataSet } from '@syncfusion/ej2-pivotview';
import { pivotData } from './datasource.ts';

let pivotTableObj: PivotView = new PivotView({
    dataSourceSettings: {
        dataSource: pivotData as IDataSet[],
        rows: [{ name: 'Country' }],
        columns: [{ name: 'Products' }],
        values: [{ name: 'Sold', caption: 'Units Sold' }],
        filterSettings: [
            {
                name: 'Products',
                type: 'Label',
                condition: 'BeginWith',
                value1: 'Bike'
            }
        ]
    },
    height: 350
});
pivotTableObj.appendTo('#PivotTable');
```

### Between Operator
```typescript
filterSettings: [
    {
        name: 'Country',
        type: 'Label',
        condition: 'Between',
        value1: 'A',
        value2: 'M'
    }
]
```

## Value Filtering

Value filtering applies conditions to aggregated values in the Pivot Table.

### Example: Value Filtering
```typescript
import { PivotView, IDataSet } from '@syncfusion/ej2-pivotview';
import { pivotData } from './datasource.ts';

let pivotTableObj: PivotView = new PivotView({
    dataSourceSettings: {
        dataSource: pivotData as IDataSet[],
        rows: [{ name: 'Country' }],
        columns: [{ name: 'Year' }],
        values: [{ name: 'Amount', caption: 'Sold Amount' }],
        filterSettings: [
            {
                name: 'Country',
                type: 'Value',
                measure: 'Amount',
                condition: 'GreaterThan',
                value1: '300000'
            }
        ]
    },
    height: 350
});
pivotTableObj.appendTo('#PivotTable');
```

## TypeScript Code Examples

### Basic Filtering Setup
```typescript
import { PivotView, IDataSet } from '@syncfusion/ej2-pivotview';
import { pivotData } from './datasource.ts';

let pivotTableObj: PivotView = new PivotView({
    dataSourceSettings: {
        dataSource: pivotData as IDataSet[],
        expandAll: false,
        rows: [{ name: 'Country' }, { name: 'Products' }],
        columns: [{ name: 'Year', caption: 'Production Year' }, { name: 'Quarter' }],
        values: [{ name: 'Sold', caption: 'Units Sold' }, { name: 'Amount', caption: 'Sold Amount' }],
        formatSettings: [{ name: 'Amount', format: 'C0' }],
        filterSettings: [
            { name: 'Year', type: 'Exclude', items: ['FY 2015'] }
        ]
    },
    height: 350
});
pivotTableObj.appendTo('#PivotTable');
```

### Member Filter Editor Events
```typescript
import { PivotView, IDataSet, MemberFilteringEventArgs, MemberEditorOpenEventArgs } from '@syncfusion/ej2-pivotview';

let pivotTableObj: PivotView = new PivotView({
    dataSourceSettings: {
        dataSource: pivotData as IDataSet[],
        rows: [{ name: 'Country' }],
        columns: [{ name: 'Year' }],
        values: [{ name: 'Sold' }]
    },
    memberFiltering: (args: MemberFilteringEventArgs) => {
        // Customize filtering behavior
        console.log('Member filtering:', args.filterSettings);
    },
    memberEditorOpen: (args: MemberEditorOpenEventArgs) => {
        // Customize member editor
        console.log('Member editor opened for:', args.fieldName);
    },
    height: 350
});
pivotTableObj.appendTo('#PivotTable');
```

## Configuration Options

### FilterSettings Properties

| Property | Type | Description |
|----------|------|-------------|
| `name` | string | Field name to apply filter |
| `type` | string | Filter type: 'Include', 'Exclude', 'Label', 'Value' |
| `items` | string[] | Members to include/exclude |
| `condition` | string | Operator for label/value filtering |
| `value1` | string | First value for condition |
| `value2` | string | Second value (for Between, NotBetween) |
| `measure` | string | Measure name for value filtering |
| `levelCount` | number | Hierarchy level to apply filter |

### Performance Options

```typescript
import { PivotView, IDataSet } from '@syncfusion/ej2-pivotview';

let pivotTableObj: PivotView = new PivotView({
    dataSourceSettings: {
        dataSource: largeData as IDataSet[],
        rows: [{ name: 'Country' }],
        columns: [{ name: 'Year' }],
        values: [{ name: 'Sold' }]
    },
    // Limit members shown in filter dialog (top-level property)
    maxNodeLimitInMemberEditor: 500,
    
    // Enable load-on-demand for large datasets (top-level property - OLAP only)
    loadOnDemandInMemberEditor: true,
    
    height: 350
});
pivotTableObj.appendTo('#PivotTable');
```

**Important:** Both `maxNodeLimitInMemberEditor` and `loadOnDemandInMemberEditor` are top-level properties of PivotView, NOT within `dataSourceSettings`.

## Common Patterns

### Multiple Filters
```typescript
filterSettings: [
    { name: 'Country', type: 'Include', items: ['France', 'Germany', 'United Kingdom'] },
    { name: 'Products', type: 'Label', condition: 'Contains', value1: 'Bike' },
    { name: 'Year', type: 'Value', measure: 'Amount', condition: 'GreaterThan', value1: '500000' }
]
```

### Dynamic Filtering
```typescript
// Add filter programmatically
pivotTableObj.dataSourceSettings.filterSettings.push({
    name: 'Country',
    type: 'Include',
    items: ['France', 'Germany']
});

// Refresh the Pivot Table
pivotTableObj.refresh();
```

### Clear Filters
```typescript
// Clear all filters
pivotTableObj.dataSourceSettings.filterSettings = [];
pivotTableObj.refresh();

// Clear specific filter
pivotTableObj.dataSourceSettings.filterSettings = 
    pivotTableObj.dataSourceSettings.filterSettings.filter(f => f.name !== 'Country');
pivotTableObj.refresh();
```

## Troubleshooting

### Issue: Filter not applied
**Solution:** Ensure the field name matches exactly with the data source field name. Check for case sensitivity.

```typescript
// Correct
filterSettings: [{ name: 'Country', type: 'Include', items: ['France'] }]

// Incorrect
filterSettings: [{ name: 'country', type: 'Include', items: ['France'] }]
```

### Issue: Performance issues with large datasets
**Solution:** Enable load-on-demand and limit member count:

```typescript
let pivotTableObj: PivotView = new PivotView({
    dataSourceSettings: {
        dataSource: largeData,
        rows: [{ name: 'Country' }],
        columns: [{ name: 'Year' }],
        values: [{ name: 'Sold' }]
    },
    maxNodeLimitInMemberEditor: 500,
    loadOnDemandInMemberEditor: true,
    height: 350
});
pivotTableObj.appendTo('#PivotTable');
```

### Issue: Value filter not working
**Solution:** Ensure the measure field is specified correctly:

```typescript
filterSettings: [
    {
        name: 'Country',
        type: 'Value',
        measure: 'Amount', // Must match value field name
        condition: 'GreaterThan',
        value1: '300000'
    }
]
```

### Issue: Between filter returning unexpected results
**Solution:** Ensure value1 and value2 are in correct order (value1 < value2):

```typescript
// Correct
filterSettings: [
    { name: 'Country', type: 'Label', condition: 'Between', value1: 'A', value2: 'M' }
]

// Incorrect
filterSettings: [
    { name: 'Country', type: 'Label', condition: 'Between', value1: 'M', value2: 'A' }
]
```

### Issue: Filter dialog takes too long to load
**Solution:** Reduce maxNodeLimitInMemberEditor or enable load-on-demand:

```typescript
let pivotTableObj: PivotView = new PivotView({
    dataSourceSettings: {
        dataSource: largeData,
        rows: [{ name: 'Country' }],
        columns: [{ name: 'Year' }],
        values: [{ name: 'Sold' }]
    },
    maxNodeLimitInMemberEditor: 500, // Reduced from default 1000
    loadOnDemandInMemberEditor: true, // For OLAP data sources
    height: 350
});
pivotTableObj.appendTo('#PivotTable');
```

## Events

### memberFiltering
Triggered when applying member filter.

```typescript
memberFiltering: (args: MemberFilteringEventArgs) => {
    // args.filterSettings - Current filter configuration
    // args.cancel - Set to true to cancel filtering
}
```

### memberEditorOpen
Triggered when opening member filter editor.

```typescript
memberEditorOpen: (args: MemberEditorOpenEventArgs) => {
    // args.fieldName - Field being filtered
    // args.fieldMembers - Available members
    // args.cancel - Set to true to cancel opening
}
```

### actionBegin / actionComplete / actionFailure
General action events for filtering operations.

```typescript
actionBegin: (args) => {
    if (args.actionName === 'Filter field') {
        // Filter action starting
    }
},
actionComplete: (args) => {
    if (args.actionName === 'Field filtered') {
        // Filter applied successfully
    }
},
actionFailure: (args) => {
    console.error('Filter failed:', args);
}
```
