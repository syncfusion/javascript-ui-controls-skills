# Advanced Features

Explore JWT authentication, localization, image preview/resize, and complex upload workflows.

## Table of Contents
- [JWT Authentication](#jwt-authentication)
- [Localization & Multi-Language](#localization--multi-language)
- [File Preview](#file-preview)
- [Image Resizing](#image-resizing)
- [Template Customization](#template-customization)
- [Form Data Binding](#form-data-binding)
- [File Removal & Cleanup](#file-removal--cleanup)

---

## JWT Authentication

### Secure Upload with Bearer Token

```typescript
import { Uploader, UploadingEventArgs } from '@syncfusion/ej2-inputs';

class SecureUploader {
  private uploader: Uploader;
  private jwtToken: string = '';

  constructor() {
    this.jwtToken = this.getJWTToken(); // Get from localStorage or API
    
    this.uploader = new Uploader({
      asyncSettings: {
        saveUrl: 'https://api.example.com/upload',
        removeUrl: 'https://api.example.com/remove'
      }
    });
    this.uploader.appendTo('#fileupload');
    this.attachAuthHandler();
  }

  private getJWTToken(): string {
    // Get JWT from localStorage or session
    return localStorage.getItem('jwtToken') || '';
  }

  private attachAuthHandler(): void {
    // Add JWT token to upload request
    this.uploader.uploading = (args: UploadingEventArgs) => {
      // Set custom header with JWT
      if (this.uploader.xhr) {
        this.uploader.xhr.setRequestHeader('Authorization', `Bearer ${this.jwtToken}`);
      }
    };

    // Handle 401 Unauthorized (token expired)
    this.uploader.failure = (args: any) => {
      if (args.statusCode === 401) {
        console.log('Token expired, refreshing...');
        this.refreshToken();
      }
    };
  }

  private refreshToken(): void {
    fetch('https://api.example.com/refresh-token', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        refreshToken: localStorage.getItem('refreshToken')
      })
    })
    .then(res => res.json())
    .then(data => {
      this.jwtToken = data.accessToken;
      localStorage.setItem('jwtToken', data.accessToken);
      // Retry upload
      this.uploader.upload(this.uploader.getFilesData());
    })
    .catch(error => console.error('Token refresh failed:', error));
  }
}

// Usage
new SecureUploader();
```

### Server-Side Token Validation

```typescript
// Express.js example
import express from 'express';
import jwt from 'jsonwebtoken';

app.post('/api/FileUploader/Save', (req, res) => {
  // Verify JWT
  const token = req.headers.authorization?.split(' ')[1];
  
  try {
    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    console.log('User:', decoded.userId);
    
    // Process upload
    // Save file to storage
    res.json({ status: 'File uploaded successfully' });
  } catch (error) {
    res.status(401).json({ error: 'Invalid token' });
  }
});
```

---

## Localization & Multi-Language

### Setup Localization

```typescript
import { Uploader } from '@syncfusion/ej2-inputs';
import { L10n } from '@syncfusion/ej2-base';

// Define custom localization strings
L10n.load({
  'en': {
    'uploader': {
      'Browse': 'Choose File',
      'Clear': 'Clear All',
      'Upload': 'Upload Files',
      'dropFilesHint': 'Drop files here to upload',
      'invalidFileType': 'File type not supported'
    }
  },
  'es': {
    'uploader': {
      'Browse': 'Elegir archivo',
      'Clear': 'Limpiar todo',
      'Upload': 'Cargar archivos',
      'dropFilesHint': 'Suelta los archivos aquí para cargar',
      'invalidFileType': 'Tipo de archivo no compatible'
    }
  },
  'fr': {
    'uploader': {
      'Browse': 'Choisir un fichier',
      'Clear': 'Effacer tout',
      'Upload': 'Télécharger les fichiers',
      'dropFilesHint': 'Déposez les fichiers ici pour télécharger',
      'invalidFileType': 'Type de fichier non pris en charge'
    }
  },
  'ar': {
    'uploader': {
      'Browse': 'اختر ملف',
      'Clear': 'مسح الكل',
      'Upload': 'رفع الملفات',
      'dropFilesHint': 'اسحب الملفات هنا للتحميل',
      'invalidFileType': 'نوع الملف غير مدعوم'
    }
  }
});

// Create uploader with Spanish locale
const uploader = new Uploader({
  asyncSettings: {
    saveUrl: 'https://api.example.com/upload',
    removeUrl: 'https://api.example.com/remove'
  },
  locale: 'es'  // Spanish
});
uploader.appendTo('#fileupload');
```

### Language Switcher

```typescript
class MultiLanguageUploader {
  private uploader: Uploader;
  private currentLocale = 'en';

  constructor() {
    this.uploader = new Uploader({
      asyncSettings: {
        saveUrl: 'https://api.example.com/upload',
        removeUrl: 'https://api.example.com/remove'
      },
      locale: 'en'
    });
    this.uploader.appendTo('#fileupload');
    this.setupLanguageSwitcher();
  }

  private setupLanguageSwitcher(): void {
    document.querySelectorAll('.language-selector').forEach(btn => {
      btn.addEventListener('click', (e: any) => {
        const locale = e.target.dataset.locale;
        this.switchLanguage(locale);
      });
    });
  }

  private switchLanguage(locale: string): void {
    this.currentLocale = locale;
    this.uploader.locale = locale;
    // Recreate component to apply new locale
    this.uploader.destroy();
    this.uploader = new Uploader({
      asyncSettings: {
        saveUrl: 'https://api.example.com/upload',
        removeUrl: 'https://api.example.com/remove'
      },
      locale: locale
    });
    this.uploader.appendTo('#fileupload');
  }
}

// Usage
new MultiLanguageUploader();
```

HTML:
```html
<div>
  <button class="language-selector" data-locale="en">English</button>
  <button class="language-selector" data-locale="es">Español</button>
  <button class="language-selector" data-locale="fr">Français</button>
  <button class="language-selector" data-locale="ar">العربية</button>
</div>

<input type="file" id="fileupload" />
```

---

## File Preview

### Image Preview Before Upload

```typescript
import { Uploader } from '@syncfusion/ej2-inputs';

class ImagePreviewUploader {
  private uploader: Uploader;

  constructor() {
    this.uploader = new Uploader({
      asyncSettings: {
        saveUrl: 'https://api.example.com/upload',
        removeUrl: 'https://api.example.com/remove'
      },
      allowedExtensions: '.jpg,.jpeg,.png,.gif,.webp'
    });
    this.uploader.appendTo('#fileupload');
    this.attachPreviewHandler();
  }

  private attachPreviewHandler(): void {
    this.uploader.selected = (args: any) => {
      const previewContainer = document.getElementById('preview');
      if (previewContainer) {
        previewContainer.innerHTML = ''; // Clear previous previews
      }

      args.filesData.forEach((file: any) => {
        const reader = new FileReader();

        reader.onload = (e: any) => {
          const preview = document.createElement('div');
          preview.className = 'image-preview';
          preview.innerHTML = `
            <img src="${e.target.result}" alt="${file.name}" />
            <p>${file.name}</p>
            <small>${(file.size / 1024).toFixed(2)} KB</small>
          `;
          
          previewContainer?.appendChild(preview);
        };

        reader.readAsDataURL(file.rawFile);
      });
    };
  }
}

new ImagePreviewUploader();
```

HTML:
```html
<input type="file" id="fileupload" accept="image/*" multiple />
<div id="preview" class="preview-container"></div>
```

CSS:
```css
.preview-container {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
  gap: 15px;
  margin-top: 20px;
}

.image-preview {
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 10px;
  text-align: center;
  background: #f9f9f9;
}

.image-preview img {
  max-width: 100%;
  max-height: 120px;
  margin-bottom: 10px;
}

.image-preview p {
  margin: 5px 0;
  font-weight: 600;
  word-break: break-word;
}

.image-preview small {
  color: #999;
}
```

### Document Preview

```typescript
uploader.selected = (args: any) => {
  args.filesData.forEach((file: any) => {
    // For PDF files
    if (file.type === 'application/pdf') {
      const reader = new FileReader();
      reader.onload = (e: any) => {
        // Use PDF.js library
        const loadingTask = pdfjsLib.getDocument(e.target.result);
        loadingTask.promise.then((pdf: any) => {
          console.log(`PDF loaded: ${pdf.numPages} pages`);
        });
      };
      reader.readAsArrayBuffer(file.rawFile);
    }
  });
};
```

---

## Image Resizing

### Resize Images Before Upload

```typescript
import { Uploader, SelectedEventArgs } from '@syncfusion/ej2-inputs';

class ImageResizeUploader {
  private uploader: Uploader;
  private maxWidth = 800;
  private maxHeight = 600;
  private quality = 0.8;

  constructor() {
    this.uploader = new Uploader({
      asyncSettings: {
        saveUrl: 'https://api.example.com/upload',
        removeUrl: 'https://api.example.com/remove'
      },
      allowedExtensions: '.jpg,.jpeg,.png'
    });
    this.uploader.appendTo('#fileupload');
    this.attachResizeHandler();
  }

  private attachResizeHandler(): void {
    this.uploader.selected = async (args: SelectedEventArgs) => {
      const resizedFiles = [];

      for (const file of args.filesData) {
        if (file.type.startsWith('image/')) {
          const resized = await this.resizeImage(file.rawFile);
          resizedFiles.push(resized);
        } else {
          resizedFiles.push(file.rawFile);
        }
      }

      // Replace files with resized versions
      args.modifiedFilesData = resizedFiles.map((file, index) => ({
        name: args.filesData[index].name,
        rawFile: file,
        size: file.size,
        type: file.type
      }));
    };
  }

  private resizeImage(file: File): Promise<Blob> {
    return new Promise((resolve, reject) => {
      const reader = new FileReader();

      reader.onload = (e: any) => {
        const img = new Image();
        img.onload = () => {
          // Calculate new dimensions
          let width = img.width;
          let height = img.height;

          if (width > height) {
            if (width > this.maxWidth) {
              height = (height * this.maxWidth) / width;
              width = this.maxWidth;
            }
          } else {
            if (height > this.maxHeight) {
              width = (width * this.maxHeight) / height;
              height = this.maxHeight;
            }
          }

          // Create canvas and resize
          const canvas = document.createElement('canvas');
          canvas.width = width;
          canvas.height = height;

          const ctx = canvas.getContext('2d');
          ctx?.drawImage(img, 0, 0, width, height);

          canvas.toBlob(
            (blob: Blob | null) => {
              if (blob) {
                resolve(blob);
              } else {
                reject(new Error('Image resize failed'));
              }
            },
            file.type,
            this.quality
          );
        };

        img.onerror = () => reject(new Error('Image load failed'));
        img.src = e.target.result;
      };

      reader.readAsDataURL(file);
    });
  }
}

new ImageResizeUploader();
```

---

## Template Customization

### Advanced File Item Template

```typescript
const uploader = new Uploader({
  asyncSettings: {
    saveUrl: 'https://api.example.com/upload',
    removeUrl: 'https://api.example.com/remove'
  },
  template: `
    <span class="file-item" data-file-name="\${name}">
      <span class="file-icon">
        <i class="icon-\${getFileType(\${name})}"></i>
      </span>
      <span class="file-details">
        <span class="file-name">\${name}</span>
        <span class="file-size">\${size} bytes</span>
        <div class="progress-container">
          <div class="progress-bar"></div>
          <span class="progress-text">0%</span>
        </div>
      </span>
      <span class="file-status">
        <span class="status-icon"></span>
      </span>
      <button class="btn-remove" id="file_remove"></button>
    </span>
  `
});
```

---

## Form Data Binding

### Integrate with Form Submission

```typescript
import { Uploader } from '@syncfusion/ej2-inputs';

class FormWithUpload {
  private uploader: Uploader;

  constructor() {
    this.uploader = new Uploader({
      asyncSettings: {
        saveUrl: 'https://api.example.com/upload',
        removeUrl: 'https://api.example.com/remove'
      },
      autoUpload: false
    });
    this.uploader.appendTo('#fileupload');
    this.setupFormSubmission();
  }

  private setupFormSubmission(): void {
    document.getElementById('submitForm')?.addEventListener('click', (e: Event) => {
      e.preventDefault();
      this.submitFormWithFiles();
    });
  }

  private async submitFormWithFiles(): Promise<void> {
    const form = document.getElementById('myForm') as HTMLFormElement;
    const formData = new FormData(form);

    // Add files from uploader
    const files = this.uploader.getFilesData();
    files.forEach((file: any, index: number) => {
      formData.append(`attachments[${index}]`, file.rawFile);
    });

    try {
      const response = await fetch('https://api.example.com/submit-form', {
        method: 'POST',
        body: formData
      });

      if (response.ok) {
        const result = await response.json();
        console.log('Form submitted successfully:', result);
        alert('Form submitted successfully!');
      } else {
        throw new Error(`Server error: ${response.status}`);
      }
    } catch (error) {
      console.error('Form submission failed:', error);
      alert('Form submission failed. Please try again.');
    }
  }
}

new FormWithUpload();
```

HTML:
```html
<form id="myForm">
  <input type="text" name="fullName" placeholder="Full Name" required />
  <input type="email" name="email" placeholder="Email" required />
  <textarea name="message" placeholder="Message" required></textarea>
  
  <label>Attachments:</label>
  <input type="file" id="fileupload" name="attachments" multiple />
  
  <button type="button" id="submitForm">Submit Form</button>
</form>
```

---

## File Removal & Cleanup

### Handle File Removal

```typescript
const uploader = new Uploader({
  asyncSettings: {
    saveUrl: 'https://api.example.com/upload',
    removeUrl: 'https://api.example.com/remove'
  }
});
uploader.appendTo('#fileupload');

// When file is removed
uploader.removed = (args: any) => {
  console.log('Files removed:', args.removedFilesData);
  
  args.removedFilesData.forEach((file: any) => {
    console.log(`Removed: ${file.name} (${file.size} bytes)`);
  });
};

// When all files are cleared
uploader.onCleared = () => {
  console.log('All files cleared');
};
```

### Clear All Files Programmatically

```typescript
// Clear all files
document.getElementById('clearAllBtn')?.addEventListener('click', () => {
  uploader.clearAll();
  console.log('All files cleared');
});

// Remove specific file
function removeFileByName(fileName: string): void {
  const files = uploader.getFilesData();
  const fileToRemove = files.find(f => f.name === fileName);
  
  if (fileToRemove) {
    uploader.remove(fileToRemove);
  }
}
```

---

## Next Steps

- **For styling**: Read [customization-and-styling.md](customization-and-styling.md)
- **For troubleshooting**: Read [troubleshooting-and-examples.md](troubleshooting-and-examples.md)
- **For validation**: Read [file-validation.md](file-validation.md)
