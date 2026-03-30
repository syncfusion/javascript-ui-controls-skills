# Columns in Syncfusion Gantt Chart

## Table of Contents
- [Defining Columns](#defining-columns)
- [Column Properties](#column-properties)
- [Column Header Customization](#column-header-customization)
- [Column Formatting](#column-formatting)
- [Tree / Expander Column](#tree--expander-column)
- [Show / Hide Columns](#show--hide-columns)
- [Column Type](#column-type)
- [Controlling Column Actions](#controlling-column-actions)
- [Checkbox Columns](#checkbox-columns)
- [Column Template](#column-template)
- [Responsive Columns](#responsive-columns)
- [Column Spanning](#column-spanning)

## Defining Columns

Use the `columns` property to define grid columns. If not defined, default columns are rendered based on `taskFields` mappings.

```typescript
import { Gantt } from '@syncfusion/ej2-gantt';

let gantt: Gantt = new Gantt({
    dataSource: GanttData,
    taskFields: { id: 'TaskID', name: 'TaskName', startDate: 'StartDate', duration: 'Duration', progress: 'Progress', parentID: 'ParentID' },
    height: '450px',
    splitterSettings: { columnIndex: 2 },
    columns: [
        { field: 'TaskID', width: '150' },
        { field: 'TaskName', width: '250' }
    ]
});
gantt.appendTo('#Gantt');
```

---

## Column Properties

| Property | Type | Description |
|----------|------|-------------|
| `field` | string | Data source field name to bind |
| `headerText` | string | Column header display text |
| `width` | string \| number | Column width |
| `minWidth` | string \| number | Minimum width during resize |
| `maxWidth` | string \| number | Maximum width during resize |
| `textAlign` | string | `'Left'` \| `'Center'` \| `'Right'` \| `'Justify'` |
| `type` | string | `'string'` \| `'number'` \| `'boolean'` \| `'date'` \| `'dateTime'` |
| `format` | string \| object | Number/date format string or object |
| `visible` | boolean | Show/hide column initially |
| `allowEditing` | boolean | Per-column edit control |
| `allowFiltering` | boolean | Per-column filter control |
| `allowSorting` | boolean | Per-column sort control |
| `allowReordering` | boolean | Per-column reorder control |
| `allowResizing` | boolean | Per-column resize control |
| `showColumnMenu` | boolean | Show/hide column menu icon for this column |
| `template` | string | Cell template selector (HTML template ID) |
| `headerTemplate` | string | Header template selector |
| `displayAsCheckBox` | boolean | Render boolean values as checkboxes |
| `hideAtMedia` | string | CSS media query to hide the column |
| `isFrozen` | boolean | Freeze this column |
| `freeze` | string | `'Left'` \| `'Right'` \| `'Fixed'` — freeze direction |

---

## Column Header Customization

Use `headerText` for a custom label and `headerTemplate` for a full custom header:

```typescript
columns: [
    { field: 'TaskName', headerTemplate: '#projectName', width: '150' },
    { field: 'StartDate', headerTemplate: '#dateTemplate', width: '150' },
    { field: 'Duration', headerText: 'Duration (Days)', width: '150' }
]
```

---

## Column Formatting

### Number Formatting

| Format | Description |
|--------|-------------|
| `'N2'` | Numeric with 2 decimals |
| `'C2'` | Currency with 2 decimals |
| `'P2'` | Percentage (input 0–100) |

### Date Formatting

```typescript
columns: [
    // Built-in date skeleton
    { field: 'StartDate', format: 'yMd' },
    // Custom format string
    { field: 'EndDate', format: { format: 'dd/MM/yyyy', type: 'date' } },
    // DateTime format
    { field: 'CreatedAt', format: { type: 'dateTime', format: 'dd/MM/yyyy hh:mm a' } }
]
```

| Format | Output Example |
|--------|---------------|
| `{ type:'date', format:'dd/MM/yyyy' }` | 04/07/2019 |
| `{ type:'date', format:'dd.MM.yyyy' }` | 04.07.2019 |
| `{ type:'date', skeleton:'short' }` | 7/4/19 |
| `{ type: 'dateTime', format: 'dd/MM/yyyy hh:mm a' }` | 04/07/2019 12:00 AM |

> Default culture for number/date formatting is `en-US`.

---

## Tree / Expander Column

The `treeColumnIndex` property specifies which column index has the expand/collapse icons (default: `0`).

```typescript
let gantt: Gantt = new Gantt({
    dataSource: GanttData,
    taskFields: { id: 'TaskID', name: 'TaskName', /* ... */ },
    treeColumnIndex: 2  // expander icons in 3rd column
});
```

---

## Show / Hide Columns

Toggle column visibility programmatically:

```typescript
import { Gantt, Edit, Selection } from '@syncfusion/ej2-gantt';

Gantt.Inject(Edit, Selection);

let gantt: Gantt = new Gantt({ /* config */ });
gantt.appendTo('#Gantt');

// Show columns by field name
gantt.showColumn(['TaskName', 'Duration']);

// Hide columns by field name
gantt.hideColumn(['TaskName', 'Duration']);
```

Use `visible: false` in the column definition for initially hidden columns.

---

## Column Type

Explicitly set column data type to ensure correct formatting:

```typescript
columns: [
    { field: 'Progress', type: 'number', format: 'N2' },
    { field: 'StartDate', type: 'date', format: 'yMd' },
    { field: 'verified', type: 'boolean', displayAsCheckBox: true }
]
```

> If `type` is not defined, it is inferred from the first record. Define it explicitly when the first record has null/blank values.

---

## Controlling Column Actions

Disable or enable specific column actions per-column:

```typescript
import { Gantt, Filter, Sort, Resize, Reorder, Selection, Edit } from '@syncfusion/ej2-gantt';

Gantt.Inject(Edit, Selection, Filter, Sort, Resize, Reorder);

let gantt: Gantt = new Gantt({
    allowFiltering: true,
    allowSorting: true,
    allowReordering: true,
    editSettings: { allowEditing: true },
    columns: [
        { field: 'TaskID' },
        { field: 'Progress', allowReordering: false },
        { field: 'TaskName', allowSorting: false },
        { field: 'StartDate', allowEditing: false },
        { field: 'Duration', allowFiltering: false }
    ]
});
```

---

## Checkbox Columns

Render boolean fields as checkboxes with `displayAsCheckBox: true`:

```typescript
columns: [
    { field: 'TaskID' },
    { field: 'TaskName' },
    { field: 'verified', headerText: 'Verified', displayAsCheckBox: true, type: 'boolean' }
]
```

---

## Column Template

Customize cell content using HTML templates. Reference the template element by its ID:

```typescript
// In HTML: <script id="columnTemplate" type="text/x-template">
//   <div class="resource-cell">
//     <img src="${imageUrl}" class="resource-icon" />
//     <span>${ResourceName}</span>
//   </div>
// </script>

columns: [
    { field: 'TaskID' },
    { field: 'TaskName' },
    { field: 'Resources', headerText: 'Resources', width: '250', template: '#columnTemplate' }
]
```

Use `rowHeight` when templates contain larger content (e.g., images).

---

## Responsive Columns

Hide columns based on CSS media queries using `hideAtMedia`:

```typescript
columns: [
    { field: 'TaskName', hideAtMedia: '(min-width: 700px)' }, // hidden when viewport >= 700px
    { field: 'Duration', hideAtMedia: '(max-width: 500px)' }  // hidden when viewport <= 500px
]
```

---

## Column Spanning

Span adjacent cells using the `queryCellInfo` event by setting `args.colSpan`:

```typescript
import { Gantt, Edit, Selection, QueryCellInfoEventArgs } from '@syncfusion/ej2-gantt';

Gantt.Inject(Edit, Selection);

let gantt: Gantt = new Gantt({
    queryCellInfo: (args: QueryCellInfoEventArgs) => {
        if (args.column.field === 'work1' && args.data.taskData.work1 === 'support') {
            args.colSpan = 2;  // span this cell over next 2 columns
        }
    }
});
```
