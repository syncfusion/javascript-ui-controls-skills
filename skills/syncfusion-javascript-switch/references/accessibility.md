# Accessibility

## Table of Contents
- [WCAG 2.2 Compliance](#wcag-22-compliance)
- [ARIA Attributes](#aria-attributes)
- [Keyboard Navigation](#keyboard-navigation)
- [Screen Reader Support](#screen-reader-support)
- [Color Contrast Guidelines](#color-contrast-guidelines)
- [Mobile Accessibility](#mobile-accessibility)
- [Testing and Validation](#testing-and-validation)

## WCAG 2.2 Compliance

The Syncfusion Switch component meets the Web Content Accessibility Guidelines (WCAG) 2.2 standards, which are the international standard for web accessibility.

### Coverage Areas

| Accessibility Criteria | Status |
|---|---|
| WCAG 2.2 Support | ✓ Full Support |
| Section 508 Support | ✓ Full Support |
| Screen Reader Support | ✓ Full Support |
| Right-to-Left Support | ✓ Full Support |
| Color Contrast | ✓ Compliant |
| Mobile Device Support | ✓ Full Support |
| Keyboard Navigation | ✓ Full Support |

### Accessibility Compliance Levels

- **Level A** - Basic accessibility
- **Level AA** - Enhanced accessibility (recommended)
- **Level AAA** - Highest accessibility

The Switch component meets **Level AA** compliance by default.

## ARIA Attributes

ARIA (Accessible Rich Internet Applications) attributes help assistive technologies understand interactive components.

### Role Attribute

The Switch component uses the `switch` role:

```html
<input id="element" type="checkbox" />
```

When initialized as a Switch, it automatically has:
```
role="switch"
```

This tells screen readers that the element is a switch control, not a checkbox.

### aria-disabled

When you disable a switch, it automatically includes the proper ARIA attribute:

```typescript
import { Switch } from '@syncfusion/ej2-buttons';

const switchObj = new Switch({ 
  disabled: true,
  checked: true
});
switchObj.appendTo('#element');
```

The rendered element includes:
```html
aria-disabled="true"
```

### aria-label and aria-labelledby

For better screen reader support, label your switches:

**Method 1: Using HTML label element**
```html
<label for="notifications-switch">Enable Notifications</label>
<input id="notifications-switch" type="checkbox" />
```

**Method 2: Using aria-label**
```html
<input id="settings-switch" type="checkbox" aria-label="Enable Dark Mode" />
```

**Method 3: Using aria-labelledby**
```html
<h3 id="switch-label">System Settings</h3>
<input id="system-switch" type="checkbox" aria-labelledby="switch-label" />
```

### Practical Example with ARIA

```typescript
import { Switch } from '@syncfusion/ej2-buttons';

// Create accessible switch with label
const switchObj = new Switch({ 
  checked: false
});
switchObj.appendTo('#dark-mode-switch');

// Ensure label is associated
const switchLabel = document.querySelector('label[for="dark-mode-switch"]');
if (switchLabel) {
  switchLabel.textContent = 'Dark Mode';
}
```

HTML:
```html
<label for="dark-mode-switch">Dark Mode</label>
<input id="dark-mode-switch" type="checkbox" />
```

## Keyboard Navigation

The Switch component supports keyboard interaction following accessibility standards.

### Keyboard Shortcuts

| Key | Action |
|-----|--------|
| **Space** | Toggle switch state when focused |
| **Tab** | Navigate to/from switch |
| **Shift+Tab** | Navigate backwards to switch |

### Keyboard Navigation Example

```typescript
import { Switch } from '@syncfusion/ej2-buttons';

const switchObj = new Switch({ 
  checked: false
});
switchObj.appendTo('#keyboard-test-switch');

// The switch is keyboard accessible by default
// Users can:
// 1. Tab to the switch
// 2. Press Space to toggle
// 3. Shift+Tab to go back
```

### Focus Indicators

The Switch component shows a visible focus indicator when using keyboard:

```css
/* Focus styles (automatically applied by Syncfusion) */
.e-switch:focus,
.e-switch:focus-visible {
  outline: 2px solid #2196F3;  /* Blue outline */
  outline-offset: 2px;
}
```

**Important:** Never remove focus indicators as they're critical for keyboard users.

### Ensuring Keyboard Accessibility

1. **Don't disable keyboard access** - Users relying on keyboards can't access disabled switches
2. **Maintain focus order** - Tab order should be logical (top to bottom, left to right)
3. **Use proper HTML structure** - Use semantic HTML elements
4. **Test with keyboard only** - Disable mouse to test keyboard navigation

## Screen Reader Support

Screen readers announce switch state and changes to visually impaired users.

### Default Screen Reader Behavior

When a switch is focused, screen readers announce:
- Element type: "Switch"
- Current state: "On" or "Off"
- Disabled status: "Disabled" (if applicable)

Example announcement:
```
"Dark Mode, switch, on, checked"
```

### Improving Screen Reader Announcements

```typescript
import { Switch } from '@syncfusion/ej2-buttons';

// Create switch with clear label
const switchObj = new Switch({ 
  checked: false,
  change: announceChange
});
switchObj.appendTo('#notifications');

function announceChange(args: any): void {
  // Provide feedback for screen readers
  const status = args.checked ? 'enabled' : 'disabled';
  const announcement = `Notifications ${status}`;
  
  // Create live region for announcement
  const liveRegion = document.createElement('div');
  liveRegion.setAttribute('aria-live', 'polite');
  liveRegion.setAttribute('aria-atomic', 'true');
  liveRegion.textContent = announcement;
  document.body.appendChild(liveRegion);
  
  // Remove after announcement
  setTimeout(() => liveRegion.remove(), 1000);
}
```

### Testing with Screen Readers

Popular screen readers to test with:
- **NVDA** (Windows, free)
- **JAWS** (Windows, paid)
- **VoiceOver** (macOS/iOS, built-in)
- **TalkBack** (Android, built-in)

## Color Contrast Guidelines

The Switch component meets WCAG AA color contrast requirements (4.5:1 for text).

### Default Color Contrast

| Theme | Contrast Ratio | Compliance |
|-------|---|---|
| Material | 4.5:1 | ✓ AA |
| Bootstrap 5 | 4.5:1 | ✓ AA |
| Fabric | 4.5:1 | ✓ AA |

### Custom Color Contrast

When customizing colors, ensure adequate contrast:

```css
/* Good contrast - dark on light */
.custom-switch .e-switch-inner {
  background-color: #FFFFFF;  /* Light background */
}

.custom-switch .e-switch-handle {
  background-color: #000000;  /* Dark handle */
}

/* Meets 7:1 AAA ratio */
```

### Checking Contrast

Use these tools to verify contrast:
1. **WebAIM Contrast Checker** - https://webaim.org/resources/contrastchecker/
2. **Chrome DevTools** - Built-in accessibility audit
3. **axe DevTools** - Browser extension
4. **Lighthouse** - Chrome's built-in auditor

```typescript
// Don't do this - insufficient contrast
.poor-contrast .e-switch-inner {
  background-color: #AAAAAA;  /* Gray */
  color: #BBBBBB;            /* Lighter gray - poor contrast */
}
```

## Mobile Accessibility

Switch components on mobile devices need special consideration.

### Touch Target Size

The Switch handle should be at least 44x44 pixels for touch targets:

```css
.e-switch-handle {
  min-width: 44px;
  min-height: 44px;
}
```

### Accessibility Examples

```typescript
import { Switch } from '@syncfusion/ej2-buttons';

// Mobile-friendly switch with good touch target
const switchObj = new Switch({ 
  cssClass: 'mobile-accessible',
  checked: false
});
switchObj.appendTo('#element');
```

CSS:
```css
.mobile-accessible .e-switch {
  min-height: 48px;  /* Increased touch target */
  padding: 8px;
}

.mobile-accessible .e-switch-handle {
  min-width: 44px;
  min-height: 44px;
}
```

### Mobile Screen Reader Testing

Test on:
- iOS with VoiceOver
- Android with TalkBack
- Zoom/magnification levels
- Reduced motion preferences

## Testing and Validation

### Automated Testing Tools

1. **axe DevTools**
   - Browser extension for Chrome/Firefox
   - Detects accessibility violations
   - Provides remediation guidance

2. **Lighthouse**
   ```bash
   # Run Lighthouse accessibility audit
   npm install -g lighthouse
   lighthouse https://yoursite.com --view
   ```

3. **WAVE Tool**
   - https://wave.webaim.org
   - Web accessibility evaluation

### Manual Testing Checklist

- [ ] Tab through all switches - focus moves logically
- [ ] Space key toggles switch when focused
- [ ] Screen reader announces role ("switch"), state ("on"/"off")
- [ ] Color not the only indicator of state
- [ ] Touch targets are 44x44 pixels (mobile)
- [ ] No content is keyboard-inaccessible
- [ ] Focus indicators are visible
- [ ] RTL layout works correctly (if applicable)

### Practical Testing Example

```typescript
import { Switch } from '@syncfusion/ej2-buttons';

// Create test switches
const switches = [
  new Switch({ checked: true }).appendTo('#switch1'),
  new Switch({ disabled: true }).appendTo('#switch2'),
  new Switch({ cssClass: 'custom-style' }).appendTo('#switch3')
];

// Test script for validation
console.log('Accessibility Testing Checklist:');
switches.forEach((sw, i) => {
  console.log(`\nSwitch ${i + 1}:`);
  console.log(`  - Disabled: ${sw.disabled}`);
  console.log(`  - Checked: ${sw.checked}`);
  console.log(`  - CSS Class: ${sw.cssClass}`);
});
```

### Accessibility Validator Integration

```typescript
// Check if accessibility features are properly configured
function validateAccessibility(): boolean {
  const switchElements = document.querySelectorAll('.e-switch');
  let isAccessible = true;

  switchElements.forEach((element) => {
    // Check for label
    const label = document.querySelector(`label[for="${element.id}"]`);
    if (!label) {
      console.warn(`Switch ${element.id} is missing a label`);
      isAccessible = false;
    }

    // Check for aria-label if no associated label
    if (!label && !element.getAttribute('aria-label')) {
      console.warn(`Switch ${element.id} needs aria-label`);
      isAccessible = false;
    }
  });

  return isAccessible;
}
```

## Accessibility Resources

- [W3C ARIA Authoring Practices Guide](https://www.w3.org/WAI/ARIA/apg/patterns/switch/)
- [WebAIM](https://webaim.org/)
- [MDN Accessibility](https://developer.mozilla.org/en-US/docs/Web/Accessibility)
- [WCAG 2.2 Guidelines](https://www.w3.org/WAI/WCAG22/quickref/)

## Accessibility Best Practices

1. **Always include labels** - Use `<label>` or `aria-label`
2. **Test with keyboard** - Don't assume mouse-only usage
3. **Test with screen readers** - Use NVDA, JAWS, or VoiceOver
4. **Maintain focus indicators** - Never remove visible focus
5. **Verify color contrast** - Use contrast checking tools
6. **Test on mobile** - Check touch accessibility
7. **Test with zoom** - Verify at 200% zoom level
8. **Respect user preferences** - Honor `prefers-reduced-motion`

## Next Steps

Explore related features:
- [Form Integration](form-integration.md) - Use switches accessibly in forms
- [States and Behavior](states-and-behavior.md) - Event handling and state
- [Advanced Features](advanced-features.md) - Additional capabilities
