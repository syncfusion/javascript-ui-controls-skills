---
name: syncfusion-javascript-spinner
description: Implement Syncfusion TypeScript Spinner control for loading indicators and asynchronous operations. Use this skill when implementing loading spinners, progress indicators, busy loaders, or waiting animations in TypeScript projects using Syncfusion Essential JS 2. Covers setup, show/hide methods, theming, target configuration, and integration with async operations.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
  platform: "TypeScript"
---

# Implementing Syncfusion TypeScript Spinner

A comprehensive skill for implementing the Syncfusion Spinner control in TypeScript projects using Essential JS 2. The Spinner displays a loading animation to indicate ongoing operations, providing user feedback during asynchronous tasks.

## When to Use This Skill

Use this skill when the user needs to:

- **Display loading spinners** during data fetching, file uploads, or async operations
- **Create busy indicators** for operations that take time to complete
- **Set up progress feedback** for better user experience
- **Configure spinner appearance** with themes, sizes, and custom labels
- **Integrate spinners** with API calls, form submissions, or background tasks
- **Show/hide spinners** based on application state changes
- **Apply themes** (Material, Bootstrap, Fabric) to spinners
- **Target specific elements** for spinner display (global or element-specific)

**Trigger Keywords:** Spinner, loading indicator, busy loader, progress indicator, loading animation, async feedback, show loading, hide spinner, Syncfusion spinner, ej2-popups spinner

## Spinner Overview

The Syncfusion Spinner is a lightweight, themeable loading indicator component that:

- **Provides visual feedback** - Users know the application is processing
- **Essential JS 2** - Part of the @syncfusion/ej2-popups package
- **TypeScript ready** - Full type support and ES6 module exports
- **Themeable** - Material, Bootstrap, Fabric, and custom themes
- **Flexible targeting** - Display globally or on specific DOM elements
- **Easy control** - Simple show/hide/create API methods
- **Framework agnostic** - Works in vanilla TypeScript or with any framework

## Documentation and Navigation Guide

### Getting Started

📄 **Read:** [references/getting-started.md](references/getting-started.md)

When you need to:
- Install @syncfusion/ej2-popups dependencies
- Import CSS themes (Material, Bootstrap, Fabric)
- Set up your first spinner in a TypeScript project
- Configure HTML containers and targets
- Create a minimal working example

### Spinner Methods & Control

📄 **Read:** [references/spinner-methods.md](references/spinner-methods.md)

When you need to:
- Understand createSpinner() configuration options
- Show spinners with showSpinner()
- Hide spinners with hideSpinner()
- Configure spinner labels and messages
- Set target elements (global or specific containers)
- Control spinner lifecycle and timing

### Theming & Styling

📄 **Read:** [references/theming-and-styling.md](references/theming-and-styling.md)

When you need to:
- Apply built-in themes (Material, Bootstrap, Fabric)
- Import theme CSS files correctly
- Customize spinner colors and sizes
- Implement custom CSS styling
- Support RTL (right-to-left) layouts
- Match spinner styling to your application design

### Custom Spinner Templates

📄 **Read:** [references/custom-templates.md](references/custom-templates.md)

When you need to:
- Create branded or custom loading indicators
- Use `setSpinner()` method for custom templates
- Design custom spinner animations and patterns
- Integrate custom spinners with Syncfusion components
- Optimize template performance

### Advanced Usage Patterns

📄 **Read:** [references/advanced-usage.md](references/advanced-usage.md)

When you need to:
- Create multiple spinners on one page
- Integrate spinners with async operations and Promises
- Handle global vs element-specific spinner scenarios
- Optimize spinner performance
- Use TypeScript interfaces and typing
- Implement error handling with spinners
- Build production-ready spinner managers

## Quick Start Example

Here's a minimal working spinner example:

```typescript
import { createSpinner, showSpinner, hideSpinner } from '@syncfusion/ej2-popups';

// 1. Create spinner on a target element
createSpinner({
  target: document.getElementById('loader-container')
});

// 2. Show spinner when operation starts
showSpinner(document.getElementById('loader-container'));

// 3. Simulate async work
setTimeout(() => {
  // Hide spinner when operation completes
  hideSpinner(document.getElementById('loader-container'));
}, 3000);
```

**HTML:**
```html
<div id="loader-container" style="height: 400px; display: flex; align-items: center; justify-content: center;"></div>
```

**CSS:**
```css
@import '@syncfusion/ej2-popups/styles/material.css';
```

## Common Patterns

### Pattern 1: Loading Data from API

```typescript
async function fetchUserData(userId: string): Promise<void> {
  const container = document.getElementById('data-container');
  
  // Show spinner before API call
  showSpinner(container);
  
  try {
    const response = await fetch(`/api/users/${userId}`);
    const data = await response.json();
    // Update UI with data
    container.innerHTML = `<p>${data.name}</p>`;
  } finally {
    // Always hide spinner, even on error
    hideSpinner(container);
  }
}
```

### Pattern 2: Form Submission with Spinner

```typescript
document.getElementById('submitBtn').addEventListener('click', async () => {
  const container = document.getElementById('spinner-target');
  
  // Show spinner during submission
  showSpinner(container);
  
  try {
    await submitForm();
    console.log('Form submitted successfully');
  } catch (error) {
    console.error('Submission failed:', error);
  } finally {
    hideSpinner(container);
  }
});
```

### Pattern 3: Multiple Operations with Spinners

```typescript
const container1 = document.getElementById('spinner-1');
const container2 = document.getElementById('spinner-2');

// Create spinners for different sections
createSpinner({ target: container1 });
createSpinner({ target: container2 });

// Show both spinners during parallel operations
showSpinner(container1);
showSpinner(container2);

Promise.all([operation1(), operation2()]).then(() => {
  hideSpinner(container1);
  hideSpinner(container2);
});
```

## Key Concepts

### createSpinner(options)

Creates a spinner element on a target container. Must be called before show/hide operations.

```typescript
createSpinner({
  target: document.getElementById('container'), // Required: target DOM element
  // Optional: add label or customize appearance
});
```

### showSpinner(target)

Makes the spinner visible. Shows animation until hideSpinner() is called.

```typescript
showSpinner(document.getElementById('container'));
```

### hideSpinner(target)

Hides the spinner and stops animation.

```typescript
hideSpinner(document.getElementById('container'));
```

### Target Configuration

Spinners can be:
- **Global** - Display center-screen covering entire page
- **Element-specific** - Display within specific container

```typescript
// Global spinner
createSpinner({ target: document.body });

// Element-specific spinner
createSpinner({ target: document.getElementById('section') });
```

## Design Decisions

### Why Spinner vs ProgressBar?

Use Spinner when:
- **Duration unknown** - Operation time is indeterminate
- **Feedback only** - Just need to show "something is happening"
- **Simple indicator** - No need to show progress percentage

Use ProgressBar when:
- **Known duration** - Can measure completion percentage
- **Detailed feedback** - Users benefit from progress tracking
- **File uploads** - Show upload percentage

### When to Show/Hide

**Show spinner:**
- Before async operations start
- API calls, file processing, data loading

**Hide spinner:**
- When operation completes (success or error)
- In finally block to guarantee cleanup
- After transition animations complete

## Common Use Cases

1. **Data Loading** - Show spinner while fetching from API
2. **Form Submission** - Display spinner during form processing
3. **File Upload** - Indicate upload in progress
4. **Page Navigation** - Show loading between route transitions
5. **Batch Operations** - Feedback during bulk actions
6. **Authentication** - Loading indicator during login/logout
7. **Report Generation** - Processing feedback for complex queries
8. **Real-time Sync** - Sync status indicator for background updates

---

**Next Steps:**
- Read [getting-started.md](references/getting-started.md) to install and set up
- Choose a theme from [theming-and-styling.md](references/theming-and-styling.md)
- Explore advanced patterns in [advanced-usage.md](references/advanced-usage.md)
