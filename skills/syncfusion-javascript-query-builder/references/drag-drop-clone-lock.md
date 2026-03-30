# Drag-Drop, Clone, and Lock Features

## Table of Contents
- [Overview](#overview)
- [Drag and Drop](#drag-and-drop)
- [Clone Rules](#clone-rules)
- [Lock Rules](#lock-rules)
- [Separator Configuration](#separator-configuration)
- [Events and Handling](#events-and-handling)
- [Advanced Interactions](#advanced-interactions)

## Overview

QueryBuilder provides rich interaction features:
- **Drag-Drop** - Reorder rules within groups or between groups
- **Clone** - Duplicate rules to create similar conditions
- **Lock** - Prevent modification of critical rules
- **Separators** - Visual connectors between conditions

## Drag and Drop

### Enable Drag-Drop

```typescript
import { QueryBuilder } from '@syncfusion/ej2-querybuilder';

const qb = new QueryBuilder({
  width: '100%',
  dataSource: employeeData,
  columns: columns,
  allowDragDrop: true  // Enable drag-drop
});

qb.appendTo('#querybuilder');
```

### Drag-Drop Use Cases

**Within a Group:**
```typescript
// User can reorder rules:
// Original: (City = Seattle) AND (Title = Sales) AND (Salary > 50000)
// After drag: (Salary > 50000) AND (City = Seattle) AND (Title = Sales)
```

**Between Groups:**
```typescript
// Move rule from one group to another
// Original:
// AND
//  ├── City = Seattle
//  └── OR
//      ├── Title = Sales
//      └── Title = Manager

// After drag (Title = Manager moved to root AND):
// AND
//  ├── City = Seattle
//  ├── Title = Manager
//  └── OR
//      └── Title = Sales
```

### Drag-Drop Configuration

```typescript
interface DragDropOptions {
  allowDragDrop: boolean;     // Enable/disable drag-drop
  displayMode?: string;       // 'Inline' or 'Vertical' affects drag behavior
}

const qb = new QueryBuilder({
  allowDragDrop: true,
  displayMode: 'Inline',      // Horizontal layout - easier dragging
  dataSource: employeeData,
  columns: columns
});

qb.appendTo('#querybuilder');
```

## Clone Rules

### Clone a Rule

```typescript
import { QueryBuilder } from '@syncfusion/ej2-querybuilder';

const qb = new QueryBuilder({
  width: '100%',
  dataSource: employeeData,
  columns: columns,
  showButtons: {
    ruleDelete: true,
    groupDelete: true,
    groupInsert: true,
    ruleInsert: true
  }
});

qb.appendTo('#querybuilder');

// Clone rule programmatically
function cloneRule(ruleId: string) {
  const rules = qb.getRules();
  const ruleToClone = findRuleById(rules, ruleId);
  
  if (ruleToClone && !ruleToClone.rules) {
    // It's a leaf rule (not a group)
    const clonedRule = JSON.parse(JSON.stringify(ruleToClone));
    qb.addRules([clonedRule], 'group0');
  }
}

function findRuleById(rules: RuleModel, targetId: string): Rule | undefined {
  for (const item of rules.rules) {
    if (item.id === targetId) return item;
    if (item.rules) {
      const found = findRuleById(item as RuleModel, targetId);
      if (found) return found;
    }
  }
  return undefined;
}
```

### Clone Group

```typescript
// Clone an entire group (all nested rules)
function cloneGroup(groupId: string) {
  const rules = qb.getRules();
  const groupToClone = findRuleById(rules, groupId);
  
  if (groupToClone && groupToClone.rules) {
    // It's a group
    const clonedGroup = JSON.parse(JSON.stringify(groupToClone));
    qb.addGroups([clonedGroup], 'group0');
  }
}
```

### Use Cases for Clone

**Scenario 1: Similar Filters**
```typescript
// User creates filter for Seattle (City = Seattle AND Salary > 50000)
// Then clones and changes to: City = Tacoma AND Salary > 50000
// Without clone, would need to recreate all conditions
```

**Scenario 2: Template Generation**
```typescript
// Admin creates saved filter template
// Users clone template and customize for their needs
```

**Scenario 3: A/B Testing**
```typescript
// Create filter condition A (clone of baseline)
// Create filter condition B (different variation)
// Compare results
```

## Lock Rules

### Lock a Rule to Prevent Editing

```typescript
import { QueryBuilder, RuleModel } from '@syncfusion/ej2-querybuilder';

// Create a locked rule
const lockedRule: RuleModel = {
  condition: 'and',
  rules: [
    {
      field: 'Status',
      type: 'string',
      operator: 'equal',
      value: 'Active',
      // lock: true  // Not a standard property, use CSS instead
    }
  ]
};

const qb = new QueryBuilder({
  rule: lockedRule,
  dataSource: employeeData,
  columns: columns
});

qb.appendTo('#querybuilder');
```

### Implement Lock via CSS/UI

```typescript
// Add locked state to rules
function lockRule(ruleElement: HTMLElement) {
  ruleElement.classList.add('rule-locked');
  
  // Disable inputs
  const inputs = ruleElement.querySelectorAll('input, select');
  inputs.forEach(input => {
    (input as HTMLInputElement).disabled = true;
  });
}

// CSS for locked rules
const lockStyle = `
.rule-locked {
  opacity: 0.6;
  background-color: #f5f5f5;
  border-left: 4px solid #ff9800;
  padding-left: 10px;
}

.rule-locked input,
.rule-locked select {
  cursor: not-allowed;
  background-color: #efefef;
}
`;
```

### Lock with Visual Indicator

```typescript
// Create lock icon and handler
function createLockedRule(): RuleModel {
  return {
    condition: 'and',
    rules: [
      {
        field: 'Status',
        type: 'string',
        operator: 'equal',
        value: 'Active'
        // Add custom property for lock state
        // locked: true
      }
    ]
  };
}

// Apply lock styling after rendering
qb.addEventListener('renderComplete', () => {
  const lockedRules = document.querySelectorAll('[data-locked="true"]');
  lockedRules.forEach(rule => {
    lockRule(rule as HTMLElement);
  });
});
```

### Use Cases for Locking

**Scenario 1: Mandatory Filters**
```typescript
// System admin locks Status = Active
// Users cannot delete or modify this filter
// Ensures data compliance
```

**Scenario 2: Template Protection**
```typescript
// Template includes locked base filters
// Users add additional custom filters on top
// Prevents accidental removal of template
```

**Scenario 3: Audit Requirements**
```typescript
// Compliance rule locked: Department = Sales
// Users filter within Sales department only
// Cannot view other departments' data
```

## Separator Configuration

### Separator and Connector Visuals

```typescript
// Configure display mode and connectors
const qb = new QueryBuilder({
  width: '100%',
  dataSource: employeeData,
  columns: columns,
  displayMode: 'Inline',      // 'Inline' or 'Vertical'
  separateConnector: true     // Show AND/OR connectors
});

qb.appendTo('#querybuilder');
```

### Display Modes

**Inline Mode (Default):**
```
(City = Seattle) AND (Salary > 50000) AND (Title = Sales Rep)
```
- All rules in one horizontal line
- Compact layout
- Good for simple queries

**Vertical Mode:**
```
AND
├── City = Seattle
├── Salary > 50000
└── Title = Sales Rep
```
- Rules stacked vertically
- Shows hierarchy clearly
- Better for complex nested conditions

### Separator Styling

```typescript
// Custom separator CSS
const separatorStyle = `
.e-querybuilder .e-group-separator {
  background-color: #2196F3;
  padding: 0 10px;
  border-radius: 3px;
  color: white;
  font-weight: bold;
  margin: 0 5px;
}

.e-querybuilder .e-group-separator:hover {
  background-color: #1976D2;
  cursor: pointer;
}

/* Toggle separator style */
.e-querybuilder .e-group-separator.or {
  background-color: #f44336;
}

.e-querybuilder .e-group-separator.or:hover {
  background-color: #d32f2f;
}
`;
```

### Separate Connector Property

```typescript
// Show/hide AND/OR connectors
const withConnectors = new QueryBuilder({
  separateConnector: true,  // Show connectors
  dataSource: employeeData,
  columns: columns
});

const withoutConnectors = new QueryBuilder({
  separateConnector: false, // Hide connectors
  dataSource: employeeData,
  columns: columns
});
```

## Events and Handling

### Drag-Drop Events

```typescript
// Before rule is dragged
qb.addEventListener('dragStart', (args: any) => {
  console.log('Drag started:', args);
  // Can prevent drag if needed
  // args.cancel = true;
});

// While dragging
qb.addEventListener('drag', (args: any) => {
  console.log('Dragging:', args);
});

// After rule is dropped
qb.addEventListener('dragStop', (args: any) => {
  console.log('Dropped:', args);
  console.log('New rule order:', qb.getRules());
});
```

### Clone Events

```typescript
// Detect when rule is cloned (if supported)
qb.addEventListener('ruleAdded', (args: any) => {
  if (args.isClone) {
    console.log('Rule cloned:', args);
  } else {
    console.log('New rule added:', args);
  }
});
```

### Lock State Events

```typescript
// Track lock state changes
let lockedRules = new Set();

qb.addEventListener('ruleUpdated', (args: any) => {
  if (args.locked) {
    lockedRules.add(args.ruleId);
  } else {
    lockedRules.delete(args.ruleId);
  }
});
```

## Advanced Interactions

### Drag-Drop with Validation

```typescript
// Prevent dragging rules between incompatible groups
qb.addEventListener('dragStart', (args: any) => {
  const rule = args.data;
  const targetGroup = args.targetGroup;
  
  // Example: prevent dragging date rules to non-date groups
  if (rule.type === 'date' && targetGroup.type !== 'date') {
    args.cancel = true;
    alert('Cannot move date rules to non-date groups');
  }
});
```

### Clone with Modifications

```typescript
// Clone rule but modify the value
function cloneAndModifyRule(ruleId: string, newValue: any) {
  const rules = qb.getRules();
  const ruleToClone = findRuleById(rules, ruleId);
  
  if (ruleToClone) {
    const cloned = JSON.parse(JSON.stringify(ruleToClone));
    cloned.value = newValue;
    qb.addRules([cloned], 'group0');
  }
}

// Usage: Clone "City = Seattle" as "City = Tacoma"
cloneAndModifyRule('rule0', 'Tacoma');
```

### Protected Rules with Warnings

```typescript
// Mark critical rules and warn before deletion
const criticalRules = ['rule0', 'rule1']; // System rules

qb.addEventListener('ruleDelete', (args: any) => {
  if (criticalRules.includes(args.ruleId)) {
    const confirmed = confirm(
      'This is a critical rule. Delete anyway?'
    );
    args.cancel = !confirmed;
  }
});

qb.addEventListener('groupDelete', (args: any) => {
  const group = findRuleById(qb.getRules(), args.groupId);
  if (group && hasCriticalRules(group)) {
    alert('Cannot delete group with critical rules');
    args.cancel = true;
  }
});

function hasCriticalRules(group: RuleModel): boolean {
  return group.rules.some(rule => {
    if (!rule.rules) {
      return criticalRules.includes(rule.id);
    }
    return hasCriticalRules(rule as RuleModel);
  });
}
```

### Auto-Sort Rules After Drag

```typescript
// Automatically sort rules alphabetically by field after drag
qb.addEventListener('dragStop', (args: any) => {
  const rules = qb.getRules();
  rules.rules.sort((a, b) => {
    if (a.rules || b.rules) return 0; // Don't sort groups
    return (a.field || '').localeCompare(b.field || '');
  });
  
  qb.rule = rules;
  qb.refresh();
});
```

### Visual Feedback for Dragging

```typescript
// Add visual effects during drag
const style = document.createElement('style');
style.innerHTML = `
.e-querybuilder .e-drag-handler:hover {
  cursor: grab;
}

.e-querybuilder.dragging .e-rule {
  opacity: 0.7;
}

.e-querybuilder .e-rule.drag-over {
  background-color: #e3f2fd;
  border: 2px dashed #2196F3;
}

.e-querybuilder .e-group.drag-over {
  border: 2px dashed #ff9800;
}
`;
document.head.appendChild(style);

// Apply drag-over class
qb.addEventListener('drag', (args: any) => {
  const container = args.dropTarget;
  container?.classList.add('drag-over');
});

qb.addEventListener('dragStop', (args: any) => {
  document.querySelectorAll('.drag-over').forEach(el => {
    el.classList.remove('drag-over');
  });
});
```

---

**Need help?** Check [references/rules-and-groups.md](rules-and-groups.md) for managing rule structures or [references/templates-customization.md](templates-customization.md) for UI customization.
