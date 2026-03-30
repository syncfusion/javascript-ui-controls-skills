# Advanced Features

## Table of Contents
- [Technical Indicators](#technical-indicators)
- [Trendlines](#trendlines)
- [Export Functionality](#export-functionality)
- [Animations & Transitions](#animations--transitions)
- [Print Functionality](#print-functionality)

**Note:** Configuration syntax is identical for TypeScript and JavaScript. Examples use object literal format.

**API Reference:** See [api-reference.md](api-reference.md#enumerations) for `TechnicalIndicators` and `TrendlineTypes` enumerations. See [Trendline](api-reference.md#trendline), [TechnicalIndicator](api-reference.md#technical-indicator), and [Export](api-reference.md#export-module) class documentation.

## Technical Indicators

Technical indicators analyze price/value movements for financial data. Configuration is identical in TS & JS.

### Simple Moving Average (SMA)

Smooth data by averaging values over a period:

```typescript
// TypeScript or JavaScript (identical configuration)
indicators: [{
  type: 'Sma', field: 'Close', seriesName: 'Apple Inc', fill: 'blue',
  period: 3, animation: { enable: true }
}],
```

**Parameters:**
- `period: 20` - 20-day moving average

### Exponential Moving Average (EMA)

Recent values weighted more heavily:

```typescript
indicators: [{
  type: 'Ema', field: 'Close', seriesName: 'Apple Inc', fill: 'blue',
  period: 3, animation: { enable: true }
}],
```

### MACD (Moving Average Convergence Divergence)

Identify trend changes:

```typescript
indicators: [{
  type: 'Macd', period: 3,  fastPeriod: 5,  slowPeriod: 2,  seriesName: 'gold',
  macdType: 'Both',  width: 2,  fill: 'blue',  yAxisName: 'secondary',
}],
```

### Bollinger Bands

Show volatility levels:

```typescript
indicators: [{
  type: 'BollingerBands', field: 'Close', seriesName: 'USA', fill: 'blue',
  period: 3, animation: { enable: true }, upperLine: { color: 'orange' }, lowerLine: { color: 'yellow' }
}],
```

### RSI (Relative Strength Index)

Measure momentum:

```typescript
indicators: [{
  type: 'Rsi', field: 'Close', seriesName: 'USA', yAxisName: 'secondary', fill: 'blue',
  showZones: true, overBought: 70, overSold: 30,
  period: 3, animation: { enable: true }, upperLine: { color: 'red' }, lowerLine: { color: 'green' }
}],
```

### ATR (Average True Range)

Measure volatility:

```typescript
indicators: [{
  type: 'Atr', field: 'Low', seriesName: 'Apple Inc', yAxisName: 'secondary', fill: 'blue',
  period: 3, animation: { enable: true}
}],
```

### Stochastic Oscillator

Compare closing price to range:

```typescript
indicators: [{
  type: 'Stochastic', field: 'Close', seriesName: 'USA', yAxisName: 'secondary', fill: 'blue',
  kPeriod: 2, dPeriod: 3, showZones: true,
  period: 3, animation: { enable: false },
}],
```

## Trendlines

Visualize data trends and patterns.

### Linear Trendline

Straight line showing overall trend:

```typescript
series: [{
  dataSource: data,
  xName: 'x',
  yName: 'y',
  type: 'Column',
  trendlines: [{
    type: 'Linear',
    name: 'Linear Trend'
  }]
}]
```

### Exponential Trendline

```typescript
trendlines: [{
  type: 'Exponential',
  name: 'Exponential Trend'
}]
```

### Logarithmic Trendline

```typescript
trendlines: [{
  type: 'Logarithmic',
  name: 'Log Trend'
}]
```

### Power Trendline

```typescript
trendlines: [{
  type: 'Power',
  name: 'Power Trend'
}]
```

### Polynomial Trendline

```typescript
trendlines: [{
  type: 'Polynomial',
  polynomialOrder: 2,  // Quadratic (2), Cubic (3), etc.
  name: 'Polynomial Trend'
}]
```

### Trendline Styling

```typescript
trendlines: [{
  type: 'Linear',
  name: 'Trend',
  width: 2,
  fill: '#FF5733',
  dashArray: '5,5'
}]
```

### Multiple Trendlines

```typescript
trendlines: [
  { type: 'Linear', name: 'Linear' },
  { type: 'Exponential', name: 'Exponential' }
]
```

## Export Functionality

### Export to PNG

```typescript
// Create export button
const exportBtn = document.getElementById('export-png');
exportBtn.addEventListener('click', () => {
  chart.export('PNG', 'chart.png');
});
```

### Export to SVG

```typescript
const exportBtn = document.getElementById('export-svg');
exportBtn.addEventListener('click', () => {
  chart.export('SVG', 'chart.svg');
});
```

### Export to PDF

```typescript
const exportBtn = document.getElementById('export-pdf');
exportBtn.addEventListener('click', () => {
  chart.export('PDF', 'chart.pdf');  // Portrait or Landscape
});
});
```

## Animations & Transitions

### Enable Animation

```typescript
animation: {
  enable: true,
  duration: 2000          // 2 seconds
}
```

### Duration

```typescript
animation: {
  enable: true,
  duration: 1500          // 1.5 seconds
}
```

### Disable Animation

```typescript
animation: {
  enable: false
}
```

### Series Animation

```typescript
series: [{
  dataSource: data,
  xName: 'x',
  yName: 'y',
  type: 'Column',
  animation: {
    enable: true,
    duration: 2000
  }
}]
```

### Point Animation

```typescript
pointRender: (args: IPointRenderEventArgs) => {
  // Animate based on point properties
  args.border = { width: 2, color: '#333' };
}
```

## Print Functionality

### Print Chart

```typescript
// Create print button
const printBtn = document.getElementById('print-btn');
printBtn.addEventListener('click', () => {
  chart.print();
});
```

### Print with Custom Title

```typescript
print: {
  title: 'Sales Report - Q4 2024'
}
```

## Common Configurations

### Configuration 1: Stock Chart with Indicators

Observe the different [Technical Indicator](https://ej2.syncfusion.com/documentation/chart/technical-indicators) samples in Stock chart.

### Configuration 2: Data Analysis with Trendlines

Observe the [Trendline](https://ej2.syncfusion.com/documentation/chart/technical-indicators) usages in different samples in chart.

### Configuration 3: Exportable Dashboard

```typescript
// Add export/print buttons
const exportPNG = () => chart.export('PNG', 'dashboard.png');
const exportPDF = () => chart.export('PDF', 'dashboard.pdf');
const printChart = () => chart.print();

// Attach to UI
document.getElementById('export-png').onclick = exportPNG;
document.getElementById('export-pdf').onclick = exportPDF;
document.getElementById('print').onclick = printChart;
```

