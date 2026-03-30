# Data Handling & Updates

## Table of Contents
- [Dynamic Data Updates](#dynamic-data-updates)
- [Real-Time Charts](#real-time-charts)
- [Data Editing](#data-editing)
- [Large Datasets](#large-datasets)
- [Data Sources](#data-sources)

**Note:** All data update syntax is identical for TypeScript and JavaScript.

**API Reference:** See [api-reference.md](api-reference.md#methods) for Chart methods like `refresh()`, `updateSeries()`, `addSeries()`, and `removeSeries()`. See [Series](api-reference.md#series) class for `dataSource` property documentation.

## Dynamic Data Updates

### Update Chart Data

Replace entire dataset (TS & JS identical):

```javascript
// TypeScript or JavaScript
// Original chart created
const chart = new ej.charts.Chart({
  series: [{
    dataSource: initialData,
    xName: 'month',
    yName: 'sales'
  }]
}, '#chart');
```

### Add New Data Point

Append single data point (TS & JS identical):

```javascript
// Get current data
let currentData = chart.series[0].dataSource;

// Add new point
currentData.push({ month: 'May', sales: 45 });

// Refresh
chart.series[0].dataSource = currentData;
chart.refresh();
```

### Remove Data Point

```javascript
// Remove last point
let currentData = chart.series[0].dataSource;
currentData.pop();

chart.series[0].dataSource = currentData;
chart.refresh();
```

### Update Specific Points

```javascript
// Modify data values
const data = chart.series[0].dataSource;
data[0].sales = 55;    // Update first point

chart.series[0].dataSource = data;
chart.refresh();
```

### Animate on Update

```typescript
// Chart will animate when refreshed
chart.series[0].dataSource = newData;
chart.refresh();  // Uses animation settings
```

## Real-Time Charts

### Streaming Data Update

Update chart with new data points at intervals:

```typescript
// Simulate real-time data
const realTimeData = [];
let counter = 0;

const interval = setInterval(() => {
  counter++;
  realTimeData.push({
    timestamp: new Date(),
    value: Math.random() * 100
  });
  
  // Keep only last 50 points
  if (realTimeData.length > 50) {
    realTimeData.shift();
  }
  
  chart.series[0].dataSource = realTimeData;
  chart.refresh();
}, 1000);  // Update every second
```

### Real-Time with WebSocket

```typescript
// Connect to WebSocket
const ws = new WebSocket('ws://your-server/data');

ws.onmessage = (event) => {
  const newData = JSON.parse(event.data);
  
  // Update chart
  chart.series[0].dataSource.push(newData);
  
  // Remove old data
  if (chart.series[0].dataSource.length > 100) {
    chart.series[0].dataSource.shift();
  }
  
  chart.refresh();
};
```

### Real-Time with API Polling

```typescript
// Fetch data periodically
setInterval(async () => {
  const response = await fetch('/api/latest-data');
  const newData = await response.json();
  
  chart.series[0].dataSource = newData;
  chart.refresh();
}, 5000);  // Poll every 5 seconds
```

## Data Editing

### Enable Point Editing

Allow users to modify data points:

```typescript
const chart = new Chart({
  series: [{
    dataSource: data,
    xName: 'x',
    yName: 'y',
    type: 'Column'
  }],
}, '#chart');
```

### Handle Point Selection for Editing

```typescript
pointRender: (args: IPointRenderEventArgs) => {
  // Make points interactive
},
pointClick: (args: IPointEventArgs) => {
  // Handle point click
  console.log(`Clicked: ${args.point.x}, ${args.point.y}`);
}
```

### Update Data from Form

```typescript
// Form inputs
const categoryInput = document.getElementById('category');
const valueInput = document.getElementById('value');
const addBtn = document.getElementById('add-btn');

addBtn.addEventListener('click', () => {
  const newPoint = {
    category: categoryInput.value,
    value: parseFloat(valueInput.value)
  };
  
  // Add to chart data
  chart.series[0].dataSource.push(newPoint);
  chart.refresh();
  
  // Clear inputs
  categoryInput.value = '';
  valueInput.value = '';
});
```

### Delete Points

```typescript
// Right-click to delete
document.addEventListener('contextmenu', (e) => {
  if (e.target.closest('.e-chart')) {
    e.preventDefault();
    
    // Get clicked point
    const pointIndex = chart.series[0].dataSource.length - 1;
    chart.series[0].dataSource.splice(pointIndex, 1);
    chart.refresh();
  }
});
```

## Large Datasets

### Performance Optimization

For large datasets (1000+ points), use strategies to maintain performance:

```typescript
const largeData = generateLargeDataset(10000);

const chart = new Chart({
  series: [{
    dataSource: largeData,
    xName: 'x',
    yName: 'y',
    type: 'Line'
  }],
  // Disable unnecessary features
  tooltip: { enable: false },  // Disable tooltips
  legend: { visible: false },  // Hide legend
  annotation: []               // Remove annotations
}, '#chart');
```

### Virtual Scrolling

```typescript
zoomSettings: {
  enable: true,
  mode: 'X',
  enableMouseWheelZooming: true,
  enableSelectionZooming: true
}
```

### Data Point Filtering

Show only relevant data:

```typescript
// Filter to last 100 points
const filteredData = largeData.slice(-100);

chart.series[0].dataSource = filteredData;
chart.refresh();
```

### Aggregation for Summary

```typescript
// Aggregate daily to weekly for large time-series
function aggregateData(data, interval) {
  const aggregated = [];
  for (let i = 0; i < data.length; i += interval) {
    const chunk = data.slice(i, i + interval);
    const avg = chunk.reduce((sum, d) => sum + d.value, 0) / chunk.length;
    aggregated.push({
      date: chunk[0].date,
      value: avg
    });
  }
  return aggregated;
}

const summaryData = aggregateData(largeData, 7);  // Weekly summary
```

## Data Sources

### Array of Objects

```typescript
const data = [
  { month: 'Jan', sales: 35 },
  { month: 'Feb', sales: 28 },
  { month: 'Mar', sales: 34 }
];

series: [{
  dataSource: data,
  xName: 'month',
  yName: 'sales'
}]
```

### Remote Data (URL)

```typescript
series: [{
  dataSource: 'https://api.example.com/data',
  xName: 'month',
  yName: 'sales'
}]
```

### Async Data Loading

```typescript
async function loadChartData() {
  try {
    const response = await fetch('/api/chart-data');
    const data = await response.json();
    
    chart.series[0].dataSource = data;
    chart.refresh();
  } catch (error) {
    console.error('Error loading data:', error);
  }
}

loadChartData();
```

### CSV Data

```typescript
// Parse CSV
function parseCSV(csvString) {
  const lines = csvString.trim().split('\n');
  const headers = lines[0].split(',');
  
  return lines.slice(1).map(line => {
    const values = line.split(',');
    const obj = {};
    headers.forEach((header, index) => {
      obj[header] = isNaN(values[index]) ? values[index] : parseFloat(values[index]);
    });
    return obj;
  });
}

const csvData = parseCSV(csvString);
chart.series[0].dataSource = csvData;
```

### Database Query

```typescript
// Fetch from database
async function getChartDataFromDB(query) {
  const response = await fetch('/api/query', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ query })
  });
  
  return await response.json();
}

const dbData = await getChartDataFromDB('SELECT * FROM sales');
chart.series[0].dataSource = dbData;
chart.refresh();
```

## Common Configurations

### Configuration 1: Real-Time Stock Data

```typescript
const chart = new Chart({
  primaryXAxis: {
    valueType: 'DateTime',
    intervalType: 'Minutes',
    interval: 1
  },
  series: [{
    dataSource: [],
    xName: 'timestamp',
    yName: 'price',
    type: 'Line'
  }]
}, '#chart');

// Stream data
const ws = new WebSocket('ws://stream.example.com/stock');
ws.onmessage = (e) => {
  const price = JSON.parse(e.data);
  chart.series[0].dataSource.push(price);
  
  if (chart.series[0].dataSource.length > 100) {
    chart.series[0].dataSource.shift();
  }
  
  chart.refresh();
};
```

### Configuration 2: User-Editable Data Table

```typescript
// Data grid linked to chart
const dataGrid = new Grid({
  dataSource: chartData,
  columns: [
    { field: 'category', headerText: 'Category' },
    { field: 'value', headerText: 'Value' }
  ]
});

// Update chart when grid data changes
dataGrid.actionComplete = () => {
  chart.series[0].dataSource = dataGrid.getCurrentViewRecords();
  chart.refresh();
};
```

### Configuration 3: Large Dataset with Filtering

```typescript
// Filter controls
const categorySelect = document.getElementById('category-filter');

categorySelect.addEventListener('change', (e) => {
  const filtered = largeDataset.filter(d => d.category === e.target.value);
  
  chart.series[0].dataSource = filtered;
  chart.refresh();
});

// Or use aggregation for very large datasets
function updateWithAggregation(filterCriteria) {
  const filtered = largeDataset.filter(filterCriteria);
  const aggregated = aggregateData(filtered, 7);  // Weekly aggregation
  
  chart.series[0].dataSource = aggregated;
  chart.refresh();
}
```
