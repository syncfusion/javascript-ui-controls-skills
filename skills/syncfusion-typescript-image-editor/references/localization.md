# Localization

## Table of Contents
- [Language Support](#language-support)
- [Setting Locale](#setting-locale)
- [Custom Translations](#custom-translations)
- [RTL Support](#rtl-support)
- [Common Patterns](#common-patterns)

## Language Support

### Supported Languages

The Image Editor supports multiple languages out of the box:

```typescript
import { ImageEditor } from '@syncfusion/ej2-image-editor';

// Supported locales:
const supportedLanguages = [
    'en-US',      // English (United States)
    'en',         // English (default)
    'de',         // German
    'fr',         // French
    'es',         // Spanish
    'it',         // Italian
    'ja',         // Japanese
    'ko',         // Korean
    'zh',         // Chinese (Simplified)
    'zh-TW',      // Chinese (Traditional)
    'ru',         // Russian
    'ar',         // Arabic
    'he',         // Hebrew
    'pt',         // Portuguese
    'pt-BR',      // Portuguese (Brazil)
    'tr',         // Turkish
    'ur',         // Urdu
    'vi',         // Vietnamese
    'id',         // Indonesian
    'th',         // Thai
    'sw',         // Swedish
    'da',         // Danish
    'nl',         // Dutch
    'fi',         // Finnish
    'nb',         // Norwegian
    'pl',         // Polish
    'hu',         // Hungarian
    'cs',         // Czech
    'ro',         // Romanian
    'el',         // Greek
    'tr',         // Turkish
];
```

### Language-Specific Features

Different languages have different text directions and formatting:

```typescript
// Left-to-Right (LTR) languages
const ltrLanguages = ['en', 'de', 'fr', 'es', 'ja', 'ko', 'zh'];

// Right-to-Left (RTL) languages
const rtlLanguages = ['ar', 'he', 'fa', 'ur'];
```

## Setting Locale

### Basic Locale Setting

```typescript
import { ImageEditor } from '@syncfusion/ej2-image-editor';

const imageEditor = new ImageEditor({
    width: '550px',
    height: '330px',
    locale: 'fr'  // French
});

imageEditor.appendTo('#imageeditor');
```

### Common Locale Examples

```typescript
// English (US)
imageEditor.locale = 'en-US';

// German
imageEditor.locale = 'de';

// Spanish
imageEditor.locale = 'es';

// Japanese
imageEditor.locale = 'ja';

// Arabic (with RTL)
imageEditor.locale = 'ar';
imageEditor.enableRtl = true;

// Chinese (Simplified)
imageEditor.locale = 'zh';

// Portuguese (Brazil)
imageEditor.locale = 'pt-BR';
```

### Dynamic Locale Change

```typescript
function changeLanguage(languageCode: string) {
    imageEditor.locale = languageCode;
    
    // If RTL language, enable RTL
    const rtlLanguages = ['ar', 'he', 'fa', 'ur'];
    imageEditor.enableRtl = rtlLanguages.includes(languageCode);
    
    // Refresh UI
    imageEditor.refresh();
}

// Language selector
document.getElementById('languageSelect').addEventListener('change', (e) => {
    changeLanguage((e.target as HTMLSelectElement).value);
});
```

## Custom Translations

### Load Custom Locale Data

```typescript
import { L10n } from '@syncfusion/ej2-base';

// Define custom translations
L10n.load({
    'en-US': {
        'image-editor': {
            'open': 'Open Image',
            'save': 'Save Image',
            'crop': 'Crop',
            'rotate': 'Rotate',
            'flip': 'Flip',
            'undo': 'Undo',
            'redo': 'Redo',
            'reset': 'Reset',
            'zoom': 'Zoom',
            'filter': 'Filter',
            'annotation': 'Add Text',
            'shape': 'Add Shape',
            'freehand': 'Draw'
        }
    },
    'es': {
        'image-editor': {
            'open': 'Abrir imagen',
            'save': 'Guardar imagen',
            'crop': 'Cortar',
            'rotate': 'Girar',
            'flip': 'Voltear',
            'undo': 'Deshacer',
            'redo': 'Rehacer',
            'reset': 'Reiniciar',
            'zoom': 'Zoom',
            'filter': 'Filtro',
            'annotation': 'Añadir texto',
            'shape': 'Añadir forma',
            'freehand': 'Dibujar'
        }
    }
});

const imageEditor = new ImageEditor({
    locale: 'es'
});
```

### Add Translation for New Language

```typescript
import { L10n } from '@syncfusion/ej2-base';

// Add Japanese translation
L10n.load({
    'ja': {
        'image-editor': {
            'open': '画像を開く',
            'save': '画像を保存',
            'crop': 'トリミング',
            'rotate': '回転',
            'flip': '反転',
            'undo': '元に戻す',
            'redo': 'やり直す',
            'reset': 'リセット',
            'zoom': 'ズーム',
            'filter': 'フィルター',
            'annotation': 'テキスト追加',
            'shape': 'シェイプ追加',
            'freehand': '描画'
        }
    }
});

const imageEditor = new ImageEditor({
    locale: 'ja'
});
```

### Override Existing Translations

```typescript
import { L10n } from '@syncfusion/ej2-base';

// Get current translations for English
const enTranslations = L10n.load().get('en-US')['image-editor'];

// Modify specific terms
L10n.load({
    'en-US': {
        'image-editor': {
            ...enTranslations,
            'save': 'Export Image',      // Override
            'reset': 'Revert Changes'     // Override
        }
    }
});
```

## RTL Support

### Enable RTL

```typescript
import { ImageEditor } from '@syncfusion/ej2-image-editor';

const imageEditor = new ImageEditor({
    width: '550px',
    height: '330px',
    locale: 'ar',          // Arabic language
    enableRtl: true        // Enable RTL layout
});

imageEditor.appendTo('#imageeditor');
```

### RTL with Different Locales

```typescript
// Arabic
imageEditor.locale = 'ar';
imageEditor.enableRtl = true;

// Hebrew
imageEditor.locale = 'he';
imageEditor.enableRtl = true;

// Persian
imageEditor.locale = 'fa';
imageEditor.enableRtl = true;

// Urdu
imageEditor.locale = 'ur';
imageEditor.enableRtl = true;
```

### RTL CSS

```html
<!DOCTYPE html>
<html dir="rtl">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Image Editor RTL</title>
    <link href="styles/material.css" rel="stylesheet" />
    <style>
        body { direction: rtl; }
        .e-image-editor { direction: rtl; }
    </style>
</head>
<body>
    <div id="imageeditor"></div>
</body>
</html>
```

### Dynamic RTL Toggle

```typescript
function toggleRTL(enable: boolean) {
    imageEditor.enableRtl = enable;
    
    if (enable) {
        document.documentElement.dir = 'rtl';
    } else {
        document.documentElement.dir = 'ltr';
    }
    
    imageEditor.refresh();
}

document.getElementById('rtlToggle').addEventListener('change', (e) => {
    toggleRTL((e.target as HTMLInputElement).checked);
});
```

## Common Patterns

### Pattern: Language Selector

```typescript
function createLanguageSelector() {
    const languages = [
        { code: 'en-US', name: 'English' },
        { code: 'es', name: 'Español' },
        { code: 'fr', name: 'Français' },
        { code: 'de', name: 'Deutsch' },
        { code: 'ja', name: '日本語' },
        { code: 'zh', name: '中文' },
        { code: 'ar', name: 'العربية' },
        { code: 'pt-BR', name: 'Português (BR)' }
    ];
    
    const select = document.createElement('select');
    select.id = 'languageSelector';
    
    languages.forEach(lang => {
        const option = document.createElement('option');
        option.value = lang.code;
        option.textContent = lang.name;
        select.appendChild(option);
    });
    
    select.addEventListener('change', (e) => {
        const code = (e.target as HTMLSelectElement).value;
        imageEditor.locale = code;
        
        // Enable RTL for Arabic, Hebrew
        imageEditor.enableRtl = ['ar', 'he', 'fa'].includes(code);
        
        imageEditor.refresh();
    });
    
    document.getElementById('controls').appendChild(select);
}
```

### Pattern: Detect Browser Language

```typescript
function autoDetectLanguage() {
    const browserLanguage = navigator.language || 'en-US';
    
    // Map browser language to supported locales
    const languageMap = {
        'de': 'de',
        'fr': 'fr',
        'es': 'es',
        'it': 'it',
        'ja': 'ja',
        'zh': 'zh',
        'ar': 'ar',
        'pt': 'pt-BR'
    };
    
    const languageCode = browserLanguage.split('-')[0];
    const locale = languageMap[languageCode] || 'en-US';
    
    imageEditor.locale = locale;
    imageEditor.enableRtl = ['ar', 'he', 'fa'].includes(locale);
}

autoDetectLanguage();
```

### Pattern: Multi-Language Application

```typescript
class MultiLanguageApp {
    private imageEditor: ImageEditor;
    private currentLocale: string = 'en-US';
    
    constructor() {
        this.imageEditor = new ImageEditor({
            width: '550px',
            height: '330px',
            locale: this.currentLocale
        });
    }
    
    setLanguage(locale: string) {
        this.currentLocale = locale;
        this.imageEditor.locale = locale;
        
        // Update RTL
        const rtlLanguages = ['ar', 'he', 'fa', 'ur'];
        this.imageEditor.enableRtl = rtlLanguages.includes(locale);
        
        // Save preference
        localStorage.setItem('preferredLanguage', locale);
        
        this.imageEditor.refresh();
    }
    
    getSavedLanguage() {
        return localStorage.getItem('preferredLanguage') || 'en-US';
    }
    
    loadSavedLanguage() {
        const saved = this.getSavedLanguage();
        this.setLanguage(saved);
    }
}

const app = new MultiLanguageApp();
app.loadSavedLanguage();
```

### Pattern: Locale Fallback

```typescript
function setLocaleWithFallback(preferredLocale: string) {
    const supported = ['en-US', 'es', 'fr', 'de', 'ja', 'zh', 'ar'];
    
    // Try exact match first
    if (supported.includes(preferredLocale)) {
        imageEditor.locale = preferredLocale;
        return;
    }
    
    // Try language prefix
    const languageCode = preferredLocale.split('-')[0];
    const fallback = supported.find(l => l.startsWith(languageCode));
    
    if (fallback) {
        imageEditor.locale = fallback;
        return;
    }
    
    // Default to English
    imageEditor.locale = 'en-US';
}

setLocaleWithFallback('pt-PT');  // Falls back to 'pt-BR'
```

