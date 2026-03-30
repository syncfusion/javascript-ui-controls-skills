# How-To Guide

## Table of Contents
- [Render in Dialog](#render-in-dialog)
- [Clear Image Before Reopening](#clear-image-before-reopening)
- [Reset to Original State](#reset-to-original-state)
- [Fit Image to Dimensions](#fit-image-to-dimensions)
- [Dialog State Management](#dialog-state-management)

## Render in Dialog

### Basic Dialog Setup

Render the Image Editor inside a modal dialog:

```typescript
import { ImageEditor } from '@syncfusion/ej2-image-editor';
import { Dialog } from '@syncfusion/ej2-popups';

// Create dialog
const dialog = new Dialog({
    isModal: true,
    width: '600px',
    height: '500px',
    closeOnEscape: true,
    visible: false,
    minHeight: '400px',
    content: document.getElementById('imageeditor')
});
dialog.appendTo('#profile-dialog');

// Create image editor inside dialog
const imageEditor = new ImageEditor({
    width: '100%',
    height: '100%'
});
imageEditor.appendTo('#imageeditor');

// Show dialog with image
const openBtn = document.getElementById('openBtn');
openBtn.onclick = () => {
    dialog.show();
    setTimeout(() => {
        imageEditor.open('flower.png');
    }, 10);
};
```

### HTML Structure

```html
<div id="profile-dialog">
    <div id="imageeditor"></div>
</div>

<button id="openBtn">Open Image Editor</button>
```

### Dialog with Save Button

```typescript
const dialog = new Dialog({
    isModal: true,
    width: '600px',
    height: '500px',
    buttons: [
        {
            buttonModel: { content: 'Save', isPrimary: true },
            click: () => {
                imageEditor.save('edited-image.png');
                dialog.close();
            }
        },
        {
            buttonModel: { content: 'Cancel' },
            click: () => {
                dialog.close();
            }
        }
    ]
});
```

## Clear Image Before Reopening

### Clear Image Method

Use `clearImage()` to reset editor before reopening dialog:

```typescript
import { ImageEditor } from '@syncfusion/ej2-image-editor';
import { Dialog } from '@syncfusion/ej2-popups';

const imageEditor = new ImageEditor({
    width: '550px',
    height: '330px'
});

const dialog = new Dialog({
    isModal: true,
    visible: false
});
dialog.appendTo('#profile-dialog');

// Close handler
dialog.beforeClose = () => {
    // Clear image before closing
    imageEditor.clearImage();
};

// Open handler
const openBtn = document.getElementById('openBtn');
openBtn.onclick = () => {
    // Ensure clean state
    imageEditor.clearImage();
    dialog.show();
    
    setTimeout(() => {
        imageEditor.open('new-image.jpg');
    }, 10);
};
```

### Why Clear Before Reopening

**Problem:** Image editor retains previous image when dialog reopens
**Solution:** Call `clearImage()` to ensure fresh start

```typescript
// BAD: Dialog remembers old image
dialog.show();
imageEditor.open('new-image.jpg');  // Old image still there!

// GOOD: Clear first
dialog.show();
imageEditor.clearImage();
imageEditor.open('new-image.jpg');  // Clean slate
```

## Reset to Original State

### Reset Method

Revert all changes to original image:

```typescript
// Undo all modifications and return to original
imageEditor.reset();

// This reverts:
// - All annotations
// - All filters and effects
// - All transformations (rotate, flip, crop)
// - Image returns to opened state
```

### Reset Button

```typescript
const resetBtn = document.getElementById('resetBtn');
resetBtn.onclick = () => {
    if (confirm('Reset all changes to original image?')) {
        imageEditor.reset();
    }
};
```

### Compare Before/After

```typescript
function setupBeforeAfterComparison() {
    let isOriginal = false;
    const compareBtn = document.getElementById('compareBtn');
    
    compareBtn.onclick = () => {
        if (isOriginal) {
            // Show edited version
            // (would require saving state before reset)
            console.log('Show edited version');
        } else {
            // Show original
            imageEditor.reset();
        }
        isOriginal = !isOriginal;
        compareBtn.textContent = isOriginal ? 'Show Edited' : 'Show Original';
    };
}
```

## Fit Image to Dimensions

### Fit to Width

Scale image to match editor width:

```typescript
function fitToWidth() {
    imageEditor.zoom(1);  // Reset to 100% first
    
    const containerWidth = imageEditor.upperCanvas.width;
    const dimension = imageEditor.getImageDimension();
    
    // Calculate zoom to fit width
    let zoomLevel = 1;
    let imageWidth = dimension.width;
    let zoomFactor = 0.1;
    
    while (imageWidth < containerWidth) {
        zoomLevel++;
        imageWidth = dimension.width * (1 + zoomFactor);
        zoomFactor += 0.1;
    }
    
    imageEditor.zoom(zoomLevel);
}

document.getElementById('fitWidthBtn').onclick = fitToWidth;
```

### Fit to Height

Scale image to match editor height:

```typescript
function fitToHeight() {
    imageEditor.zoom(1);  // Reset first
    
    const containerHeight = imageEditor.upperCanvas.height;
    const dimension = imageEditor.getImageDimension();
    
    // Calculate zoom to fit height
    let zoomLevel = 1;
    let imageHeight = dimension.height;
    let zoomFactor = 0.1;
    
    while (imageHeight < containerHeight) {
        zoomLevel++;
        imageHeight = dimension.height * (1 + zoomFactor);
        zoomFactor += 0.1;
    }
    
    imageEditor.zoom(zoomLevel);
}

document.getElementById('fitHeightBtn').onclick = fitToHeight;
```

### Fit to Container

```typescript
function fitToContainer() {
    imageEditor.zoom(1);  // Reset
    
    const containerWidth = imageEditor.upperCanvas.width;
    const containerHeight = imageEditor.upperCanvas.height;
    const dimension = imageEditor.getImageDimension();
    
    // Calculate zoom to fit both dimensions
    const zoomWidth = containerWidth / dimension.width;
    const zoomHeight = containerHeight / dimension.height;
    const zoom = Math.min(zoomWidth, zoomHeight);
    
    imageEditor.zoom(Math.max(0.1, zoom));
}

document.getElementById('fitContainerBtn').onclick = fitToContainer;
```

## Dialog State Management

### Save Dialog State

```typescript
class DialogStateManager {
    private state: any = {};
    
    constructor(private imageEditor: ImageEditor, private dialog: Dialog) {}
    
    // Save current state before closing
    saveState() {
        const dimension = this.imageEditor.getImageDimension();
        this.state = {
            zoom: this.imageEditor.zoomLevel,
            rotation: this.imageEditor.rotation,
            canvasWidth: dimension.clientWidth,
            canvasHeight: dimension.clientHeight
        };
    }
    
    // Restore state when reopening
    restoreState() {
        if (this.state.zoom) {
            this.imageEditor.zoom(this.state.zoom);
        }
        if (this.state.rotation) {
            this.imageEditor.rotate(this.state.rotation);
        }
    }
    
    // Clear state
    clearState() {
        this.state = {};
    }
}

const stateManager = new DialogStateManager(imageEditor, dialog);

// Save before close
dialog.beforeClose = () => {
    stateManager.saveState();
    imageEditor.clearImage();
};

// Restore on open
document.getElementById('openBtn').onclick = () => {
    imageEditor.clearImage();
    dialog.show();
    stateManager.restoreState();
    imageEditor.open('image.jpg');
};
```

### Multiple Editor Instances

```typescript
class MultiDialogManager {
    private editors: Map<string, ImageEditor> = new Map();
    private dialogs: Map<string, Dialog> = new Map();
    
    createEditorDialog(id: string, title: string) {
        // Create dialog
        const dialog = new Dialog({
            isModal: true,
            width: '600px',
            height: '500px',
            title: title,
            visible: false
        });
        dialog.appendTo(`#${id}-dialog`);
        
        // Create editor
        const editor = new ImageEditor({
            width: '100%',
            height: '100%'
        });
        editor.appendTo(`#${id}`);
        
        this.editors.set(id, editor);
        this.dialogs.set(id, dialog);
    }
    
    openEditor(id: string, imageUrl: string) {
        const dialog = this.dialogs.get(id);
        const editor = this.editors.get(id);
        
        if (dialog && editor) {
            editor.clearImage();
            dialog.show();
            editor.open(imageUrl);
        }
    }
    
    closeEditor(id: string) {
        const dialog = this.dialogs.get(id);
        const editor = this.editors.get(id);
        
        if (dialog && editor) {
            editor.clearImage();
            dialog.close();
        }
    }
}

// Usage
const manager = new MultiDialogManager();
manager.createEditorDialog('editor1', 'Edit Profile Photo');
manager.createEditorDialog('editor2', 'Edit Cover Photo');

document.getElementById('editProfileBtn').onclick = () => {
    manager.openEditor('editor1', 'profile.jpg');
};

document.getElementById('editCoverBtn').onclick = () => {
    manager.openEditor('editor2', 'cover.jpg');
};
```

### Dialog with Confirmation

```typescript
const dialog = new Dialog({
    isModal: true,
    beforeClose: (args) => {
        // Prevent close if changes not saved
        if (hasUnsavedChanges()) {
            args.cancel = true;
            
            // Show confirmation
            if (confirm('You have unsaved changes. Close anyway?')) {
                imageEditor.clearImage();
                dialog.destroy();
            }
        }
    }
});

function hasUnsavedChanges() {
    // Check if image has been modified
    return (imageEditor.history || []).length > 0;
}
```

### Dialog Lifecycle

```typescript
class DialogLifecycle {
    constructor(private imageEditor: ImageEditor, private dialog: Dialog) {
        this.setupHandlers();
    }
    
    setupHandlers() {
        this.dialog.open = () => {
            console.log('Dialog opened');
            this.onDialogOpen();
        };
        
        this.dialog.beforeClose = () => {
            console.log('Dialog closing');
            this.onBeforeDialogClose();
        };
        
        this.dialog.close = () => {
            console.log('Dialog closed');
            this.onDialogClosed();
        };
    }
    
    onDialogOpen() {
        console.log('Initialize editor on open');
    }
    
    onBeforeDialogClose() {
        console.log('Save or discard changes');
        this.imageEditor.clearImage();
    }
    
    onDialogClosed() {
        console.log('Cleanup after close');
    }
}
```

