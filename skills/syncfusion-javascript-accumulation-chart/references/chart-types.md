# Chart Types in Accumulation Charts

## Table of Contents
- [Pie Charts](#pie-charts)
- [Doughnut Charts](#doughnut-charts)
- [Funnel Charts](#funnel-charts)
- [Pyramid Charts](#pyramid-charts)
- [Chart Type Comparison](#chart-type-comparison)

**API Reference:** [AccumulationSeriesModel](https://ej2.syncfusion.com/documentation/api/accumulation-chart/accumulationseriesmodel/)

## Pie Charts

Pie charts display data as a circular graph divided into slices proportional to the values they represent. Ideal for showing percentage composition or parts of a whole.

**API:** [PieSeries](https://ej2.syncfusion.com/documentation/api/accumulation-chart/accumulationseriesmodel/#type)

### Basic Pie Chart

**TypeScript:**
```typescript
import { AccumulationChart, PieSeries } from '@syncfusion/ej2-charts';

AccumulationChart.Inject(PieSeries);

const data = [
  { x: 'Jan', y: 3 },
  { x: 'Feb', y: 3.5 },
  { x: 'Mar', y: 7 },
  { x: 'Apr', y: 13.5 },
  { x: 'May', y: 19 },
  { x: 'Jun', y: 23.5 }
];

const chart = new AccumulationChart({
  series: [{
    dataSource: data,
    xName: 'x',
    yName: 'y',
    type: 'Pie'
  }]
}, '#pie-chart');
```

### Radius Customization

By default, pie radius is 80% of the chart's minimum dimension. Customize using the `radius` property:

**TypeScript:**
```typescript
const chart = new AccumulationChart({
  series: [{
    dataSource: data,
    xName: 'x',
    yName: 'y',
    type: 'Pie',
    radius: '100%'  // Full size or pixel value '150px'
  }]
}, '#chart');
```

**Use Cases:**
- `radius: '80%'` - Standard default size
- `radius: '100%'` - Maximize chart area
- `radius: '120px'` - Fixed pixel size
- `radius: '60%'` - Smaller chart for space constraints

## Doughnut Charts

Doughnut (donut) charts are pie charts with a hollow center. The inner radius creates the "hole," enabling center labels and additional information display.

**API:** [CenterLabelModel](https://ej2.syncfusion.com/documentation/api/accumulation-chart/centerlabelmodel/)

### Basic Doughnut Chart

**TypeScript:**
```typescript
import { AccumulationChart, PieSeries } from '@syncfusion/ej2-charts';

AccumulationChart.Inject(PieSeries);

const chart = new AccumulationChart({
  series: [{
    dataSource: data,
    xName: 'x',
    yName: 'y',
    innerRadius: '40%'  // Creates the hole
  }]
}, '#doughnut-chart');
```

### Inner Radius Values

The `innerRadius` creates the hollow center. Valid values:
- `innerRadius: '0%'` - No hole (converts to pie)
- `innerRadius: '20%'` - Small hole, thick ring
- `innerRadius: '40%'` - Medium hole, standard doughnut
- `innerRadius: '60%'` - Large hole, thin ring
- `innerRadius: '80%'` - Extra large hole (rarely used)

### Center Label for Doughnut

Add text or metrics in the center of doughnut:

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
    text: 'Total Sales',
    hoverTextFormat: '${point.x}: ${point.y}%'
  }
}, '#chart');
```

## Funnel Charts

Funnel charts display data in a funnel shape, showing progression through stages. Typically used for sales pipelines, conversion funnels, or hierarchical data flow.

**API:** [FunnelSeries](https://ej2.syncfusion.com/documentation/api/accumulation-chart/accumulationseriesmodel/#type)

### Basic Funnel Chart

**TypeScript:**
```typescript
import { AccumulationChart, FunnelSeries } from '@syncfusion/ej2-charts';

AccumulationChart.Inject(FunnelSeries);

const stageData = [
  { x: 'Awareness', y: 10000 },
  { x: 'Consideration', y: 8000 },
  { x: 'Evaluation', y: 6000 },
  { x: 'Purchase', y: 4000 },
  { x: 'Loyalty', y: 3000 }
];

const chart = new AccumulationChart({
  series: [{
    dataSource: stageData,
    xName: 'x',
    yName: 'y',
    type: 'Funnel'
  }]
}, '#funnel-chart');
```

### Funnel Customization

**Neck Width:**
```typescript
series: [{
  type: 'Funnel',
  neckWidth: '15%'  // Width of bottom segment (0-100%)
}]
```

**Gutter Spacing:**
```typescript
series: [{
  type: 'Funnel',
  gapRatio: 0.1  // Space between segments (0-1)
}]
```

## Pyramid Charts

Pyramid charts display hierarchical data in a pyramid shape. Useful for organizational hierarchies, population distribution, or data hierarchy visualization.

**API:** [PyramidSeries](https://ej2.syncfusion.com/documentation/api/accumulation-chart/accumulationseriesmodel/#type)

### Basic Pyramid Chart

**TypeScript:**
```typescript
import { AccumulationChart, PyramidSeries } from '@syncfusion/ej2-charts';

AccumulationChart.Inject(PyramidSeries);

const hierarchyData = [
  { x: 'Management', y: 2 },
  { x: 'Senior Staff', y: 8 },
  { x: 'Mid-Level', y: 20 },
  { x: 'Junior Staff', y: 45 },
  { x: 'Interns', y: 15 }
];

const chart = new AccumulationChart({
  series: [{
    dataSource: hierarchyData,
    xName: 'x',
    yName: 'y',
    type: 'Pyramid'
  }]
}, '#pyramid-chart');
```

### Pyramid Mode

Control how pyramid segments are sized:

**TypeScript:**
```typescript
series: [{
  type: 'Pyramid',
  pyramidMode: 'Linear'  // Linear or Surface
}]
```

- `Linear` - All segments same height, width proportional to value
- `Surface` - Both height and width proportional to value

## Chart Type Comparison

| Feature | Pie | Doughnut | Funnel | Pyramid |
|---------|-----|----------|--------|---------|
| **Best For** | Composition (parts of whole) | Composition with center info | Stages/flow/conversion | Hierarchy/levels |
| **Inner Hole** | No | Yes (40-60% typical) | No | No |
| **Center Label** | No | Yes | No | No |
| **Data Flow** | Circular | Circular | Top-to-bottom | Top-to-bottom |
| **Common Use** | % Distribution | KPI dashboard | Sales pipeline | Organization chart |
| **Segments** | Unlimited | Unlimited | 5-10 typical | 4-7 typical |
| **Radius Control** | Via `radius` | Via `innerRadius` | Via `neckWidth` | Via `pyramidMode` |

## Choosing Your Chart Type

**Use Pie when:**
- Showing percentage distribution
- Total adds up to 100%
- Simple, easy-to-understand composition
- Limited number of categories (3-5)

**Use Doughnut when:**
- Pie chart, but need center space for KPI/metric
- Want professional dashboard appearance
- Need center label for additional context

**Use Funnel when:**
- Showing conversion or drop-off rates
- Displaying stages in a process
- Sales pipeline or user journey
- Data naturally decreases through stages

**Use Pyramid when:**
- Showing organizational hierarchy
- Population distribution by levels
- Team structure visualization
- Data organized in hierarchical levels

## Examples in Context

### Sales Dashboard
```typescript
// Pie: Product mix by revenue
// Doughnut: Total revenue in center
// Funnel: Sales pipeline conversion
// Pyramid: Team hierarchy

const salesData = [
  { category: 'Product A', sales: 25000 },
  { category: 'Product B', sales: 18000 },
  { category: 'Product C', sales: 12000 }
];

const pipelineData = [
  { stage: 'Leads', count: 1000 },
  { stage: 'Qualified', count: 500 },
  { stage: 'Proposal', count: 200 },
  { stage: 'Closed', count: 80 }
];
```

### Multi-Chart Approach
Consider combining multiple chart types in a dashboard:
- **Pie** for product breakdown
- **Funnel** for sales pipeline
- **Doughnut** for total metrics
- **Pyramid** for team structure
