# Sanity Image Text Block Plugin

By **Multidots**

A Sanity Studio v3+ plugin for creating image and text combination blocks with flexible positioning and styling.

[![npm version](https://badge.fury.io/js/@multidots%2Fsanity-plugin-image-text-block.svg)](https://badge.fury.io/js/@multidots%2Fsanity-plugin-image-text-block)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Downloads](https://img.shields.io/npm/dm/@multidots/sanity-plugin-image-text-block.svg)](https://npmjs.com/package/@multidots/sanity-plugin-image-text-block)

## 🚀 Features

- ✅ **Image Positioning** - Left or right side positioning
- ✅ **Rich Content** - Title, subtitle, and rich text descriptions  
- ✅ **Custom Buttons** - Call-to-action buttons with custom styling
- ✅ **Color Customization** - Background, text, image background, and button colors
- ✅ **Content Alignment** - Top or center alignment options
- ✅ **Responsive Design** - Works on all device sizes
- ✅ **SSR Safe** - No hydration errors
- ✅ **TypeScript Support** - Fully typed

## 📦 Installation

```bash
npm install @multidots/sanity-plugin-image-text-block
```

Or with yarn:

```bash
yarn add @multidots/sanity-plugin-image-text-block
```

## 🔧 Add to Sanity Studio

### 1. Add Plugin to Sanity Config

```ts
import { defineConfig } from 'sanity'
import { ImageTextBlockPlugin } from '@multidots/sanity-plugin-image-text-block'

export default defineConfig({
  // ... other config
  plugins: [
    ImageTextBlockPlugin(),
    // ... other plugins
  ],
})
```

### 2. Use in Schema

```ts
// schemas/page.ts
import { defineType, defineField } from 'sanity'

export default defineType({
  name: 'page',
  title: 'Page',
  type: 'document',
  fields: [
    defineField({
      name: 'content',
      title: 'Page Content',
      type: 'array',
      of: [
        { type: 'ImageTextBlockType' },
        // ... other block types
      ],
    }),
  ],
})
```

## ⚛️ Frontend Usage (Next.js)

### Basic Implementation

```tsx
// pages/[slug].tsx or app/[slug]/page.tsx
import { ImageTextBlock } from '@multidots/sanity-plugin-image-text-block'
import { createClient } from '@sanity/client'

const sanityClient = createClient({
  projectId: process.env.NEXT_PUBLIC_SANITY_PROJECT_ID,
  dataset: process.env.NEXT_PUBLIC_SANITY_DATASET,
  useCdn: true,
  apiVersion: '2023-05-03',
})

export default function Page({ pageData }) {
  return (
    <main>
      {pageData.content?.map((block, index) => {
        if (block._type === 'ImageTextBlockType') {
          return (
            <ImageTextBlock
              key={index}
              title={block.title}
              subTitle={block.subTitle}
              description={block.description}
              mainImage={block.mainImage}
              imagePosition={block.imagePosition}
              button={block.button}
              backgroundColor={block.backgroundColor}
              imageBackgroundColor={block.imageBackgroundColor}
              textColor={block.textColor}
              contentAlignment={block.contentAlignment}
              sanityClient={sanityClient}
            />
          )
        }
        return null
      })}
    </main>
  )
}
```

### With Custom Styling

```tsx
import { ImageTextBlock } from '@multidots/sanity-plugin-image-text-block'

// Custom wrapper component
function CustomImageTextBlock(props) {
  return (
    <div className="my-custom-wrapper">
      <ImageTextBlock
        {...props}
        backgroundColor={{ hex: '#f8f9fa' }}
        textColor={{ hex: '#333333' }}
        button={{
          ...props.button,
          buttonBackgroundColor: { hex: '#007bff' },
          buttonTextColor: { hex: '#ffffff' }
        }}
      />
    </div>
  )
}
```

### Environment Variables (.env.local)

```bash
NEXT_PUBLIC_SANITY_PROJECT_ID=your-project-id
NEXT_PUBLIC_SANITY_DATASET=production
```

## 🎛️ Plugin Settings and Screenshots

### Available Fields in Sanity Studio

#### Content Fields
- **Title** - Main heading text
- **Subtitle** - Secondary heading text
- **Description** - Rich text content with formatting options
- **Main Image** - Image upload with alt text support
- **Image Position** - Choose left or right positioning
- **Content Alignment** - Top or center vertical alignment

#### Styling Options  
- **Background Color** - Content area background color picker
- **Image Background Color** - Image area background color picker  
- **Text Color** - Universal text color for all content

#### Button Configuration
- **Button Text** - Call-to-action button label
- **Button Link** - URL destination
- **Open in New Tab** - External link behavior
- **Button Background Color** - Custom button background
- **Button Text Color** - Custom button text color

### Studio Interface

*Note: The plugin provides an intuitive interface in Sanity Studio with:*
- Visual color pickers for all color options
- Image upload with drag-and-drop support
- Rich text editor for descriptions
- Toggle options for positioning and alignment
- Real-time preview of settings

### Frontend Output Examples

**Left Image Position:**
```
[IMAGE]  |  Title
         |  Subtitle  
         |  Description text here...
         |  [Button]
```

**Right Image Position:**
```
Title          |  [IMAGE]
Subtitle       |
Description... |
[Button]       |
```

## 🐛 Troubleshooting

### Images not displaying
**Problem:** Images appear broken or don't load
**Solution:** Ensure you're passing the Sanity client properly:
```tsx
<ImageTextBlock 
  {...blockData} 
  sanityClient={sanityClient} 
/>
```

### Hydration mismatch errors
**Problem:** Next.js hydration errors in development
**Solution:** Update to the latest version:
```bash
npm update @multidots/sanity-plugin-image-text-block
```

### TypeScript errors
**Problem:** Type errors when using the component
**Solution:** Import proper types:
```tsx
import type { SanityImageSource } from '@sanity/image-url/lib/types/types'
```

### Button links not working
**Problem:** Button clicks don't navigate
**Solution:** Ensure the link field includes protocol:
```
✅ https://example.com
✅ /internal-page
❌ example.com (missing protocol)
```

### Colors not applying
**Problem:** Custom colors not showing in frontend
**Solution:** Check that color values are in the correct format:
```tsx
// Correct format
backgroundColor={{ hex: '#ffffff' }}

// Incorrect format  
backgroundColor="#ffffff"
```

### Responsive issues
**Problem:** Layout breaks on mobile
**Solution:** The plugin is responsive by default, but ensure your container has proper CSS:
```css
.page-container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 1rem;
}
```

## 📤 Exports

The plugin exports the following components and types:

### Main Exports
```tsx
// Plugin for Sanity Studio
export const ImageTextBlockPlugin: PluginOptions

// React component for frontend
export const ImageTextBlock: React.FC<ImageTextBlockProps>

// Default export (plugin)
export default ImageTextBlockPlugin
```

### TypeScript Interfaces
```tsx
interface ImageTextBlockProps {
  title?: string
  subTitle?: string
  description?: any // Rich text from Sanity
  mainImage?: SanityImageSource & {
    alt?: string
  }
  imagePosition?: 'left' | 'right'
  backgroundColor?: {
    hex: string
  }
  imageBackgroundColor?: {
    hex: string
  }
  textColor?: {
    hex: string
  }
  contentAlignment?: 'top' | 'center'
  button?: {
    text: string
    link: string
    openInNewTab: boolean
    buttonBackgroundColor?: {
      hex: string
    }
    buttonTextColor?: {
      hex: string
    }
  }
  sanityClient?: any // Sanity client for image URL generation
}
```

### Usage Examples
```tsx
// Import plugin for Sanity config
import { ImageTextBlockPlugin } from '@multidots/sanity-plugin-image-text-block'

// Import component for React/Next.js
import { ImageTextBlock } from '@multidots/sanity-plugin-image-text-block'

// Import default (same as plugin)
import ImageTextBlockPlugin from '@multidots/sanity-plugin-image-text-block'
```

---

## Contributor

### Multidots

**Enterprise Web Agency · Inc. 5000 Company**

Multidots is located at Austin, Texas, US

[Visit Multidots's profile](https://multidots.com)

## Useful links

* [NPM](https://www.npmjs.com/package/@multidots/sanity-plugin-image-text-block)
* [GitHub](https://github.com/multidots/sanity-plugin-image-text-block)

---

## 📄 License

[MIT](LICENSE) © [Multidots](https://multidots.com)

**Made with ❤️ by [Multidots](https://multidots.com)**