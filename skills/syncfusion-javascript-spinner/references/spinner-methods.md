# Spinner Methods & Control

## Table of Contents
- [Overview](#overview)
- [createSpinner Method](#createspinner-method)
- [showSpinner Method](#showspinner-method)
- [hideSpinner Method](#hidespinner-method)
- [Target Configuration](#target-configuration)
- [Common Patterns](#common-patterns)
- [Method Timing](#method-timing)

---

## Overview

The Spinner component provides three main methods for lifecycle management:

| Method | Purpose | Call Order |
|--------|---------|-----------|
| `createSpinner()` | Initialize spinner on target | 1st (required) |
| `showSpinner()` | Display and animate spinner | 2nd (anytime) |
| `hideSpinner()` | Hide and stop animation | 3rd (anytime) |

**Key Rule:** Always call `createSpinner()` before `showSpinner()` or `hideSpinner()`.

---

## createSpinner Method

Creates a spinner element on the specified target DOM element.

### Signature

```typescript
createSpinner(options: ISpinnerConfig): void
```

### Parameters

```typescript
interface ISpinnerConfig {
  target: HTMLElement;  // Required: DOM element where spinner will be created
  label?: string;       // Optional: Display text below spinner
  type?: string;        // Optional: 'Material', 'Bootstrap', etc. (theme-specific)
  size?: string;        // Optional: 'small', 'medium', 'large'
}
```

### Required Parameter: target

The target element where the spinner will be rendered.

```typescript
// Get target element
const container = document.getElementById('spinner-container');

// Create spinner on target
createSpinner({
  target: container
});
```

### Optional Parameter: label

Add text displayed below the spinner animation.

```typescript
createSpinner({
  target: document.getElementById('container'),
  label: 'Loading data...'
});
```

### Optional Parameter: type

Specify spinner type (varies by theme).

```typescript
createSpinner({
  target: document.getElementById('container'),
  type: 'Material'
});
```

### Optional Parameter: size

Control spinner size.

```typescript
createSpinner({
  target: document.getElementById('container'),
  size: 'large'  // 'small', 'medium', 'large'
});
```

### Complete Example

```typescript
import { createSpinner } from '@syncfusion/ej2-popups';

const container = document.getElementById('loader');

// Create spinner with all options
createSpinner({
  target: container,
  label: 'Fetching user data...',
  type: 'Material',
  size: 'medium'
});
```

### When to Call createSpinner()

- **Initialization:** Call once when the application loads or component mounts
- **Setup phase:** Not every time you show/hide
- **Before first show:** Must precede first `showSpinner()` call

```typescript
// CORRECT: Create once during setup
createSpinner({ target: container });

// Then show/hide multiple times
showSpinner(container);
// ... user does something
hideSpinner(container);

// Show again - no need to recreate
showSpinner(container);
```

---

## showSpinner Method

Displays the spinner and starts the loading animation.

### Signature

```typescript
showSpinner(target: HTMLElement): void
```

### Parameters

```typescript
target  // HTMLElement: The same element passed to createSpinner()
```

### Basic Usage

```typescript
import { showSpinner } from '@syncfusion/ej2-popups';

const container = document.getElementById('spinner-container');
showSpinner(container);
```

### With Async Operation

```typescript
async function fetchData() {
  const container = document.getElementById('loader');
  
  // Show spinner before API call
  showSpinner(container);
  
  try {
    const response = await fetch('/api/data');
    const data = await response.json();
    return data;
  } finally {
    // Hide spinner when done (success or error)
    hideSpinner(container);
  }
}
```

### Multiple Show/Hide Cycles

```typescript
const container = document.getElementById('spinner');

// First operation
showSpinner(container);
setTimeout(() => hideSpinner(container), 2000);

// Second operation - same container
setTimeout(() => {
  showSpinner(container);
  setTimeout(() => hideSpinner(container), 1500);
}, 3000);
```

### Show Spinner on Button Click

```typescript
document.getElementById('loadBtn').addEventListener('click', () => {
  const container = document.getElementById('spinner-container');
  showSpinner(container);
  
  // Simulate async work
  setTimeout(() => {
    hideSpinner(container);
  }, 3000);
});
```

---

## hideSpinner Method

Hides the spinner and stops the loading animation.

### Signature

```typescript
hideSpinner(target: HTMLElement): void
```

### Parameters

```typescript
target  // HTMLElement: The same element passed to createSpinner()
```

### Basic Usage

```typescript
import { hideSpinner } from '@syncfusion/ej2-popups';

const container = document.getElementById('spinner-container');
hideSpinner(container);
```

### Always Hide in Finally Block

**Best Practice:** Hide spinner in `finally` block to ensure it's hidden even if errors occur.

```typescript
async function saveData(data: unknown) {
  const container = document.getElementById('loader');
  
  showSpinner(container);
  
  try {
    const response = await fetch('/api/save', {
      method: 'POST',
      body: JSON.stringify(data)
    });
    
    if (!response.ok) {
      throw new Error('Save failed');
    }
    
    console.log('Data saved successfully');
  } catch (error) {
    console.error('Error:', error);
    // Still need to hide spinner on error
  } finally {
    // Always hide spinner
    hideSpinner(container);
  }
}
```

### Hide After Delay

```typescript
function hideSpinnerAfterDelay(target: HTMLElement, delayMs: number) {
  setTimeout(() => {
    hideSpinner(target);
  }, delayMs);
}

// Usage
showSpinner(container);
hideSpinnerAfterDelay(container, 2000); // Hide after 2 seconds
```

---

## Target Configuration

### Global Spinner

Display spinner centered on entire page, covering all content.

```typescript
import { createSpinner, showSpinner } from '@syncfusion/ej2-popups';

// Create global spinner
createSpinner({
  target: document.body,
  label: 'Processing...'
});

// Show globally
showSpinner(document.body);
```

### Element-Specific Spinner

Display spinner within a specific container element.

```typescript
// Spinner appears inside the div, not full page
createSpinner({
  target: document.getElementById('data-section')
});

showSpinner(document.getElementById('data-section'));
```

### Multiple Spinners

Create independent spinners on different elements.

```typescript
const container1 = document.getElementById('section-1');
const container2 = document.getElementById('section-2');

// Create separate spinners
createSpinner({ target: container1 });
createSpinner({ target: container2 });

// Control independently
showSpinner(container1);
showSpinner(container2);

// Can hide one without affecting the other
hideSpinner(container1);
// container2 spinner still visible
```

### Nested Containers

Spinners work in nested elements. Target the specific container.

```typescript
// HTML structure:
// <div id="outer">
//   <div id="inner">...</div>
// </div>

const inner = document.getElementById('inner');
const outer = document.getElementById('outer');

// Spinner on inner element
createSpinner({ target: inner });
showSpinner(inner);
// Spinner displays inside inner div

// Spinner on outer element
createSpinner({ target: outer });
showSpinner(outer);
// Spinner displays inside outer div
```

---

## Common Patterns

### Pattern 1: API Data Loading

```typescript
async function loadUserProfile(userId: string) {
  const container = document.getElementById('profile-loader');
  
  createSpinner({
    target: container,
    label: 'Loading profile...'
  });
  
  try {
    showSpinner(container);
    
    const response = await fetch(`/api/users/${userId}`);
    if (!response.ok) throw new Error('Failed to load profile');
    
    const profile = await response.json();
    displayProfile(profile);
    
  } catch (error) {
    console.error('Error:', error);
    displayError('Could not load profile');
  } finally {
    hideSpinner(container);
  }
}
```

### Pattern 2: Form Submission

```typescript
document.getElementById('submitForm').addEventListener('submit', async (e) => {
  e.preventDefault();
  
  const container = document.getElementById('submit-spinner');
  const formData = new FormData(e.target as HTMLFormElement);
  
  createSpinner({
    target: container,
    label: 'Saving...'
  });
  
  try {
    showSpinner(container);
    
    const response = await fetch('/api/submit', {
      method: 'POST',
      body: formData
    });
    
    if (!response.ok) throw new Error('Submission failed');
    
    alert('Submitted successfully!');
    (e.target as HTMLFormElement).reset();
    
  } catch (error) {
    alert(`Error: ${(error as Error).message}`);
  } finally {
    hideSpinner(container);
  }
});
```

### Pattern 3: Parallel Operations

```typescript
async function loadMultipleSections() {
  const containers = {
    users: document.getElementById('users-loader'),
    posts: document.getElementById('posts-loader'),
    comments: document.getElementById('comments-loader')
  };
  
  // Create spinners for each section
  Object.values(containers).forEach(container => {
    createSpinner({ target: container });
  });
  
  // Show all spinners
  Object.values(containers).forEach(container => {
    showSpinner(container);
  });
  
  try {
    // Load data in parallel
    const [users, posts, comments] = await Promise.all([
      fetch('/api/users').then(r => r.json()),
      fetch('/api/posts').then(r => r.json()),
      fetch('/api/comments').then(r => r.json())
    ]);
    
    // Update UI with data
    displayUsers(users);
    displayPosts(posts);
    displayComments(comments);
    
  } finally {
    // Hide all spinners
    Object.values(containers).forEach(container => {
      hideSpinner(container);
    });
  }
}
```

### Pattern 4: Conditional Spinner

```typescript
function showConditionalSpinner(shouldShow: boolean) {
  const container = document.getElementById('optional-spinner');
  
  if (shouldShow) {
    createSpinner({ target: container });
    showSpinner(container);
  } else {
    hideSpinner(container);
  }
}

// Usage based on data fetch speed
const startTime = Date.now();
showConditionalSpinner(true);

const data = await fetchData();

const elapsedTime = Date.now() - startTime;
if (elapsedTime < 500) {
  // Fast operation, hide spinner before user notices
  showConditionalSpinner(false);
} else {
  // Operation took time, spinner was useful
  showConditionalSpinner(false);
}
```

---

## Method Timing

### Initialization Timeline

```
Application Start
     ↓
createSpinner()  ← Call once per container
     ↓
showSpinner()    ← User interaction or event
     ↓
(async operation happens)
     ↓
hideSpinner()    ← Operation completes
```

### Example: Complete Lifecycle

```typescript
import { createSpinner, showSpinner, hideSpinner } from '@syncfusion/ej2-popups';

// 1. INITIALIZATION: App starts
function initializeApp() {
  const container = document.getElementById('spinner');
  
  // Create spinner once at app startup
  createSpinner({
    target: container,
    label: 'Ready'
  });
}

// Call during app initialization
window.addEventListener('DOMContentLoaded', initializeApp);

// 2. USAGE: User triggers action
document.getElementById('actionBtn').addEventListener('click', async () => {
  const container = document.getElementById('spinner');
  
  // Show spinner when user clicks
  showSpinner(container);
  
  try {
    // Do async work
    const result = await performAction();
    console.log('Done:', result);
  } finally {
    // Hide spinner when complete
    hideSpinner(container);
  }
});

// 3. Can repeat show/hide cycles on same container
// Multiple clicks = multiple show/hide cycles
// No need to recreate spinner
```

---

## Troubleshooting

### Spinner Not Showing?

**Issue:** `showSpinner()` called but spinner not visible.

**Solutions:**
1. Verify `createSpinner()` was called first:
   ```typescript
   createSpinner({ target: container }); // Required
   showSpinner(container); // Then show
   ```

2. Check target element exists:
   ```typescript
   const target = document.getElementById('spinner-container');
   if (!target) console.error('Target not found');
   ```

3. Verify CSS theme is imported (see getting-started.md)

### Spinner Persists After hideSpinner()?

**Solution:** Ensure `hideSpinner()` is called:

```typescript
// WRONG: Spinner stays visible
showSpinner(container);

// RIGHT: Always hide
showSpinner(container);
hideSpinner(container);
```

### Multiple Spinners Interfering?

**Solution:** Always use the correct target element:

```typescript
// WRONG: Different containers
createSpinner({ target: container1 });
showSpinner(container2); // Wrong target!

// RIGHT: Same container
createSpinner({ target: container1 });
showSpinner(container1); // Correct target
```

---

## Next Steps

- Explore [theming-and-styling.md](theming-and-styling.md) for visual customization
- See [advanced-usage.md](advanced-usage.md) for integration patterns
