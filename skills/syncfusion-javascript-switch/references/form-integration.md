# Form Integration

## Table of Contents
- [Form Integration Basics](#form-integration-basics)
- [Name and Value Attributes](#name-and-value-attributes)
- [Form Submission](#form-submission)
- [Grouped Switches](#grouped-switches)
- [Disabled vs Unchecked Behavior](#disabled-vs-unchecked-behavior)
- [Multiple Switches Example](#multiple-switches-example)

## Form Integration Basics

The Switch component can be integrated into HTML forms to submit data when the form is submitted.

### Basic Concept

When a form containing switches is submitted:
- **Checked switches** send their name and value to the server
- **Unchecked switches** don't send any data
- **Disabled switches** don't send any data (even if checked)

```html
<form id="settings-form" method="POST" action="/save-settings">
  <!-- Switch inside form -->
  <input id="dark-mode" type="checkbox" name="darkMode" value="enabled" />
  
  <button type="submit">Save Settings</button>
</form>
```

```typescript
import { Switch } from '@syncfusion/ej2-buttons';

const switchObj = new Switch({ 
  checked: true,
  name: 'darkMode',
  value: 'enabled'
});
switchObj.appendTo('#dark-mode');
```

When submitted, sends:
```
darkMode=enabled
```

## Name and Value Attributes

The `name` and `value` attributes control how switches are submitted in forms.

### name Attribute

The `name` attribute groups related form fields:

```typescript
const switchObj = new Switch({
  name: 'connectivity',  // Name for form submission
  value: 'wifi',
  checked: true
});
switchObj.appendTo('#wifi-switch');
```

### value Attribute

The `value` attribute is what gets sent when the switch is checked:

```typescript
const switchObj = new Switch({
  name: 'connectivity',
  value: 'wifi',    // This value is sent when checked
  checked: false
});
switchObj.appendTo('#wifi-switch');
```

### Complete Form Example

```html
<form id="tethering-form" method="POST" action="/api/settings">
  <fieldset>
    <legend>Tethering Options</legend>
    
    <label>
      <input id="usb-tether" type="checkbox" name="tethering" value="usb" />
      USB
    </label>
    
    <label>
      <input id="wifi-tether" type="checkbox" name="tethering" value="wifi" />
      Wi-Fi
    </label>
    
    <label>
      <input id="bt-tether" type="checkbox" name="tethering" value="bluetooth" disabled />
      Bluetooth
    </label>
    
    <button type="submit">Apply Settings</button>
  </fieldset>
</form>
```

```typescript
import { Switch } from '@syncfusion/ej2-buttons';

// USB Tether
let usbSwitch = new Switch({
  name: 'tethering',
  value: 'usb',
  checked: true
});
usbSwitch.appendTo('#usb-tether');

// Wi-Fi Tether
let wifiSwitch = new Switch({
  name: 'tethering',
  value: 'wifi',
  checked: true
});
wifiSwitch.appendTo('#wifi-tether');

// Bluetooth Tether (disabled)
let btSwitch = new Switch({
  name: 'tethering',
  value: 'bluetooth',
  disabled: true
});
btSwitch.appendTo('#bt-tether');
```

When submitted with USB and Wi-Fi checked:
```
tethering=usb&tethering=wifi
```

Note: Bluetooth is not sent because it's disabled.

## Form Submission

### Programmatic Form Submission

```typescript
import { Switch } from '@syncfusion/ej2-buttons';

const switchObj = new Switch({
  name: 'darkMode',
  value: 'on',
  checked: false
});
switchObj.appendTo('#theme-switch');

// Get the form and submit it
const form = document.getElementById('settings-form') as HTMLFormElement;

document.getElementById('submit-btn')?.addEventListener('click', () => {
  if (form) {
    form.submit();
  }
});
```

### Capturing Form Data Before Submission

```typescript
import { Switch } from '@syncfusion/ej2-buttons';

const switchObj = new Switch({
  name: 'notifications',
  value: 'enabled',
  checked: true
});
switchObj.appendTo('#notifications-switch');

const form = document.getElementById('settings-form') as HTMLFormElement;

form?.addEventListener('submit', (e: Event) => {
  e.preventDefault();
  
  // Get form data
  const formData = new FormData(form);
  
  // Log switch value
  console.log('Notifications:', formData.get('notifications'));
  
  // Send via fetch API
  fetch('/api/settings', {
    method: 'POST',
    body: formData
  })
  .then(response => response.json())
  .then(data => console.log('Saved:', data));
});
```

### Getting Switch State Before Form Submission

```typescript
const switchObj = new Switch({
  name: 'acceptTerms',
  value: 'yes'
});
switchObj.appendTo('#terms-switch');

const form = document.getElementById('form') as HTMLFormElement;

form?.addEventListener('submit', (e: Event) => {
  // Check if required switches are enabled
  if (!switchObj.checked) {
    e.preventDefault();
    alert('You must accept the terms');
  }
});
```

## Grouped Switches

Multiple switches with the same `name` attribute form a group. When submitted, all checked values are sent as separate fields.

### Example: Notification Preferences

```html
<form id="notifications-form" method="POST">
  <fieldset>
    <legend>Notification Preferences</legend>
    
    <label>
      <input id="email-notif" type="checkbox" name="notifications" value="email" />
      Email Notifications
    </label>
    
    <label>
      <input id="sms-notif" type="checkbox" name="notifications" value="sms" />
      SMS Notifications
    </label>
    
    <label>
      <input id="push-notif" type="checkbox" name="notifications" value="push" />
      Push Notifications
    </label>
    
    <button type="submit">Save</button>
  </fieldset>
</form>
```

```typescript
import { Switch } from '@syncfusion/ej2-buttons';

// All switches share the same 'name'
new Switch({
  name: 'notifications',
  value: 'email',
  checked: true
}).appendTo('#email-notif');

new Switch({
  name: 'notifications',
  value: 'sms',
  checked: false
}).appendTo('#sms-notif');

new Switch({
  name: 'notifications',
  value: 'push',
  checked: true
}).appendTo('#push-notif');
```

When submitted with Email and Push checked:
```
notifications=email&notifications=push
```

### Server-Side Processing (Node.js/Express Example)

```javascript
// Parse grouped form data
app.post('/save-preferences', (req, res) => {
  const notifications = Array.isArray(req.body.notifications)
    ? req.body.notifications
    : [req.body.notifications];
    
  console.log('User enabled:', notifications);
  // notifications = ['email', 'push']
  
  res.json({ success: true });
});
```

## Disabled vs Unchecked Behavior

Understanding the difference is crucial for form submission.

### Unchecked Switches

```typescript
const switchObj = new Switch({
  name: 'feature',
  value: 'enabled',
  checked: false  // Not checked
});
switchObj.appendTo('#element');
```

Form submission sends: **Nothing** (field is omitted)

### Disabled Switches

```typescript
const switchObj = new Switch({
  name: 'feature',
  value: 'enabled',
  checked: true,   // Checked
  disabled: true   // But disabled
});
switchObj.appendTo('#element');
```

Form submission sends: **Nothing** (disabled fields are never submitted)

### Comparison Table

| State | Checked | Disabled | Submitted | Value |
|-------|---------|----------|-----------|-------|
| Normal ON | ✓ | ✗ | ✓ Yes | `value` |
| Normal OFF | ✗ | ✗ | ✗ No | - |
| Disabled ON | ✓ | ✓ | ✗ No | - |
| Disabled OFF | ✗ | ✓ | ✗ No | - |

### Practical Example: Required Feature

```typescript
import { Switch } from '@syncfusion/ej2-buttons';

const agreeSwitch = new Switch({
  name: 'termsAgreed',
  value: 'yes',
  checked: false
});
agreeSwitch.appendTo('#terms-switch');

const form = document.getElementById('form') as HTMLFormElement;

form?.addEventListener('submit', (e: Event) => {
  // Unchecked = not submitted = must reject
  if (!agreeSwitch.checked) {
    e.preventDefault();
    alert('You must agree to the terms');
  }
});
```

## Multiple Switches Example

Complete real-world example with multiple switch groups:

```html
<form id="comprehensive-form" method="POST" action="/save-all">
  <fieldset>
    <legend>Account Settings</legend>
    
    <!-- Theme preference (single switch) -->
    <div class="setting-group">
      <label for="dark-mode">Dark Mode</label>
      <input id="dark-mode" type="checkbox" name="theme" value="dark" />
    </div>
    
    <!-- Notifications group -->
    <div class="setting-group">
      <legend>Notifications</legend>
      <label>
        <input id="notif-email" type="checkbox" name="notifications" value="email" />
        Email
      </label>
      <label>
        <input id="notif-sms" type="checkbox" name="notifications" value="sms" />
        SMS
      </label>
      <label>
        <input id="notif-push" type="checkbox" name="notifications" value="push" />
        Push
      </label>
    </div>
    
    <!-- Privacy group -->
    <div class="setting-group">
      <legend>Privacy</legend>
      <label>
        <input id="priv-analytics" type="checkbox" name="privacy" value="analytics" />
        Analytics
      </label>
      <label>
        <input id="priv-marketing" type="checkbox" name="privacy" value="marketing" disabled />
        Marketing
      </label>
    </div>
    
    <button type="submit">Save All Settings</button>
  </fieldset>
</form>
```

```typescript
import { Switch } from '@syncfusion/ej2-buttons';

// Theme preference
new Switch({
  name: 'theme',
  value: 'dark',
  checked: false
}).appendTo('#dark-mode');

// Notifications
new Switch({
  name: 'notifications',
  value: 'email',
  checked: true
}).appendTo('#notif-email');

new Switch({
  name: 'notifications',
  value: 'sms',
  checked: false
}).appendTo('#notif-sms');

new Switch({
  name: 'notifications',
  value: 'push',
  checked: true
}).appendTo('#notif-push');

// Privacy
new Switch({
  name: 'privacy',
  value: 'analytics',
  checked: true
}).appendTo('#priv-analytics');

new Switch({
  name: 'privacy',
  value: 'marketing',
  checked: false,
  disabled: true  // Always disabled
}).appendTo('#priv-marketing');

// Handle form submission
const form = document.getElementById('comprehensive-form') as HTMLFormElement;
form?.addEventListener('submit', async (e: Event) => {
  e.preventDefault();
  
  const formData = new FormData(form);
  const settings = Object.fromEntries(formData);
  
  console.log('Submitted:', settings);
  // Output:
  // {
  //   'notifications': ['email', 'push'],
  //   'privacy': 'analytics'
  // }
  
  try {
    const response = await fetch('/save-all', {
      method: 'POST',
      body: formData
    });
    
    if (response.ok) {
      console.log('Settings saved');
    }
  } catch (error) {
    console.error('Save failed:', error);
  }
});
```

### Form Data Processing

When the form above is submitted with the shown state:

**FormData:**
```
theme=                          (empty, not checked)
notifications=email
notifications=push
privacy=analytics
                                (marketing omitted - disabled)
```

**Server receives:**
```javascript
{
  notifications: ['email', 'push'],
  privacy: 'analytics'
}
```

## Form Validation Pattern

```typescript
import { Switch } from '@syncfusion/ej2-buttons';

class FormWithSwitches {
  private switches: { [key: string]: Switch } = {};
  
  constructor(private formId: string) {
    this.initializeSwitches();
    this.setupValidation();
  }
  
  private initializeSwitches(): void {
    const form = document.getElementById(this.formId);
    const switchInputs = form?.querySelectorAll('input[type="checkbox"]');
    
    switchInputs?.forEach((input: Element) => {
      const switchObj = new Switch({
        name: (input as HTMLInputElement).name,
        value: (input as HTMLInputElement).value
      });
      switchObj.appendTo(input as HTMLInputElement);
      
      this.switches[(input as HTMLInputElement).id] = switchObj;
    });
  }
  
  private setupValidation(): void {
    const form = document.getElementById(this.formId) as HTMLFormElement;
    
    form?.addEventListener('submit', (e: Event) => {
      if (!this.isValid()) {
        e.preventDefault();
        this.showErrors();
      }
    });
  }
  
  private isValid(): boolean {
    // Add custom validation logic
    return true;
  }
  
  private showErrors(): void {
    console.log('Form has validation errors');
  }
}

// Usage
new FormWithSwitches('comprehensive-form');
```

## Next Steps

Explore related features:
- [Accessibility](accessibility.md) - Ensure forms are accessible
- [States and Behavior](states-and-behavior.md) - Event handling in forms
- [Advanced Features](advanced-features.md) - Dynamic form fields
