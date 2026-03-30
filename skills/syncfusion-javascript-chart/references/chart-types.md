# Chart Types & Selection

## Table of Contents
- [Overview](#overview)
- [Categorical Charts](#categorical-charts)
- [Circular Charts](#circular-charts)
- [Polar & Radar](#polar--radar)
- [Specialized Charts](#specialized-charts)
- [Comparison Matrix](#comparison-matrix)

## Overview

Syncfusion provides 15+ chart types optimized for different data visualization scenarios. Choose based on your data structure and analysis needs. **Configuration syntax is identical in TypeScript and JavaScript.**

**API Reference:** See [api-reference.md](api-reference.md#chartseries-type) for complete `ChartSeriesType` enumeration with all available chart types.

**Quick Selection:**
- **Compare values across categories** → Column, Bar, Line
- **Show trends over time** → Line, Area, Spline
- **Compare parts of a whole** → Pie, Doughnut
- **Show distribution** → Histogram, Box and Whisker
- **Financial/Stock data** → Candlestick, OHLC, HiLo
- **Technical analysis** → Line with Indicators, Trendlines

## Categorical Charts

### Line Chart

Best for: Trends, continuous data, multiple series comparison

```typescript
import { Chart, LineSeries, Category } from '@syncfusion/ej2-charts';

Chart.Inject(LineSeries, Category);

const chart = new Chart({
  primaryXAxis: { valueType: 'Category' },
  series: [{
    dataSource: data,
    xName: 'month',
    yName: 'sales',
    type: 'Line',
    marker: { visible: true, width: 10, height: 10 },
    width: 2
  }]
}, '#chart');
```

**Use cases:**
- Stock price trends
- Temperature changes over time
- Website traffic patterns
- Performance metrics

### Area Chart

Best for: Cumulative trends, emphasis on magnitude, stacked comparisons

```typescript
import { Chart, AreaSeries, DateTime } from '@syncfusion/ej2-charts';

Chart.Inject(AreaSeries, DateTime);

const chart = new Chart({
  primaryXAxis: { valueType: 'DateTime' },
  series: [{
    dataSource: data,
    xName: 'date',
    yName: 'revenue',
    type: 'Area',
    opacity: 0.6
  }]
}, '#chart');
```

**Variants:**
- `Area` - Simple filled area
- `StackingArea` - Stacked areas
- `SplineArea` - Smooth curves
- `StackingArea100` - Percentage stacked

### Bar Chart

Best for: Horizontal comparisons, long labels, rankings

```typescript
import { Chart, BarSeries, Category } from '@syncfusion/ej2-charts';

Chart.Inject(BarSeries, Category);

const chart = new Chart({
  primaryXAxis: { valueType: 'Category' },
  series: [{
    dataSource: data,
    xName: 'value',
    yName: 'category',
    type: 'Bar'
  }]
}, '#chart');
```

**Configuration notes:**
- X-axis is typically numeric for bar charts
- Y-axis should be category type
- Bar charts require Y-axis to be category type and X-axis numeric

**Use cases:**
- Department comparisons
- Top performers ranking
- Budget allocation
- Geographic data

### Column Chart

Best for: Categorical comparisons, vertical emphasis, side-by-side comparison

```typescript
import { Chart, ColumnSeries, Category } from '@syncfusion/ej2-charts';

Chart.Inject(ColumnSeries, Category);

const chart = new Chart({
  primaryXAxis: { valueType: 'Category' },
  series: [{
    dataSource: data,
    xName: 'quarter',
    yName: 'profit',
    type: 'Column',
    cornerRadius: {topLeft: 3, topRight: 3}
  }]
}, '#chart');
```

**Variants:**
- `Column` - Grouped columns
- `StackingColumn` - Stacked (absolute values)
- `StackingColumn100` - Stacked (percentage)
- `RangeColumn` - Range values

### Scatter Chart

Best for: Correlation analysis, distribution patterns, outlier detection

```typescript
import { Chart, ScatterSeries } from '@syncfusion/ej2-charts';

Chart.Inject(ScatterSeries);

const chart = new Chart({
  primaryXAxis: { valueType: 'Double' },
  primaryYAxis: { valueType: 'Double' },
  series: [{
    dataSource: data,
    xName: 'age',
    yName: 'income',
    type: 'Scatter',
    marker: { width: 15, height: 15 }
  }]
}, '#chart');
```

**Use cases:**
- Correlation studies
- Outlier detection
- Multivariate analysis
- Bubble charts (3D scatter)

### Bubble Chart

Best for: Three-variable relationships (X, Y, bubble size)

```typescript
import { Chart, BubbleSeries, Tooltip } from '@syncfusion/ej2-charts';

Chart.Inject(BubbleSeries, Tooltip);

const chart = new Chart({
  primaryXAxis: { valueType: 'Double' },
  primaryYAxis: { valueType: 'Double' },
  tooltip: { enable: true },
  series: [{
    dataSource: data,
    xName: 'gdp',
    yName: 'lifeExpectancy',
    size: 'population',
    type: 'Bubble'
  }]
}, '#chart');
```

**Properties:**
- `size` - Bubble size (maps to third variable)
- `marker.width/height` - Min/max bubble size

## Polar & Radar

### Polar Chart

Best for: Multi-dimensional data, performance metrics

```typescript
import { Chart, PolarSeries, Category } from '@syncfusion/ej2-charts';

Chart.Inject(PolarSeries, Category);

const chart = new Chart({
  primaryXAxis: { valueType: 'Category' },
  series: [{
    dataSource: data,
    xName: 'skill',
    yName: 'proficiency',
    type: 'Polar'
  }]
}, '#chart');
```

**Subtypes:** Line, Spline, Area, StackingArea, Scatter

### Radar Chart

Polar area chart (closed area):

```typescript
import { Chart, RadarSeries, Category } from '@syncfusion/ej2-charts';

Chart.Inject(RadarSeries, Category);

const chart = new Chart({
  primaryXAxis: { valueType: 'Category' },
  series: [{
    dataSource: data,
    xName: 'category',
    yName: 'value',
    type: 'Radar'
  }]
}, '#chart');
```

**Use cases:**
- Skill assessments
- Product comparisons
- Performance dashboards
- Multi-criteria analysis

## Specialized Charts

### Histogram

Best for: Distribution analysis, frequency distribution

```typescript
import { Chart, HistogramSeries } from '@syncfusion/ej2-charts';

Chart.Inject(HistogramSeries);

const chart = new Chart({
  series: [
    {
      dataSource: chartData,
      type: 'Histogram', 
      width: 2, 
      yName: 'y',
      binInterval: 20,
      showNormalDistribution: true, 
      columnWidth: 0.99
  }
]}, '#chart');
```

**Properties:**
- `binInterval` - Bin size/range
- Shows frequency distribution

### Box and Whisker

Best for: Statistical distribution, quartile analysis

```typescript
import { Chart, BoxAndWhiskerSeries, Category } from '@syncfusion/ej2-charts';

Chart.Inject(BoxAndWhiskerSeries, Category);

const chart = new Chart({
  primaryXAxis: { valueType: 'Category' },
  series: [{
    dataSource: data,
    xName: 'category',
    yName: 'value',
    type: 'BoxAndWhisker'
  }]
}, '#chart');
```

### Waterfall Chart

Best for: Sequential value changes, cumulative breakdown

```typescript
import { Chart, WaterfallSeries, Category } from '@syncfusion/ej2-charts';

Chart.Inject(WaterfallSeries, Category);

const chart = new Chart({
  primaryXAxis: { valueType: 'Category' },
  series: [{
    dataSource: data,
    xName: 'stage',
    yName: 'amount',
    type: 'Waterfall'
  }]
}, '#chart');
```

**Use cases:**
- Revenue breakdown
- Budget allocation
- Cascade effects
- Financial analysis

### Stock Chart Types

**Candlestick:**
```typescript
import { Chart, CandleSeries, DateTime } from '@syncfusion/ej2-charts';

Chart.Inject(CandleSeries, DateTime);

const chart = new Chart({
  primaryXAxis: { valueType: 'DateTime' },
  series: [{
    dataSource: data,
    xName: 'date',
    low: 'low', high: 'high', open: 'open', close: 'close',
    type: 'Candle'
  }]
}, '#chart');
```

**OHLC (Open-High-Low-Close):**
```typescript
import { Chart, HiloOpenCloseSeries, DateTime } from '@syncfusion/ej2-charts';

Chart.Inject(HiloOpenCloseSeries, DateTime);

const chart = new Chart({
  primaryXAxis: { valueType: 'DateTime' },
  series: [{
    dataSource: data,
    xName: 'date',
    low: 'low', high: 'high', open: 'open', close: 'close',
    type: 'HiloOpenClose'
  }]
}, '#chart');
```

**HiLo (High-Low):**
```typescript
import { Chart, HiloSeries, DateTime } from '@syncfusion/ej2-charts';

Chart.Inject(HiloSeries, DateTime);

const chart = new Chart({
  primaryXAxis: { valueType: 'DateTime' },
  series: [{
    dataSource: data,
    xName: 'date',
    low: 'low', high: 'high',
    type: 'Hilo'
  }]
}, '#chart');
```

## Comparison Matrix

| Chart Type | Best For | Data Points | Series | Use Case |
|-----------|----------|------------|--------|----------|
| Line | Trends | Many | Multiple | Time series, monitoring |
| Area | Cumulative trends | Many | Multiple | Stacked metrics, volume |
| Column | Comparisons | Moderate | Multiple | Sales by category |
| Bar | Rankings | Moderate | Single | Top products, regions |
| Pie/Doughnut | Composition | Few (5-8) | One | Market share, segments |
| Scatter | Correlation | Many | Multiple | Outlier detection |
| Bubble | 3D relationship | Many | Multiple | Population studies |
| Polar/Radar | Multi-dimensional | Moderate | Multiple | Performance metrics |
| Histogram | Distribution | Many | One | Frequency analysis |
| Waterfall | Sequential change | Moderate | One | Financial breakdown |
| Candlestick | Stock data | Many | One | OHLC analysis |
| Funnel | Conversion | Few (5-10) | One | Sales pipeline |

## Implementation Pattern

```typescript
import { Chart, LineSeries, Category } from '@syncfusion/ej2-charts';

// 1. Inject required modules
Chart.Inject(LineSeries, Category);

// 2. Prepare data
const data = [/* array of objects */];

// 3. Create chart with type
const chart = new Chart({
  primaryXAxis: { valueType: 'Category' },
  primaryYAxis: { valueType: 'Double' },
  series: [{
    dataSource: data,
    xName: 'category',
    yName: 'value',
    type: 'Line'  // ← Change type here
  }]
}, '#chart');
```
