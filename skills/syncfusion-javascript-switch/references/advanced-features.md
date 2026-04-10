# Advanced Features

## Table of Contents
- [Ripple Effects on Labels](#ripple-effects-on-labels)
- [Advanced Event Handling](#advanced-event-handling)
- [Method Reference](#method-reference)
- [Event Argument Types](#event-argument-types)
- [Dynamic Switch Creation](#dynamic-switch-creation)
- [Integration with Form Validation](#integration-with-form-validation)

## Ripple Effects on Labels

By default, the Switch component shows a ripple effect on the switch handle when clicked. However, you can extend ripple effects to associated labels.

### Basic Ripple Setup

```typescript
import { Switch, rippleMouseHandler } from '@syncfusion/ej2-buttons';
import { enableRipple } from '@syncfusion/ej2-base';

// Enable ripple globally
enableRipple(true);

const switchObj = new Switch({ checked: true });
switchObj.appendTo('#element');
```

### Adding Ripple to Labels

To enable ripple effects when clicking the label:

```typescript
import { Switch, rippleMouseHandler } from '@syncfusion/ej2-buttons';
import { enableRipple } from '@syncfusion/ej2-base';

enableRipple(true);

// Initialize switch
const switchObj = new Switch({ checked: true });
switchObj.appendTo('#switch1');

// Get the label element
const label = document.querySelector('label[for="switch1"]');

// Add ripple handler to label
if (label) {
  label.addEventListener('mouseup', rippleHandler);
  label.addEventListener('mousedown', rippleHandler);
  
  function rippleHandler(e: MouseEvent): void {
    // Get ripple container from next element
    const rippleSpan = label?.parentElement
      ?.nextElementSibling
      ?.querySelector('.e-ripple-container');
    
    if (rippleSpan) {
      rippleMouseHandler(e, rippleSpan);
    }
  }
}
```

### Complete Example with HTML

```html
<!DOCTYPE html>
<html>
<head>
  <link href="https://cdn.syncfusion.com/ej2/ej2-base/styles/material.css" rel="stylesheet" />
  <link href="https://cdn.syncfusion.com/ej2/ej2-buttons/styles/material.css" rel="stylesheet" />
</head>
<body>
  <div class="lSize">
    <label for="switch1">Dark Mode</label>
    <input id="switch1" type="checkbox" />
  </div>
  
  <script src="app.ts"></script>
</body>
</html>
```

```typescript
import { Switch, rippleMouseHandler } from '@syncfusion/ej2-buttons';
import { enableRipple } from '@syncfusion/ej2-base';

enableRipple(true);

const switchObj = new Switch({ checked: true });
switchObj.appendTo('#switch1');

// Setup ripple on label
const label = document.querySelector('.lSize label');
if (label) {
  label.addEventListener('mouseup', rippleHandler);
  label.addEventListener('mousedown', rippleHandler);
}

function rippleHandler(e: MouseEvent): void {
  const rippleSpan = document.querySelector(
    '.lSize .e-ripple-container'
  );
  if (rippleSpan) {
    rippleMouseHandler(e, rippleSpan);
  }
}
```

## Advanced Event Handling

### Multiple Event Listeners

```typescript
import { Switch, BeforeChangeEventArgs, ChangeEventArgs } from '@syncfusion/ej2-buttons';

const switchObj = new Switch({
  checked: false,
  beforeChange: onBeforeChange,
  change: onChange
});
switchObj.appendTo('#element');

// Track state changes
let changeLog: any[] = [];

function onBeforeChange(args: BeforeChangeEventArgs): void {
  console.log(`[BEFORE] Attempting to change from ${args.previousValue} to ${args.newValue}`);
  
  // Validate change
  if (!isValidStateTransition(args.previousValue, args.newValue)) {
    args.cancel = true;
    console.log('[BLOCKED] Invalid state transition');
  }
}

function onChange(args: any): void {
  console.log(`[AFTER] Changed from ${args.previousValue} to ${args.checked}`);
  
  changeLog.push({
    timestamp: new Date(),
    from: args.previousValue,
    to: args.checked
  });
}

function isValidStateTransition(from: boolean, to: boolean): boolean {
  // Example: prevent rapid toggling
  const now = Date.now();
  const lastChange = changeLog[changeLog.length - 1];
  
  if (lastChange && (now - lastChange.timestamp.getTime()) < 100) {
    return false;  // Too fast
  }
  
  return true;
}
```

### Chained Event Handlers

```typescript
import { Switch } from '@syncfusion/ej2-buttons';

const switchObj = new Switch({
  checked: false,
  change: function(args: any) {
    console.log('Event 1: State changed');
    updateUI(args.checked);
    
    // Chain next action
    saveToServer(args.checked).then(() => {
      console.log('Event 2: Saved to server');
      notifyUser(args.checked);
    });
  }
});
switchObj.appendTo('#element');

function updateUI(state: boolean): void {
  console.log('UI updated:', state);
}

async function saveToServer(state: boolean): Promise<void> {
  const response = await fetch('/api/state', {
    method: 'POST',
    body: JSON.stringify({ state })
  });
  return response.json();
}

function notifyUser(state: boolean): void {
  console.log('User notified:', state ? 'ON' : 'OFF');
}
```

### Conditional Event Handling

```typescript
import { Switch, BeforeChangeEventArgs } from '@syncfusion/ej2-buttons';

class ConditionalSwitch {
  private switchObj: Switch;
  private requiresConfirmation: boolean = false;
  
  constructor(elementId: string) {
    this.switchObj = new Switch({
      checked: false,
      beforeChange: this.handleBeforeChange.bind(this)
    });
    this.switchObj.appendTo(`#${elementId}`);
  }
  
  private handleBeforeChange(args: BeforeChangeEventArgs): void {
    // Require confirmation only when turning OFF
    if (!args.newValue && this.requiresConfirmation) {
      const confirmed = confirm('Are you sure?');
      args.cancel = !confirmed;
    }
  }
  
  setRequiresConfirmation(requires: boolean): void {
    this.requiresConfirmation = requires;
  }
}

// Usage
const criticalSwitch = new ConditionalSwitch('critical-feature');
criticalSwitch.setRequiresConfirmation(true);
```

## Method Reference

### toggle() Method

```typescript
import { Switch } from '@syncfusion/ej2-buttons';

const switchObj = new Switch({ checked: true });
switchObj.appendTo('#element');

// Toggle the state
switchObj.toggle();  // Now checked: false
console.log(switchObj.checked);  // false

switchObj.toggle();  // Now checked: true
console.log(switchObj.checked);  // true
```

### Methods Available

| Method | Parameters | Returns | Description |
|--------|-----------|---------|-------------|
| `toggle()` | none | void | Toggle between ON and OFF |
| `appendTo(selector)` | string | void | Render to DOM element |
| `destroy()` | none | void | Remove component |

### Destroy Method

```typescript
const switchObj = new Switch({ checked: true });
switchObj.appendTo('#element');

// Later, remove the component
switchObj.destroy();
```

## Event Argument Types

### BeforeChangeEventArgs Interface

```typescript
interface BeforeChangeEventArgs {
  cancel: boolean;        // Set to true to prevent change
  newValue: boolean;      // The new state being set
  previousValue: boolean; // Current state before change
}
```

### ChangeEventArgs Interface

```typescript
interface ChangeEventArgs {
  checked: boolean;       // New state after change
  event?: Event;         // Native DOM event
  previousValue: boolean; // Previous state
}
```

### Using Event Arguments Correctly

```typescript
import { Switch, BeforeChangeEventArgs } from '@syncfusion/ej2-buttons';

const switchObj = new Switch({
  checked: false,
  beforeChange: function(args: BeforeChangeEventArgs) {
    // Correct usage of BeforeChangeEventArgs
    console.log(`Attempting: ${args.previousValue} -> ${args.newValue}`);
    
    if (shouldBlock(args.newValue)) {
      args.cancel = true;
    }
  },
  change: function(args: any) {
    // Use args.checked for current state
    console.log(`Changed to: ${args.checked}`);
    console.log(`Was: ${args.previousValue}`);
  }
});
switchObj.appendTo('#element');

function shouldBlock(newState: boolean): boolean {
  return newState === true && !isConditionMet();
}

function isConditionMet(): boolean {
  return true;
}
```

## Dynamic Switch Creation

### Creating Switches Dynamically

```typescript
import { Switch } from '@syncfusion/ej2-buttons';

function createSwitch(id: string, label: string, checked: boolean = false): void {
  // Create container
  const container = document.createElement('div');
  container.className = 'switch-item';
  
  // Create label
  const labelElement = document.createElement('label');
  labelElement.setAttribute('for', id);
  labelElement.textContent = label;
  
  // Create input
  const input = document.createElement('input');
  input.type = 'checkbox';
  input.id = id;
  
  // Initialize switch
  container.appendChild(labelElement);
  container.appendChild(input);
  document.body.appendChild(container);
  
  const switchObj = new Switch({ checked });
  switchObj.appendTo(`#${id}`);
  
  return switchObj;
}

// Create multiple switches
createSwitch('feature1', 'Feature 1', true);
createSwitch('feature2', 'Feature 2', false);
createSwitch('feature3', 'Feature 3', true);
```

### Dynamic Lists

```typescript
import { Switch } from '@syncfusion/ej2-buttons';

class DynamicSwitchList {
  private container: HTMLElement;
  private switches: Map<string, Switch> = new Map();
  
  constructor(containerId: string) {
    this.container = document.getElementById(containerId) || document.body;
  }
  
  addSwitch(id: string, label: string, value: any = null): Switch {
    // Create elements
    const wrapper = document.createElement('div');
    wrapper.className = 'switch-row';
    
    const labelEl = document.createElement('label');
    labelEl.setAttribute('for', id);
    labelEl.textContent = label;
    
    const input = document.createElement('input');
    input.type = 'checkbox';
    input.id = id;
    
    wrapper.appendChild(labelEl);
    wrapper.appendChild(input);
    this.container.appendChild(wrapper);
    
    // Create switch
    const switchObj = new Switch({ 
      checked: false,
      change: (args: any) => this.onSwitchChange(id, args)
    });
    switchObj.appendTo(`#${id}`);
    
    this.switches.set(id, switchObj);
    return switchObj;
  }
  
  removeSwitch(id: string): void {
    const switchObj = this.switches.get(id);
    if (switchObj) {
      switchObj.destroy();
      this.switches.delete(id);
    }
  }
  
  private onSwitchChange(id: string, args: any): void {
    console.log(`Switch ${id} changed to:`, args.checked);
  }
  
  getAll(): Map<string, Switch> {
    return this.switches;
  }
  
  getState(): { [key: string]: boolean } {
    const state: { [key: string]: boolean } = {};
    this.switches.forEach((switchObj, id) => {
      state[id] = switchObj.checked;
    });
    return state;
  }
}

// Usage
const list = new DynamicSwitchList('switches-container');
list.addSwitch('switch1', 'Enable Analytics');
list.addSwitch('switch2', 'Enable Notifications');
list.addSwitch('switch3', 'Enable Tracking');

// Get all states
console.log(list.getState());
```

## Integration with Form Validation

### Real-Time Validation

```typescript
import { Switch, BeforeChangeEventArgs } from '@syncfusion/ej2-buttons';

class ValidatedSwitchForm {
  private form: HTMLFormElement;
  private switches: Map<string, Switch> = new Map();
  private validationRules: Map<string, (value: boolean) => boolean> = new Map();
  
  constructor(formId: string) {
    this.form = document.getElementById(formId) as HTMLFormElement;
    this.initializeSwitches();
    this.setupFormValidation();
  }
  
  private initializeSwitches(): void {
    const inputs = this.form.querySelectorAll('input[type="checkbox"]');
    
    inputs.forEach((input: Element) => {
      const id = (input as HTMLInputElement).id;
      const switchObj = new Switch({
        checked: (input as HTMLInputElement).checked,
        beforeChange: (args: BeforeChangeEventArgs) => {
          this.validateChange(id, args);
        }
      });
      switchObj.appendTo(input as HTMLInputElement);
      this.switches.set(id, switchObj);
    });
  }
  
  private validateChange(switchId: string, args: BeforeChangeEventArgs): void {
    const rule = this.validationRules.get(switchId);
    
    if (rule) {
      const isValid = rule(args.newValue);
      if (!isValid) {
        args.cancel = true;
        this.showError(switchId, 'Invalid state change');
      }
    }
  }
  
  addValidationRule(switchId: string, rule: (value: boolean) => boolean): void {
    this.validationRules.set(switchId, rule);
  }
  
  private setupFormValidation(): void {
    this.form.addEventListener('submit', (e: Event) => {
      if (!this.isFormValid()) {
        e.preventDefault();
        this.showFormError();
      }
    });
  }
  
  private isFormValid(): boolean {
    // Check all switches meet their validation rules
    for (const [id, switchObj] of this.switches) {
      const rule = this.validationRules.get(id);
      if (rule && !rule(switchObj.checked)) {
        return false;
      }
    }
    return true;
  }
  
  private showError(switchId: string, message: string): void {
    const element = document.getElementById(switchId);
    console.error(`${switchId}: ${message}`);
    element?.classList.add('error');
  }
  
  private showFormError(): void {
    alert('Form validation failed');
  }
}

// Usage
const form = new ValidatedSwitchForm('settings-form');

// Add validation rules
form.addValidationRule('terms', (value) => value === true);  // Must be checked
form.addValidationRule('notifications', (value) => true);    // Always valid
form.addValidationRule('critical', (value) => {
  // Can only be ON during business hours
  const hour = new Date().getHours();
  return value ? (hour >= 9 && hour < 17) : true;
});
```

### Dependent Switch Validation

```typescript
import { Switch } from '@syncfusion/ej2-buttons';

class DependentSwitchGroup {
  private switches: Map<string, Switch> = new Map();
  private dependencies: Map<string, string[]> = new Map();
  
  createDependentSwitch(
    id: string,
    label: string,
    dependsOn?: string
  ): Switch {
    const input = document.createElement('input');
    input.type = 'checkbox';
    input.id = id;
    document.body.appendChild(input);
    
    const switchObj = new Switch({
      checked: false,
      beforeChange: (args: any) => {
        this.validateDependencies(id, args);
      }
    });
    switchObj.appendTo(`#${id}`);
    
    this.switches.set(id, switchObj);
    
    if (dependsOn) {
      if (!this.dependencies.has(dependsOn)) {
        this.dependencies.set(dependsOn, []);
      }
      this.dependencies.get(dependsOn)!.push(id);
      
      // Disable initially if dependency is off
      const parentSwitch = this.switches.get(dependsOn);
      if (parentSwitch && !parentSwitch.checked) {
        switchObj.disabled = true;
      }
    }
    
    return switchObj;
  }
  
  private validateDependencies(switchId: string, args: any): void {
    const dependents = this.dependencies.get(switchId);
    
    if (dependents) {
      dependents.forEach(dependentId => {
        const dependentSwitch = this.switches.get(dependentId);
        if (dependentSwitch) {
          dependentSwitch.disabled = !args.checked;
          
          // Reset if disabling
          if (!args.checked) {
            dependentSwitch.checked = false;
          }
        }
      });
    }
  }
}

// Usage
const group = new DependentSwitchGroup();
group.createDependentSwitch('main-feature', 'Main Feature');
group.createDependentSwitch('sub-feature', 'Sub Feature', 'main-feature');
group.createDependentSwitch('advanced', 'Advanced Options', 'sub-feature');
```

## Next Steps

- [Form Integration](form-integration.md) - Submit switches in forms
- [Accessibility](accessibility.md) - Advanced accessibility patterns
- [Customization](customization.md) - Advanced styling techniques
