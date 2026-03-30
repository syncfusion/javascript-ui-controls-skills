# Legend Configuration

## Table of Contents
- [Overview](#overview)
- [Positioning](#positioning)
- [Alignment](#alignment)
- [Styling & Customization](#styling--customization)
- [Visibility Control](#visibility-control)
- [Interactive Legend Features](#interactive-legend-features)

**API Reference:** [LegendSettingsModel](https://ej2.syncfusion.com/documentation/api/accumulation-chart/legendsettingsmodel/) | [AccumulationLegend Module](https://ej2.syncfusion.com/documentation/api/accumulation-chart/accumulationlegend/)

## Overview

Legends identify data categories and values in your accumulation chart. By default, legends automatically position based on chart dimensions (right for wide charts, bottom for tall charts). Customize position, alignment, appearance, and behavior to match your design requirements.

## Positioning

Control where the legend appears around the chart.

### Basic Positioning

**TypeScript:**
```typescript
import { AccumulationChart, AccumulationLegend } from '@syncfusion/ej2-charts';

AccumulationChart.Inject(AccumulationLegend);

const chart = new AccumulationChart({
  series: [{
    dataSource: data,
    xName: 'x',
    yName: 'y',
    type: 'Pie'
  }],
  legendSettings: {
    visible: true,
    position: 'Right'  // Right, Left, Top, Bottom, Custom
  }
}, '#chart');
```

### Position Options

| Position | Use Case | Layout |
|----------|----------|--------|
| **Right** | Default for wide charts | Legend on right side, vertical |
| **Left** | RTL layouts or space preference | Legend on left side, vertical |
| **Top** | Dashboard headers | Legend on top, horizontal |
| **Bottom** | Traditional charts | Legend on bottom, horizontal |
| **Custom** | Advanced layouts | Positioned via x/y coordinates |

### Right Position (Default)
```typescript
legendSettings: {
  position: 'Right'  // Default, vertical layout
}
```

### Left Position
```typescript
legendSettings: {
  position: 'Left'  // Vertical layout on left
}
```

### Top Position
```typescript
legendSettings: {
  position: 'Top'  // Horizontal layout at top
}
```

### Bottom Position
```typescript
legendSettings: {
  position: 'Bottom'  // Horizontal layout at bottom
}
```

### Custom Position
```typescript
legendSettings: {
  position: 'Custom',
  location: {
      x: 100,  // Distance from left
      y: 50
  }    // Distance from top
}
```

## Alignment

Control how legend items align within their position area.

### Alignment Options

**TypeScript:**
```typescript
legendSettings: {
  position: 'Right',
  alignment: 'Center'  // Center, Near, Far
}
```

| Alignment | Vertical Position | Horizontal Position |
|-----------|------------------|---------------------|
| **Center** | Middle of chart | Middle of chart |
| **Near** | Top/Left of chart | Top/Left of chart |
| **Far** | Bottom/Right of chart | Bottom/Right of chart |

### Examples

**Right Legend with Near Alignment:**
```typescript
legendSettings: {
  position: 'Right',
  alignment: 'Near'  // Legend at top-right
}
```

**Bottom Legend with Far Alignment:**
```typescript
legendSettings: {
  position: 'Bottom',
  alignment: 'Far'  // Legend at bottom-right
}
```

**Top Legend with Center Alignment:**
```typescript
legendSettings: {
  position: 'Top',
  alignment: 'Center'  // Legend centered horizontally
}
```

## Styling & Customization

Control legend appearance with styling options.

### Border and Background

**TypeScript:**
```typescript
legendSettings: {
  border: {
    color: '#000000',
    width: 1
  },
  background: '#F5F5F5',
  margin: {
    left: 10,
    right: 10,
    top: 10,
    bottom: 10
  }
}
```

### Font Customization

**TypeScript:**
```typescript
legendSettings: {
  textStyle: {
    color: '#333333',
    fontFamily: 'Segoe UI',
    size: '14px',
    fontStyle: 'Normal',
    fontWeight: 'Normal'
  }
}
```

### Label Formatting

**TypeScript:**
```typescript
legendSettings: {
  template: '${series.name}: ${point.x}',
  maximumLabelWidth: 18  // Wrap long labels
}
```

### Shape Customization

**TypeScript:**
```typescript
legendSettings: {
  shapeHeight: 15,
  shapeWidth: 15,
  shapePadding: 5  // Space between shape and label
}
```

## Visibility Control

Control when legends appear and their visibility state.

### Toggle Visibility

**TypeScript:**
```typescript
legendSettings: {
  visible: true  // false to hide legend
}
```

### Clicking Legend Items

By default, clicking a legend item toggles series visibility (if configured):

**TypeScript:**
```typescript
legendSettings: {
  visible: true,
  toggleVisibility: true,
  mode: 'Point'  // Point or Series mode
}
```

## Interactive Legend Features

Create rich legend interactions.

### Complete Configuration Example

**TypeScript:**
```typescript
import { AccumulationChart, PieSeries, AccumulationLegend } from '@syncfusion/ej2-charts';

AccumulationChart.Inject(PieSeries, AccumulationLegend);

const chart = new AccumulationChart({
  series: [{
    dataSource: [
      { x: 'North', y: 135 },
      { x: 'South', y: 198 },
      { x: 'East', y: 160 },
      { x: 'West', y: 121 }
    ],
    xName: 'x',
    yName: 'y',
    type: 'Pie'
  }],
  legendSettings: {
    // Visibility
    visible: true,
    mode: 'Point',
    
    // Position & Alignment
    position: 'Right',
    alignment: 'Center',
    
    // Styling
    background: '#FFFFFF',
    border: {
      color: '#D3D3D3',
      width: 1
    },
    
    // Text Style
    textStyle: {
      color: '#424242',
      fontFamily: 'Segoe UI',
      size: '12px'
    },
    
    // Shape & Layout
    shapeWidth: 12,
    shapeHeight: 12,
    shapePadding: 8,
    
    // Margins
    margin: {
      left: 10,
      right: 10,
      top: 10,
      bottom: 10
    }
  }
}, '#chart');
```

### Legend with Custom Colors

**TypeScript:**
```typescript
const chart = new AccumulationChart({
  series: [{
    dataSource: data,
    xName: 'x',
    yName: 'y',
    type: 'Pie',
    pointColorMapping: 'color'  // Use 'color' field from data
  }],
  legendSettings: {
    visible: true,
    position: 'Right'
  }
}, '#chart');
```

**Data with colors:**
```typescript
const data = [
  { x: 'Product A', y: 25000, color: '#FF5733' },
  { x: 'Product B', y: 18000, color: '#3366FF' },
  { x: 'Product C', y: 12000, color: '#33CC33' }
];
```

## Common Legend Patterns

### Pattern 1: Top Legend with Horizontal Layout
Dashboard style with legend at top:

```typescript
legendSettings: {
  position: 'Top',
  alignment: 'Center'
}
```

### Pattern 2: Right Legend (Default Professional)
Standard business chart layout:

```typescript
legendSettings: {
  position: 'Right',
  alignment: 'Center'
}
```

### Pattern 3: Hidden Legend
For charts with data labels (legend not needed):

```typescript
legendSettings: {
  visible: false
}
```

### Pattern 4: Custom Styled Legend
Brand-aligned legend styling:

```typescript
legendSettings: {
  visible: true,
  position: 'Right',
  background: '#F8F9FA',
  border: { color: '#007AFF', width: 2 },
  textStyle: {
    color: '#007AFF',
    fontSize: '13px',
    fontWeight: '500'
  }
}
```

## API Reference Links

- [LegendSettingsModel Properties](https://ej2.syncfusion.com/documentation/api/accumulation-chart/legendsettingsmodel/)
- [AccumulationLegend Module](https://ej2.syncfusion.com/documentation/api/accumulation-chart/accumulationlegend/)
- [legendClick Event](https://ej2.syncfusion.com/documentation/api/accumulation-chart/#legendclick)
- [legendRender Event](https://ej2.syncfusion.com/documentation/api/accumulation-chart/#legendrender)

## Troubleshooting

**Issue: Legend not appearing**
- Solution: Set `legendSettings.visible = true`
- Ensure `AccumulationLegend` is injected
- See [AccumulationLegend API](https://ej2.syncfusion.com/documentation/api/accumulation-chart/accumulationlegend/)

**Issue: Legend overlapping chart**
- Solution: Adjust `margin` property
- Use `position: 'Top'` or `position: 'Bottom'` to move legend

**Issue: Legend text too small/large**
- Solution: Adjust `textStyle.fontSize` property
- Use `maximumLabelWidth` for long text wrapping

**Issue: Legend shows wrong data**
- Solution: Verify `x` field contains distinct values
- Check data source xName mapping is correct
