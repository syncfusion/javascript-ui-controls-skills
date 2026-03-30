# States and Behavior

## Table of Contents
- [Checked and Unchecked States](#checked-and-unchecked-states)
- [Disabled State](#disabled-state)
- [Toggle Method](#toggle-method)
- [Change Event](#change-event)
- [BeforeChange Event](#beforechange-event)
- [RTL Support](#rtl-support)
- [Event Arguments](#event-arguments)

## Checked and Unchecked States

The Switch component has two states represented by the `checked` property:

- **checked: true** - Switch is in ON state (active)
- **checked: false** - Switch is in OFF state (inactive)

### Setting Initial State

```typescript
import { Switch } from '@syncfusion/ej2-buttons';

// Create a switch that starts ON
const onSwitch = new Switch({ checked: true });
onSwitch.appendTo('#switch-on');

// Create a switch that starts OFF
const offSwitch = new Switch({ checked: false });
offSwitch.appendTo('#switch-off');
```

### Reading Current State

You can check the current state of a switch programmatically:

```typescript
const switchObj = new Switch({ checked: true });
switchObj.appendTo('#element');

// Read the current state
console.log(switchObj.checked);  // Output: true
```

### Changing State Programmatically

Set the checked property to change state:

```typescript
const switchObj = new Switch({ checked: false });
switchObj.appendTo('#element');

// Later, turn it on
switchObj.checked = true;
```

## Disabled State

The `disabled` property disables the switch, preventing user interaction:

```typescript
import { Switch } from '@syncfusion/ej2-buttons';

// Create a disabled switch
const switchObj = new Switch({ 
  disabled: true,
  checked: true  // Can be checked but not toggleable
});
switchObj.appendTo('#element');
```

### Disabled Switch Characteristics

- **Cannot be clicked** - User cannot toggle
- **Cannot be keyboard activated** - Space key doesn't work
- **Appears dimmed** - Visual indication of disabled state
- **Not submitted in forms** - Disabled switches don't send values

### Disabling Dynamically

Enable/disable a switch after creation:

```typescript
const switchObj = new Switch({ checked: true });
switchObj.appendTo('#element');

// Disable the switch later
switchObj.disabled = true;

// Re-enable it
switchObj.disabled = false;
```

### Practical Example: Dependent Switches

```typescript
import { Switch } from '@syncfusion/ej2-buttons';

// Main feature toggle
const featureSwitch = new Switch({ 
  checked: false,
  change: onFeatureChange
});
featureSwitch.appendTo('#feature-toggle');

// Settings switch (initially disabled)
const settingsSwitch = new Switch({ 
  disabled: true,
  checked: false
});
settingsSwitch.appendTo('#settings-toggle');

function onFeatureChange(args: any): void {
  // Enable settings only when feature is enabled
  settingsSwitch.disabled = !args.checked;
}
```

In this example, the settings switch is only usable when the main feature is turned on.

## Toggle Method

The `toggle()` method programmatically toggles the switch between ON and OFF states:

```typescript
import { Switch } from '@syncfusion/ej2-buttons';

const switchObj = new Switch({ checked: true });
switchObj.appendTo('#element');

// Toggle the state
switchObj.toggle();  // Now OFF (checked: false)

switchObj.toggle();  // Now ON (checked: true)
```

### Key Points About toggle()

- **Reverses state** - ON becomes OFF, OFF becomes ON
- **Triggers change event** - The change event fires when toggling
- **Works on disabled switches** - Toggle works even if disabled (programmatically)
- **No parameters** - Simply call `toggle()` with no arguments

### Practical Example: Timer-Based Toggle

```typescript
const switchObj = new Switch({ checked: false });
switchObj.appendTo('#element');

// Toggle the switch every 2 seconds
setInterval(() => {
  switchObj.toggle();
}, 2000);
```

## Change Event

The `change` event fires when the switch state changes (either by user click or programmatic toggle):

```typescript
import { Switch } from '@syncfusion/ej2-buttons';

const switchObj = new Switch({ 
  checked: false,
  change: onChangeHandler
});
switchObj.appendTo('#element');

function onChangeHandler(args: any): void {
  console.log('Switch is now:', args.checked ? 'ON' : 'OFF');
  console.log('Previous state:', args.previousValue);
}
```

### Change Event Argument Properties

| Property | Type | Description |
|----------|------|-------------|
| `checked` | boolean | Current state after change |
| `previousValue` | boolean | Previous state before change |
| `event` | Event | Native DOM event |

### Practical Example: Auto-Save on Change

```typescript
const switchObj = new Switch({ 
  checked: false,
  change: autoSavePreference
});
switchObj.appendTo('#notifications');

async function autoSavePreference(args: any): Promise<void> {
  try {
    const response = await fetch('/api/preferences', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ 
        notificationsEnabled: args.checked 
      })
    });
    
    if (response.ok) {
      console.log('Preference saved');
    } else {
      // Revert on error
      switchObj.checked = args.previousValue;
    }
  } catch (error) {
    console.error('Save failed:', error);
    switchObj.checked = args.previousValue;
  }
}
```

## BeforeChange Event

The `beforeChange` event fires **before** the state changes, allowing you to prevent the change:

```typescript
import { Switch, BeforeChangeEventArgs } from '@syncfusion/ej2-buttons';

const switchObj = new Switch({ 
  checked: true,
  beforeChange: onBeforeChange
});
switchObj.appendTo('#element');

function onBeforeChange(args: BeforeChangeEventArgs): void {
  // Prevent the change
  args.cancel = true;
}
```

### Preventing State Changes

Set `args.cancel = true` to prevent the toggle:

```typescript
function onBeforeChange(args: BeforeChangeEventArgs): void {
  if (userHasUnsavedChanges) {
    args.cancel = true;
    alert('Please save your changes first');
  }
}
```

### Practical Example: Confirmation Before Disabling

```typescript
const switchObj = new Switch({ 
  checked: true,
  beforeChange: confirmBeforeDisable
});
switchObj.appendTo('#critical-feature');

function confirmBeforeDisable(args: BeforeChangeEventArgs): void {
  // Only prevent if turning OFF
  if (!args.newValue) {
    const confirmed = confirm(
      'This is a critical feature. Are you sure you want to disable it?'
    );
    if (!confirmed) {
      args.cancel = true;
    }
  }
}
```

### BeforeChange Event Argument Properties

| Property | Type | Description |
|----------|------|-------------|
| `cancel` | boolean | Set to true to prevent the change |
| `newValue` | boolean | The proposed new state |
| `previousValue` | boolean | The current state |

## RTL Support

Right-to-left (RTL) language support is enabled with the `enableRtl` property:

```typescript
import { Switch } from '@syncfusion/ej2-buttons';

const switchObj = new Switch({ 
  checked: true,
  enableRtl: true  // Enable right-to-left layout
});
switchObj.appendTo('#element');
```

### Global RTL Setup

To enable RTL globally for all components:

```typescript
import { Switch } from '@syncfusion/ej2-buttons';
import { enableRtl } from '@syncfusion/ej2-base';

enableRtl(true);  // Enable RTL for all Syncfusion components

const switchObj = new Switch({ checked: true });
switchObj.appendTo('#element');
```

### RTL Effects on Switch

When RTL is enabled:
- Switch handle moves from right to left
- Text labels reverse direction
- Touch interactions work in RTL context
- ARIA labels adjust for RTL

### Practical Example: Language Toggle

```typescript
import { Switch } from '@syncfusion/ej2-buttons';
import { enableRtl } from '@syncfusion/ej2-base';

const languageSwitch = new Switch({ 
  checked: false,  // false = English (LTR), true = Arabic (RTL)
  change: onLanguageChange
});
languageSwitch.appendTo('#language-toggle');

function onLanguageChange(args: any): void {
  const isRTL = args.checked;
  enableRtl(isRTL);
  
  // Update document direction
  document.documentElement.lang = isRTL ? 'ar' : 'en';
  document.documentElement.dir = isRTL ? 'rtl' : 'ltr';
}
```

## Event Arguments

### Complete Reference of Event Arguments

**Change Event Arguments:**
```typescript
interface ChangeEventArgs {
  checked: boolean;        // Current state
  previousValue: boolean;  // Previous state
  event: Event;           // Native DOM event
}
```

**BeforeChange Event Arguments:**
```typescript
interface BeforeChangeEventArgs {
  cancel: boolean;        // Set to true to prevent change
  newValue: boolean;      // Proposed new state
  previousValue: boolean; // Current state
}
```

### Accessing Event Arguments

```typescript
const switchObj = new Switch({ 
  checked: false,
  change: handleChange,
  beforeChange: handleBeforeChange
});
switchObj.appendTo('#element');

function handleChange(args: any): void {
  console.log('Change details:');
  console.log('  New state:', args.checked);
  console.log('  Previous state:', args.previousValue);
  console.log('  Native event:', args.event);
}

function handleBeforeChange(args: any): void {
  console.log('Before change details:');
  console.log('  Proposed state:', args.newValue);
  console.log('  Current state:', args.previousValue);
}
```

## Complete Behavior Example

Here's a comprehensive example combining multiple behaviors:

```typescript
import { Switch, BeforeChangeEventArgs } from '@syncfusion/ej2-buttons';

const switchObj = new Switch({ 
  checked: false,
  disabled: false,
  change: logStateChange,
  beforeChange: validateChange
});
switchObj.appendTo('#behavioral-switch');

function validateChange(args: BeforeChangeEventArgs): void {
  console.log('Validating change from', args.previousValue, 'to', args.newValue);
  
  // Prevent changes under certain conditions
  if (args.newValue && !isTimeValid()) {
    args.cancel = true;
    console.log('Change prevented - invalid time');
  }
}

function logStateChange(args: any): void {
  console.log('State changed successfully');
  console.log('Current:', args.checked, 'Previous:', args.previousValue);
}

function isTimeValid(): boolean {
  const hour = new Date().getHours();
  return hour >= 9 && hour < 17;  // Business hours
}

// Programmatic toggle
setTimeout(() => {
  switchObj.toggle();
}, 3000);

// Later: disable the switch
setTimeout(() => {
  switchObj.disabled = true;
}, 6000);
```

## Next Steps

Explore related features:
- [Customization](customization.md) - Style your switches
- [Form Integration](form-integration.md) - Use switches in forms
- [Advanced Features](advanced-features.md) - Ripple effects and more
