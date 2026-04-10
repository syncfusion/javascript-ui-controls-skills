# Data & Layers in Maps

## Table of Contents
- [Layers Overview](#layers-overview)
- [Map Layer Types](#map-layer-types)
- [Data Binding Patterns](#data-binding-patterns)
- [Working with Multiple Layers](#working-with-multiple-layers)
- [Layer Configuration](#layer-configuration)

**Note:** Configuration syntax is identical for TypeScript and JavaScript. Examples use object literal format.

## Layers Overview

Layers are the fundamental organizational structure in Maps. Each layer:
- Represents one set of geographical data or background map
- Contains its own data source and visualization settings
- Can be toggled independently
- Supports different visualization types (markers, bubbles, polygons)

**Why layers matter:** They let you combine multiple data sources on one map without visual conflicts.

## Map Layer Types

### 1. OSM (OpenStreetMap) Layer

Free, open-source background map:

```typescript
const layerConfig = {
    layers: [
        {
            urlTemplate: 'your URL link',
            type: 'OSM'
        }
    ]
};
```

**Use when:** You need a free, widely-supported background map with no API key.

### 2. Bing Maps Layer

Bing Maps with satellite, road, and aerial options:

```typescript
{
    layers: [
        {
            urlTemplate: 'http://dev.virtualearth.net/REST/v1/Imagery/Metadata/Road?output=json',
            type: 'Bing',
            bingMapType: 'Road',
            key: 'YOUR_BING_MAPS_KEY'
        }
    ]
}
```

**Use when:** You need high-quality imagery or satellite view.

### 3. Geometry Layer

Displays geojson/shapefile data:

```typescript
{
    layers: [
        {
            shapeData: geojsonData,
            type: 'Geometry',
            shapeSettings: {
                fill: 'rgba(200, 200, 200, 0.5)',
                border: { color: '#000', width: 1 }
            }
        }
    ]
}
```

**Use when:** Displaying administrative boundaries, custom regions, or polygons.

## Data Binding Patterns

### Pattern 1: Markers from Data Source

Add markers to specific locations:

```typescript
import { Maps, Marker } from '@syncfusion/ej2-maps';

Maps.Inject(Marker);

const map = new Maps({
  layers: [
    {
      urlTemplate: 'your URL link',

      markerSettings: [
        {
          visible: true,
          dataSource: [
            { latitude: 37.6872, longitude: -122.0808, name: 'San Francisco' }
          ],
          latitudeValuePath: 'latitude',
          longitudeValuePath: 'longitude'
        }
      ]
    }
  ]
});

map.appendTo('#element');
```

**Data structure:**
- `latitude`: Decimal degrees (e.g., 37.6872)
- `longitude`: Decimal degrees (e.g., -122.0808)
- Custom properties: Any additional data is accessible in templates

### Pattern 2: Bubbles for Data Aggregation

Visualize aggregated data with bubble size:

```typescript
const bubbleData = [
    { latitude: 37.6872, longitude: -122.0808, population: 873965 },
    { latitude: 34.0522, longitude: -118.2437, population: 3979576 }
];

const map = new Maps({
    layers: [
        {
            urlTemplate: 'your URL link',
           bubbleSettings: [
            {
              visible: true,
              dataSource: bubbleData,
              valuePath: 'population',
              colorValuePath: 'population',

              minRadius: 10,
              maxRadius: 40
            }]
        }
    ]
});
```

**Why bubbles:** They show magnitude at a glance; larger bubbles = higher values.

### Pattern 3: Polygon/Region Data

Display regional data with color mapping:

```typescript
const regionData = [
    { name: 'California', value: 39000000 },
    { name: 'Texas', value: 30000000 },
    { name: 'Florida', value: 22000000 }
];

const map = new Maps({
    layers: [
        {
            shapeData: usaGeometry,
            dataSource: regionData,
            shapeDataPath: 'name',
            shapePropertyPath: 'properties.name'
        }
    ]
});
```

## Working with Multiple Layers

### Adding Multiple Layers

Combine different data sources:

```typescript
const map = new Maps({
    layers: [
        // Base map layer
        {
            urlTemplate: 'your URL link',
            type: 'OSM'
        },
        // Markers layer
        {
            type: 'Geometry',
            markerSettings: {
                dataSource: cityMarkers
            }
        },
        // Bubbles layer
        {
            type: 'Geometry',
            bubbleSettings: {
                dataSource: populationBubbles
            }
        }
    ]
});
```

### Layer Visibility Toggle

Show/hide layers dynamically:

```typescript
// Hide layer at index 1
map.layers[1].visible = false;
map.refresh();

// Show layer
map.layers[1].visible = true;
map.refresh();
```

### Accessing Layer Data

Modify or access layer data at runtime:

```typescript
// Get first layer
const layer = map.layers[0];

// Update data source
layer.dataSource = newMarkerData;
map.refresh();
```

## Layer Configuration

### Core Layer Properties

```typescript
interface LayerSettingsModel {
  type?: 'Layer' | 'SubLayer';

  urlTemplate?: string;
  

  shapeData?: object | DataManager | MapAjax;
  dataSource?: object[] | DataManager | MapAjax;
  query?: Query;

  shapeDataPath?: string;
  shapePropertyPath?: string | string[];

  geometryType?: 'Geographic' | 'Normal';

  markerSettings?: MarkerSettingsModel[];
  bubbleSettings?: BubbleSettingsModel[];
  navigationLineSettings?: NavigationLineSettingsModel[];

  shapeSettings?: ShapeSettingsModel;

  dataLabelSettings?: DataLabelSettingsModel;
  tooltipSettings?: TooltipSettingsModel;

  visible?: boolean;
```

## Common Patterns

### Pattern: Base Map + Overlays

Separate concerns - one layer for background, others for data:

```typescript
// Layer 0: Background
layers: [
    {
        urlTemplate: 'your URL link',
       
    },
    // Layer 1: Data overlay
    {
        markerSettings: { dataSource: locationData }
    }
]
```

**Why:** Keeps background and data separate; easier to toggle or update.

### Pattern: Regional + Point Data

Combine regional shading with specific location markers:

```typescript
layers: [
    {
        shapeData: stateGeometry,
        dataSource: stateData,
        shapeSettings: { colorValuePath: 'metrics' }  // Color by metric
    },
    {
        markerSettings: { dataSource: cityMarkers }  // Specific cities
    }
]
```

## Data Tips

- **Coordinates:** Use decimal degrees (not degrees/minutes/seconds)
- **Data Source:** Must be array of objects with latitude/longitude or matching properties
- **Performance:** For 1000+ markers, consider clustering or aggregation
- **Refresh:** Call `map.refresh()` after updating layer data for changes to render

## Related API Reference

### LayerSettings Interface

```typescript
interface LayerSettingsModel {
  type?: 'Layer' | 'SubLayer';

  urlTemplate?: string;
  

  shapeData?: object | DataManager | MapAjax;
  dataSource?: object[] | DataManager | MapAjax;
  query?: Query;

  shapeDataPath?: string;
  shapePropertyPath?: string | string[];

  geometryType?: 'Geographic' | 'Normal';

  markerSettings?: MarkerSettingsModel[];
  bubbleSettings?: BubbleSettingsModel[];
  navigationLineSettings?: NavigationLineSettingsModel[];

  shapeSettings?: ShapeSettingsModel;

  dataLabelSettings?: DataLabelSettingsModel;
  tooltipSettings?: TooltipSettingsModel;

  visible?: boolean;
  
  markerClusterSettings?: MarkerClusterSettingsModel;
  polygonSettings?: PolygonSettingsModel;
  selectionSettings?: SelectionSettingsModel;
  highlightSettings?: HighlightSettingsModel;
  toggleLegendSettings?: ToggleLegendSettingsModel;
  initialShapeSelection?: InitialShapeSelectionSettingsModel[];
  animationDuration?: number;
}
```

### Core Layer Properties

| Property | Type | Purpose |
|----------|------|---------|
| `type` | string | Layer type: 'OSM', 'Bing', or 'Geometry' |
| `urlTemplate` | string | URL template for tile-based layers |
| `shapeData` | object | GeoJSON data for geometry layers |
| `dataSource` | object[] | Data array for markers, bubbles, or shapes |
| `shapeDataPath` | string | Property path in data to match with geometry |
| `shapePropertyPath` | string | GeoJSON property path to match with data |
| `visible` | boolean | Show or hide the layer |

### Layer Access and Manipulation

```typescript
// Access layers
const layer = map.layers[0];

// Update data
layer.dataSource = newData;
map.refresh();

// Toggle visibility
map.layers[1].visible = false;
map.refresh();

// Add/remove layers programmatically
map.layers.push(newLayerConfig);
map.refresh();
```

### Layer-Level Events

| Event | Purpose |
|-------|---------|
| `layerRender` | Fired when layer is rendering |
| `shapeRender` | Fired when shape/polygon renders |
| `markerRender` | Fired when markers render |

**For complete LayerSettings API, see:** [api-reference-guide.md#LayerSettings](api-reference-guide.md#LayerSettings)

## Next Steps

- Customize markers with icons and tooltips (see markers-and-navigation.md)
- Add color mapping to visualize data patterns (see visualization-features.md)
- Configure interactivity and events (see map-providers-and-selection.md)
- Review complete layer API (see api-reference-guide.md)
