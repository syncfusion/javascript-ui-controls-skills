# Filters and Effects

## Table of Contents
- [Filter Effects](#filter-effects)
- [Fine-Tuning Adjustments](#fine-tuning-adjustments)
- [Effect Combinations](#effect-combinations)
- [Common Patterns](#common-patterns)

## Filter Effects

### Available Filters

Apply preset filter effects to images:

```typescript
import { ImageEditor } from '@syncfusion/ej2-image-editor';

const imageEditor = new ImageEditor({ width: '550px', height: '330px' });
imageEditor.appendTo('#imageeditor');

// Available filter effects
const filters = [
    'None',              // No effect
    'Default',           // Default processing
    'Blur',              // Blur effect
    'Sharpen',           // Sharpen effect
    'BlackAndWhite',     // Grayscale
    'Invert',            // Invert colors
    'Sepia',             // Sepia tone
    'Saturate',          // Increase saturation
    'Desaturate',        // Decrease saturation
    'HueShift',          // Shift hue
    'Negative',          // Negative colors
    'Posterize',         // Posterize effect
    'Brightness',        // Adjust brightness
    'Contrast',          // Adjust contrast
    'Saturation',        // Adjust saturation
    'Hue',               // Adjust hue
    'Lightness',         // Adjust lightness
    'Temperature',       // Color temperature
    'Tint',              // Add tint
    'Pixelate'           // Pixelation effect
];
```

### Apply Filter

```typescript
// Apply filter with effect name
imageEditor.filter('Blur', 5);              // Blur with intensity 5
imageEditor.filter('Sharpen', 3);           // Sharpen with intensity 3
imageEditor.filter('BlackAndWhite', 1);     // Black and white (no value needed)
imageEditor.filter('Sepia', 1);             // Sepia tone
imageEditor.filter('Invert', 1);            // Invert colors
```

### Filter via Toolbar

```typescript
const imageEditor = new ImageEditor({
    toolbar: ['Filter'],
    // User clicks Filter → Select effect from dropdown
    // Effect applied immediately
});
```

## Fine-Tuning Adjustments

### Brightness

Adjust image brightness:

```typescript
// Increase brightness (positive value brightens)
imageEditor.finetuneImage('brightness', 30);  // +30

// Decrease brightness (negative value darkens)
imageEditor.finetuneImage('brightness', -20); // -20

// Reset to original (0)
imageEditor.finetuneImage('brightness', 0);
```

### Contrast

Adjust image contrast:

```typescript
// Increase contrast (makes darks darker, lights lighter)
imageEditor.finetuneImage('contrast', 25);

// Decrease contrast (makes image more uniform)
imageEditor.finetuneImage('contrast', -15);
```

### Saturation

Adjust color saturation:

```typescript
// Increase saturation (more vibrant colors)
imageEditor.finetuneImage('saturation', 30);

// Decrease saturation (less vibrant, toward grayscale)
imageEditor.finetuneImage('saturation', -40);

// Complete desaturation = -100 (black and white)
imageEditor.finetuneImage('saturation', -100);
```

### Hue

Shift color hue:

```typescript
// Shift hue (rotate on color wheel)
imageEditor.finetuneImage('hue', 45);        // Shift 45 degrees
imageEditor.finetuneImage('hue', -120);      // Shift -120 degrees
imageEditor.finetuneImage('hue', 180);       // Invert all colors
```

### Blur

Apply blur effect:

```typescript
// Blur with intensity 1-10
imageEditor.filter('Blur', 5);               // Medium blur
imageEditor.filter('Blur', 1);               // Slight blur
imageEditor.filter('Blur', 10);              // Heavy blur
```

### Sharpen

Enhance image sharpness:

```typescript
// Sharpen with intensity
imageEditor.filter('Sharpen', 5);            // Medium sharpen
imageEditor.filter('Sharpen', 2);            // Slight sharpen
```

### Opacity/Alpha

```typescript
// Note: Transparency adjustments in filters
imageEditor.finetuneImage('opacity', 80);    // 80% opaque
imageEditor.finetuneImage('opacity', 50);    // 50% opaque
```

## Effect Combinations

### Black and White with Contrast

```typescript
// First convert to grayscale
imageEditor.filter('BlackAndWhite', 1);

// Then increase contrast for effect
imageEditor.finetuneImage('contrast', 20);
```

### Sepia with Saturation

```typescript
// Apply sepia tone
imageEditor.filter('Sepia', 1);

// Adjust saturation for intensity
imageEditor.finetuneImage('saturation', 15);
```

### Warm Tone (Temperature + Hue)

```typescript
// Increase temperature (warm colors)
imageEditor.finetuneImage('temperature', 25);

// Adjust hue slightly toward orange
imageEditor.finetuneImage('hue', 15);
```

### Cool Tone (Temperature + Hue)

```typescript
// Decrease temperature (cool colors)
imageEditor.finetuneImage('temperature', -25);

// Adjust hue toward blue
imageEditor.finetuneImage('hue', -30);
```

### Vintage Look

```typescript
// Apply sepia
imageEditor.filter('Sepia', 1);

// Reduce contrast slightly
imageEditor.finetuneImage('contrast', -10);

// Add slight blur
imageEditor.filter('Blur', 2);

// Increase lightness
imageEditor.finetuneImage('lightness', 15);
```

### High Contrast Black and White

```typescript
// Convert to black and white
imageEditor.filter('BlackAndWhite', 1);

// Increase contrast significantly
imageEditor.finetuneImage('contrast', 40);

// Slightly increase brightness
imageEditor.finetuneImage('brightness', 5);
```

## Fine-Tuning Values

### Value Ranges

| Property | Range | Default | Effect |
|----------|-------|---------|--------|
| brightness | -100 to +100 | 0 | -100 = black, +100 = bright |
| contrast | -100 to +100 | 0 | -100 = flat, +100 = extreme |
| saturation | -100 to +100 | 0 | -100 = grayscale, +100 = vivid |
| hue | -180 to +180 | 0 | Color wheel rotation |
| lightness | -100 to +100 | 0 | -100 = black, +100 = white |
| temperature | -100 to +100 | 0 | -100 = cool, +100 = warm |

## Common Patterns

### Pattern: Reset All Adjustments

```typescript
function resetAllEffects() {
    imageEditor.finetuneImage('brightness', 0);
    imageEditor.finetuneImage('contrast', 0);
    imageEditor.finetuneImage('saturation', 0);
    imageEditor.finetuneImage('hue', 0);
    imageEditor.finetuneImage('lightness', 0);
    imageEditor.finetuneImage('temperature', 0);
    imageEditor.filter('None', 0);
}

document.getElementById('resetBtn').onclick = resetAllEffects;
```

### Pattern: Adjustment Sliders

```typescript
function setupEffectSliders(imageEditor: ImageEditor) {
    const effects = [
        { name: 'brightness', id: 'brightnessSlider', min: -100, max: 100 },
        { name: 'contrast', id: 'contrastSlider', min: -100, max: 100 },
        { name: 'saturation', id: 'saturationSlider', min: -100, max: 100 },
        { name: 'hue', id: 'hueSlider', min: -180, max: 180 }
    ];
    
    effects.forEach(effect => {
        const slider = document.getElementById(effect.id) as HTMLInputElement;
        slider.min = String(effect.min);
        slider.max = String(effect.max);
        slider.value = '0';
        
        slider.addEventListener('input', (e) => {
            const value = parseInt((e.target as HTMLInputElement).value);
            imageEditor.finetuneImage(effect.name, value);
        });
    });
}
```

### Pattern: Preset Filter Gallery

```typescript
function createFilterGallery(imageEditor: ImageEditor) {
    const presets = [
        { name: 'Grayscale', apply: () => imageEditor.filter('BlackAndWhite', 1) },
        { name: 'Sepia', apply: () => imageEditor.filter('Sepia', 1) },
        { name: 'Invert', apply: () => imageEditor.filter('Invert', 1) },
        { name: 'Sharpen', apply: () => imageEditor.filter('Sharpen', 5) },
        { name: 'Blur', apply: () => imageEditor.filter('Blur', 5) },
        { name: 'Saturated', apply: () => imageEditor.finetuneImage('saturation', 50) },
        { name: 'Desaturated', apply: () => imageEditor.finetuneImage('saturation', -50) }
    ];
    
    const gallery = document.getElementById('filterGallery');
    
    presets.forEach(preset => {
        const btn = document.createElement('button');
        btn.textContent = preset.name;
        btn.onclick = () => {
            imageEditor.undo();  // Undo previous filter
            preset.apply();
        };
        gallery.appendChild(btn);
    });
}
```

### Pattern: Before/After Comparison

```typescript
function setupBeforeAfter(imageEditor: ImageEditor, originalUrl: string) {
    let originalLoaded = false;
    
    document.getElementById('beforeBtn').onclick = () => {
        if (!originalLoaded) {
            imageEditor.open(originalUrl);
            originalLoaded = true;
        } else {
            // Reload original
            imageEditor.undo();
        }
    };
    
    document.getElementById('afterBtn').onclick = () => {
        // Show current edited version
        console.log('Current edited state');
    };
    
    document.getElementById('compareBtn').onclick = () => {
        // Show split view (requires additional implementation)
        console.log('Compare view');
    };
}
```

### Pattern: Auto-Enhance

```typescript
function autoEnhance(imageEditor: ImageEditor) {
    // Automatic enhancement suggestions
    
    // Slightly increase contrast
    imageEditor.finetuneImage('contrast', 10);
    
    // Increase saturation mildly
    imageEditor.finetuneImage('saturation', 15);
    
    // Slight sharpening
    imageEditor.filter('Sharpen', 2);
    
    // Optional: slight brightness adjustment
    // imageEditor.finetuneImage('brightness', 5);
}

document.getElementById('autoEnhanceBtn').onclick = () => autoEnhance(imageEditor);
```

