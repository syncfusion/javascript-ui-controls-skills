# Troubleshooting Guide for Maps

## Table of Contents

- [Common Setup Issues](#common-setup-issues)
    - [Issue: Map Container Not Displaying](#issue-map-container-not-displaying)
    - [Issue: Blank Tiles or Gray Map](#issue-blank-tiles-or-gray-map)
    - [Issue: Markers Not Appearing](#issue-markers-not-appearing)
    - [Issue: TypeScript Compilation Errors](#issue-typescript-compilation-errors)
- [Data Loading Issues](#data-loading-issues)
    - [Issue: Color Mapping Not Working](#issue-color-mapping-not-working)
    - [Issue: Bubbles Displaying Incorrectly](#issue-bubbles-displaying-incorrectly)
- [Performance Issues](#performance-issues)
    - [Issue: Map Freezes or Runs Slowly](#issue-map-freezes-or-runs-slowly)
    - [Issue: Memory Leaks](#issue-memory-leaks)
- [Interaction Issues](#interaction-issues)
    - [Issue: Markers Not Responding to Clicks](#issue-markers-not-responding-to-clicks)
    - [Issue: Tooltips Not Showing](#issue-tooltips-not-showing)
- [Browser Compatibility](#browser-compatibility)
- [Debugging Tips](#debugging-tips)
- [Platform-Specific Issues](#platform-specific-issues)
- [Related API Reference](#related-api-reference)
- [Validation Checklist](#validation-checklist)

## Common Setup Issues

### Issue: Map Container Not Displaying

**Symptom:** Blank div, nothing renders 

> ⚠️ `urlTemplate` is REQUIRED for tile maps

**Causes & Solutions:**

1. **Missing container dimensions**
   ```css
   /* ❌ Wrong */
   #container { }
   
   /* ✅ Correct */
   #container {
       width: 100%;
       height: 600px;  /* Must have explicit height */
   }
   ```

2. **Container not in DOM before appendTo()**
   ```typescript
   // ❌ Wrong - container must exist
   const map = new Maps({});
   map.appendTo('#nonexistent');
   
   // ✅ Correct - add to DOM first
   // <div id='container'></div>
   
   const map = new Maps({});
   map.appendTo('#container');
   ```

3. **Syncfusion CSS not loaded**

**TypeScript:**
   ```typescript
   import '@syncfusion/ej2-maps/styles/material.css';
   ```

### Issue: Blank Tiles or Gray Map

**Symptom:** Map loads but shows no tiles or blank squares

**Causes & Solutions:**

1. **Invalid map provider URL**
   ```typescript
   // ❌ Wrong - typo in URL
   urlTemplate: 'your URL link .PNG'
   
   // ✅ Correct - case matters!
   urlTemplate: 'your URL link .png'
   ```

2. **Network blocking**
   - Check browser console for CORS errors
   - Map provider may be blocked by network policy
   - Try alternate tile provider using `urlTemplate` (OSM / Azure / Mapbox)

### Issue: Markers Not Appearing

**Symptom:** Map renders but no markers visible

**Causes & Solutions:**

1. **Missing marker data**
   ```typescript
   // ❌ Wrong - no dataSource
   markerSettings: { }
   
   // ✅ Correct - include dataSource - array
   markerSettings:[ {
       dataSource: [
           { latitude: 37.6872, longitude: -122.0808 }
       ]
   }]
   ```

2. **Invalid coordinates**
   ```typescript
   // ❌ Wrong - coordinates out of range
   { latitude: 37.6872, longitude: 200 }  // Longitude must be -180 to 180
   
   // ✅ Correct - valid decimal degrees
   { latitude: 37.6872, longitude: -122.0808 }
   ```

3. **Marker outside current viewport**
   - Center map on marker coordinates
   - Adjust zoom level to include markers

   ```typescript
   const map = new Maps({
       center: { latitude: 37.6872, longitude: -122.0808 },
       zoomSettings: { zoomFactor: 5 }
   });
   ```

### Issue: TypeScript Compilation Errors

**Symptom:** `Cannot find module '@syncfusion/ej2-maps'`

**Solutions:**

1. **Install package**
   ```bash
   npm install @syncfusion/ej2-maps
   ```

2. **Check package.json**
   - Verify `@syncfusion/ej2-maps` is listed in dependencies
   - Run `npm install` again if missing

3. **Update TypeScript definitions**
   ```bash
   npm install --save-dev @types/syncfusion__ej2-maps
   ```

## Core Configuration Rules (Must Know)

These rules prevent most runtime and TypeScript errors.

---

### 1. Everything is inside `layers`

```typescript
layers: [
    {
        urlTemplate: 'your URL link',
        markerSettings: [ { ... } ],
        navigationLineSettings: [ { ... } ]
    }
]
## Data Loading Issues

### Issue: Color Mapping Not Working

**Symptom:** Colors not applied to shapes/bubbles

**Causes & Solutions:**

1. **Missing dataSource**
   ```typescript
   // ❌ Wrong - color mapping without data
   shapeSettings: { colorValuePath: 'value' }
   
   // ✅ Correct - include dataSource
   dataSource: stateData,
   shapeSettings: { colorValuePath: 'value' }
   ```

2. **Mismatched property paths**
   ```typescript
   // ❌ Wrong - paths don't match data structure
   dataSource: [{ name: 'California', value: 100 }]
   shapeDataPath: 'id'  // Property doesn't exist!
   
   // ✅ Correct - use existing properties
   dataSource: [{ name: 'California', value: 100 }]
   shapeDataPath: 'name'
   ```

3. **Color mapping ranges incomplete**
   ```typescript
   // ❌ Wrong - gaps in range
   colorMappings: [
       { from: 0, to: 50, color: '#FFF' },
       { from: 100, to: 150, color: '#FF0' }
       // Values 50-100 not covered!
   ]
   
   // ✅ Correct - continuous ranges
   colorMappings: [
       { from: 0, to: 50, color: '#FFF' },
       { from: 50, to: 100, color: '#FF0' },
       { from: 100, to: 150, color: '#F00' }
   ]
   ```

### Issue: Bubbles Displaying Incorrectly

**Symptom:** Bubble sizes don't correlate with data, wrong colors

**Causes & Solutions:**

1. **Invalid valuePath**
   ```typescript
   // ❌ Wrong - property doesn't exist
   bubbleSettings: {
       dataSource: data,
       valuePath: 'invalidProperty'  // Not in data!
   }
   
   // ✅ Correct - use actual data property
   bubbleSettings: {
       dataSource: data,
       valuePath: 'population'  // Exists in each data item
   }
   ```

2. **Bubble radius too small/large**
   ```typescript
   // ❌ Wrong - too subtle or huge
   minRadius: 1,
   maxRadius: 1000
   
   // ✅ Correct - reasonable range
   minRadius: 5,
   maxRadius: 30
   ```

## Performance Issues

### Issue: Map Freezes or Runs Slowly

**Symptoms:** Lag when panning/zooming, unresponsive interactions

**Causes & Solutions:**
1. **markerSettings must ALWAYS be an array**

```typescript
// ❌ Wrong
markerSettings: { ... }

// ✅ Correct
markerSettings: [
    {
        dataSource: [...]
    }
]
```

2. **Too many markers**
   ```typescript
   // ❌ Slow - 5000+ markers on single layer
   markerSettings: {
       dataSource: manyMarkers  // 5000 items
   }
   
   // ✅ Better - use clustering
   markerSettings: [{
       dataSource: manyMarkers,
       markerClusterSettings: {
           allowClustering: true,
           clusterRadius: 100
       }
   }]
   ```

3. **Complex marker templates**
   ```typescript
   // ❌ Slow - heavy HTML in template
   template: '<div style="complex CSS..."><img>...</div>'
   
   // ✅ Faster - simple template
   template: '<span style="background: red; width: 20px; height: 20px;"></span>'
   ```

4. **Expensive event handlers**
   ```typescript
   // ❌ Slow - runs on every pixel moved
   mouseMove: (args) => {
       expensiveCalculation();  // Fires hundreds of times
   }
   
   // ✅ Better - throttle the handler
   mouseMove: throttle((args) => {
       expensiveCalculation();
   }, 100)  // Only run every 100ms
   ```

5. **Large GeoJSON files**
   - Simplify GeoJSON (reduce polygon vertices)
   - Use TopoJSON instead (more compact)
   - Lazy-load regions based on zoom level

### Issue: Memory Leaks

**Symptom:** Browser memory usage keeps growing

**Solution:** Properly dispose of maps

```typescript
// Create map
const map = new Maps({});
map.appendTo('#container');

// Later, when done
if (map) {
    map.destroy();  // Clean up resources
}
```

### Issue: Navigation Lines Not Visible

**Symptom:** Markers show but lines are missing

**Causes & Solutions:**

1. **Module not injected**
```typescript
Maps.Inject(NavigationLine);
```

## Interaction Issues

### Issue: Markers Not Responding to Clicks

**Symptom:** markerClick event doesn't fire

**Causes & Solutions:**

1. **Event handler not configured**
   ```typescript
   // ❌ Wrong - no event handler
   const map = new Maps({});
   
   // ✅ Correct - add event handler
   const map = new Maps({
       markerClick: (args) => {
           console.log('Clicked!');
       }
   });
   ```

2. **Selection not enabled**
   ```typescript
   // ❌ Wrong - selection disabled
   markerSettings: {
       dataSource: markers,
       selectionSettings: { enable: false }
   }
   
   // ✅ Correct - enable selection
   markerSettings: [{
       dataSource: markers,
       selectionSettings: { enable: true }
   }]
   ```

### Issue: Tooltips Not Showing

**Symptom:** Hover produces no tooltip

**Causes & Solutions:**

1. **Tooltips not enabled**
   ```typescript
   // ❌ Wrong - tooltips disabled
   markerSettings: {
       dataSource: markers
   }
   
   // ✅ Correct - enable tooltips
   markerSettings:[{
       dataSource: markers,
       tooltipSettings: { visible: true }
   }]
   ```

2. **Empty valuePath**
   ```typescript
   // ❌ Wrong - valuePath doesn't exist
   tooltipSettings: {
       visible: true,
       valuePath: 'nonexistent'
   }
   
   // ✅ Correct - use valid property
   tooltipSettings: {
       visible: true,
       valuePath: 'name'
   }
   ```

## Browser Compatibility

### Issue: Map Works in Chrome but Not in Safari/Edge

**Common Causes:**

1. **CSS vendor prefixes missing**
   ```css
   /* Add for better compatibility */
   -webkit-transform: translate3d(0, 0, 0);
   -moz-transform: translate3d(0, 0, 0);
   transform: translate3d(0, 0, 0);
   ```

2. **Promise polyfill needed**
   ```typescript
   // Include for IE 11 support
   <script src="https://cdn.jsdelivr.net/npm/promise-polyfill@8/dist/polyfill.min.js"></script>
   ```

3. **Async/await support**
   - Use Babel transpiler for older browsers
   - Or rewrite using .then() instead of async/await

## Debugging Tips

### 1. Browser DevTools

```javascript
// In console
map.centerPosition  // Current center
map.zoomSettings.zoomFactor  // Current zoom
map.layers[0].dataSource  // Layer data
```

### 2. Console Logging

```typescript
const map = new Maps({
    load: () => console.log('Map loaded'),
    zoomEnd: () => console.log('Zoom:', map.zoomSettings.zoomFactor),
    markerClick: (args) => console.log('Marker:', args.data)
});
```

### 3. Check Network Tab

- Verify tile URLs are resolving
- Check for CORS errors
- Ensure API keys are valid

### 4. Inspect Elements (TS & JS identical)

```javascript
// In browser DevTools console
document.querySelector('.e-map')      // Map container
document.querySelector('.e-marker')   // First marker
document.querySelector('.e-bubble')   // First bubble
```

---

## Platform-Specific Issues

### TypeScript Issues

**Issue: Type errors with event handlers**

```typescript
// ❌ Error: No type checking
layerMouseMove: (args) => {  // args is any type
    console.log(args.latitude);
}

// ✓ Correct: Import and use types
import { IMouseEventArgs } from '@syncfusion/ej2-maps';

layerMouseMove: (args: IMouseEventArgs) => {
    console.log(args.latitude);  // Type-safe
}
```

**Issue: Module injection missing**

```typescript
// ❌ Error: Marker not available
const map = new Maps({ layers: [{ markerSettings: {} }] }, '#map');
// Markers not rendering - module not injected

// ✓ Correct: Inject modules
import { Maps, Marker } from '@syncfusion/ej2-maps';
Maps.Inject(Marker);
const map = new Maps({ layers: [{ markerSettings: [{}] }] }, '#map');
```

**Troubleshooting TypeScript:**
- Always run `npm install` after pulling updated dependencies
- Clear webpack cache: `rm -rf dist node_modules/.cache`
- Import required types from `@syncfusion/ej2-maps`
- Use TypeScript compiler in strict mode

### JavaScript Issues

**Issue: Global namespace not available**

```javascript
// ❌ Error if scripts not loaded
var map = new ej.maps.Maps({});  // ej is undefined

// ✓ Ensure CDN or local scripts loaded FIRST
<script src="https://cdn.syncfusion.com/ej2/ej2-maps/dist/global/ej2-maps.min.js"></script>
<script>
  var map = new ej.maps.Maps({});  // Now works
</script>
```

**Issue: Script loading order**

```html
<!-- ❌ Wrong order - dependencies first! -->
<script src="ej2-maps.min.js"></script>
<script src="ej2-base.min.js"></script>
<script src="ej2-data.min.js"></script>

<!-- ✓ Correct order -->
<script src="ej2-base.min.js"></script>
<script src="ej2-data.min.js"></script>
<script src="ej2-svg-base.min.js"></script>
<script src="ej2-maps.min.js"></script>
```

**Issue: CSS not applied**

```html
<!-- ❌ CSS after scripts - wrong -->
<script src="ej2-maps.min.js"></script>
<link rel="stylesheet" href="material.css" />

<!-- ✓ CSS before scripts -->
<link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/material.css" />
<script src="ej2-maps.min.js"></script>
```

**Troubleshooting JavaScript:**
- Check browser DevTools console for errors
- Verify all dependency scripts are present and loaded
- Use `typeof window.ej` to debug namespace availability
- Verify CDN links are not blocked by network policy

---

## Related API Reference

### Key Debugging Methods

```typescript
// Check current state in browser console
map.centerPosition           // Current center
map.zoomSettings.zoomFactor // Current zoom level
map.layers[0].dataSource    // Layer data

// Inspect DOM elements
document.querySelector('.e-map')      // Map container
document.querySelector('.e-marker')   // First marker
document.querySelector('.e-bubble')   // First bubble
document.querySelector('.e-shape')    // First shape
```

### Useful Type Definitions

```typescript
// Center position
interface CenterPosition {
    latitude: number;
    longitude: number;
}

// Zoom settings
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

// Layer settings
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

### Component Lifecycle

```typescript
// Initialization
const map = new Maps({...});

// Rendering
map.appendTo('#container');

// After render
map.loaded: (args) => { }
map.load: (args) => { }

// Cleanup
map.destroy();
```

### Validation Checklist

Before debugging, verify:

- [ ] Container exists with valid id
- [ ] Container has height (e.g., 600px)
- [ ] Syncfusion CSS loaded
- [ ] Modules injected (Marker, NavigationLine)
- [ ] `layers` is defined
- [ ] Layer has:
  - [ ] `urlTemplate` OR `shapeData`
- [ ] `markerSettings` is an array
- [ ] `navigationLineSettings` is an array
- [ ] Data variables are defined before use
- [ ] Coordinates are valid
- [ ] map.appendTo() called after DOM ready

**For complete debugging and API documentation, see:** [api-reference-guide.md](api-reference-guide.md)

## Getting Help

1. **Check documentation:** [Syncfusion Maps docs](https://www.syncfusion.com/javascript-ui-controls/js-maps)
2. **API Reference:** [api-reference-guide.md](api-reference-guide.md)
3. **Search issues:** GitHub issues or Stack Overflow
4. **Support:** Syncfusion support portal
5. **Community:** Syncfusion forums

## Common Gotchas

- **Case sensitivity:** `tileY` not `tiley` in URL templates (matters!)
- **Coordinates:** Always use decimal degrees (37.6872 not 37°41'14.04")
- **Memory:** Remember to `destroy()` maps when removing from DOM
- **Refresh:** Call `map.refresh()` after data changes
- **Async data:** Ensure data loads before adding to map
- **Namespacing:** JS uses `ej.maps.Maps`, TS uses imported `Maps`
