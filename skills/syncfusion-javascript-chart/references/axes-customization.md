# Axes Customization

## Table of Contents
- [Axis Types](#axis-types)
- [Axis Labels & Formatting](#axis-labels--formatting)
- [Axis Ranges & Scaling](#axis-ranges--scaling)
- [Multiple Axes](#multiple-axes)

**Note:** Configuration syntax is identical for TypeScript and JavaScript. Examples show object literal format.

**API Reference:** See [api-reference.md](api-reference.md#axis-classes) for complete `Axis`, `AxisLine`, `MajorGridLines`, `MinorGridLines`, and related class documentation.

## Axis Types

### Category Axis

Best for: Discrete categories, time periods, groups

```javascript
// TypeScript or JavaScript (identical)
primaryXAxis: {
  valueType: 'Category',
  labelPlacement: 'OnTicks'
}
```

**Properties:**
- `labelPlacement: 'OnTicks'` - Labels on axis tick marks
- `labelPlacement: 'BetweenTicks'` - Labels between ticks

### Numeric Axis

Best for: Continuous numeric data, measurements

```javascript
primaryXAxis: {
  valueType: 'Double',
  minimum: 0,
  maximum: 100,
  interval: 10
}
```

**Properties:**
- `minimum` - Minimum value
- `maximum` - Maximum value
- `interval` - Space between ticks
- `title: { text: 'Values' }` - Axis title

### DateTime Axis

Best for: Time series, dates, temporal data

```javascript
primaryXAxis: {
  valueType: 'DateTime',
  intervalType: 'Days',      // Days, Hours, Minutes, Months, Years
  interval: 1
}
```

**Interval Types:**
```javascript
// All supported interval types
'Days'         // Daily intervals
'Hours'        // Hourly intervals
'Minutes'      // Minute intervals
'Months'       // Monthly intervals
'Years'        // Yearly intervals
```

### Logarithmic Axis

Best for: Exponential data, large value ranges

```javascript
primaryYAxis: {
  valueType: 'Logarithmic',
  logBase: 10
}
```

**Use cases:**
- Stock prices (large ranges)
- Scientific data
- Richter scale measurements

## Axis Labels & Formatting

### Label Format

```typescript
primaryYAxis: {
  labelFormat: '{value}%'         // Append %
  labelFormat: '${value}k'        // Append k
  labelFormat: '{n2}'       // 2 decimal places
  labelFormat: '{p0}'       // Percentage format
}
```

### Custom Label Formatting Function

```typescript
primaryYAxis: {
  labelFormat: (args: any) => {
    if (args.value >= 1000) {
      return (args.value / 1000) + 'K';
    }
    return args.value.toString();
  }
}
```

### Label Styling

```typescript
primaryXAxis: {
  labelStyle: {
    size: '14px',
    color: '#333',
    fontFamily: 'Arial',
    fontWeight: 'Bold'
  }
}
```

### Rotation & Padding

```typescript
primaryXAxis: {
  labelRotation: 45,          // Rotate labels 45°
  labelIntersectAction: 'Rotate45',  // Auto-rotate on overlap
  labelPadding: 10            // Space around labels
}
```

## Axis Ranges & Scaling

### Setting Axis Range

```typescript
primaryYAxis: {
  minimum: 0,
  maximum: 100,
  interval: 20
}
```

### Auto-Scaling

Let Syncfusion calculate optimal range:

```typescript
primaryYAxis: {
  minimum: null,              // Auto
  maximum: null,              // Auto
  interval: 'auto'
}
```

### Range Gaps

```typescript
primaryYAxis: {
  rangePadding: 'Additional',  // Add extra space
  majorTickLines: { width: 0 },
  minorTickLines: { width: 0 }
}
```

### Reversed Axis

```typescript
primaryYAxis: {
  isInversed: true            // Bottom to top
}
```

## Multiple Axes

### Secondary Y-Axis

Use when series have different scales:

```typescript
primaryYAxis: {
  title: { text: 'Sales (₹)' },
  labelFormat: '${value}k'
},
axes: [
  {
    name: 'yAxis2',
    title: { text: 'Units' },
    opposedPosition: true,    // Right side
    labelFormat: '{value}%'
  }
],
series: [
  {
    dataSource: data,
    xName: 'month',
    yName: 'sales',
    type: 'Column',
    name: 'Sales'
  },
  {
    dataSource: data,
    xName: 'month',
    yName: 'units',
    type: 'Line',
    yAxisName: 'yAxis2',      // Use secondary axis
    name: 'Units'
  }
]
```

### Secondary X-Axis

```typescript
primaryXAxis: {
  title: { text: 'Time (Hours)' }
},
axes: [
  {
    name: 'xAxis2',
    opposedPosition: true,     // Top
    title: { text: 'Intervals' }
  }
],
series: [
  { dataSource: data, xName: 'hour', yName: 'value1', type: 'Line' },
  { dataSource: data, xName: 'interval', yName: 'value2', type: 'Line', xAxisName: 'xAxis2' }
]
```

### Multiple Panes

Separate chart areas for different series:

```typescript
rows: [
  { height: '50%' },
  { height: '50%' }
],
series: [
  { dataSource: data1, xName: 'x', yName: 'y', type: 'Line' },
  { dataSource: data2, xName: 'x', yName: 'y', type: 'Column', yAxisName: 'yAxis2' }
],
axes: [
  { name: 'yAxis2', rowIndex: 1 }
]
```

## Advanced Customization

### Axis Title & Subtitle

```typescript
primaryXAxis: {
  title: {
    text: 'Sales Categories',
    style: {
      size: '16px',
      fontFamily: 'Arial Bold',
      color: '#333'
    }
  }
},
primaryYAxis: {
  title: {
    text: 'Revenue (₹ Thousands)',
    rotation: -90,
    offset: 20
  }
}
```

### Grid Lines Customization

```typescript
primaryXAxis: {
  majorGridLines: {
    width: 1,
    color: '#e0e0e0',
    dashArray: '5,5'
  },
  minorGridLines: {
    width: 0.5,
    color: '#f0f0f0'
  }
}
```

### Tick Lines

```typescript
primaryXAxis: {
  majorTickLines: {
    width: 1,
    size: 5
  },
  minorTickLines: {
    width: 1,
    size: 3
  }
}
```

### Strip Lines (Highlights)

Highlight data ranges:

```typescript
primaryYAxis: {
  stripLines: [
    {
      start: 30,
      end: 40,
      color: '#e0e0e0',
      opacity: 0.5,
      text: 'Target Range'
    }
  ]
}
```

### Axis Line & Border

```typescript
primaryXAxis: {
  lineStyle: {
    width: 2,
    color: '#333'
  },
  opposedPosition: false
}
```

### Label Bounds

```typescript
primaryXAxis: {
  desiredIntervals: 5,        // Target number of labels
  edgeLabelPlacement: 'Shift' // Shift labels at edges
}
```

### Chart sample with all propeties

```typescript
import {
    Chart, LineSeries, Category, Legend,  StripLine, Trendlines
  } from '@syncfusion/ej2-charts';
  
  Chart.Inject(
    LineSeries, Category, Legend, StripLine, Trendlines
  );
  
  const data = [
    { x: 'Jan', y: 34, high: 40, low: 30, open: 32, close: 36 },
    { x: 'Feb', y: 28, high: 35, low: 25, open: 30, close: 29 },
    { x: 'Mar', y: 32, high: 38, low: 29, open: 31, close: 35 }
  ];
  
  let chart: Chart = new Chart({
  
    title: 'Master Demo Chart',
    subTitle: 'All Syncfusion Chart Features in One Demo',
  
    border: { width: 1, color: '#bfbfbf' },
    background: '#ffffff',
  
    rows: [
      { height: '60%' },
      { height: '40%' }
    ],
    columns: [
      { width: '70%' },
      { width: '30%' }
    ],
  
    primaryXAxis: {
      title: 'Months',
      valueType: 'Category',
      majorGridLines: { width: 1 },
      stripLines: [{ start: 1, end: 2, text: 'Q1 Mid', color: '#f3f3f3' }]
    },
  
    primaryYAxis: {
      title: 'Value',
      minimum: 0,
      maximum: 50,
      interval: 10,
      stripLines: [
        { start: 30, end: 40, color: '#ffecec', text: 'High Range' }
      ]
    },
  
    axes: [
      {
        name: 'secondaryAxis',
        rowIndex: 1,
        opposedPosition: true,
        minimum: 0,
        maximum: 100,
        interval: 20,
        title: 'Secondary Axis'
      }
    ],
  series: [
      {
        type: 'Line',
        dataSource: data,
        xName: 'x',
        yName: 'y',
        name: 'Sales',
        width: 2,
        trendlines: [
          { type: 'Linear', name: 'Linear Trendline', width: 2, fill: 'red' }
        ]
      }
    ],
    legendSettings: {
      visible: true
    },
    theme: 'Material',
    width: '100%',
    height: '100%',
  });
  
  chart.appendTo('#element');
```