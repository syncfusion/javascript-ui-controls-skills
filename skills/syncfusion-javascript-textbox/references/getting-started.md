# Getting Started with TextBox

This guide covers the foundational steps to install, configure, and implement the TextBox component in your TypeScript application.

## Installation and Setup

### 1. Install Dependencies

Install the Syncfusion TextBox component via npm:

```bash
npm install @syncfusion/ej2-inputs @syncfusion/ej2-base
```

The `@syncfusion/ej2-inputs` package contains all input components, while `@syncfusion/ej2-base` provides core utilities.

### 2. Import Styles

Add the required CSS themes to your main CSS file or TypeScript entry point:

```css
/* Import base theme */
@import '@syncfusion/ej2-base/styles/material.css';

/* Import inputs theme */
@import '@syncfusion/ej2-inputs/styles/material.css';
```

**Available themes:** `material.css`, `fabric.css`, `bootstrap.css`, `bootstrap4.css`, `highcontrast.css`

### 3. HTML Setup

Add an input element to your HTML:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>TextBox Sample</title>
  </head>
  <body>
    <!-- Element for TextBox -->
    <input id="textbox" />
  </body>
</html>
```

## Basic Implementation

### Simple TextBox

The most basic TextBox requires just an element ID and minimal configuration:

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

// Create and render TextBox
const textbox = new TextBox({
  placeholder: 'Enter your name'
});

textbox.appendTo('#textbox');
```

**Output:** A text input with placeholder "Enter your name"

### TextBox with Initial Value

Set an initial value using the `value` property:

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const textbox = new TextBox({
  placeholder: 'Enter your email',
  value: 'user@example.com'
});

textbox.appendTo('#textbox');
```

### TextBox with Floating Label

Floating labels provide elegant label animations when the input is focused or contains a value:

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const textbox = new TextBox({
  placeholder: 'First Name',
  floatLabelType: 'Auto'  // Label floats on focus or when filled
});

textbox.appendTo('#textbox');
```

**Float label types:**
- `'Never'` (default): Label never floats
- `'Auto'`: Label floats on focus or when filled
- `'Always'`: Label always floats above the input

## Adding Icons

Icons enhance the visual appearance and provide context about the expected input type.

### Add Icon Programmatically

Use the `addIcon()` method in the `created` event:

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const textbox = new TextBox({
  placeholder: 'Enter Date',
  created: () => {
    textbox.addIcon('append', 'e-icons e-calendar');
  }
});

textbox.appendTo('#textbox');
```

**Icon placement options:**
- `'append'`: Place icon after the input (right side)
- `'prepend'`: Place icon before the input (left side)

### Common Syncfusion Icons

| Icon Class | Usage |
|-----------|-------|
| `e-icons e-search` | Search inputs |
| `e-icons e-calendar` | Date pickers |
| `e-icons e-people` | User/email inputs |
| `e-icons e-envelope` | Email inputs |
| `e-icons e-phone` | Phone inputs |
| `e-icons e-eye` | Password visibility toggle |
| `e-icons e-location` | Location inputs |

### TextBox with Both Prepend and Append Icons

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const textbox = new TextBox({
  placeholder: 'Search',
  created: () => {
    textbox.addIcon('prepend', 'e-icons e-search');
    textbox.addIcon('append', 'e-icons e-close');
  }
});

textbox.appendTo('#textbox');
```

## CSS Theme Classes

Syncfusion provides pre-defined CSS classes for common styling needs:

### Material Theme Classes

- `e-small` - Smaller text input
- `e-bigger` - Larger text input
- `e-filled` - Filled style (Material theme only)
- `e-outline` - Outline style (Material theme only)
- `e-error` - Error validation state
- `e-warning` - Warning validation state
- `e-success` - Success validation state

### Apply CSS Classes

Use the `cssClass` property:

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

// Small textbox with outline style
const smallBox = new TextBox({
  placeholder: 'Small Input',
  cssClass: 'e-small e-outline'
});
smallBox.appendTo('#small');

// Large textbox with filled style
const largeBox = new TextBox({
  placeholder: 'Large Input',
  cssClass: 'e-bigger e-filled'
});
largeBox.appendTo('#large');
```

## Complete Getting Started Example

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

// Basic TextBox
const basicBox = new TextBox({
  placeholder: 'Username',
  value: 'john_doe'
});
basicBox.appendTo('#basic');

// TextBox with floating label
const floatBox = new TextBox({
  placeholder: 'Email Address',
  floatLabelType: 'Auto'
});
floatBox.appendTo('#float');

// TextBox with icon
const iconBox = new TextBox({
  placeholder: 'Search Products',
  created: () => {
    iconBox.addIcon('append', 'e-icons e-search');
  }
});
iconBox.appendTo('#search');

// Styled TextBox
const styledBox = new TextBox({
  placeholder: 'Comments',
  cssClass: 'e-small e-outline',
  floatLabelType: 'Auto'
});
styledBox.appendTo('#styled');
```

## Troubleshooting

**Issue:** TextBox not appearing
- **Solution:** Ensure CSS imports are included and element ID matches

**Issue:** Placeholder text not visible
- **Solution:** Add `placeholder` property to TextBox configuration

**Issue:** Icon not showing
- **Solution:** Verify icon class name (e.g., `e-icons e-search`) and CSS imports

## Related Topics

- [Input States and Styling](./input-states.md) - Validation states and disabled/readonly modes
- [Floating Labels](./floating-labels.md) - Advanced label customization
- [Adornments](./adornments.md) - Custom content before/after input
