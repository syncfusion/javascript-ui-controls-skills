# Baseline in Syncfusion Gantt Chart

## Table of Contents
- [Overview](#overview)
- [Data Source Configuration](#data-source-configuration)
- [Enabling Baseline Display](#enabling-baseline-display)
- [Baseline Milestones](#baseline-milestones)
- [Customizing Baseline Appearance](#customizing-baseline-appearance)

---

## Overview

The baseline feature displays the **original planned schedule** alongside the actual current schedule, allowing visual comparison of planned vs. actual progress. A thin baseline bar appears beneath each taskbar showing where the task was originally planned.

Use baselines to:
- Track schedule deviations (early/late delivery)
- Assess project performance against the original plan
- Identify which tasks are ahead or behind schedule

---

## Data Source Configuration

Add baseline date fields to your data source and map them in `taskFields`:

| taskFields property | Description |
|--------------------|-------------|
| `baselineStartDate` | Originally planned start date |
| `baselineEndDate` | Originally planned end date |
| `baselineDuration` | Originally planned duration (string or number). Set to `'0'` for milestone baseline. |

```typescript
let projectData: Object[] = [
    {
        TaskID: 1, TaskName: 'Project Planning',
        StartDate: new Date('04/05/2024'),   // actual start (later than planned)
        EndDate: new Date('04/10/2024'),
        Duration: 5, Progress: 80,
        BaselineStartDate: new Date('04/02/2024'),   // planned start
        BaselineEndDate: new Date('04/06/2024'),     // planned end
        BaselineDuration: '5'
    },
    {
        TaskID: 2, TaskName: 'Design Phase',
        StartDate: new Date('04/11/2024'),
        EndDate: new Date('04/16/2024'),
        Duration: 5, Progress: 50, ParentID: 1,
        BaselineStartDate: new Date('04/08/2024'),
        BaselineEndDate: new Date('04/12/2024'),
        BaselineDuration: '5'
    },
    {
        TaskID: 3, TaskName: 'Milestone: Design Complete',
        StartDate: new Date('04/17/2024'),
        EndDate: new Date('04/17/2024'),
        Duration: 0, Progress: 0, ParentID: 1,
        BaselineStartDate: new Date('04/13/2024'),
        BaselineEndDate: new Date('04/13/2024'),
        BaselineDuration: '0'    // baseline milestone — MUST be '0', not same dates alone
    }
];
```

---

## Enabling Baseline Display

```typescript
import { Gantt } from '@syncfusion/ej2-gantt';

let gantt: Gantt = new Gantt({
    dataSource: projectData,
    height: '450px',
    renderBaseline: true,         // show baseline bars
    baselineColor: 'red',         // baseline bar color (CSS color string)
    taskFields: {
        id: 'TaskID', name: 'TaskName', startDate: 'StartDate',
        duration: 'Duration', progress: 'Progress', parentID: 'ParentID',
        baselineStartDate: 'BaselineStartDate',
        baselineEndDate: 'BaselineEndDate',
        baselineDuration: 'BaselineDuration'
    }
});
gantt.appendTo('#Gantt');
```

---

## Baseline Milestones

To display a **baseline milestone** (diamond), you must set `baselineDuration` to `'0'` explicitly:

```typescript
// ✅ Correct: baseline milestone
{
    TaskID: 5, TaskName: 'Go-Live Milestone',
    StartDate: new Date('05/01/2024'), Duration: 0,
    BaselineStartDate: new Date('04/28/2024'),
    BaselineEndDate: new Date('04/28/2024'),
    BaselineDuration: '0'    // required for milestone rendering
}

// ❌ Wrong: same BaselineStartDate and BaselineEndDate without baselineDuration: '0'
//    → renders as 1-day baseline bar, not a milestone
{
    BaselineStartDate: new Date('04/28/2024'),
    BaselineEndDate: new Date('04/28/2024')
    // missing BaselineDuration: '0'
}
```

---

## Customizing Baseline Appearance

**Using `baselineColor` property:**
```typescript
let gantt: Gantt = new Gantt({
    renderBaseline: true,
    baselineColor: 'rgba(255, 107, 107, 0.8)',   // semi-transparent red
    // ... other config
});
```

**Advanced CSS override:**
```css
/* Baseline bar styling */
.e-gantt .e-gantt-chart .e-baseline-bar {
    height: 4px;
    border-radius: 2px;
    opacity: 0.9;
    background-color: #4CAF50;   /* green baseline */
}

/* Baseline milestone diamond */
.e-gantt .e-gantt-chart .e-baseline-gantt-milestone-container {
    border-color: #4CAF50;
}
```

> The `baselineColor` property takes precedence as a quick configuration option. For advanced per-element control, use CSS overrides.
