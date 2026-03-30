# Input States and Styling

## Table of Contents
- [Validation States](#validation-states)
- [Disabled State](#disabled-state)
- [Read-Only State](#read-only-state)
- [Clear Button](#clear-button)
- [Value Manipulation](#value-manipulation)

---

## Validation States

The TextBox supports three validation states that provide visual feedback to users: error, warning, and success. These are implemented using CSS classes applied through the `cssClass` property.

### Error State

Indicates invalid input or validation failure:

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const errorBox = new TextBox({
  placeholder: 'Username',
  cssClass: 'e-error',
  value: ''
});
errorBox.appendTo('#error');
```

**Usage:** Display when:
- Required field is empty
- Input doesn't match expected format
- Validation fails (e.g., invalid email)

**Visual effect:** Red border and text

### Warning State

Indicates caution or incomplete input:

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const warningBox = new TextBox({
  placeholder: 'Password Strength',
  cssClass: 'e-warning',
  value: '123'
});
warningBox.appendTo('#warning');
```

**Usage:** Display when:
- Password is weak
- Input is incomplete but not invalid
- Minimum length not met

**Visual effect:** Yellow/orange border and text

### Success State

Indicates valid or confirmed input:

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const successBox = new TextBox({
  placeholder: 'Confirmed Email',
  cssClass: 'e-success',
  value: 'user@example.com'
});
successBox.appendTo('#success');
```

**Usage:** Display when:
- Input is valid
- Validation passed
- Email confirmed or password matches

**Visual effect:** Green border and text

### Dynamic Validation State

Update validation state based on user input:

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const emailBox = new TextBox({
  placeholder: 'Email Address',
  floatLabelType: 'Auto'
});
emailBox.appendTo('#email');

// Validate on blur
emailBox.element.addEventListener('blur', (e) => {
  const email = emailBox.value;
  const isValid = /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
  
  if (email === '') {
    emailBox.cssClass = '';
  } else if (isValid) {
    emailBox.cssClass = 'e-success';
  } else {
    emailBox.cssClass = 'e-error';
  }
  emailBox.dataBind();
});
```

### Combining Validation States with Other Styles

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

// Error state with small size
const errorSmall = new TextBox({
  placeholder: 'Username',
  cssClass: 'e-error e-small',
  floatLabelType: 'Auto'
});
errorSmall.appendTo('#error-small');

// Success state with outline style
const successOutline = new TextBox({
  placeholder: 'Confirmed',
  cssClass: 'e-success e-outline',
  floatLabelType: 'Auto'
});
successOutline.appendTo('#success-outline');
```

---

## Disabled State

A disabled TextBox is not interactive and appears grayed out. Users cannot type, select, or focus on the input.

### Enable/Disable TextBox

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const textbox = new TextBox({
  placeholder: 'Enter your name',
  disabled: false  // default
});
textbox.appendTo('#textbox');

// Disable the TextBox
textbox.enabled = false;
textbox.dataBind();

// Re-enable
textbox.enabled = true;
textbox.dataBind();
```

**Properties:**
- `disabled: true` - Create disabled TextBox (in constructor)
- `textbox.enabled = false` - Disable after creation

### Use Cases

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

// Conditional field (disabled until condition met)
const phoneBox = new TextBox({
  placeholder: 'Phone Number',
  disabled: true,
  floatLabelType: 'Auto'
});
phoneBox.appendTo('#phone');

// Enable phone field when email is provided
const emailBox = new TextBox({
  placeholder: 'Email Address',
  floatLabelType: 'Auto'
});
emailBox.appendTo('#email');

emailBox.element.addEventListener('input', () => {
  if (emailBox.value) {
    phoneBox.enabled = true;
  } else {
    phoneBox.enabled = false;
  }
  phoneBox.dataBind();
});
```

---

## Read-Only State

A read-only TextBox displays its value but prevents user editing. Unlike disabled, read-only fields can be focused and selected.

### Set Read-Only Mode

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const textbox = new TextBox({
  placeholder: 'Confirmation Code',
  value: '123456',
  readonly: true
});
textbox.appendTo('#textbox');
```

**Properties:**
- `readonly: true` - Create read-only TextBox (in constructor)
- `textbox.readonly = true` - Set after creation

### Use Cases

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

// Display-only field (like confirmation numbers)
const confirmBox = new TextBox({
  placeholder: 'Order ID',
  value: 'ORD-2026-001234',
  readonly: true,
  floatLabelType: 'Auto'
});
confirmBox.appendTo('#order');

// Reference code (copy-able but not editable)
const refBox = new TextBox({
  placeholder: 'Reference Code',
  value: 'REF-ABC-XYZ-123',
  readonly: true,
  cssClass: 'e-outline'
});
refBox.appendTo('#ref');
```

### Disabled vs Read-Only Comparison

| Aspect | Disabled | Read-Only |
|--------|----------|-----------|
| Appearance | Grayed out | Normal |
| Focusable | No | Yes |
| Selectable | No | Yes |
| Value submitted | No | Yes |
| Copy possible | No | Yes |

---

## Clear Button

The clear button allows users to quickly empty the TextBox. It appears only when the field has a value.

### Enable Clear Button

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const textbox = new TextBox({
  placeholder: 'Search',
  showClearButton: true,
  floatLabelType: 'Auto'
});
textbox.appendTo('#textbox');
```

### Clear Button with Icon

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const searchBox = new TextBox({
  placeholder: 'Search Products',
  showClearButton: true,
  created: () => {
    searchBox.addIcon('prepend', 'e-icons e-search');
  }
});
searchBox.appendTo('#search');
```

### Clear Button Event Handling

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const textbox = new TextBox({
  placeholder: 'Enter search term',
  showClearButton: true
});
textbox.appendTo('#textbox');

// Detect when clear button is clicked
const clearBtn = textbox.element.parentElement.querySelector('.e-clear-icon');
if (clearBtn) {
  clearBtn.addEventListener('click', () => {
    console.log('Clear button clicked');
    // Perform additional cleanup if needed
  });
}
```

---

## Value Manipulation

Programmatically get, set, and modify TextBox values.

### Get Current Value

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const textbox = new TextBox({
  placeholder: 'Enter name',
  value: 'John'
});
textbox.appendTo('#textbox');

// Get value
const currentValue = textbox.value;
console.log(currentValue);  // Output: "John"
```

### Set Value Programmatically

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const textbox = new TextBox({
  placeholder: 'Enter name'
});
textbox.appendTo('#textbox');

// Set value
textbox.value = 'Jane';
textbox.dataBind();  // Update DOM
```

### Clear Value

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const textbox = new TextBox({
  placeholder: 'Enter name',
  value: 'John'
});
textbox.appendTo('#textbox');

// Clear value
textbox.value = '';
textbox.dataBind();
```

### Real-Time Value Monitoring

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const textbox = new TextBox({
  placeholder: 'Username',
  floatLabelType: 'Auto'
});
textbox.appendTo('#textbox');

// Listen for input changes
textbox.element.addEventListener('input', (e) => {
  const value = textbox.value;
  const charCount = value.length;
  
  console.log(`Current value: ${value}`);
  console.log(`Character count: ${charCount}`);
  
  // Display character count
  document.querySelector('#charCount').textContent = charCount;
});
```

### Complete Example: Form Field Interaction

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

// First name field
const firstNameBox = new TextBox({
  placeholder: 'First Name',
  floatLabelType: 'Auto'
});
firstNameBox.appendTo('#firstName');

// Last name field
const lastNameBox = new TextBox({
  placeholder: 'Last Name',
  floatLabelType: 'Auto'
});
lastNameBox.appendTo('#lastName');

// Full name display (read-only)
const fullNameBox = new TextBox({
  placeholder: 'Full Name',
  readonly: true,
  floatLabelType: 'Always'
});
fullNameBox.appendTo('#fullName');

// Update full name when first or last name changes
const updateFullName = () => {
  const fullName = `${firstNameBox.value} ${lastNameBox.value}`.trim();
  fullNameBox.value = fullName;
  fullNameBox.dataBind();
};

firstNameBox.element.addEventListener('input', updateFullName);
lastNameBox.element.addEventListener('input', updateFullName);
```

## Related Topics

- [Floating Labels](./floating-labels.md) - Label customization
- [Adornments](./adornments.md) - Icons and custom content
- [Styling and Sizing](./styling-sizing.md) - CSS classes and themes
