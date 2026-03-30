# Tooltips & Selection

## Table of Contents

- [Tooltip Configuration](#tooltip-configuration)
  - [Enable Tooltips](#enable-tooltips)
  - [Tooltip Format](#tooltip-format)
  - [Format Placeholders](#format-placeholders)
  - [Complete Tooltip Configuration](#complete-tooltip-configuration)
  - [Tooltip with Custom Template](#tooltip-with-custom-template)
- [Selection Modes](#selection-modes)
  - [Point Selection](#point-selection)
  - [No Selection](#no-selection)
- [Selection Patterns](#selection-patterns)
- [Selection Events](#selection-events)
  - [PointRender Event](#pointrender-event)
  - [PointClick Event](#pointclick-event)
- [Interactive Example](#interactive-example)
- [Selection Patterns Reference](#selection-patterns-reference)
- [API Reference Links](#api-reference-links)
- [Troubleshooting](#troubleshooting)

**API Reference:** [TooltipSettingsModel](https://ej2.syncfusion.com/documentation/api/accumulation-chart/tooltipsettingsmodel/) | [AccumulationSelectionMode](https://ej2.syncfusion.com/documentation/api/accumulation-chart/accumulationselectionmode/) | [SelectionPattern](https://ej2.syncfusion.com/documentation/api/accumulation-chart/selectionpattern/)

## Tooltip Configuration

Tooltips display detailed information when hovering over chart elements. Essential for showing data not visible in labels or data points.

### Enable Tooltips

**TypeScript:**
```typescript
import { AccumulationChart, PieSeries, AccumulationTooltip } from '@syncfusion/ej2-charts';

AccumulationChart.Inject(PieSeries, AccumulationTooltip);

const chart = new AccumulationChart({
  series: [{
    dataSource: data,
    xName: 'x',
    yName: 'y',
    type: 'Pie'
  }],
  tooltip: {
    enable: true  // Enable hover tooltips
  }
}, '#chart');
```

### Tooltip Format

Customize tooltip content using format strings:

**TypeScript:**
```typescript
tooltip: {
  enable: true,
  format: '<b>${point.x}</b><br>Value: ${point.y}'
}
```

### Format Placeholders

| Placeholder | Shows |
|-------------|-------|
| `${point.x}` | Category name |
| `${point.y}` | Numeric value |
| `${point.percentage}` | Percentage of total |
| `${series.name}` | Series name |

### Complete Tooltip Configuration

**TypeScript:**
```typescript
tooltip: {
  enable: true,
  format: '<b>${point.x}</b><br>Sales: $${point.y}k<br>Share: ${point.percentage}%',
  opacity: 0.9,
  border: {
    color: '#000000',
    width: 1
  },
  textStyle: {
    color: '#000000',
    fontFamily: 'Segoe UI',
    size: '12px'
  }
}
```

### Tooltip with Custom Template

**TypeScript:**
```typescript
tooltip: {
  enable: true,
  template: '<div style="border:1px solid #ccc; padding:5px">' +
            '<b>${x}</b><br>' +
            'Value: ${y}' +
            '</div>'
}
```

## Selection Modes

Control how users can select data points or series.

### Point Selection

Select individual data points by clicking:

**TypeScript:**
```typescript
const chart = new AccumulationChart({
  series: [{ 
    dataSource: data, 
    xName: 'x', 
    yName: 'y', 
    type: 'Pie' 
  }],
  selectionMode: 'Point'  // Select individual points
}, '#chart');
```

### No Selection

Disable selection capability:

**TypeScript:**
```typescript
selectionMode: 'None'  // No selection allowed
```

## Selection Patterns

### Pattern 1: Point Selection with Visual Feedback

**TypeScript:**
```typescript
const chart = new AccumulationChart({
  series: [{
    dataSource: data,
    xName: 'x',
    yName: 'y',
    type: 'Pie'
  }],
  selectionMode: 'Point',
  selectionPattern: 'Dots'
}, '#chart');
```

### Pattern 2: Selection with Highlight Pattern

**TypeScript:**
```typescript
const chart = new AccumulationChart({
  series: [{
    dataSource: data,
    xName: 'x',
    yName: 'y',
    type: 'Pie'
  }],
  selectionMode: 'Point',
  selectionPattern: 'Chequered',  // Chequered, Dots, DiagonalForward, etc.
  highlightPattern: 'DiagonalForward'
}, '#chart');
```

## Selection Events

Handle selection programmatically:

### PointRender Event

Triggered when a point is rendered:

**TypeScript:**
```typescript
import { AccumulationChart, PieSeries } from '@syncfusion/ej2-charts';
import { IAccPointRenderEventArgs } from '@syncfusion/ej2-charts';

const chart = new AccumulationChart({
  pointRender: (args: IAccPointRenderEventArgs) => {
    console.log('Point rendered:', args.point);
  },
  series: [{ dataSource: data, xName: 'x', yName: 'y', type: 'Pie' }]
}, '#chart');
```

### Point Event

Triggered when user clicks a data point:

**TypeScript:**
```typescript
import { IPointEventArgs } from '@syncfusion/ej2-charts';

const chart = new AccumulationChart({
  pointClick: (args: IPointEventArgs) => {
    console.log('Point clicked:', args.point.x);
    // Handle point click logic
  },
  series: [{ dataSource: data, xName: 'x', yName: 'y', type: 'Pie' }]
}, '#chart');
```

## Interactive Example

**TypeScript:**
```typescript
import { AccumulationChart, PieSeries, AccumulationDataLabel, AccumulationTooltip, AccumulationSelection } from '@syncfusion/ej2-charts';

AccumulationChart.Inject(PieSeries, AccumulationDataLabel, AccumulationTooltip, AccumulationSelection);

const data = [
  { x: 'Q1', y: 25000 },
  { x: 'Q2', y: 30000 },
  { x: 'Q3', y: 28000 },
  { x: 'Q4', y: 32000 }
];

const chart = new AccumulationChart({
  // Data
  series: [{
    dataSource: data,
    xName: 'x',
    yName: 'y',
    type: 'Pie'
  }],
  
  // Tooltips
  tooltip: {
    enable: true,
    format: '<b>${point.x}</b><br>Value: $${point.y}k'
  },
  
  // Selection
  selectionMode: 'Point',
  
  // Events
  pointClick: (args: any) => {
    console.log('Selected:', args.point.x);
  },
  
  load: (args: any) => {
    console.log('Chart loaded');
  }
}, '#chart');
```

## Selection Patterns Reference

| Pattern | Appearance |
|---------|-----------|
| **Chequered** | Checkerboard pattern |
| **Dots** | Small dot pattern |
| **DiagonalForward** | Forward slash lines |
| **DiagonalBackward** | Backslash lines |
| **Grid** | Grid pattern |
| **Box** | Box outline |
| **PatternByIndex** | Alternating patterns |

## API Reference Links

- [TooltipSettingsModel](https://ej2.syncfusion.com/documentation/api/accumulation-chart/tooltipsettingsmodel/)
- [AccumulationSelectionMode Enum](https://ej2.syncfusion.com/documentation/api/accumulation-chart/accumulationselectionmode/)
- [SelectionPattern Enum](https://ej2.syncfusion.com/documentation/api/accumulation-chart/selectionpattern/)
- [selectionComplete Event](https://ej2.syncfusion.com/documentation/api/accumulation-chart/#selectioncomplete)
- [tooltipRender Event](https://ej2.syncfusion.com/documentation/api/accumulation-chart/#tooltiprender)
- [pointClick Event](https://ej2.syncfusion.com/documentation/api/accumulation-chart/#pointclick)

## Troubleshooting

**Issue: Tooltip not showing**
- Solution: Set `tooltip.enable: true`
- Ensure `AccumulationTooltip` module is injected
- Check that `format` string uses valid placeholders
- See [TooltipSettingsModel API](https://ej2.syncfusion.com/documentation/api/accumulation-chart/tooltipsettingsmodel/)

**Issue: Selection not working**
- Solution: Set `selectionMode: 'Point'` or `'None'`
- Verify clicking on data points (not empty chart area)
- See [AccumulationSelectionMode API](https://ej2.syncfusion.com/documentation/api/accumulation-chart/accumulationselectionmode/)

**Issue: Wrong data in tooltip**
- Solution: Check format placeholder matches data fields
- Verify data source has x, y, and custom fields
