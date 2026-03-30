# Styling & Appearance

## Table of Contents
- [Color Schemes](#color-schemes)
- [Fonts & Text](#fonts--text)
- [Borders & Backgrounds](#borders--backgrounds)
- [Chart Area Styling](#chart-area-styling)
- [Responsive Design](#responsive-design)
- [Themes](#themes)

**Note:** Configuration syntax is identical for TypeScript and JavaScript. Examples use object literal format.

**API Reference:** See [api-reference.md](api-reference.md#enumerations) for complete `ChartTheme` and styling enumerations. See [Font](api-reference.md#font), [Border](api-reference.md#border), and [LinearGradient/RadialGradient](api-reference.md#gradients) classes.

## Color Schemes

### Predefined Palettes

```typescript
import { Chart, ColumnSeries, Category } from '@syncfusion/ej2-charts';

Chart.Inject(ColumnSeries, Category);

const chart = new Chart({
  palettes: ['#FF5733', '#33FF57', '#3357FF', '#FF33F1', '#FFFF33'],
  primaryXAxis: { valueType: 'Category' },
  series: [{ dataSource: data, xName: 'x', yName: 'y', type: 'Column' }]
}, '#chart');
```

### Custom Color Per Series

```typescript
import { Chart, ColumnSeries, Category } from '@syncfusion/ej2-charts';

Chart.Inject(ColumnSeries, Category);

const chart = new Chart({
  primaryXAxis: { valueType: 'Category' },
  series: [
    { dataSource: data, xName: 'x', yName: 'y1', type: 'Column', fill: '#FF5733' },
    { dataSource: data, xName: 'x', yName: 'y2', type: 'Column', fill: '#33FF57' }
  ]
}, '#chart');
```

### Gradient Colors

First, define the gradient in SVG (add to your HTML):
```html
<svg style="height: 0">
  <defs>
    <linearGradient id="gradient" x1="0%" y1="0%" x2="100%" y2="0%">
      <stop offset="0%" style="stop-color: #FF5733; stop-opacity: 1" />
      <stop offset="100%" style="stop-color: #FFFF33; stop-opacity: 1" />
    </linearGradient>
  </defs>
</svg>
```

Then reference it in your chart:
```typescript
import { Chart, AreaSeries, Category } from '@syncfusion/ej2-charts';

Chart.Inject(AreaSeries, Category);

const chart = new Chart({
  primaryXAxis: { valueType: 'Category' },
  series: [{
    dataSource: data,
    xName: 'x',
    yName: 'y',
    type: 'Area',
    fill: 'url(#gradient)'
  }]
}, '#chart');
```

### Conditional Coloring (TypeScript)

In TypeScript, use type-safe event handling:

```typescript
import { Chart, ColumnSeries, Category } from '@syncfusion/ej2-charts';
import { IPointRenderEventArgs } from '@syncfusion/ej2-charts';

Chart.Inject(ColumnSeries, Category);

let chart: Chart = new Chart({
    primaryXAxis: {
        valueType: 'Category',
        title: 'Countries'
    },
    primaryYAxis: {
        minimum: 0, 
        maximum: 80,
        interval: 20, 
        title: 'Medals'
    },
    series:[{
        dataSource: data,
        xName: 'month', yName: 'sales',
        // Series type as column series
        type: 'Column'
    }],
    title: 'Olympic Medals',
    pointRender: (args: IPointRenderEventArgs) => {
        if (args.point.y <= 40) {
            args.fill = '#ff6347';
        }
        else {
            args.fill = '#009cb8';
        }
    }
}, '#chart');
```

## Fonts & Text

### Title Font

```typescript
import { Chart, ColumnSeries, Category } from '@syncfusion/ej2-charts';

Chart.Inject(ColumnSeries, Category);

const chart = new Chart({
  title: 'Sales Report',
  titleStyle: {
    fontFamily: 'Arial',
    fontStyle: 'Bold',
    size: '18px',
    color: '#333',
    opacity: 1
  },
  primaryXAxis: { valueType: 'Category' },
  series: [{ dataSource: data, xName: 'x', yName: 'y', type: 'Column' }]
}, '#chart');
```

### Subtitle Font

```typescript
subTitle: 'Q4 2024',
subTitleStyle: {
  fontFamily: 'Arial',
  fontStyle: 'Normal',
  size: '14px',
  color: '#666'
}
```

### Axis Label Font

```typescript
import { Chart, ColumnSeries, Category } from '@syncfusion/ej2-charts';

Chart.Inject(ColumnSeries, Category);

const chart = new Chart({
  primaryXAxis: {
    valueType: 'Category',
    labelStyle: {
      size: '12px',
      color: '#333',
      fontFamily: 'Arial'
    }
  },
  primaryYAxis: {
    labelStyle: {
      size: '12px',
      color: '#333',
      fontFamily: 'Arial'
    }
  },
  series: [{ dataSource: data, xName: 'x', yName: 'y', type: 'Column' }]
}, '#chart');
```

### Axis Title Font

```typescript
import { Chart, ColumnSeries, Category } from '@syncfusion/ej2-charts';

Chart.Inject(ColumnSeries, Category);

const chart = new Chart({
  primaryXAxis: {
    valueType: 'Category',
    title: 'Months',
    titleStyle: {
        size: '14px',
        fontFamily: 'Arial',
        color: 'red'
      }  
  },
  series: [{ dataSource: data, xName: 'x', yName: 'y', type: 'Column' }]
}, '#chart');
```

### Data Label Font

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
        font: {
          size: '12px',
          color: '#000',
          fontFamily: 'Arial'
        }
      }
    }
  }]
}, '#chart');
```

### Legend Font

```typescript
import { Chart, ColumnSeries, Legend, Category } from '@syncfusion/ej2-charts';

Chart.Inject(ColumnSeries, Legend, Category);

const chart = new Chart({
  primaryXAxis: { valueType: 'Category' },
  legendSettings: {
    visible: true,
    textStyle: {
      size: '12px',
      color: '#333',
      fontFamily: 'Arial'
    }
  },
  series: [{ dataSource: data, xName: 'x', yName: 'y', type: 'Column', name: 'Sales' }]
}, '#chart');
```

## Borders & Backgrounds

### Border Styling

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
    border: {
      color: '#333',
      width: 1
    }
  }]
}, '#chart');
```

### Series Background

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
    fill: '#FF5733',
    opacity: 0.7
  }]
}, '#chart');
```

### Marker Border

```typescript
import { Chart, LineSeries, Category } from '@syncfusion/ej2-charts';

Chart.Inject(LineSeries, Category);

const chart = new Chart({
  primaryXAxis: { valueType: 'Category' },
  series: [{
    dataSource: data,
    xName: 'x',
    yName: 'y',
    type: 'Line',
    marker: {
      visible: true,
      border: { color: '#333', width: 2 },
      fill: '#FFF'
    }
  }]
}, '#chart');
```

### Tooltip Border

```typescript
import { Chart, ColumnSeries, Tooltip, Category } from '@syncfusion/ej2-charts';

Chart.Inject(ColumnSeries, Tooltip, Category);

const chart = new Chart({
  primaryXAxis: { valueType: 'Category' },
  tooltip: {
    enable: true,
    border: {
      color: '#333',
      width: 1
    },
    fill: '#FFF'
  },
  series: [{ dataSource: data, xName: 'x', yName: 'y', type: 'Column' }]
}, '#chart');
```

## Chart Area Styling

### Plot Area Styling

```typescript
import { Chart, ColumnSeries, Category } from '@syncfusion/ej2-charts';

Chart.Inject(ColumnSeries, Category);

const chart = new Chart({
  chartArea: {
    background: '#F5F5F5',
    border: { color: '#DDD', width: 1 }
  },
  primaryXAxis: {
    valueType: 'Category',
    majorGridLines: { color: '#E0E0E0', width: 1 }
  },
  primaryYAxis: {
    majorGridLines: { color: '#E0E0E0', width: 1 }
  },
  series: [{ dataSource: data, xName: 'x', yName: 'y', type: 'Column' }]
}, '#chart');
```

### Chart Background Color

```typescript
import { Chart, ColumnSeries, Category } from '@syncfusion/ej2-charts';

Chart.Inject(ColumnSeries, Category);

const chart = new Chart({
  background: '#FFFFFF',
  chartArea: {
    background: '#F9F9F9'
  },
  primaryXAxis: { valueType: 'Category' },
  series: [{ dataSource: data, xName: 'x', yName: 'y', type: 'Column' }]
}, '#chart');
```

## Responsive Design

### Responsive Sizing

```typescript
import { Chart, ColumnSeries, Category } from '@syncfusion/ej2-charts';

Chart.Inject(ColumnSeries, Category);

const chart = new Chart({
  width: '100%',          // Fill container width
  height: '420px',        // Fixed or percentage height
  primaryXAxis: { valueType: 'Category' },
  series: [{ dataSource: data, xName: 'x', yName: 'y', type: 'Column' }]
}, '#chart');
```

### Responsive Font Sizing

```typescript
import { Chart, ColumnSeries, Legend, Category } from '@syncfusion/ej2-charts';
import { Browser } from '@syncfusion/ej2-base';

Chart.Inject(ColumnSeries, Legend, Category);

const fontSize = Browser.isDevice ? '10px' : '24px';

const chart = new Chart({
  primaryXAxis: { valueType: 'Category' },
  titleStyle: {
    size: fontSize
  },
  legendSettings: {
    visible: true,
    textStyle: {
      size: fontSize
    }
  },
  series: [{ dataSource: data, xName: 'x', yName: 'y', type: 'Column', name: 'Sales' }]
}, '#chart');
```

### Mobile-Optimized Chart

```typescript
import { Chart, LineSeries, ColumnSeries, Legend, Category } from '@syncfusion/ej2-charts';
import { Browser } from '@syncfusion/ej2-base';

Chart.Inject(LineSeries, ColumnSeries, Legend, Category);

const isMobile = Browser.isDevice;
const chartType = isMobile ? 'Line' : 'Column';

const chart = new Chart({
  width: '100%',
  height: isMobile ? '300px' : '420px',
  title: 'Sales',
  primaryXAxis: { valueType: 'Category' },
  legendSettings: {
    visible: true,
    position: isMobile ? 'Bottom' : 'Right',
    alignment: 'Center'
  },
  series: [{
    dataSource: data,
    xName: 'x',
    yName: 'y',
    type: chartType,
    name: 'Sales'
  }]
}, '#chart');
```

## Themes

### Apply Theme

```typescript
import { Chart, ColumnSeries, Category } from '@syncfusion/ej2-charts';

Chart.Inject(ColumnSeries, Category);

const chart = new Chart({
  theme: 'Material',
  primaryXAxis: { valueType: 'Category' },
  series: [{ dataSource: data, xName: 'x', yName: 'y', type: 'Column' }]
}, '#chart');
```

**Available themes:**
- `Material` - Google Material Design
- `Fabric` - Microsoft Office Fabric
- `Bootstrap` - Bootstrap framework
- `Highcontrast` - High contrast
- `Fluent` - Microsoft Fluent Design
- `Bootstrap4` - Bootstrap 4

### Theme Switching

```typescript
// Switch theme dynamically
function switchTheme(themeName: string) {
  chart.theme = themeName as any;
  chart.refresh();
}

const themeSelector = document.getElementById('themeSelector') as HTMLSelectElement;
themeSelector.addEventListener('change', (e) => {
  const target = e.target as HTMLSelectElement;
  switchTheme(target.value);
});
```

### Custom Styling

```typescript
import { Chart, ColumnSeries, Category } from '@syncfusion/ej2-charts';

Chart.Inject(ColumnSeries, Category);

const chart = new Chart({
  palettes: ['#FF5733', '#33FF57', '#3357FF'],
  chartArea: { background: '#F9F9F9' },
  titleStyle: {
    fontStyle: 'Bold',
    size: '20px'
  },
  primaryXAxis: { valueType: 'Category' },
  series: [{ dataSource: data, xName: 'x', yName: 'y', type: 'Column' }]
}, '#chart');
```

## Common Configurations

### Configuration 1: Modern Dashboard Style

```typescript
const chart = new Chart({
  theme: 'Fluent',
  palettes: ['#0078D4', '#50E6FF', '#00BCF2', '#1081D1'],
  chartArea: { background: '#FFF' },
  title: 'Sales Dashboard',
  titleStyle: {
    fontFamily: 'Segoe UI',
    size: '18px',
    color: '#333',
    fontStyle: 'Bold'
  },
  primaryXAxis: {
    labelStyle: { color: '#666' }
  },
  primaryYAxis: {
    labelStyle: { color: '#666' },
    majorGridLines: { color: '#E0E0E0' }
  },
  legendSettings: {
    position: 'Right',
    textStyle: { color: '#333' }
  },
  tooltip: {
    enable: true,
    fill: '#FFF',
    border: { color: '#DDD' }
  }
}, '#chart');
```

### Configuration 2: Minimalist Style

```typescript
theme: 'Bootstrap',
palette: ['#333', '#666', '#999'],
chartArea: { background: '#FFF' },
primaryXAxis: {
  majorGridLines: { width: 0 },
  majorTickLines: { width: 0 }
},
primaryYAxis: {
  majorGridLines: { color: '#E0E0E0', width: 1 }
},
series: [{
  dataSource: data,
  type: 'Line',
  xName: 'x',
  yName: 'y',
  width: 2,
  marker: { visible: false }
}]
```

