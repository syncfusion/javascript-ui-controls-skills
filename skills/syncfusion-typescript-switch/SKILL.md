---
name: syncfusion-typescript-switch
description: Implement Syncfusion TypeScript Switch component for toggle controls. Use this skill when creating a Switch/toggle control, configuring switch states (checked/disabled), adding switch events (change/beforeChange), customizing switch appearance (colors, sizes), implementing form-integrated switches, or needing accessibility features. Covers installation, basic usage, state management, styling, form integration, and accessibility patterns for Syncfusion Switch in TypeScript projects.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
  platform: "TypeScript"
---

# Implementing Syncfusion TypeScript Switch

A comprehensive skill for implementing Syncfusion Switch components in TypeScript projects. The Switch component provides a touch-friendly way to toggle between two mutually exclusive states (ON/OFF, true/false).

## When to Use This Skill

Use this skill when you need to:

- **Create a Switch component** with checked/unchecked states
- **Handle state changes** with events and methods
- **Customize appearance** (colors, sizes, styling)
- **Integrate switches in forms** with name/value submission
- **Implement accessibility** features (keyboard navigation, ARIA)
- **Prevent state changes** conditionally using beforeChange event
- **Add interactive toggle controls** to your application

**Trigger Keywords:** Syncfusion Switch, toggle control, TypeScript switch, ej2-buttons Switch, checked state, disabled switch, switch customization, form toggle

## Overview

The Syncfusion Switch component is a touch-friendly control for toggling between two states. It's:

- **Lightweight** - Minimal dependencies (@syncfusion/ej2-buttons)
- **Accessible** - WCAG 2.2 compliant with keyboard support
- **Themeable** - Works with Material, Bootstrap, and other Syncfusion themes
- **Event-driven** - Supports change, beforeChange, and other events
- **Form-integrated** - Submits name/value pairs in forms
- **RTL-ready** - Full right-to-left language support
- **Customizable** - CSS-driven styling for colors and appearance

## Documentation Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation and npm package setup
- Project structure (HTML + TypeScript)
- Basic switch initialization
- Text labels (onLabel/offLabel)
- Rendering to DOM
- Theme configuration
- First working example

### States and Behavior
📄 **Read:** [references/states-and-behavior.md](references/states-and-behavior.md)
- Checked and unchecked states
- Disabled state configuration
- Toggle method for programmatic changes
- Change event handling
- beforeChange event for conditional logic
- RTL support
- Event arguments and properties

### Customization
📄 **Read:** [references/customization.md](references/customization.md)
- CSS class customization with cssClass property
- Size variants (default, small via e-small)
- Color customization techniques
- Bar and handle styling
- Custom color examples
- Border-radius and shape modifications

### Accessibility
📄 **Read:** [references/accessibility.md](references/accessibility.md)
- WCAG 2.2 compliance details
- ARIA attributes (role, aria-disabled)
- Keyboard navigation (Space key support)
- Screen reader compatibility
- Accessibility testing with tools
- Color contrast guidelines

### Form Integration
📄 **Read:** [references/form-integration.md](references/form-integration.md)
- Name and value attributes
- Form submission behavior
- Grouping switches with name attribute
- Disabled vs unchecked switch behavior
- Server-side value handling
- Multiple switches in forms

### Advanced Features
📄 **Read:** [references/advanced-features.md](references/advanced-features.md)
- Ripple effects on switch labels
- Advanced event handling patterns
- Method reference (toggle)
- Event argument types (BeforeChangeEventArgs)
- Dynamic switch creation
- Integration with form validation

## Quick Start

```typescript
import { Switch } from '@syncfusion/ej2-buttons';

// Create a basic switch
const switchObj = new Switch({ 
  checked: true 
});

// Render it to an HTML element with id="element"
switchObj.appendTo('#element');
```

HTML markup:
```html
<input id="element" type="checkbox" />
```

## Common Patterns

### Pattern 1: Basic Toggle with Event Handler
```typescript
import { Switch } from '@syncfusion/ej2-buttons';

const switchObj = new Switch({ 
  checked: false,
  change: onSwitchChange
});
switchObj.appendTo('#toggle1');

function onSwitchChange(args: any) {
  console.log('Switch is now:', args.checked ? 'ON' : 'OFF');
}
```

### Pattern 2: Switch with Text Labels
```typescript
const switchObj = new Switch({ 
  onLabel: 'ON',
  offLabel: 'OFF',
  checked: true
});
switchObj.appendTo('#toggle2');
```

### Pattern 3: Disabled Switch
```typescript
const switchObj = new Switch({ 
  disabled: true,
  checked: true
});
switchObj.appendTo('#toggle3');
```

### Pattern 4: Prevent State Changes Conditionally
```typescript
const switchObj = new Switch({ 
  checked: false,
  beforeChange: beforeChangeHandler
});
switchObj.appendTo('#toggle4');

function beforeChangeHandler(args: any) {
  if (somethingInvalid()) {
    args.cancel = true;  // Prevent the change
  }
}
```

### Pattern 5: Styled Switch with Custom Colors
```typescript
const switchObj = new Switch({ 
  cssClass: 'custom-color-switch',
  checked: true
});
switchObj.appendTo('#toggle5');
```

CSS:
```css
.custom-color-switch.e-switch-inner {
  background-color: #4CAF50;
}
```

## Key Props Reference

| Property | Type | Description |
|----------|------|-------------|
| `checked` | boolean | Initial state (true = ON, false = OFF) |
| `disabled` | boolean | Disables the switch if true |
| `onLabel` | string | Text displayed in ON state |
| `offLabel` | string | Text displayed in OFF state |
| `cssClass` | string | CSS class for styling (e.g., 'e-small') |
| `name` | string | Name attribute for form submission |
| `value` | string | Value attribute for form submission |
| `enableRtl` | boolean | Enable right-to-left layout |

## Common Use Cases

1. **Feature toggles** - Enable/disable features in settings
2. **Notifications** - Toggle notification preferences
3. **Dark mode** - Switch between themes
4. **Visibility** - Show/hide sections based on switch state
5. **Form options** - Accept terms, opt-in/out options
6. **Filtering** - Toggle between view modes or filter states

## Related Resources

- [Syncfusion Switch Documentation](https://ej2.syncfusion.com/documentation/switch/getting-started/)
- [Component API Reference](https://ej2.syncfusion.com/javascript/api/switch/)
- [Theme Studio](https://ej2.syncfusion.com/themestudio/)
- [Accessibility Guidelines](https://www.w3.org/WAI/ARIA/apg/patterns/switch/)
