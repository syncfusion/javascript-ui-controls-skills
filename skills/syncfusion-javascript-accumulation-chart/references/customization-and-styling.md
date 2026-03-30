# Customization & Styling

**API Reference:** [AccumulationTheme](https://ej2.syncfusion.com/documentation/api/accumulation-chart/accumulationtheme/) | [TitleStyleSettingsModel](https://ej2.syncfusion.com/documentation/api/accumulation-chart/titlestylesettingsmodel/) | [Export Method](https://ej2.syncfusion.com/documentation/api/accumulation-chart/#export)

## Table of Contents
- [Theme Options](#theme-options)
- [CSS Customization](#css-customization)
- [Responsive Sizing](#responsive-sizing)
- [Title & Subtitle](#title--subtitle)
- [Print & Export](#print--export)

## Theme Options

Apply predefined themes to your accumulation chart for consistent, professional styling.

### Available Themes

**TypeScript:**
```typescript
import { AccumulationChart, PieSeries } from '@syncfusion/ej2-charts';

AccumulationChart.Inject(PieSeries);

const chart = new AccumulationChart({
  series: [{ dataSource: data, xName: 'x', yName: 'y', type: 'Pie' }],
  theme: 'Material'  // Material, Bootstrap, Fabric, HighContrast, Fluent, Bootstrap5
}, '#chart');
```

### Theme Reference

| Theme | Description | Use Case |
|-------|-------------|----------|
| **Material** | Material Design colors & layout | Modern, clean applications |
| **Bootstrap** | Bootstrap default colors | Bootstrap-based projects |
| **Fabric** | Microsoft Fabric design | Enterprise applications |
| **HighContrast** | High contrast for accessibility | Accessibility requirements |
| **Fluent** | Microsoft Fluent Design | Modern Microsoft-style apps |
| **Bootstrap5** | Bootstrap 5 styling | Bootstrap 5 projects |

### Series-Level Styling

**TypeScript:**
```typescript
series: [{
  dataSource: data,
  xName: 'x',
  yName: 'y',
  type: 'Pie',
  border: {
    color: '#CC0000',
    width: 2
  }
}]
```

### Container-Based Sizing

**HTML:**
```html
<div id="chart-container" style="width: 100%; height: 500px;">
  <div id="chart"></div>
</div>
```

**TypeScript:**
```typescript
const container = document.getElementById('chart-container');
const width = container.offsetWidth;
const height = container.offsetHeight;

const chart = new AccumulationChart({
  series: [{ dataSource: data, xName: 'x', yName: 'y', type: 'Pie' }],
  height: `${height}px`,
  width: `${width}px`,
}, '#chart');
```

## Title & Subtitle

Add context to your chart with titles and subtitles.

### Basic Title

**TypeScript:**
```typescript
const chart = new AccumulationChart({
  series: [{ dataSource: data, xName: 'x', yName: 'y', type: 'Pie' }],
  title: 'Sales Distribution by Region'
}, '#chart');
```

### Title with Subtitle

**TypeScript:**
```typescript
const chart = new AccumulationChart({
  series: [{ dataSource: data, xName: 'x', yName: 'y', type: 'Pie' }],
  title: 'Sales Distribution by Region',
  subTitle: 'Quarterly Report - Q1 2024'
}, '#chart');
```

### Title Styling

**TypeScript:**
```typescript
const chart = new AccumulationChart({
  series: [{ dataSource: data, xName: 'x', yName: 'y', type: 'Pie' }],
  title: 'Sales Distribution',
  titleStyle: {
    fontFamily: 'Segoe UI',
    fontStyle: 'Normal',
    fontWeight: 'Bold',
    size: '18px',
    color: '#2F5496'
  },
  subTitle: 'Q1 2024',
  subTitleStyle: {
    fontFamily: 'Segoe UI',
    fontStyle: 'Italic',
    size: '12px',
    color: '#666666'
  }
}, '#chart');
```

### Complete Title Configuration

**TypeScript:**
```typescript
const chart = new AccumulationChart({
  series: [{ dataSource: data, xName: 'x', yName: 'y', type: 'Pie' }],
  
  // Title
  title: 'Regional Sales Performance',
  titleStyle: {
    color: '#1F77B4',
    fontWeight: 'Bold',
    size: '16px'
  },
  
  // Subtitle
  subTitle: 'Fiscal Year 2024',
  subTitleStyle: {
    color: '#4472C4',
    size: '13px'
  }
}, '#chart');
```

## Print & Export

Export charts to various formats or print directly.

**Note** Import and Inject `Export`.

### Export to PNG

**TypeScript:**
```typescript
import { AccumulationChart, Export } from '@syncfusion/ej2-charts';

// In your component/class
const chart = new AccumulationChart({
    series: [{ dataSource: data, xName: 'x', yName: 'y', type: 'Pie' }]
  }, '#chart');

// Export function
document.getElementById('exportPNG').onclick = () => {
    chart.exportModule.export('PNG', 'result');
};
```

### Export to SVG

**TypeScript:**
```typescript
document.getElementById('exportSVG').onclick = () => {
    chart.exportModule.export('SVG', 'result');
};
```

### Export to PDF

**TypeScript:**
```typescript
document.getElementById('exportPDF').onclick = () => {
    chart.exportModule.export('PDF', 'result');
};
```

### Print Chart

**TypeScript:**
```typescript
document.getElementById('print').onclick = () => {
  chart.print();
};
```

### Export/Print Button Example

**HTML:**
```html
<button id="exportPNG">Export PNG</button>
<button id="exportSVG">Export SVG</button>
<button id="exportPDF">Export PDF</button>
<button id="print">Print</button>

<div id="chart"></div>
```

**TypeScript:**
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
  title: 'Sales Distribution'
}, '#chart');

// Button event listeners
document.getElementById('exportPNG').addEventListener('click', () => {
  chart.export('PNG', 'sales-chart.png');
});

document.getElementById('exportSVG').addEventListener('click', () => {
  chart.export('SVG', 'sales-chart.svg');
});

document.getElementById('exportPDF').addEventListener('click', () => {
  chart.export('PDF', 'sales-chart.pdf');
});

document.getElementById('print').addEventListener('click', () => {
  chart.print();
});
```

## Complete Customization Example

**TypeScript:**
```typescript
import { 
  AccumulationChart, 
  PieSeries, 
  AccumulationDataLabel,
  AccumulationLegend,
  Tooltip 
} from '@syncfusion/ej2-charts';

AccumulationChart.Inject(
  PieSeries, 
  AccumulationDataLabel, 
  AccumulationLegend,
  Tooltip
);

const data = [
  { x: 'East', y: 135 },
  { x: 'West', y: 198 },
  { x: 'North', y: 160 },
  { x: 'South', y: 121 }
];

const chart = new AccumulationChart({
  // Data
  series: [{
    dataSource: data,
    xName: 'x',
    yName: 'y',
    type: 'Pie',
    dataLabel: {
      visible: true,
      position: 'Outside'
    }
  }],
  
  // Styling
  theme: 'Fluent',
  background: '#F5F7FA',
  
  // Title & Subtitle
  title: 'Regional Sales Analysis',
  titleStyle: { color: '#2F5496', size: '16px' },
  subTitle: 'Quarter 1 2024',
  subTitleStyle: { color: '#666666', size: '13px' },
  
  // Legend
  legendSettings: {
    visible: true,
    position: 'Right',
    alignment: 'Center'
  },
  
  // Tooltip
  tooltip: {
    enable: true,
    format: '<b>${point.x}</b><br>Sales: $${point.y}k'
  },
  
  // Responsive
  height: '100%',
  width: '100%'
}, '#chart');

// Export functionality
document.getElementById('exportPNG').onclick = () => {
  chart.exportModule.export('PNG', 'result');
};
```

## API Reference Links

- [AccumulationTheme Enum](https://ej2.syncfusion.com/documentation/api/accumulation-chart/accumulationtheme/)
- [TitleStyleSettingsModel](https://ej2.syncfusion.com/documentation/api/accumulation-chart/titlestylesettingsmodel/)
- [BorderModel](https://ej2.syncfusion.com/documentation/api/accumulation-chart/bordermodel/)
- [MarginModel](https://ej2.syncfusion.com/documentation/api/accumulation-chart/marginmodel/)
- [export() Method](https://ej2.syncfusion.com/documentation/api/accumulation-chart/#export)
- [print() Method](https://ej2.syncfusion.com/documentation/api/accumulation-chart/#print)

## Troubleshooting

**Issue: Theme not applying**
- Solution: Verify theme CSS is imported
- Check theme name spelling (case-sensitive)
- Clear browser cache
- See [AccumulationTheme API](https://ej2.syncfusion.com/documentation/api/accumulation-chart/accumulationtheme/)

**Issue: Custom CSS not working**
- Solution: Use correct Syncfusion class names
- Check CSS specificity (may need !important)
- Verify CSS is loaded after theme CSS

**Issue: Export not working**
- Solution: Ensure chart is fully rendered before export
- Check browser permissions for downloads
- Verify export feature is enabled
- See [export() API](https://ej2.syncfusion.com/documentation/api/accumulation-chart/#export)

**Issue: Title too long/overlapping**
- Solution: Reduce font size in `titleStyle`
- Adjust `titlePadding` for spacing
- Use shorter title text
- See [TitleStyleSettingsModel API](https://ej2.syncfusion.com/documentation/api/accumulation-chart/titlestylesettingsmodel/)
