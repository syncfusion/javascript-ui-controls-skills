# Import/Export and Persistence

## Table of Contents
- [Overview](#overview)
- [Export to JSON](#export-to-json)
- [Import from JSON](#import-from-json)
- [SQL Generation](#sql-generation)
- [Data Persistence](#data-persistence)
- [State Management](#state-management)
- [Serialization Patterns](#serialization-patterns)

## Overview

QueryBuilder supports exporting and importing queries for:
- Saving user-created filters
- Sharing filters between users
- Storing in databases
- Generating SQL queries
- State management and history

## Export to JSON

### Basic Export

```typescript
import { QueryBuilder } from '@syncfusion/ej2-querybuilder';

const qb = new QueryBuilder({
  width: '100%',
  dataSource: employeeData,
  columns: columns
});

qb.appendTo('#querybuilder');

// Export current rules as JSON
const rules = qb.getRules();
const jsonString = JSON.stringify(rules);
console.log('Exported JSON:', jsonString);
```

### Export Format

```json
{
  "condition": "and",
  "rules": [
    {
      "id": "rule0",
      "label": "Employee ID",
      "field": "EmployeeID",
      "type": "number",
      "operator": "greaterthan",
      "value": 1
    },
    {
      "id": "group1",
      "condition": "or",
      "rules": [
        {
          "id": "rule1",
          "label": "Title",
          "field": "Title",
          "type": "string",
          "operator": "contains",
          "value": "Sales"
        },
        {
          "id": "rule2",
          "label": "Title",
          "field": "Title",
          "type": "string",
          "operator": "contains",
          "value": "Manager"
        }
      ]
    }
  ]
}
```

### Save to File

```typescript
// Export to JSON file
function exportToFile() {
  const rules = qb.getRules();
  const jsonString = JSON.stringify(rules, null, 2);
  
  const blob = new Blob([jsonString], { type: 'application/json' });
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a');
  a.href = url;
  a.download = `query-${Date.now()}.json`;
  a.click();
  URL.revokeObjectURL(url);
}

// Add button to trigger export
document.getElementById('exportBtn')?.addEventListener('click', exportToFile);
```

### Save to Database

```typescript
// Send rules to server
async function saveQueryToDatabase(name: string) {
  const rules = qb.getRules();
  
  const response = await fetch('/api/queries', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      name: name,
      rules: rules,
      createdAt: new Date().toISOString()
    })
  });
  
  const saved = await response.json();
  console.log('Query saved with ID:', saved.id);
}

// Usage
document.getElementById('saveBtn')?.addEventListener('click', () => {
  const queryName = (document.getElementById('queryName') as HTMLInputElement).value;
  if (queryName) {
    saveQueryToDatabase(queryName);
  }
});
```

## Import from JSON

### Basic Import

```typescript
// Import rules from JSON
const importedJSON = `{
  "condition": "and",
  "rules": [
    {
      "field": "City",
      "type": "string",
      "operator": "equal",
      "value": "Seattle"
    },
    {
      "field": "Salary",
      "type": "number",
      "operator": "greaterthan",
      "value": 50000
    }
  ]
}`;

const importedRules = JSON.parse(importedJSON);

const qb = new QueryBuilder({
  width: '100%',
  dataSource: employeeData,
  columns: columns,
  rule: importedRules  // Load with imported rules
});

qb.appendTo('#querybuilder');
```

### Load from File

```typescript
// Load JSON file
function loadFromFile(file: File) {
  const reader = new FileReader();
  
  reader.onload = (event) => {
    try {
      const content = event.target?.result as string;
      const rules = JSON.parse(content);
      
      qb.rule = rules;
      qb.refresh();
      console.log('Query loaded from file');
    } catch (error) {
      console.error('Invalid JSON file:', error);
    }
  };
  
  reader.readAsText(file);
}

// File input handler
document.getElementById('fileInput')?.addEventListener('change', (e) => {
  const file = (e.target as HTMLInputElement).files?.[0];
  if (file) {
    loadFromFile(file);
  }
});
```

### Load from Database

```typescript
// Fetch and load saved query
async function loadQueryFromDatabase(queryId: string) {
  const response = await fetch(`/api/queries/${queryId}`);
  const data = await response.json();
  
  qb.rule = data.rules;
  qb.refresh();
  console.log('Query loaded:', data.name);
}

// Load from dropdown
document.getElementById('queryDropdown')?.addEventListener('change', (e) => {
  const queryId = (e.target as HTMLSelectElement).value;
  if (queryId) {
    loadQueryFromDatabase(queryId);
  }
});
```

### Validate Before Import

```typescript
// Validate imported rules
function validateRules(rules: RuleModel): { valid: boolean; errors: string[] } {
  const errors: string[] = [];
  
  function validate(ruleModel: RuleModel) {
    for (const rule of ruleModel.rules) {
      if (rule.rules) {
        // It's a group
        if (!['and', 'or'].includes(rule.condition)) {
          errors.push(`Invalid condition: ${rule.condition}`);
        }
        validate(rule as RuleModel);
      } else {
        // It's a rule - validate fields
        if (!rule.field) errors.push('Rule missing field');
        if (!rule.operator) errors.push('Rule missing operator');
        if (rule.value === undefined) errors.push('Rule missing value');
        
        // Check if field exists in columns
        const fieldExists = columns.some(c => c.field === rule.field);
        if (!fieldExists) {
          errors.push(`Unknown field: ${rule.field}`);
        }
      }
    }
  }
  
  validate(rules);
  return {
    valid: errors.length === 0,
    errors
  };
}

// Import with validation
async function safeImportRules(jsonString: string) {
  try {
    const rules = JSON.parse(jsonString);
    const validation = validateRules(rules);
    
    if (!validation.valid) {
      console.error('Validation failed:', validation.errors);
      return false;
    }
    
    qb.rule = rules;
    qb.refresh();
    return true;
  } catch (error) {
    console.error('Parse error:', error);
    return false;
  }
}
```

## SQL Generation

### Convert Rules to SQL

```typescript
// Generate SQL WHERE clause from rules
function rulesToSQL(rules: RuleModel): string {
  function buildSQL(ruleModel: RuleModel): string {
    const conditions = ruleModel.rules.map(rule => {
      if (rule.rules) {
        // It's a group - wrap in parentheses
        return `(${buildSQL(rule as RuleModel)})`;
      } else {
        // It's a rule - build condition
        const field = rule.field;
        const value = formatValue(rule.value, rule.type);
        const operator = sqlOperator(rule.operator);
        
        switch (rule.operator) {
          case 'between':
            return `${field} BETWEEN ${value[0]} AND ${value[1]}`;
          case 'notbetween':
            return `${field} NOT BETWEEN ${value[0]} AND ${value[1]}`;
          case 'in':
            return `${field} IN (${value})`;
          case 'notin':
            return `${field} NOT IN (${value})`;
          case 'contains':
          case 'startswith':
          case 'endswith':
            return `${field} LIKE '${value}'`;
          default:
            return `${field} ${operator} ${value}`;
        }
      }
    });
    
    const connector = ruleModel.condition === 'and' ? 'AND' : 'OR';
    return conditions.join(` ${connector} `);
  }
  
  function sqlOperator(op: string): string {
    const map: {[key: string]: string} = {
      'equal': '=',
      'notequal': '!=',
      'greaterthan': '>',
      'lessthan': '<',
      'greaterthanorequal': '>=',
      'lessthanorequal': '<=',
      'contains': 'LIKE',
      'startswith': 'LIKE',
      'endswith': 'LIKE'
    };
    return map[op] || '=';
  }
  
  function formatValue(value: any, type: string): any {
    if (type === 'string') {
      return String(value).replace(/'/g, "''");
    } else if (type === 'date') {
      return `'${new Date(value).toISOString().split('T')[0]}'`;
    }
    return value;
  }
  
  return `SELECT * FROM table WHERE ${buildSQL(rules)}`;
}

// Usage
const rules = qb.getRules();
const sqlQuery = rulesToSQL(rules);
console.log('SQL:', sqlQuery);
// Output: SELECT * FROM table WHERE (City = 'Seattle') AND ((Title LIKE 'Sales') OR (Title LIKE 'Manager'))
```

### SQL for Different Databases

```typescript
// MongoDB query
function rulesToMongoQuery(rules: RuleModel): object {
  function buildMongo(ruleModel: RuleModel): object {
    const $and = ruleModel.rules.map(rule => {
      if (rule.rules) {
        return buildMongo(rule as RuleModel);
      } else {
        return {
          [rule.field]: mongoOperator(rule.operator, rule.value)
        };
      }
    });
    
    if (ruleModel.condition === 'and') {
      return { $and };
    } else {
      return { $or: $and };
    }
  }
  
  function mongoOperator(op: string, value: any): object {
    const map: {[key: string]: object} = {
      'equal': { $eq: value },
      'notequal': { $ne: value },
      'greaterthan': { $gt: value },
      'lessthan': { $lt: value },
      'contains': { $regex: value },
      'in': { $in: value }
    };
    return map[op] || { $eq: value };
  }
  
  return buildMongo(rules);
}

// Elasticsearch query
function rulesToElasticsearch(rules: RuleModel): object {
  // Similar pattern with Elasticsearch query syntax
}
```

## Data Persistence

### LocalStorage

```typescript
// Save to browser localStorage
function saveToLocalStorage(key: string) {
  const rules = qb.getRules();
  localStorage.setItem(key, JSON.stringify(rules));
  console.log('Saved to localStorage');
}

// Load from localStorage
function loadFromLocalStorage(key: string) {
  const saved = localStorage.getItem(key);
  if (saved) {
    const rules = JSON.parse(saved);
    qb.rule = rules;
    qb.refresh();
    console.log('Loaded from localStorage');
  }
}

// Auto-save every 10 seconds
setInterval(() => {
  saveToLocalStorage('currentQuery');
}, 10000);
```

### SessionStorage

```typescript
// Temporary session storage
function saveToSession() {
  const rules = qb.getRules();
  sessionStorage.setItem('querySession', JSON.stringify(rules));
}

function loadFromSession() {
  const saved = sessionStorage.getItem('querySession');
  if (saved) {
    qb.rule = JSON.parse(saved);
    qb.refresh();
  }
}

// Clear on logout
window.addEventListener('beforeunload', () => {
  sessionStorage.removeItem('querySession');
});
```

### IndexedDB (Large Storage)

```typescript
// IndexedDB for larger data
const dbName = 'QueryBuilderDB';
const storeName = 'queries';

function openDB(): Promise<IDBDatabase> {
  return new Promise((resolve, reject) => {
    const request = indexedDB.open(dbName, 1);
    
    request.onupgradeneeded = (e) => {
      const db = (e.target as IDBOpenDBRequest).result;
      db.createObjectStore(storeName, { keyPath: 'id', autoIncrement: true });
    };
    
    request.onsuccess = () => resolve(request.result);
    request.onerror = () => reject(request.error);
  });
}

async function saveToIndexedDB(queryName: string) {
  const db = await openDB();
  const tx = db.transaction(storeName, 'readwrite');
  const store = tx.objectStore(storeName);
  
  store.add({
    name: queryName,
    rules: qb.getRules(),
    createdAt: new Date()
  });
}

async function loadFromIndexedDB(queryId: number) {
  const db = await openDB();
  const tx = db.transaction(storeName, 'readonly');
  const store = tx.objectStore(storeName);
  const request = store.get(queryId);
  
  request.onsuccess = () => {
    if (request.result) {
      qb.rule = request.result.rules;
      qb.refresh();
    }
  };
}
```

## State Management

### Query History

```typescript
class QueryHistory {
  private history: RuleModel[] = [];
  private currentIndex = -1;
  private maxSize = 50;
  
  add(rules: RuleModel) {
    // Remove redo history
    this.history = this.history.slice(0, this.currentIndex + 1);
    
    // Add new state
    this.history.push(JSON.parse(JSON.stringify(rules)));
    this.currentIndex++;
    
    // Limit history size
    if (this.history.length > this.maxSize) {
      this.history.shift();
      this.currentIndex--;
    }
  }
  
  undo(): RuleModel | null {
    if (this.currentIndex > 0) {
      this.currentIndex--;
      return this.history[this.currentIndex];
    }
    return null;
  }
  
  redo(): RuleModel | null {
    if (this.currentIndex < this.history.length - 1) {
      this.currentIndex++;
      return this.history[this.currentIndex];
    }
    return null;
  }
  
  canUndo(): boolean {
    return this.currentIndex > 0;
  }
  
  canRedo(): boolean {
    return this.currentIndex < this.history.length - 1;
  }
}

// Usage
const history = new QueryHistory();

qb.addEventListener('change', () => {
  history.add(qb.getRules());
  updateHistoryButtons();
});

document.getElementById('undoBtn')?.addEventListener('click', () => {
  const prev = history.undo();
  if (prev) {
    qb.rule = prev;
    qb.refresh();
    updateHistoryButtons();
  }
});

document.getElementById('redoBtn')?.addEventListener('click', () => {
  const next = history.redo();
  if (next) {
    qb.rule = next;
    qb.refresh();
    updateHistoryButtons();
  }
});

function updateHistoryButtons() {
  document.getElementById('undoBtn')!.disabled = !history.canUndo();
  document.getElementById('redoBtn')!.disabled = !history.canRedo();
}
```

## Serialization Patterns

### Custom Serialization

```typescript
// Add metadata to serialized data
function serializeWithMetadata(rules: RuleModel): string {
  const data = {
    version: '1.0',
    createdAt: new Date().toISOString(),
    application: 'QueryBuilder',
    rules: rules
  };
  
  return JSON.stringify(data);
}

// Deserialize with version checking
function deserializeWithValidation(jsonString: string) {
  const data = JSON.parse(jsonString);
  
  if (data.version !== '1.0') {
    console.warn('Different version:', data.version);
  }
  
  if (new Date(data.createdAt) < new Date(Date.now() - 30*24*60*60*1000)) {
    console.warn('Query is older than 30 days');
  }
  
  return data.rules;
}
```

### Compression for Large Rules

```typescript
// Compress JSON before storage
function compressRules(rules: RuleModel): string {
  const jsonString = JSON.stringify(rules);
  return btoa(jsonString); // Base64 encode
}

function decompressRules(compressed: string): RuleModel {
  const jsonString = atob(compressed); // Base64 decode
  return JSON.parse(jsonString);
}

// Usage
const compressed = compressRules(qb.getRules());
localStorage.setItem('compressedQuery', compressed);

// Load
const loaded = decompressRules(localStorage.getItem('compressedQuery')!);
qb.rule = loaded;
```

---

**Need help?** Check [references/rules-and-groups.md](rules-and-groups.md) for rule structures or [references/filtering-search.md](filtering-search.md) for query execution.
