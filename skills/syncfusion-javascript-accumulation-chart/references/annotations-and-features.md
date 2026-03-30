# Annotations & Features

**API Reference:** [AccumulationAnnotation Module](https://ej2.syncfusion.com/documentation/api/accumulation-chart/accumulationannotation/) | [CenterLabelModel](https://ej2.syncfusion.com/documentation/api/accumulation-chart/centerlabelmodel/) | [AccumulationAnnotationSettingsModel](https://ej2.syncfusion.com/documentation/api/accumulation-chart/accumulationannotationsettingsmodel/)

## Table of Contents
- [Annotations](#annotations)
- [Gradient Fills](#gradient-fills)
- [Center Labels](#center-labels)
- [Grouping & Clubbing Points](#grouping--clubbing-points)
- [Empty Point Handling](#empty-point-handling)

## Annotations

Annotations add custom shapes, text, or images to highlight specific areas or provide additional context.

### Basic Annotation

**TypeScript:**
```typescript
import { AccumulationChart, PieSeries, AccumulationAnnotation } from '@syncfusion/ej2-charts';

AccumulationChart.Inject(PieSeries, AccumulationAnnotation);

const chart = new AccumulationChart({
  series: [{ dataSource: data, xName: 'x', yName: 'y', type: 'Pie' }],
  annotations: [{
    content: '<div>Important Note</div>',
    x: '50%',
    y: '50%'
  }]
}, '#chart');
```

### Annotation with HTML

**TypeScript:**
```typescript
annotations: [{
  content: '<div style="background:#FF5733;color:white;padding:10px;border-radius:5px;">' +
           '<b>Peak Sales</b><br>March 2024' +
           '</div>',
  x: '75%',
  y: '25%',
  coordinateUnits: 'Pixel'
}]
```

### Annotation Positioning

| Parameter | Purpose |
|-----------|---------|
| `x` | Horizontal position (pixels or %) |
| `y` | Vertical position (pixels or %) |
| `coordinateUnits` | 'Pixel' or 'Point' |

## Gradient Fills

Apply gradient color schemes to chart segments for enhanced visual appeal.

### Linear Gradient

**TypeScript:**
```typescript
import { AccumulationChart, PieSeries } from '@syncfusion/ej2-charts';

AccumulationChart.Inject(PieSeries);

const chart = new AccumulationChart({
  series: [{
    dataSource: data,
    xName: 'x',
    yName: 'y',
    type: 'Pie',
    linearGradient: {
      x1: 0.1, y1: 0,
      x2: 0.9, y2: 1,
      gradientColorStop: [
          { color: '#4F46E5', offset: 0, opacity: 1, brighten: 0.55 },
          { color: '#4F46E5', offset: 60, opacity: 0.98, brighten: 0.15 },
          { color: '#4F46E5', offset: 100, opacity: 0.95, brighten: -0.25 },
      ]
  }}]
}, '#chart');
```

### Point-Level Gradient with Color Mapping

**TypeScript:**
```typescript
const data = [
  { x: 'A', y: 25, color: '#FF5733' },
  { x: 'B', y: 30, color: '#33FF57' },
  { x: 'C', y: 20, color: '#3357FF' },
  { x: 'D', y: 25, color: '#FF33F5' }
];

const chart = new AccumulationChart({
  series: [{
    dataSource: data,
    xName: 'x',
    yName: 'y',
    type: 'Pie',
    pointColorMapping: 'color'  // Use 'color' field from data
  }]
}, '#chart');
```

### Palette-Based Gradient

**TypeScript:**
```typescript
const chart = new AccumulationChart({
  series: [{ dataSource: data, xName: 'x', yName: 'y', type: 'Pie', palettes: [ '#FF6B6B', '#4ECDC4', '#45B7D1', '#FFA07A', '#98D8C8', '#6C5CE7'
]}]
}, '#chart');
```

## Center Labels

Doughnut charts can display text in the center space. Useful for showing totals, KPIs, or metrics.

### Basic Center Label

**TypeScript:**
```typescript
const chart = new AccumulationChart({
  series: [{
    dataSource: data,
    xName: 'x',
    yName: 'y',
    innerRadius: '50%'
  }],
  centerLabel: {
    text: 'Total Sales'
  }
}, '#chart');
```

### Center Label with Hover Text

**TypeScript:**
```typescript
centerLabel: {
  text: 'Sales 2024',
  hoverTextFormat: '${point.x}: ${point.y}%',
  textStyle: {
    size: '16px',
    fontWeight: 'Bold',
    color: '#2F5496'
  }
}
```

### Dynamic Center Label

**TypeScript:**
```typescript
const data = [
  { x: 'Q1', y: 25000 },
  { x: 'Q2', y: 30000 },
  { x: 'Q3', y: 28000 },
  { x: 'Q4', y: 32000 }
];

const total = data.reduce((sum, item) => sum + item.y, 0);

const chart = new AccumulationChart({
  series: [{
    dataSource: data,
    xName: 'x',
    yName: 'y',
    innerRadius: '55%'
  }],
  centerLabel: {
    text: `$${(total / 1000).toFixed(0)}k`,
    hoverTextFormat: '${point.x}: $${point.y}k'
  }
}, '#chart');
```

## Grouping & Clubbing Points

Combine small data points into a "Others" category to reduce clutter and improve readability.

### Club Points by Count

**TypeScript:**
```typescript
const chart = new AccumulationChart({
  series: [{
    dataSource: data,
    xName: 'x',
    yName: 'y',
    type: 'Pie',
    groupMode: 'Point'
  }]
}, '#chart');
```

### Club Points by Value

**TypeScript:**
```typescript
series: [{
  dataSource: data,
  xName: 'x',
  yName: 'y',
  type: 'Pie',
  groupMode: 'Value'
}]
```

### Complete Clubbing Example

**TypeScript:**
```typescript
const data = [
  { x: 'Chrome', y: 45.5 },
  { x: 'Firefox', y: 25.2 },
  { x: 'Safari', y: 15.8 },
  { x: 'Edge', y: 8.4 },
  { x: 'Opera', y: 3.2 },
  { x: 'Internet Explorer', y: 1.2 },
  { x: 'Other', y: 0.7 }
];

const chart = new AccumulationChart({
  series: [{
    dataSource: data,
    xName: 'x',
    yName: 'y',
    type: 'Pie',
    groupMode: 'Point'
  }],
  title: 'Browser Market Share'
}, '#chart');
```

## Empty Point Handling

Define how empty, null, or zero values are displayed in charts.

### Default Empty Point Display

**TypeScript:**
```typescript
const data = [
  { x: 'A', y: 25 },
  { x: 'B', y: null },      // Empty point
  { x: 'C', y: 20 },
  { x: 'D', y: undefined }  // Empty point
];

const chart = new AccumulationChart({
  series: [{
    dataSource: data,
    xName: 'x',
    yName: 'y',
    type: 'Pie'
  }]
}, '#chart');
```

### Custom Empty Point Style

**TypeScript:**
```typescript
const chart = new AccumulationChart({
  series: [{
    dataSource: data,
    xName: 'x',
    yName: 'y',
    type: 'Pie',
    emptyPointSettings: {
      fill: '#E0E0E0',           // Gray color for empty points
      border: { color: '#999999' },
      mode: 'Drop'               // Drop, Zero, Average
    }
  }]
}, '#chart');
```

### Empty Point Modes

| Mode | Behavior |
|------|----------|
| **Drop** | Hide empty points completely |
| **Zero** | Treat as zero value |
| **Average** | Use average of adjacent values |

### Complete Empty Point Configuration

**TypeScript:**
```typescript
const data = [
  { x: 'Jan', y: 20 },
  { x: 'Feb', y: null },
  { x: 'Mar', y: 25 },
  { x: 'Apr', y: undefined },
  { x: 'May', y: 30 }
];

const chart = new AccumulationChart({
  series: [{
    dataSource: data,
    xName: 'x',
    yName: 'y',
    innerRadius: '40%',
    emptyPointSettings: {
      fill: '#CBCBCB',
      border: {
        color: '#999999',
        width: 2
      },
      mode: 'Drop'
    },
    dataLabel: {
      visible: true,
      format: '${point.x}'
    }
  }]
}, '#chart');
```

## Combined Features Example

**TypeScript:**
```typescript
import { 
    AccumulationChart, 
    PieSeries, 
    AccumulationDataLabel,
    AccumulationTooltip,
    AccumulationAnnotation 
  } from '@syncfusion/ej2-charts';
  
  AccumulationChart.Inject(
    PieSeries, 
    AccumulationDataLabel, 
    AccumulationTooltip,
    AccumulationAnnotation
  );
  
  const salesData = [
    { x: 'Product A', y: 25000, color: '#FF6B6B' },
    { x: 'Product B', y: 30000, color: '#4ECDC4' },
    { x: 'Product C', y: 20000, color: '#45B7D1' },
    { x: 'Product D', y: 15000, color: '#FFA07A' },
    { x: 'Product E', y: 5000, color: '#98D8C8' }
  ];
  
  const chart = new AccumulationChart({
    // Series with grouping
    series: [{
      dataSource: salesData,
      xName: 'x',
      yName: 'y',
      type: 'Pie',
      pointColorMapping: 'color',
      groupMode: 'Point',
      dataLabel: {
        visible: true
      }
    }],
    
    // Tooltip
    tooltip: {
      enable: true,
      format: '<b>${point.x}</b><br>Sales: $${point.y}k'
    },
    
    // Annotation
    annotations: [{
      content: '<div style="background:#FFD700;padding:8px;border-radius:3px;">' +
               '<b>Top Performer</b>' +
               '</div>',
      x: '60%',
      y: '20%'
    }],
    
    title: 'Product Sales Distribution'
  }, '#chart');
```

## API Reference Links

- [AccumulationAnnotation Module](https://ej2.syncfusion.com/documentation/api/accumulation-chart/accumulationannotation/)
- [AccumulationAnnotationSettingsModel](https://ej2.syncfusion.com/documentation/api/accumulation-chart/accumulationannotationsettingsmodel/)
- [CenterLabelModel](https://ej2.syncfusion.com/documentation/api/accumulation-chart/centerlabelmodel/)
- [annotationRender Event](https://ej2.syncfusion.com/documentation/api/accumulation-chart/#annotationrender)
- [setAnnotationValue Method](https://ej2.syncfusion.com/documentation/api/accumulation-chart/#setannotationvalue)

## Troubleshooting

**Issue: Annotations not showing**
- Solution: Ensure annotations array is provided
- Check x, y coordinates are within chart bounds
- See [AccumulationAnnotation API](https://ej2.syncfusion.com/documentation/api/accumulation-chart/accumulationannotation/)

**Issue: Gradient colors not appearing**
- Solution: Use `pointColorMapping` for point-level colors
- Verify `palettes` is an array of valid color values

**Issue: Center label not visible in doughnut**
- Solution: Check `innerRadius` is > 0
- Verify `centerLabel.text` is set
- Ensure `type: 'Doughnut'` is specified
- See [CenterLabelModel API](https://ej2.syncfusion.com/documentation/api/accumulation-chart/centerlabelmodel/)

**Issue: Grouped points not appearing**
- Solution: Verify `groupMode` is set correctly
- Check `groupPadding` threshold is appropriate
