# Visualization Features in Maps

## Table of Contents
- [Bubble Visualization](#bubble-visualization)
- [Polygon Rendering](#polygon-rendering)
- [Color Mapping](#color-mapping)
- [Data Labels](#data-labels)
- [Combining Visualizations](#combining-visualizations)

## Bubble Visualization

Bubbles represent data points where **bubble size indicates magnitude** or relative value.

### Basic Bubble Setup

```typescript
const bubbleData = [
    { latitude: 37.6872, longitude: -122.0808, population: 873965 },
    { latitude: 34.0522, longitude: -118.2437, population: 3979576 },
    { latitude: 40.7128, longitude: -74.0060, population: 8398748 }
];

const mapConfig = {
    layers: [
        {
            urlTemplate: 'your URL link',
            bubbleSettings: [{
                visible: true,
                dataSource: bubbleData,
                valuePath: 'population',         // Property for bubble size
                colorValuePath: 'population'    // Property for bubble color
            }]
        }
    ]
};
```

### Bubble Sizing

```typescript
bubbleSettings:[{
    visible: true,
    dataSource: bubbleData,
    valuePath: 'population',
    // Size mapping
    minRadius: 5,     // Smallest bubble in pixels
    maxRadius: 30,    // Largest bubble in pixels
    opacity: 0.8,
    border: {
        color: '#000',
        width: 1
    }
}]
```

**How sizing works:**
- Data values are normalized to the min/max radius range
- Smallest value → minRadius
- Largest value → maxRadius
- Intermediate values distributed proportionally

### Bubble Colors

```typescript
bubbleSettings: [{
    visible: true,
    dataSource: bubbleData,
    valuePath: 'population',
    colorValuePath: 'population',  // Use same property or different
    colorMapping: [
        { value: 1000000, color: '#FFF5E6' },  // Light: <1M
        { value: 5000000, color: '#FF9999' },  // Medium: <5M
        { value: 10000000, color: '#FF0000' }  // Dark: >5M
    ]
}]
```

### Multiple Bubble Groups

Different bubbles for different data series:

```typescript
const layer1 = {
    markerSettings: {
        dataSource: populationData
    }
};

const layer2 = {
    bubbleSettings: [{
        visible: true,
        dataSource: incomeData,
        valuePath: 'average_income'
    }]
};

const map = new Maps({
layers: [
    {
        urlTemplate: '...',
        markerSettings: [...],
        bubbleSettings: [...]
    }
]
});
```

### Bubble Interactions

```typescript
bubbleSettings: [{
    visible: true,
    dataSource: bubbleData,
    valuePath: 'population',
    selectionSettings: {
        enable: true,
        fill: '#FFFF00',  // Highlight color
        opacity: 0.5
    },
    tooltipSettings: {
        visible: true,
        valuePath: 'population'
    }
}]
```

## Polygon Rendering

Polygons fill geographical regions or shapes.

### Basic Polygon Setup

```typescript
import { Maps } from '@syncfusion/ej2-maps';
import * as usaGeometry from 'path/to/usa.json';  // GeoJSON

const map = new Maps({
    layers: [
        {
            shapeData: usaGeometry,
            shapeSettings: {
                fill: 'rgba(100, 150, 200, 0.5)',
                border: {
                    color: '#000',
                    width: 1
                }
            }
        }
    ]
});
```

### Styling Polygons

```typescript
shapeSettings: {
    fill: '#8DD3C7',
    border: {
        color: '#333333',
        width: 2
    },
    opacity: 0.8
}
```

### Data-Driven Polygon Colors

Assign colors based on data:

```typescript
const stateData = [
    { name: 'California', gdp: 3000 },
    { name: 'Texas', gdp: 1800 },
    { name: 'Florida', gdp: 1100 }
];

const layer = {
    shapeData: usaGeometry,
    dataSource: stateData,
    shapeDataPath: 'name',  // Which property in data
    shapePropertyPath: 'properties.name',  // Which property in GeoJSON
    shapeSettings: {
        colorValuePath: 'gdp'  // Use this data property for color
    }
};
```

## Color Mapping

Color mapping visualizes data variation across geographical regions.

### Range Color Mapping

Assign color ranges for continuous data:

```typescript
const map = new Maps({
    layers: [
        {
            shapeData: usaGeometry,
            dataSource: populationData,
            shapeDataPath: 'name',
            shapePropertyPath: 'properties.name',
            shapeSettings: {
                colorMapping: [
                    {
                        from: 0,
                        to: 1000000,
                        color: '#FFF5E6'  // Light yellow
                    },
                    {
                        from: 1000000,
                        to: 5000000,
                        color: '#FFD700'  // Gold
                    },
                    {
                        from: 5000000,
                        to: 10000000,
                        color: '#FF8C00'  // Dark orange
                    },
                    {
                        from: 10000000,
                        to: Infinity,
                        color: '#FF4500'  // Red-orange
                    }
                ]
            }
        }
    ]
});
```

**Use range mapping when:**
- Data has continuous numerical ranges
- You want smooth visual gradients
- Showing metrics like population, income, temperature

### Equal Color Mapping

Assign colors to specific data values:

```typescript
colorMapping: [
    { value: 'High', color: '#FF0000' },
    { value: 'Medium', color: '#FFFF00' },
    { value: 'Low', color: '#00FF00' },
    { value: 'Unknown', color: '#CCCCCC' }
]
```

**Use equal mapping when:**
- Data has categorical values
- Each category should have distinct color
- Examples: risk levels, priority levels, status categories

### Desaturation Color Mapping

Gradually shift colors based on data:

```typescript
shapeSettings: {
    colorMapping: [
        {
            from: 0,
            to: 100,
            color: '#A4D4AE',  // Light green
            minOpacity: 0.2,
            maxOpacity: 1.0
        }
    ],
    desaturation: true
}
```

**Effect:** Adds variation through opacity and desaturation rather than hue.

## Data Labels

Display text labels on map elements.

### Marker Data Labels

```typescript
markerSettings: {
    dataSource: locationData,
    dataLabelSettings: {
        visible: true,
        labelPath: 'name',  // Which property to show
        border: {
            color: '#000',
            width: 1
        },
        background: '#FFFFFF'
    }
}
```

### Polygon Data Labels

```typescript
datalabelSettings: {
    visible: true,
    labelPath: 'properties.name',  // GeoJSON property path
    textStyle: {
        fontFamily: 'Arial',
        size: '14px',
        color: '#000000'
    }
}
```

## Combining Visualizations

### Polygons + Data Labels

Show regional data with region names:

```typescript
{
    shapeData: usaGeometry,
    dataSource: stateData,
    shapeSettings: {
        colorValuePath: 'population'
    },
    dataLabelSettings: {
        visible: true,
        labelPath: 'properties.name'
    }
}
```

### Base Map + Bubbles + Markers

Layer multiple visualization types:

```typescript
const map = new Maps({
    layers: [
        // Base map
        {
            urlTemplate: 'your URL link',
        },
        // Regional color mapping
        {
            shapeData: usaGeometry,
            dataSource: regionalData,
            shapeSettings: {
                colorValuePath: 'metric'
            }
        },
        // Specific locations with markers
        {
            markerSettings: {
                dataSource: importantCities
            }
        }
    ]
});
```

### Bubbles + Color Mapping

Combine bubble size and color for multivariate display:

```typescript
bubbleSettings:[ {
    visible: true,
    dataSource: complexData,
    valuePath: 'metric1',  // Size shows metric1
    colorValuePath: 'metric2',  // Color shows metric2
    colorMapping: [
        { from: 0, to: 50, color: '#0000FF' },
        { from: 50, to: 100, color: '#FF0000' }
    ]
}]
```

**Result:** Bubble size and color represent different metrics—powerful for multidimensional data.

## Common Patterns

### Pattern 1: Heat Map

Show intensity variation across regions:

```typescript
// Use color mapping with gradient
shapeSettings: {
    colorMapping: [
        { from: 0, to: 25, color: '#E8F4F8' },
        { from: 25, to: 50, color: '#7FD4E8' },
        { from: 50, to: 75, color: '#3FA9D6' },
        { from: 75, to: 100, color: '#004B87' }
    ]
}
```

### Pattern 2: Comparative Visualization

Show two metrics with bubbles (size) and color:

```typescript
bubbleSettings: [{
    visible: true,
    dataSource: metrics,
    valuePath: 'users',  // Size = user count
    colorValuePath: 'engagement',  // Color = engagement level
    // Bubbles: larger = more users, redder = more engagement
}]
```

### Pattern 3: Risk/Status Dashboard

Categorical colors for quick assessment:

```typescript
shapeSettings: {
    colorMapping: [
        { value: 'CRITICAL', color: '#FF0000' },
        { value: 'WARNING', color: '#FF9900' },
        { value: 'OK', color: '#00CC00' }
    ]
}
```

## Performance Tips

- **Large datasets:** Color mapping is efficient for 100+ regions
- **Bubble limits:** 100+ bubbles may slow rendering; consider aggregation
- **Label density:** Reduce label font size or hide labels at low zoom levels
- **Caching:** Reuse GeoJSON objects instead of reloading

## Related API Reference

### BubbleSettings Interface

```typescript
interface BubbleSettings {
    // Visibility
    visible?: boolean;

    // Data
    dataSource?: object[] | DataManager;
    valuePath?: string;
    colorValuePath?: string;

    // Sizing
    minRadius?: number;
    maxRadius?: number;

    // Appearance
    fill?: string;
    opacity?: number;

    // Border
    border?: {
        color: string;
        width: number;
    };

    // Bubble type
    bubbleType?: 'Circle' | 'Square';

    // Color mapping
    colorMapping?: ColorMappingSettingsModel[];

    // Animation
    animationDelay?: number;
    animationDuration?: number;

    // Interaction
    selectionSettings?: SelectionSettingsModel;
    highlightSettings?: HighlightSettingsModel;

    // Tooltip
    tooltipSettings?: TooltipSettingsModel;

    // Query
    query?: Query;
}
```

### ShapeSettings Interface

```typescript
interface ShapeSettings {
    fill?: string;
    strokeColor?: string;
    border?: { color: string; width: number; dashArray?: string };
    opacity?: number;
    colorValuePath?: string;
    colorMapping?: colorMappingsSettings[];
    highlightColor?: string;
    selectionColor?: string;
}
```

### colorMappingsSettings Interface

```typescript
interface colorMappingsSettings {
    // For range mapping
    from?: number;
    to?: number;
    
    // For categorical mapping
    value?: string | number;
    
    // Color and label
    color?: string;
    label?: string;
    
    // Opacity control
    minOpacity?: number;
    maxOpacity?: number;
}
```

### Data Label Settings

```typescript
interface DataLabelSettings {
    visible: boolean;
    labelPath?: string;
    template?: string;
    textStyle?: FontSettings;
    background?: string;
    border?: { color: string; width: number };
    opacity?: number;
    intersectionAction?: 'Hide' | 'Trim' | 'None' | 'WrapByWord';
}
```

### Bubble and Shape Events

| Event | Args | Description |
|-------|------|-------------|
| `bubbleClick` | IBubbleClickEventArgs | Bubble is clicked |
| `bubbleRender` | IBubbleRenderingEventArgs | Bubble is rendering |
| `shapeRendering` | IShapeRenderingEventArgs | Shape is rendering |
| `shapeSelected` | IShapeSelectedEventArgs | Shape is selected |

### Color Mapping Examples

**Range-based mapping:**
```typescript
colorMappings: [
    { from: 0, to: 1000000, color: '#FFF5E6', label: '< 1M' },
    { from: 1000000, to: 5000000, color: '#FFD700', label: '1M - 5M' },
    { from: 5000000, to: Infinity, color: '#FF4500', label: '> 5M' }
]
```

**Categorical mapping:**
```typescript
colorMappings: [
    { value: 'High', color: '#FF0000' },
    { value: 'Medium', color: '#FFFF00' },
    { value: 'Low', color: '#00FF00' }
]
```

**For complete Bubble and Shape APIs, see:** [api-reference-guide.md](api-reference-guide.md)

## Next Steps

- Add legends to explain colors and symbols (see legends-and-annotations.md)
- Configure tooltips for data details (see markers-and-navigation.md)
- Apply themes and custom styling (see customization-and-persistence.md)
- Review complete visualization APIs (see api-reference-guide.md)
