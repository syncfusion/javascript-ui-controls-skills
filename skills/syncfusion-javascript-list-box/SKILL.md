---
name: syncfusion-javascript-list-box
description: Comprehensive guide for implementing Syncfusion ListBox component for creating selectable lists with single/multiple selection, data binding, drag-and-drop functionality, sorting, grouping, keyboard navigation, and accessibility support. Use this skill when the user mentions ListBox, dropdown lists, item selection, multi-select lists, list reordering, or dual-list components.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion ListBox

## When to Use This Skill

Use this skill when:
- Creating a list component that displays selectable items
- Implementing single or multiple item selection
- Binding data from local or remote sources
- Enabling drag-and-drop reordering between lists
- Adding sorting or grouping to list items
- Requiring keyboard navigation and accessibility
- Building dual-list components for item transfer
- Customizing list appearance with icons and templates

## ListBox Overview

The Syncfusion ListBox is a versatile dropdown control that displays a list of items for user selection. It supports:

- **Selection Modes**: Single selection or multiple selection with checkboxes
- **Data Sources**: Local arrays, remote data via DataManager, or HTML elements
- **Organization**: Sorting and grouping capabilities
- **Interaction**: Drag-and-drop within same list or between lists, keyboard navigation
- **Accessibility**: Full WAI-ARIA support, keyboard shortcuts, screen reader compatibility
- **Customization**: Icons, templates, styling, and responsive design

## Documentation & Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation and package setup
- Basic ListBox initialization
- Simple data binding
- Minimal working example
- CSS imports and theming

### Data Binding & Field Mapping
📄 **Read:** [references/data-binding.md](references/data-binding.md)
- Local data sources (strings, objects, complex nested data)
- Remote data binding with DataManager
- Field mapping and configuration
- HTML element initialization (SELECT, UL)
- Best practices for complex data

### Selection & Checkboxes
📄 **Read:** [references/selection-and-checkboxes.md](references/selection-and-checkboxes.md)
- Single selection mode
- Multiple selection mode
- Checkbox selection with Select All option
- Keyboard shortcuts for selection
- Change event handling
- Programmatic selection methods

### Drag & Drop
📄 **Read:** [references/drag-and-drop.md](references/drag-and-drop.md)
- Single ListBox drag-and-drop reordering
- Multi-ListBox drag-and-drop with scope binding
- Drag events (dragStart, drag, drop)
- Handling item reordering
- Common patterns and gotchas

### Sorting & Grouping
📄 **Read:** [references/sorting-and-grouping.md](references/sorting-and-grouping.md)
- Ascending/descending sort order
- Grouping items by category
- HTML optgroup support
- Combining sorting with grouping
- Visual organization patterns

### Dual ListBox Configuration
📄 **Read:** [references/dual-list-box.md](references/dual-list-box.md)
- Two-list setup for item transfer
- Toolbar items and move operations
- Scope binding between lists
- Common use cases

### Accessibility & Keyboard Navigation
📄 **Read:** [references/accessibility-and-keyboard.md](references/accessibility-and-keyboard.md)
- ARIA attributes and roles
- Keyboard shortcuts reference
- WAI-ARIA compliance
- Screen reader support
- Accessible implementation patterns

### Styling & Appearance
📄 **Read:** [references/styling-and-appearance.md](references/styling-and-appearance.md)
- Icons and templates
- CSS customization
- Height and scrolling
- Theme integration
- Responsive design

### How-To Guide
📄 **Read:** [references/how-to-guide.md](references/how-to-guide.md)
- Add items dynamically
- Enable/disable items
- Select items programmatically
- Enable scrolling
- Show tooltips
- Filter list data
- Handle form submission

## Quick Start Example

```typescript
import { ListBox } from '@syncfusion/ej2-dropdowns';

// Sample data
const sportsData: { [key: string]: Object }[] = [
    { id: 'game1', sports: 'Badminton' },
    { id: 'game2', sports: 'Cricket' },
    { id: 'game3', sports: 'Football' },
    { id: 'game4', sports: 'Golf' },
    { id: 'game5', sports: 'Tennis' }
];

// Create ListBox with multiple selection
const listObj: ListBox = new ListBox({
    dataSource: sportsData,
    fields: { text: 'sports', value: 'id' },
    selectionSettings: {
        mode: 'Multiple',
        showCheckbox: true
    }
});

listObj.appendTo('#listbox');
```

## Common Patterns

### Pattern 1: Single Selection with Change Event
Use when you need to respond to user selections:
- Set `mode: 'Single'` in `selectionSettings`
- Bind to the `change` event
- Access selected item via `getSelectedItems()` method

### Pattern 2: Multi-Select with Checkboxes
Use for allowing multiple item selection:
- Set `mode: 'Multiple'` with `showCheckbox: true`
- Optionally add `showSelectAll: true` for select-all checkbox
- Use `selectAll()` method for programmatic selection

### Pattern 3: Drag-and-Drop Between Lists
Use for item transfer between lists:
- Set `allowDragAndDrop: true` on both lists
- Use matching `scope` value to link lists (e.g., `scope: '#target-list-id'`)
- Toolbar with move operations provides UI controls

### Pattern 4: Sorted and Grouped List
Use for organized data presentation:
- Set `sortOrder: 'Ascending'` or `'Descending'`
- Map `groupBy` field for grouping categories
- Combine for categorized, sorted display

### Pattern 5: Remote Data with Filtering
Use for large datasets from APIs:
- Create DataManager with API endpoint
- Use `Query` to filter/select fields
- Implement proper field mapping via `fields` property

## Key Props Summary

| Prop | Type | Purpose | Example |
|------|------|---------|---------|
| `dataSource` | Array / DataManager | Bind local or remote data | `[{text: 'Item'}]` |
| `fields` | FieldSettings | Map data object properties | `{ text: 'name', value: 'id' }` |
| `selectionSettings` | SelectionSettings | Control selection mode & checkboxes | `{ mode: 'Multiple', showCheckbox: true }` |
| `sortOrder` | string | Sort items alphabetically | `'Ascending'` |
| `allowDragAndDrop` | boolean | Enable reordering | `true` |
| `scope` | string | Link lists for drag-drop | `'#listbox2'` |
| `height` | string | Set list height | `'300px'` |
| `toolbarSettings` | ToolbarSettings | Configure move buttons | `{ items: ['moveUp', 'moveDown'] }` |

## Common Use Cases

**Single-Select Dropdown Menu**: Use single selection mode with focused item navigation
**Multi-Select Picker**: Use multiple selection with checkboxes for batch operations
**Item Reordering**: Enable drag-and-drop for user-controlled list ordering
**Categorized List**: Combine grouping with sorting for organized data display
**Data Transfer**: Use dual-list with toolbar for moving items between lists
**Searchable List**: Combine with TextBox for client-side or server-side filtering
**Accessible Form Field**: Ensure ARIA attributes and keyboard navigation enabled

---

**Next Steps**: Choose a reference guide above based on your specific need, or read through each section to build comprehensive ListBox implementation knowledge.
