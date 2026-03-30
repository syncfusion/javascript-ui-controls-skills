---
name: syncfusion-javascript-textbox
description: Create and configure Syncfusion JavaScript TextBox components. Use this skill when building text input fields, implementing floating labels, adding adornments (icons/buttons), creating multiline text areas, applying validation states, or customizing TextBox styling. Covers single-line inputs, read-only fields, disabled states, sizing variations, and accessibility features.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing TextBox

The TextBox is a lightweight input control that accepts text and allows customization with floating labels, adornments, validation states, and styling. This skill guides you through implementing and configuring TextBox components for various use cases.

## When to Use This Skill

Use this skill when you need to:
- **Create basic text inputs** with placeholders and initial values
- **Add floating labels** that animate when focused or filled
- **Enhance inputs with adornments** (icons, buttons, prefixes/suffixes)
- **Implement validation states** (error, warning, success)
- **Build multiline text areas** with auto-resizing
- **Apply custom styling** with sizing variations and themes
- **Support accessibility** with ARIA attributes and keyboard navigation
- **Add interactive features** like clear buttons or password toggles

## Control Overview

The TextBox is essential for collecting user input in modern applications. It provides:
- **Flexible input modes**: Single-line, multiline, read-only, disabled
- **Visual feedback**: Validation states, floating labels, styling options
- **Extensible design**: Support for icons, buttons, and custom templates
- **Accessibility compliance**: WCAG 2.2, screen reader support, keyboard navigation

## Installation

The TextBox component is available in the `@syncfusion/ej2-inputs` package:

```typescript
npm install @syncfusion/ej2-inputs @syncfusion/ej2-base
```

Import required CSS themes:

```typescript
import '@syncfusion/ej2-base/styles/material.css';
import '@syncfusion/ej2-inputs/styles/material.css';
```

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation and package setup
- Basic TextBox implementation in TypeScript
- Importing CSS styles and themes
- Adding floating labels
- Creating TextBox with icons programmatically
- Essential properties: `placeholder`, `value`, `floatLabelType`

### Input States and Styling
📄 **Read:** [references/input-states.md](references/input-states.md)
- Validation states (error, warning, success)
- Disabled and read-only states
- Clear button functionality
- Setting and retrieving values programmatically
- When to use each state for user feedback

### Adornments and Icons
📄 **Read:** [references/adornments.md](references/adornments.md)
- Adding adornments with `prependTemplate` and `appendTemplate`
- Icon placement and styling
- Creating action buttons (password toggle, clear)
- Input separators and visual organization
- Event handling for interactive elements

### Floating Labels
📄 **Read:** [references/floating-labels.md](references/floating-labels.md)
- Float label types: Auto, Always, Never
- Floating labels with icons
- Custom label styling and colors
- Label positioning and alignment

### Multiline TextBox
📄 **Read:** [references/multiline-textbox.md](references/multiline-textbox.md)
- Creating multiline TextBox with `multiline: true`
- Using textarea elements
- Auto-resizing implementation
- Text length limiting with `maxLength`
- Disabling resize functionality

### Styling and Sizing
📄 **Read:** [references/styling-sizing.md](references/styling-sizing.md)
- Size variations: normal, small (`e-small`), bigger (`e-bigger`)
- Filled and outline modes (`e-filled`, `e-outline`)
- CSS customization with `cssClass`
- Theme Studio integration
- Background and text color customization

### Accessibility
📄 **Read:** [references/accessibility.md](references/accessibility.md)
- WCAG 2.2 and Section 508 compliance
- ARIA attributes: `aria-placeholder`, `aria-labelledby`
- Keyboard navigation support
- Screen reader compatibility
- Right-to-left (RTL) support

## Quick Start Example

### Basic TextBox

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

// Create a simple text input
const textbox = new TextBox({
  placeholder: 'Enter your name'
});

textbox.appendTo('#textbox');
```

### TextBox with Floating Label

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const textbox = new TextBox({
  placeholder: 'Email Address',
  floatLabelType: 'Auto'  // Label floats on focus or when filled
});

textbox.appendTo('#email');
```

### TextBox with Icon

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const textbox = new TextBox({
  placeholder: 'Search',
  floatLabelType: 'Never',
  created: () => {
    textbox.addIcon('append', 'e-icons e-search');
  }
});

textbox.appendTo('#search');
```

### TextBox with Validation State

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

// Error state
const errorBox = new TextBox({
  placeholder: 'Username',
  cssClass: 'e-error',
  value: ''
});
errorBox.appendTo('#error');

// Success state
const successBox = new TextBox({
  placeholder: 'Confirmed',
  cssClass: 'e-success',
  value: 'valid input'
});
successBox.appendTo('#success');
```

## Common Patterns

### Pattern 1: Email Input with Icon

```typescript
const emailBox = new TextBox({
  placeholder: 'Email Address',
  floatLabelType: 'Auto',
  created: () => {
    emailBox.addIcon('prepend', 'e-icons e-people');
  }
});
emailBox.appendTo('#email');
```

**When to use:** Collecting email addresses in forms or contact sections.

### Pattern 2: Password Toggle

```typescript
const passwordBox = new TextBox({
  placeholder: 'Password',
  type: 'password',
  floatLabelType: 'Auto',
  created: () => {
    passwordBox.addIcon('append', 'e-icons e-eye');
  }
});
passwordBox.appendTo('#password');

// Toggle visibility on icon click
const eyeIcon = document.querySelector('#password .e-eye');
eyeIcon.addEventListener('click', () => {
  if (passwordBox.type === 'password') {
    passwordBox.type = 'text';
    eyeIcon.className = 'e-icons e-eye-slash';
  } else {
    passwordBox.type = 'password';
    eyeIcon.className = 'e-icons e-eye';
  }
  passwordBox.dataBind();
});
```

**When to use:** Secure password entry with visibility toggle option.

### Pattern 3: Multiline Address Input

```typescript
const addressBox = new TextBox({
  placeholder: 'Enter your address',
  floatLabelType: 'Auto',
  multiline: true,
  created: (args) => {
    addressBox.addAttributes({ rows: '4' });
  }
});
addressBox.appendTo('#address');
```

**When to use:** Collecting longer text like addresses, descriptions, or comments.

### Pattern 4: Inline Validation Feedback

```typescript
const usernameBox = new TextBox({
  placeholder: 'Username (3+ characters)',
  floatLabelType: 'Auto'
});
usernameBox.appendTo('#username');

usernameBox.element.addEventListener('blur', (e) => {
  const value = usernameBox.value;
  if (value.length < 3) {
    usernameBox.cssClass = 'e-error';
  } else {
    usernameBox.cssClass = 'e-success';
  }
  usernameBox.dataBind();
});
```

**When to use:** Real-time validation feedback as users type or leave the field.

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `placeholder` | string | Hint text displayed when empty |
| `value` | string | Current input value |
| `floatLabelType` | 'Never' \| 'Always' \| 'Auto' | Label behavior (default: 'Never') |
| `multiline` | boolean | Enable multiline mode (textarea) |
| `cssClass` | string | Custom CSS classes (e.g., 'e-error', 'e-success', 'e-small') |
| `type` | string | Input type ('text', 'password', 'email', etc.) |
| `disabled` | boolean | Disable the input |
| `readonly` | boolean | Make input read-only |
| `prependTemplate` | string | HTML content before the input |
| `appendTemplate` | string | HTML content after the input |
| `created` | function | Event fired when component is created |

## Related Resources

- [TextBox API Reference](https://ej2.syncfusion.com/documentation/api/textbox/)
- [Syncfusion Documentation](https://ej2.syncfusion.com/documentation/introduction/)
- [Theme Studio](https://ej2.syncfusion.com/themestudio/)
