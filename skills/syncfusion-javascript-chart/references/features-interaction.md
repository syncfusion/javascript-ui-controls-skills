# Features & Interaction

## Table of Contents
- [Tooltips](#tooltips)
- [Crosshairs & Track Ball](#crosshairs--track-ball)
- [Legend](#legend)
- [Selection](#selection)
- [Zooming & Panning](#zooming--panning)
- [Annotations](#annotations)

**Note:** Configuration syntax is identical for TypeScript and JavaScript. Examples use object literal format.

**API Reference:** See [api-reference.md](api-reference.md#configuration-classes) for complete `CrosshairSettings`, `LegendSettings`, `ZoomSettings`, and `ChartAnnotationSettings` class documentation.

## Tooltips

### Enable Tooltips

```typescript
import { Chart, ColumnSeries, Tooltip, Category } from '@syncfusion/ej2-charts';

Chart.Inject(ColumnSeries, Tooltip, Category);

const chart = new Chart({
  primaryXAxis: { valueType: 'Category' },
  tooltip: { enable: true },
  series: [{ dataSource: data, xName: 'x', yName: 'y', type: 'Column' }]
}, '#chart');
```

### Tooltip Formatting

```typescript
import { Chart, ColumnSeries, Tooltip, Category } from '@syncfusion/ej2-charts';

Chart.Inject(ColumnSeries, Tooltip, Category);

const chart = new Chart({
  primaryXAxis: { valueType: 'Category' },
  tooltip: {
    enable: true,
    format: '<b>${point.x}</b>: ${point.y}%',
    padding: { left: 10, right: 10, top: 5, bottom: 5 }
  },
  series: [{ dataSource: data, xName: 'x', yName: 'y', type: 'Column' }]
}, '#chart');
```

### Tooltip Properties

```typescript
import { Chart, ColumnSeries, Tooltip, Category } from '@syncfusion/ej2-charts';

Chart.Inject(ColumnSeries, Tooltip, Category);

const chart = new Chart({
  primaryXAxis: { valueType: 'Category' },
  tooltip: {
    enable: true,
    duration: 1000,            // Fade out duration (ms)
    enableAnimation: true,
    fill: '#FFFFFF',
    border: { color: '#333', width: 1 },
    opacity: 0.9
  },
  series: [{ dataSource: data, xName: 'x', yName: 'y', type: 'Column' }]
}, '#chart');
```

### Shared Tooltip (Multiple Series)

Display tooltip for all series at same X value:

```typescript
import { Chart, ColumnSeries, Tooltip, Category } from '@syncfusion/ej2-charts';

Chart.Inject(ColumnSeries, Tooltip, Category);

const chart = new Chart({
  primaryXAxis: { valueType: 'Category' },
  tooltip: {
    enable: true,
    shared: true,
    format: '<b>${point.x}</b><br/>${series.name}: ${point.y}'
  },
  series: [
    { dataSource: data, xName: 'x', yName: 'y1', type: 'Column', name: 'Series 1' },
    { dataSource: data, xName: 'x', yName: 'y2', type: 'Column', name: 'Series 2' }
  ]
}, '#chart');
```

## Crosshairs & Track Ball

### Crosshair (Vertical + Horizontal Lines)

```typescript
import { Chart, LineSeries, Tooltip, Category } from '@syncfusion/ej2-charts';

Chart.Inject(LineSeries, Tooltip, Category);

const chart = new Chart({
  primaryXAxis: { valueType: 'Category' },
  crosshair: {
    enable: true,
    lineType: 'Vertical',      // Vertical, Horizontal, Both
    line: {
      width: 1,
      color: '#999'
    }
  },
  series: [{ dataSource: data, xName: 'x', yName: 'y', type: 'Line' }]
}, '#chart');
```

### Crosshair with Tooltip

```typescript
import { Chart, LineSeries, Tooltip, Category } from '@syncfusion/ej2-charts';

Chart.Inject(LineSeries, Tooltip, Category);

const chart = new Chart({
  primaryXAxis: { valueType: 'Category' },
  crosshair: {
    enable: true,
    lineType: 'Both',          // Both vertical and horizontal
    line: { width: 1 }
  },
  tooltip: {
    enable: true,
    shared: true,
    format: '${series.name}: ${point.y}'
  },
  series: [{ dataSource: data, xName: 'x', yName: 'y', type: 'Line', name: 'Sales' }]
}, '#chart');
```

## Legend

### Enable & Position Legend

```typescript
import { Chart, LineSeries, Legend, Category } from '@syncfusion/ej2-charts';

Chart.Inject(LineSeries, Legend, Category);

const chart = new Chart({
  primaryXAxis: { valueType: 'Category' },
  legendSettings: {
    visible: true,
    position: 'Bottom',        // Top, Bottom, Left, Right
    alignment: 'Center'        // Near, Center, Far
  },
  series: [{ dataSource: data, xName: 'x', yName: 'y', type: 'Line', name: 'Sales' }]
}, '#chart');
```

### Legend Styling

```typescript
import { Chart, LineSeries, Legend, Category } from '@syncfusion/ej2-charts';

Chart.Inject(LineSeries, Legend, Category);

const chart = new Chart({
  primaryXAxis: { valueType: 'Category' },
  legendSettings: {
    visible: true,
    position: 'Right',
    textStyle: {
      size: '14px',
      color: '#333',
      fontFamily: 'Arial'
    }
  },
  series: [{ dataSource: data, xName: 'x', yName: 'y', type: 'Line', name: 'Sales' }]
}, '#chart');
```

### Interactive Legend

```typescript
import { Chart, LineSeries, Legend, Category } from '@syncfusion/ej2-charts';

Chart.Inject(LineSeries, Legend, Category);

const chart = new Chart({
  primaryXAxis: { valueType: 'Category' },
  legendSettings: {
    visible: true,
    enableHighlight: true      // Highlight series on hover
  },
  series: [
    { dataSource: data, xName: 'x', yName: 'y1', type: 'Line', name: 'Series 1' },
    { dataSource: data, xName: 'x', yName: 'y2', type: 'Line', name: 'Series 2' }
  ]
}, '#chart');
```

## Selection

### Point Selection

Select individual data points:

```typescript
import { Chart, ColumnSeries, Category, Selection } from '@syncfusion/ej2-charts';

Chart.Inject(ColumnSeries, Category, Selection);

const chart = new Chart({
  primaryXAxis: { valueType: 'Category' },
  selectionMode: 'Point',    // Point, Series, Cluster
  series: [{ dataSource: data, xName: 'x', yName: 'y', type: 'Column' }]
}, '#chart');
```

### Series Selection

Select entire series:

```typescript
import { Chart, ColumnSeries, Category } from '@syncfusion/ej2-charts';

Chart.Inject(ColumnSeries, Category);

const chart = new Chart({
  primaryXAxis: { valueType: 'Category' },
  selectionMode: 'Series',
  series: [{ dataSource: data1, xName: 'x', yName: 'y', type: 'Column' },
    { dataSource: data2, xName: 'x', yName: 'y', type: 'Column' }
  ]
}, '#chart');
```

### Point Render Event (Custom Styling)

```typescript
import { Chart, ColumnSeries, Category, IPointRenderEventArgs } from '@syncfusion/ej2-charts';

Chart.Inject(ColumnSeries, Category);

const chart = new Chart({
  primaryXAxis: { valueType: 'Category' },
  pointRender: (args: IPointRenderEventArgs) => {
    if (args.point.y > 50) {
      args.fill = '#FF5733';   // Red for high values
    } else if (args.point.y < 20) {
      args.fill = '#33FF57';   // Green for low values
    }
  },
  series: [{ dataSource: data, xName: 'x', yName: 'y', type: 'Column' }]
}, '#chart');
```

## Zooming & Panning

### Enable Zooming

```typescript
import { Chart, LineSeries, Category, Zoom } from '@syncfusion/ej2-charts';

Chart.Inject(LineSeries, Category, Zoom);

const chart = new Chart({
  primaryXAxis: { valueType: 'Category' },
  zoomSettings: {
    enableMouseWheelZooming: true,
    mode: 'X',                 // X, Y, XY
    toolbarItems: ['Zoom', 'ZoomIn', 'ZoomOut', 'Pan', 'Reset']
  },
  series: [{ dataSource: data, xName: 'x', yName: 'y', type: 'Line' }]
}, '#chart');
```

### Panning with Mouse Wheel

```typescript
import { Chart, LineSeries, Category, Zoom } from '@syncfusion/ej2-charts';

Chart.Inject(LineSeries, Category, Zoom);

const chart = new Chart({
  primaryXAxis: { valueType: 'Category' },
  zoomSettings: {
    enablePan: true,
    mode: 'XY',
    enablePinchZooming: true,
    enableMouseWheelZooming: true,
    enableSelectionZooming: true
  },
  series: [{ dataSource: data, xName: 'x', yName: 'y', type: 'Line' }]
}, '#chart');
```

## Annotations

### Text Annotation

```typescript
import { Chart, ColumnSeries, Category, ChartAnnotation } from '@syncfusion/ej2-charts';

Chart.Inject(ColumnSeries, Category, ChartAnnotation);

const chart = new Chart({
  primaryXAxis: { valueType: 'Category' },
  annotations: [
    {
      content: '<div style="color:blue;font-weight:bold">Peak Feb</div>',
      x: 'Feb',
      y: 35,
      coordinateUnits: 'Point'
    },
    {
      content: '<div style="color:green;">Secondary Pane Note</div>',
      x: 'Jan',
      y: 60,
      coordinateUnits: 'Point',
      yAxisName: 'secondaryAxis'
    }
  ],
  series: [{ dataSource: data, xName: 'x', yName: 'y', type: 'Column' }]
}, '#chart');
```

## Common Configurations

### Configuration 1: Interactive Dashboard

```typescript
import { Chart, LineSeries, Legend, Tooltip, Category, Zoom } from '@syncfusion/ej2-charts';

Chart.Inject(LineSeries, Legend, Tooltip, Category, Zoom);

const chart = new Chart({
  primaryXAxis: { valueType: 'Category' },
  tooltip: {
    enable: true,
    shared: true,
    format: '${series.name}: ${point.y}'
  },
  crosshair: {
    enable: true,
    lineType: 'Both'
  },
  legendSettings: {
    visible: true,
    position: 'Bottom'
  },
  zoomSettings: {
    enableSelectionZooming: true,
    mode: 'XY',
    toolbarItems: ['Zoom', 'ZoomIn', 'ZoomOut', 'Pan', 'Reset']
  },
  series: [{ dataSource: data, xName: 'x', yName: 'y', type: 'Line', name: 'Sales' }]
}, '#chart');
```

### Configuration 2: Stock Chart with Zoom

```typescript
import { Chart, CandleSeries, DateTime, Tooltip, Zoom } from '@syncfusion/ej2-charts';

Chart.Inject(CandleSeries, DateTime, Tooltip, Zoom);

const chart = new Chart({
  primaryXAxis: { valueType: 'DateTime' },
  tooltip: {
    enable: true,
    shared: true
  },
  crosshair: {
    enable: true,
    lineType: 'Vertical',
    line: { width: 1 }
  },
  zoomSettings: {
    enableScrollbar: true,
    mode: 'X',
    enableMouseWheelZooming: true
  },
  series: [{
    dataSource: data,
    xName: 'date',
    low: 'low', high: 'high', open: 'open', close: 'close',
    type: 'Candle'
  }]
}, '#chart');
```

### Configuration 3: Multi-Series with Legend Interaction

```typescript
import { Chart, LineSeries, AreaSeries, Legend, Category } from '@syncfusion/ej2-charts';

Chart.Inject(LineSeries, AreaSeries, Legend, Category);

const chart = new Chart({
  primaryXAxis: { valueType: 'Category' },
  legendSettings: {
    visible: true,
    position: 'Right',
    enableHighlight: true,
    textStyle: { size: '12px' }
  },
  series: [
    { dataSource: data, xName: 'x', yName: 'sales', type: 'Line', name: 'Sales' },
    { dataSource: data, xName: 'x', yName: 'profit', type: 'Line', name: 'Profit' },
    { dataSource: data, xName: 'x', yName: 'revenue', type: 'Area', name: 'Revenue', opacity: 0.5 }
  ]
}, '#chart');
```

## Event Handlers

### Point Event

```typescript
pointRender: (args: IPointRenderEventArgs) => {
  if (args.point.index === 0) {
    args.border = { color: '#FF5733', width: 2 };
  }
}
```

### Legend Event

```typescript
legendRender: (args: ILegendRenderEventArgs) => {
  args.text = args.text.toUpperCase();
}
```

### Mouse Event

```typescript
chartMouseMove: (args: IMouseEventArgs) => {
  console.log(`Position: ${args.x}, ${args.y}`);
}
```
