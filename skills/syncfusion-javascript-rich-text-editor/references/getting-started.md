# Getting Started

Setup and initialization for both HTML (WYSIWYG) and Markdown editor modes.

## Table of Contents
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [CSS Imports](#css-imports)
- [HTML Editor Setup](#html-editor-setup)
- [Markdown Editor Setup](#markdown-editor-setup)
- [Feature Module Reference](#feature-module-reference)
- [Initialization on Textarea](#initialization-on-textarea)

---

## Prerequisites

- Node.js 24.13.0 or higher
- Vite 7 project (`npm create vite@7 my-app -- --template vanilla-ts`)

The Markdown Editor and Rich Text Editor share the **same npm package**:

```bash
npm install @syncfusion/ej2-richtexteditor
```

---

## Installation

```bash
npm create vite@7 my-app -- --template vanilla-ts
cd my-app
npm install
npm install @syncfusion/ej2-richtexteditor
npm run dev
```

**Package dependency tree:**

```
@syncfusion/ej2-richtexteditor
  ├── @syncfusion/ej2-base
  ├── @syncfusion/ej2-buttons
  ├── @syncfusion/ej2-data
  ├── @syncfusion/ej2-inputs
  ├── @syncfusion/ej2-lists
  ├── @syncfusion/ej2-navigations
  ├── @syncfusion/ej2-popups
  ├── @syncfusion/ej2-splitbuttons
  └── @syncfusion/ej2-filemanager
```

All dependencies are pulled in automatically when you install `@syncfusion/ej2-richtexteditor`.

---

## CSS Imports

Add these to `src/style.css` (tailwind3 theme):

```css
@import '../node_modules/@syncfusion/ej2-base/styles/tailwind3.css';
@import '../node_modules/@syncfusion/ej2-buttons/styles/tailwind3.css';
@import '../node_modules/@syncfusion/ej2-inputs/styles/tailwind3.css';
@import '../node_modules/@syncfusion/ej2-lists/styles/tailwind3.css';
@import '../node_modules/@syncfusion/ej2-navigations/styles/tailwind3.css';
@import '../node_modules/@syncfusion/ej2-popups/styles/tailwind3.css';
@import '../node_modules/@syncfusion/ej2-splitbuttons/styles/tailwind3.css';
@import '../node_modules/@syncfusion/ej2-richtexteditor/styles/tailwind3.css';
```

> Replace `tailwind3` with your theme: `material`, `bootstrap5`, `fabric`, `fluent2`, `tailwind3-dark`, etc. Use the same theme name across all imports.

---

## HTML Editor Setup

The HTML editor is the default — `editorMode: 'HTML'` is implicit.

**`src/main.ts`:**

```typescript
import './style.css';
import { RichTextEditor, Toolbar, Link, Image, HtmlEditor, QuickToolbar } from '@syncfusion/ej2-richtexteditor';

// Inject required modules before instantiation
RichTextEditor.Inject(Toolbar, Link, Image, HtmlEditor, QuickToolbar);

const editor = new RichTextEditor({
  toolbarSettings: {
    items: [
      'Undo', 'Redo', '|',
      'Bold', 'Italic', 'Underline', 'StrikeThrough', '|',
      'FontName', 'FontSize', 'FontColor', 'BackgroundColor', '|',
      'Formats', 'Alignments', '|',
      'OrderedList', 'UnorderedList', '|',
      'CreateLink', 'Image', '|',
      'ClearFormat', 'SourceCode', 'FullScreen'
    ]
  },
  value: '<p>Start editing here...</p>'
});
editor.appendTo('#editor');
```

**`index.html`:**

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Syncfusion Rich Text Editor</title>
  </head>
  <body>
    <div id="editor"></div>
    <script type="module" src="/src/main.ts"></script>
  </body>
</html>
```

---

## Markdown Editor Setup

To activate Markdown mode: set `editorMode: 'Markdown'` and inject `MarkdownEditor` instead of `HtmlEditor`.

**`src/main.ts`:**

```typescript
import './style.css';
import { RichTextEditor, Toolbar, Link, Image, Table, MarkdownEditor } from '@syncfusion/ej2-richtexteditor';

RichTextEditor.Inject(Toolbar, Link, Image, Table, MarkdownEditor);

const editor = new RichTextEditor({
  editorMode: 'Markdown',
  toolbarSettings: {
    items: [
      'Bold', 'Italic', 'StrikeThrough', 'InlineCode',
      'SuperScript', 'SubScript', '|',
      'Formats', 'Blockquote', '|',
      'OrderedList', 'UnorderedList',
      'CreateLink', 'Image', 'CreateTable', '|',
      'Undo', 'Redo'
    ]
  },
  value: `## Welcome\n\nStart writing **Markdown** here.`
});
editor.appendTo('#editor');
```

> The `value` in Markdown mode is a plain markdown string, not HTML.
> The editor outputs markdown — you need a third-party library like [Marked](https://marked.js.org/) to render it as HTML in the preview.

---

## Feature Module Reference

Inject only the modules you need. Each adds functionality and a small bundle cost.

| Feature | Module | Import path |
|---------|--------|-------------|
| HTML editing | `HtmlEditor` | `@syncfusion/ej2-richtexteditor` |
| Markdown editing | `MarkdownEditor` | `@syncfusion/ej2-richtexteditor` |
| Toolbar | `Toolbar` | `@syncfusion/ej2-richtexteditor` |
| Hyperlinks | `Link` | `@syncfusion/ej2-richtexteditor` |
| Images | `Image` | `@syncfusion/ej2-richtexteditor` |
| Audio | `Audio` | `@syncfusion/ej2-richtexteditor` |
| Video | `Video` | `@syncfusion/ej2-richtexteditor` |
| Tables | `Table` | `@syncfusion/ej2-richtexteditor` |
| Context quick toolbar | `QuickToolbar` | `@syncfusion/ej2-richtexteditor` |
| Paste cleanup | `PasteCleanup` | `@syncfusion/ej2-richtexteditor` |
| Character count | `Count` | `@syncfusion/ej2-richtexteditor` |
| Resize handle | `Resize` | `@syncfusion/ej2-richtexteditor` |
| File browser | `FileManager` | `@syncfusion/ej2-richtexteditor` |

**Always inject `Toolbar` and the editor type module (`HtmlEditor` or `MarkdownEditor`) at minimum.**

```typescript
// Minimal HTML editor
RichTextEditor.Inject(Toolbar, HtmlEditor);

// Minimal Markdown editor
RichTextEditor.Inject(Toolbar, MarkdownEditor);

// Full-featured HTML editor
RichTextEditor.Inject(Toolbar, Link, Image, Audio, Video, Table, HtmlEditor, QuickToolbar, PasteCleanup);
```

---

## Initialization on Textarea

RTE can also initialize on a `<textarea>` element. This is useful for form integration — the textarea value is automatically kept in sync.

```html
<form>
  <textarea id="editor"></textarea>
  <button type="submit">Save</button>
</form>
```

```typescript
const editor = new RichTextEditor({ /* options */ });
editor.appendTo('#editor'); // textarea element
```

> On form submit, the textarea value contains the editor's HTML/Markdown content. No extra step needed.

---

## Troubleshooting

**Styles not applied:** Verify all CSS imports use the same theme name and that `ej2-base` is imported first.

**Editor not rendering:** Confirm `RichTextEditor.Inject(...)` is called before `new RichTextEditor(...)`.

**Markdown mode showing HTML tags:** You injected `HtmlEditor` instead of `MarkdownEditor`. They are mutually exclusive.

**TypeScript errors on import:** Ensure `@syncfusion/ej2-richtexteditor` is installed and `tsconfig.json` has `"moduleResolution": "bundler"` or `"node"`.
