# Undo and Redo

## Table of Contents
- [Keyboard Shortcuts](#keyboard-shortcuts)
- [Undo/Redo Methods](#undoredo-methods)
- [History Management](#history-management)
- [Common Patterns](#common-patterns)

## Keyboard Shortcuts

### Standard Shortcuts

Users can undo and redo using keyboard:

```
Ctrl + Z  : Undo last action
Ctrl + Y  : Redo last undone action
```

### Supported Operations

The undo/redo history tracks all modifications:

- Image opening/loading
- Zooming operations
- Rotation and flipping
- Cropping actions
- Resizing operations
- Annotations (text, shapes, freehand)
- Filters and fine-tuning adjustments
- Layer operations (z-order changes)
- Redaction actions
- Straightening adjustments

## Undo/Redo Methods

### Undo Last Action

```typescript
import { ImageEditor } from '@syncfusion/ej2-image-editor';

const imageEditor = new ImageEditor({ width: '550px', height: '330px' });
imageEditor.appendTo('#imageeditor');

// Undo last action programmatically
imageEditor.undo();

// Multiple undos in sequence
imageEditor.undo();
imageEditor.undo();
imageEditor.undo();
```

### Redo Last Undone Action

```typescript
// Redo last undone action
imageEditor.redo();

// Multiple redos
imageEditor.redo();
imageEditor.redo();
```

### Undo Specific Count

```typescript
// Undo multiple times (approach: call multiple times)
for (let i = 0; i < 5; i++) {
    imageEditor.undo();
}
```

### Check Undo/Redo Available

```typescript
// Check if undo is available
if (imageEditor.canUndo()) {
    imageEditor.undo();
}

// Check if redo is available
if (imageEditor.canRedo()) {
    imageEditor.redo();
}
```

## History Management

### Get History Stack

```typescript
// Get all actions in history
const history = imageEditor.history;

if (history) {
    console.log(`Total actions: ${history.length}`);
    history.forEach((action, index) => {
        console.log(`${index}: ${action.action}`);
    });
}
```

### Clear History

```typescript
// Reset history after major operation
imageEditor.clearHistory();

// Or when opening new image
imageEditor.open('new-image.jpg');
imageEditor.clearHistory();  // Clear old history
```

### History Limits

```typescript
// Configure history size (if supported)
const imageEditor = new ImageEditor({
    width: '550px',
    height: '330px',
    historyMaxSize: 100  // Keep last 100 actions
});
```

### Action Tracking

Track what actions are being performed:

```typescript
const imageEditor = new ImageEditor({
    width: '550px',
    height: '330px'
});

// Perform actions
imageEditor.open('image.jpg');
imageEditor.rotate(90);
imageEditor.drawText(100, 100, 'Annotation');
imageEditor.finetuneImage('brightness', 20);

// Later check what can be undone
if (imageEditor.history && imageEditor.history.length > 0) {
    console.log('Last action can be undone');
}
```

## Toolbar Integration

### Enable Undo/Redo Buttons

```typescript
const imageEditor = new ImageEditor({
    toolbar: ['Undo', 'Redo'],
    // Users see Undo/Redo buttons
});
```

### Button Enabling/Disabling

```typescript
// Buttons automatically enable/disable based on history state
// Undo button: enabled if actions available to undo
// Redo button: enabled if actions available to redo
```

### Full History Toolbar

```typescript
const imageEditor = new ImageEditor({
    toolbar: [
        'Open', 'Save',
        'ZoomIn', 'ZoomOut',
        'Crop', 'Rotate', 'Flip',
        'Undo', 'Redo', 'Reset',
        'Annotation',
        'Filter'
    ]
});
```

## Common Patterns

### Pattern: Undo/Redo with UI Buttons

```typescript
function setupUndoRedoButtons(imageEditor: ImageEditor) {
    const undoBtn = document.getElementById('undoBtn') as HTMLButtonElement;
    const redoBtn = document.getElementById('redoBtn') as HTMLButtonElement;
    
    undoBtn.onclick = () => {
        if (imageEditor.canUndo()) {
            imageEditor.undo();
            updateButtonStates();
        }
    };
    
    redoBtn.onclick = () => {
        if (imageEditor.canRedo()) {
            imageEditor.redo();
            updateButtonStates();
        }
    };
    
    function updateButtonStates() {
        undoBtn.disabled = !imageEditor.canUndo();
        redoBtn.disabled = !imageEditor.canRedo();
    }
    
    // Initial state
    updateButtonStates();
}
```

### Pattern: Action Counter

```typescript
function showActionCounter(imageEditor: ImageEditor) {
    const counterDisplay = document.getElementById('actionCounter');
    
    // Update counter after operations
    function updateCounter() {
        const history = imageEditor.history || [];
        const totalActions = history.length;
        const undoCount = imageEditor.canUndo() ? 1 : 0;
        const redoCount = imageEditor.canRedo() ? 1 : 0;
        
        counterDisplay.textContent = `Total actions: ${totalActions}`;
    }
    
    // Listen to image editor events
    const originalUndo = imageEditor.undo.bind(imageEditor);
    const originalRedo = imageEditor.redo.bind(imageEditor);
    
    imageEditor.undo = () => {
        originalUndo();
        updateCounter();
    };
    
    imageEditor.redo = () => {
        originalRedo();
        updateCounter();
    };
}
```

### Pattern: Undo Stack Preview

```typescript
function showHistoryPreview(imageEditor: ImageEditor) {
    const historyList = document.getElementById('historyList');
    
    function updateHistoryList() {
        historyList.innerHTML = '';
        
        const history = imageEditor.history || [];
        history.forEach((action, index) => {
            const li = document.createElement('li');
            li.textContent = `${index + 1}. ${action.action}`;
            
            // Click to jump to this point
            li.onclick = () => {
                const stepsToUndo = history.length - index - 1;
                for (let i = 0; i < stepsToUndo; i++) {
                    imageEditor.undo();
                }
            };
            
            historyList.appendChild(li);
        });
    }
    
    // Update after operations
    updateHistoryList();
}
```

### Pattern: Revert to Original

```typescript
function revertToOriginal(imageEditor: ImageEditor) {
    // Undo all actions
    while (imageEditor.canUndo()) {
        imageEditor.undo();
    }
}

document.getElementById('revertBtn').onclick = () => {
    if (confirm('Revert all changes to original?')) {
        revertToOriginal(imageEditor);
    }
};
```

### Pattern: Checkpoint System

```typescript
class ImageEditorCheckpoint {
    private checkpoints: any[] = [];
    
    constructor(private imageEditor: ImageEditor) {}
    
    // Save checkpoint
    saveCheckpoint(name: string) {
        this.checkpoints.push({
            name: name,
            historyState: this.imageEditor.history.length,
            timestamp: Date.now()
        });
    }
    
    // List checkpoints
    getCheckpoints() {
        return this.checkpoints;
    }
    
    // Revert to checkpoint
    revertToCheckpoint(index: number) {
        const checkpoint = this.checkpoints[index];
        const currentState = this.imageEditor.history.length;
        const stepsBack = currentState - checkpoint.historyState;
        
        for (let i = 0; i < stepsBack; i++) {
            this.imageEditor.undo();
        }
    }
    
    // Clear old checkpoints
    clearCheckpoints() {
        this.checkpoints = [];
    }
}

// Usage
const checkpoint = new ImageEditorCheckpoint(imageEditor);

// Save at key points
checkpoint.saveCheckpoint('After crop');
checkpoint.saveCheckpoint('After annotation');

// Later revert
checkpoint.revertToCheckpoint(0);
```

### Pattern: Undo Limit Alert

```typescript
function setupUndoLimitWarning(imageEditor: ImageEditor) {
    const maxHistory = 50;  // Limit to 50 actions
    
    setInterval(() => {
        const history = imageEditor.history || [];
        if (history.length > maxHistory) {
            console.warn(`History approaching limit: ${history.length}/${maxHistory}`);
            
            // Optionally clear oldest entries
            // imageEditor.clearHistory();
        }
    }, 1000);
}
```

### Pattern: Transaction-like Operations

```typescript
class ImageEditorTransaction {
    private startHistoryLength: number;
    
    constructor(private imageEditor: ImageEditor) {
        this.startHistoryLength = (this.imageEditor.history || []).length;
    }
    
    // Undo all operations in this transaction
    rollback() {
        const endLength = (this.imageEditor.history || []).length;
        const operationCount = endLength - this.startHistoryLength;
        
        for (let i = 0; i < operationCount; i++) {
            this.imageEditor.undo();
        }
    }
    
    // Get operation count
    getOperationCount() {
        return (this.imageEditor.history || []).length - this.startHistoryLength;
    }
}

// Usage
const transaction = new ImageEditorTransaction(imageEditor);

imageEditor.rotate(90);
imageEditor.drawText(100, 100, 'Text');
imageEditor.finetuneImage('brightness', 20);

// If something goes wrong, rollback entire transaction
if (someError) {
    transaction.rollback();
}
```

