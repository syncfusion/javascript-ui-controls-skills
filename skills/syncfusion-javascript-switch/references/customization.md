# Customization

## Table of Contents
- [CSS Class Approach](#css-class-approach)
- [Size Customization](#size-customization)
- [Color Customization](#color-customization)
- [Bar and Handle Styling](#bar-and-handle-styling)
- [Advanced Styling Examples](#advanced-styling-examples)

## CSS Class Approach

The Switch component uses the `cssClass` property to apply custom CSS styling. This is the primary method for customizing appearance:

```typescript
import { Switch } from '@syncfusion/ej2-buttons';

const switchObj = new Switch({ 
  cssClass: 'custom-style-class'
});
switchObj.appendTo('#element');
```

### Internal CSS Classes

The Switch component has several internal CSS classes you can target:

| Class | Element | Purpose |
|-------|---------|---------|
| `.e-switch` | Container | Main switch wrapper |
| `.e-switch-inner` | Track | Switch bar/track |
| `.e-switch-handle` | Thumb | Switch handle/toggle |
| `.e-icon` | Label icon | Label icon element |

### Example: Custom Styling with cssClass

```typescript
const switchObj = new Switch({ 
  cssClass: 'my-custom-switch',
  checked: true
});
switchObj.appendTo('#element');
```

CSS:
```css
/* Target the switch with custom class */
.my-custom-switch.e-switch {
  margin: 10px;
}

.my-custom-switch .e-switch-inner {
  border-radius: 20px;  /* More rounded */
}

.my-custom-switch .e-switch-handle {
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
}
```

## Size Customization

### Built-in Small Size

The Syncfusion library provides a pre-made small size variant using the `e-small` CSS class:

```typescript
import { Switch } from '@syncfusion/ej2-buttons';

// Default size switch
const defaultSwitch = new Switch({ checked: true });
defaultSwitch.appendTo('#switch-default');

// Small size switch
const smallSwitch = new Switch({ 
  cssClass: 'e-small',
  checked: true
});
smallSwitch.appendTo('#switch-small');
```

### Custom Size

Create custom sizes by modifying dimensions:

```typescript
const switchObj = new Switch({ 
  cssClass: 'custom-size',
  checked: true
});
switchObj.appendTo('#element');
```

CSS:
```css
/* Large switch */
.custom-size.e-switch {
  --e-switch-width: 60px;   /* Wider track */
  --e-switch-height: 35px;  /* Taller track */
}

.custom-size .e-switch-handle {
  width: 30px;   /* Bigger handle */
  height: 30px;
}
```

### Size Comparison Example

```html
<div class="size-comparison">
  <h4>Default Size</h4>
  <input id="switch-default" type="checkbox" />
  
  <h4>Small Size</h4>
  <input id="switch-small" type="checkbox" />
  
  <h4>Large Custom Size</h4>
  <input id="switch-large" type="checkbox" />
</div>
```

```typescript
import { Switch } from '@syncfusion/ej2-buttons';

new Switch({ checked: true }).appendTo('#switch-default');
new Switch({ cssClass: 'e-small', checked: true }).appendTo('#switch-small');
new Switch({ cssClass: 'large-switch', checked: true }).appendTo('#switch-large');
```

CSS:
```css
.large-switch.e-switch {
  transform: scale(1.5);
  transform-origin: left center;
}
```

## Color Customization

### Changing Bar Color

Customize the track (bar) color:

```typescript
const switchObj = new Switch({ 
  cssClass: 'custom-bar-color',
  checked: true
});
switchObj.appendTo('#element');
```

CSS:
```css
.custom-bar-color .e-switch-inner {
  background-color: #FF6B6B;  /* Custom red */
  border-color: #CC5555;
}
```

### Changing Handle Color

Customize the handle color:

```typescript
const switchObj = new Switch({ 
  cssClass: 'custom-handle-color',
  checked: true
});
switchObj.appendTo('#element');
```

CSS:
```css
.custom-handle-color .e-switch-handle {
  background-color: #FFD700;  /* Gold */
  box-shadow: 0 0 4px rgba(255, 215, 0, 0.5);
}
```

### Dual Color Scheme

```typescript
const switchObj = new Switch({ 
  cssClass: 'dual-color-scheme',
  checked: false
});
switchObj.appendTo('#element');
```

CSS:
```css
/* OFF state - gray */
.dual-color-scheme:not(.e-checked) .e-switch-inner {
  background-color: #CCCCCC;
  border-color: #999999;
}

/* ON state - green */
.dual-color-scheme.e-checked .e-switch-inner {
  background-color: #4CAF50;
  border-color: #388E3C;
}
```

## Bar and Handle Styling

### Rounded vs Square Shapes

Change the shape from rounded (default) to square:

```typescript
const switchObj = new Switch({ 
  cssClass: 'square-switch',
  checked: true
});
switchObj.appendTo('#element');
```

CSS:
```css
/* Square shape */
.square-switch .e-switch-inner {
  border-radius: 0;  /* No rounding */
}

.square-switch .e-switch-handle {
  border-radius: 0;
}
```

### Custom Border Styles

```typescript
const switchObj = new Switch({ 
  cssClass: 'custom-border-switch',
  checked: true
});
switchObj.appendTo('#element');
```

CSS:
```css
.custom-border-switch .e-switch-inner {
  border: 2px solid #333333;
  border-radius: 10px;
  background-color: #F5F5F5;
}

.custom-border-switch .e-switch-handle {
  border: 1px solid #666666;
  border-radius: 8px;
  background: linear-gradient(135deg, #FFFFFF 0%, #F0F0F0 100%);
}
```

### Shadow and Depth Effects

```typescript
const switchObj = new Switch({ 
  cssClass: 'shadow-switch',
  checked: true
});
switchObj.appendTo('#element');
```

CSS:
```css
.shadow-switch .e-switch-inner {
  box-shadow: inset 0 1px 3px rgba(0, 0, 0, 0.1);
  background-color: #F9F9F9;
}

.shadow-switch .e-switch-handle {
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.2);
}

.shadow-switch.e-checked .e-switch-handle {
  box-shadow: 0 3px 8px rgba(76, 175, 80, 0.3);
}
```

## Advanced Styling Examples

### iOS-Style Switch

Mimic iOS switch appearance:

```typescript
const switchObj = new Switch({ 
  cssClass: 'ios-style',
  checked: false
});
switchObj.appendTo('#element');
```

CSS:
```css
.ios-style.e-switch {
  --e-switch-width: 51px;
  --e-switch-height: 31px;
}

.ios-style .e-switch-inner {
  background-color: #CCCCCC;
  border-radius: 16px;
  border: none;
  transition: background-color 0.3s ease;
}

.ios-style.e-checked .e-switch-inner {
  background-color: #4CD964;
}

.ios-style .e-switch-handle {
  background-color: #FFFFFF;
  border-radius: 15px;
  width: 27px;
  height: 27px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
}
```

### Material Design Switch

Follow Material Design guidelines:

```typescript
const switchObj = new Switch({ 
  cssClass: 'material-design',
  checked: false
});
switchObj.appendTo('#element');
```

CSS:
```css
.material-design .e-switch-inner {
  background-color: #B0BEC5;
  border-radius: 7px;
}

.material-design.e-checked .e-switch-inner {
  background-color: rgba(33, 150, 243, 0.5);
}

.material-design .e-switch-handle {
  background-color: #FAFAFA;
  border-radius: 50%;
  box-shadow: 0 2px 1px -1px rgba(0, 0, 0, 0.2),
              0 1px 1px 0 rgba(0, 0, 0, 0.14),
              0 1px 3px 0 rgba(0, 0, 0, 0.12);
}

.material-design.e-checked .e-switch-handle {
  background-color: #2196F3;
  box-shadow: 0 3px 1px -2px rgba(33, 150, 243, 0.2),
              0 2px 2px 0 rgba(33, 150, 243, 0.14),
              0 1px 5px 0 rgba(33, 150, 243, 0.12);
}
```

### Gradient Fill

```typescript
const switchObj = new Switch({ 
  cssClass: 'gradient-switch',
  checked: true
});
switchObj.appendTo('#element');
```

CSS:
```css
.gradient-switch.e-switch-inner {
  background: linear-gradient(135deg, #667EEA 0%, #764BA2 100%);
  border: none;
}

.gradient-switch .e-switch-handle {
  background: linear-gradient(180deg, #FFFFFF 0%, #F5F5F5 100%);
  box-shadow: 0 4px 12px rgba(102, 126, 234, 0.4);
}
```

### Neon/Glowing Effect

```typescript
const switchObj = new Switch({ 
  cssClass: 'neon-switch',
  checked: false
});
switchObj.appendTo('#element');
```

CSS:
```css
.neon-switch .e-switch-inner {
  background-color: #222222;
  border: 2px solid #00FF00;
  border-radius: 12px;
  box-shadow: 0 0 10px #00FF00, inset 0 0 10px rgba(0, 255, 0, 0.2);
}

.neon-switch.e-checked .e-switch-inner {
  border-color: #FF0080;
  box-shadow: 0 0 15px #FF0080, inset 0 0 10px rgba(255, 0, 128, 0.2);
}

.neon-switch .e-switch-handle {
  background-color: #00FF00;
  border-radius: 50%;
  box-shadow: 0 0 8px #00FF00;
}

.neon-switch.e-checked .e-switch-handle {
  background-color: #FF0080;
  box-shadow: 0 0 12px #FF0080;
}
```

### Animated Color Transition

```typescript
const switchObj = new Switch({ 
  cssClass: 'animated-switch',
  checked: false
});
switchObj.appendTo('#element');
```

CSS:
```css
.animated-switch .e-switch-inner {
  background-color: #E0E0E0;
  border-radius: 12px;
  transition: background-color 0.4s ease, 
              box-shadow 0.4s ease;
}

.animated-switch.e-checked .e-switch-inner {
  background-color: #4CAF50;
  box-shadow: 0 0 12px rgba(76, 175, 80, 0.5);
}

.animated-switch .e-switch-handle {
  background-color: #FFFFFF;
  border-radius: 50%;
  transition: background-color 0.4s ease,
              box-shadow 0.4s ease;
}

.animated-switch.e-checked .e-switch-handle {
  background-color: #FFF;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.2);
}
```

## Multi-Class Combinations

You can combine multiple style classes:

```typescript
const switchObj = new Switch({ 
  cssClass: 'custom-color material-design',  // Multiple classes
  checked: true
});
switchObj.appendTo('#element');
```

CSS targets both classes:
```css
.custom-color.material-design .e-switch-inner {
  background-color: #FF9800;
}
```

## Performance Considerations

- Use CSS classes instead of inline styles for better performance
- Avoid complex animations or transitions if performance is critical
- Test on mobile devices to ensure smooth animations
- Use hardware-accelerated properties (transform, opacity)

## Next Steps

Explore related features:
- [States and Behavior](states-and-behavior.md) - Event handling and state management
- [Accessibility](accessibility.md) - Maintain accessibility while styling
- [Advanced Features](advanced-features.md) - Ripple effects and more
