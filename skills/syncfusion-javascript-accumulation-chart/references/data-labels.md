# Data Labels

## Table of Contents
- [Overview](#overview)
- [Enabling Data Labels](#enabling-data-labels)
- [Positioning](#positioning)
- [Smart Labels & Arrangement](#smart-labels--arrangement)
- [Formatting & Templates](#formatting--templates)
- [Customization Options](#customization-options)

**API Reference:** [AccumulationDataLabel Module](https://ej2.syncfusion.com/documentation/api/accumulation-chart/accumulationdatalabel/) | [Data Label Properties](https://ej2.syncfusion.com/documentation/api/accumulation-chart/accumulationseriesmodel/#datalabel)

## Overview

Data labels display values directly on chart segments or near data points, eliminating the need to reference legends or tooltips for basic information. Use data labels to:
- Show exact values or percentages
- Display category names
- Highlight specific metrics
- Improve chart readability

## Enabling Data Labels

### Basic Data Label

**TypeScript:**
```typescript
import { AccumulationChart, PieSeries, AccumulationDataLabel } from '@syncfusion/ej2-charts';

AccumulationChart.Inject(PieSeries, AccumulationDataLabel);

const chart = new AccumulationChart({
  series: [{
    dataSource: data,
    xName: 'x',
    yName: 'y',
    type: 'Pie',
    dataLabel: {
      visible: true  // Enable labels
    }
  }]
}, '#chart');
```

### Default Label Content

By default, labels show the category name (x-field value). Customize using templates or name property.

## Positioning

Control where labels appear relative to chart segments.

### Inside Positioning

Place labels inside pie/doughnut segments:

**TypeScript:**
```typescript
dataLabel: {
  visible: true,
  position: 'Inside'  // Inside or Outside
}
```

**Best for:** Charts with few, large segments

### Outside Positioning

Place labels outside segments with connector lines:

**TypeScript:**
```typescript
dataLabel: {
  visible: true,
  position: 'Outside'  // Prevents overlap
}
```

**Best for:** Charts with many or small segments

### Example with Position

```typescript
const chart = new AccumulationChart({
  series: [{
    dataSource: [
      { x: 'A', y: 45 },
      { x: 'B', y: 30 },
      { x: 'C', y: 15 },
      { x: 'D', y: 10 }
    ],
    xName: 'x',
    yName: 'y',
    type: 'Pie',
    dataLabel: {
      visible: true,
      position: 'Outside'  // Outside with connectors
    }
  }]
}, '#chart');
```

## Smart Labels & Arrangement

Smart labels automatically prevent overlapping and arrange labels intelligently.

### Enable Smart Labels

**TypeScript:**
```typescript
const chart = new AccumulationChart({
  enableSmartLabels: true,  // Chart-level property
  series: [{
    dataSource: data,
    xName: 'x',
    yName: 'y',
    type: 'Pie',
    dataLabel: { visible: true }
  }]
}, '#chart');
```

### Smart Label Behavior

When enabled, labels:
- Automatically reposition to avoid overlaps
- Extend connector lines if needed
- Arrange in optimal positions around chart
- Recalculate on chart resize

### Complete Smart Label Example

```typescript
const chart = new AccumulationChart({
  enableSmartLabels: true,
  series: [{
    dataSource: [
      { x: 'Product A', y: 18 },
      { x: 'Product B', y: 18 },
      { x: 'Product C', y: 17 },
      { x: 'Product D', y: 13.5 },
      { x: 'Product E', y: 19 },
      { x: 'Product F', y: 23.5 }
    ],
    xName: 'x',
    yName: 'y',
    type: 'Pie',
    dataLabel: {
      visible: true,
      position: 'Outside',  // Works best with Outside
      name: 'x'  // Show category name
    }
  }],
  height: '400px'
}, '#chart');
```

## Formatting & Templates

Control what data appears in labels using format strings or custom templates.

### Format String

Use placeholder syntax to build custom label text:

**TypeScript:**
```typescript
dataLabel: {
  visible: true,
  format: 'n1'
}
```

### Format Examples

| Value  | Format | Resultant Value | Description |
|--------|--------|-----------------|-------------|
| 1000   | n1     | 1000.0          | The number is rounded to 1 decimal place. |
| 1000   | n2     | 1000.00         | The number is rounded to 2 decimal places. |
| 1000   | n3     | 1000.000        | The number is rounded to 3 decimal places. |
| 0.01   | p1     | 1.0%            | The number is converted to percentage with 1 decimal place. |
| 0.01   | p2     | 1.00%           | The number is converted to percentage with 2 decimal places. |
| 0.01   | p3     | 1.000%          | The number is converted to percentage with 3 decimal places. |
| 1000   | c1     | $1000.0         | The currency symbol is appended to number and number is rounded to 1 decimal place. |
| 1000   | c2     | $1000.00        | The currency symbol is appended to number and number is rounded to 2 decimal places. |

### Complete Format Example

```typescript
const chart = new AccumulationChart(
  {
    enableSmartLabels: true,
    series: [
      {
        dataSource: [
          { x: 'Jan', y: 13, text: 'Jan: 3' },
          { x: 'Feb', y: 13, text: 'Feb: 3.5' },
          { x: 'Mar', y: 7, text: 'Mar: 7' },
          { x: 'Apr', y: 13, text: 'Apr: 13.5' },
        ],
        xName: 'x',
        yName: 'y',
        dataLabel: { visible: true, format: 'n2' },
      },
    ],
    height: '400px',
  },
  '#chart'
);
```

## Customization Options

Fine-tune label appearance and behavior.

### Font Customization

**TypeScript:**
```typescript
dataLabel: {
  visible: true,
  font: {
    color: '#FFFFFF',
    fontFamily: 'Segoe UI',
    size: '12px',
    fontWeight: 'Bold'
  }
}
```

### Border and Background

**TypeScript:**
```typescript
dataLabel: {
  visible: true,
  border: {
    color: '#000000',
    width: 1
  },
  rx: 3,  // Border radius
  ry: 3
}
```

### Label Rotation

**TypeScript:**
```typescript
dataLabel: {
  visible: true,
  enableRotation: true,
  angle: 90  // Degrees (0-360)
}
```

### Complete Customization Example

**TypeScript:**
```typescript
import { AccumulationChart, PieSeries, AccumulationDataLabel } from '@syncfusion/ej2-charts';

AccumulationChart.Inject(PieSeries, AccumulationDataLabel);

const chart = new AccumulationChart({
  enableSmartLabels: true,
  series: [{
    dataSource: [
      { x: 'North', y: 135, custom: 'North Region' },
      { x: 'South', y: 198, custom: 'South Region' },
      { x: 'East', y: 160, custom: 'East Region' },
      { x: 'West', y: 121, custom: 'West Region' }
    ],
    xName: 'x',
    yName: 'y',
    type: 'Pie',
    dataLabel: {
      // Visibility
      visible: true,
      position: 'Outside',
      
      // Styling
      font: {
        color: '#1F77B4',
        fontFamily: 'Arial',
        size: '14px',
        fontWeight: 'Bold'
      },
      
      // Border & Background
      border: {
        color: '#1F77B4',
        width: 1
      },
      
      // Spacing
      margin: {
        left: 5,
        right: 5,
        top: 5,
        bottom: 5
      }
    }
  }],
  height: '400px'
}, '#chart');
```

## API Reference Links

- [AccumulationDataLabel Module](https://ej2.syncfusion.com/documentation/api/accumulation-chart/accumulationdatalabel/)
- [enableSmartLabels Property](https://ej2.syncfusion.com/documentation/api/accumulation-chart/#enablesmartlabels)
- [textRender Event](https://ej2.syncfusion.com/documentation/api/accumulation-chart/#textrender)

## Troubleshooting

**Issue: Labels overlapping**
- Solution: Enable `enableSmartLabels: true`
- Use `position: 'Outside'` for better spacing
- Reduce font size in `dataLabel.font`
- See [enableSmartLabels API](https://ej2.syncfusion.com/documentation/api/accumulation-chart/#enablesmartlabels)

**Issue: Labels not visible**
- Solution: Set `dataLabel.visible: true`
- Ensure `AccumulationDataLabel` is injected
- Check font color contrasts with background
- See [AccumulationDataLabel API](https://ej2.syncfusion.com/documentation/api/accumulation-chart/accumulationdatalabel/)

**Issue: Wrong data showing**
- Solution: Verify format string uses correct placeholders
- Check data source has required fields (x, y)
- Verify xName and yName properties match data

**Issue: Labels cut off**
- Solution: Increase chart container height
- Adjust margins: `margin.top`, `margin.left`
- Use `position: 'Inside'` if appropriate
