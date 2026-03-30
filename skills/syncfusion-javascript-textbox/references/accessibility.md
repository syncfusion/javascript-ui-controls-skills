# Accessibility

The TextBox component is built with comprehensive accessibility support, meeting WCAG 2.2, Section 508, and international accessibility standards.

## Accessibility Compliance Standards

The TextBox meets the following accessibility requirements:

| Standard | Support | Details |
|----------|---------|---------|
| **WCAG 2.2** | ✓ Full | Meets Level AA compliance |
| **Section 508** | ✓ Full | US federal accessibility requirement |
| **ADA** | ✓ Full | Americans with Disabilities Act compliance |
| **Screen Reader** | ✓ Full | Compatible with JAWS, NVDA, Voice Over |
| **Keyboard Navigation** | ✓ Full | Full keyboard support |
| **Right-to-Left (RTL)** | ✓ Full | Supports RTL languages |
| **Color Contrast** | ✓ Full | WCAG AA contrast ratios |
| **Mobile Device** | ✓ Full | Touch and mobile support |

## ARIA Attributes

The TextBox uses WAI-ARIA attributes to provide semantic information to assistive technologies:

### aria-placeholder

Provides a hint about expected input when using floating labels:

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const textbox = new TextBox({
  placeholder: 'Enter your email address',
  floatLabelType: 'Auto'
  // aria-placeholder is automatically set
});

textbox.appendTo('#email');
```

**ARIA output:**
```html
<input aria-placeholder="Enter your email address" ... />
```

### aria-labelledby

Associates the TextBox with a floating label element:

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const textbox = new TextBox({
  placeholder: 'First Name',
  floatLabelType: 'Auto'
  // aria-labelledby automatically references the floating label
});

textbox.appendTo('#firstName');
```

**Screen reader announces:** "First Name, edit text, focused"

### Custom ARIA Labels

For complex scenarios, add custom ARIA attributes:

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const textbox = new TextBox({
  placeholder: 'Username',
  floatLabelType: 'Auto'
});

textbox.appendTo('#username');

// Add custom ARIA attributes
textbox.addAttributes({
  'aria-label': 'Enter your unique username (3-20 characters)',
  'aria-required': 'true',
  'aria-describedby': 'username-help'
});
```

**HTML with help text:**
```html
<input aria-label="Enter your unique username" aria-required="true" aria-describedby="username-help" />
<small id="username-help">Username must be 3-20 characters</small>
```

---

## Keyboard Navigation

The TextBox supports full keyboard navigation:

### Standard Keyboard Controls

| Key | Action |
|-----|--------|
| **Tab** | Focus/blur TextBox |
| **Shift+Tab** | Focus previous element |
| **Type** | Enter text content |
| **Backspace** | Delete character before cursor |
| **Delete** | Delete character after cursor |
| **Ctrl+A** | Select all text |
| **Ctrl+C** | Copy selected text |
| **Ctrl+X** | Cut selected text |
| **Ctrl+V** | Paste text |
| **Home** | Move cursor to start |
| **End** | Move cursor to end |

### Focus Management Example

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

// Create sequential TextBox fields
const firstBox = new TextBox({
  placeholder: 'First Name',
  floatLabelType: 'Auto'
});
firstBox.appendTo('#firstName');

const lastBox = new TextBox({
  placeholder: 'Last Name',
  floatLabelType: 'Auto'
});
lastBox.appendTo('#lastName');

const emailBox = new TextBox({
  placeholder: 'Email',
  floatLabelType: 'Auto'
});
emailBox.appendTo('#email');

// Users can Tab through all fields in order
```

### Programmatic Focus

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const textbox = new TextBox({
  placeholder: 'Focus me'
});
textbox.appendTo('#textbox');

// Set focus programmatically
setTimeout(() => {
  textbox.element.focus();
}, 1000);
```

---

## Screen Reader Support

Screen readers announce TextBox information automatically:

### Basic Announcement

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const textbox = new TextBox({
  placeholder: 'Enter your name',
  floatLabelType: 'Auto'
});
textbox.appendTo('#textbox');

// Screen reader announces: "Edit text, Enter your name, focused"
```

### With Validation State

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const emailBox = new TextBox({
  placeholder: 'Email',
  type: 'email',
  floatLabelType: 'Auto',
  cssClass: 'e-error'
});
emailBox.appendTo('#email');

// Add error message for screen readers
emailBox.addAttributes({
  'aria-invalid': 'true',
  'aria-describedby': 'email-error'
});
```

**HTML with error message:**
```html
<input aria-invalid="true" aria-describedby="email-error" />
<span id="email-error" role="alert">Please enter a valid email address</span>
```

### Help Text for Complex Inputs

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const passwordBox = new TextBox({
  type: 'password',
  placeholder: 'Password',
  floatLabelType: 'Auto'
});
passwordBox.appendTo('#password');

// Add detailed help text
passwordBox.addAttributes({
  'aria-describedby': 'password-requirements'
});
```

**HTML:**
```html
<input aria-describedby="password-requirements" />
<ul id="password-requirements">
  <li>At least 8 characters</li>
  <li>One uppercase letter</li>
  <li>One number</li>
  <li>One special character</li>
</ul>
```

---

## Right-to-Left (RTL) Support

Enable RTL layout for languages like Arabic, Hebrew, Persian:

### Enable RTL

Set RTL on parent container:

```html
<div dir="rtl">
  <input id="textbox" />
</div>
```

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const textbox = new TextBox({
  placeholder: 'أدخل اسمك',  // Arabic placeholder
  floatLabelType: 'Auto'
});
textbox.appendTo('#textbox');
```

### RTL with Icons

Icons automatically reverse position in RTL:

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const textbox = new TextBox({
  placeholder: 'بحث',  // Arabic: "Search"
  floatLabelType: 'Auto',
  created: () => {
    textbox.addIcon('append', 'e-icons e-search');  // Appears on left in RTL
  }
});

// In RTL context, icons automatically adjust position
textbox.appendTo('#rtlTextbox');
```

### RTL with Adornments

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const textbox = new TextBox({
  placeholder: 'رقم الهاتف',  // Arabic: "Phone Number"
  floatLabelType: 'Auto',
  prependTemplate: '<span class="country-code">+966</span>',  // Swapped in RTL
  appendTemplate: '<span class="e-icons e-phone"></span>'
});
textbox.appendTo('#rtlPhone');
```

---

## Color Contrast

The TextBox maintains WCAG AA color contrast ratios:

### Default Contrast

- **Text on background**: 7:1 (exceeds WCAG AAA)
- **Focus indicator**: Clear, visible outline
- **Validation colors**: High contrast for error/warning/success states

### Verify Contrast

Use tools like:
- [WebAIM Contrast Checker](https://webaim.org/resources/contrastchecker/)
- [Axe DevTools](https://www.deque.com/axe/devtools/)
- [WAVE Extension](https://wave.webaim.org/extension/)

### Custom Contrast Adjustments

If using custom colors, ensure contrast compliance:

```css
/* Ensure sufficient contrast */
.custom-textbox .e-input {
  color: #2D3436;        /* Dark text */
  background: #FFFFFF;   /* Light background */
  border-color: #636E72; /* Visible border */
}

.custom-textbox .e-input::placeholder {
  color: #95A5A6;  /* Sufficient contrast with background */
}
```

---

## Complete Accessible Example

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

class AccessibleForm {
  constructor() {
    this.createNameField();
    this.createEmailField();
    this.createPhoneField();
    this.createPasswordField();
    this.createMessageField();
  }

  private createNameField() {
    const nameBox = new TextBox({
      placeholder: 'Full Name',
      floatLabelType: 'Auto'
    });
    nameBox.appendTo('#name');
    
    nameBox.addAttributes({
      'aria-label': 'Enter your full name',
      'aria-required': 'true'
    });
  }

  private createEmailField() {
    const emailBox = new TextBox({
      placeholder: 'Email Address',
      type: 'email',
      floatLabelType: 'Auto'
    });
    emailBox.appendTo('#email');

    emailBox.addAttributes({
      'aria-label': 'Enter your email address',
      'aria-required': 'true',
      'aria-describedby': 'email-format'
    });
  }

  private createPhoneField() {
    const phoneBox = new TextBox({
      placeholder: 'Phone Number',
      type: 'tel',
      floatLabelType: 'Auto'
    });
    phoneBox.appendTo('#phone');

    phoneBox.addAttributes({
      'aria-label': 'Enter your phone number',
      'aria-describedby': 'phone-format'
    });
  }

  private createPasswordField() {
    const passwordBox = new TextBox({
      placeholder: 'Password',
      type: 'password',
      floatLabelType: 'Auto'
    });
    passwordBox.appendTo('#password');

    passwordBox.addAttributes({
      'aria-label': 'Enter password',
      'aria-required': 'true',
      'aria-describedby': 'password-requirements'
    });

    // Toggle visibility
    const toggleBtn = document.querySelector('#toggle-password');
    if (toggleBtn) {
      toggleBtn.addEventListener('click', () => {
        const newType = passwordBox.type === 'password' ? 'text' : 'password';
        passwordBox.type = newType;
        toggleBtn.setAttribute('aria-label', 
          newType === 'password' ? 'Show password' : 'Hide password'
        );
      });
    }
  }

  private createMessageField() {
    const messageBox = new TextBox({
      placeholder: 'Your Message',
      multiline: true,
      floatLabelType: 'Auto'
    });
    messageBox.appendTo('#message');

    messageBox.addAttributes({
      'aria-label': 'Enter your message',
      'aria-describedby': 'message-char-count'
    });

    // Character counter for screen readers
    messageBox.element.addEventListener('input', () => {
      const count = messageBox.value.length;
      const counter = document.querySelector('#message-char-count');
      if (counter) {
        counter.textContent = `${count} characters entered`;
      }
    });
  }
}

// Usage
const accessibleForm = new AccessibleForm();
```

## Testing for Accessibility

### Automated Testing

Use axe-core for automated accessibility checks:

```typescript
import * as axe from 'axe-core';

// Run accessibility audit
axe.run(document.querySelector('#textbox'), (results) => {
  console.log('Accessibility violations:', results.violations);
  console.log('Passes:', results.passes);
});
```

### Manual Testing Checklist

- [ ] Tab through all fields in logical order
- [ ] Test with screen readers (JAWS, NVDA)
- [ ] Verify focus indicators are visible
- [ ] Check color contrast ratios
- [ ] Test RTL layout if applicable
- [ ] Verify error messages announce properly
- [ ] Test keyboard-only navigation

## Related Topics

- [Getting Started](./getting-started.md) - Basic implementation
- [Input States](./input-states.md) - Validation states and feedback
- [Floating Labels](./floating-labels.md) - Label implementation
- [WCAG Guidelines](https://www.w3.org/WAI/WCAG21/quickref/) - Official WCAG documentation
