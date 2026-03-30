# Legends & Annotations in Maps

## Table of Contents

- [Legends Overview](#legends-overview)
- [Legend Configuration](#legend-configuration)
    - [Basic Legend](#basic-legend)
    - [Legend Positioning](#legend-positioning)
    - [Legend Modes](#legend-modes)
        - [List Mode](#list-mode)
        - [Interactive Mode](#interactive-mode)
- [Legend for Color Mapping](#legend-for-color-mapping)
- [Legend for Bubbles](#legend-for-bubbles)
- [Annotations](#annotations)
    - [Basic Annotation](#basic-annotation)
    - [Annotations with Coordinates](#annotations-with-coordinates)
    - [Custom Annotation Templates](#custom-annotation-templates)
    - [Styling Annotations](#styling-annotations)
- [Tooltips](#tooltips)
    - [Marker Tooltips](#marker-tooltips)
    - [Bubble Tooltips](#bubble-tooltips)
    - [Shape/Polygon Tooltips](#shapepolygon-tooltips)
    - [Custom Tooltip Styling](#custom-tooltip-styling)
- [Common Patterns](#common-patterns)
- [Combining Elements](#combining-elements)
- [Related API Reference](#related-api-reference)
- [Tips for Effective Legends and Annotations](#tips-for-effective-legends-and-annotations)
- [Next Steps](#next-steps)

## Legends Overview

Legends explain the meaning of map colors, sizes, and symbols, helping users interpret the data.

## Legend Configuration

### Basic Legend

```typescript
const mapConfig = {
    legendSettings: {
        visible: true,
        mode: 'Default',  // 'Default' or 'Interactive'
        position: 'Left',  
        title: { text: 'Legend'}
    },
    layers: []  // Layer config here
};
```

### Legend Positioning

```typescript
legendSettings: {
    visible: true,
    position: 'Right',  // Corner positions
    background: '#FFFFFF',    // Legend background color
    border: {
        color: '#000000',
        width: 1
    },
    width: '200px',
    height: 'auto'
}
```

**Positions:**  'Top' , 'Left' , 'Bottom' , 'Right' ';
### Legend Modes

#### List Mode
Displays as a static list:

```typescript
legendSettings: {
    visible: true,
    mode: 'Default'
}
```

#### Interactive Mode
Users can click legend items to toggle visibility:

```typescript
legendSettings: {
    visible: true,
    mode: 'Interactive'
}
```

## Legend for Color Mapping

Automatically generate legend from color mappings:

```typescript
const mapConfig = {
    legendSettings: {
        visible: true,
        mode: 'Default'
    },
    layers: [
        {
            shapeData: usaGeometry,
            dataSource: stateData,
            shapeSettings: {
                colorMapping: [
                    { from: 0, to: 1000000, color: '#FFF5E6' },
                    { from: 1000000, to: 5000000, color: '#FF9999' },
                    { from: 5000000, to: 10000000, color: '#FF0000' }
                ]
            }
        }
    ]
};
```

**How it works:** The legend automatically creates entries for each color range.

## Legend for Bubbles

Show size and color scales for bubble visualization:

```typescript
bubbleSettings: [{
    dataSource: bubbleData,
    valuePath: 'population',
    colorValuePath: 'population',
    minRadius: 5,
    maxRadius: 30
}],
legendSettings: {
    visible: true,
     title:{text:'Population'}
}
```

## Annotations

Annotations add custom text, shapes, or HTML content to the map.
The supported values are Center, Far, Near, and None

### Basic Annotation

```typescript
annotations: [
    {
        content: '<div style="background: white; padding: 10px;">San Francisco</div>',
        x: '10%',
        y: '20%',
        verticalAlignment: 'Center',
        horizontalAlignment: 'Center'
    }
]
```


### Styling Annotations

```typescript
annotations: [
    {
        content: '<div style="background: #FFFFCC; border: 1px solid #000; padding: 8px; border-radius: 4px;">' +
                 'Important Location' +
                 '</div>',
        x: '50%',
        y: '30%',
        zIndex: '1'
    }
]
```

## Tooltips

Display information on hover over map elements.

### Marker Tooltips

```typescript
markerSettings: {
    dataSource: locationData,
    tooltipSettings: {
        visible: true,
        valuePath: 'name',  // Property to display
        template: '<div>${name}<br/>${city}</div>'
    }
}
```

### Bubble Tooltips

```typescript
bubbleSettings: {
    dataSource: bubbleData,
    valuePath: 'population',
    tooltipSettings: {
        visible: true,
        valuePath: 'population',
        template: '<div>${name}: ${population}</div>'
    }
}
```

### Shape/Polygon Tooltips

```typescript
shapeSettings: {
    colorValuePath: 'metric'
},
tooltipSettings: {
    visible: true,
    valuePath: 'name',
    template: '<div>${name}: ${metric}</div>'
}
```

### Custom Tooltip Styling

```typescript
tooltipSettings: {
    visible: true,
    valuePath: 'name',
    textStyle: {
        fontFamily: 'Arial',
        size: '14px',
        color: '#000000'
    }
}
```

## Common Patterns

### Pattern 1: Data Legend with Color Ranges

Show what each color represents:

```typescript
const map = new Maps({
    legendSettings: {
        visible: true,
        mode: 'List',
        position: 'BottomLeft',
        title: 'Population Density'
    },
    layers: [
        {
            shapeData: usaGeometry,
            dataSource: populationData,
            shapeSettings: {
                colorMappings: [
                    { from: 0, to: 1000000, color: '#FFF5E6', label: 'Less than 1M' },
                    { from: 1000000, to: 5000000, color: '#FF9999', label: '1M - 5M' },
                    { from: 5000000, to: Infinity, color: '#FF0000', label: 'More than 5M' }
                ]
            }
        }
    ]
});
```

### Pattern 2: Tooltip with Formatted Data

Show detailed information on hover:

```typescript
layers: [{
tooltipSettings: {
    visible: true,
    template: '<div class="tooltip">' +
              '<strong>${state}</strong><br/>' +
              'Population: ${population:N0}<br/>' +
              'Area: ${area} sq mi' +
              '</div>'
}
}]
```

### Pattern 3: Interactive Legend

Allow users to toggle layers by clicking legend items:

```typescript
legendSettings: {
    visible: true,
    mode: 'Interactive'
}

## Combining Elements

### Full Example: Map with Legend, Tooltips, and Annotations

```typescript

import { Maps, Annotations } from '@syncfusion/ej2-maps';
Maps.Inject(Annotations);
const map = new Maps({
    center: { latitude: 37.6872, longitude: -122.0808 },
    
    // Legend configuration
    legendSettings: {
        visible: true,
        mode: 'Default',
        position: 'Top',
        title: {text: 'Data Legend'}
    },
    
    // Annotations for special markers
    annotations: [
        {
            content: '<div style="background: gold; padding: 5px;">★ Headquarters</div>',
         
        }
    ],
    
    layers: [
        {
            urlTemplate: 'https://tile.openstreetmap.org/level/tileX/tileY.png',
          
            
            // Shape with color mapping
            shapeData: usaGeometry,
            dataSource: stateData,
            shapeSettings: {
                colorMapping: [
                    { from: 0, to: 50, color: '#FFF5E6' },
                    { from: 50, to: 100, color: '#FF0000' }
                ]
            },
            
            // Tooltips on hover
            tooltipSettings: {
                visible: true,
                template: '<div>${name}: ${value}%</div>'
            },
            
            // Markers with tooltips
            markerSettings: {
                dataSource: citiesData,
                tooltipSettings: {
                    visible: true,
                    valuePath: 'name'
                }
            }
        }
    ]
});

map.appendTo('#container');
```

## Related API Reference

### LegendSettings Interface

```typescript
interface LegendSettings {
    // Visibility
    visible?: boolean;

    // Mode
    mode?: 'Default' | 'Interactive';

    // Positioning
    position?: 'Top' | 'Bottom' | 'Left' | 'Right' | 'Float';
    alignment?: 'Near' | 'Center' | 'Far';
    orientation?: 'Horizontal' | 'Vertical';
    location?: { x: number; y: number };

    // Dimensions
    width?: string;
    height?: string;

    // Appearance
    background?: string;
    fill?: string;
    opacity?: number;

    // Border
    border?: {
        color: string;
        width: number;
    };

    // Shape
    shape?: string;
    shapeWidth?: number;
    shapeHeight?: number;
    shapePadding?: number;
    shapeBorder?: {
        color: string;
        width: number;
    };

    // Labels
    valuePath?: string;
    labelDisplayMode?: 'None' | 'Trim' | 'Hide';
    labelPosition?: 'Before' | 'After';

    // Text styling
    textStyle?: FontSettings;

    // Title
    title?: {
        text: string;
    };
    titleStyle?: FontSettings;

    // Behavior
    toggleVisibility?: boolean;
    removeDuplicateLegend?: boolean;
    showLegendPath?: string;
    useMarkerShape?: boolean;
    invertedPointer?: boolean;

    // Advanced
    toggleLegendSettings?: ToggleLegendSettings;
    type?: string;
}
```

### Annotation Interface

```typescript
interface Annotation {
    content: string;
    x?: string;
    y?: string;
    verticalAlignment?: 'None' | 'Near' | 'Center' | 'Far';
    horizontalAlignment?: 'None' | 'Near' | 'Center' | 'Far';
    zIndex?: string;
}
```

### TooltipSettings Interface

```typescript
interface TooltipSettings {
    // Visibility
    visible?: boolean;

    // Content
    valuePath?: string;
    format?: string;
    template?: string | Function;

    // Styling
    textStyle?: FontSettings;
    fill?: string;
    border?: {
        color: string;
        width: number;
    };

    // Behavior
    duration?: number;
}
```

### Legend Events

| Event | Args | Description |
|-------|------|-------------|
| `legendRender` | ILegendRenderingEventArgs | Legend is rendering |
| `annotationRender` | IAnnotationRenderingEventArgs | Annotation is rendering |
| `tooltipRender` | ITooltipRenderEventArgs | Tooltip is rendering |

### Event Arguments

```typescript
// Legend rendering event
interface ILegendRenderingEventArgs {
    name: string;
    label: string;
    fill: string;
}

// Tooltip rendering event
interface ITooltipRenderEventArgs {
    name: string;
    tooltip: {
        content: string;
        textStyle?: FontSettings;
    };
    data: object;
}

// Annotation rendering event
interface IAnnotationRenderingEventArgs {
    name: string;
    annotation: Annotation;
    eventTarget: HTMLElement;
}
```

### Legend Positioning

**Available Positions:**
```typescript
type LegendPosition = 
    | 'Top'
    | 'Right'
    | 'Bottom'
    | 'Left';
```

**For complete Legend, Annotation, and Tooltip APIs, see:** [api-reference-guide.md](api-reference-guide.md)

## Tips for Effective Legends and Annotations

1. **Keep legends simple:** Too many entries confuse users
2. **Place strategically:** Position legends where they don't block important data
3. **Use consistent styling:** Match colors between legend and map
4. **Annotations for emphasis:** Use for important locations or markers
5. **Tooltip content:** Show relevant data without overwhelming users

## Next Steps

- Configure user interactions and events (see map-providers-and-selection.md)
- Customize styling and themes (see customization-and-persistence.md)
- Optimize performance for large maps (see troubleshooting.md)
- Review complete legend and annotation APIs (see api-reference-guide.md)
