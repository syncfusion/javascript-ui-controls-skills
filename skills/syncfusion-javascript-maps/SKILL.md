---
name: syncfusion-javascript-maps
description: "Guide to implementing Syncfusion Maps in TypeScript and JavaScript. Use this skill whenever the user needs to create interactive maps, add markers, visualize geographical data, work with map layers, apply color mapping, add annotations, configure legends, or handle map interactions and events. Works with TypeScript (module-based) and JavaScript (CDN/ES5)."
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
  category: "Data Visualization"
---

# Implementing Syncfusion Maps (TypeScript & JavaScript)

The Syncfusion Maps component is a powerful tool for visualizing geographical data, displaying interactive maps with layers, markers, bubbles, and advanced features like color mapping and navigation lines. Works in both TypeScript (with full type safety) and JavaScript (ES5 CDN-based or webpack).

## Platform Support

This skill covers TypeScript implementation:

**TypeScript:**
- Full type definitions and IDE support
- Module-based imports with npm
- Webpack/bundler-based setup
- Advanced type checking

## When to Use This Skill

Use this skill when you need to:
- Create interactive geographical maps in TypeScript applications
- Display data across different geographical regions
- Add markers, bubbles, or polygons to map visualizations
- Apply color mapping based on data values
- Configure map layers and different map providers
- Add user interactions like selection and tooltips
- Implement state persistence for map settings
- Print or export map visualizations
- Create navigation lines between locations
- Customize map appearance with styling and themes

## Maps Overview

The Syncfusion Maps component provides:
- **Multiple Map Providers**: Support for OpenStreetMap, Bing Maps, and other providers
- **Data Visualization**: Bubbles, polygons, and color mapping for data representation
- **Interactive Features**: Markers, tooltips, legends, annotations, and navigation lines
- **User Interactions**: Selection, zooming, panning, and mouse events
- **Customization**: Extensive styling options and theming capabilities
- **State Management**: Save and restore map state and viewport settings
- **Export**: Print functionality and export capabilities

## Documentation and Navigation Guide

### API Reference Guide
📄 **Read:** [references/api-reference-guide.md](references/api-reference-guide.md)
- **Core Maps Class** - Main component and its properties
- **Configuration Interfaces** - All available settings and options
  - CenterPosition, ZoomSettings, LayerSettings
  - MarkerSettings, BubbleSettings, ShapeSettings
  - NavigationLineSettings, LegendSettings, Annotation
  - TooltipSettings, SelectionSettings, HighlightSettings
  - DataLabelSettings, ColorMappingSettings
  - TitleSettings, SubTitleSettings, and more
- **Event Arguments** - Event handler parameters and data structures
  - IMarkerClickEventArgs, IMarkerRenderingEventArgs
  - IShapeSelectedEventArgs, IBubbleClickEventArgs
  - IMapZoomEventArgs, IMapPanEventArgs
  - ITooltipRenderEventArgs, IMouseEventArgs, and more
- **Enumerations** - Predefined constants and types
  - ExportType, MapsTheme, ProjectionType
  - LegendPosition, LegendMode, LegendShape
- **Methods** - Available component methods
  - appendTo(), refresh(), destroy(), export(), print()
- **Quick API Reference Table** - At-a-glance property mapping

### Getting Started & Setup
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation and package setup
- Creating your first map
- Setting up the map container
- Basic TypeScript configuration and initialization

### Data & Layers
📄 **Read:** [references/data-and-layers.md](references/data-and-layers.md)
- Populating maps with geographical data
- Working with map layers (OSM, Bing, Geometry)
- Data binding patterns
- Multiple layer configuration

### Markers & Navigation
📄 **Read:** [references/markers-and-navigation.md](references/markers-and-navigation.md)
- Adding markers to map locations
- Creating navigation lines between locations
- Customizing marker appearance and templates
- Interactive marker features and events
- Marker clustering for performance

### Visualization Features
📄 **Read:** [references/visualization-features.md](references/visualization-features.md)
- Bubble visualization for data points
- Polygon rendering on maps
- Color mapping strategies (range, equal, desaturation)
- Adding data labels to map elements
- Combining multiple visualization types

### Legends & Annotations
📄 **Read:** [references/legends-and-annotations.md](references/legends-and-annotations.md)
- Configuring and customizing legends
- Legend positioning and modes (List, Interactive)
- Adding annotations and overlays
- Tooltip configuration and formatting
- Data label styling

### Map Providers & Selection
📄 **Read:** [references/map-providers-and-selection.md](references/map-providers-and-selection.md)
- Using different map providers (OpenStreetMap, Bing Maps)
- User interaction events (click, zoom, pan)
- Selection and highlighting features
- Provider comparison and setup guides
- Advanced selection patterns

### Customization & Persistence
📄 **Read:** [references/customization-and-persistence.md](references/customization-and-persistence.md)
- CSS customization and theming
- Built-in themes and custom styling
- State persistence and viewport management
- Print and export functionality (PNG, SVG, PDF)
- Advanced styling techniques
- Performance optimization

### Troubleshooting
📄 **Read:** [references/troubleshooting.md](references/troubleshooting.md)
- Common issues and solutions
- Setup problems (container, tiles, markers)
- Data loading and color mapping issues
- Performance optimization tips
- Browser compatibility considerations
- TypeScript and JavaScript specific issues

## Quick Start Example

**TypeScript:**
```typescript
import { Maps, Marker } from '@syncfusion/ej2-maps';

// Inject required modules
Maps.Inject(Marker);

const map = new Maps({
    center: { latitude: 37.6872, longitude: -122.0808 },
    zoomSettings: { maxZoom: 13, minZoom: 1 },
    layers: [
        {
            urlTemplate: 'https://tile.openstreetmap.org/level/tileX/tileY.png',
            type: 'OSM',
            markerSettings: {
                dataSource: [
                    { latitude: 37.6872, longitude: -122.0808, name: 'San Francisco' }
                ]
            }
        }
    ]
});

map.appendTo('#map');
```

## Common Patterns

### Pattern 1: Data Visualization with Color Mapping
Use color mapping when displaying data that varies across regions (population, revenue, etc.):
- Choose mapping type: range, equal, or desaturation
- Define data source and property mapping
- Configure color ranges for visual representation
- Enable legends for clarity

### Pattern 2: Multi-Layer Maps
Combine multiple data sources using layers:
- Create separate layers for different data types
- Use markers for specific locations
- Overlay bubbles for aggregated data
- Combine with color mapping for regions

### Pattern 3: Interactive User Experience
Enhance map interactivity:
- Add tooltips for hover information
- Enable selection highlighting
- Implement custom events (click, double-click)
- Provide zoom and pan controls

### Pattern 4: Navigation & Route Planning
Show connections between locations:
- Use navigation lines to draw routes
- Add markers at key waypoints
- Include distance or duration information
- Style routes distinctly

## Key Features at a Glance

| Feature | Purpose | Use When |
|---------|---------|----------|
| **Markers** | Point locations | Marking specific addresses or coordinates |
| **Bubbles** | Sized data points | Showing relative magnitude of data |
| **Polygons** | Region shading | Highlighting geographical areas |
| **Color Mapping** | Data visualization | Displaying regional statistics or metrics |
| **Layers** | Data organization | Managing multiple data sources |
| **Navigation Lines** | Route visualization | Showing connections or journeys |
| **Legends** | Visual reference | Explaining colors, sizes, or symbols |
| **Annotations** | Custom overlays | Adding labels or custom content |
| **Tooltips** | Hover information | Providing context on hover |
| **State Persistence** | User experience | Remembering user's viewport/settings |

## Next Steps

1. Choose your starting reference based on your immediate need
2. Follow the implementation examples provided
3. Combine features as needed for your use case
4. Refer to troubleshooting guide if you encounter issues
