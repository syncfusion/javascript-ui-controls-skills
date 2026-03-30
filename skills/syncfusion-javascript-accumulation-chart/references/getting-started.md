# Getting Started with Accumulation Charts

## Table of Contents

- [Installation & Setup](#installation--setup)
  - [TypeScript Setup](#typescript-setup)
- [Module Injection Pattern](#module-injection-pattern)
- [Creating Your First Pie Chart](#creating-your-first-pie-chart)
- [Theme Selection](#theme-selection)
- [Basic Data Binding](#basic-data-binding)
- [Adding Legend](#adding-legend)
- [Enabling Tooltips](#enabling-tooltips)
- [Common Configuration Issues](#common-configuration-issues)
- [API Reference](#api-reference)
- [Next Steps](#next-steps)

## Installation & Setup

### TypeScript Setup

Install the required Syncfusion packages via npm:

```bash
npm install @syncfusion/ej2-charts --save
```

**Key dependencies:**
```
@syncfusion/ej2-charts
  ├── @syncfusion/ej2-base
  ├── @syncfusion/ej2-data
  ├── @syncfusion/ej2-pdf-export
  ├── @syncfusion/ej2-file-utils
  ├── @syncfusion/ej2-compression
  └── @syncfusion/ej2-svg-base
```

Import required modules in your TypeScript file:

```typescript
import { AccumulationChart, PieSeries, AccumulationDataLabel, AccumulationLegend, Tooltip } from '@syncfusion/ej2-charts';
```

## Module Injection Pattern

Accumulation charts use a modular architecture. You must inject the chart types and features you plan to use.

### TypeScript Module Injection

```typescript
// Inject modules for features you'll use
AccumulationChart.Inject(
  PieSeries,                      // For Pie charts
  AccumulationDataLabel,          // For data labels
  AccumulationLegend,             // For legends
  Tooltip,                        // For tooltips
  AccumulationSelection           // For selection
);
```

## Creating Your First Pie Chart

### TypeScript Example

```typescript
import { AccumulationChart, PieSeries, AccumulationDataLabel } from '@syncfusion/ej2-charts';

AccumulationChart.Inject(PieSeries, AccumulationDataLabel);

// Sample data
const salesData = [
  { x: 'Q1', y: 25000, text: 'Q1: $25K' },
  { x: 'Q2', y: 30000, text: 'Q2: $30K' },
  { x: 'Q3', y: 28000, text: 'Q3: $28K' },
  { x: 'Q4', y: 32000, text: 'Q4: $32K' }
];

// Create chart
const chart = new AccumulationChart({
  // Data source
  series: [{
    dataSource: salesData,
    xName: 'x',      // Category field
    yName: 'y',      // Value field
    type: 'Pie'      // Chart type
  }],
  // Chart title
  title: 'Quarterly Sales Distribution',
  // Container element ID
  height: '400px',
  width: '100%'
}, '#chart-container');
```

**HTML:**
```html
<div id="chart-container"></div>
```

## Theme Selection

Apply predefined themes to your chart:

### TypeScript

```typescript
const chart = new AccumulationChart({
  series: [{ dataSource: data, xName: 'x', yName: 'y', type: 'Pie' }],
  theme: 'Bootstrap5'  // Material, Bootstrap, Fabric, HighContrast, Fluent, Bootstrap5
}, '#chart');
```

## Basic Data Binding

Data binding requires a data source (array of objects) with:
1. **x-field** - Category or label
2. **y-field** - Numeric value for sizing

```typescript
const data = [
  { category: 'Product A', revenue: 15000 },
  { category: 'Product B', revenue: 22000 },
  { category: 'Product C', revenue: 18000 },
  { category: 'Product D', revenue: 25000 }
];

const chart = new AccumulationChart({
  series: [{
    dataSource: data,
    xName: 'category',    // Maps to x-axis labels
    yName: 'revenue',     // Maps to segment sizes
    type: 'Pie'
  }]
}, '#chart');
```

## Adding Legend

Legends help identify data categories:

```typescript
const chart = new AccumulationChart({
  series: [{ dataSource: data, xName: 'x', yName: 'y', type: 'Pie' }],
  legendSettings: {
    visible: true,
    position: 'Right'  // Right, Left, Top, Bottom
  }
}, '#chart');
```

## Enabling Tooltips

Tooltips show detailed information on hover:

```typescript
const chart = new AccumulationChart({
  series: [{ 
    dataSource: data, 
    xName: 'x', 
    yName: 'y', 
    type: 'Pie' 
  }],
  tooltip: {
    enable: true,
    format: '${point.x}: <b>${point.y}</b>'
  }
}, '#chart');
```

## Common Configuration Issues

**Issue: Chart not rendering**
- Solution: Verify the container element has an ID matching your chart initialization
- Ensure CSS is loaded (theme CSS is required)

**Issue: "AccumulationChart is not defined"**
- Solution: Check module injection - you must inject required modules before instantiation
- For CDN, verify all scripts are loaded in correct order

**Issue: No data appearing**
- Solution: Verify data has correct xName and yName fields
- Check that y-values are numeric (not strings)
- Ensure data format matches expected structure

## API Reference

For complete API documentation, see:
- **AccumulationChart API:** [API Reference](api-reference.md)
- **Core Properties:** [AccumulationChart Properties](https://ej2.syncfusion.com/documentation/api/accumulation-chart/)
- **Series Configuration:** [AccumulationSeriesModel](https://ej2.syncfusion.com/documentation/api/accumulation-chart/accumulationseriesmodel/)
- **Module Injection:** [Inject() Method](https://ej2.syncfusion.com/documentation/api/accumulation-chart/#inject)

