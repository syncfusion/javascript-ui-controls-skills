# Accessibility & Advanced Features

## Table of Contents

- [Advanced Accessibility Configuration](#advanced-accessibility-configuration)
  - [Chart-Level Accessibility](#chart-level-accessibility)
  - [Series Accessibility](#series-accessibility)
  - [Legend Accessibility](#legend-accessibility)
- [Keyboard Navigation](#keyboard-navigation)
  - [Enabling Keyboard Navigation](#enabling-keyboard-navigation)
  - [Complete Accessible Chart Example](#complete-accessible-chart-example)
- [Focus Border Configuration](#focus-border-configuration)
- [Screen Reader Support](#screen-reader-support)
  - [Data Label for Screen Readers](#data-label-for-screen-readers)
  - [Announce Chart Changes](#announce-chart-changes)
- [Dynamic Data Updates](#dynamic-data-updates)
  - [Update Data Source](#update-data-source)
  - [Real-Time Streaming Example](#real-time-streaming-example)
- [Animation & Transitions](#animation--transitions)
  - [Animation Settings](#animation-settings)
  - [Animation Events](#animation-events)
- [Event Handling](#event-handling)
  - [Common Events](#common-events)
  - [Point Click Event](#point-click-event)
- [Performance Optimization](#performance-optimization)
  - [Large Dataset Configuration](#large-dataset-configuration)
  - [Group Small Values](#group-small-values)
- [Troubleshooting](#troubleshooting)
- [Advanced Patterns](#advanced-patterns)
  - [Pattern 1: Dashboard with Multiple Charts](#pattern-1-dashboard-with-multiple-charts)
  - [Pattern 2: Drill-Down Navigation](#pattern-2-drill-down-navigation)
  - [Pattern 3: Conditional Formatting](#pattern-3-conditional-formatting)
- [API Reference Links](#api-reference-links)
- [Next Steps](#next-steps)

**API Reference:** [AccessibilityModel](https://ej2.syncfusion.com/documentation/api/accumulation-chart/accessibilityModel/) | [IAccLoadedEventArgs](https://ej2.syncfusion.com/documentation/api/accumulation-chart/iaccloadedeventargs/) | [Accumulation Chart API](https://ej2.syncfusion.com/documentation/api/accumulation-chart/)

## Advanced Accessibility Configuration

Accumulation charts support WCAG 2.2 standards and comprehensive accessibility customization options. The control provides robust customization options for accessibility, allowing you to enhance the user experience for those with disabilities.

### Chart-Level Accessibility

The accumulation chart control supports the following accessibility properties:

- **accessibilityDescription** - Provides a text description for the accumulation chart, improving support for screen readers.
- **accessibilityRole** - Specifies the role of the accumulation chart, helping screen readers to identify the element appropriately.
- **focusable** - Allows the accumulation chart to receive focus, making it accessible via keyboard navigation.
- **focusBorderColor** - Sets the color of the focus border, enhancing visibility when the accumulation chart is focused.
- **focusBorderMargin** - Defines the margin around the focus border.
- **focusBorderWidth** - Specifies the width of the focus border.
- **tabIndex** - Specifies the tab order for the accumulation chart element, enabling efficient keyboard navigation.

**Example: Chart-Level Accessibility**
```typescript
import { AccumulationChart, PieSeries, AccumulationLegend } from '@syncfusion/ej2-charts';

AccumulationChart.Inject(PieSeries, AccumulationLegend);

const chart = new AccumulationChart({
  series: [{
    dataSource: [
      { x: 'Jan', y: 3 },  { x: 'Feb', y: 3.5 }, 
      { x: 'Mar', y: 7 },  { x: 'Apr', y: 13.5 }, 
      { x: 'May', y: 19 }, { x: 'Jun', y: 23.5 }, 
      { x: 'Jul', y: 26 }, { x: 'Aug', y: 25 },
      { x: 'Sep', y: 21 }, { x: 'Oct', y: 15 },
      { x: 'Nov', y: 15 }, { x: 'Dec', y: 15 }
    ],
    xName: 'x',
    yName: 'y'
  }],
  legendSettings: { visible: true },
  accessibility: {
    accessibilityDescription: 'Pie chart representing the distribution of data across months.',
    accessibilityRole: 'chart'
  },
  focusBorderColor: '#FF0000',
  focusBorderWidth: 3,
  focusBorderMargin: 5
}, '#element');
```

### Series Accessibility

The series supports customization of accessibility for data points with the following properties:

- **accessibilityDescription** - Provides a text description for the series root element, enhancing support for screen readers.
- **accessibilityRole** - Specifies the role of the series, helping screen readers to identify the element appropriately.
- **focusable** - Allows the series to receive focus, making it accessible via keyboard navigation.
- **tabIndex** - Specifies the tab order of the series element, enabling efficient keyboard navigation.

**Example: Series Accessibility**
```typescript
import { AccumulationChart, PyramidSeries, AccumulationLegend } from '@syncfusion/ej2-charts';

AccumulationChart.Inject(PyramidSeries, AccumulationLegend);

const chart = new AccumulationChart({
  series: [{
    type: 'Pyramid',
    dataSource: [
      { x: 'Australia', y: 20 },
      { x: 'France',    y: 22 },
      { x: 'China',     y: 23 },
      { x: 'India',     y: 24 },
      { x: 'Japan',     y: 25 },
      { x: 'Germany',   y: 27 }
    ],
    xName: 'x', 
    yName: 'y',
    accessibility: {
      accessibilityDescription: 'This pyramid chart represents the sales distribution of cars by region, with each section representing a different region and its respective sales percentage.',
      accessibilityRole: 'presentation'
    }
  }],
  title: 'Sales Distribution of Car by Region',
  legendSettings: { visible: false }
}, '#element');
```

### Legend Accessibility

The legend settings provide information about the series shown in the accumulation chart. The accessibility properties in `legendSettings` can be used to alter the accessibility of the chart's legend:

- **accessibilityDescription** - Provides a text description for the legend root element, enhancing support for screen readers.
- **accessibilityRole** - Specifies the role of the legend items to screen readers, providing appropriate context.
- **focusable** - Specifies whether legend items are focusable via keyboard navigation.
- **tabIndex** - Specifies the tab order of the legend element, enabling efficient keyboard navigation.

**Example: Legend Accessibility**
```typescript
import { AccumulationChart, PieSeries, AccumulationLegend } from '@syncfusion/ej2-charts';

AccumulationChart.Inject(PieSeries, AccumulationLegend);

const chart = new AccumulationChart({
  series: [{
    dataSource: [
      { x: 'Jan', y: 3 },  { x: 'Feb', y: 3.5 }, 
      { x: 'Mar', y: 7 },  { x: 'Apr', y: 13.5 }, 
      { x: 'May', y: 19 }, { x: 'Jun', y: 23.5 }, 
      { x: 'Jul', y: 26 }, { x: 'Aug', y: 25 },
      { x: 'Sep', y: 21 }, { x: 'Oct', y: 15 },
      { x: 'Nov', y: 15 }, { x: 'Dec', y: 15 }
    ],
    xName: 'x',
    yName: 'y'
  }],
  legendSettings: {
    visible: true,
    accessibility: {
      accessibilityDescription: 'The legend identifies the data categories represented in the chart and associates them with their corresponding labels.',
      accessibilityRole: 'group'
    }
  }
}, '#element');
```

## Keyboard Navigation

Users can navigate chart elements using keyboard interactions. The chart is keyboard accessible by default when `focusable` is set to true.

### Enabling Keyboard Navigation

**TypeScript:**
```typescript
import { AccumulationChart, PieSeries } from '@syncfusion/ej2-charts';

AccumulationChart.Inject(PieSeries);

const chart = new AccumulationChart({
  series: [{ 
    dataSource: [
      { x: 'Product A', y: 25000 },
      { x: 'Product B', y: 30000 },
      { x: 'Product C', y: 20000 }
    ], 
    xName: 'x', 
    yName: 'y', 
    type: 'Pie' 
  }],
  // Chart is keyboard accessible when focused
  accessibility: {
    focusable: true
  },
  tabIndex: 0  // Enable tabbing to chart
}, '#chart');
```

### Complete Accessible Chart Example

**TypeScript:**
```typescript
import { AccumulationChart, PieSeries, AccumulationLegend } from '@syncfusion/ej2-charts';

AccumulationChart.Inject(PieSeries, AccumulationLegend);

const chart = new AccumulationChart({
  series: [{
    dataSource: [
      { x: 'Product A', y: 25000 },
      { x: 'Product B', y: 30000 },
      { x: 'Product C', y: 20000 }
    ],
    xName: 'x',
    yName: 'y',
    type: 'Pie',
    dataLabel: {
      visible: true
    },
    accessibility: {
      accessibilityDescription: 'Product sales distribution pie chart',
      accessibilityRole: 'presentation'
    }
  }],
  
  title: 'Product Sales Distribution',
  accessibility: {
    accessibilityDescription: 'Pie chart showing product sales by category',
    accessibilityRole: 'chart'
  },
  
  tooltip: { enable: true },
  legendSettings: { 
    visible: true,
    accessibility: {
      accessibilityDescription: 'Legend for product categories',
      accessibilityRole: 'group'
    }
  },
  
  tabIndex: 0,
  focusBorderColor: '#0078D4',
  focusBorderWidth: 2,
  focusBorderMargin: 3,
  
  // Accessibility events
  loaded: (args) => {
    console.log('Chart accessible and ready');
  }
}, '#chart');
```

## Focus Border Configuration

The focus border provides visual feedback when the chart element receives keyboard focus. This is crucial for keyboard users and those using accessibility tools.

**Example: Custom Focus Border**
```typescript
const chart = new AccumulationChart({
  series: [{
    dataSource: data,
    xName: 'x',
    yName: 'y',
    type: 'Pie'
  }],
  // Customize focus border appearance
  focusBorderColor: '#FF0000',      // Red focus border
  focusBorderWidth: 3,              // 3px border width
  focusBorderMargin: 5,             // 5px margin from chart boundary
}, '#chart');
```

## Screen Reader Support

Chart announces data through screen readers when properly configured with accessibility properties and descriptive labels.

### Data Label for Screen Readers

Ensure data labels display all necessary information for screen reader users:

```typescript
import { AccumulationChart, PieSeries } from '@syncfusion/ej2-charts';

AccumulationChart.Inject(PieSeries);

const chart = new AccumulationChart({
  series: [{
    dataSource: [
      { x: 'Product A', y: 25000 },
      { x: 'Product B', y: 30000 },
      { x: 'Product C', y: 20000 }
    ],
    xName: 'x',
    yName: 'y',
    type: 'Pie',
    dataLabel: {
      visible: true,
      format: '${point.x}: ${point.y} (${point.percentage}%)'
    }
  }],
  accessibility: {
    accessibilityDescription: 'Sales data visualization',
    accessibilityRole: 'chart'
  }
}, '#chart');
```

### Announce Chart Changes

```typescript
import { AccumulationChart, PieSeries } from '@syncfusion/ej2-charts';

AccumulationChart.Inject(PieSeries);

const chart = new AccumulationChart({
  series: [{ 
    dataSource: data, 
    xName: 'x', 
    yName: 'y', 
    type: 'Pie' 
  }],
  pointRender: (args) => {
    // Log point information (can be announced via screen readers)
    console.log(`Point: ${args.point.x}, Value: ${args.point.y}, Percentage: ${args.point.percentage}%`);
  }
}, '#chart');
```

## Dynamic Data Updates

Update chart data in real-time while maintaining interactivity and accessibility.

### Update Data Source

**TypeScript:**
```typescript
import { AccumulationChart, PieSeries } from '@syncfusion/ej2-charts';

AccumulationChart.Inject(PieSeries);

const initialData = [
  { x: 'Category A', y: 25000 },
  { x: 'Category B', y: 30000 },
  { x: 'Category C', y: 20000 }
];

const chart = new AccumulationChart({
  series: [{
    dataSource: initialData,
    xName: 'x',
    yName: 'y',
    type: 'Pie'
  }]
}, '#chart');

// Update data dynamically
function updateChartData(newData) {
  chart.series[0].dataSource = newData;
  chart.refresh();  // Redraw chart
}

// Example: Update every 5 seconds
setInterval(() => {
  const updatedData = fetchLatestData();  // Your API call
  updateChartData(updatedData);
}, 5000);
```

### Real-Time Streaming Example

```typescript
import { AccumulationChart, PieSeries } from '@syncfusion/ej2-charts';

AccumulationChart.Inject(PieSeries);

// Simulate real-time data updates
let realTimeData = [
  { x: 'A', y: 25 },
  { x: 'B', y: 30 },
  { x: 'C', y: 20 },
  { x: 'D', y: 25 }
];

const chart = new AccumulationChart({
  series: [{
    dataSource: realTimeData,
    xName: 'x',
    yName: 'y',
    type: 'Pie'
  }],
  title: 'Real-Time Data',
  accessibility: {
    accessibilityDescription: 'Real-time data visualization',
    accessibilityRole: 'chart'
  }
}, '#chart');

// Update data every 2 seconds
const updateInterval = setInterval(() => {
  realTimeData = realTimeData.map(item => ({
    ...item,
    y: Math.max(5, item.y + (Math.random() * 10 - 5))  // Random change, minimum 5
  }));
  
  chart.series[0].dataSource = realTimeData;
  chart.refresh();
}, 2000);

// Stop updates after 60 seconds
setTimeout(() => clearInterval(updateInterval), 60000);
```

## Animation & Transitions

Configure animation behavior on chart load and data updates while maintaining accessibility.

### Animation Settings

**TypeScript:**
```typescript
import { AccumulationChart, PieSeries } from '@syncfusion/ej2-charts';

AccumulationChart.Inject(PieSeries);

const chart = new AccumulationChart({
  series: [{
    dataSource: [
      { x: 'Product A', y: 25000 },
      { x: 'Product B', y: 30000 },
      { x: 'Product C', y: 20000 }
    ],
    xName: 'x',
    yName: 'y',
    type: 'Pie',
    animation: {
      enable: true,
      duration: 1000
    }
  }],
  enableAnimation: true
}, '#chart');
```

### Animation Events

**TypeScript:**
```typescript
import { AccumulationChart, PieSeries } from '@syncfusion/ej2-charts';

AccumulationChart.Inject(PieSeries);

const chart = new AccumulationChart({
  series: [{
    dataSource: [
      { x: 'Product A', y: 25000 },
      { x: 'Product B', y: 30000 },
      { x: 'Product C', y: 20000 }
    ],
    xName: 'x',
    yName: 'y',
    type: 'Pie'
  }],
  
  // Animation complete event
  animationComplete: () => {
    console.log('Animation finished');
    // Perform actions after animation
  }
}, '#chart');
```

## Event Handling

Handle various chart events for custom interactions and enhanced functionality.

### Common Events

**TypeScript:**
```typescript
import { AccumulationChart, PieSeries } from '@syncfusion/ej2-charts';

AccumulationChart.Inject(PieSeries);

const chart = new AccumulationChart({
  series: [{
    dataSource: [
      { x: 'Product A', y: 25000 },
      { x: 'Product B', y: 30000 },
      { x: 'Product C', y: 20000 }
    ],
    xName: 'x',
    yName: 'y',
    type: 'Pie'
  }],
  
  // Chart loaded event
  loaded: () => {
    console.log('Chart loaded successfully');
  },
  
  // Series render event
  seriesRender: () => {
    console.log('Series rendering');
  },
  
  // Point render event
  pointRender: (args) => {
    // Customize points before rendering
    if (args.point.y > 25000) {
      args.point.color = '#00D084';  // Highlight high values
    }
  },
  
  // Text render event
  textRender: (args) => {
    console.log('Label rendered:', args.text);
  },
  
  // Tooltip render event
  tooltipRender: (args) => {
    args.text = args.text.toUpperCase();  // Customize tooltip
  }
}, '#chart');
```

### Point Click Event

```typescript
import { AccumulationChart, PieSeries } from '@syncfusion/ej2-charts';

AccumulationChart.Inject(PieSeries);

const chart = new AccumulationChart({
  series: [{ 
    dataSource: [
      { x: 'Product A', y: 25000 },
      { x: 'Product B', y: 30000 },
      { x: 'Product C', y: 20000 }
    ], 
    xName: 'x', 
    yName: 'y', 
    type: 'Pie' 
  }],
  
  pointClick: (args) => {
    console.log(`Clicked: ${args.point.x}, Value: ${args.point.y}, Percentage: ${args.point.percentage}%`);
    // Send data to server, update dashboard, etc.
  }
}, '#chart');
```

## Performance Optimization

Optimize chart rendering for large datasets while maintaining accessibility.

### Large Dataset Configuration

**TypeScript:**
```typescript
import { AccumulationChart, PieSeries } from '@syncfusion/ej2-charts';

AccumulationChart.Inject(PieSeries);

// Create large dataset
const largeDataset = Array.from({ length: 100 }, (_, i) => ({
  x: `Item ${i + 1}`,
  y: Math.floor(Math.random() * 10000)
}));

const chart = new AccumulationChart({
  series: [{
      dataSource: largeDataset,
      xName: 'x',
      yName: 'y',
      type: 'Pie',
      groupMode: 'Point',
      groupTo: '10'
    }],
  
  // Performance optimizations
  enableSmartLabels: true
}, '#chart');
```

### Group Small Values

```typescript
series: [{
  dataSource: data,
  xName: 'x',
  yName: 'y',
  type: 'Pie',
  groupMode: 'Value'
}]
```

## Troubleshooting

### Issue: Animation too slow/fast
- **Solution:** Adjust `duration` property (milliseconds)
- Reduce from 1000ms to 500ms for faster animation
- Increase to 2000ms+ for slower animation

### Issue: Keyboard navigation not working
- **Solution:** Set `tabIndex: 0` on chart
- Ensure chart container has focus
- Check browser keyboard accessibility settings
- Verify `accessibility: { focusable: true }` is configured

### Issue: Real-time updates lag
- **Solution:** Reduce update frequency
- Use `groupMode` to reduce data points
- Consider pagination for large datasets
- Optimize data source refresh intervals

### Issue: Accessibility descriptions not announced
- **Solution:** Verify screen reader is enabled
- Check browser console for errors
- Ensure data labels are visible
- Verify `accessibility` properties are correctly configured
- Test with multiple screen readers (NVDA, JAWS, VoiceOver)

### Issue: Focus border not visible
- **Solution:** Adjust `focusBorderColor` and `focusBorderWidth`
- Increase `focusBorderWidth` (try 2-3px)
- Choose contrasting colors for `focusBorderColor`
- Verify `focusBorderMargin` is appropriate

## Advanced Patterns

### Pattern 1: Dashboard with Multiple Charts

```typescript
import { AccumulationChart, PieSeries, AccumulationLegend } from '@syncfusion/ej2-charts';

AccumulationChart.Inject(PieSeries, AccumulationLegend);

const data = [
  { x: 'Product A', y: 25000 },
  { x: 'Product B', y: 30000 },
  { x: 'Product C', y: 20000 }
];

// Pie chart
const pieChart = new AccumulationChart({
  series: [{ 
    dataSource: data, 
    xName: 'x', 
    yName: 'y', 
    type: 'Pie' 
  }],
  title: 'Sales by Region',
  accessibility: {
    accessibilityDescription: 'Sales by region pie chart',
    accessibilityRole: 'chart'
  }
}, '#pie-chart');

// Doughnut chart
const doughnutChart = new AccumulationChart({
  series: [{ 
    dataSource: data, 
    xName: 'x', 
    yName: 'y', 
    type: 'Doughnut',
    innerRadius: '50%'
  }],
  title: 'Revenue Distribution',
  accessibility: {
    accessibilityDescription: 'Revenue distribution doughnut chart',
    accessibilityRole: 'chart'
  }
}, '#doughnut-chart');
```

### Pattern 2: Drill-Down Navigation

Observe this sample for [Drill-Down navigation](https://ej2.syncfusion.com/demos/#/tailwind3/chart/drill-down-pie.html).

### Pattern 3: Conditional Formatting

Observe this sample for [Conditional Formatting](https://ej2.syncfusion.com/demos/#/tailwind3/chart/drill-down-pie.html) using `pointRender`.

## API Reference Links

**Accessibility Properties:**
- [AccessibilityModel](https://ej2.syncfusion.com/documentation/api/accumulation-chart/accessibilityModel/)
- [accessibilityDescription](https://ej2.syncfusion.com/documentation/api/accumulation-chart/accessibilityModel/#accessibilitydescription)
- [accessibilityRole](https://ej2.syncfusion.com/documentation/api/accumulation-chart/accessibilityModel/#accessibilityrole)
- [focusable](https://ej2.syncfusion.com/documentation/api/accumulation-chart/accessibilityModel/#focusable)
- [tabIndex](https://ej2.syncfusion.com/documentation/api/accumulation-chart/accessibilityModel/#tabindex)

**Focus Border Properties:**
- [focusBorderColor](https://ej2.syncfusion.com/documentation/api/accumulation-chart/#focusbordercolor)
- [focusBorderWidth](https://ej2.syncfusion.com/documentation/api/accumulation-chart/#focusborderwidth)
- [focusBorderMargin](https://ej2.syncfusion.com/documentation/api/accumulation-chart/#focusbordermargin)

**Events:**
- [load Event](https://ej2.syncfusion.com/documentation/api/accumulation-chart/#load)
- [loaded Event](https://ej2.syncfusion.com/documentation/api/accumulation-chart/#loaded)
- [animationComplete Event](https://ej2.syncfusion.com/documentation/api/accumulation-chart/#animationcomplete)
- [pointRender Event](https://ej2.syncfusion.com/documentation/api/accumulation-chart/#pointrender)
- [seriesRender Event](https://ej2.syncfusion.com/documentation/api/accumulation-chart/#seriesrender)
- [pointClick Event](https://ej2.syncfusion.com/documentation/api/accumulation-chart/#pointclick)
- [tooltipRender Event](https://ej2.syncfusion.com/documentation/api/accumulation-chart/#tooltiprender)
- [textRender Event](https://ej2.syncfusion.com/documentation/api/accumulation-chart/#textrender)
- [beforeResize Event](https://ej2.syncfusion.com/documentation/api/accumulation-chart/#beforeresize)
- [resized Event](https://ej2.syncfusion.com/documentation/api/accumulation-chart/#resized)

**Methods:**
- [refresh() Method](https://ej2.syncfusion.com/documentation/api/accumulation-chart/#refresh)
- [Inject() Method](https://ej2.syncfusion.com/documentation/api/accumulation-chart/#inject)

**Legend Settings:**
- [legendSettings](https://ej2.syncfusion.com/documentation/api/accumulation-chart/#legendsettings)
- [Legend Accessibility](https://ej2.syncfusion.com/documentation/api/accumulation-chart/legendSettingsModel/#accessibility)

## Next Steps

- **Review Getting Started** for basic setup
- **Explore Chart Types** for choosing the right visualization
- **Implement Accessibility** for inclusive design
- **Optimize Performance** for large datasets
- **Check Common Patterns** for specific use cases
- **Review API Reference** for complete API documentation
