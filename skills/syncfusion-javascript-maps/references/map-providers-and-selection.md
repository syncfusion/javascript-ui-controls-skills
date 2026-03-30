# Map Providers & Selection in Maps

## Table of Contents

- [Map Providers Overview](#map-providers-overview)
    - [OpenStreetMap (OSM)](#openstreetmap-osm)
    - [Bing Maps](#bing-maps)
        - [Bing Map Types](#bing-map-types)
        - [Getting a Bing Maps Key](#getting-a-bing-maps-key)
- [User Interactions](#user-interactions)
    - [Mouse Interactions](#mouse-interactions)
    - [Selection Features](#selection-features)
        - [Marker Selection](#marker-selection)
        - [Polygon/Shape Selection](#polygonshape-selection)
        - [Bubble Selection](#bubble-selection)
    - [Selection Events](#selection-events)
    - [Multi-Selection](#multi-selection)
- [Events & Interactions](#events--interactions)
    - [Map Events](#map-events)
    - [Layer Events](#layer-events)
    - [Selection & Click Events](#selection--click-events)
- [Common Patterns](#common-patterns)
- [Provider Comparison](#provider-comparison)
- [Related API Reference](#related-api-reference)
- [Tips for User Interactions](#tips-for-user-interactions)
- [Next Steps](#next-steps)

## Map Providers Overview

Map providers are different source services for the map background.

## OpenStreetMap (OSM)

Free, open-source map provider.

```typescript
import { Maps } from '@syncfusion/ej2-maps';
let map: Maps = new Maps({
        layers: [
            {
                urlTemplate: 'https://tile.openstreetmap.org/level/tileX/tileY.png',
            }
        ]
  
});
map.appendTo('#element');
```

**Advantages:**
- ✅ Free (no API key required)
- ✅ Open source and community-driven
- ✅ Good coverage worldwide
- ✅ No usage limits

**Disadvantages:**
- ⚠️ Quality varies by region
- ⚠️ Simpler UI than commercial providers

**Use when:** Building public projects, no budget for API costs, need full control.

## Bing Maps

Microsoft's mapping service with multiple tile styles.

```typescript
import { Maps } from '@syncfusion/ej2-maps';
let map: Maps = new Maps({
 
    layers: [
        {
            urlTemplate: 'https://ecn.t3.tiles.virtualearth.net/tiles/r{quadkey}.jpeg?g=1&mkt=en-US&shading=hill&stl=H&key=YOUR_BING_MAPS_KEY'
        }
    ]

});
map.appendTo('#element');

```

### Bing Map Types

```typescript
### Bing Map Types
// Aerial (satellite) view
bingMapType: 'Aerial'

// Road view (default)
bingMapType: 'Road'

// Aerial with labels
bingMapType: 'AerialWithLabel'

// Dark theme
bingMapType: 'CanvasDark'

// Light theme
bingMapType: 'CanvasLight'

// Grayscale theme
bingMapType: 'CanvasGray'
```

**Getting a Bing Maps Key:**
1. Visit [Bing Maps Dev Center](https://www.bingmapsportal.com/)
2. Create account and application
3. Generate API key
4. Add to maps configuration

**Advantages:**
- ✅ High-quality satellite imagery
- ✅ Multiple tile styles
- ✅ Good for enterprise use
- ✅ Reliable and well-maintained

**Disadvantages:**
- ⚠️ Requires API key
- ⚠️ Usage-based pricing

**Use when:** Need high-quality satellite views, enterprise application, willing to pay for services.

## User Interactions

### Mouse Interactions

Users can naturally interact with the map:

```typescript
const map = new Maps({
    // Enable/disable interactions
    zoomSettings: {
        enable: true
    },
    navigationLineSettings: [],
    layers: [/* ... */]
});
```

**Default interactions:**
- **Zoom:** Mouse wheel to zoom in/out
- **Pan:** Click and drag to move map
- **Click:** Select markers or regions

### Selection Features

Enable users to select map elements.

#### Marker Selection

```typescript
markerSettings: {
    dataSource: locationData,
    selectionSettings: {
        enable: true,
        fill: '#FFFF00',  // Highlight color
        opacity: 0.5,     // Unselected opacity
        border: {
            color: '#FF0000',
            width: 2
        }
    }
}
```

**Effect:** Users can click markers; selected marker highlights in yellow, others dim.

#### Polygon/Shape Selection

```typescript
shapeSettings: {
    colorValuePath: 'metric',
    selectionSettings: {
        enable: true,
        fill: '#FFFF00',
        border: {
            color: '#FF0000',
            width: 2
        }
    }
}
```

#### Bubble Selection

```typescript
bubbleSettings: {
    dataSource: bubbleData,
    valuePath: 'population',
    selectionSettings: {
        enable: true,
        fill: '#00FF00',
        border: { color: '#FF0000', width: 2 }
    }
}
```

### Selection Events

Respond to user selections:

```typescript
const map = new Maps({
    // Fired when marker is clicked/selected
    markerClick: (args: IMarkerClickEventArgs) => {
        console.log('Selected marker:', args.data);
    },
    
    // Fired when shape/polygon is clicked
    shapeSelected: (args: IShapeSelectedEventArgs) => {
        console.log('Selected shape:', args.shapeData);
    },
    
    // Fired when bubble is clicked
    bubbleClick: (args: IBubbleClickEventArgs) => {
        console.log('Selected bubble:', args.data);
    },
    
    layers: [/* ... */]
});
```

### Multi-Selection

Allow selecting multiple elements:

```typescript
// This depends on map implementation
// Some versions support Ctrl+Click for multi-select
// Check your Syncfusion version documentation
```

## Events & Interactions

### Map Events

```typescript
const map = new Maps({
    // Map is fully loaded
    load: (args: ILoadEventArgs) => {
        console.log('Map loaded');
    },
    
    // Zoom level changes
    zoomEnd: (args: IZoomEventArgs) => {
        console.log('New zoom level:', map.zoomSettings.zoomFactor);
    },
    
    // Pan/drag completes
    panEnd: (args: IPanEventArgs) => {
        console.log('Pan ended');
    },
    
    // Mouse moves over map
    mouseMove: (args: IMouseEventArgs) => {
        console.log('Mouse at:', args.pageX, args.pageY);
    },
    
    // Tooltip is rendered
    tooltipRender: (args: ITooltipRenderEventArgs) => {
        // Customize tooltip content
        args.tooltip.content = customContent;
    },
    
    layers: [/* ... */]
});
```

### Layer Events

```typescript
layerRender: (args: ILayerRenderEventArgs) => {
    console.log('Layer rendered:', args.layer);
}
```

### Selection & Click Events

```typescript
markerClick: (args: IMarkerClickEventArgs) => {
    console.log('Marker clicked');
    console.log('Latitude:', args.data.latitude);
    console.log('Longitude:', args.data.longitude);
}

shapeSelected: (args: IShapeSelectedEventArgs) => {
    console.log('Shape selected:', args.shapeData);
}

bubbleClick: (args: IBubbleClickEventArgs) => {
    console.log('Bubble clicked:', args.data);
}
```

## Common Patterns

### Pattern 1: Clickable Markers with Details

Show information panel when user clicks marker:

```typescript
const map = new Maps({
    markerClick: (args) => {
        const marker = args.data;
        showDetailPanel({
            name: marker.name,
            lat: marker.latitude,
            lng: marker.longitude
        });
    },
    
    layers: [
        {
            markerSettings: {
                dataSource: locations,
                selectionSettings: {
                    enable: true,
                    fill: '#FFFF00'
                }
            }
        }
    ]
});

function showDetailPanel(marker: any) {
    document.getElementById('details').innerHTML = `
        <h3>${marker.name}</h3>
        <p>Lat: ${marker.lat}</p>
        <p>Lng: ${marker.lng}</p>
    `;
}
```

### Pattern 2: Zoom to Selected Area

Focus map on clicked shape:

```typescript
shapeSelected: (args: IShapeSelectedEventArgs) => {
    const bounds = calculateBounds(args.shapeData);
    map.centerPosition = bounds.center;
    map.zoomSettings.zoomFactor = 5;
    map.refresh();
}
```

### Pattern 3: Filter on Selection

Update data when user selects region:

```typescript
shapeSelected: (args) => {
    const selectedRegion = args.shapeData.properties.name;
    
    // Filter data for this region
    const filteredData = dataSource.filter(d => d.region === selectedRegion);
    
    // Update legend and display
    updateUI(filteredData);
}
```

### Pattern 4: Highlight Related Elements

Show connections when selecting an element:

```typescript
markerClick: (args) => {
    const selectedMarker = args.data;
    
    // Hide all navigation lines
    map.navigationLineSettings.forEach(line => {
        line.opacity = 0.1;
    });
    
    // Highlight related routes
    map.navigationLineSettings.forEach(line => {
        if (line.includes(selectedMarker.latitude)) {
            line.opacity = 1.0;
        }
    });
    
    map.refresh();
}
```

## Provider Comparison

| Feature | OSM | Bing |
|---------|-----|------|
| **Cost** | Free | Paid |
| **API Key Required** | No | Yes |
| **Map Quality** | Good | Excellent |
| **Satellite Imagery** | Limited | Full |
| **Support** | Community | Microsoft |
| **Best For** | Open projects | Enterprise |

## Related API Reference

### ZoomSettings Interface

```typescript
interface ZoomSettings {
    enable?: boolean;      // Enable/disable zoom (default: true)
    zoomFactor?: number;   // Current zoom level
    minZoom?: number;      // Minimum zoom level (default: 1)
    maxZoom?: number;      // Maximum zoom level (default: 20)
    enablePanning?: boolean;
    mouseWheelZoom?: boolean;
    doubleClickZoom?: boolean;
    pinchZooming?: boolean;
    toolbarSettings?: ZoomToolbarSettings;
    animationDuration?: number; // Animation duration in milliseconds
    enableSelectionZooming?: boolean;
    enableToolbar?: boolean;
    zoomOnClick?: boolean;
}
```

### SelectionSettings Interface

```typescript
interface SelectionSettings {
    // Enable/disable
    enable: boolean;                // Enable selection

    enableMultiSelect: boolean;     // Enable selection of multiple shapes in maps.
    
    // Appearance when selected
    fill?: string;                  // Fill color when selected
    
    // Appearance when not selected
    opacity?: number;               // Opacity of unselected items
    
    // Interaction
    border?: {
        color: string;
        opacity: number;
        width: number;
    };
}
```

### HighlightSettings Interface

```typescript
interface HighlightSettings {
    // Enable/disable
    enable: boolean;                // Enable highlighting
    
    // Appearance
    fill?: string;                  // Highlight fill color
    opacity?: number;               // Highlight opacity
    
    // Border
    border?: {
        color: string;
        opacity: number;
        width: number;
    };
}
```

### Interaction Events

| Event | Args | Description |
|-------|------|-------------|
| `markerClick` | IMarkerClickEventArgs | Marker is clicked |
| `shapeSelected` | IShapeSelectedEventArgs | Shape is selected |
| `bubbleClick` | IBubbleClickEventArgs | Bubble is clicked |
| `zoomEnd` | IMapZoomEventArgs | Zoom completes |
| `panEnd` | IMapPanEventArgs | Pan completes |
| `mouseMove` | IMouseEventArgs | Mouse moves on map |
| `layerMouseMove` | IMouseEventArgs | Mouse moves on layer |

### Event Arguments

```typescript
// Marker click event
interface IMarkerClickEventArgs {
    // Event name
    name?: string;

    // Control
    cancel?: boolean;

    // Maps instance
    maps?: Maps;

    // Marker instance
    marker?: MarkerSettingsModel;

    // Marker data
    data?: object;
    value?: string;

    // Position
    latitude?: number;
    longitude?: number;

    // Mouse position
    x?: number;
    y?: number;

    // Target element
    target?: string;

    // Selection state
    isShapeSelected?: boolean;
}

// Shape selection event
interface IShapeSelectedEventArgs {
    // Event name
    name?: string;

    // Control
    cancel?: boolean;

    // Maps instance
    maps?: Maps;

    // Selected shape data
    data?: object;

    // GeoJSON shape
    shapeData?: object;

    // Multiple selection
    shapeDataCollection?: object;

    // Styling
    fill?: string;
    opacity?: number;
    border?: {
        color: string;
        width: number;
    };

    // Target element
    target?: string;
}

// Zoom event
interface IMapZoomEventArgs {
    // Event name
    name?: string;

    // Control
    cancel?: boolean;

    // Maps instance
    maps?: Maps;

    // Zoom values
    previousZoomFactor?: number;
    zoomFactor?: number;

    // Mouse position
    x?: number;
    y?: number;

    // Target element
    target?: string;

    // Visible bounds
    minLatitude?: number;
    maxLatitude?: number;
    minLongitude?: number;
    maxLongitude?: number;
}

// Pan event
interface IMapPanEventArgs {
    // Event name
    name?: string;

    // Control
    cancel?: boolean;

    // Maps instance
    maps?: Maps;

    // Mouse position
    x?: number;
    y?: number;

    // Target element
    target?: string;

    // Visible bounds
    minLatitude?: number;
    maxLatitude?: number;
    minLongitude?: number;
    maxLongitude?: number;
}
```

### Common Selection Patterns

**Enable marker selection:**
```typescript
markerSettings: {
    dataSource: markers,
    selectionSettings: {
        enable: true,
        fill: '#FFFF00'
    }
}
```

**Listen for selection events:**
```typescript
markerClick: (args: IMarkerClickEventArgs) => {
    console.log('Selected:', args.data);
}

shapeSelected: (args: IShapeSelectedEventArgs) => {
    console.log('Shape:', args.shapeData);
}
```

**For complete Provider, Selection, and Event APIs, see:** [api-reference-guide.md](api-reference-guide.md)

## Tips for User Interactions

1. **Provide visual feedback:** Highlight selected elements clearly
2. **Responsive events:** Handle clicks and selections immediately
3. **Clear tooltips:** Show relevant information on hover
4. **Intuitive controls:** Use standard zoom/pan interactions
5. **Performance:** Limit event handlers that run on every mouse move

## Next Steps

- Customize map styling and themes (see customization-and-persistence.md)
- Add state persistence for user settings (see customization-and-persistence.md)
- Optimize performance for large datasets (see troubleshooting.md)
- Review complete interaction and provider APIs (see api-reference-guide.md)
