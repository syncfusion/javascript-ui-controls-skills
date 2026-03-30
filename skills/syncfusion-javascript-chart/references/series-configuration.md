# Series Configuration

## Table of Contents
- [Series Properties](#series-properties)
- [Data Labels](#data-labels)
- [Marker Customization](#marker-customization)
- [Stacking & Grouping](#stacking--grouping)
- [Empty Points](#empty-points)

**Note:** Configuration syntax is identical for TypeScript and JavaScript. Examples show JS object literal format.

**API Reference:** See [api-reference.md](api-reference.md#series) for complete `Series`, `MarkerSettings`, and `DataLabelSettings` class documentation.

## Series Properties

### Basic Series Configuration

**TypeScript:**
```typescript
import { Chart, ColumnSeries, Category } from '@syncfusion/ej2-charts';

Chart.Inject(ColumnSeries, Category);

const chart = new Chart({
  primaryXAxis: { valueType: 'Category' },
  series: [{
    dataSource: data,           // Data array
    xName: 'month',            // X-axis field
    yName: 'sales',            // Y-axis field
    type: 'Column',            // Chart type
    name: 'Monthly Sales',     // Series label
    visible: true              // Show/hide series
  }]
}, '#chart');
```

### Multiple Series

Combine multiple data series for comparison:

```typescript
import { Chart, ColumnSeries, Category } from '@syncfusion/ej2-charts';

Chart.Inject(ColumnSeries, Category);

const chart = new Chart({
  primaryXAxis: { valueType: 'Category' },
  series: [
    {
      dataSource: data,
      xName: 'quarter',
      yName: 'sales',
      type: 'Column',
      name: 'Sales'
    },
    {
      dataSource: data,
      xName: 'quarter',
      yName: 'revenue',
      type: 'Column',
      name: 'Revenue'
    }
  ]
}, '#chart');
```

### Series Color & Styling

```typescript
import { Chart, LineSeriesLineSeries, Category } from '@syncfusion/ej2-charts';

Chart.Inject(LineSeries, Category);

const chart = new Chart({
  primaryXAxis: { valueType: 'Category' },
  series: [{
    dataSource: data,
    xName: 'x',
    yName: 'y',
    type: 'Line',
    fill: '#FF5733',           // Line/fill color
    width: 2,                  // Line width
    opacity: 0.8,              // Transparency (0-1)
    cornerRadius: {topLeft: 5, topRight: 5 }     // Corner roundness (for columns)
  }]
}, '#chart');
```

## Data Labels

### Enable Data Labels

Display values on/near data points:

```typescript
import { Chart, ColumnSeries, DataLabel, Category } from '@syncfusion/ej2-charts';

Chart.Inject(ColumnSeries, DataLabel, Category);

const chart = new Chart({
  primaryXAxis: { valueType: 'Category' },
  series: [{
    dataSource: data,
    xName: 'x',
    yName: 'y',
    type: 'Column',
    marker: {
      dataLabel: {
        visible: true
      }
    }
  }]
}, '#chart');
```

### Data Label Formatting

```typescript
import { Chart, ColumnSeries, DataLabel, Category } from '@syncfusion/ej2-charts';

Chart.Inject(ColumnSeries, DataLabel, Category);

const chart = new Chart({
  primaryXAxis: { valueType: 'Category' },
  series: [{
    dataSource: data,
    xName: 'x',
    yName: 'y',
    type: 'Column',
    marker: {
      dataLabel: {
        visible: true,
        position: 'Top',           // Top, Bottom, Middle, Auto
        margin: { left: 5, right: 5 },
        font: {
          size: '16px',
          color: '#333',
          fontFamily: 'Arial'
        }
      }
    }
  }]
}, '#chart');
```

### Data Label Positions

Supported positions vary by chart type:

```typescript
// For Column/Bar/Line charts
marker: {
  dataLabel: {
    position: 'Top'      // Above the column
    // Other options: 'Bottom', 'Middle', 'Auto'
  }
}

// For Pie/Doughnut charts
marker: {
  dataLabel: {
    position: 'Inside'   // Inside slice
    // Other options: 'Outside'
  }
}
```

### Advanced Label Customization

```typescript
import { Chart, ColumnSeries, DataLabel, Category } from '@syncfusion/ej2-charts';

Chart.Inject(ColumnSeries, DataLabel, Category);

const chart = new Chart({
  primaryXAxis: { valueType: 'Category' },
  series: [{
    dataSource: data,
    xName: 'x',
    yName: 'y',
    type: 'Column',
    marker: {
      dataLabel: {
        visible: true,
        fill: '#FFFFFF',
        border: { color: '#333', width: 1 }
      }
    }
  }]
}, '#chart');
```

## Marker Customization

### Enable Markers

```typescript
import { Chart, LineSeries, DateTime } from '@syncfusion/ej2-charts';

Chart.Inject(LineSeries, DateTime);

const chart = new Chart({
  primaryXAxis: { valueType: 'DateTime' },
  series: [{
    dataSource: data,
    xName: 'date',
    yName: 'value',
    type: 'Line',
    marker: {
      visible: true,
      width: 8,
      height: 8,
      shape: 'Circle'    // Circle, Rectangle, Triangle, Diamond, Cross, Plus, Star
    }
  }]
}, '#chart');
```

### Marker Styling

```typescript
import { Chart, LineSeries, DateTime } from '@syncfusion/ej2-charts';

Chart.Inject(LineSeries, DateTime);

const chart = new Chart({
  primaryXAxis: { valueType: 'DateTime' },
  series: [{
    dataSource: data,
    xName: 'date',
    yName: 'value',
    type: 'Line',
    marker: {
      visible: true,
      shape: 'Diamond',
      width: 12,
      height: 12,
      fill: '#FF5733',
      border: {
        color: '#FFF',
        width: 2
      },
      opacity: 1
    }
  }]
}, '#chart');
```

### Marker Types

```typescript
shape: 'Circle'      // Standard circle
shape: 'Rectangle'   // Square/rect
shape: 'Triangle'    // Triangle pointing up
shape: 'Diamond'     // Diamond shape
shape: 'Cross'       // Plus/cross
shape: 'Star'        // 5-point star
shape: 'Image'       // Custom image
```

### Custom Image Markers

```typescript
import { Chart, LineSeries, DateTime } from '@syncfusion/ej2-charts';

Chart.Inject(LineSeries, DateTime);

const chart = new Chart({
  primaryXAxis: { valueType: 'DateTime' },
  series: [{
    dataSource: data,
    xName: 'date',
    yName: 'value',
    type: 'Line',
    marker: {
      visible: true,
      shape: 'Image',
      imageUrl: '/assets/marker.png',
      width: 16,
      height: 16
    }
  }]
}, '#chart');
```

## Stacking & Grouping

### Stacking Column/Area Charts

**Absolute stacking (values cumulative):**
```typescript
import { Chart, StackingColumnSeries, Category } from '@syncfusion/ej2-charts';

Chart.Inject(StackingColumnSeries, Category);

const chart = new Chart({
  primaryXAxis: { valueType: 'Category' },
  series: [
    { dataSource: data, xName: 'x', yName: 'value1', type: 'StackingColumn', name: 'Q1' },
    { dataSource: data, xName: 'x', yName: 'value2', type: 'StackingColumn', name: 'Q2' },
    { dataSource: data, xName: 'x', yName: 'value3', type: 'StackingColumn', name: 'Q3' }
  ]
}, '#chart');
```

**Percentage stacking (normalized to 100%):**
```typescript
import { Chart, StackingColumnSeries, Category } from '@syncfusion/ej2-charts';

Chart.Inject(StackingColumnSeries, Category);

const chart = new Chart({
  primaryXAxis: { valueType: 'Category' },
  series: [
    { dataSource: data, xName: 'x', yName: 'value1', type: 'StackingColumn100' },
    { dataSource: data, xName: 'x', yName: 'value2', type: 'StackingColumn100' }
  ]
}, '#chart');
```

### Grouping Series

Display series side-by-side for easy comparison:

```typescript
import { Chart, ColumnSeries, Category } from '@syncfusion/ej2-charts';

Chart.Inject(ColumnSeries, Category);

const chart = new Chart({
  primaryXAxis: { valueType: 'Category' },
  series: [
    { dataSource: data, xName: 'month', yName: 'jan', type: 'Column', name: 'January' },
    { dataSource: data, xName: 'month', yName: 'feb', type: 'Column', name: 'February' }
  ]
}, '#chart');
```

### Group Padding

```typescript
import { Chart, ColumnSeries, Category } from '@syncfusion/ej2-charts';

Chart.Inject(ColumnSeries, Category);

const chart = new Chart({
  primaryXAxis: { valueType: 'Category' },
  series: [{
    dataSource: data,
    xName: 'x',
    yName: 'y',
    type: 'Column',
    columnSpacing: 0.1,     // Space between columns (0-1)
    columnWidth: 0.8        // Column width (0-1)
  }]
}, '#chart');
```

## Empty Points

### Handling Missing Data

When data has gaps or null values:

```typescript
import { Chart, LineSeries, Category } from '@syncfusion/ej2-charts';

Chart.Inject(LineSeries, Category);

const data = [
  { x: 'Jan', y: 20 },
  { x: 'Feb', y: null },    // Missing value
  { x: 'Mar', y: 30 },
  { x: 'Apr', y: undefined }
];

const chart = new Chart({
  primaryXAxis: { valueType: 'Category' },
  series: [{
    dataSource: data,
    xName: 'x',
    yName: 'y',
    type: 'Line',
    emptyPointSettings: {
      mode: 'Gap',            // Gap, Zero, Average, Interpolate
      fill: '#CCCCCC',
      border: { color: '#999' }
    }
  }]
}, '#chart');
```

### Empty Point Modes

```typescript
mode: 'Gap'           // Skip empty point, break line
mode: 'Zero'          // Treat as 0
mode: 'Average'       // Use average of neighbors
mode: 'Interpolate'   // Linear interpolation
```

### Visibility Options

```typescript
emptyPointSettings: {
  mode: 'Gap',
  visible: false      // Don't show empty points
}
```

## Common Configurations

### Configuration 1: Sales Comparison Chart

```typescript
series: [
  {
    dataSource: salesData,
    xName: 'quarter',
    yName: 'amount',
    type: 'Column',
    name: '2023',
    cornerRadius: {topLeft:3, topRight: 3},
    marker: { dataLabel: { visible: true, position: 'Top' } }
  },
  {
    dataSource: salesData,
    xName: 'quarter',
    yName: 'previousYear',
    type: 'Column',
    name: '2022',
    cornerRadius: {topLeft:3, topRight: 3},
  }
],
tooltip: {
  enable: true,
  format: '${series.name}: ${point.y}'
},
legendSettings: { visible: true }
```

### Configuration 2: Multi-Axis Series

Mix different chart types:

```typescript
series: [
  {
    dataSource: data,
    xName: 'date',
    yName: 'temperature',
    type: 'Line',
    yAxisName: 'YAxis',
    marker: { visible: true },
    name: 'Temperature'
  },
  {
    dataSource: data,
    xName: 'date',
    yName: 'humidity',
    type: 'Column',
    yAxisName: 'YAxis2',
    name: 'Humidity',
    opacity: 0.6
  }
],
axes: [
  { name: 'YAxis', title: 'Temp (°C)' },
  { name: 'YAxis2', title: 'Humidity (%)', opposedPosition: true }
]
```

### Configuration 3: Real-Time Data with Markers

```typescript
series: [{
  dataSource: realtimeData,
  xName: 'timestamp',
  yName: 'value',
  type: 'Line',
  width: 2,
  marker: {
    visible: true,
    width: 6,
    height: 6,
    shape: 'Circle',
    dataLabel: {
      visible: true,
      position: 'Top',
      font: { size: '10px' }
    }
  },
}]
```

