---
name: styling-appearance
description: 'Styling and appearance in Syncfusion Grid: themes, CSS, row/cell styling, dark mode, size modes.'
---

# Styling and Appearance

## Table of Contents
- [Overview](#overview)
- [Themes](#themes)
- [CSS Customization](#css-customization)
- [Dark Mode](#dark-mode)
- [Size Modes](#size-modes)

## When to Use This Reference

- Apply custom CSS classes and themes to grid
- Configure row and cell styling based on data
- Customize header and footer appearance
- Implement conditional formatting and colors
- Create custom grid templates and layouts

## Overview

The grid provides comprehensive theming and customization options to match your application's design.

## Themes

### Available Themes

```ts
// Material 3 (Default)
import '@syncfusion/ej2-grids/styles/material3.css';

// Bootstrap
import '@syncfusion/ej2-grids/styles/bootstrap.css';

// Bootstrap 5
import '@syncfusion/ej2-grids/styles/bootstrap5.css';

// Fabric
import '@syncfusion/ej2-grids/styles/fabric.css';

// Tailwind CSS
import '@syncfusion/ej2-grids/styles/tailwind.css';
```

### Switch Themes Dynamically

```ts
import { Grid } from '@syncfusion/ej2-grids';

let gridInstance: Grid;

const switchTheme = (themeName: string) => {
  // Remove current theme
  const link = document.querySelector('link[data-theme]') as HTMLLinkElement;
  if (link) link.remove();

  // Add new theme
  const newLink = document.createElement('link');
  newLink.rel = 'stylesheet';
  newLink.href = `@syncfusion/ej2-grids/styles/${themeName}.css`;
  newLink.setAttribute('data-theme', themeName);
  document.head.appendChild(newLink);
};

document.getElementById('themeSelect')?.addEventListener('change', (e: any) => {
  switchTheme(e.target.value);
});
```

## CSS Customization

### Custom Row Styling

```ts
import { Grid } from '@syncfusion/ej2-grids';

const grid = new Grid({
  dataSource: data,
  columns: [
    { field: 'OrderID', headerText: 'Order ID', width: 100 },
    { field: 'Freight', headerText: 'Freight', width: 120 }
  ],
  rowDataBound: (args: any) => {
    const className = args.data.Freight > 50 ? 'high-value' : 'normal-value';
    args.row?.classList.add(className);
  }
});

grid.appendTo('#grid');

// CSS
const styles = document.createElement('style');
styles.textContent = `
  .high-value { background-color: #ffe6e6; font-weight: bold; }
  .normal-value { background-color: #ffffff; }
`;
document.head.appendChild(styles);
```

### Alternating Row Colors

```ts
const gridStyle = `
  .e-grid .e-table tbody tr:nth-child(odd) { background-color: #f9f9f9; }
  .e-grid .e-table tbody tr:nth-child(even) { background-color: #ffffff; }
  .e-grid .e-table tbody tr:hover { background-color: #e3f2fd; }
`;

const styles = document.createElement('style');
styles.textContent = gridStyle;
document.head.appendChild(styles);
```

### Column Cell Styling

```ts
const grid = new Grid({
  dataSource: data,
  columns: [
    { field: 'OrderID', headerText: 'Order ID', width: 100 },
    { field: 'Freight', headerText: 'Freight', width: 120 }
  ],
  queryCellInfo: (args: any) => {
    if (args.column.field === 'Freight' && args.data.Freight > 50) {
      args.cell.style.backgroundColor = '#ffcccc';
      args.cell.style.color = '#cc0000';
      args.cell.style.fontWeight = 'bold';
    }
  }
});

grid.appendTo('#grid');
```

---

## Dark Mode

### Enable Dark Theme

```ts
// Import dark theme CSS
import '@syncfusion/ej2-grids/styles/material3-dark.css';
// Or import '@syncfusion/ej2-grids/styles/material-dark.css';

const grid = new Grid({
  dataSource: data,
  columns: [
    { field: 'OrderID', headerText: 'Order ID', width: 100 },
    { field: 'CustomerID', headerText: 'Customer', width: 120 }
  ]
});

grid.appendTo('#grid');
```

### Toggle Dark Mode

```ts
import { Grid } from '@syncfusion/ej2-grids';

let isDarkMode = false;

const toggleDarkMode = () => {
  const htmlElement = document.documentElement;
  isDarkMode = !isDarkMode;

  if (isDarkMode) {
    htmlElement.setAttribute('data-theme', 'dark');
  } else {
    htmlElement.removeAttribute('data-theme');
  }
};

document.getElementById('darkModeToggle')?.addEventListener('click', toggleDarkMode);
```

---

## Size Modes

### Compact Mode

```ts
import { Grid } from '@syncfusion/ej2-grids';

const grid = new Grid({
  dataSource: data,
  columns: [
    { field: 'OrderID', headerText: 'Order ID', width: 80 },
    { field: 'CustomerID', headerText: 'Customer', width: 100 }
  ],
  rowHeight: 32
});

grid.appendTo('#grid');
```

### Spacious Mode

```ts
import { Grid } from '@syncfusion/ej2-grids';

const grid = new Grid({
  dataSource: data,
  columns: [
    { field: 'OrderID', headerText: 'Order ID', width: 120 },
    { field: 'CustomerID', headerText: 'Customer', width: 150 }
  ],
  rowHeight: 48
});

const styles = document.createElement('style');
styles.textContent = '.e-grid { font-size: 16px; }';
document.head.appendChild(styles);

grid.appendTo('#grid');
```

### Custom Font and Colors

```ts
import { Grid } from '@syncfusion/ej2-grids';

const customStyle = `
  .e-grid {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    font-size: 14px;
    color: #333;
  }
  .e-grid .e-th {
    background-color: #2c3e50;
    color: #ffffff;
    font-weight: 600;
  }
  .e-grid .e-td { border-bottom: 1px solid #ecf0f1; }
  .e-grid .e-altrow { background-color: #f8f9fa; }
`;

const styles = document.createElement('style');
styles.textContent = customStyle;
document.head.appendChild(styles);

const grid = new Grid({
  dataSource: data,
  columns: [
    { field: 'OrderID', headerText: 'Order ID', width: 100 },
    { field: 'CustomerID', headerText: 'Customer', width: 150 }
  ]
});

grid.appendTo('#grid');
```

