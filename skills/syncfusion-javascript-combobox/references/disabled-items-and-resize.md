# Disabled Items & Resizable Popup

## Table of Contents
- [Disabling Specific Items](#disabling-specific-items)
- [Dynamic Item Disabling](#dynamic-item-disabling)
- [Disable Entire ComboBox](#disable-entire-combobox)
- [Resizable Popup](#resizable-popup)
- [Configure Popup Size](#configure-popup-size)
- [Real-World Examples](#real-world-examples)

## Disabling Specific Items

### Mark Items as Disabled

Use the `disabled` field to mark certain items as non-selectable:

```typescript
import { ComboBox } from '@syncfusion/ej2-dropdowns';

const statusData: { [key: string]: Object }[] = [
    { Status: 'Open', isDisabled: false },
    { Status: 'Waiting for Customer', isDisabled: false },
    { Status: 'On Hold', isDisabled: true },
    { Status: 'Follow-up', isDisabled: false },
    { Status: 'Closed', isDisabled: true },
    { Status: 'Solved', isDisabled: false },
    { Status: 'Feature Request', isDisabled: false }
];

const comboBox: ComboBox = new ComboBox({
    dataSource: statusData,
    fields: { text: 'Status', value: 'Status', disabled: 'isDisabled' },
    placeholder: "Select status"
});

comboBox.appendTo('#comboelement');

// Result:
// Open ✓ clickable
// Waiting for Customer ✓ clickable
// On Hold ✗ disabled (grayed out)
// Follow-up ✓ clickable
// Closed ✗ disabled (grayed out)
// Solved ✓ clickable
// Feature Request ✓ clickable
```

**Disabled items appear grayed out but are still visible in the dropdown.**

---

### Disable Items Based on Condition

Mark items as disabled based on logic:

```typescript
const serviceList: { [key: string]: Object }[] = [
    { 
        name: 'Premium Support',
        price: '$99/month',
        available: true,
        disabled: false
    },
    { 
        name: 'Enterprise Support',
        price: '$299/month',
        available: false,  // Out of stock
        disabled: true
    },
    { 
        name: 'Community Support',
        price: 'Free',
        available: true,
        disabled: false
    },
    { 
        name: 'Beta Support',
        price: '$49/month',
        available: false,  // Still in beta
        disabled: true
    }
];

const comboBox: ComboBox = new ComboBox({
    dataSource: serviceList,
    fields: { 
        text: 'name', 
        value: 'name',
        disabled: 'disabled'  // Use disabled field
    },
    placeholder: "Select support tier"
});

comboBox.appendTo('#comboelement');

// Result: Only available services are selectable
```

---

## Dynamic Item Disabling

### Disable Item Programmatically

Use the `disableItem()` method to disable items after initialization:

```typescript
import { ComboBox } from '@syncfusion/ej2-dropdowns';

const itemList: string[] = ['Item A', 'Item B', 'Item C', 'Item D', 'Item E'];

const comboBox: ComboBox = new ComboBox({
    dataSource: itemList,
    placeholder: "Select item"
});

comboBox.appendTo('#comboelement');

// Disable specific item by index
function disableItemAtIndex(index: number) {
    const listItems = document.querySelectorAll('.e-list-item');
    if (listItems[index]) {
        comboBox.disableItem(listItems[index] as HTMLLIElement);
    }
}

function disableItemByValue(value: string) {
    comboBox.disableItem(value);
}

// Usage:
disableItemAtIndex(2);      // Disable "Item C"
disableItemByValue('Item D'); // Disable "Item D"
```

---

### Disable Multiple Items

Disable several items with a loop:

```typescript
const orderStatusList: { [key: string]: Object }[] = [
    { id: 1, status: 'Pending', canSelect: true },
    { id: 2, status: 'Processing', canSelect: false },
    { id: 3, status: 'Shipped', canSelect: true },
    { id: 4, status: 'Delivered', canSelect: false },
    { id: 5, status: 'Cancelled', canSelect: true }
];

const comboBox: ComboBox = new ComboBox({
    dataSource: orderStatusList,
    fields: { text: 'status', value: 'id', disabled: 'canSelect' },
    placeholder: "Select order status"
});

comboBox.appendTo('#comboelement');

// Later: Disable items based on new condition
function updateDisabledItems() {
    orderStatusList.forEach((item, index) => {
        if (item.canSelect === false) {
            const listItems = document.querySelectorAll('.e-list-item');
            if (listItems[index]) {
                comboBox.disableItem(listItems[index] as HTMLLIElement);
            }
        }
    });
}
```

---

### Disable Item with Index

```typescript
const comboBox: ComboBox = new ComboBox({
    dataSource: ['Option 1', 'Option 2', 'Option 3', 'Option 4'],
    placeholder: "Select..."
});

comboBox.appendTo('#comboelement');

// Get list items and disable by index
const items = document.querySelectorAll('.e-list-item');

// Disable second item (index 1)
comboBox.disableItem(items[1] as HTMLLIElement);

// Disable fourth item (index 3)
comboBox.disableItem(items[3] as HTMLLIElement);
```

---

## Disable Entire ComboBox

### Disable Component

Prevent all interactions with the ComboBox:

```typescript
const comboBox: ComboBox = new ComboBox({
    dataSource: ['Option A', 'Option B', 'Option C'],
    enabled: false,  // Disabled by default
    placeholder: "Disabled"
});

comboBox.appendTo('#comboelement');

// Result: Component appears grayed out, cannot open or select
```

---

### Enable/Disable Based on Condition

```typescript
const comboBox: ComboBox = new ComboBox({
    dataSource: ['Option A', 'Option B', 'Option C'],
    placeholder: "Select option"
});

comboBox.appendTo('#comboelement');

// Toggle enable/disable
function toggleComboBox(shouldDisable: boolean) {
    comboBox.enabled = !shouldDisable;
}

// Disable if user doesn't have permission
function disableIfUnauthorized(hasPermission: boolean) {
    if (!hasPermission) {
        comboBox.enabled = false;
    } else {
        comboBox.enabled = true;
    }
}

// Usage
toggleComboBox(true);              // Disable
toggleComboBox(false);             // Enable
disableIfUnauthorized(false);      // Unauthorized—disable
disableIfUnauthorized(true);       // Authorized—enable
```

---

### Read-Only vs Disabled

| State | Behavior | Use Case |
|-------|----------|----------|
| **Disabled** | Cannot open, edit, or select; grayed out | Unavailable feature |
| **Read-Only** | Can view value; cannot change; appears normal | Display-only mode |

```typescript
// Disabled (grayed out, no interaction)
const comboBox1: ComboBox = new ComboBox({
    dataSource: ['Option A', 'Option B'],
    enabled: false,
    value: 'Option A'
});

// Read-only (normal appearance, shows value, no changes)
const comboBox2: ComboBox = new ComboBox({
    dataSource: ['Option A', 'Option B'],
    readonly: true,
    value: 'Option A'
});
```

---

## Resizable Popup

### Enable Popup Resizing

Allow users to drag and resize the dropdown:

```typescript
import { ComboBox } from '@syncfusion/ej2-dropdowns';

const statusData: { [key: string]: Object }[] = [
    { Status: 'Open', State: false },
    { Status: 'Waiting for Customer', State: false },
    { Status: 'On Hold', State: true },
    { Status: 'Follow-up', State: false },
    { Status: 'Closed', State: true },
    { Status: 'Solved', State: false },
    { Status: 'Feature Request', State: false },
    { Status: 'In Progress', State: false },
    { Status: 'Pending', State: true },
    { Status: 'Escalated', State: true },
    { Status: 'New', State: false },
    { Status: 'Under Review', State: true },
    { Status: 'Reopened', State: false },
    { Status: 'Approved', State: true },
    { Status: 'Awaiting Approval', State: false },
    { Status: 'Scheduled', State: false },
    { Status: 'Canceled', State: true },
    { Status: 'Completed', State: true }
];

const comboBox: ComboBox = new ComboBox({
    dataSource: statusData,
    fields: { text: 'Status', value: 'Status' },
    allowResize: true,  // Enable resize handle
    popupHeight: '200px',
    placeholder: "Select status (resizable popup)"
});

comboBox.appendTo('#comboelement');

// Result: Resize handle appears in bottom-right corner of dropdown
// User can drag to expand/shrink popup
```

---

### Resize Behavior

With `allowResize: true`:
- ✅ Resize handle appears in bottom-right corner
- ✅ Drag to expand width and height
- ✅ Resized size persists during session
- ✅ User can make popup larger for better visibility

---

## Configure Popup Size

### Set Initial Size

```typescript
const comboBox: ComboBox = new ComboBox({
    dataSource: ['Item A', 'Item B', 'Item C'],
    
    popupHeight: '300px',  // Height
    popupWidth: '400px',   // Width
    
    placeholder: "Select item"
});

comboBox.appendTo('#comboelement');

// Custom dimensions for better UX
```

---

### Auto-Size Popup

```typescript
const comboBox: ComboBox = new ComboBox({
    dataSource: largeDataset,  // Many items
    
    popupHeight: 'auto',  // Auto-calculate based on items
    popupWidth: 'auto',   // Auto-calculate based on content
    
    enableVirtualization: true,  // For large datasets
    
    placeholder: "Select..."
});

comboBox.appendTo('#comboelement');

// Popup height/width adjust automatically
```

---

### Combine Resize with Size Config

```typescript
const comboBox: ComboBox = new ComboBox({
    dataSource: employeeData,
    fields: { text: 'name', value: 'id' },
    
    // Set initial size
    popupHeight: '250px',
    popupWidth: '350px',
    
    // Allow user to resize
    allowResize: true,
    
    // Also enable filtering for large lists
    allowFiltering: true,
    
    placeholder: "Select employee (resizable)"
});

comboBox.appendTo('#comboelement');

// User can:
// 1. See 250px height initially
// 2. Filter to narrow results
// 3. Drag to resize if needed
```

---

## Real-World Examples

### Example 1: Permission-Based Item Disabling

```typescript
interface Feature {
    id: number;
    name: string;
    requiredPlan: 'free' | 'pro' | 'enterprise';
}

const features: Feature[] = [
    { id: 1, name: 'Basic Reports', requiredPlan: 'free' },
    { id: 2, name: 'Advanced Reports', requiredPlan: 'pro' },
    { id: 3, name: 'Custom Reports', requiredPlan: 'enterprise' },
    { id: 4, name: 'API Access', requiredPlan: 'pro' },
    { id: 5, name: 'Priority Support', requiredPlan: 'enterprise' }
];

function getAvailableFeatures(userPlan: 'free' | 'pro' | 'enterprise') {
    return features.map(f => ({
        ...f,
        disabled: getPriorityValue(f.requiredPlan) > getPriorityValue(userPlan)
    }));
}

function getPriorityValue(plan: string): number {
    return { free: 1, pro: 2, enterprise: 3 }[plan] || 0;
}

const comboBox: ComboBox = new ComboBox({
    dataSource: getAvailableFeatures('pro'),  // User has pro plan
    fields: { text: 'name', value: 'id', disabled: 'disabled' },
    placeholder: "Select feature"
});

comboBox.appendTo('#comboelement');

// Result: 
// Basic Reports ✓ selectable
// Advanced Reports ✓ selectable
// Custom Reports ✗ disabled (requires enterprise)
// API Access ✓ selectable
// Priority Support ✗ disabled (requires enterprise)
```

---

### Example 2: Inventory Status Disabling

```typescript
interface Product {
    id: number;
    name: string;
    inStock: boolean;
}

const products: Product[] = [
    { id: 1, name: 'Laptop', inStock: true },
    { id: 2, name: 'Monitor', inStock: false },
    { id: 3, name: 'Keyboard', inStock: true },
    { id: 4, name: 'Mouse', inStock: false },
    { id: 5, name: 'Headphones', inStock: true }
];

const comboBox: ComboBox = new ComboBox({
    dataSource: products.map(p => ({
        ...p,
        disabled: !p.inStock
    })),
    fields: { 
        text: 'name', 
        value: 'id', 
        disabled: 'disabled'
    },
    placeholder: "Select product (out of stock items disabled)",
    allowResize: true  // Help user see availability
});

comboBox.appendTo('#comboelement');

// Result:
// Laptop ✓ available
// Monitor ✗ out of stock
// Keyboard ✓ available
// Mouse ✗ out of stock
// Headphones ✓ available
```

---

## Next Steps

- ✅ **Done:** Disabled items and resizable popup
- 📖 **Next:** [How-To Guides](../how-to-guides.md) - Autofill, cascading, icons

---

## See Also

- [API: disabled Field](https://ej2.syncfusion.com/documentation/api/combo-box/#fields)
- [API: disableItem Method](https://ej2.syncfusion.com/documentation/api/combo-box/#disableitem)
- [API: enabled Property](https://ej2.syncfusion.com/documentation/api/combo-box/#enabled)
- [API: allowResize](https://ej2.syncfusion.com/documentation/api/combo-box/#allowresize)
