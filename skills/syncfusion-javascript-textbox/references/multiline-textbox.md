# Multiline TextBox

## Table of Contents
- [Overview](#overview)
- [Creating Multiline TextBox](#creating-multiline-textbox)
- [Floating Labels](#floating-labels)
- [Auto-Resizing](#auto-resizing)
- [Text Length Limiting](#text-length-limiting)
- [Disable Resizing](#disable-resizing)

---

## Overview

The multiline TextBox is a textarea-like component that accepts multiple lines of text. It's perfect for collecting longer content like addresses, descriptions, comments, and messages.

**Key features:**
- Accepts one or more lines of text
- Vertical resize capability by default
- Can auto-resize based on content
- Supports floating labels
- Text length limiting
- Customizable sizing

---

## Creating Multiline TextBox

### Using the `multiline` Property

Convert a standard TextBox to multiline by setting `multiline: true`:

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const multilineBox = new TextBox({
  placeholder: 'Enter your address',
  multiline: true
});

multilineBox.appendTo('#address');
```

### Using a Textarea Element

Alternatively, render on a textarea HTML element:

```html
<textarea id="comments"></textarea>
```

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const multilineBox = new TextBox({
  placeholder: 'Leave your feedback',
  value: 'Your comments here...'
});

multilineBox.appendTo('#comments');
```

### Multiline with Initial Value

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const multilineBox = new TextBox({
  placeholder: 'Enter your address',
  multiline: true,
  value: 'Mr. Dodsworth Dodsworth, System Analyst, Studio 103, The Business Center'
});

multilineBox.appendTo('#address');
```

### Multiline with Specific Row Count

Set initial rows using the `created` event:

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const multilineBox = new TextBox({
  placeholder: 'Enter your bio',
  multiline: true,
  created: (args) => {
    multilineBox.addAttributes({ rows: '5' });  // 5 visible rows
  }
});

multilineBox.appendTo('#bio');
```

---

## Floating Labels

### Basic Floating Label

Apply floating label to multiline TextBox:

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const multilineBox = new TextBox({
  placeholder: 'Enter your address',
  multiline: true,
  floatLabelType: 'Auto'
});

multilineBox.appendTo('#address');
```

### Float Label Types

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

// Auto - floats on focus or when filled
const autoFloat = new TextBox({
  placeholder: 'Address (Auto)',
  multiline: true,
  floatLabelType: 'Auto'
});
autoFloat.appendTo('#auto');

// Always - label always floats
const alwaysFloat = new TextBox({
  placeholder: 'Address (Always)',
  multiline: true,
  floatLabelType: 'Always'
});
alwaysFloat.appendTo('#always');

// Never - label never floats
const neverFloat = new TextBox({
  placeholder: 'Address (Never)',
  multiline: true,
  floatLabelType: 'Never'
});
neverFloat.appendTo('#never');
```

---

## Auto-Resizing

### Implement Auto-Resizing

Create a multiline TextBox that automatically grows as users type:

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const autoResizeBox = new TextBox({
  placeholder: 'Enter your address',
  multiline: true,
  floatLabelType: 'Auto',
  value: 'Mr. Dodsworth Dodsworth, System Analyst, Studio 103, The Business Center',
  created: (args) => {
    // Set initial height based on content
    autoResizeBox.addAttributes({ rows: '1' });
    autoResizeBox.element.style.height = 'auto';
    autoResizeBox.element.style.height = (autoResizeBox.element.scrollHeight - 7) + 'px';
  },
  input: (args) => {
    // Adjust height on every input change
    args.event.currentTarget.style.height = 'auto';
    args.event.currentTarget.style.height = (args.event.currentTarget.scrollHeight) + 'px';
  }
});

autoResizeBox.appendTo('#autoResize');
```

**How it works:**
1. **`created` event**: Calculates initial height based on content
2. **`input` event**: Recalculates height on each keystroke
3. **`scrollHeight`**: TextBox content height determines new height

### Complete Auto-Resize Example

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

class AutoResizeTextBox {
  constructor(elementId: string) {
    this.textbox = new TextBox({
      placeholder: 'Type to expand...',
      multiline: true,
      floatLabelType: 'Auto',
      created: () => this.initializeHeight(),
      input: (e) => this.adjustHeight(e)
    });
    this.textbox.appendTo(elementId);
  }

  private initializeHeight() {
    this.textbox.addAttributes({ rows: '1' });
    this.setHeight();
  }

  private adjustHeight(event: any) {
    this.setHeight(event.currentTarget);
  }

  private setHeight(element?: HTMLElement) {
    const target = element || this.textbox.element;
    target.style.height = 'auto';
    target.style.height = (target.scrollHeight) + 'px';
  }
}

// Usage
const autoResizeTextBox = new AutoResizeTextBox('#autoResize');
```

---

## Text Length Limiting

### Set Maximum Text Length

Limit the number of characters users can enter:

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';
import { Button } from '@syncfusion/ej2-buttons';

const multilineBox = new TextBox({
  placeholder: 'Enter your comment (max 200 characters)',
  multiline: true,
  floatLabelType: 'Auto',
  created: () => {
    multilineBox.addAttributes({ maxLength: '200' });
  }
});

multilineBox.appendTo('#comment');
```

### Character Counter

Display remaining character count:

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const maxLength = 200;
const multilineBox = new TextBox({
  placeholder: 'Enter your feedback',
  multiline: true,
  floatLabelType: 'Auto',
  created: () => {
    multilineBox.addAttributes({ maxLength: maxLength.toString() });
    updateCounter();
  },
  input: () => {
    updateCounter();
  }
});

multilineBox.appendTo('#feedback');

// Update character count
function updateCounter() {
  const remaining = maxLength - multilineBox.value.length;
  const counterEl = document.querySelector('#counter');
  if (counterEl) {
    counterEl.textContent = `${remaining} characters remaining`;
    
    // Visual feedback
    if (remaining < 20) {
      counterEl.className = 'warning';
    } else if (remaining < 50) {
      counterEl.className = 'caution';
    } else {
      counterEl.className = 'normal';
    }
  }
}
```

**HTML:**
```html
<div>
  <textarea id="feedback"></textarea>
  <p id="counter">200 characters remaining</p>
</div>
```

**CSS:**
```css
#counter {
  font-size: 12px;
  margin-top: 5px;
}

#counter.normal {
  color: #999;
}

#counter.caution {
  color: #FF9800;
}

#counter.warning {
  color: #F44336;
}
```

### Enforce Maximum Length

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const multilineBox = new TextBox({
  placeholder: 'Enter your bio (max 150 chars)',
  multiline: true,
  created: () => {
    multilineBox.addAttributes({ maxLength: '150' });
  }
});

multilineBox.appendTo('#bio');

// Prevent exceeding limit
multilineBox.element.addEventListener('input', () => {
  if (multilineBox.value.length > 150) {
    multilineBox.value = multilineBox.value.substring(0, 150);
    multilineBox.dataBind();
  }
});
```

---

## Disable Resizing

### Remove Resize Capability

By default, multiline TextBox allows vertical resizing. Disable with CSS:

```css
/* Disable resize for all textareas */
textarea.e-input,
.e-float-input textarea,
.e-float-input.e-control-wrapper textarea,
.e-input-group textarea,
.e-input-group.e-control-wrapper textarea {
  resize: none;
}
```

### Fixed-Size Multiline TextBox

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

const fixedBox = new TextBox({
  placeholder: 'Enter your address',
  multiline: true,
  floatLabelType: 'Auto',
  created: () => {
    fixedBox.addAttributes({ rows: '4' });
    fixedBox.element.style.resize = 'none';
  }
});

fixedBox.appendTo('#address');
```

### Restrict Horizontal Resize Only

```css
textarea.e-input {
  resize: vertical;  /* Allow only vertical resize */
}
```

---

## Complete Example: Multi-Feature Multiline TextBox

```typescript
import { TextBox } from '@syncfusion/ej2-inputs';

class CommentBox {
  private textbox: TextBox;
  private maxChars = 500;

  constructor() {
    this.textbox = new TextBox({
      placeholder: 'Share your thoughts (max 500 characters)',
      multiline: true,
      floatLabelType: 'Auto',
      created: () => this.onCreated(),
      input: () => this.onInput(),
      change: () => this.onChange()
    });
    this.textbox.appendTo('#commentBox');
  }

  private onCreated() {
    // Set rows and max length
    this.textbox.addAttributes({ 
      rows: '5',
      maxLength: this.maxChars.toString()
    });

    // Disable resize
    this.textbox.element.style.resize = 'none';

    // Initialize counter
    this.updateCounter();
  }

  private onInput() {
    // Auto-expand on long text
    const scrollHeight = this.textbox.element.scrollHeight;
    if (scrollHeight > 200) {
      this.textbox.element.style.height = scrollHeight + 'px';
    }

    // Update counter
    this.updateCounter();
  }

  private onChange() {
    console.log('Comment:', this.textbox.value);
  }

  private updateCounter() {
    const remaining = this.maxChars - this.textbox.value.length;
    const counterEl = document.querySelector('#counter') as HTMLElement;
    
    if (counterEl) {
      counterEl.textContent = `${remaining}/${this.maxChars}`;
      
      if (remaining <= 50) {
        counterEl.className = 'counter warning';
      } else if (remaining <= 150) {
        counterEl.className = 'counter caution';
      } else {
        counterEl.className = 'counter normal';
      }
    }
  }

  public getValue(): string {
    return this.textbox.value;
  }

  public setValue(value: string) {
    this.textbox.value = value;
    this.textbox.dataBind();
    this.updateCounter();
  }

  public clear() {
    this.setValue('');
  }
}

// Usage
const commentBox = new CommentBox();
```

## Related Topics

- [Getting Started](./getting-started.md) - Basic implementation
- [Input States](./input-states.md) - Validation states
- [Floating Labels](./floating-labels.md) - Label customization
- [Styling and Sizing](./styling-sizing.md) - CSS customization
