# Accessibility & Keyboard Navigation in ListBox

## Table of Contents
- [ARIA Attributes](#aria-attributes)
- [Keyboard Shortcuts](#keyboard-shortcuts)
- [WAI-ARIA Compliance](#wai-aria-compliance)
- [Screen Reader Support](#screen-reader-support)
- [Accessible Implementation](#accessible-implementation)
- [Testing for Accessibility](#testing-for-accessibility)

## ARIA Attributes

ARIA (Accessible Rich Internet Applications) attributes communicate control information to assistive technologies like screen readers.

### ListBox ARIA Roles

The ListBox component uses these ARIA roles:

| Role | Element | Purpose |
|------|---------|---------|
| `listbox` | Root container | Identifies the widget as a listbox |
| `option` | Individual item | Identifies each selectable item |

### ARIA Attributes Applied

| Attribute | Applied To | Value | Purpose |
|-----------|-----------|-------|---------|
| `role="listbox"` | Container | — | Identifies widget type |
| `aria-multiselectable` | Container | `true`/`false` | Indicates multiple selection support |
| `aria-selected` | Option | `true`/`false` | Indicates selected state |
| `aria-disabled` | Option | `true`/`false` | Indicates disabled state |
| `aria-label` | Container | User-provided | Accessible name |
| `aria-labelledby` | Container | ID reference | Associates label element |

### Automatic ARIA Management

```typescript
import { ListBox } from '@syncfusion/ej2-dropdowns';

const listObj: ListBox = new ListBox({
    dataSource: ['Item 1', 'Item 2', 'Item 3'],
    selectionSettings: {
        mode: 'Multiple'
    }
});

listObj.appendTo('#listbox');

// Component automatically sets:
// - role="listbox" on container
// - aria-multiselectable="true" (multiple mode)
// - role="option" on each item
// - aria-selected="true/false" on each item
```

### Example ARIA Output

```html
<div id="listbox" role="listbox" aria-multiselectable="true">
    <div role="option" aria-selected="true">Item 1</div>
    <div role="option" aria-selected="false">Item 2</div>
    <div role="option" aria-selected="false">Item 3</div>
</div>
```

### Adding Custom Labels

```typescript
// HTML
<label id="list-label">Select countries:</label>
<div id="listbox"></div>

// TypeScript
const listObj: ListBox = new ListBox({
    dataSource: countryData,
    fields: { text: 'Name', value: 'Code' }
    // ARIA attributes set automatically
});

listObj.appendTo('#listbox');

// Add reference to label
const container = document.getElementById('listbox');
container.setAttribute('aria-labelledby', 'list-label');
```

## Keyboard Shortcuts

### Navigation Keys

| Key | Action | Usage |
|-----|--------|-------|
| **↑ Up Arrow** | Move focus to previous item | Navigate up |
| **↓ Down Arrow** | Move focus to next item | Navigate down |
| **Home** | Move focus to first item | Jump to start |
| **End** | Move focus to last item | Jump to end |

### Selection Keys

| Key | Action | Usage |
|-----|--------|-------|
| **Space** | Toggle focused item selection | Toggle item in checkbox mode |
| **Ctrl + A** | Select all items | Select entire list |
| **Ctrl + Shift + Home** | Select from current to first | Extended selection up |
| **Ctrl + Shift + End** | Select from current to last | Extended selection down |
| **Ctrl + ↑ / Ctrl + ↓** | Select/deselect while navigating | Multi-select mode |

### Full Keyboard Navigation Example

```typescript
import { ListBox } from '@syncfusion/ej2-dropdowns';

const listObj: ListBox = new ListBox({
    dataSource: [
        'Item 1',
        'Item 2',
        'Item 3',
        'Item 4',
        'Item 5'
    ],
    selectionSettings: {
        mode: 'Multiple',
        showCheckbox: true
    }
});

listObj.appendTo('#listbox');

// User workflow:
// 1. Tab to focus ListBox
// 2. Down arrow to navigate to Item 3
// 3. Space to toggle checkbox
// 4. Shift+Down to extend selection to Item 5
// 5. Ctrl+A to select all
```

## WAI-ARIA Compliance

### WCAG 2.1 Compliance Levels

The ListBox achieves WCAG 2.1 Level AA compliance:

**Level A**: Minimum accessibility
**Level AA**: Enhanced accessibility (recommended)
**Level AAA**: Advanced accessibility

### WAI-ARIA Requirements Met

✅ **Perceivable**: Information presented in multiple ways
- Visual selection highlighting
- Checkbox visual feedback
- Group headers distinguishable

✅ **Operable**: Full keyboard navigation
- All functions accessible via keyboard
- No keyboard traps
- Consistent navigation patterns

✅ **Understandable**: Clear and predictable
- Consistent behavior across states
- Predictable keyboard shortcuts
- Clear error prevention

✅ **Robust**: Compatible with assistive technologies
- Valid semantic HTML
- Proper ARIA roles and attributes
- Screen reader compatible

### Implementation for Compliance

```typescript
import { ListBox, CheckBoxSelection } from '@syncfusion/ej2-dropdowns';

ListBox.Inject(CheckBoxSelection);

const listObj: ListBox = new ListBox({
    dataSource: accessibleData,
    fields: { text: 'name', value: 'id' },
    selectionSettings: {
        mode: 'Multiple',
        showCheckbox: true,
        showSelectAll: true  // Improves accessibility
    }
    // WCAG compliance enabled by default
});

listObj.appendTo('#listbox');

// Add visual label
const label = document.createElement('label');
label.id = 'listbox-label';
label.textContent = 'Select items:';
document.getElementById('listbox').parentElement.insertBefore(
    label,
    document.getElementById('listbox')
);

const container = document.getElementById('listbox');
container.setAttribute('aria-labelledby', 'listbox-label');
```

## Screen Reader Support

### How Screen Readers Interact

1. **Announcement**: Listbox announced with role and label
   - "Listbox, Select items"

2. **Navigation**: Items announced with selection state
   - "Item 1, selected, 1 of 5"
   - "Item 2, not selected, 2 of 5"

3. **Interaction**: Changes announced in real-time
   - User presses Space: "Item 2 selected"
   - User presses Ctrl+A: "All 5 items selected"

### Testing with Screen Readers

**NVDA (Free, Windows)**:
1. Download and install NVDA
2. Start NVDA
3. Navigate using arrow keys
4. Listen for announcements

**JAWS (Commercial, Windows)**:
1. Start JAWS
2. Press Insert+Down for contextual help
3. Navigate list with arrow keys

**VoiceOver (Mac/iOS)**:
1. Press Cmd+F5 to enable
2. Use VO+Arrow keys to navigate
3. Press VO+Space to interact

### Example Screen Reader Output

```
NVDA announces:
"Listbox, select multiple items"
"Item 1, selected"
Down arrow pressed
"Item 2, not selected"
Space pressed
"Item 2, selected"
```

## Accessible Implementation

### Complete Accessible ListBox

```typescript
import { ListBox, CheckBoxSelection } from '@syncfusion/ej2-dropdowns';

ListBox.Inject(CheckBoxSelection);

// HTML with semantic markup
const html = `
    <div class="listbox-wrapper">
        <label for="country-list" class="listbox-label">
            Select countries (multiple selection available)
        </label>
        <div id="country-list"></div>
    </div>
`;

// Data
const countryData = [
    { countryId: 'usa', countryName: 'United States' },
    { countryId: 'uk', countryName: 'United Kingdom' },
    { countryId: 'canada', countryName: 'Canada' },
    { countryId: 'australia', countryName: 'Australia' }
];

// Fully accessible ListBox
const listObj: ListBox = new ListBox({
    dataSource: countryData,
    fields: { text: 'countryName', value: 'countryId' },
    selectionSettings: {
        mode: 'Multiple',
        showCheckbox: true,
        showSelectAll: true
    }
});

listObj.appendTo('#country-list');

// Keyboard event handling
const container = document.getElementById('country-list');
container.addEventListener('keydown', (e: KeyboardEvent) => {
    if (e.key === 'Enter') {
        const selected = listObj.getSelectedItems();
        console.log('Selection confirmed:', selected.value);
    }
});

// Screen reader support
container.setAttribute('aria-label', 'Select countries');
container.setAttribute('aria-describedby', 'instructions');

// Instructions for users
const instructions = document.createElement('div');
instructions.id = 'instructions';
instructions.className = 'sr-only';
instructions.textContent = 
    'Use arrow keys to navigate. Space to select. Ctrl+A to select all.';
container.parentElement.appendChild(instructions);
```

### CSS for Screen Readers

```css
/* Hide from sighted users, visible to screen readers */
.sr-only {
    position: absolute;
    width: 1px;
    height: 1px;
    padding: 0;
    margin: -1px;
    overflow: hidden;
    clip: rect(0, 0, 0, 0);
    white-space: nowrap;
    border: 0;
}

/* Visible focus indicator for keyboard navigation */
.e-listbox .e-list-item:focus {
    outline: 2px solid #4CAF50;
    outline-offset: 2px;
}

/* High contrast mode support */
@media (prefers-contrast: more) {
    .e-listbox .e-list-item {
        border: 1px solid currentColor;
    }
    
    .e-listbox .e-list-item.e-selected {
        background-color: #000080;
        color: #FFFF00;
    }
}

/* Reduced motion support */
@media (prefers-reduced-motion: reduce) {
    .e-listbox .e-list-item {
        transition: none;
    }
}
```

## Testing for Accessibility

### Keyboard Testing Checklist

- [ ] Tab into ListBox
- [ ] Arrow keys navigate items
- [ ] Home/End move to first/last
- [ ] Space toggles selection
- [ ] Ctrl+A selects all
- [ ] Shift+Arrow extends selection
- [ ] No keyboard traps
- [ ] Focus indicator visible

### Screen Reader Testing Checklist

- [ ] ListBox announced with role
- [ ] Items announced with selection state
- [ ] Selection changes announced
- [ ] Labels associated correctly
- [ ] Instructions accessible

### Automated Testing

```typescript
// Example: Testing with axe-core
import { axe } from 'axe-core';

async function testAccessibility() {
    const listElement = document.getElementById('listbox');
    
    const results = await axe.run(listElement);
    
    if (results.violations.length === 0) {
        console.log('✅ No accessibility violations');
    } else {
        console.error('❌ Violations found:', results.violations);
    }
}

testAccessibility();
```

### Browser DevTools Testing

1. Open DevTools (F12)
2. Go to Lighthouse tab
3. Run accessibility audit
4. Review report for issues

## Common Accessibility Issues

### Issue: Focus not visible

**Cause**: CSS removes outline.

**Solution**:
```css
.e-listbox .e-list-item:focus {
    outline: 2px solid #0078d4;
}
```

### Issue: Color-only selection indication

**Cause**: Only color shows selection.

**Solution**: Add pattern or icon:
```css
.e-listbox .e-list-item.e-selected::before {
    content: '✓ ';
    color: #0078d4;
}
```

### Issue: Keyboard shortcuts not working

**Cause**: Keyboard event not reaching ListBox.

**Solution**: Ensure ListBox has focus:
```typescript
listObj.focusIn();
```

### Issue: Screen reader announcements missing

**Cause**: ARIA attributes not set.

**Solution**: Component sets them automatically; verify:
```javascript
const container = document.getElementById('listbox');
console.log(container.getAttribute('role'));  // Should be 'listbox'
```

## Next Steps

- **Selection**: See [selection-and-checkboxes.md](selection-and-checkboxes.md) for accessible selection patterns
- **Keyboard**: See [getting-started.md](getting-started.md) for basic setup
- **Data Binding**: See [data-binding.md](data-binding.md) for accessible remote data loading
