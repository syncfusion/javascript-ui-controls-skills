# Task Dependency in Syncfusion Gantt Chart

## Table of Contents
- [Dependency Types](#dependency-types)
- [Basic Configuration](#basic-configuration)
- [Multiple Predecessors](#multiple-predecessors)
- [Offsets (Lag & Lead)](#offsets-lag--lead)
- [Parent Dependencies](#parent-dependencies)
- [Connector Line Customization](#connector-line-customization)
- [Dependency Editing (Taskbar)](#dependency-editing-taskbar)
- [Programmatic Methods](#programmatic-methods)
- [Validation on Edit](#validation-on-edit)
- [Validation Dialog](#validation-dialog)
- [Predecessor Offset Synchronization on Initial Load](#predecessor-offset-synchronization-on-initial-load)
- [Disable Automatic Dependency Offset Updates](#disable-automatic-dependency-offset-updates)
- [Show or Hide Dependency Lines Dynamically](#show-or-hide-dependency-lines-dynamically)
- [Disable Predecessor Validation](#disable-predecessor-validation)

---

## Dependency Types

| Type | Format | Description |
|------|--------|-------------|
| Finish to Start | `'2FS'` | Task starts after task 2 finishes (most common) |
| Start to Start | `'2SS'` | Task starts when task 2 starts |
| Finish to Finish | `'2FF'` | Task finishes when task 2 finishes |
| Start to Finish | `'2SF'` | Task finishes when task 2 starts |

---

## Basic Configuration

Map the predecessor field in `taskFields.dependency`:

```typescript
import { Gantt } from '@syncfusion/ej2-gantt';

let data: Object[] = [
    { TaskID: 1, TaskName: 'Project Initiation', StartDate: new Date('04/02/2024'), EndDate: new Date('04/21/2024') },
    { TaskID: 2, TaskName: 'Identify Site location', StartDate: new Date('04/02/2024'), Duration: 4, Progress: 50, ParentID: 1 },
    { TaskID: 3, TaskName: 'Perform Soil test', StartDate: new Date('04/02/2024'), Duration: 4, Progress: 50, ParentID: 1 },
    { TaskID: 4, TaskName: 'Soil test approval', StartDate: new Date('04/02/2024'), Duration: 4,
      Predecessor: '2FS', Progress: 50, ParentID: 1 },   // starts after task 2 finishes
    { TaskID: 5, TaskName: 'Project Estimation', StartDate: new Date('04/02/2024'), EndDate: new Date('04/21/2024') },
    { TaskID: 6, TaskName: 'Develop floor plan', StartDate: new Date('04/04/2024'), Duration: 3, Progress: 50, ParentID: 5 },
    { TaskID: 7, TaskName: 'List materials', StartDate: new Date('04/04/2024'), Duration: 3, Progress: 50, ParentID: 5 },
    { TaskID: 8, TaskName: 'Estimation approval', StartDate: new Date('04/04/2024'), Duration: 0,
      Predecessor: '6SS', Progress: 50, ParentID: 5 }   // starts when task 6 starts
];

let gantt: Gantt = new Gantt({
    dataSource: data,
    height: '450px',
    taskFields: {
        id: 'TaskID', name: 'TaskName', startDate: 'StartDate',
        endDate: 'EndDate', duration: 'Duration', progress: 'Progress',
        dependency: 'Predecessor',   // maps the predecessor string field
        parentID: 'ParentID'
    }
});
gantt.appendTo('#Gantt');
```

---

## Multiple Predecessors

Assign comma-separated predecessor strings to define multiple dependencies on one task:

```typescript
// Task 4 depends on both task 2 (FS) AND task 3 (FS)
{ TaskID: 4, TaskName: 'Soil test approval', Predecessor: '2FS,3FS', Duration: 0 }

// Task 8 depends on task 6 (SS) AND task 5 (FF)
{ TaskID: 8, TaskName: 'Estimation approval', Predecessor: '6SS,5FF', Duration: 0 }
```

---

## Offsets (Lag & Lead)

Add a delay (lag) or advance (lead) to a dependency using `+` or `-` with a unit:

```
'2FS+3d'    → start 3 days AFTER task 2 finishes (3-day lag)
'2FS-2d'    → start 2 days BEFORE task 2 finishes (2-day lead)
'2FS+8h'    → start 8 hours after task 2 finishes (hour offset)
'2FS+30min' → start 30 minutes after task 2 finishes (minute offset)
'2SS+1d'    → start 1 day after task 2 starts
```

```typescript
let data = [
    { TaskID: 1, TaskName: 'Design', StartDate: new Date('04/02/2024'), Duration: 5 },
    { TaskID: 2, TaskName: 'Development', StartDate: new Date('04/02/2024'), Duration: 8,
      Predecessor: '1FS+2d' },     // dev starts 2 days after design finishes
    { TaskID: 3, TaskName: 'Testing', StartDate: new Date('04/02/2024'), Duration: 3,
      Predecessor: '2FS-1d' }      // testing starts 1 day before dev finishes
];
```

---

## Parent Dependencies

By default, `allowParentDependency` is `true`, enabling dependencies between:
- Parent → Parent
- Child → Child
- Parent → Child
- Child → Parent

```typescript
let gantt: Gantt = new Gantt({
    dataSource: GanttData,
    height: '450px',
    allowParentDependency: true,   // default: true
    taskFields: {
        id: 'TaskID', name: 'TaskName', startDate: 'StartDate',
        duration: 'Duration', dependency: 'Predecessor', child: 'subtasks'
    }
});
gantt.appendTo('#Gantt');
```

Set `allowParentDependency: false` to restrict dependencies to leaf tasks only.

---

## Connector Line Customization

Customize the dependency connector lines appearance:

```typescript
let gantt: Gantt = new Gantt({
    dataSource: GanttData,
    height: '450px',
    taskFields: { id: 'TaskID', name: 'TaskName', startDate: 'StartDate', dependency: 'Predecessor', child: 'subtasks' },
    connectorLineWidth: 2,                  // line thickness in pixels
    connectorLineBackground: '#ff6b6b',     // line color (CSS color string)
});
gantt.appendTo('#Gantt');
```

**Dynamic styling per dependency using `queryTaskbarInfo`:**
```typescript
queryTaskbarInfo: function (args: any) {
    if (args.data.Predecessor && args.data.Predecessor.includes('FS')) {
        args.connectorLineColor = '#4CAF50';   // green for FS dependencies
    }
}
```

---

## Dependency Editing (Taskbar)

Enable interactive dependency creation by dragging connector points on taskbars:

```typescript
import { Gantt, Edit } from '@syncfusion/ej2-gantt';

Gantt.Inject(Edit);

let gantt: Gantt = new Gantt({
    dataSource: GanttData,
    height: '450px',
    taskFields: {
        id: 'TaskID', name: 'TaskName', startDate: 'StartDate',
        duration: 'Duration', dependency: 'Predecessor', child: 'subtasks'
    },
    editSettings: {
        allowTaskbarEditing: true    // enables dragging connector points to create/edit dependencies
    }
});
gantt.appendTo('#Gantt');
```

> Hover over a taskbar to reveal connector points. Drag from one taskbar to another to create a dependency.

---

## Programmatic Methods

Manage dependencies via public API methods:

```typescript
// Add a predecessor to a task
gantt.addPredecessor(taskId: number, predecessorString: string);

// Example: make task 4 depend on task 3 (FS)
gantt.addPredecessor(4, '3FS');

// Remove a predecessor from a task
gantt.removePredecessor(taskId: number);

// Example: remove all predecessors from task 4
gantt.removePredecessor(4);

// Update predecessor string
gantt.updatePredecessor(taskId: number, predecessorString: string);
```

---

## Validation on Edit

When a dependency edit would create a conflict, use `actionBegin` with `requestType: 'validateLinkedTask'`. The `validateMode` argument defines three modes:

| Mode | Behavior |
|------|----------|
| `respectLink` | Prioritizes links — reverts invalid edits |
| `removeLink` | Prioritizes editing — removes conflicting links |
| `preserveLinkWithEditing` | Updates offsets to maintain links (default) |

```typescript
import { Gantt, Edit } from '@syncfusion/ej2-gantt';

Gantt.Inject(Edit);

let gantt: Gantt = new Gantt({
    dataSource: GanttData,
    height: '380px',
    taskFields: {
        id: 'TaskID', name: 'TaskName', startDate: 'StartDate',
        endDate: 'EndDate', dependency: 'Predecessor',
        duration: 'Duration', progress: 'Progress', parentID: 'ParentID'
    },
    editSettings: { allowTaskbarEditing: true },
    actionBegin: function (args: any) {
        if (args.requestType === 'validateLinkedTask') {
            args.validateMode.respectLink = true;   // revert edits that violate links
        }
    }
});
gantt.appendTo('#Gantt');
```

---

## Validation Dialog

When all validation modes are disabled in `actionBegin`, Gantt displays a dialog prompting the user to choose an action (e.g., remove the link, cancel the edit) based on the successor's start date relative to the predecessor.

```typescript
import { Gantt, Edit } from '@syncfusion/ej2-gantt';

Gantt.Inject(Edit);

let gantt: Gantt = new Gantt({
    dataSource: GanttData,
    height: '380px',
    taskFields: {
        id: 'TaskID', name: 'TaskName', startDate: 'StartDate',
        endDate: 'EndDate', dependency: 'Predecessor',
        duration: 'Duration', progress: 'Progress', parentID: 'ParentID'
    },
    editSettings: { allowTaskbarEditing: true },
    actionBegin: function (args: any) {
        if (args.requestType === 'validateLinkedTask') {
            args.validateMode.preserveLinkWithEditing = false;   // disables default mode → triggers dialog
        }
    }
});
gantt.appendTo('#Gantt');
```

> The dialog offers options like "Remove the link and move the task" or "Cancel" depending on the conflict type.

---

## Predecessor Offset Synchronization on Initial Load

The `autoUpdatePredecessorOffset` property controls whether the Gantt recalculates and synchronizes predecessor offset values during initial data load to match actually rendered taskbar positions (accounting for calendar rules, weekends, holidays, and working times).

| Value | Behavior |
|-------|----------|
| `true` (default) | Recalculates offsets on load — predecessor column reflects actual rendered positions |
| `false` | Displays original offset values from the data source — may cause visual mismatch |

```typescript
import { Gantt } from '@syncfusion/ej2-gantt';

let gantt: Gantt = new Gantt({
    dataSource: GanttData,
    height: '420px',
    taskFields: {
        id: 'TaskID', name: 'TaskName', startDate: 'StartDate',
        duration: 'Duration', progress: 'Progress',
        dependency: 'Predecessor', child: 'subtasks'
    },
    autoUpdatePredecessorOffset: true,   // recalculate offsets after applying calendar rules
    holidays: [
        { from: '04/04/2019', to: '04/05/2019', label: 'Public holidays' },
        { from: '04/12/2019', to: '04/12/2019', label: 'Public holiday' }
    ]
});
gantt.appendTo('#Gantt');
```

> Setting `autoUpdatePredecessorOffset: false` is useful when you want to preserve the exact offset strings from the data source without recalculation.

---

## Disable Automatic Dependency Offset Updates

By default, dependency offsets are automatically recalculated when a taskbar is edited. Set `updateOffsetOnTaskbarEdit` to `false` to preserve existing offsets, requiring manual updates via the dependency tab or predecessor column.

```typescript
import { Gantt, Edit, Toolbar } from '@syncfusion/ej2-gantt';

Gantt.Inject(Edit, Toolbar);

let gantt: Gantt = new Gantt({
    dataSource: GanttData,
    height: '450px',
    taskFields: {
        id: 'TaskID', name: 'TaskName', startDate: 'StartDate',
        dependency: 'Predecessor', duration: 'Duration',
        progress: 'Progress', parentID: 'ParentID'
    },
    updateOffsetOnTaskbarEdit: false,   // offsets unchanged when taskbar is dragged
    editSettings: {
        allowAdding: true, allowEditing: true,
        allowDeleting: true, allowTaskbarEditing: true
    },
    toolbar: ['Add', 'Edit', 'Delete', 'Update', 'Cancel']
});
gantt.appendTo('#Gantt');
```

---

## Show or Hide Dependency Lines Dynamically

Toggle the visibility of all connector/dependency lines by manipulating the CSS `visibility` property on the `.e-gantt-dependency-view-container` element:

```typescript
import { Gantt } from '@syncfusion/ej2-gantt';
import { Switch } from '@syncfusion/ej2-buttons';

let switchObj: Switch = new Switch({ checked: false, change: onToggle });
switchObj.appendTo('#switch');

function onToggle() {
    const container = document.querySelector('.e-gantt-dependency-view-container') as HTMLElement;
    container.style.visibility = switchObj.checked ? 'hidden' : 'visible';
}

let gantt: Gantt = new Gantt({
    dataSource: GanttData,
    height: '420px',
    taskFields: {
        id: 'TaskID', name: 'TaskName', startDate: 'StartDate',
        dependency: 'Predecessor', duration: 'Duration',
        progress: 'Progress', parentID: 'ParentID'
    },
    editSettings: { allowTaskbarEditing: true }
});
gantt.appendTo('#Gantt');
```

> This approach is purely visual — it hides the SVG connector lines without affecting the underlying dependency data.

---

## Disable Predecessor Validation

By default, Gantt validates and adjusts task dates based on predecessor values. Set `enablePredecessorValidation` to `false` to allow tasks to retain their original dates regardless of predecessor constraints.

```typescript
import { Gantt } from '@syncfusion/ej2-gantt';

let gantt: Gantt = new Gantt({
    dataSource: GanttData,
    height: '380px',
    taskFields: {
        id: 'TaskID', name: 'TaskName', startDate: 'StartDate',
        endDate: 'EndDate', dependency: 'Predecessor',
        duration: 'Duration', progress: 'Progress', parentID: 'ParentID'
    },
    enablePredecessorValidation: false   // tasks keep their dates; predecessor lines are drawn but not enforced
});
gantt.appendTo('#Gantt');
```

> Use this when you want to visualize existing dependency relationships without allowing the Gantt to reschedule tasks automatically.
