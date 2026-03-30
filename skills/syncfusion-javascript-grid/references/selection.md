---
name: selection-modes
description: 'Selection modes in Syncfusion Grid: row, cell, column, checkbox, multi-select.'
---

# Selection Modes

## Table of Contents
- [Row Selection](#row-selection)
- [Cell Selection](#cell-selection)
- [Column Selection](#column-selection)
- [Checkbox Selection](#checkbox-selection)
- [Multi-Select Configurations](#multi-select-configurations)

## When to Use This Reference

- Configure row, cell, or range selection modes
- Implement multi-select with checkboxes
- Handle selection changed events
- Get selected rows or cells programmatically
- Apply custom selection styling and behavior

## Row Selection

### Enable Row Selection

```ts
import { Grid, Selection, Page } from '@syncfusion/ej2-grids';

Grid.Inject(Selection, Page);

const data = [
  { OrderID: 10248, CustomerName: 'VINET', OrderDate: new Date(1996, 6, 4), Freight: 32.38 },
  { OrderID: 10249, CustomerName: 'TOMSP', OrderDate: new Date(1996, 6, 5), Freight: 11.61 }
];

const grid = new Grid({
  dataSource: data,
  columns: [
    { field: 'OrderID', headerText: 'Order ID', width: 100 },
    { field: 'CustomerName', headerText: 'Customer Name', width: 150 },
    { field: 'OrderDate', headerText: 'Order Date', width: 130, type: 'date' },
    { field: 'Freight', headerText: 'Freight', width: 120 }
  ],
  allowSelection: true,
  selectionSettings: {
    type: 'Multiple',
    mode: 'Row'
  }
});

grid.appendTo('#grid');
```

### Single Row Selection

```ts
const grid = new Grid({
  dataSource: data,
  columns: [
    { field: 'OrderID', headerText: 'Order ID', width: 100 },
    { field: 'CustomerName', headerText: 'Customer Name', width: 150 }
  ],
  allowSelection: true,
  selectionSettings: {
    type: 'Single',
    mode: 'Row'
  }
});

grid.appendTo('#grid');
```

## Cell Selection

### Enable Cell Selection

```ts
import { Grid, Selection } from '@syncfusion/ej2-grids';

Grid.Inject(Selection);

const grid = new Grid({
  dataSource: data,
  columns: [
    { field: 'OrderID', headerText: 'Order ID', width: 100 },
    { field: 'CustomerName', headerText: 'Customer Name', width: 150 },
    { field: 'Freight', headerText: 'Freight', width: 120 }
  ],
  allowSelection: true,
  selectionSettings: {
    type: 'Multiple',
    mode: 'Cell'
  }
});

grid.appendTo('#grid');
```

## Column Selection

### Enable Column Selection

```ts
import { Grid, Selection } from '@syncfusion/ej2-grids';

Grid.Inject(Selection);

const grid = new Grid({
  dataSource: data,
  columns: [
    { field: 'OrderID', headerText: 'Order ID', width: 100 },
    { field: 'CustomerName', headerText: 'Customer Name', width: 150 },
    { field: 'Freight', headerText: 'Freight', width: 120 }
  ],
  allowSelection: true,
  selectionSettings: {
    type: 'Single',
    mode: 'Column'
  }
});

grid.appendTo('#grid');
```

## Checkbox Selection

### Enable Checkbox Column

```ts
import { Grid, Selection, Toolbar, Page } from '@syncfusion/ej2-grids';

Grid.Inject(Selection, Toolbar, Page);

const grid = new Grid({
  dataSource: data,
  columns: [
    { type: 'checkbox', width: 50 },
    { field: 'OrderID', headerText: 'Order ID', width: 100 },
    { field: 'CustomerName', headerText: 'Customer Name', width: 150 },
    { field: 'Freight', headerText: 'Freight', width: 120 }
  ],
  toolbar: ['Add', 'Edit', 'Delete'],
  allowSelection: true,
  selectionSettings: {
    type: 'Multiple',
    mode: 'Row'
  }
});

grid.appendTo('#grid');
```

### Checkbox with Events

```ts
const grid = new Grid({
  dataSource: data,
  columns: [
    { type: 'checkbox', width: 50 },
    { field: 'OrderID', headerText: 'Order ID', width: 100 },
    { field: 'CustomerName', headerText: 'Customer Name', width: 150 }
  ],
  rowSelecting: (args: any) => {
    console.log('Row selecting:', args.rowIndex);
  },
  rowSelected: (args: any) => {
    console.log('Row selected:', args.rowIndex);
  },
  rowDeselecting: (args: any) => {
    console.log('Row deselecting:', args.rowIndex);
  },
  allowSelection: true,
  selectionSettings: { type: 'Multiple', mode: 'Row' }
});

grid.appendTo('#grid');
```

## Multi-Select Configurations

### Get Selected Rows

```ts
const grid = new Grid({
  dataSource: data,
  columns: [
    { type: 'checkbox', width: 50 },
    { field: 'OrderID', headerText: 'Order ID', width: 100 },
    { field: 'CustomerName', headerText: 'Customer Name', width: 150 }
  ],
  allowSelection: true,
  selectionSettings: { type: 'Multiple', mode: 'Row' }
});

grid.appendTo('#grid');

// Get selected row indexes
const selectedRowIndexes = grid.getSelectedRowIndexes();
console.log('Selected rows:', selectedRowIndexes);

// Get selected records
const selectedRecords = grid.getSelectedRecords();
console.log('Selected data:', selectedRecords);
```

### Programmatic Selection

```ts
const grid = new Grid({
  dataSource: data,
  columns: [
    { field: 'OrderID', headerText: 'Order ID', width: 100 },
    { field: 'CustomerName', headerText: 'Customer Name', width: 150 }
  ],
  allowSelection: true,
  selectionSettings: { type: 'Multiple', mode: 'Row' }
});

grid.appendTo('#grid');

// Select row by index
grid.selectRow(0);
grid.selectRow(2);

// Select all rows
grid.selectAll();

// Clear selection
grid.clearSelection();
```

### Clear Selection Button

```ts
import { Grid, Selection, Toolbar } from '@syncfusion/ej2-grids';

Grid.Inject(Selection, Toolbar);

const grid = new Grid({
  dataSource: data,
  columns: [
    { type: 'checkbox', width: 50 },
    { field: 'OrderID', headerText: 'Order ID', width: 100 },
    { field: 'CustomerName', headerText: 'Customer Name', width: 150 }
  ],
  toolbar: [
    'Add',
    { text: 'Clear Selection', tooltipText: 'Clear', prefixIcon: 'e-icons e-clear' }
  ],
  toolbarClick: (args: any) => {
    if (args.item.text === 'Clear Selection') {
      grid.clearSelection();
    }
  },
  allowSelection: true,
  selectionSettings: { type: 'Multiple', mode: 'Row' }
});

grid.appendTo('#grid');
```

