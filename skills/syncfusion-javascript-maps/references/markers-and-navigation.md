# Markers & Navigation in Maps

## Table of Contents
- [Adding Markers](#adding-markers)
- [Marker Customization](#marker-customization)
- [Navigation Lines](#navigation-lines)
- [Interactive Features](#interactive-features)

**Note:** Configuration syntax is identical for TypeScript and JavaScript. Examples use object literal format.

## Adding Markers

Markers pinpoint specific locations on the map.

### Basic Marker Setup

```typescript
import { Maps, Marker} from '@syncfusion/ej2-maps';
Maps.Inject(Marker);
let map: Maps = new Maps({
    layers: [
        {
            urlTemplate: 'your URL link',
            markerSettings: [{
                dataSource: [
                    { latitude: 37.6872, longitude: -122.0808, name: 'San Francisco' },
                    { latitude: 34.0522, longitude: -118.2437, name: 'Los Angeles' }
                ]
            }]
        }
    ]
});
map.appendTo('#element');
```

### Marker Data Structure

Each marker object should contain:

```javascript
{
    latitude: number,      // Required: Decimal degrees
    longitude: number,     // Required: Decimal degrees
    name: string,          // Custom property: Display name
    city: string,          // Custom property: Any custom field
    // Additional custom properties allowed
}
```

### Dynamic Marker Addition

```typescript
// Add markers after map initialization
const newMarkers = [
    { latitude: 40.7128, longitude: -74.0060, name: 'New York' }
];

const layer = map.layers[0];
layer.markerSettings[0].dataSource = [
    ...layer.markerSettings[0].dataSource,
    ...newMarkers
];

map.refresh();
```

## Marker Customization

### Marker Appearance

```typescript
markerSettings: [{
    dataSource: locationData,
    // Size
    width: 30,
    height: 30,
    // Styling
    border: {
        color: '#000000',
        width: 2
    },
    fill: '#FF0000',
    opacity: 1.0,
    // Shape
    shape: 'Circle' 
    // 'Circle' | 'Rectangle' | 'Diamond' | 'Cross' | 'Plus' | 'Star' | 'Balloon' | 'Image'
}]
```

### Custom Marker Icons

Use SVG or image-based custom markers:

```typescript
markerSettings: [{
    dataSource: locationData,
    // Template for custom marker
    template: '<svg width="30" height="30" viewBox="0 0 30 30">' +
              '<circle cx="15" cy="15" r="14" fill="#FF5733"/>' +
              '</svg>',
    // Or use image
    imageUrl: 'path/to/marker-icon.png'
}]
```

### Template with Data Binding

Display dynamic content in marker template:

```typescript
markerSettings:[{
    dataSource: locationData,
    template: '<div style="background: #FFF; padding: 5px; border-radius: 3px;">' +
              '${name}</div>'
}]
```

The `${propertyName}` syntax binds to marker data properties.

## Navigation Lines

Draw lines connecting locations to show routes or relationships.

### Creating Navigation Lines

```typescript
navigationLineSettings: [
    {
        visible: true
        latitude: [37.6872, 34.0522],  // Start and end latitudes
        longitude: [-122.0808, -118.2437],  // Start and end longitudes
        color: '#FF0000',
        width: 3,
        dashArray: '5,5'  // Dashed line
    },
    {
        latitude: [34.0522, 40.7128],
        longitude: [-118.2437, -74.0060],
        color: '#0000FF',
        width: 2
    }
]
```

**Structure:**
- `latitude`: Array [startLat, endLat]
- `longitude`: Array [startLong, endLong]
- `color`: Line color (hex or named)
- `width`: Line thickness in pixels
- `dashArray`: Pattern for dashed lines

### Styling Navigation Lines

```typescript
navigationLineSettings: [
    {
        visible: true
        latitude: [37.6872, 34.0522],
        longitude: [-122.0808, -118.2437],
        color: '#FF5733',
        width: 4,
        dashArray: '10,5',  // 10px dash, 5px gap
        opacity: 0.7
    }
]
```

### Dynamic Navigation Lines

Add or update routes programmatically:

```typescript
// Add new navigation line
const newRoute = {
    latitude: [40.7128, 37.6872],
    longitude: [-74.0060, -122.0808],
    color: '#00FF00',
    width: 3
};

map.layers[0].navigationLineSettings.push(newRoute);
map.refresh();
```

## Interactive Features

### Marker Tooltips

Display information on hover:

```typescript
markerSettings: [{
    dataSource: locationData,
    tooltipSettings: {
        visible: true,
        valuePath: 'name',  // Which property to show
        template: '<div>${name}<br>${city}</div>'
    }
}]
```

### Marker Selection

Enable clicking and selecting markers:

```typescript
markerSettings:[ {
    dataSource: locationData,
    selectionSettings: {
        enable: true,
        opacity: 0.5,  // Opacity of unselected markers
        fill: '#FFFF00'  // Color of selected marker
    }
}]
```

### Click Events on Markers

Respond to marker interactions:

```typescript
const map = new Maps({
    markerClick: (args: IMarkerClickEventArgs) => {
        console.log('Clicked marker:', args.data);
        // args.data contains the marker object
    },
    // ... other config
});
```

## Events and Interactions

### Marker Events

Available marker-related events:

```typescript
const map = new Maps({
    // Fired when marker is clicked
    markerClick: (args) => {
        console.log('Marker clicked:', args.data);
    },
    
    // Fired when marker rendering starts
    markerRendering: (args) => {
        // Customize marker appearance before rendering
    },
    
    // Fired when marker cluster is clicked
    markerClusterClick: (args) => {
        console.log('Cluster clicked');
    },
    
    layers: [/* ... */]
});
```

### Navigation Line Events

```typescript
// Fired when navigation line is rendered
navigationLineRendering: (args) => {
    console.log('Navigation line rendering');
}
```

### Handling Marker Interactions

```typescript
const map = new Maps({
    markerClick: (args: IMarkerClickEventArgs) => {
        const marker = args.data;
        console.log(`Marker at ${marker.latitude}, ${marker.longitude}`);
        
        // Example: Show details or navigate
        showMarkerDetails(marker);
    },
    
    layers: [/* ... */]
});

function showMarkerDetails(marker: any) {
    // Display marker information
    console.log(`Name: ${marker.name}`);
}
```

## Common Patterns

### Pattern 1: Route Visualization

Show a journey with markers at waypoints and lines connecting them:

```typescript
import { Maps, Marker, NavigationLine } from '@syncfusion/ej2-maps';

Maps.Inject(Marker, NavigationLine);

const waypoints = [
    { latitude: 37.6872, longitude: -122.0808, name: 'Start' },
    { latitude: 35.1264, longitude: -111.0225, name: 'Stop 1' },
    { latitude: 34.0522, longitude: -118.2437, name: 'End' }
];

const map = new Maps({
  
        layers: [
            {
                urlTemplate: 'your URL link',
    
                markerSettings: [
                    {
                        dataSource: waypoints
                    }
                ],
                navigationLineSettings: [
                    {
                        visible: true,
                        latitude: [37.6872, 35.1264],
                        longitude: [-122.0808, -111.0225]
                    },
                    {
                        visible: true,
                        latitude: [35.1264, 34.0522],
                        longitude: [-111.0225, -118.2437]
                    }
                ]
            }
        ]
    });


map.appendTo('#container');
```

### Pattern 2: Location Clusters

Group nearby markers for performance and clarity:

```typescript
markerSettings: [
    {
        dataSource: manyLocations,
        markerClusterSettings: {
            allowClustering: true,
            labelSettings: {
                visible: true
            }
        }
    }
]
```

### Pattern 3: Interactive Selection

Allow users to select and view marker details:

```typescript
const map = new Maps({
    markerClick: (args) => {
        const selectedMarker = args.data;

        const layer = map.layers[0];

        // Correct access
        layer.markerSettings[0].selectionSettings = {
            enable: true,
            opacity: 0.3
        };

        updateDetailPanel(selectedMarker);
    },

    layers: [
        {
            markerSettings: [
                {
                    dataSource: []
                }
            ]
        }
    ]
});
```

## Performance Tips

- **For 100+ markers:** Consider clustering to reduce DOM elements
- **Lazy loading:** Load markers based on zoom level
- **Template simplicity:** Keep marker templates simple for performance
- **Event throttling:** Debounce zoom/pan events that update markers

## Related API Reference

### MarkerSettings Interface

```typescript
nterface MarkerSettings {
    visible?: boolean;

    // Data
    dataSource?: object[] | DataManager;

    // Position mapping
    latitudeValuePath?: string;
    longitudeValuePath?: string;

    // Appearance
    width?: number;
    height?: number;
    widthValuePath?: string;
    heightValuePath?: string;

    shape?: 'Circle' | 'Rectangle' | 'Diamond' | 'Cross' | 'Plus' | 'Star';
    shapeValuePath?: string;

    fill?: string;
    colorValuePath?: string;
    opacity?: number;
    dashArray?: string;

    // Border
    border?: {
        color: string;
        width: number;
    };

    // Image / Template
    imageUrl?: string;
    imageUrlValuePath?: string;
    template?: string | Function;

    // Offset
    offset?: { x: number; y: number };

    // Animation
    animationDelay?: number;
    animationDuration?: number;

    // Clustering
    clusterSettings?: MarkerClusterSettingsModel;

    // Interaction
    enableDrag?: boolean;
    selectionSettings?: SelectionSettingsModel;
    highlightSettings?: HighlightSettingsModel;

    // Tooltip
    tooltipSettings?: TooltipSettingsModel;

    // Labels & legend
    legendText?: string;
    dataLabelSettings?: DataLabelSettingsModel;

    // Initial selection
    initialMarkerSelection?: InitialMarkerSelectionSettingsModel[];

    // Data query
    query?: Query;
}
```

### NavigationLineSettings Interface

```typescript
interface NavigationLineSettings {
    // Coordinates
    latitude: number[];
    longitude: number[];

    // Appearance
    color?: string;
    width?: number;
    dashArray?: string;

    // Curve
    angle?: number;

    // Arrow styling
    arrowSettings?: ArrowModel;

    // Interaction
    highlightSettings?: HighlightSettingsModel;
    selectionSettings?: SelectionSettingsModel;

    // Visibility
    visible?: boolean;
}
```

### Marker Events

| Event | Args | Description |
|-------|------|-------------|
| `markerClick` | IMarkerClickEventArgs | Marker is clicked |
| `markerRender` | IMarkerRenderingEventArgs | Marker is rendering |
| `markerDrag` | IMarkerDragEventArgs | Marker is being dragged |
| `markerMove` | IMarkerMoveEventArgs | Marker moves on map |
| `markerClusterClick` | IMarkerClusterClickEventArgs | Cluster is clicked |

### Marker Event Arguments

```typescript
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
```

### MarkerClusterSettings

```typescript
interface MarkerClusterSettings {
    // Enable
    allowClustering?: boolean;
    allowClusterExpand?: boolean;
    allowDeepClustering?: boolean;

    // Appearance
    fill?: string;
    width?: number;
    height?: number;
    opacity?: number;
    shape?: string;
    imageUrl?: string;

    // Border
    border?: {
        color: string;
        width: number;
    };

    // Label
    labelStyle?: FontSettings;

    // Layout
    offset?: { x: number; y: number };

    // Line (when expanded)
    connectorLineSettings?: ConnectorLineSettings;

    // Styling
    dashArray?: string;
}
```

### Working with Marker Clusters

```typescript
// Enable clustering
markerSettings: {
    dataSource: manyMarkers,
    markerClusterSettings: {
        allowClustering: true,
        fill: '#0099FF'
    }
}

// Listen for cluster clicks
markerClusterClick: (args: IMarkerClusterClickEventArgs) => {
    console.log('Cluster clicked');
    // Zoom to cluster
    map.zoomSettings.zoomFactor = map.zoomSettings.zoomFactor + 1;
}
```

**For complete Marker and Navigation Line APIs, see:** [api-reference-guide.md](api-reference-guide.md)

## Next Steps

- Enhance data visualization with bubbles or color mapping (see visualization-features.md)
- Add legends to explain marker types (see legends-and-annotations.md)
- Configure user interactions and events (see map-providers-and-selection.md)
- Review complete marker and navigation APIs (see api-reference-guide.md)
