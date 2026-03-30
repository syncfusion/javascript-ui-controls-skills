# Filtering and Search Operations

## Table of Contents
- [Overview](#overview)
- [Basic Filtering](#basic-filtering)
- [ShowButtons Configuration](#showbuttons-configuration)
- [Search Functionality](#search-functionality)
- [Custom Filters](#custom-filters)
- [Query Execution](#query-execution)
- [Event Handling](#event-handling)

## Overview

QueryBuilder provides UI controls for creating, modifying, and executing filters. Users can add/remove rules and groups through buttons, or programmatically via methods.

## Basic Filtering

### Enable/Disable Buttons

```typescript
import { QueryBuilder, ColumnsModel } from '@syncfusion/ej2-querybuilder';

const qb = new QueryBuilder({
  width: '100%',
  dataSource: employeeData,
  columns: columns,
  showButtons: {
    ruleDelete: true,      // Show delete button on rules
    groupDelete: true,     // Show delete button on groups
    groupInsert: true,     // Show add group button
    ruleInsert: true       // Show add rule button
  }
});

qb.appendTo('#querybuilder');
```

### Default Button Configuration

```typescript
// Default behavior (all buttons enabled)
showButtons: {
  ruleDelete: true,
  groupDelete: true,
  groupInsert: true,
  ruleInsert: true
}

// Disable deletion
showButtons: {
  ruleDelete: false,
  groupDelete: false,
  groupInsert: true,
  ruleInsert: true
}

// Read-only QueryBuilder (all buttons disabled)
showButtons: {
  ruleDelete: false,
  groupDelete: false,
  groupInsert: false,
  ruleInsert: false
}
```

### User Interactions

With buttons enabled, users can:

1. **Add Rule** - Click "+" button to add a new condition
2. **Add Group** - Click "Add Group" to create nested conditions
3. **Delete Rule** - Click "X" button to remove a condition
4. **Delete Group** - Remove entire group of conditions
5. **Change Condition** - Switch between AND/OR logic

```typescript
// UI with buttons (user interaction)
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

// Programmatic changes (backend logic)
document.getElementById('addRuleBtn').addEventListener('click', () => {
  qb.addRules([{
    field: 'City',
    operator: 'equal',
    value: 'Seattle'
  }], 'group0');
});
```

## ShowButtons Configuration

### Individual Button Control

```typescript
interface ShowButtons {
  ruleDelete?: boolean;     // Delete rule button
  groupDelete?: boolean;    // Delete group button
  groupInsert?: boolean;    // Insert group button
  ruleInsert?: boolean;     // Insert rule button
}
```

### Use Cases

#### Scenario 1: Allow Only Rule Addition

```typescript
// Users can add rules but not delete
showButtons: {
  ruleDelete: false,
  groupDelete: false,
  groupInsert: false,
  ruleInsert: true
}
```

#### Scenario 2: Template-Based Filtering

```typescript
// Predefined template - users refine but don't add new rules
showButtons: {
  ruleDelete: true,     // Can delete existing
  groupDelete: false,   // Cannot delete groups
  groupInsert: false,   // Cannot add groups
  ruleInsert: false     // Cannot add rules
}
```

#### Scenario 3: Read-Only Viewer

```typescript
// Display rules without editing capability
showButtons: {
  ruleDelete: false,
  groupDelete: false,
  groupInsert: false,
  ruleInsert: false
}
```

## Search Functionality

### Basic Search Within QueryBuilder

While QueryBuilder doesn't have built-in search, you can implement field search:

```typescript
// Search for columns by name
function searchColumns(searchTerm: string): ColumnsModel[] {
  return columns.filter(col =>
    col.label?.toLowerCase().includes(searchTerm.toLowerCase()) ||
    col.field.toLowerCase().includes(searchTerm.toLowerCase())
  );
}

// Dynamically update columns based on search
const searchInput = document.getElementById('columnSearch') as HTMLInputElement;
searchInput.addEventListener('input', (e) => {
  const filtered = searchColumns(e.target.value);
  qb.columns = filtered;
  qb.refresh();
});
```

### Search in Filter Values

```typescript
// Get all rules and search their values
function searchRuleValues(searchTerm: string): Rule[] {
  const results: Rule[] = [];
  
  function traverse(rules: RuleModel) {
    for (const rule of rules.rules) {
      if (!rule.rules) {
        // Leaf rule
        if (String(rule.value).toLowerCase().includes(searchTerm.toLowerCase())) {
          results.push(rule);
        }
      } else {
        // Group - recurse
        traverse(rule as RuleModel);
      }
    }
  }
  
  traverse(qb.getRules());
  return results;
}

// Search example
const matches = searchRuleValues('Seattle');
console.log('Rules matching Seattle:', matches);
```

## Custom Filters

### Implement Custom Filter Logic

```typescript
// Create custom filter functions
function applyCustomFilter() {
  const rules = qb.getRules();
  const customRules: RuleModel = {
    condition: 'and',
    rules: [
      {
        field: 'Salary',
        operator: 'between',
        value: [50000, 150000]
      },
      {
        condition: 'or',
        rules: [
          { field: 'Title', operator: 'contains', value: 'Manager' },
          { field: 'Title', operator: 'contains', value: 'Director' }
        ]
      }
    ]
  };
  
  // Combine with existing rules
  const combined: RuleModel = {
    condition: 'and',
    rules: [
      ...rules.rules,
      ...customRules.rules
    ]
  };
  
  return combined;
}
```

### Restrict Operators Per Column

```typescript
const restrictedColumns: ColumnsModel[] = [
  {
    field: 'Status',
    type: 'string',
    operators: [
      { key: 'equal', text: '=' },
      { key: 'notequal', text: '≠' }
    ]
    // Only equal and notequal allowed - no contains/startswith
  },
  {
    field: 'CreatedDate',
    type: 'date',
    operators: [
      { key: 'between', text: 'Between' },
      { key: 'equal', text: 'On' }
    ]
    // Only between and equal for dates
  }
];

const qb = new QueryBuilder({
  columns: restrictedColumns,
  dataSource: data
});

qb.appendTo('#querybuilder');
```

### Validate Filter Conditions

```typescript
function validateFilter(): { valid: boolean; errors: string[] } {
  const errors: string[] = [];
  const rules = qb.getRules();
  
  function check(ruleModel: RuleModel, depth = 0) {
    if (depth > 5) {
      errors.push('Filter nesting too deep (max 5 levels)');
    }
    
    for (const rule of ruleModel.rules) {
      if (!rule.rules) {
        // Validate leaf rule
        if (!rule.field) errors.push('Rule missing field');
        if (!rule.operator) errors.push('Rule missing operator');
        if (rule.value === undefined || rule.value === null) {
          errors.push(`Rule ${rule.field} missing value`);
        }
        
        // Type-specific validation
        if (rule.type === 'number' && typeof rule.value !== 'number') {
          errors.push(`${rule.field} should be a number`);
        }
      } else {
        check(rule as RuleModel, depth + 1);
      }
    }
  }
  
  check(rules);
  
  return {
    valid: errors.length === 0,
    errors
  };
}

// Usage
const validation = validateFilter();
if (!validation.valid) {
  console.error('Filter validation failed:', validation.errors);
} else {
  console.log('Filter is valid');
}
```

## Query Execution

### Get Query Results

```typescript
// Get the current filter as a rule
const filter = qb.getRules();

// Filter data based on rules
function executeFilter(data: any[], filter: RuleModel): any[] {
  return data.filter(item => evaluateRules(item, filter));
}

function evaluateRules(item: any, ruleModel: RuleModel): boolean {
  const results = ruleModel.rules.map(rule => {
    if (rule.rules) {
      // It's a group - recurse
      return evaluateRules(item, rule as RuleModel);
    } else {
      // It's a rule - evaluate
      return evaluateRule(item, rule);
    }
  });
  
  // Combine with AND/OR
  if (ruleModel.condition === 'and') {
    return results.every(r => r === true);
  } else {
    return results.some(r => r === true);
  }
}

function evaluateRule(item: any, rule: Rule): boolean {
  const fieldValue = item[rule.field];
  const ruleValue = rule.value;
  
  switch (rule.operator) {
    case 'equal':
      return fieldValue === ruleValue;
    case 'notequal':
      return fieldValue !== ruleValue;
    case 'contains':
      return String(fieldValue).includes(String(ruleValue));
    case 'startswith':
      return String(fieldValue).startsWith(String(ruleValue));
    case 'endswith':
      return String(fieldValue).endsWith(String(ruleValue));
    case 'greaterthan':
      return fieldValue > ruleValue;
    case 'lessthan':
      return fieldValue < ruleValue;
    case 'between':
      return fieldValue >= ruleValue[0] && fieldValue <= ruleValue[1];
    default:
      return true;
  }
}

// Execute filter
const filteredData = executeFilter(employeeData, qb.getRules());
console.log('Filtered results:', filteredData);
```

### Export Query to SQL

```typescript
function rulestoSQL(rules: RuleModel): string {
  function buildSQL(ruleModel: RuleModel): string {
    const sqlParts = ruleModel.rules.map(rule => {
      if (rule.rules) {
        // It's a group
        return `(${buildSQL(rule as RuleModel)})`;
      } else {
        // It's a rule
        const field = rule.field;
        const op = getSQLOperator(rule.operator);
        const value = formatSQLValue(rule.value, rule.type);
        return `${field} ${op} ${value}`;
      }
    });
    
    const operator = ruleModel.condition === 'and' ? 'AND' : 'OR';
    return sqlParts.join(` ${operator} `);
  }
  
  function getSQLOperator(op: string): string {
    const map = {
      'equal': '=',
      'notequal': '!=',
      'contains': 'LIKE',
      'startswith': 'LIKE',
      'endswith': 'LIKE',
      'greaterthan': '>',
      'lessthan': '<'
    };
    return map[op] || '=';
  }
  
  function formatSQLValue(value: any, type: string): string {
    if (type === 'string') {
      return `'${String(value).replace(/'/g, "''")}'`;
    }
    return String(value);
  }
  
  return `SELECT * FROM table WHERE ${buildSQL(rules)}`;
}

// Export to SQL
const sqlQuery = rulestoSQL(qb.getRules());
console.log('SQL Query:', sqlQuery);
// Output: SELECT * FROM table WHERE (City = 'Seattle') AND (Salary > 50000)
```

## Event Handling

### Listen to Filter Changes

```typescript
// When rules change, handle the event
qb.addEventListener('change', (args: any) => {
  console.log('Filter changed');
  const newRules = qb.getRules();
  console.log('New rules:', newRules);
});

// When rule is added
qb.addEventListener('ruleAdded', (args: any) => {
  console.log('Rule added:', args);
});

// When rule is deleted
qb.addEventListener('ruleDeleted', (args: any) => {
  console.log('Rule deleted:', args);
});

// When group is added
qb.addEventListener('groupAdded', (args: any) => {
  console.log('Group added:', args);
});

// When group is deleted
qb.addEventListener('groupDeleted', (args: any) => {
  console.log('Group deleted:', args);
});
```

### Auto-Save Filter

```typescript
// Auto-save filter to localStorage when changed
qb.addEventListener('change', () => {
  const rules = qb.getRules();
  localStorage.setItem('savedFilter', JSON.stringify(rules));
  console.log('Filter auto-saved');
});

// Load saved filter on page load
function loadSavedFilter() {
  const saved = localStorage.getItem('savedFilter');
  if (saved) {
    const rules: RuleModel = JSON.parse(saved);
    qb.rule = rules;
    qb.refresh();
    console.log('Filter loaded');
  }
}

window.addEventListener('DOMContentLoaded', loadSavedFilter);
```

### Apply Filter Action

```typescript
// Create "Apply Filter" button
const applyBtn = document.getElementById('applyFilter');
applyBtn?.addEventListener('click', () => {
  const rules = qb.getRules();
  
  // Send to server or apply locally
  applyFilterToData(rules);
});

function applyFilterToData(rules: RuleModel) {
  const filteredData = executeFilter(employeeData, rules);
  updateUI(filteredData);
  
  // Could also send to server
  fetch('/api/filter', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(rules)
  });
}
```

---

**Need help?** Check [references/rules-and-groups.md](rules-and-groups.md) for managing rules or [references/import-export.md](import-export.md) for saving filters.
