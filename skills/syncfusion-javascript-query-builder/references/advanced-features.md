# Advanced Features and Troubleshooting

## Table of Contents
- [Overview](#overview)
- [Global and Local Configuration](#global-and-local-configuration)
- [Model Binding](#model-binding)
- [Advanced Validation](#advanced-validation)
- [Performance Optimization](#performance-optimization)
- [Event Handling](#event-handling)
- [Common Issues](#common-issues)
- [Best Practices](#best-practices)

## Overview

This reference covers advanced patterns for complex scenarios, troubleshooting common issues, and performance optimization.

## Global and Local Configuration

### Application-Wide Settings

```typescript
import { QueryBuilder } from '@syncfusion/ej2-querybuilder';

// Set global defaults
const queryBuilderConfig = {
  allowDragDrop: true,
  displayMode: 'Inline',
  enableAccessibility: true,
  enableRtl: false,
  locale: 'en',
  showButtons: {
    ruleDelete: true,
    groupDelete: true,
    groupInsert: true,
    ruleInsert: true
  }
};

// Create instances with global config
const createQueryBuilder = (containerID: string, columns, data) => {
  return new QueryBuilder({
    ...queryBuilderConfig,
    width: '100%',
    columns: columns,
    dataSource: data
  });
};

// Usage
const qb1 = createQueryBuilder('qb1', columns1, data1);
const qb2 = createQueryBuilder('qb2', columns2, data2);
```

### Instance-Specific Overrides

```typescript
// Global default but override for specific instance
const qb = new QueryBuilder({
  ...queryBuilderConfig,
  displayMode: 'Vertical',  // Override from vertical
  allowDragDrop: false,     // Disable for this instance
  columns: columns,
  dataSource: data
});

qb.appendTo('#querybuilder');
```

### Configuration Manager

```typescript
class QueryBuilderConfigManager {
  private static defaults = {
    allowDragDrop: true,
    displayMode: 'Inline',
    enableAccessibility: true,
    theme: 'material'
  };
  
  static getConfig(options?: any) {
    return { ...this.defaults, ...options };
  }
  
  static setDefaults(defaults: any) {
    this.defaults = { ...this.defaults, ...defaults };
  }
  
  static createInstance(container: string, columns: any, data: any, options?: any) {
    const config = this.getConfig(options);
    const qb = new QueryBuilder({
      ...config,
      columns: columns,
      dataSource: data
    });
    
    qb.appendTo(container);
    return qb;
  }
}

// Usage
QueryBuilderConfigManager.setDefaults({ locale: 'es', theme: 'fabric' });
const qb = QueryBuilderConfigManager.createInstance('#qb', cols, data);
```

## Model Binding

### Bind to Class Properties

```typescript
class FilterModel {
  condition: 'and' | 'or' = 'and';
  rules: FilterRule[] = [];
}

class FilterRule {
  field: string = '';
  operator: string = 'equal';
  value: any = '';
}

// Create QueryBuilder with model
const filterModel = new FilterModel();
filterModel.rules = [
  { field: 'City', operator: 'equal', value: 'Seattle' }
];

const qb = new QueryBuilder({
  rule: filterModel as any,  // Bind to model
  columns: columns,
  dataSource: data
});

qb.appendTo('#querybuilder');

// Get updated model
qb.addEventListener('change', () => {
  const updatedRule = qb.getRules();
  filterModel.condition = updatedRule.condition;
  filterModel.rules = updatedRule.rules as any;
});
```

### Two-Way Binding

```typescript
// Update QueryBuilder when model changes
class TwoWayQueryBuilder {
  private qb: QueryBuilder;
  private model: FilterModel;
  
  constructor(container: string, columns: any, data: any) {
    this.model = new FilterModel();
    
    this.qb = new QueryBuilder({
      rule: this.model as any,
      columns: columns,
      dataSource: data
    });
    
    this.qb.appendTo(container);
    this.attachListeners();
  }
  
  private attachListeners() {
    // Listen to QueryBuilder changes
    this.qb.addEventListener('change', () => {
      this.updateModel();
    });
  }
  
  private updateModel() {
    const rules = this.qb.getRules();
    this.model.condition = rules.condition;
    this.model.rules = rules.rules as any;
  }
  
  updateQueryBuilder(model: FilterModel) {
    this.model = model;
    this.qb.rule = model as any;
    this.qb.refresh();
  }
  
  getModel(): FilterModel {
    return this.model;
  }
}

const twqb = new TwoWayQueryBuilder('#qb', columns, data);
```

## Advanced Validation

### Custom Rule Validation

```typescript
// Validate rules before applying
function validateRulesAdvanced(rules: RuleModel): ValidationResult {
  const errors: ValidationError[] = [];
  
  function validate(ruleModel: RuleModel, path: string = 'root') {
    for (let i = 0; i < ruleModel.rules.length; i++) {
      const rule = ruleModel.rules[i];
      const rulePath = `${path}[${i}]`;
      
      if (rule.rules) {
        // Validate group
        if (!['and', 'or'].includes(rule.condition)) {
          errors.push({
            path: rulePath,
            message: `Invalid condition: ${rule.condition}`
          });
        }
        validate(rule as RuleModel, rulePath);
      } else {
        // Validate leaf rule
        if (!rule.field) {
          errors.push({ path: rulePath, message: 'Missing field' });
        }
        
        if (!rule.operator) {
          errors.push({ path: rulePath, message: 'Missing operator' });
        }
        
        if (rule.value === undefined || rule.value === '') {
          errors.push({
            path: rulePath,
            message: 'Missing value',
            rule: rule
          });
        }
        
        // Type-specific validation
        validateValueType(rule, rulePath, errors);
        
        // Cross-field validation
        validateCrossFields(rule, ruleModel, rulePath, errors);
      }
    }
  }
  
  validate(rules);
  
  return {
    valid: errors.length === 0,
    errors: errors
  };
}

interface ValidationError {
  path: string;
  message: string;
  rule?: any;
}

interface ValidationResult {
  valid: boolean;
  errors: ValidationError[];
}

function validateValueType(
  rule: Rule,
  path: string,
  errors: ValidationError[]
) {
  switch (rule.type) {
    case 'number':
      if (typeof rule.value !== 'number') {
        errors.push({
          path,
          message: `${rule.field} must be a number, got ${typeof rule.value}`
        });
      }
      break;
      
    case 'date':
      if (!(rule.value instanceof Date) && typeof rule.value !== 'string') {
        errors.push({
          path,
          message: `${rule.field} must be a date`
        });
      }
      break;
      
    case 'string':
      if (typeof rule.value !== 'string') {
        errors.push({
          path,
          message: `${rule.field} must be a string`
        });
      }
      break;
  }
}

function validateCrossFields(
  rule: Rule,
  group: RuleModel,
  path: string,
  errors: ValidationError[]
) {
  // Example: Can't have two equal filters for same field
  const sameFieldRules = group.rules.filter(r =>
    !r.rules && r.field === rule.field && r.operator === 'equal'
  );
  
  if (sameFieldRules.length > 1) {
    errors.push({
      path,
      message: `Duplicate filter for field: ${rule.field}`
    });
  }
}

// Usage
const rules = qb.getRules();
const validation = validateRulesAdvanced(rules);

if (!validation.valid) {
  console.error('Validation errors:', validation.errors);
  validation.errors.forEach(err => {
    console.error(`[${err.path}] ${err.message}`);
  });
}
```

## Performance Optimization

### Large Dataset Handling

```typescript
// Use virtual scrolling for large datasets
const largeDataset = generateMillionRecords();

const qb = new QueryBuilder({
  width: '100%',
  dataSource: largeDataset,
  columns: columns,
  
  // Performance optimization
  enableVirtualization: true,  // If supported
  pageSize: 50  // Load in batches
});

qb.appendTo('#querybuilder');

function generateMillionRecords() {
  const records = [];
  for (let i = 0; i < 1000000; i++) {
    records.push({
      id: i,
      name: `Employee ${i}`,
      dept: ['Sales', 'IT', 'HR', 'Finance'][i % 4],
      salary: 50000 + Math.random() * 100000
    });
  }
  return records;
}
```

### Defer Rule Processing

```typescript
// Don't process rules until explicitly applied
class DeferredQueryBuilder {
  private qb: QueryBuilder;
  private pendingChanges = false;
  private debounceTimer: NodeJS.Timeout;
  
  constructor(container: string, columns: any, data: any) {
    this.qb = new QueryBuilder({
      columns: columns,
      dataSource: data
    });
    
    this.qb.appendTo(container);
    this.setupDeferredProcessing();
  }
  
  private setupDeferredProcessing() {
    this.qb.addEventListener('change', () => {
      this.pendingChanges = true;
      
      clearTimeout(this.debounceTimer);
      this.debounceTimer = setTimeout(() => {
        this.applyChanges();
      }, 500);  // Wait 500ms before processing
    });
  }
  
  private applyChanges() {
    const rules = this.qb.getRules();
    console.log('Applying filter with rules:', rules);
    // Process rules here
    this.pendingChanges = false;
  }
  
  forceApply() {
    clearTimeout(this.debounceTimer);
    this.applyChanges();
  }
}
```

### Memoization

```typescript
// Cache filtered results
class CachedQueryBuilder {
  private cache: Map<string, any> = new Map();
  private qb: QueryBuilder;
  
  constructor(container: string, columns: any, data: any) {
    this.qb = new QueryBuilder({
      columns: columns,
      dataSource: data
    });
    
    this.qb.appendTo(container);
  }
  
  executeFilter(data: any[]) {
    const rules = this.qb.getRules();
    const cacheKey = JSON.stringify(rules);
    
    // Check cache
    if (this.cache.has(cacheKey)) {
      return this.cache.get(cacheKey);
    }
    
    // Filter and cache
    const result = this.filterData(data, rules);
    this.cache.set(cacheKey, result);
    
    return result;
  }
  
  private filterData(data: any[], rules: RuleModel): any[] {
    // Apply filtering logic
    return data.filter(item => evaluateRules(item, rules));
  }
  
  clearCache() {
    this.cache.clear();
  }
}
```

## Event Handling

### Complete Event List

```typescript
const qb = new QueryBuilder({
  columns: columns,
  dataSource: data
});

qb.appendTo('#querybuilder');

// Rule events
qb.addEventListener('ruleAdded', (args: any) => {
  console.log('Rule added:', args);
});

qb.addEventListener('ruleUpdated', (args: any) => {
  console.log('Rule updated:', args);
});

qb.addEventListener('ruleDeleted', (args: any) => {
  console.log('Rule deleted:', args);
});

// Group events
qb.addEventListener('groupAdded', (args: any) => {
  console.log('Group added:', args);
});

qb.addEventListener('groupUpdated', (args: any) => {
  console.log('Group updated:', args);
});

qb.addEventListener('groupDeleted', (args: any) => {
  console.log('Group deleted:', args);
});

// Change event
qb.addEventListener('change', (args: any) => {
  console.log('Query changed:', qb.getRules());
});

// Drag-drop events
qb.addEventListener('dragStart', (args: any) => {
  console.log('Drag started:', args);
});

qb.addEventListener('drag', (args: any) => {
  console.log('Dragging:', args);
});

qb.addEventListener('dragStop', (args: any) => {
  console.log('Drag stopped:', args);
});
```

## Common Issues

### Issue 1: Column Field Not Found

**Symptom:** Column data appears empty in dropdowns

```typescript
// ❌ WRONG - field name doesn't match data
const columns = [{ field: 'FirstN', label: 'Name' }];
const data = [{ FirstName: 'John' }];  // Different name

// ✅ CORRECT - field matches data
const columns = [{ field: 'FirstName', label: 'Name' }];
const data = [{ FirstName: 'John' }];
```

### Issue 2: Rules Not Saving

**Symptom:** Changes lost after page refresh

```typescript
// ❌ WRONG - not persisting
const rules = qb.getRules();
// (forgot to save)

// ✅ CORRECT - save to storage
qb.addEventListener('change', () => {
  const rules = qb.getRules();
  localStorage.setItem('savedRules', JSON.stringify(rules));
});
```

### Issue 3: RTL Not Working

**Symptom:** Text still flows left-to-right

```typescript
// ❌ WRONG - only enableRtl on component
const qb = new QueryBuilder({ enableRtl: true });

// ✅ CORRECT - set HTML dir and enableRtl
document.documentElement.dir = 'rtl';
const qb = new QueryBuilder({ enableRtl: true });
```

### Issue 4: Slow with Large Data

**Symptom:** Freezes when typing in dropdowns

```typescript
// ❌ WRONG - thousands of values
{ field: 'Product', values: allProductsList }  // Too many

// ✅ CORRECT - use remote filtering or pagination
{ field: 'Product', type: 'string' }
// Server filters suggestions as user types
```

## Best Practices

### 1. Validate Early

```typescript
// Validate before processing
function processQuery() {
  const validation = validateRulesAdvanced(qb.getRules());
  
  if (!validation.valid) {
    showValidationErrors(validation.errors);
    return;
  }
  
  // Safe to proceed
  executeFilter();
}
```

### 2. Provide User Feedback

```typescript
// Show feedback for user actions
qb.addEventListener('change', () => {
  showMessage('Filter updated');
});

qb.addEventListener('ruleAdded', () => {
  showMessage('Rule added', 'success');
});

qb.addEventListener('ruleDeleted', () => {
  showMessage('Rule removed', 'info');
});

function showMessage(text: string, type: string = 'info') {
  const msg = document.createElement('div');
  msg.className = `message ${type}`;
  msg.textContent = text;
  document.body.appendChild(msg);
  
  setTimeout(() => msg.remove(), 3000);
}
```

### 3. Handle Errors Gracefully

```typescript
// Catch and handle errors
try {
  const importedRules = JSON.parse(jsonString);
  const validation = validateRulesAdvanced(importedRules);
  
  if (!validation.valid) {
    throw new Error(`Validation failed: ${validation.errors[0].message}`);
  }
  
  qb.rule = importedRules;
  qb.refresh();
} catch (error) {
  console.error('Error:', error);
  showError(`Failed to load query: ${error.message}`);
}
```

### 4. Optimize for Performance

```typescript
// Debounce expensive operations
function createOptimizedQB(container: string, columns: any, data: any) {
  const qb = new QueryBuilder({
    columns: columns,
    dataSource: data,
    allowDragDrop: true,
    displayMode: 'Inline'
  });
  
  qb.appendTo(container);
  
  let timeout: NodeJS.Timeout;
  qb.addEventListener('change', () => {
    clearTimeout(timeout);
    timeout = setTimeout(() => {
      const rules = qb.getRules();
      applyFilter(rules);  // Expensive operation
    }, 300);
  });
  
  return qb;
}
```

---

**Need help?** Check other references for specific features or [references/getting-started.md](getting-started.md) for basics.
