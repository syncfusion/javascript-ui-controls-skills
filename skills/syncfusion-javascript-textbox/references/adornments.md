# Adornments: Custom Content Before and After TextBox

## Table of Contents
- [Overview](#overview)
- [Adding Adornments](#adding-adornments)
- [Common Use Cases](#common-use-cases)
- [Advanced Patterns](#advanced-patterns)
- [Styling Adornments](#styling-adornments)

---

## Overview

Adornments allow you to add custom HTML content before or after the TextBox using `prependTemplate` and `appendTemplate` properties. This is useful for:

- **Icons**: Visual indicators for input type (email, phone, location)
- **Action buttons**: Password visibility toggles, clear buttons, submit icons
- **Units**: Currency symbols ($), measurement units (kg, lbs)
- **Text labels**: Domain extensions (.com), prefixes (@, #)
- **Validation indicators**: Icons showing input validity

Adornments enhance usability by providing immediate visual context and reducing the need for separate label elements.

---

## Adding Adornments

### Basic Prepend (Before Input)

Add content before the TextBox:

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const textbox = new TextBox({
  placeholder: 'Enter your name',
  floatLabelType: 'Auto',
  prependTemplate: '<span class="e-icons e-user"></span>'
});
textbox.appendTo('#textbox');
```

**HTML output:**
```html
<span class="e-icons e-user"></span>
<input placeholder="Enter your name" ... />
```

### Basic Append (After Input)

Add content after the TextBox:

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const textbox = new TextBox({
  placeholder: 'Email',
  floatLabelType: 'Auto',
  appendTemplate: '<span class="e-icons e-envelope"></span>'
});
textbox.appendTo('#textbox');
```

**HTML output:**
```html
<input placeholder="Email" ... />
<span class="e-icons e-envelope"></span>
```

### Prepend and Append Together

Combine both sides for complete framing:

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const textbox = new TextBox({
  placeholder: 'Amount',
  floatLabelType: 'Auto',
  prependTemplate: '<span class="currency">$</span>',
  appendTemplate: '<span class="suffix">.00</span>'
});
textbox.appendTo('#textbox');
```

**Output:** `$ [input] .00`

### Input Separator

Add visual separation between input and adornments:

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const textbox = new TextBox({
  placeholder: 'Search',
  floatLabelType: 'Never',
  prependTemplate: '<span class="e-icons e-search"></span><span class="e-input-separator"></span>',
  appendTemplate: '<span class="e-input-separator"></span><span class="e-icons e-filter"></span>'
});
textbox.appendTo('#textbox');
```

---

## Common Use Cases

### Use Case 1: Email Input with Icon

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const emailBox = new TextBox({
  placeholder: 'Email Address',
  type: 'email',
  floatLabelType: 'Auto',
  prependTemplate: '<span class="e-icons e-people"></span><span class="e-input-separator"></span>'
});
emailBox.appendTo('#email');
```

**When to use:** Contact forms, user registration, newsletter signup

### Use Case 2: Password Visibility Toggle

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const passwordBox = new TextBox({
  type: 'password',
  placeholder: 'Password',
  floatLabelType: 'Auto',
  appendTemplate: '<span id="eye-icon" class="e-icons e-eye"></span>'
});
passwordBox.appendTo('#password');

// Toggle visibility on icon click
const eyeIcon = document.querySelector('#eye-icon');
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

**When to use:** Login forms, password setup, security-sensitive fields

### Use Case 3: Currency Input

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const priceBox = new TextBox({
  placeholder: '0.00',
  floatLabelType: 'Auto',
  type: 'number',
  prependTemplate: '<span class="currency-label">USD $</span><span class="e-input-separator"></span>'
});
priceBox.appendTo('#price');
```

**When to use:** E-commerce, pricing forms, payment inputs

### Use Case 4: Hashtag/Mention Input

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const hashtagBox = new TextBox({
  placeholder: 'Add hashtag',
  floatLabelType: 'Never',
  prependTemplate: '<span class="prefix">#</span>'
});
hashtagBox.appendTo('#hashtag');
```

**When to use:** Social media, tagging systems, search filters

### Use Case 5: Domain Extension

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const usernameBox = new TextBox({
  placeholder: 'username',
  floatLabelType: 'Auto',
  appendTemplate: '<span class="domain">.com</span>'
});
usernameBox.appendTo('#username');
```

**When to use:** Email usernames, URL slugs, domain names

### Use Case 6: Clear Button with Icon

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const searchBox = new TextBox({
  placeholder: 'Search',
  floatLabelType: 'Never',
  appendTemplate: '<span id="clear-btn" class="e-icons e-close" style="cursor: pointer;"></span>'
});
searchBox.appendTo('#search');

// Handle clear
document.querySelector('#clear-btn').addEventListener('click', () => {
  searchBox.value = '';
  searchBox.dataBind();
});
```

**When to use:** Search fields, filter inputs, quick clear

### Use Case 7: Copy Button

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const codeBox = new TextBox({
  placeholder: 'API Key',
  readonly: true,
  value: 'sk-abc123xyz789',
  floatLabelType: 'Auto',
  appendTemplate: '<span id="copy-btn" class="e-icons e-copy" style="cursor: pointer;"></span>'
});
codeBox.appendTo('#apikey');

// Handle copy
document.querySelector('#copy-btn').addEventListener('click', () => {
  navigator.clipboard.writeText(codeBox.value);
  alert('Copied to clipboard!');
});
```

**When to use:** API keys, tokens, codes that need copying

---

## Advanced Patterns

### Interactive Adornment with Event Handling

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const passwordBox = new TextBox({
  type: 'password',
  placeholder: 'Confirm Password',
  floatLabelType: 'Auto',
  appendTemplate: '<span id="toggle-icon" class="e-icons e-eye"></span>'
});
passwordBox.appendTo('#confirm-password');

// Get reference to adornment and add event listener
const toggleIcon = document.querySelector('#toggle-icon');
let isVisible = false;

toggleIcon.addEventListener('click', () => {
  isVisible = !isVisible;
  passwordBox.type = isVisible ? 'text' : 'password';
  toggleIcon.className = isVisible 
    ? 'e-icons e-eye-slash' 
    : 'e-icons e-eye';
});
```

### Validation Status Indicator

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const emailBox = new TextBox({
  placeholder: 'Email Address',
  type: 'email',
  floatLabelType: 'Auto',
  appendTemplate: '<span id="validation-icon" class="e-icons"></span>'
});
emailBox.appendTo('#email');

// Real-time validation
const validationIcon = document.querySelector('#validation-icon');
emailBox.element.addEventListener('input', () => {
  const email = emailBox.value;
  const isValid = /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
  
  if (email === '') {
    validationIcon.className = 'e-icons';
  } else if (isValid) {
    validationIcon.className = 'e-icons e-checkmark';
    emailBox.cssClass = 'e-success';
  } else {
    validationIcon.className = 'e-icons e-close';
    emailBox.cssClass = 'e-error';
  }
  emailBox.dataBind();
});
```

### Custom HTML Adornment

Use any custom HTML with event listeners:

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const uploadBox = new TextBox({
  placeholder: 'File name',
  readonly: true,
  floatLabelType: 'Auto',
  appendTemplate: `
    <label for="fileInput" class="upload-btn" style="cursor: pointer;">
      <span class="e-icons e-upload"></span>
    </label>
    <input type="file" id="fileInput" style="display: none;" accept=".pdf,.doc,.txt" />
  `
});
uploadBox.appendTo('#upload');

// Handle file selection
const fileInput = document.querySelector('#fileInput');
fileInput.addEventListener('change', (e) => {
  const fileName = e.target.files[0]?.name || '';
  uploadBox.value = fileName;
  uploadBox.dataBind();
});
```

---

## Styling Adornments

### CSS Classes for Adornments

```css
/* Style prepend content */
.textbox-prepend {
  color: #666;
  padding: 0 8px;
  font-weight: 500;
}

/* Style append content */
.textbox-append {
  color: #999;
  padding: 0 8px;
  cursor: pointer;
}

/* Hover effect on append */
.textbox-append:hover {
  color: #333;
}

/* Icon styling */
.textbox-icon {
  font-size: 16px;
  line-height: 1;
}
```

### Apply Styling to Adornments

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const textbox = new TextBox({
  placeholder: 'Search',
  prependTemplate: '<span class="textbox-prepend e-icons e-search"></span>',
  appendTemplate: '<span class="textbox-append e-icons e-close"></span>'
});
textbox.appendTo('#textbox');
```

### Responsive Adornment Sizing

```css
/* Normal screen */
.textbox-icon {
  font-size: 18px;
}

/* Tablet */
@media (max-width: 768px) {
  .textbox-icon {
    font-size: 16px;
  }
}

/* Mobile */
@media (max-width: 480px) {
  .textbox-icon {
    font-size: 14px;
  }
}
```

## Related Topics

- [Getting Started](./getting-started.md) - Basic implementation and icon setup
- [Input States](./input-states.md) - Validation states and disabled mode
- [Floating Labels](./floating-labels.md) - Label customization
