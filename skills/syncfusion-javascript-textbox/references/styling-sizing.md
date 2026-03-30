# Styling and Sizing

Customize TextBox appearance with predefined CSS classes, sizing variations, and custom styling options.

## Sizing Options

The TextBox supports three size variations controlled through the `cssClass` property:

### Normal Size (Default)

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const normalBox = new TextBox({
  placeholder: 'Normal size input',
  floatLabelType: 'Auto'
});

normalBox.appendTo('#normal');
```

**Characteristics:**
- Default size
- Balanced padding and font
- Suitable for most forms

### Small Size

Add the `e-small` class for compact inputs:

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const smallBox = new TextBox({
  placeholder: 'Small size input',
  cssClass: 'e-small',
  floatLabelType: 'Auto'
});

smallBox.appendTo('#small');
```

**Characteristics:**
- Reduced padding and font size
- Compact vertical space
- Ideal for dense layouts

### Bigger Size

Add the `e-bigger` class for larger inputs:

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const biggerBox = new TextBox({
  placeholder: 'Bigger size input',
  cssClass: 'e-bigger',
  floatLabelType: 'Auto'
});

biggerBox.appendTo('#bigger');
```

**Characteristics:**
- Increased padding and font size
- Enhanced touch target
- Better for mobile interfaces

### Size Comparison Example

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

// Create all three sizes
const sizes = [
  { id: '#normal', class: '', label: 'Normal' },
  { id: '#small', class: 'e-small', label: 'Small' },
  { id: '#bigger', class: 'e-bigger', label: 'Bigger' }
];

sizes.forEach(size => {
  const textbox = new TextBox({
    placeholder: `Enter text (${size.label})`,
    cssClass: size.class,
    floatLabelType: 'Auto'
  });
  textbox.appendTo(size.id);
});
```

---

## Filled and Outline Modes

Material theme provides two distinct styles:

### Filled Mode

Underline style with filled background:

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const filledBox = new TextBox({
  placeholder: 'Filled style',
  cssClass: 'e-filled',
  floatLabelType: 'Auto'
});

filledBox.appendTo('#filled');
```

**Characteristics:**
- Light background color
- Bottom border accent
- Modern material style

### Outline Mode

Border style (Material theme only):

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const outlineBox = new TextBox({
  placeholder: 'Outline style',
  cssClass: 'e-outline',
  floatLabelType: 'Auto'
});

outlineBox.appendTo('#outlined');
```

**Characteristics:**
- Full border around input
- No background fill
- Clean, minimal appearance

### Combining Size and Style

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

// Small outline input
const compactOutline = new TextBox({
  placeholder: 'Compact outline',
  cssClass: 'e-small e-outline',
  floatLabelType: 'Auto'
});
compactOutline.appendTo('#compact');

// Bigger filled input
const largeFilledBox = new TextBox({
  placeholder: 'Large filled',
  cssClass: 'e-bigger e-filled',
  floatLabelType: 'Auto'
});
largeFilledBox.appendTo('#large');
```

**Note:** Filled and Outline modes are Material theme only.

---

## CSS Customization

### Custom Color Scheme

Define custom styles using CSS classes:

```css
/* Custom blue theme */
.custom-blue .e-input {
  border-color: #2196F3;
  color: #2196F3;
}

.custom-blue .e-input:focus {
  border-color: #1976D2;
  box-shadow: 0 0 0 3px rgba(33, 150, 243, 0.1);
}

.custom-blue .e-float-text {
  color: #2196F3;
}
```

Apply custom class:

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const customBox = new TextBox({
  placeholder: 'Custom blue theme',
  cssClass: 'custom-blue',
  floatLabelType: 'Auto'
});

customBox.appendTo('#textbox');
```

### Background and Text Color Customization

```css
/* Custom background and text */
.e-custom-box {
  --e-input-bg: #f5f5f5;
  --e-input-color: #333;
}

.e-custom-box .e-input {
  background-color: var(--e-input-bg);
  color: var(--e-input-color);
}

.e-custom-box .e-input::placeholder {
  color: #999;
}
```

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const customBgBox = new TextBox({
  placeholder: 'Custom background',
  cssClass: 'e-custom-box',
  floatLabelType: 'Auto'
});

customBgBox.appendTo('#textbox');
```

### Rounded Corners

```css
.e-rounded {
  --border-radius: 8px;
}

.e-rounded .e-input {
  border-radius: var(--border-radius);
}

.e-rounded.e-outline .e-input {
  border-radius: var(--border-radius);
}
```

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const roundedBox = new TextBox({
  placeholder: 'Rounded corners',
  cssClass: 'e-rounded e-outline',
  floatLabelType: 'Auto'
});

roundedBox.appendTo('#textbox');
```

### Shadow Effects

```css
.e-shadow .e-input {
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.e-shadow .e-input:focus {
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.15);
}
```

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const shadowBox = new TextBox({
  placeholder: 'With shadow',
  cssClass: 'e-shadow e-outline',
  floatLabelType: 'Auto'
});

shadowBox.appendTo('#textbox');
```

---

## Theme Integration

### Using Built-in Themes

Import different themes in your CSS:

```css
/* Material theme (default) */
@import '@syncfusion/ej2-inputs/styles/material.css';

/* Fabric theme */
@import '@syncfusion/ej2-inputs/styles/fabric.css';

/* Bootstrap theme */
@import '@syncfusion/ej2-inputs/styles/bootstrap.css';

/* Bootstrap 4 theme */
@import '@syncfusion/ej2-inputs/styles/bootstrap4.css';

/* High contrast theme */
@import '@syncfusion/ej2-inputs/styles/highcontrast.css';
```

Switch themes by changing the import or using dynamic stylesheet loading:

```typescript
// Load theme dynamically
function setTheme(themeName: string) {
  const link = document.querySelector('link[data-theme]') as HTMLLinkElement;
  if (link) {
    link.href = `@syncfusion/ej2-inputs/styles/${themeName}.css`;
  }
}

// Usage
setTheme('bootstrap4');
```

### Theme Studio Integration

Use the [Theme Studio](https://ej2.syncfusion.com/themestudio/) to create custom themes:

1. Visit Theme Studio
2. Customize colors and fonts
3. Export the CSS
4. Import in your project

```css
/* Custom theme exported from Theme Studio */
.e-input {
  --primary-color: #FF6B6B;
  --text-color: #2D3436;
  --border-color: #DFE6E9;
}
```

---

## Responsive Sizing

Adapt TextBox sizes based on screen size:

```css
/* Desktop */
@media (min-width: 1024px) {
  .responsive-textbox {
    width: 100%;
    padding: 12px;
    font-size: 16px;
  }
}

/* Tablet */
@media (min-width: 768px) and (max-width: 1023px) {
  .responsive-textbox {
    width: 100%;
    padding: 10px;
    font-size: 15px;
  }
}

/* Mobile */
@media (max-width: 767px) {
  .responsive-textbox {
    width: 100%;
    padding: 10px;
    font-size: 16px;  /* Prevent zoom on iOS */
  }
}
```

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const responsiveBox = new TextBox({
  placeholder: 'Responsive input',
  cssClass: 'responsive-textbox',
  floatLabelType: 'Auto'
});

responsiveBox.appendTo('#textbox');
```

---

## Complete Styling Example

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

class StyledTextBoxCollection {
  constructor() {
    this.createStyledBoxes();
  }

  private createStyledBoxes() {
    // Standard input
    const standard = new TextBox({
      placeholder: 'Standard input',
      floatLabelType: 'Auto'
    });
    standard.appendTo('#standard');

    // Small filled
    const smallFilled = new TextBox({
      placeholder: 'Small filled',
      cssClass: 'e-small e-filled',
      floatLabelType: 'Auto'
    });
    smallFilled.appendTo('#smallFilled');

    // Bigger outline
    const biggerOutline = new TextBox({
      placeholder: 'Bigger outline',
      cssClass: 'e-bigger e-outline',
      floatLabelType: 'Auto'
    });
    biggerOutline.appendTo('#biggerOutline');

    // Custom styled
    const custom = new TextBox({
      placeholder: 'Custom theme',
      cssClass: 'custom-blue e-rounded e-shadow',
      floatLabelType: 'Auto'
    });
    custom.appendTo('#custom');

    // Success state
    const success = new TextBox({
      placeholder: 'Valid input',
      cssClass: 'e-success e-outline',
      floatLabelType: 'Auto',
      value: 'Validated'
    });
    success.appendTo('#success');

    // Error state
    const error = new TextBox({
      placeholder: 'Invalid input',
      cssClass: 'e-error e-outline',
      floatLabelType: 'Auto'
    });
    error.appendTo('#error');
  }
}

// Usage
const styledCollection = new StyledTextBoxCollection();
```

## Related Topics

- [Getting Started](./getting-started.md) - Basic implementation
- [Input States](./input-states.md) - Validation states
- [Floating Labels](./floating-labels.md) - Label customization
- [Adornments](./adornments.md) - Icons and custom content
