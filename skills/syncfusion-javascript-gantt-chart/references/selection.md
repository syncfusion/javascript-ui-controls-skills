# Selection in Syncfusion Gantt Chart

## Table of Contents
- [Basic Setup](#basic-setup)
- [Selection Settings](#selection-settings)
- [Hover Highlighting](#hover-highlighting)
- [Row Selection](#row-selection)
- [Cell Selection](#cell-selection)
- [Programmatic Selection](#programmatic-selection)
- [Get Selected Items](#get-selected-items)
- [Clear Selection](#clear-selection)
- [Touch Interaction](#touch-interaction)

---

## Basic Setup

**Module required**: `Selection`

```typescript
import { Gantt, Selection } from '@syncfusion/ej2-gantt';

Gantt.Inject(Selection);

let gantt: Gantt = new Gantt({
    dataSource: GanttData,
    height: '450px',
    taskFields: { id: 'TaskID', name: 'TaskName', startDate: 'StartDate', duration: 'Duration', progress: 'Progress', parentID: 'ParentID' },
    selectionSettings: {
        mode: 'Row',       // 'Row' | 'Cell' | 'Both'
        type: 'Multiple',  // 'Single' | 'Multiple'
        enableToggle: false
    }
});
gantt.appendTo('#Gantt');
```

> Set `allowSelection: false` to disable selection entirely. `Row` mode is the default.

---

## Selection Settings

| Property | Options | Description |
|----------|---------|-------------|
| `mode` | `'Row'` \| `'Cell'` \| `'Both'` | What can be selected (default: `'Row'`) |
| `type` | `'Single'` \| `'Multiple'` | How many items can be selected (default: `'Single'`) |
| `enableToggle` | boolean | Click selected item again to deselect (default: `false`) |

---

## Hover Highlighting

Highlights tree grid rows, taskbars, header cells, and timeline cells on hover:

```typescript
enableHover: true,
selectionSettings: { mode: 'Row', type: 'Multiple' }
```

---

## Row Selection

### Select Row on Initial Load

Use `selectedRowIndex` to pre-select a row when the Gantt loads:

```typescript
selectedRowIndex: 3
```

### Select Rows Dynamically

```typescript
gantt.selectionModule.selectRow(2);              // select single row by index
gantt.selectionModule.selectRows([1, 2, 3]);     // select multiple rows by index array
```

### Multiple Row Selection

Hold **Ctrl** and click to select multiple rows:

```typescript
selectionSettings: { mode: 'Row', type: 'Multiple' }
```

### Prevent Row Selection

Use `rowSelecting` to cancel selection of specific rows. `rowSelected` fires after selection completes:

```typescript
rowSelecting: function(args: any) {
    if (args.data.TaskID === 4) {
        args.cancel = true;   // prevent selecting this row
    }
},
rowSelected: function(args: any) {
    console.log('Row selected:', args.data);
}
```

### Row Deselection Events

Clicking a selected row deselects it. Use `rowDeselecting` to prevent deselection of specific rows:

```typescript
rowDeselecting: function(args: any) {
    // args.cancel = true to prevent deselection
    console.log('Deselecting row:', args.data.TaskID);
},
rowDeselected: function(args: any) {
    console.log('Row deselected:', args.data);
}
```

---

## Cell Selection

Enable by setting `mode: 'Cell'`. Use `getSelectedRowCellIndexes()` to retrieve selected cell info (returns `{ cellIndexes, rowIndex }[]`).

```typescript
selectionSettings: { mode: 'Cell' }
```

### Multiple Cell Selection

Hold **Ctrl** and click to select multiple cells:

```typescript
selectionSettings: { mode: 'Cell', type: 'Multiple' }
```

### Select Cell Dynamically

```typescript
gantt.selectionModule.selectCell({ rowIndex: 1, cellIndex: 1 });
```

### Prevent Cell Selection

Use `cellSelecting` to cancel selection of specific cells. `cellSelected` fires after selection completes:

```typescript
cellSelecting: function(args: any) {
    if (args.data.TaskID === 4 && args.cellIndex.cellIndex === 1) {
        args.cancel = true;   // prevent selecting this cell
    }
},
cellSelected: function(args: any) {
    console.log('Cell selected at row:', args.cellIndex.rowIndex);
}
```

> **Limitation**: Cell selection is not supported when virtualization (`enableVirtualization`) is enabled.

---

## Programmatic Selection

```typescript
// Select rows by condition in dataBound
dataBound: function() {
    const rowIndexes: number[] = [];
    (gantt.treeGrid.grid.dataSource as any[]).forEach((data, index) => {
        if (data.TaskID === 3 || data.TaskID === 4) rowIndexes.push(index);
    });
    gantt.selectRows(rowIndexes);
}
```

---

## Get Selected Items

```typescript
rowSelected: function(args: any) {
    const indexes: number[] = gantt.selectionModule.getSelectedRowIndexes();
    const records: object[] = gantt.selectionModule.getSelectedRecords();
    const cellIndexes = gantt.selectionModule.getSelectedRowCellIndexes(); // cell mode
    console.log('Selected indexes:', indexes);
    console.log('Selected records:', records);
}
```

| Method | Returns | Description |
|--------|---------|-------------|
| `getSelectedRowIndexes()` | `number[]` | Indexes of all selected rows |
| `getSelectedRecords()` | `object[]` | Full data objects of selected rows |
| `getSelectedRowCellIndexes()` | `object[]` | `{ cellIndexes, rowIndex }` of selected cells |

---

## Clear Selection

```typescript
gantt.clearSelection();   // clears all selected rows and cells
```

---

## Touch Interaction

- **Tap** a row → selects that row (single row selection)
- **Multi-row selection** → tap a row; a popup appears indicating multi-select mode; tap the popup, then tap additional rows to select them
