# Troubleshooting & Optimization

## Table of Contents
- [Common Issues](#common-issues)
- [Platform-Specific Issues](#platform-specific-issues)
- [Performance Optimization](#performance-optimization)
- [Browser Compatibility](#browser-compatibility)
- [Debugging Strategies](#debugging-strategies)

**API Reference:** See [api-reference.md](api-reference.md#methods) for Chart methods like `refresh()`, `destroy()`, and `export()`. See [Axis](api-reference.md#axis) for axis configuration properties that affect data display.

## Common Issues

### Issue 1: Chart Not Rendering

**Symptoms:** Empty or blank chart area (affects both TS & JS)

**Solutions:**

1. **Check data exists (TS & JS):**
```javascript
// TypeScript or JavaScript
console.log('Data:', chart.series[0].dataSource);
if (!chart.series[0].dataSource || chart.series[0].dataSource.length === 0) {
  console.error('No data provided to chart');
}
```

2. **Verify HTML container (Identical for TS & JS):**
```html
<!-- Ensure container exists and has ID -->
<div id="chart" style="width: 100%; height: 420px;"></div>
```

3. **Check field mapping (TS & JS identical):**
```javascript
// xName and yName must match data object properties
series: [{
  dataSource: data,
  xName: 'month',      // Must exist in data
  yName: 'sales',      // Must exist in data
  type: 'Column'
}]
```

### Issue 2: Data Not Displaying

**Symptoms:** Chart renders but no data visible

**Solutions:**

1. **Check data types (TS & JS):**
```javascript
// Ensure correct data types
const data = [
  { x: 'Jan', y: 35 },      // ✓ String/Number
  { x: 2023, y: '35' },     // ✓ Can work
  { x: null, y: 35 }        // ✗ Null causes issues
];
```

2. **Verify axis configuration:**
```typescript
primaryXAxis: {
  valueType: 'Category',     // Must match data (Category, Double, DateTime)
  labelPlacement: 'OnTicks'
}
```

3. **Check axis ranges:**
```typescript
primaryYAxis: {
  minimum: 0,
  maximum: 100,
  // If all data is outside range, nothing shows
  interval: 10
}
```

### Issue 3: Performance Degradation

**Symptoms:** Chart becomes slow with large datasets

**Solutions:**

1. **Reduce data points:**
```typescript
// Keep only last 100 points for real-time charts
if (chart.series[0].dataSource.length > 100) {
  chart.series[0].dataSource.shift();
}
```

2. **Disable features:**
```typescript
tooltip: { enable: false },     // Heavy on large datasets
legend: { visible: false },
annotation: []
```

3. **Use efficient chart types:**
```typescript
// Prefer: Line, Column
// Avoid: Scatter with 10000+ points, complex animations
type: 'Line'  // Better than Scatter for large data
```

### Issue 4: Memory Leak

**Symptoms:** Memory usage grows over time

---

## Platform-Specific Issues

### TypeScript Issues

**Issue: Type errors with event handlers**

```typescript
// ❌ Error: No type checking
pointRender: (args) => {  // args is any type
  args.fill = '#FF5733';
}

// ✓ Correct: Import type
import { IPointRenderEventArgs } from '@syncfusion/ej2-charts';

pointRender: (args: IPointRenderEventArgs) => {
  args.fill = '#FF5733';  // Type-safe
}
```

**Issue: Module injection errors**

```typescript
// ❌ Missing Chart.Inject()
const chart = new Chart({ series: [{ type: 'Line' }] }, '#chart');
// Error: LineSeries not available

// ✓ Correct: Inject modules
Chart.Inject(LineSeries);
const chart = new Chart({ series: [{ type: 'Line' }] }, '#chart');
```

**Troubleshooting TypeScript:**
- Always run `npm install` after pulling updated dependencies
- Clear webpack cache: `rm -rf dist node_modules/.cache`
- Use TypeScript compiler in strict mode: `tsc --strict`

**Issue: Script loading order**

```html
<!-- ❌ Wrong order - dependency scripts must come first -->
<script src="ej2-charts.min.js"></script>
<script src="ej2-base.min.js"></script>

<!-- ✓ Correct order -->
<script src="ej2-base.min.js"></script>
<script src="ej2-data.min.js"></script>
<script src="ej2-svg-base.min.js"></script>
<script src="ej2-charts.min.js"></script>
```

**Issue: Missing CSS in ES5/CDN**

```html
<!-- Include CSS separately in CDN approach -->
<link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/material.css" />
```

---

**Solutions:**

1. **Dispose chart properly:**
```typescript
// When removing chart
chart.destroy();
document.getElementById('chart').innerHTML = '';
```

2. **Clear event listeners:**
```typescript
// Remove listeners before destroying
window.removeEventListener('resize', resizeHandler);
```

3. **Manage data references:**
```typescript
// Don't keep old data references
chart.series[0].dataSource = null;
chart.refresh();
```

### Issue 5: Tooltip Not Showing

**Symptoms:** Tooltips don't appear on hover

**Solutions:**

1. **Enable tooltips:**
```typescript
tooltip: {
  enable: true  // Must be enabled
}
```

2. **Check format:**
```typescript
tooltip: {
  enable: true,
  format: '<b>${point.x}</b>: ${point.y}'  // Template must have content
}
```

3. **Verify interaction:**
```typescript
// May be hidden by CSS
.e-tooltip {
  z-index: 1000 !important;  // Ensure tooltip is on top
}
```

### Issue 6: Legend Not Working

**Symptoms:** Legend doesn't highlight/hide series

**Solutions:**

1. **Enable legend:**
```typescript
legend: {
  visible: true,
  enableHighlight: true
}
```

2. **Ensure series names:**
```typescript
series: [{
  name: 'Sales',  // Legend uses series name
  dataSource: data,
  type: 'Column'
}]
```

3. **Check positioning:**
```typescript
legend: {
  visible: true,
  position: 'Bottom'  // Try different position
}
```

## Performance Optimization

### Optimization 1: Large Dataset Handling

```typescript
const largeData = generateData(50000);

// Strategy 1: Limit visible points
const visibleData = largeData.slice(-500);

const chart = new Chart({
  series: [{
    dataSource: visibleData,
    type: 'Line'
  }],
  zoomSettings: {
    enable: true,
    mode: 'X'  // Let users zoom to see more
  }
}, '#chart');
```

### Optimization 2: Lazy Load Data

```typescript
// Load data in chunks
async function loadDataInChunks(total, chunkSize) {
  let loadedData = [];
  
  for (let i = 0; i < total; i += chunkSize) {
    const chunk = await fetchData(i, chunkSize);
    loadedData.push(...chunk);
    
    // Update chart progressively
    chart.series[0].dataSource = loadedData;
    if (i === 0) chart.refresh();
  }
}

loadDataInChunks(10000, 1000);
```

### Optimization 3: Aggregation

```typescript
// Aggregate data for display
function aggregateData(data, interval) {
  const aggregated = [];
  
  for (let i = 0; i < data.length; i += interval) {
    const chunk = data.slice(i, i + interval);
    const sum = chunk.reduce((acc, d) => acc + d.value, 0);
    
    aggregated.push({
      date: chunk[0].date,
      value: sum / chunk.length
    });
  }
  
  return aggregated;
}

const summary = aggregateData(largeData, 10);
chart.series[0].dataSource = summary;
```

### Optimization 4: Virtual Scrolling

```typescript
zoomSettings: {
  enable: true,
  enableMouseWheelZooming: true,
  enableSelectionZooming: true,
  enablePinchZooming: true
}
```

### Optimization 5: Debounce Updates

```typescript
let updateTimeout;

function updateChartDebounced(newData) {
  clearTimeout(updateTimeout);
  
  updateTimeout = setTimeout(() => {
    chart.series[0].dataSource = newData;
    chart.refresh();
  }, 500);  // Wait 500ms before updating
}

// Call frequently without performance penalty
window.addEventListener('mousemove', () => {
  updateChartDebounced(getCurrentData());
});
```

## Browser Compatibility

### Supported Browsers

| Browser | Min Version | Notes |
|---------|-------------|-------|
| Chrome | 60+ | Full support |
| Firefox | 55+ | Full support |
| Safari | 11+ | Full support |
| Edge | 79+ | Full support |
| IE 11 | - | Limited support |

### IE 11 Polyfills

```typescript
// Include polyfills for IE 11
import 'core-js/stable';
import 'regenerator-runtime/runtime';
```

### Feature Detection

```typescript
// Check for WebGL support
const hasWebGL = (() => {
  try {
    const canvas = document.createElement('canvas');
    return canvas.getContext('webgl') !== null;
  } catch(e) {
    return false;
  }
})();

if (!hasWebGL) {
  // Fallback to 2D rendering
  chart.enableCanvas = false;
}
```

## Debugging Strategies

### Strategy 1: Console Logging

```typescript
// Log chart state
console.log('Chart Config:', chart);
console.log('Data:', chart.series[0].dataSource);
console.log('Axes:', chart.primaryXAxis, chart.primaryYAxis);

// Log events
chart.pointRender = (args) => {
  console.log('Point rendered:', args);
};
```

### Strategy 2: Visual Debugging

```typescript
// Add visual markers to see data bounds
chart.chartArea = {
  border: { color: '#FF0000', width: 2 }  // Red border shows area
};

// Show axis lines visually
primaryXAxis: {
  lineStyle: { width: 2, color: '#0000FF' }
}
```

### Strategy 3: Browser DevTools

```typescript
// In browser console:
// 1. Inspect chart object
ej.charts.Chart.instances[0]

// 2. Get series data
ej.charts.Chart.instances[0].series[0].dataSource

// 3. Trigger refresh
ej.charts.Chart.instances[0].refresh()
```

### Strategy 4: Network Debugging

```typescript
// Monitor data loading
fetch('/api/chart-data')
  .then(r => r.json())
  .then(data => {
    console.log('Data received:', data);
    console.log('Data points:', data.length);
    console.log('First point:', data[0]);
  })
  .catch(err => console.error('Load failed:', err));
```

### Strategy 5: Event Tracking

```typescript
// Track all chart events
const events = ['load', 'pointRender', 'seriesRender', 'axisLabelRender', 'tooltipRender'];

events.forEach(event => {
  chart[event] = (args) => {
    console.log(`Event: ${event}`, args);
  };
});
```

## Optimization Checklist

**Performance:**
- [ ] Use appropriate chart type
- [ ] Limit data points (1000-5000 optimal)
- [ ] Disable unused features
- [ ] Enable virtual scrolling for large data
- [ ] Use aggregation for summaries
- [ ] Implement debouncing for updates

**Browser Support:**
- [ ] Test in target browsers
- [ ] Include polyfills for IE 11
- [ ] Handle missing WebGL gracefully
- [ ] Test touch/mobile devices

**Accessibility:**
- [ ] High contrast colors
- [ ] Keyboard navigation
- [ ] ARIA labels
- [ ] Data table alternative

**Debugging:**
- [ ] Use browser DevTools
- [ ] Log chart state
- [ ] Monitor network requests
- [ ] Track events
- [ ] Check data format

