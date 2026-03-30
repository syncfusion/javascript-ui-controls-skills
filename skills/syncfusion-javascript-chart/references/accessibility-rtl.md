# Accessibility & RTL Support

## Table of Contents
- [Core Accessibility Configuration](#core-accessibility-configuration)
- [Series Accessibility](#series-accessibility)
- [Title & Subtitle Accessibility](#title--subtitle-accessibility)
- [Annotations Accessibility](#annotations-accessibility)
- [Trendlines Accessibility](#trendlines-accessibility)
- [Zoom Settings Accessibility](#zoom-settings-accessibility)
- [Technical Indicators Accessibility](#technical-indicators-accessibility)
- [Legend Accessibility](#legend-accessibility)
- [Keyboard Navigation](#keyboard-navigation)
- [RTL Language Support](#rtl-language-support)

**Note:** Accessibility configuration is identical for TypeScript and JavaScript.

**API Reference:** See the [Syncfusion Documentation](https://ej2.syncfusion.com/documentation/chart/advanced-accessibility-configuration) for comprehensive API details.

## Core Accessibility Configuration

The Chart control exposes the following accessibility attributes:

- **accessibilityDescription** - Provides a text description for the chart, improving support for screen readers
- **accessibilityRole** - Specifies the role of the chart, helping screen readers identify the element appropriately
- **focusable** - Allows the chart to receive focus, making it accessible via keyboard navigation
- **focusBorderColor** - Sets the color of the focus border, enhancing visibility when the chart is focused
- **focusBorderMargin** - Defines the margin around the focus border
- **focusBorderWidth** - Specifies the width of the focus border
- **tabIndex** - Specifies the tab order of the chart element, enabling efficient keyboard navigation

### Example: Fully Accessible Chart

```typescript
import { Chart, LineSeries, Tooltip, Legend, Category, DataLabel } from '@syncfusion/ej2-charts';

Chart.Inject(LineSeries, Tooltip, Legend, Category, DataLabel);

let chartData: Object[] = [
    { month: 'Jan', sales: 35 }, { month: 'Feb', sales: 28 },
    { month: 'Mar', sales: 34 }, { month: 'Apr', sales: 32 },
    { month: 'May', sales: 40 }, { month: 'Jun', sales: 32 },
    { month: 'Jul', sales: 35 }, { month: 'Aug', sales: 55 },
    { month: 'Sep', sales: 38 }, { month: 'Oct', sales: 30 },
    { month: 'Nov', sales: 25 }, { month: 'Dec', sales: 32 }
];

let chart: Chart = new Chart({
    primaryXAxis: {
        valueType: 'Category'
    },
    primaryYAxis: {
        labelFormat: '${value}K'
    },
    series: [
        {
            dataSource: chartData,
            name: 'Sales',
            xName: 'month',
            yName: 'sales',
            type: 'Line',
            marker: {
                visible: true,
                dataLabel: {
                    visible: true
                }
            }
        }
    ],
    tooltip: { enable: true },
    legendSettings: { visible: true },
    title: 'Sales Analysis',
    accessibility: {
        accessibilityDescription: 'A line chart displaying the sales analysis for each month.',
        accessibilityRole: 'chart'
    },
    focusBorderColor: '#FF0000',
    focusBorderWidth: 3,
    focusBorderMargin: 5
}, '#element');
```

## Series Accessibility

The series accessibility feature allows customization of accessibility for data points:

- **accessibilityDescription** - Provides a text description for the series root element
- **accessibilityDescriptionFormat** - Specifies a format for accessibility descriptions of each point (supports placeholders like `${series.name}`, `${point.x}`, `${point.y}`, etc.)
- **accessibilityRole** - Specifies the role of the series
- **focusable** - Allows the series to receive focus via keyboard navigation
- **tabIndex** - Specifies the tab order of the series element

### Example: Series with Accessibility

```typescript
import { Chart, ColumnSeries, Category } from '@syncfusion/ej2-charts';

Chart.Inject(ColumnSeries, Category);

let columnData: Object[] = [
    { country: "USA",       gold: 50 },
    { country: "China",     gold: 40 },
    { country: "Japan",     gold: 70 },
    { country: "Australia", gold: 60 },
    { country: "France",    gold: 50 },
    { country: "Germany",   gold: 40 },
    { country: "Italy",     gold: 40 },
    { country: "Sweden",    gold: 30 }
];

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
    series: [{
        dataSource: columnData,
        xName: 'country', 
        yName: 'gold',
        type: 'Column',
        accessibility: {
            accessibilityDescription: 'This series displays the number of gold medals won by each country in the Olympics.',
            accessibilityRole: 'series',
            accessibilityDescriptionFormat: 'The country ${point.x} won ${point.y} gold medals.'
        }
    }],
    title: 'Olympic Medals'
}, '#element');
```

## Title & Subtitle Accessibility

Customize the accessibility of chart titles and subtitles using the `titleStyle` and `subTitleStyle` properties with an `accessibility` object:

- **accessibilityDescription** - Text description for the title/subtitle
- **accessibilityRole** - Role for the title/subtitle
- **focusable** - Enables keyboard focus for the title/subtitle
- **tabIndex** - Tab order of the title/subtitle

### Example: Title and Subtitle with Accessibility

```typescript
import { Chart, ColumnSeries, Category, Legend } from '@syncfusion/ej2-charts';

Chart.Inject(ColumnSeries, Category, Legend);

let columnData: Object[] = [
    { country: "USA",       gold: 50, silver: 70, bronze: 45 },
    { country: "China",     gold: 40, silver: 60, bronze: 55 },
    { country: "Japan",     gold: 70, silver: 60, bronze: 50 },
    { country: "Australia", gold: 60, silver: 56, bronze: 40 },
    { country: "France",    gold: 50, silver: 45, bronze: 35 },
    { country: "Germany",   gold: 40, silver: 30, bronze: 22 },
    { country: "Italy",     gold: 40, silver: 35, bronze: 37 },
    { country: "Sweden",    gold: 30, silver: 25, bronze: 27 }
];

let chart: Chart = new Chart({
    primaryXAxis: {
        valueType: 'Category',
        title: 'Countries',
        labelPlacement: 'OnTicks'
    },
    primaryYAxis: {
        minimum: 0, 
        maximum: 80,
        interval: 20, 
        title: 'Medals'
    },
    series: [
        { dataSource: columnData, xName: 'country', yName: 'gold', name: 'Gold', type: 'Column' },
        { dataSource: columnData, xName: 'country', yName: 'silver', name: 'Silver', type: 'Column' },
        { dataSource: columnData, xName: 'country', yName: 'bronze', name: 'Bronze', type: 'Column' }
    ],
    title: 'Olympic Medals Comparison by Country',
    titleStyle: {
        accessibility: {
            accessibilityDescription: 'This chart shows the number of gold, silver, and bronze medals won by different countries in the Olympics.',
            accessibilityRole: 'heading'
        }
    },
    subTitle: 'Medal Comparison',
    subTitleStyle: {
        accessibility: {
            accessibilityDescription: 'The subtitle provides additional context for the Olympic medal distribution chart.',
            accessibilityRole: 'heading'
        }
    }
}, '#element');
```

## Annotations Accessibility

Add accessibility to chart annotations using the `accessibility` property:

- **accessibilityDescription** - Text description for the annotation
- **accessibilityRole** - Role of the annotation
- **focusable** - Enables keyboard focus for the annotation
- **tabIndex** - Tab order of the annotation

### Example: Annotations with Accessibility

```typescript
import { Chart, ColumnSeries, Category, ChartAnnotation } from '@syncfusion/ej2-charts';

Chart.Inject(ColumnSeries, Category, ChartAnnotation);

let columnData: Object[] = [
    { country: "USA",       gold: 50 }, 
    { country: "China",     gold: 40 }, 
    { country: "Japan",     gold: 70 },
    { country: "Australia", gold: 60 }, 
    { country: "France",    gold: 50 }, 
    { country: "Germany",   gold: 40 },
    { country: "Italy",     gold: 40 }, 
    { country: "Sweden",    gold: 30 }
];

let chart: Chart = new Chart({
    primaryXAxis: {
        valueType: 'Category',
        title: 'Countries'
    },
    primaryYAxis: {
        title: 'Medals'
    },
    annotations: [{
        content: '<div style="border: 1px solid #000; background-color: #f8f8f8; padding: 5px; border-radius: 4px; font-size: 12px; font-weight: bold;">70 Gold Medals</div>',
        coordinateUnits: 'Point',
        x: 'France',
        y: 55,
        accessibility: {
            accessibilityDescription: 'Annotation indicating that France has won 70 Gold Medals.',
            accessibilityRole: 'note',
            focusable: true
        }
    }],
    series: [{
        dataSource: columnData,
        xName: 'country', 
        yName: 'gold',
        type: 'Column'
    }],
    title: 'Olympic Medals'
}, '#element');
```

## Trendlines Accessibility

Configure accessibility for trendlines using the `accessibility` property:

- **accessibilityDescription** - Text description for the trendline
- **accessibilityRole** - Role of the trendline
- **focusable** - Enables keyboard focus for the trendline
- **tabIndex** - Tab order of the trendline

### Example: Trendline with Accessibility

```typescript
import { Chart, Trendlines, ScatterSeries, Tooltip, Legend } from '@syncfusion/ej2-charts';

Chart.Inject(Trendlines, ScatterSeries, Tooltip, Legend);

let data: Object[] = [
    { x: 1, y: 7.66 }, { x: 2, y: 8.03 }, { x: 3, y: 8.41 },
    { x: 4, y: 8.97 }, { x: 5, y: 8.77 }, { x: 6, y: 8.20 }
];

let chart: Chart = new Chart({
    primaryXAxis: {
        title: 'Time Period'
    },
    primaryYAxis: {
        title: 'Value',
        interval: 5
    },
    series: [{
        dataSource: data,
        xName: 'x', 
        yName: 'y',
        name: 'Data Series',
        fill: '#0066FF',
        type: 'Scatter',
        trendlines: [{
            type: 'Linear',
            accessibility: {
                accessibilityDescription: 'A linear trendline representing the general trend of the data series.',
                accessibilityRole: 'line'
            }
        }]
    }],
    tooltip: { enable: true },
    chartArea: { border: { width: 0 } },
    title: 'Trend Analysis',
    legendSettings: { visible: false }
}, '#element');
```

## Zoom Settings Accessibility

Configure accessibility for chart zooming using the `zoomSettings` property:

- **accessibilityDescription** - Text description for the zoom toolkit items
- **accessibilityRole** - Role of the zoom toolkit items
- **focusable** - Enables keyboard focus for zoom items
- **tabIndex** - Tab order of the zoom element

### Example: Zoom with Accessibility

```typescript
import { Chart, AreaSeries, Legend, Zoom, DateTime } from '@syncfusion/ej2-charts';

Chart.Inject(AreaSeries, Legend, Zoom, DateTime);

let series1: Object[] = [];
let value: number = 40;
for (let i = 1; i < 100; i++) {
    if (Math.random() > 0.5) {
        value += Math.random();
    } else {
        value -= Math.random();
    }
    series1.push({ x: new Date(2023, 0, i), y: value.toFixed(1) });
}

let chart: Chart = new Chart({
    primaryXAxis: {
        valueType: 'DateTime'
    },
    series: [{
        type: 'Area',
        dataSource: series1,
        name: 'Product X',
        xName: 'x',
        yName: 'y',
        border: { width: 0.5, color: '#00bdae' },
        animation: { enable: false }
    }],
    zoomSettings: {
        enableMouseWheelZooming: true,
        enablePinchZooming: true,
        enableSelectionZooming: true,
        accessibility: {
            accessibilityDescription: 'This allows users to zoom in and out of the chart using mouse wheel, pinch gestures, or selection box.',
            accessibilityRole: 'zoom'
        }
    },
    title: 'Sales History of Product X',
    legendSettings: { visible: false }
}, '#element');
```

## Technical Indicators Accessibility

Configure accessibility for technical indicators using the `accessibility` property:

- **accessibilityDescription** - Text description for the indicators
- **accessibilityRole** - Role of the indicators
- **focusable** - Enables keyboard focus for indicators
- **tabIndex** - Tab order of the indicators

### Example: Technical Indicator with Accessibility

Observe this [Candle series](https://ej2.syncfusion.com/documentation/chart/technical-indicators) sample for how technical indicators rendered.

```typescript
indicators: [{
    type: 'AccumulationDistribution',
    field: 'Close',
    seriesName: 'Apple Inc',
    fill: 'blue',
    period: 3,
    animation: { enable: true },
    yAxisName: 'secondary',
    accessibility: {
        accessibilityDescription: 'The Accumulation Distribution indicator is used to assess the buying and selling pressure of Apple Inc. stock.',
        accessibilityRole: 'indicator'
    }
}],
```

## Legend Accessibility

Configure accessibility for legends using the `accessibility` property in `legendSettings`:

- **accessibilityDescription** - Text description for the legend
- **accessibilityRole** - Role of the legend
- **focusable** - Enables keyboard focus for legend items
- **tabIndex** - Tab order of the legend

### Example: Legend with Accessibility

```typescript
import { Chart, ColumnSeries, Category, Legend } from '@syncfusion/ej2-charts';

Chart.Inject(ColumnSeries, Category, Legend);

let chartData: Object[] = [
    { country: "USA",       gold: 50, silver: 70, bronze: 45 },
    { country: "China",     gold: 40, silver: 60, bronze: 55 },
    { country: "Japan",     gold: 70, silver: 60, bronze: 50 },
    { country: "Australia", gold: 60, silver: 56, bronze: 40 },
    { country: "France",    gold: 50, silver: 45, bronze: 35 },
    { country: "Germany",   gold: 40, silver: 30, bronze: 22 },
    { country: "Italy",     gold: 40, silver: 35, bronze: 37 },
    { country: "Sweden",    gold: 30, silver: 25, bronze: 27 }
];

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
    series: [
        { dataSource: chartData, xName: 'country', yName: 'gold', name: 'Gold', type: 'Column' },
        { dataSource: chartData, xName: 'country', yName: 'silver', name: 'Silver', type: 'Column' },
        { dataSource: chartData, xName: 'country', yName: 'bronze', name: 'Bronze', type: 'Column' }
    ],
    title: 'Olympic Medals',
    legendSettings: {
        visible: true,
        accessibility: {
            accessibilityDescription: 'Legend displaying medal counts by country for Gold, Silver, and Bronze.',
            accessibilityRole: 'presentation'
        }
    }
}, '#element');
```

## Keyboard Navigation

The Chart control supports keyboard navigation through the `focusable` property set to `true`. Users can:

- Navigate between chart elements using Tab/Shift+Tab
- Interact with chart elements when focused
- Access chart features via keyboard shortcuts

### Example: Setting Focus

```typescript
let chart: Chart = new Chart({
    // ... chart configuration
    accessibility: {
        focusable: true
    }
}, '#element');
```

## RTL Language Support

The Chart control supports Right-to-Left (RTL) languages through the `enableRtl` property:

### Enable RTL Mode

```typescript
let chart: Chart = new Chart({
    enableRtl: true,
    title: 'مبيعات',
    primaryXAxis: {
        title: { text: 'الأشهر' },
        categories: ['يناير', 'فبراير', 'مارس', 'أبريل']
    },
    primaryYAxis: {
        title: { text: 'الإيرادات' }
    },
    legendSettings: {
        visible: true,
        position: 'Left'  // Left in RTL
    },
    series: [{
        dataSource: arabicData,
        xName: 'month',
        yName: 'revenue',
        type: 'Line'
    }]
}, '#element');
```

### HTML Setup for RTL

```html
<!-- Set HTML direction -->
<html dir="rtl" lang="ar">
<head>
    <meta charset="utf-8">
    <title>Chart RTL Example</title>
</head>
<body>
    <div id="element"></div>
</body>
</html>
```

## Best Practices Checklist

- [x] Use `accessibilityDescription` for chart, series, and other elements
- [x] Set `accessibilityRole` appropriately (e.g., 'chart', 'series', 'legend')
- [x] Configure `focusable` properties to enable keyboard navigation
- [x] Use `focusBorderColor`, `focusBorderWidth`, and `focusBorderMargin` for visibility
- [x] Set `tabIndex` for proper tab order
- [x] Use `accessibilityDescriptionFormat` in series for dynamic descriptions
- [x] Enable tooltips for additional context
- [x] Provide clear titles and axis labels
- [x] Use high contrast colors (4.5:1 minimum for WCAG AA compliance)
- [x] Support RTL languages when needed using `enableRtl`

## API Reference

For detailed API documentation, refer to the [Syncfusion EJ2 Chart Accessibility Documentation](https://ej2.syncfusion.com/documentation/chart/advanced-accessibility-configuration).

