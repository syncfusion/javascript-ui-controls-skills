# Getting Started with Charts

## Table of Contents
- [Setup (TypeScript)](#setup-typescript)
- [Setup (JavaScript)](#setup-javascript)
- [Basic Chart Creation](#basic-chart-creation)
- [Data Binding Fundamentals](#data-binding-fundamentals)
- [Theme Selection](#theme-selection)
- [Common Scenarios](#common-scenarios)
- [Chart Samples With ALl propetties](#chart-samples-with-all-propeties)

**API Reference:** See [api-reference.md](api-reference.md) for the complete Chart API reference, including all classes, methods, and configuration options.

## Setup (TypeScript)

### Package Installation

```bash
npm install @syncfusion/ej2-charts
npm install @syncfusion/ej2-base
```

### Import Required Modules

```typescript
import { Chart, LineSeries, Tooltip, Legend, DateTime, Category } from '@syncfusion/ej2-charts';

// Register required chart modules with Chart.Inject()
Chart.Inject(LineSeries, Tooltip, Legend, DateTime, Category);
```

### HTML Container

Add a container div for the chart:

```html
<div id="chart"></div>
```

## Basic Chart Creation

### Minimal Example: Line Chart (TypeScript)

```typescript
import { Chart, LineSeries, Category } from '@syncfusion/ej2-charts';

Chart.Inject(LineSeries, Category);

const chart = new Chart({
  primaryXAxis: {
    valueType: 'Category'
  },
  series: [{
    dataSource: [
      { month: 'Jan', sales: 21 },
      { month: 'Feb', sales: 28 },
      { month: 'Mar', sales: 19 },
      { month: 'Apr', sales: 36 }
    ],
    xName: 'month',
    yName: 'sales',
    type: 'Line'
  }],
  title: 'Monthly Sales',
  height: '420px',
  width: '100%'
}, '#chart');
```

**What happens:**
1. Imports Chart and required modules
2. Injects LineSeries and Category modules
3. Defines X-axis as category type
4. Maps data fields (month → xName, sales → yName)
5. Creates line series connecting data points
6. Renders chart with specified dimensions

## Data Binding Fundamentals

### Data Format

Charts expect array of objects:

**TypeScript:**
```typescript
const data = [
  { month: 'Jan', sales: 35, revenue: 42 },
  { month: 'Feb', sales: 28, revenue: 45 },
  { month: 'Mar', sales: 34, revenue: 38 }
];
```

### Mapping Data Fields

Use `xName` and `yName` to map data:

```typescript
series: [{
  dataSource: data,
  xName: 'month',      // X-axis values
  yName: 'sales',      // Y-axis values
  type: 'Column'
}]
```

### Dynamic Data Assignment

Update chart data after initialization:

```typescript
chart.series[0].dataSource = newData;
chart.refresh();
```

## Theme Selection

### Built-in Themes

```typescript
// TypeScript
const chart = new Chart({
  theme: 'Material',
  // ... rest of config
}, '#chart');
```

**Available themes:**
- `Material` - Google Material Design
- `Fabric` - Microsoft Fluent Design
- `Bootstrap` - Bootstrap theme
- `Highcontrast` - High contrast for accessibility
- `Fluent` - Microsoft Fluent Design (v20+)
- `Bootstrap4` - Bootstrap 4 theme

### Custom Colors

**TypeScript:**
```typescript
const chart = new Chart({
  palette: ['#FF5733', '#33FF57', '#3357FF', '#FF33F1'],
  // ... rest of config
}, '#chart');
```

## Configuration Options

### Title & Subtitle

```typescript
{
  title: 'Quarterly Sales Report',
  subTitle: 'FY 2024',
  titleStyle: {
    fontFamily: 'Arial',
    fontStyle: 'italic'
  }
}
```

### Tooltip Configuration

```typescript
{
  tooltip: {
    enable: true,              // Enable tooltips
    format: '<b>${point.x}</b>: ${point.y}%',
    shared: true               // Shared for multiple series
  }
}
```

### Legend Positioning

```typescript
{
  legend: {
    visible: true,
    position: 'Bottom',        // Top, Bottom, Left, Right, Custom
    alignment: 'Center'
  }
}
```

## Common Scenarios

### Scenario 1: Dashboard Chart with Real Data (TypeScript)

```typescript
import { Chart, LineSeries, DateTime, Tooltip } from '@syncfusion/ej2-charts';

Chart.Inject(LineSeries, DateTime, Tooltip);

// Fetch data from API
fetch('/api/sales')
  .then(res => res.json())
  .then(data => {
    const chart = new Chart({
      primaryXAxis: { valueType: 'DateTime' },
      tooltip: { enable: true },
      title: 'Daily Sales',
      series: [{
        dataSource: data,
        xName: 'date',
        yName: 'amount',
        type: 'Line',
        name: 'Daily Sales'
      }]
    }, '#chart');
  });
```

### Scenario 2: Responsive Chart (TypeScript)

```typescript
import { Chart, ColumnSeries } from '@syncfusion/ej2-charts';

Chart.Inject(ColumnSeries);

const chart = new Chart({
  width: '100%',
  height: '420px',
  series: [{
    dataSource: data,
    xName: 'x',
    yName: 'y',
    type: 'Column'
  }]
}, '#chart');
```

### Chart samples with all propeties

```typescript
import {
  Chart, LineSeries, ColumnSeries, SplineSeries,
  Category, DateTime, Logarithmic, Tooltip, Crosshair,
  Zoom, Legend, DataLabel, StripLine, Trendlines,
  Export, HiloOpenCloseSeries, StochasticIndicator,
  BollingerBands, RsiIndicator, MultiColoredLineSeries,
  SeriesLabel, ChartAnnotation, DateTimeCategory
} from '@syncfusion/ej2-charts';

Chart.Inject(
  LineSeries, ColumnSeries, SplineSeries,
  Category, DateTime, DateTimeCategory, Logarithmic,
  Tooltip, Crosshair, Zoom, DataLabel, Legend,
  StripLine, Trendlines, Export, HiloOpenCloseSeries,
  StochasticIndicator, BollingerBands, RsiIndicator,
  MultiColoredLineSeries, SeriesLabel, ChartAnnotation
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

  // =====================================================================
  //  FULLY-EXPANDED SERIES MARKER + DATA LABEL CONFIG
  // =====================================================================
  series: [
    {
      type: 'Line',
      dataSource: data,
      xName: 'x',
      yName: 'y',
      name: 'Sales',
      width: 2,

      marker: {
        visible: true,
        shape: 'Circle',
        width: 12,
        height: 12,

        // Border config
        border: { width: 2, color: '#000000' },

        // Fill
        fill: '#ffffff',

        // Opacity
        opacity: 1,

        // MARKER DATA LABELS
        dataLabel: {
          visible: true,
          position: 'Top',
          name: 'text',

          // Border
          border: { width: 1, color: '#000' },

          // Background & Color
          fill: '#fff',
          rx: 4,
          ry: 4,

          // Margin
          margin: { left: 4, right: 4, top: 4, bottom: 4 },

          // Font
          font: {
            size: '12px',
            fontWeight: '600',
            color: '#000',
            fontStyle: 'Normal',
            fontFamily: 'Segoe UI'
          },

          // Alignment
          alignment: 'Center',

          // Templates
          //template: <div> <div?,

          // Rotation & Angle
          angle: 0
        }
      },

      animation: { enable: true },

      trendlines: [
        { type: 'Linear', name: 'Linear Trendline', width: 2, fill: 'red' }
      ]
    },

    {
      type: 'Column',
      dataSource: data,
      xName: 'x',
      yName: 'y',
      name: 'Profit',
      border: { width: 1, color: 'black' },
      columnSpacing: 0.2,

      marker: {
        visible: false,
        dataLabel: {
          visible: true,
          position: 'Middle',
          font: { size: '11px', fontWeight: '500' }
        }
      }
    },

    {
      type: 'HiloOpenClose',
      dataSource: data,
      xName: 'x',
      high: 'high',
      low: 'low',
      open: 'open',
      close: 'close',
      name: 'OHLC',
      yAxisName: 'secondaryAxis'
    },

    {
      type: 'Spline',
      dataSource: data,
      xName: 'x',
      yName: 'y',
      name: 'Trend Spline',
      width: 2,
      marker: {
        visible: true,
        shape: 'Diamond'
      }
    }
  ],

  // =====================================================================
  //   FULL LEGEND SETTINGS WITH ALL AVAILABLE PROPERTIES
  // =====================================================================
  legendSettings: {
    visible: true,
    background: '#ffffff',
    border: { width: 1, color: '#ccc' },
    padding: 10,
    shapeHeight: 15,
    shapeWidth: 15,
    shapePadding: 10,
    textStyle: {
      size: '13px',
      fontWeight: '400',
      color: '#000'
    },
    position: 'Bottom',
    // alignment: 'Center',
    // width: '100%',
    // height: 'auto',
    // toggleVisibility: true,
    // reverse: false,
    // isInversed: false,

    // // Page / Scroll options
    // enablePages: true,
    // maximumLabelWidth: 100,

    // // Row & Column Distribution
    // itemPadding: 8,

    // Themes
    mode: 'Series',
  },

  indicators: [
    { type: 'Stochastic', seriesName: 'Sales', period: 3, kPeriod: 3, dPeriod: 3 },
    { type: 'BollingerBands', seriesName: 'Sales', period: 14, standardDeviation: 2 },
    { type: 'Rsi', seriesName: 'Sales', period: 14 }
  ],

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

  tooltip: {
    enable: true,
    shared: true,
    format: '${series.name}: ${point.y}',
  },

  crosshair: { enable: true },

  zoomSettings: {
    enableSelectionZooming: true,
    enableMouseWheelZooming: true,
    enablePan: true,
    mode: 'XY'
  },
  theme: 'Material',
  width: '100%',
  height: '100%',
  backgroundImage: './chart.png',
  loaded: (args) => console.log('Chart Loaded', args),
  pointRender: (args) => console.log('Point Rendered', args),
  pointClick: (args) => console.log('Point Clicked', args),
  seriesRender: (args) => { console.log('Series Rendered', args); args.name = args.name + '1'; },
  legendClick: (args) => console.log('Legend Clicked', args),
  zoomComplete: (args) => console.log('Zoom Completed', args)
});

chart.appendTo('#element');
```
