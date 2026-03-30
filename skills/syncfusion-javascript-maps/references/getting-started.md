# Getting Started with Maps

## Table of Contents
- [Setup (TypeScript)](#setup-typescript)
- [Setup (JavaScript)](#setup-javascript)
- [Creating Your First Map](#creating-your-first-map)
- [Map Container Setup](#map-container-setup)
- [Configuration Basics](#configuration-basics)

## Setup (TypeScript)

### Overview

This guide walks you through setting up Syncfusion Maps in a TypeScript project using the Syncfusion quickstart template or existing webpack project.

### Prerequisites

Before getting started, ensure you have:
- Node.js v14.15.0 or higher installed
- Basic knowledge of TypeScript and webpack
- A code editor (Visual Studio Code recommended)

### Dependencies

The Maps component requires these minimum dependencies:

```
@syncfusion/ej2-maps
├── @syncfusion/ej2-base
├── @syncfusion/ej2-data
├── @syncfusion/ej2-pdf-export
└── @syncfusion/ej2-svg-base
```

### Step 1: Clone Quickstart Template

```bash
git clone https://github.com/SyncfusionExamples/ej2-quickstart-webpack ej2-quickstart
cd ej2-quickstart
```

### Step 2: Install Packages

```bash
npm install
```

The quickstart template includes `@syncfusion/ej2` which contains all Syncfusion components.



## Creating Your First Map

### TypeScript: Basic Map

**Step 1: HTML Container** (`index.html`)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>EJ2 Maps</title>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
</head>
<body>
    <div id='container'></div>
</body>
</html>
```

**Step 2: TypeScript Code** (`app.ts`)

```typescript
import { Maps, MapsModel, Zoom } from '@syncfusion/ej2-maps';

Maps.Inject(Zoom);

const map: Maps = new Maps(<MapsModel>{
    zoomSettings: {
        enable: true,
        minZoom: 1,
        maxZoom: 13
    },

    layers: [
        {
            type:'Layer',
            urlTemplate: 'https://tile.openstreetmap.org/level/tileX/tileY.png' 
        }
    ]
});

map.appendTo('#element');
```

**Step 3: CSS Styling** (`styles.css`)

```css
#container {
    width: 100%;
    height: 100vh;
}
```



## Map Container Setup

### Container Requirements

The map container must have explicit dimensions (TS & JS identical):

```html
<div id='map' style="width: 100%; height: 600px;"></div>
```

**Why:** Maps need defined width/height to render properly. Use percentages or fixed values (px, rem).

### Responsive Container

For responsive maps that adapt to screen size (identical for TS & JS):

```css
#container {
    width: 100%;
    height: 100vh;
    margin: 0;
    padding: 0;
}
```

## Configuration Basics

### Maps Constructor Options

Configuration in TypeScript:

```typescript
const mapConfig = {
    // Center coordinates (latitude, longitude)
    centerPosition: { latitude: 0, longitude: 0 },
    
    // Zoom settings
    zoomSettings: {
        enable: true,
        minZoom: 1,
        maxZoom: 15,
        zoomFactor: 2
    },
    titleSettings: {
    text: 'OpenStreetMap'
    },
    
    // Layers (required - defines the background map)
    layers: [
        {
            urlTemplate: 'https://tile.openstreetmap.org/level/tileX/tileY.png',

        }
    ]
};
```

### Core Configuration Properties

| Property | Purpose | Example |
|----------|---------|---------|
| `centerPosition` | Initial map center | `{ latitude: 37.6872, longitude: -122.0808 }` |
| `zoomSettings` | Zoom behavior and limits | `{ enable: true, minZoom: 1, maxZoom: 15 }` |
| `layers` | Background map(s) and data | Array of layer configs |
| `width` | Map width | `'100%'` or `'800px'` |
| `height` | Map height | `'100%'` or `'600px'` |

## Initialization Pattern

### TypeScript Flow

1. **Import Maps**: `import { Maps } from '@syncfusion/ej2-maps'`
2. **Create Instance**: `const map = new Maps({ /* config */ })`
3. **Configure Layers**: Add at least one layer (background map)
4. **Append to DOM**: `map.appendTo('#container')`
5. **Run**: `npm start`



## Running Your Project

**TypeScript:**
```bash
npm start
```
Development server starts at `http://localhost:8080/`

## Common Setup Issues

### Issue: Map doesn't display
**Solution:** Verify the container has explicit width/height values set in CSS.

### Issue: Blank tiles or gray map
**Solution:** Check that the map provider URL is correct and accessible from your network.

### TypeScript: Compilation errors
**Solution:** Ensure `@syncfusion/ej2-maps` is listed in your `package.json` dependencies and run `npm install`.

## Related API Reference

### Core Maps Class Properties

| Property | Type | Description |
|----------|------|-------------|
| `centerPosition` | CenterPosition | Map center coordinates (latitude, longitude) |
| `zoomSettings` | ZoomSettings | Zoom configuration (enable, min/max levels) |
| `layers` | LayerSettings[] | Array of layer configurations |
| `width` | string | Map container width |
| `height` | string | Map container height |
| `background` | string | Map background color |

### Key Configuration Options

**Maps Constructor Parameters:**
```typescript
new Maps({
    centerPosition: { latitude: 0, longitude: 0 },
    zoomSettings: { enable: true, minZoom: 1, maxZoom: 20 },
    layers: [],
    width: '100%',
    height: '600px',
    margin: { left: 0, right: 0, top: 0, bottom: 0 },
    border: { color: '#ccc', width: 1 }
})
```

### Event Handlers

| Event | Args | Fires When |
|-------|------|-----------|
| `load` | ILoadEventArgs | Map starts initializing |
| `loaded` | ILoadedEventArgs | Map is fully loaded |
| `zoomEnd` | IMapZoomEventArgs | Zoom operation completes |
| `panEnd` | IMapPanEventArgs | Pan operation completes |
| `mouseMove` | IMouseEventArgs | Mouse moves over map |

### Methods

| Method | Purpose |
|--------|---------|
| `appendTo(selector)` | Render map in specified container |
| `refresh()` | Refresh map after configuration changes |
| `destroy()` | Clean up and remove map from DOM |
| `export(type, fileName)` | Export map as PNG/SVG/PDF |
| `print()` | Print the map |

**For complete API reference, see:** [api-reference-guide.md](api-reference-guide.md)

## Next Steps

Once your basic map is rendering:
- Add markers or bubbles to display data (see markers-and-navigation.md)
- Configure layers for multiple data sources (see data-and-layers.md)
- Apply color mapping for data visualization (see visualization-features.md)
- Review full API reference (see api-reference-guide.md)
