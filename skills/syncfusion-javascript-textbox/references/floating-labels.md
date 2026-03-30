# Floating Labels

Floating labels automatically animate above the TextBox when focused or filled with a value, improving form readability and user experience.

## Float Label Types

The TextBox supports three floating label behaviors through the `floatLabelType` property:

### 1. Auto (Default Animation)

Label floats when input is focused or contains a value:

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const textbox = new TextBox({
  placeholder: 'Enter your name',
  floatLabelType: 'Auto'
});

textbox.appendTo('#textbox');
```

**Behavior:**
- Label starts as placeholder text in the input
- On focus: Label floats above the input
- On blur (if empty): Label returns to placeholder position
- On blur (if filled): Label remains floating

**Best for:** Most forms and data entry scenarios

### 2. Always (Permanent Float)

Label permanently floats above the input:

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const textbox = new TextBox({
  placeholder: 'Email Address',
  floatLabelType: 'Always'
});

textbox.appendTo('#textbox');
```

**Behavior:**
- Label always displays above the input
- Provides visual clarity
- Useful when space is not a constraint

**Best for:** Data entry forms with clear field labels

### 3. Never (No Float)

Label never floats; only placeholder text appears:

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const textbox = new TextBox({
  placeholder: 'Search',
  floatLabelType: 'Never'
});

textbox.appendTo('#textbox');
```

**Behavior:**
- Placeholder text is used instead of a label
- Compact interface
- Traditional form appearance

**Best for:** Minimal interfaces, search inputs, compact layouts

## Floating Labels with Icons

Combine floating labels with icons for enhanced visual guidance:

### Append Icon

Icon appears on the right side:

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const textbox = new TextBox({
  placeholder: 'Select date',
  floatLabelType: 'Auto',
  created: () => {
    textbox.addIcon('append', 'e-icons e-calendar');
  }
});

textbox.appendTo('#textbox');
```

### Prepend Icon

Icon appears on the left side:

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const textbox = new TextBox({
  placeholder: 'Phone number',
  floatLabelType: 'Auto',
  created: () => {
    textbox.addIcon('prepend', 'e-icons e-phone');
  }
});

textbox.appendTo('#textbox');
```

### Both Prepend and Append Icons

Use icons on both sides:

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const textbox = new TextBox({
  placeholder: 'Enter amount',
  floatLabelType: 'Auto',
  created: () => {
    textbox.addIcon('prepend', 'e-icons e-dollar');
    textbox.addIcon('append', 'e-icons e-check');
  }
});

textbox.appendTo('#textbox');
```

## Floating Label with Adornments

Use adornments (custom HTML content) with floating labels:

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const textbox = new TextBox({
  placeholder: 'Username',
  floatLabelType: 'Auto',
  prependTemplate: '<span class="e-icons e-user"></span><span class="e-input-separator"></span>',
  appendTemplate: '<span class="e-icons e-check"></span>'
});

textbox.appendTo('#textbox');
```

## Styling Floating Labels

### Change Floating Label Color

Use CSS to customize label appearance:

```css
/* Change floating label color */
.e-float-text {
  color: #2196F3;  /* Blue */
}

/* On focus */
.e-input:focus ~ .e-float-text,
.e-input.e-valid ~ .e-float-text {
  color: #4CAF50;  /* Green */
}
```

Apply in TypeScript:

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const textbox = new TextBox({
  placeholder: 'Email Address',
  floatLabelType: 'Auto',
  cssClass: 'custom-label'
});

textbox.appendTo('#textbox');
```

### Custom Label Styling

```css
/* Custom wrapper styling */
.e-float-input.custom-label {
  font-size: 14px;
}

/* Custom label text */
.e-float-input.custom-label .e-float-text {
  font-weight: 600;
  color: #555;
  font-size: 12px;
}

/* Floating line styling */
.e-float-input.custom-label .e-float-line {
  background: linear-gradient(90deg, #2196F3, #FF5722);
}
```

### Floating Label Size Variations

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

// Small floating label input
const smallBox = new TextBox({
  placeholder: 'Compact field',
  floatLabelType: 'Auto',
  cssClass: 'e-small'
});
smallBox.appendTo('#small');

// Large floating label input
const largeBox = new TextBox({
  placeholder: 'Spacious field',
  floatLabelType: 'Auto',
  cssClass: 'e-bigger'
});
largeBox.appendTo('#large');
```

## Floating Label with Different States

### With Validation States

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

// Error state with floating label
const errorBox = new TextBox({
  placeholder: 'Username',
  floatLabelType: 'Auto',
  cssClass: 'e-error'
});
errorBox.appendTo('#error');

// Success state with floating label
const successBox = new TextBox({
  placeholder: 'Confirmed',
  floatLabelType: 'Auto',
  cssClass: 'e-success',
  value: 'valid_input'
});
successBox.appendTo('#success');
```

### With Disabled State

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const disabledBox = new TextBox({
  placeholder: 'Disabled field',
  floatLabelType: 'Always',
  disabled: true,
  value: 'Disabled'
});

disabledBox.appendTo('#disabled');
```

### With Read-Only State

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const readonlyBox = new TextBox({
  placeholder: 'Read-only field',
  floatLabelType: 'Always',
  readonly: true,
  value: 'Cannot edit'
});

readonlyBox.appendTo('#readonly');
```

## Complete Example: Contact Form

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

// First name with floating label
const firstName = new TextBox({
  placeholder: 'First Name',
  floatLabelType: 'Auto'
});
firstName.appendTo('#firstName');

// Last name with floating label
const lastName = new TextBox({
  placeholder: 'Last Name',
  floatLabelType: 'Auto'
});
lastName.appendTo('#lastName');

// Email with icon and floating label
const email = new TextBox({
  placeholder: 'Email Address',
  type: 'email',
  floatLabelType: 'Auto',
  created: () => {
    email.addIcon('prepend', 'e-icons e-envelope');
  }
});
email.appendTo('#email');

// Phone with icon and floating label
const phone = new TextBox({
  placeholder: 'Phone Number',
  type: 'tel',
  floatLabelType: 'Auto',
  created: () => {
    phone.addIcon('prepend', 'e-icons e-phone');
  }
});
phone.appendTo('#phone');

// Subject with floating label
const subject = new TextBox({
  placeholder: 'Subject',
  floatLabelType: 'Auto'
});
subject.appendTo('#subject');

// Message with floating label (multiline)
const message = new TextBox({
  placeholder: 'Your Message',
  floatLabelType: 'Auto',
  multiline: true
});
message.appendTo('#message');
```

## Best Practices

1. **Use 'Auto' for most forms** - Balances aesthetics with functionality
2. **Use 'Always' when space allows** - Clear indication of field purpose
3. **Use 'Never' for compact interfaces** - Minimal space consumption
4. **Combine with icons** - Provides additional visual context
5. **Test on mobile** - Ensure labels don't overlap content
6. **Use meaningful placeholders** - Placeholders become floating labels
7. **Apply consistent styling** - Use cssClass for uniform appearance

## Troubleshooting

**Issue:** Floating label not appearing
- Verify `floatLabelType` is set to 'Auto' or 'Always'
- Check that CSS imports are included
- Ensure placeholder text is defined

**Issue:** Label overlaps with content
- Reduce font size using CSS
- Add more padding to input
- Use 'Auto' type instead of 'Always' when field is empty

## Related Topics

- [Getting Started](./getting-started.md) - Basic implementation
- [Input States](./input-states.md) - Validation and disabled states
- [Adornments](./adornments.md) - Icons and custom content
- [Styling and Sizing](./styling-sizing.md) - CSS customization
