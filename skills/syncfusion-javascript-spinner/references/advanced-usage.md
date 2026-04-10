# Advanced Usage Patterns

## Table of Contents
- [Async Operation Integration](#async-operation-integration)
- [Multiple Spinners](#multiple-spinners)
- [Error Handling with Spinners](#error-handling-with-spinners)
- [Global vs Element-Specific](#global-vs-element-specific)
- [Performance Optimization](#performance-optimization)
- [TypeScript Typing](#typescript-typing)
- [Custom Spinner Manager](#custom-spinner-manager)

---

## Async Operation Integration

### Promise-Based Operations

Integrate spinner with Promise chains:

```typescript
import { createSpinner, showSpinner, hideSpinner } from '@syncfusion/ej2-popups';

function fetchDataWithSpinner(url: string): Promise<unknown> {
  const container = document.getElementById('loader');
  
  createSpinner({
    target: container,
    label: 'Fetching data...'
  });
  
  return fetch(url)
    .then(response => {
      if (!response.ok) throw new Error(`HTTP ${response.status}`);
      return response.json();
    })
    .then(data => {
      console.log('Data received:', data);
      return data;
    })
    .catch(error => {
      console.error('Error:', error);
      throw error;
    })
    .finally(() => {
      hideSpinner(container);
    });
}

// Usage
fetchDataWithSpinner('/api/users')
  .then(data => console.log('Success:', data))
  .catch(error => console.error('Failed:', error));
```

### Async/Await Pattern

Modern async/await with spinner control:

```typescript
async function loadDataAsync() {
  const container = document.getElementById('spinner');
  
  showSpinner(container);
  
  try {
    // Step 1: Fetch users
    const usersResponse = await fetch('/api/users');
    if (!usersResponse.ok) throw new Error('Failed to fetch users');
    const users = await usersResponse.json();
    
    // Step 2: Fetch posts
    const postsResponse = await fetch('/api/posts');
    if (!postsResponse.ok) throw new Error('Failed to fetch posts');
    const posts = await postsResponse.json();
    
    // Step 3: Process data
    const combined = { users, posts };
    return combined;
    
  } catch (error) {
    console.error('Error in async operation:', error);
    throw error;
  } finally {
    hideSpinner(container);
  }
}

// Usage
try {
  const result = await loadDataAsync();
  console.log('Data loaded:', result);
} catch (error) {
  alert('Failed to load data. Please try again.');
}
```

### Parallel Operations

Load multiple resources simultaneously:

```typescript
async function loadMultipleResources() {
  const containers = {
    main: document.getElementById('main-spinner'),
    sidebar: document.getElementById('sidebar-spinner'),
    footer: document.getElementById('footer-spinner')
  };
  
  // Create spinners
  Object.entries(containers).forEach(([key, container]) => {
    createSpinner({
      target: container,
      label: `Loading ${key}...`
    });
    showSpinner(container);
  });
  
  try {
    // Fetch all data in parallel
    const [mainData, sidebarData, footerData] = await Promise.all([
      fetch('/api/main').then(r => r.json()),
      fetch('/api/sidebar').then(r => r.json()),
      fetch('/api/footer').then(r => r.json())
    ]);
    
    // Update UI
    updateMainContent(mainData);
    updateSidebar(sidebarData);
    updateFooter(footerData);
    
    return { mainData, sidebarData, footerData };
    
  } finally {
    // Hide all spinners
    Object.values(containers).forEach(container => {
      hideSpinner(container);
    });
  }
}
```

### Timeout with Spinner

Show spinner with maximum timeout:

```typescript
async function loadWithTimeout(url: string, timeoutMs: number = 30000) {
  const container = document.getElementById('spinner');
  
  showSpinner(container);
  
  const timeoutPromise = new Promise((_, reject) =>
    setTimeout(() => reject(new Error('Request timeout')), timeoutMs)
  );
  
  try {
    const dataPromise = fetch(url).then(r => r.json());
    const data = await Promise.race([dataPromise, timeoutPromise]);
    return data;
    
  } catch (error) {
    if ((error as Error).message === 'Request timeout') {
      alert('Request took too long. Please try again.');
    } else {
      console.error('Error:', error);
    }
    throw error;
    
  } finally {
    hideSpinner(container);
  }
}
```

### Retry Logic with Spinner

Retry failed operations with visual feedback:

```typescript
async function fetchWithRetry(
  url: string,
  maxRetries: number = 3,
  delayMs: number = 1000
) {
  const container = document.getElementById('spinner');
  
  for (let attempt = 1; attempt <= maxRetries; attempt++) {
    try {
      showSpinner(container);
      
      // Update label to show attempt number
      const label = attempt > 1 ? `Retrying (${attempt}/${maxRetries})...` : 'Loading...';
      container.querySelector('.e-spinner-label')!.textContent = label;
      
      const response = await fetch(url);
      if (!response.ok) throw new Error(`HTTP ${response.status}`);
      
      const data = await response.json();
      console.log(`Success on attempt ${attempt}`);
      return data;
      
    } catch (error) {
      console.error(`Attempt ${attempt} failed:`, error);
      
      if (attempt < maxRetries) {
        // Wait before retry
        await new Promise(resolve => setTimeout(resolve, delayMs));
      } else {
        throw error; // All retries exhausted
      }
    } finally {
      if (attempt === maxRetries || (error as Error)?.message.includes('HTTP')) {
        hideSpinner(container);
      }
    }
  }
}
```

---

## Multiple Spinners

### Independent Spinners

Create multiple spinners that operate independently:

```typescript
import { createSpinner, showSpinner, hideSpinner } from '@syncfusion/ej2-popups';

class MultiSpinnerManager {
  private spinners: Map<string, HTMLElement> = new Map();
  
  constructor() {
    this.initializeSpinners();
  }
  
  private initializeSpinners() {
    const spinnerIds = ['users-spinner', 'products-spinner', 'orders-spinner'];
    
    spinnerIds.forEach(id => {
      const container = document.getElementById(id);
      if (container) {
        createSpinner({ target: container });
        this.spinners.set(id, container);
      }
    });
  }
  
  show(spinnerId: string) {
    const container = this.spinners.get(spinnerId);
    if (container) showSpinner(container);
  }
  
  hide(spinnerId: string) {
    const container = this.spinners.get(spinnerId);
    if (container) hideSpinner(container);
  }
  
  showAll() {
    this.spinners.forEach(container => showSpinner(container));
  }
  
  hideAll() {
    this.spinners.forEach(container => hideSpinner(container));
  }
}

// Usage
const manager = new MultiSpinnerManager();

// Show specific spinner
manager.show('users-spinner');

// Later, hide it
manager.hide('users-spinner');

// Show all at once
manager.showAll();

// Hide all at once
manager.hideAll();
```

### Spinner Pool with Reuse

Reuse spinner instances for multiple operations:

```typescript
class SpinnerPool {
  private availableSpinners: HTMLElement[] = [];
  private activeSpinners: Set<HTMLElement> = new Set();
  private poolSize: number;
  
  constructor(poolSize: number = 3) {
    this.poolSize = poolSize;
    this.createPool();
  }
  
  private createPool() {
    for (let i = 0; i < this.poolSize; i++) {
      const container = document.createElement('div');
      container.id = `spinner-pool-${i}`;
      container.style.cssText = 'display:none;position:fixed;top:50%;left:50%;transform:translate(-50%,-50%);z-index:9999';
      document.body.appendChild(container);
      
      createSpinner({ target: container });
      this.availableSpinners.push(container);
    }
  }
  
  acquire(): HTMLElement | null {
    const spinner = this.availableSpinners.pop();
    if (spinner) {
      this.activeSpinners.add(spinner);
      spinner.style.display = 'block';
    }
    return spinner || null;
  }
  
  release(spinner: HTMLElement) {
    if (this.activeSpinners.has(spinner)) {
      hideSpinner(spinner);
      spinner.style.display = 'none';
      this.activeSpinners.delete(spinner);
      this.availableSpinners.push(spinner);
    }
  }
  
  getActive(): number {
    return this.activeSpinners.size;
  }
}

// Usage
const pool = new SpinnerPool(5);

// Acquire spinner for operation 1
const spinner1 = pool.acquire();
if (spinner1) showSpinner(spinner1);

// Later, release it for reuse
pool.release(spinner1);

// Acquire for another operation
const spinner2 = pool.acquire();
if (spinner2) showSpinner(spinner2);
```

---

## Error Handling with Spinners

### Spinner with Error State

Show error state after operation failure:

```typescript
async function operationWithErrorHandling() {
  const container = document.getElementById('spinner-container');
  
  showSpinner(container);
  
  try {
    // Simulate operation
    const data = await fetch('/api/data').then(r => r.json());
    return data;
    
  } catch (error) {
    // Hide spinner
    hideSpinner(container);
    
    // Show error message
    container.innerHTML = `
      <div style="text-align: center; color: #e81123;">
        <p>❌ Error: ${(error as Error).message}</p>
        <button onclick="location.reload()">Retry</button>
      </div>
    `;
    throw error;
    
  } finally {
    // Cleanup if needed
  }
}
```

### Fallback UI on Timeout

Show fallback content if spinner times out:

```typescript
async function operationWithFallback(
  operation: () => Promise<unknown>,
  timeoutMs: number = 5000
) {
  const container = document.getElementById('spinner');
  const fallbackContainer = document.getElementById('fallback');
  
  showSpinner(container);
  
  const timeoutId = setTimeout(() => {
    hideSpinner(container);
    container.style.display = 'none';
    fallbackContainer.style.display = 'block';
    fallbackContainer.innerHTML = '<p>⏱️ Operation taking longer than expected. Please try again later.</p>';
  }, timeoutMs);
  
  try {
    const result = await operation();
    clearTimeout(timeoutId);
    hideSpinner(container);
    return result;
    
  } catch (error) {
    clearTimeout(timeoutId);
    hideSpinner(container);
    container.innerHTML = '<p>❌ Operation failed. Please try again.</p>';
    throw error;
  }
}
```

### Progress Feedback with Spinner

Add status updates while spinner is visible:

```typescript
async function longRunningOperation() {
  const container = document.getElementById('spinner');
  const statusEl = container.querySelector('.e-spinner-label');
  
  createSpinner({
    target: container,
    label: 'Step 1 of 3...'
  });
  
  showSpinner(container);
  
  try {
    // Step 1
    statusEl!.textContent = 'Step 1: Loading data...';
    await fetch('/api/step1').then(r => r.json());
    
    // Step 2
    statusEl!.textContent = 'Step 2: Processing...';
    await fetch('/api/step2').then(r => r.json());
    
    // Step 3
    statusEl!.textContent = 'Step 3: Finalizing...';
    await fetch('/api/step3').then(r => r.json());
    
    return true;
    
  } finally {
    hideSpinner(container);
  }
}
```

---

## Global vs Element-Specific

### Global Spinner (Full Page)

```typescript
import { createSpinner, showSpinner, hideSpinner } from '@syncfusion/ej2-popups';

function showGlobalSpinner() {
  createSpinner({
    target: document.body,
    label: 'Processing...'
  });
  
  showSpinner(document.body);
}

function hideGlobalSpinner() {
  hideSpinner(document.body);
}

// Usage
showGlobalSpinner();
setTimeout(() => hideGlobalSpinner(), 3000);
```

### Element-Specific Spinner

```typescript
function showSectionSpinner(sectionId: string) {
  const section = document.getElementById(sectionId);
  
  if (section) {
    createSpinner({
      target: section,
      label: 'Loading section...'
    });
    
    showSpinner(section);
  }
}

function hideSectionSpinner(sectionId: string) {
  const section = document.getElementById(sectionId);
  if (section) hideSpinner(section);
}

// Usage - only specific sections show spinner
showSectionSpinner('users-section');
showSectionSpinner('products-section');
```

### Context-Aware Spinner Selection

Choose between global and element-specific based on operation:

```typescript
class SmartSpinnerManager {
  private isGlobalOperation: boolean = false;
  private container: HTMLElement;
  
  constructor(containerIdOrUseGlobal: string | boolean) {
    if (typeof containerIdOrUseGlobal === 'string') {
      this.container = document.getElementById(containerIdOrUseGlobal)!;
      this.isGlobalOperation = false;
    } else {
      this.container = document.body;
      this.isGlobalOperation = true;
    }
    
    createSpinner({ target: this.container });
  }
  
  show() {
    showSpinner(this.container);
  }
  
  hide() {
    hideSpinner(this.container);
  }
  
  isGlobal(): boolean {
    return this.isGlobalOperation;
  }
}

// Usage - global for critical operations
const globalSpinner = new SmartSpinnerManager(true);
globalSpinner.show();

// Usage - section-specific for non-critical
const sectionSpinner = new SmartSpinnerManager('content-section');
sectionSpinner.show();
```

---

## Performance Optimization

### Lazy Spinner Creation

Create spinners only when needed:

```typescript
class LazySpinner {
  private container: HTMLElement;
  private isCreated: boolean = false;
  
  constructor(containerId: string) {
    this.container = document.getElementById(containerId)!;
  }
  
  show() {
    // Create on first show
    if (!this.isCreated) {
      createSpinner({ target: this.container });
      this.isCreated = true;
    }
    
    showSpinner(this.container);
  }
  
  hide() {
    if (this.isCreated) {
      hideSpinner(this.container);
    }
  }
}

// Usage
const spinner = new LazySpinner('my-spinner');

// Won't create spinner until first show()
setTimeout(() => spinner.show(), 5000);
```

### Batch Operations

Combine multiple operations under single spinner:

```typescript
async function batchOperations(operations: Array<() => Promise<unknown>>) {
  const container = document.getElementById('spinner');
  
  showSpinner(container);
  
  try {
    const results = await Promise.allSettled(
      operations.map(op => op())
    );
    
    return results;
    
  } finally {
    hideSpinner(container);
  }
}

// Usage - single spinner for multiple operations
const operations = [
  () => fetch('/api/users').then(r => r.json()),
  () => fetch('/api/posts').then(r => r.json()),
  () => fetch('/api/comments').then(r => r.json())
];

batchOperations(operations)
  .then(results => console.log('All operations completed:', results));
```

### Debounced Spinner

Delay spinner display for quick operations:

```typescript
class DebouncedSpinner {
  private container: HTMLElement;
  private timeoutId: ReturnType<typeof setTimeout> | null = null;
  private debounceMs: number;
  
  constructor(containerId: string, debounceMs: number = 500) {
    this.container = document.getElementById(containerId)!;
    this.debounceMs = debounceMs;
    
    createSpinner({ target: this.container });
  }
  
  show() {
    // Delay spinner display
    this.timeoutId = setTimeout(() => {
      showSpinner(this.container);
    }, this.debounceMs);
  }
  
  hide() {
    // Clear timeout if hiding before delay
    if (this.timeoutId) {
      clearTimeout(this.timeoutId);
      this.timeoutId = null;
    }
    
    hideSpinner(this.container);
  }
}

// Usage - spinner shows only if operation takes >500ms
const spinner = new DebouncedSpinner('content', 500);

spinner.show();
setTimeout(() => spinner.hide(), 200); // Fast operation, spinner never shows
```

---

## TypeScript Typing

### Custom Spinner Configuration Interface

```typescript
import { createSpinner, showSpinner, hideSpinner } from '@syncfusion/ej2-popups';

interface SpinnerConfig {
  target: HTMLElement;
  label?: string;
  timeout?: number;
  onTimeout?: () => void;
}

interface OperationResult<T> {
  data?: T;
  error?: Error;
  duration: number;
  success: boolean;
}

class TypedSpinnerManager<T> {
  private config: SpinnerConfig;
  private startTime: number = 0;
  
  constructor(config: SpinnerConfig) {
    this.config = config;
    createSpinner({
      target: config.target,
      label: config.label || 'Loading...'
    });
  }
  
  async execute<R extends T>(
    operation: () => Promise<R>
  ): Promise<OperationResult<R>> {
    this.startTime = Date.now();
    showSpinner(this.config.target);
    
    let timeoutId: ReturnType<typeof setTimeout> | null = null;
    
    try {
      if (this.config.timeout) {
        timeoutId = setTimeout(() => {
          hideSpinner(this.config.target);
          this.config.onTimeout?.();
        }, this.config.timeout);
      }
      
      const data = await operation();
      clearTimeout(timeoutId ?? undefined);
      
      return {
        data,
        success: true,
        duration: Date.now() - this.startTime
      };
      
    } catch (error) {
      clearTimeout(timeoutId ?? undefined);
      
      return {
        error: error as Error,
        success: false,
        duration: Date.now() - this.startTime
      };
      
    } finally {
      hideSpinner(this.config.target);
    }
  }
}

// Usage
interface UserData {
  id: number;
  name: string;
  email: string;
}

const manager = new TypedSpinnerManager<UserData>({
  target: document.getElementById('spinner')!,
  label: 'Loading users...',
  timeout: 5000,
  onTimeout: () => alert('Operation timed out')
});

const result = await manager.execute(async () => {
  const response = await fetch('/api/users');
  return response.json();
});

if (result.success) {
  console.log('Users loaded:', result.data);
  console.log('Duration:', result.duration, 'ms');
} else {
  console.error('Error:', result.error);
}
```

---

## Custom Spinner Manager

### Production-Ready Manager Class

```typescript
import { createSpinner, showSpinner, hideSpinner } from '@syncfusion/ej2-popups';

interface SpinnerOptions {
  target: HTMLElement;
  label?: string;
  timeout?: number;
  showDelay?: number;
}

class SpinnerManager {
  private static instances: Map<string, SpinnerManager> = new Map();
  
  private id: string;
  private options: SpinnerOptions;
  private isVisible: boolean = false;
  private showTimeoutId: ReturnType<typeof setTimeout> | null = null;
  private operationTimeoutId: ReturnType<typeof setTimeout> | null = null;
  
  private constructor(id: string, options: SpinnerOptions) {
    this.id = id;
    this.options = options;
    
    createSpinner({
      target: options.target,
      label: options.label || 'Loading...'
    });
  }
  
  static getInstance(id: string, options: SpinnerOptions): SpinnerManager {
    if (!this.instances.has(id)) {
      this.instances.set(id, new SpinnerManager(id, options));
    }
    return this.instances.get(id)!;
  }
  
  show(): void {
    if (this.isVisible) return;
    
    // Use showDelay to avoid flashing spinners on fast operations
    this.showTimeoutId = setTimeout(() => {
      showSpinner(this.options.target);
      this.isVisible = true;
      
      // Set operation timeout if configured
      if (this.options.timeout) {
        this.operationTimeoutId = setTimeout(() => {
          this.hide();
          console.warn(`Spinner timeout after ${this.options.timeout}ms`);
        }, this.options.timeout);
      }
    }, this.options.showDelay || 0);
  }
  
  hide(): void {
    if (!this.isVisible) return;
    
    // Clear timeouts
    clearTimeout(this.showTimeoutId ?? undefined);
    clearTimeout(this.operationTimeoutId ?? undefined);
    
    hideSpinner(this.options.target);
    this.isVisible = false;
  }
  
  isShowing(): boolean {
    return this.isVisible;
  }
  
  async executeOperation<T>(operation: () => Promise<T>): Promise<T> {
    this.show();
    
    try {
      return await operation();
    } finally {
      this.hide();
    }
  }
  
  static cleanup(id: string): void {
    const manager = this.instances.get(id);
    if (manager) {
      manager.hide();
      this.instances.delete(id);
    }
  }
}

// Usage
const spinnerManager = SpinnerManager.getInstance('main-spinner', {
  target: document.getElementById('content')!,
  label: 'Processing...',
  timeout: 10000,
  showDelay: 200 // Don't show spinner for fast operations
});

spinnerManager.executeOperation(async () => {
  const response = await fetch('/api/data');
  return response.json();
})
  .then(data => console.log('Success:', data))
  .catch(error => console.error('Error:', error));
```

---

## Next Steps

- Review [spinner-methods.md](spinner-methods.md) for API reference
- Check [theming-and-styling.md](theming-and-styling.md) for customization
- See [getting-started.md](getting-started.md) for basic setup
