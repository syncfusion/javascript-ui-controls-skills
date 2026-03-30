# Task Scheduling in Syncfusion Gantt Chart

## Table of Contents
- [Scheduling Modes](#scheduling-modes)
- [Auto Scheduling](#auto-scheduling)
- [Manual Scheduling](#manual-scheduling)
- [Custom (Mixed) Scheduling](#custom-mixed-scheduling)
- [Unscheduled Tasks](#unscheduled-tasks)
- [Duration Units](#duration-units)
- [Milestone Tasks](#milestone-tasks)
- [Project Date Range](#project-date-range)
- [Working Time & Weekends](#working-time--weekends)

---

## Scheduling Modes

The `taskMode` property controls how task dates are validated:

| Mode | Value | Behaviour |
|------|-------|-----------|
| Auto | `'Auto'` | Dates validated automatically based on dependencies, working days, holidays (default) |
| Manual | `'Manual'` | User controls start/end dates; no automatic recalculation |
| Custom | `'Custom'` | Mix of auto and manual; controlled per task via a data source field |

---

## Auto Scheduling

Default mode. Parent taskbar dates derive from minimum child start and maximum child end dates. Dependencies shift successors automatically.

```typescript
import { Gantt, Toolbar, Edit, Selection } from '@syncfusion/ej2-gantt';
import { GanttData } from './datasource';

Gantt.Inject(Toolbar, Edit, Selection);

let gantt: Gantt = new Gantt({
    dataSource: GanttData,
    height: '450px',
    taskMode: 'Auto',        // default — can omit
    taskFields: {
        id: 'TaskID', name: 'TaskName', startDate: 'StartDate',
        duration: 'Duration', progress: 'Progress', endDate: 'EndDate',
        dependency: 'Predecessor', child: 'Children'
    },
    toolbar: ['Add', 'Edit', 'Update', 'Delete', 'Cancel', 'ExpandAll', 'CollapseAll'],
    editSettings: { allowEditing: true, allowDeleting: true, allowTaskbarEditing: true }
});
gantt.appendTo('#Gantt');
```

**Auto scheduling rules:**
- Parent taskbar = min(child startDates) → max(child endDates)
- Parent progress, endDate cannot be edited directly
- Changes to child dates propagate up to the parent
- Dependencies adjust successor start dates automatically

---

## Manual Scheduling

When `taskMode` is `'Manual'`, the user has full control over all task dates. No automatic recalculation occurs when dependencies or children change.

```typescript
let gantt: Gantt = new Gantt({
    dataSource: GanttData,
    height: '450px',
    taskMode: 'Manual',
    validateManualTasksOnLinking: true,   // validate dates based on predecessors even in manual mode
    taskFields: {
        id: 'TaskID', name: 'TaskName', startDate: 'StartDate',
        duration: 'Duration', progress: 'Progress', endDate: 'EndDate',
        child: 'Children'
    },
    editSettings: { allowEditing: true, allowTaskbarEditing: true }
});
gantt.appendTo('#Gantt');
```

> Set `validateManualTasksOnLinking: true` to automatically validate task dates based on predecessor values while keeping all other scheduling in manual mode.

**When to use manual mode:**
- Fixed-price contracts where dates must not shift
- Projects with hard delivery dates regardless of dependency changes
- When you want to show planned vs. actual without auto-correction

---

## Custom (Mixed) Scheduling

Mix auto and manual tasks in the same project. Map a boolean field from the data source to `taskFields.manual`.

```typescript
let mixedData: Object[] = [
    { TaskID: 1, TaskName: 'Auto Task', StartDate: new Date('04/02/2024'), Duration: 4, IsManual: false },
    { TaskID: 2, TaskName: 'Manual Task', StartDate: new Date('04/02/2024'), Duration: 4, IsManual: true, ParentId: 1 }
];

let gantt: Gantt = new Gantt({
    dataSource: mixedData,
    height: '450px',
    taskMode: 'Custom',
    taskFields: {
        id: 'TaskID', name: 'TaskName', startDate: 'StartDate',
        duration: 'Duration', parentID: 'ParentId',
        manual: 'IsManual'    // true = manual, false = auto
    }
});
gantt.appendTo('#Gantt');
```

---

## Unscheduled Tasks

Unscheduled tasks have no definite schedule — they can have only start date, only end date, only duration, or none. Enable with `allowUnscheduledTasks: true`.

```typescript
let gantt: Gantt = new Gantt({
    dataSource: GanttData,
    height: '450px',
    allowUnscheduledTasks: true,
    taskFields: {
        id: 'TaskID', name: 'TaskName', startDate: 'StartDate',
        endDate: 'EndDate', duration: 'Duration', progress: 'Progress', parentID: 'ParentID'
    },
    editSettings: { allowEditing: true, allowTaskbarEditing: true }
});
gantt.appendTo('#Gantt');
```

Taskbar rendering by available data:

| Taskbar State | Auto Mode | Manual Mode |
|---------------|-----------|-------------|
| Start date only | Taskbar from start, no fixed end | Same, no end constraint |
| End date only | Taskbar ending at end date | Same, no start constraint |
| Duration only | Taskbar from project start | Same |
| No dates (milestone) | Diamond at project start | Diamond at project start |

> If `allowUnscheduledTasks` is `false`, Gantt auto-fills missing dates: defaults to duration `1` and uses `projectStartDate` as the task start date.

---

## Duration Units

By default, duration is in **days**. Change with `durationUnit`:

```typescript
let gantt: Gantt = new Gantt({
    dataSource: GanttData,
    height: '450px',
    durationUnit: 'Hour',   // 'Day' | 'Hour' | 'Minute'
    taskFields: {
        id: 'TaskID', name: 'TaskName', startDate: 'StartDate',
        duration: 'Duration', child: 'subtasks'
    }
});
gantt.appendTo('#Gantt');
```

**Map per-task duration unit from data source** using `taskFields.durationUnit`:

```typescript
let gantt: Gantt = new Gantt({
    dataSource: GanttData,
    height: '450px',
    taskFields: {
        id: 'TaskID', name: 'TaskName', startDate: 'StartDate',
        duration: 'Duration', progress: 'Progress',
        durationUnit: 'DurationUnit',   // field in data source: 'day' | 'hour' | 'minute'
        parentID: 'ParentID'
    }
});
gantt.appendTo('#Gantt');
```

**Duration unit inline in data source strings:**
```
Duration: '4days'   → 4 days
Duration: '8hours'  → 8 hours
Duration: '30min'   → 30 minutes
```

> The duration column edit type is `string` to support inline unit values (e.g., `'4hours'`). The `durationUnit` property default is `'day'`.

---

## Milestone Tasks

A task with `Duration: 0` is rendered as a milestone (diamond shape):

```typescript
let data = [
    { TaskID: 1, TaskName: 'Project Start', StartDate: new Date('04/02/2024'), Duration: 0, Progress: 0 },
    { TaskID: 2, TaskName: 'Analysis', StartDate: new Date('04/02/2024'), Duration: 5, Progress: 50 }
];

let gantt: Gantt = new Gantt({
    dataSource: data,
    height: '450px',
    taskFields: { id: 'TaskID', name: 'TaskName', startDate: 'StartDate', duration: 'Duration' }
});
gantt.appendTo('#Gantt');
```

> Milestones represent zero-duration events/checkpoints like approvals, deliverables, or phase gates.

---

## Project Date Range

Constrain the chart to a specific date range with `projectStartDate` and `projectEndDate`:

```typescript
let gantt: Gantt = new Gantt({
    dataSource: GanttData,
    height: '450px',
    projectStartDate: new Date('03/25/2024'),
    projectEndDate: new Date('07/28/2024'),
    taskFields: { id: 'TaskID', name: 'TaskName', startDate: 'StartDate', child: 'subtasks' }
});
gantt.appendTo('#Gantt');
```

- If omitted, the chart auto-calculates range from data
- `projectEndDate` is also used as reference for critical path slack calculation

---

## Working Time & Weekends

Configure the working week and daily hours:

```typescript
let gantt: Gantt = new Gantt({
    dataSource: GanttData,
    height: '450px',
    taskFields: { id: 'TaskID', name: 'TaskName', startDate: 'StartDate', duration: 'Duration', child: 'subtasks' },
    dayWorkingTime: [
        { from: 8, to: 12 },    // morning shift: 8 AM – 12 PM
        { from: 13, to: 17 }    // afternoon shift: 1 PM – 5 PM
    ],
    highlightWeekends: true,     // visually shade Saturday/Sunday
    workWeek: ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday']  // default
});
gantt.appendTo('#Gantt');
```

**Week-specific working hours** — use `weekWorkingTime` to set different hours per day of week:

```typescript
let gantt: Gantt = new Gantt({
    dataSource: GanttData,
    height: '450px',
    taskFields: { id: 'TaskID', name: 'TaskName', startDate: 'StartDate', duration: 'Duration', parentID: 'ParentID' },
    weekWorkingTime: [
        { dayOfWeek: 'Monday',  timeRange: [{ from: 10, to: 18 }] },
        { dayOfWeek: 'Tuesday', timeRange: [{ from: 10, to: 18 }] }
        // Days not listed fall back to dayWorkingTime
    ],
    highlightWeekends: true
});
gantt.appendTo('#Gantt');
```

**Make weekends working days:**

```typescript
// Include weekends in scheduling — Saturdays and Sundays become working days
includeWeekend: true
```

**Key properties:**

| Property | Type | Description |
|----------|------|-------------|
| `dayWorkingTime` | `DayWorkingTime[]` | Working hour ranges applied to all working days |
| `weekWorkingTime` | `WeekWorkingTime[]` | Per-day working hours; overrides `dayWorkingTime` for listed days |
| `highlightWeekends` | `boolean` | Shade non-working weekends in chart |
| `workWeek` | `string[]` | Days considered working days (default: Mon–Fri) |
| `includeWeekend` | `boolean` | `true` makes Saturdays and Sundays working days |

**Priority rules:**
- `weekWorkingTime` takes priority over `dayWorkingTime` for days listed in it
- Days not listed in `weekWorkingTime` use `dayWorkingTime` (default: 8–12 and 13–17)
- If a day is a holiday or non-working day **and** listed in `weekWorkingTime`, it remains non-working

> Weekends and non-working hours are skipped when calculating auto-scheduled task end dates.
