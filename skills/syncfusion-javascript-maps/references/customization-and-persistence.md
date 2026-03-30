# Customization & Persistence in Maps

## Table of Contents

- [CSS Customization](#css-customization)
    - [Map Container Styling](#map-container-styling)
    - [Map Element Classes](#map-element-classes)
- [Theme Configuration](#theme-configuration)
    - [Built-in Themes](#built-in-themes)
    - [Custom Theme Colors](#custom-theme-colors)
- [Advanced Styling](#advanced-styling)
    - [Marker Styling](#marker-styling)
    - [Bubble Styling](#bubble-styling)
    - [Shape/Polygon Styling](#shapepolygon-styling)
    - [Navigation Line Styling](#navigation-line-styling)
- [Print Functionality](#print-functionality)
    - [Basic Print](#basic-print)
    - [Print with Title](#print-with-title)
- [State Persistence](#state-persistence)
    - [LocalStorage Persistence](#localstorage-persistence)
    - [SessionStorage for Temporary State](#sessionstorage-for-temporary-state)
- [Common Patterns](#common-patterns)
- [Performance Optimization](#performance-optimization)
- [Related API Reference](#related-api-reference)
- [Tips for Customization](#tips-for-customization)
- [Next Steps](#next-steps)

## CSS Customization

### Map Container Styling

```css
#container {
    width: 100%;
    height: 100vh;
    border: 2px solid #333;
    border-radius: 8px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}
```

### Map Element Classes

The map generates CSS classes you can customize:

```css
/* Map tile container */
.e-map {
    background-color: #f0f0f0;
}

/* Markers */
.e-marker {
    opacity: 0.9;
}

/* Bubbles */
.e-bubble {
    stroke-width: 1px;
}

/* Shapes/Polygons */
.e-shape {
    stroke-width: 0.5px;
}

/* Legend */
.e-map-legend {
    background-color: rgba(255, 255, 255, 0.95);
}

/* Tooltip */
.e-map-tooltip {
    background-color: #333;
    color: white;
    padding: 8px;
    border-radius: 4px;
}
```

## Theme Configuration

### Built-in Themes

```typescript
const mapConfig = {
    theme: 'Material',  // 'Material', 'Fabric', 'Bootstrap', 'HighContrast'
    layers: []  // Layer config
};
```

### Custom Theme Colors

```typescript
const customTheme = {
    backgroundColor: '#F5F5F5',
    textStyle: {
        fontFamily: 'Arial',
        color: '#333333',
        size: '14px'
    },
    lineSettings: {
        color: '#999999'
    }
};

const mapConfig = {
    // Apply custom theme (approach depends on version)
    layers: []  // Layer config
};
```

## Advanced Styling

### Marker Styling

```typescript
layers: [
  {
    markerSettings: [
      {
        visible: true,
        dataSource: locationData,
        latitudeValuePath: 'latitude',
        longitudeValuePath: 'longitude',

        width: 35,
        height: 35,
        fill: '#FF0000',
        opacity: 0.9,

        border: {
          color: '#FFFFFF',
          width: 2
        },

        template: `<svg width="30" height="30">
          <circle cx="15" cy="15" r="12"
            fill="#FF0000" stroke="#FFF" stroke-width="2"/>
        </svg>`,

        tooltipSettings: {
          visible: true,
          valuePath: 'name'
        }
      }
    ]
  }
]
```

### Bubble Styling

```typescript
layers: [
  {
    bubbleSettings: [
      {
        visible: true,
        dataSource: bubbleData,
        valuePath: 'population',
        colorValuePath: 'color',

        minRadius: 8,
        maxRadius: 40,

        fill: 'rgba(255,0,0,0.7)',
        opacity: 0.8,

        border: {
          color: '#333',
          width: 2
        }
      }
    ]
  }
]
```

### Shape/Polygon Styling

```typescript
shapeSettings: {
  fill: '#CCFFCC',
  opacity: 0.8,
  border: {
    color: '#333',
    width: 1.5,
    dashArray: '5,5'
  }
}
```

### Navigation Line Styling

```typescript

navigationLineSettings: [
  {
    visible: true,
    latitude: [37.6872, 34.0522],
    longitude: [-122.0808, -118.2437],

    color: '#FF0000',
    width: 3,
    dashArray: '5,5',

    angle: 0,
    opacity: 0.7
  }
]
```

## Print Functionality

### Basic Print

```typescript
import { Print } from '@syncfusion/ej2-maps';

// Add print capability
const map = new Maps({
    // ... configuration
});

map.appendTo('#container');

// Print the map
function printMap() {
    map.print();
}

// Add print button
document.getElementById('printBtn').addEventListener('click', () => {
    map.print();
});
```

### Print with Title

```typescript
// Custom print method
function printMapWithTitle() {
  const printWindow = window.open('', '', 'width=800,height=600');
  printWindow.document.write('<h1>Map Report</h1>');
  printWindow.document.close();
  map.print();
}
```

## State Persistence

Save and restore map state (zoom level, center position, etc.).

### LocalStorage Persistence

```typescript
const map = new Maps({
    load: () => {
        // Load saved state on map load
        const savedState = localStorage.getItem('mapState');
        if (savedState) {
            const state = JSON.parse(savedState);
            map.centerPosition = state.center;
            map.zoomSettings.zoomFactor = state.zoomLevel;
        }
    },
    
    zoomEnd: () => {
        // Save state when zoom changes
        saveMapState();
    },
    
    panEnd: () => {
        // Save state when pan completes
        saveMapState();
    },
    
    layers: [/* ... */]
});

// Save current map state
function saveMapState() {
    const state = {
        center: map.centerPosition,
        zoomLevel: map.zoomSettings.zoomFactor
    };
    localStorage.setItem('mapState', JSON.stringify(state));
}
```

### SessionStorage for Temporary State

```typescript
// Preserve state during user session
function saveSessionState() {
    const state = {
        center: map.centerPosition,
        zoomLevel: map.zoomSettings.zoomFactor,
        timestamp: new Date().getTime()
    };
    sessionStorage.setItem('mapState', JSON.stringify(state));
}

// Restore on page reload
function restoreSessionState() {
    const saved = sessionStorage.getItem('mapState');
    if (saved) {
        const state = JSON.parse(saved);
        map.centerPosition = state.center;
        map.zoomSettings.zoomFactor = state.zoomLevel;
    }
}
```

## Common Patterns

### Pattern 1: Dark Theme

```typescript
// CSS for dark theme
const darkThemeStyles = `
    #container {
        background-color: #1e1e1e;
    }
    
    .e-map {
        background-color: #2d2d2d;
    }
    
    .e-map-tooltip {
        background-color: #333;
        color: #fff;
    }
    
    .e-marker {
        filter: invert(1);
    }
`;

// Apply theme
const styleTag = document.createElement('style');
styleTag.innerHTML = darkThemeStyles;
document.head.appendChild(styleTag);
```

### Pattern 2: Remember User Preferences

```typescript
interface MapPreferences {
    center: { latitude: number; longitude: number };
    zoomLevel: number;
    theme: 'light' | 'dark';
    selectedLayers: string[];
}

class MapManager {
    private preferences: MapPreferences;
    
    constructor() {
        this.loadPreferences();
    }
    
    loadPreferences() {
        const saved = localStorage.getItem('userMapPreferences');
        this.preferences = saved ? JSON.parse(saved) : this.getDefaults();
    }
    
    savePreferences() {
        localStorage.setItem('userMapPreferences', JSON.stringify(this.preferences));
    }
    
    getDefaults(): MapPreferences {
        return {
            center: { latitude: 0, longitude: 0 },
            zoomLevel: 1,
            theme: 'light',
            selectedLayers: []
        };
    }
    
    apply(map: Maps) {
        map.centerPosition = this.preferences.center;
        map.zoomSettings.zoomFactor = this.preferences.zoomLevel;
    }
}
```

### Pattern 3: Responsive Styling

```css
/* Mobile */
@media (max-width: 768px) {
    #container {
        height: 60vh;
    }
    
    .e-map-tooltip {
        font-size: 12px;
    }
    
    .e-marker {
        width: 20px;
        height: 20px;
    }
}

/* Tablet */
@media (max-width: 1024px) {
    #container {
        height: 70vh;
    }
}

/* Desktop */
@media (min-width: 1025px) {
    #container {
        height: 100vh;
    }
}
```

### Pattern 4: Export as Image

```typescript
import {Print, ImageExport } from '@syncfusion/ej2-maps';

// Export current map view
function exportMapAsImage() {
    // This requires the ImageExport module
    const fileName = 'map-' + new Date().getTime();
    map.export('PNG', fileName);
}

document.getElementById('exportBtn').addEventListener('click', () => {
    exportMapAsImage();
});
```

## Performance Optimization

### Limit Rendering

```typescript
// Throttle tooltip rendering
let tooltipTimeout: any;

map.tooltipRender = (args) => {
    clearTimeout(tooltipTimeout);
    tooltipTimeout = setTimeout(() => {
        // Process tooltip
    }, 100);
};
```

### Cache Layer Data

```typescript
class LayerCache {
    private cache: { [key: string]: any } = {};
    
    getLayer(key: string) {
        return this.cache[key];
    }
    
    setLayer(key: string, data: any) {
        this.cache[key] = data;
    }
}
```

## Related API Reference

### Margin Interface

```typescript
interface Margin {
    left?: number;
    right?: number;
    top?: number;
    bottom?: number;
}
```

### Border Interface

```typescript
interface Border {
    color?: string;
    width?: number;
    opacity?: number;
    dashArray?: string;
}
```

### FontSettings Interface

```typescript
interface FontModel {
  size?: string;
  color?: string;
  fontFamily?: string;
  fontWeight?: string;
  fontStyle?: string;
}
```

### MapsTheme Enumeration

```typescript
enum MapsTheme {
  Material,
  Fabric,
  Bootstrap,
  HighContrast,

  MaterialDark,
  FabricDark,
  BootstrapDark,

  Bootstrap4,
  Bootstrap5,
  Bootstrap5Dark,

  Tailwind,
  TailwindDark,

  Fluent,
  FluentDark,

  Fluent2,
  Fluent2Dark,
  Fluent2HighContrast,

  Material3,
  Material3Dark
}
```

### ExportType Enumeration

```typescript
enum ExportType {
    PNG = 'PNG',
    SVG = 'SVG',
    PDF = 'PDF'
}
```

### Component Methods

| Method | Purpose |
|--------|---------|
| `export(type, fileName)` | Export map as PNG, SVG, or PDF |
| `print()` | Print the map |
| `refresh()` | Refresh after customization changes |
| `destroy()` | Clean up resources and remove map |

### Core CSS Classes

```css
/* Map container */
.e-map { }

/* Marker element */
.e-marker { }

/* Bubble element */
.e-bubble { }

/* Shape/polygon */
.e-shape { }

/* Legend */
.e-map-legend { }

/* Tooltip */
.e-map-tooltip { }

/* Annotation */
.e-annotation { }
```

### Export and Print Usage

```typescript
// Export as PNG
map.export(ExportType.PNG, 'map.png');

// Export as SVG
map.export(ExportType.SVG, 'map.svg');

// Export as PDF
map.export(ExportType.PDF, 'map.pdf');

// Print
map.print();

// Listen for print events
printEvent: (args: IPrintEventArgs) => {
    console.log('Map printed');
}
```

### State Persistence Events

```typescript
// Map loaded - restore saved state
load: (args: ILoadEventArgs) => {
    const saved = localStorage.getItem('mapState');
    if (saved) {
        const state = JSON.parse(saved);
        map.centerPosition = state.center;
        map.zoomSettings.zoomFactor = state.zoom;
    }
}

// Zoom ends - save state
zoomEnd: (args: IMapZoomEventArgs) => {
    saveMapState();
}

// Pan ends - save state
panEnd: (args: IMapPanEventArgs) => {
    saveMapState();
}
```

**For complete Customization and Theme APIs, see:** [api-reference-guide.md](api-reference-guide.md)

## Tips for Customization

1. **Test responsiveness:** Ensure map works on all screen sizes
2. **Accessibility:** Maintain sufficient color contrast
3. **Performance:** Minimize CSS calculations during interactions
4. **Themes:** Use consistent color schemes
5. **Persistence:** Remember critical user preferences

## Next Steps

- Troubleshoot common issues and optimize performance (see troubleshooting.md)
- Configure advanced features like clustering (see visualization-features.md)
- Implement custom interactions (see map-providers-and-selection.md)
- Review complete customization and export APIs (see api-reference-guide.md)
