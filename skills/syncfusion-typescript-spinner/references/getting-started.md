# Getting Started with Spinner

## Table of Contents
- [Dependencies](#dependencies)
- [Installation](#installation)
- [Set Up Development Environment](#set-up-development-environment)
- [Add Syncfusion Packages](#add-syncfusion-packages)
- [Import CSS Themes](#import-css-themes)
- [Create Your First Spinner](#create-your-first-spinner)
- [Basic HTML Setup](#basic-html-setup)

---

## Dependencies

The Spinner control requires the following Syncfusion packages:

```
@syncfusion/ej2-popups
├── @syncfusion/ej2-base
```

These packages provide:
- **ej2-popups** - Spinner component and methods
- **ej2-base** - Core utilities and dependencies

---

## Installation

### Using NPM

Install the required packages:

```bash
npm install @syncfusion/ej2-popups --save
```

This automatically installs @syncfusion/ej2-base as a dependency.

### Using Yarn

```bash
yarn add @syncfusion/ej2-popups
```

---

## Set Up Development Environment

### Option 1: Webpack Quickstart (Recommended)

Clone the Syncfusion quickstart repository:

```bash
git clone https://github.com/SyncfusionExamples/ej2-quickstart-webpack ej2-quickstart
cd ej2-quickstart
npm install
```

This provides a pre-configured TypeScript + Webpack environment for Syncfusion components.

### Option 2: Existing TypeScript Project

If you already have a TypeScript project with bundler (Webpack, Vite, Rollup), you can skip the quickstart and directly install packages.

**Requirements:**
- Node v14.15.0 or higher
- npm or yarn
- TypeScript configured

---

## Add Syncfusion Packages

### Check package.json

Verify the Syncfusion packages are installed:

```json
{
  "dependencies": {
    "@syncfusion/ej2-popups": "latest",
    "@syncfusion/ej2-base": "latest"
  }
}
```

### Install Dependencies

```bash
npm install
```

---

## Import CSS Themes

Syncfusion provides built-in themes. Choose one and import it in your project.

### Available Themes

- **Material** (default, modern)
- **Bootstrap** (bootstrap 4 style)
- **Fabric** (Microsoft Fabric design)
- **Bootstrap 5** (bootstrap 5 style)
- **Tailwind CSS** (tailwind style)

### Import Theme

Add the theme CSS to your main CSS file (`src/styles/styles.css` or similar):

```css
/* Material Theme */
@import '@syncfusion/ej2-popups/styles/material.css';

/* OR use Bootstrap Theme */
@import '@syncfusion/ej2-popups/styles/bootstrap.css';

/* OR use Fabric Theme */
@import '@syncfusion/ej2-popups/styles/fabric.css';
```

**Import early:** Add theme import at the top of your main CSS file before other styles.

---

## Create Your First Spinner

### Step 1: Import Methods

```typescript
import { createSpinner, showSpinner, hideSpinner } from '@syncfusion/ej2-popups';
```

### Step 2: Create Spinner

```typescript
// Create spinner on a target element
createSpinner({
  target: document.getElementById('spinner-container')
});
```

### Step 3: Show the Spinner

```typescript
// Display the spinner
showSpinner(document.getElementById('spinner-container'));
```

### Step 4: Hide the Spinner

```typescript
// Hide the spinner when operation completes
hideSpinner(document.getElementById('spinner-container'));
```

### Complete Example

```typescript
import { createSpinner, showSpinner, hideSpinner } from '@syncfusion/ej2-popups';

// Initialize
const container = document.getElementById('spinner-container');

createSpinner({
  target: container
});

// Simulate async operation
function performAsyncTask() {
  showSpinner(container);
  
  setTimeout(() => {
    hideSpinner(container);
    console.log('Task completed');
  }, 3000); // Hide after 3 seconds
}

// Trigger on button click
document.getElementById('startBtn')?.addEventListener('click', performAsyncTask);
```

---

## Basic HTML Setup

### Container Element

Create a container element where the spinner will be displayed:

```html
<div id="spinner-container" style="height: 400px; display: flex; align-items: center; justify-content: center; border: 1px solid #ddd;">
  <!-- Spinner will appear here -->
</div>
```

### Full Page Example

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Syncfusion Spinner</title>
  
  <!-- Theme CSS -->
  <link rel="stylesheet" href="src/styles/styles.css" />
</head>
<body>
  <div class="container">
    <h1>Spinner Example</h1>
    
    <!-- Spinner container -->
    <div id="spinner-container" style="height: 400px; border: 1px solid #e0e0e0; border-radius: 4px; display: flex; align-items: center; justify-content: center;">
    </div>
    
    <!-- Button to trigger spinner -->
    <button id="startBtn" style="margin-top: 20px; padding: 10px 20px; font-size: 16px;">
      Start Loading
    </button>
  </div>
  
  <!-- Application script -->
  <script src="src/app.ts"></script>
</body>
</html>
```

### TypeScript Component (src/app.ts)

```typescript
import { createSpinner, showSpinner, hideSpinner } from '@syncfusion/ej2-popups';

const container = document.getElementById('spinner-container');
const startBtn = document.getElementById('startBtn');

// Initialize spinner
createSpinner({
  target: container
});

// Handle button click
startBtn?.addEventListener('click', () => {
  showSpinner(container);
  
  // Simulate async operation (e.g., API call)
  setTimeout(() => {
    hideSpinner(container);
    alert('Operation completed!');
  }, 2000);
});
```

### CSS File (src/styles/styles.css)

```css
/* Import Syncfusion theme */
@import '@syncfusion/ej2-popups/styles/material.css';

/* Basic styling */
body {
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 20px;
  background-color: #f5f5f5;
}

.container {
  max-width: 600px;
  margin: 0 auto;
  background: white;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

h1 {
  color: #333;
  margin-top: 0;
}

button {
  background-color: #0078d4;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.2s;
}

button:hover {
  background-color: #106ebe;
}

button:active {
  background-color: #005a9e;
}
```

---

## Build and Run

### Development Server

```bash
npm start
```

Your application will be available at `http://localhost:3000` (or the configured port).

### Production Build

```bash
npm run build
```

This creates optimized files in the `dist/` folder.

---

## Troubleshooting

### Spinner Not Appearing?

**Issue:** Spinner element is created but not visible.

**Solutions:**
1. Verify CSS theme is imported:
   ```css
   @import '@syncfusion/ej2-popups/styles/material.css';
   ```

2. Check target element is visible:
   ```typescript
   const target = document.getElementById('spinner-container');
   if (!target) {
     console.error('Target element not found');
   }
   ```

3. Ensure createSpinner() is called before showSpinner():
   ```typescript
   createSpinner({ target: container });
   showSpinner(container); // Only after create
   ```

### "Cannot find module" Error?

**Solution:** Verify @syncfusion/ej2-popups is installed:

```bash
npm list @syncfusion/ej2-popups
```

If not found, install it:

```bash
npm install @syncfusion/ej2-popups --save
```

### Spinner Appears in Wrong Position?

**Issue:** Spinner not centered or positioned correctly.

**Solution:** Adjust container styling:

```html
<div id="spinner-container" 
     style="display: flex; align-items: center; justify-content: center; height: 300px; background: #f9f9f9;">
</div>
```

---

## Next Steps

- Read [spinner-methods.md](spinner-methods.md) to learn about spinner control methods
- Explore [theming-and-styling.md](theming-and-styling.md) for customization options
- Check [advanced-usage.md](advanced-usage.md) for integration patterns
