# Toolbar Configuration

## Table of Contents
- [Toolbar Overview](#toolbar-overview)
- [Header Toolbar](#header-toolbar)
- [Footer Toolbar](#footer-toolbar)
- [Response Toolbar](#response-toolbar)
- [Prompt Toolbar](#prompt-toolbar)
- [Toolbar Item Properties](#toolbar-item-properties)
- [Toolbar Item Click Events](#toolbar-item-click-events)
- [Built-in Toolbar Items](#built-in-toolbar-items)

This guide covers all toolbar configuration options including header, footer, prompt, and response toolbars.

---

## Toolbar Overview

The AI AssistView provides four customizable toolbars:

1. **Header Toolbar** (`toolbarSettings`) - Top of the control
2. **Footer Toolbar** (`footerToolbarSettings`) - Input area toolbar
3. **Response Toolbar** (`responseToolbarSettings`) - Actions for AI responses
4. **Prompt Toolbar** (`promptToolbarSettings`) - Actions for user prompts

---

## Header Toolbar

The header toolbar appears at the top of the AI AssistView control.

### Property

```typescript
toolbarSettings: ToolbarSettingsModel
```

### ToolbarSettingsModel Interface

```typescript
interface ToolbarSettingsModel {
    items: ToolbarItemModel[];  // Array of toolbar items
}
```

### Example

```typescript
import { AIAssistView, ToolbarItemClickedEventArgs } from '@syncfusion/ej2-interactive-chat';

const aiAssistView: AIAssistView = new AIAssistView({
    toolbarSettings: {
        items: [
            { type: 'Button', iconCss: 'e-icons e-refresh', align: 'Right', tooltip: 'Refresh' },
            { type: 'Button', iconCss: 'e-icons e-settings', align: 'Right', tooltip: 'Settings' }
        ],
        itemClicked: (args: ToolbarItemClickedEventArgs) => {
            if (args.item.iconCss.includes('refresh')) {
                aiAssistView.prompts = [];  // Clear conversation
                aiAssistView.promptSuggestions = suggestions;
            } else if (args.item.iconCss.includes('settings')) {
                // Open settings
            }
        }
    }
});
```

### Common Use Cases

```typescript
// Refresh button to clear conversation
{
    items: [
        { type: 'Button', iconCss: 'e-icons e-refresh', align: 'Right', tooltip: 'Clear Chat' }
    ],
    itemClicked: (args) => {
        if (args.item.iconCss.includes('refresh')) {
            aiAssistView.prompts = [];
        }
    }
}

// User profile button
{
    items: [
        { type: 'Button', iconCss: 'e-icons e-user', align: 'Right', disabled: false }
    ]
}
```

---

## Footer Toolbar

The footer toolbar appears in the prompt input area.

### Property

```typescript
footerToolbarSettings: FooterToolbarSettingsModel
```

### FooterToolbarSettingsModel Interface

```typescript
interface FooterToolbarSettingsModel {
    items: ToolbarItemModel[];              // Array of toolbar items
    toolbarPosition: 'Inline' | 'Bottom';   // Toolbar position
}
```

### Default Footer Items

By default, the footer toolbar renders:
- **Send button** - To submit prompts
- **Attachment button** (if `enableAttachments: true`)

### Toolbar Positioning

**Inline (Default):** Toolbar items appear inline with the textarea

```typescript
const aiAssistView: AIAssistView = new AIAssistView({
    footerToolbarSettings: {
        toolbarPosition: 'Inline',  // Default
        items: [
            { type: 'Button', iconCss: 'e-icons e-send' }
        ]
    }
});
```

**Bottom:** Toolbar items appear in a dedicated footer area below the textarea

```typescript
const aiAssistView: AIAssistView = new AIAssistView({
    footerToolbarSettings: {
        toolbarPosition: 'Bottom',
        items: [
            { type: 'Button', text: 'Send', iconCss: 'e-icons e-send' },
            { type: 'Button', text: 'Clear', iconCss: 'e-icons e-clear' }
        ]
    }
});
```

### Custom Footer Toolbar Example

```typescript
const aiAssistView: AIAssistView = new AIAssistView({
    enableAttachments: true,
    footerToolbarSettings: {
        toolbarPosition: 'Bottom',
        items: [
            { type: 'Button', iconCss: 'e-icons e-attachment', tooltip: 'Attach File' },
            { type: 'Button', iconCss: 'e-icons e-microphone', tooltip: 'Voice Input' },
            { type: 'Button', iconCss: 'e-icons e-send', tooltip: 'Send', align: 'Right' }
        ],
        itemClicked: (args: ToolbarItemClickedEventArgs) => {
            if (args.item.iconCss.includes('microphone')) {
                // Start voice input
            }
        }
    }
});
```

---

## Response Toolbar

The response toolbar appears next to AI response messages.

### Property

```typescript
responseToolbarSettings: ResponseToolbarSettingsModel
```

### ResponseToolbarSettingsModel Interface

```typescript
interface ResponseToolbarSettingsModel {
    items: ToolbarItemModel[];    // Array of toolbar items
    width?: string | number;      // Toolbar width
}
```

### Default Response Items

Common response toolbar items include:
- **Like/Dislike** - User feedback
- **Copy** - Copy response to clipboard
- **Refresh** - Regenerate response

### Example

```typescript
import { AIAssistView, ToolbarItemClickedEventArgs, PromptModel } from '@syncfusion/ej2-interactive-chat';

const aiAssistView: AIAssistView = new AIAssistView({
    responseToolbarSettings: {
        items: [
            { type: 'Button', iconCss: 'e-icons e-copy', tooltip: 'Copy' },
            { type: 'Button', iconCss: 'e-icons e-refresh', tooltip: 'Regenerate' },
            { type: 'Button', iconCss: 'e-icons e-assist-like', tooltip: 'Like' },
            { type: 'Button', iconCss: 'e-icons e-assist-dislike', tooltip: 'Dislike' }
        ],
        itemClicked: (args: ToolbarItemClickedEventArgs) => {
            const responseData: PromptModel = aiAssistView.prompts[args.dataIndex];

            if (args.item.iconCss.includes('copy')) {
                // Copy response to clipboard
                navigator.clipboard.writeText(responseData.response);
                console.log('Response copied!');
            } else if (args.item.iconCss.includes('refresh')) {
                // Regenerate response
                aiAssistView.executePrompt(responseData.prompt);
            } else if (args.item.iconCss.includes('like')) {
                // Mark as helpful
                responseData.isResponseHelpful = true;
                console.log('Response marked as helpful');
            } else if (args.item.iconCss.includes('dislike')) {
                // Mark as not helpful
                responseData.isResponseHelpful = false;
                console.log('Response marked as not helpful');
            }
        }
    }
});
```

### Custom Width

```typescript
responseToolbarSettings: {
    width: '200px',  // or number: 200
    items: [...]
}
```

---

## Prompt Toolbar

The prompt toolbar appears next to user prompt messages.

### Property

```typescript
promptToolbarSettings: PromptToolbarSettingsModel
```

### PromptToolbarSettingsModel Interface

```typescript
interface PromptToolbarSettingsModel {
    items: ToolbarItemModel[];    // Array of toolbar items
    width?: string | number;      // Toolbar width
}
```

### Example

```typescript
const aiAssistView: AIAssistView = new AIAssistView({
    promptToolbarSettings: {
        items: [
            { type: 'Button', iconCss: 'e-icons e-edit', tooltip: 'Edit' },
            { type: 'Button', iconCss: 'e-icons e-delete', tooltip: 'Delete' }
        ],
        itemClicked: (args: ToolbarItemClickedEventArgs) => {
            const promptData = aiAssistView.prompts[args.dataIndex];

            if (args.item.iconCss.includes('edit')) {
                // Edit prompt
                aiAssistView.prompt = promptData.prompt;
            } else if (args.item.iconCss.includes('delete')) {
                // Remove prompt-response pair
                aiAssistView.prompts.splice(args.dataIndex, 1);
                aiAssistView.refresh();
            }
        }
    }
});
```

---

## Toolbar Item Properties

### ToolbarItemModel Interface

```typescript
interface ToolbarItemModel {
    type: ItemType;                    // 'Button', 'Separator', 'Input'
    text?: string;                     // Display text
    iconCss?: string;                  // Icon CSS class
    tooltip?: string;                  // Tooltip text
    cssClass?: string;                 // Custom CSS class
    align?: ItemAlign;                 // 'Left', 'Center', 'Right'
    disabled?: boolean;                // Enable/disable state
    visible?: boolean;                 // Show/hide state
    template?: string | Function;      // Custom template
    tabIndex?: number;                 // Tab order
}
```

### Property Examples

**Basic Button:**

```typescript
{ 
    type: 'Button', 
    text: 'Send', 
    iconCss: 'e-icons e-send' 
}
```

**Icon Only:**

```typescript
{ 
    type: 'Button', 
    iconCss: 'e-icons e-copy', 
    tooltip: 'Copy to Clipboard' 
}
```

**Right Aligned:**

```typescript
{ 
    type: 'Button', 
    iconCss: 'e-icons e-settings', 
    align: 'Right' 
}
```

**Disabled State:**

```typescript
{ 
    type: 'Button', 
    text: 'Submit', 
    disabled: true 
}
```

**Custom CSS:**

```typescript
{ 
    type: 'Button', 
    text: 'Action', 
    cssClass: 'custom-button-class' 
}
```

**Separator:**

```typescript
{ 
    type: 'Separator' 
}
```

**Custom Template:**

```typescript
{ 
    type: 'Button',
    template: '<button class="custom-btn">Custom</button>'
}
```

---

## Toolbar Item Click Events

### ToolbarItemClickedEventArgs Interface

```typescript
interface ToolbarItemClickedEventArgs {
    item: ToolbarItemModel;   // The clicked toolbar item
    event: Event;             // Original click event
    dataIndex: number;        // Index of message data (for prompt/response toolbars)
    cancel: boolean;          // Set to true to cancel action
    name: string;             // Event name
}
```

### Event Handler Pattern

```typescript
itemClicked: (args: ToolbarItemClickedEventArgs) => {
    console.log('Clicked item:', args.item);
    console.log('Message index:', args.dataIndex);

    // Access the specific prompt-response data
    if (args.dataIndex !== undefined) {
        const messageData = aiAssistView.prompts[args.dataIndex];
        console.log('Associated message:', messageData);
    }

    // Perform action based on icon
    if (args.item.iconCss.includes('copy')) {
        // Copy action
    }

    // Cancel default action
    args.cancel = true;
}
```

### Complete Example with Multiple Toolbars

```typescript
const aiAssistView: AIAssistView = new AIAssistView({
    // Header toolbar
    toolbarSettings: {
        items: [
            { type: 'Button', text: 'New Chat', iconCss: 'e-icons e-plus' },
            { type: 'Separator' },
            { type: 'Button', iconCss: 'e-icons e-refresh', align: 'Right', tooltip: 'Clear' },
            { type: 'Button', iconCss: 'e-icons e-settings', align: 'Right', tooltip: 'Settings' }
        ],
        itemClicked: (args) => {
            if (args.item.iconCss.includes('plus')) {
                aiAssistView.prompts = [];
            } else if (args.item.iconCss.includes('refresh')) {
                aiAssistView.prompts = [];
                aiAssistView.promptSuggestions = initialSuggestions;
            }
        }
    },

    // Footer toolbar
    footerToolbarSettings: {
        toolbarPosition: 'Bottom',
        items: [
            { type: 'Button', iconCss: 'e-icons e-attachment', tooltip: 'Attach' },
            { type: 'Button', iconCss: 'e-icons e-microphone', tooltip: 'Voice' },
            { type: 'Button', iconCss: 'e-icons e-send', align: 'Right', tooltip: 'Send' }
        ]
    },

    // Response toolbar
    responseToolbarSettings: {
        items: [
            { type: 'Button', iconCss: 'e-icons e-copy', tooltip: 'Copy' },
            { type: 'Button', iconCss: 'e-icons e-refresh', tooltip: 'Regenerate' },
            { type: 'Button', iconCss: 'e-icons e-assist-like', tooltip: 'Like' },
            { type: 'Button', iconCss: 'e-icons e-assist-dislike', tooltip: 'Dislike' }
        ],
        itemClicked: (args) => {
            const response = aiAssistView.prompts[args.dataIndex];

            if (args.item.iconCss.includes('copy')) {
                navigator.clipboard.writeText(response.response);
            } else if (args.item.iconCss.includes('refresh')) {
                aiAssistView.executePrompt(response.prompt);
            } else if (args.item.iconCss.includes('like')) {
                response.isResponseHelpful = true;
            } else if (args.item.iconCss.includes('dislike')) {
                response.isResponseHelpful = false;
            }
        }
    },

    // Prompt toolbar
    promptToolbarSettings: {
        items: [
            { type: 'Button', iconCss: 'e-icons e-edit', tooltip: 'Edit' }
        ],
        itemClicked: (args) => {
            const prompt = aiAssistView.prompts[args.dataIndex];
            aiAssistView.prompt = prompt.prompt;
        }
    }
});
```

---

## Built-in Toolbar Items

### Default Icons (Syncfusion Icon Library)

- `e-icons e-send` - Send button
- `e-icons e-attachment` - Attachment button
- `e-icons e-copy` - Copy button
- `e-icons e-refresh` - Refresh button
- `e-icons e-assist-like` - Like button
- `e-icons e-assist-dislike` - Dislike button
- `e-icons e-edit` - Edit button
- `e-icons e-delete` - Delete button
- `e-icons e-settings` - Settings button
- `e-icons e-user` - User button
- `e-icons e-microphone` - Microphone button

### Using Custom Icons

```html
<style>
    .custom-icon::before {
        content: '✨';
        font-size: 18px;
    }
</style>
```

```typescript
{
    type: 'Button',
    iconCss: 'custom-icon',
    tooltip: 'Custom Action'
}
```

---

## Summary

**Toolbar Types:**
- Header toolbar (`toolbarSettings`) - Control-level actions
- Footer toolbar (`footerToolbarSettings`) - Input area actions with positioning
- Response toolbar (`responseToolbarSettings`) - AI response actions
- Prompt toolbar (`promptToolbarSettings`) - User prompt actions

**Key Properties:**
- `items`: Array of ToolbarItemModel
- `toolbarPosition`: 'Inline' | 'Bottom' (footer only)
- `width`: Toolbar width (prompt/response only)
- `itemClicked`: Click event handler

**Common Actions:**
- Copy, like/dislike, regenerate (responses)
- Edit, delete (prompts)
- Clear, settings, refresh (header)
- Attach, voice input, send (footer)
