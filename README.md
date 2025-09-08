# Sanity Image Text Block Plugin

By **Multidots**

A Sanity Studio v3+ plugin for creating image and text combination blocks with flexible positioning and styling.


## 🚀 Features

- ✅ **Image Positioning** - Left or right side positioning
- ✅ **Rich Content** - Title, subtitle, and rich text descriptions  
- ✅ **Custom Buttons** - Call-to-action buttons with custom styling
- ✅ **Color Customization** - Background, text, and button colors
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
            name: "imageTextBlock",
            type: "ImageTextBlockType",
        }),
  ],
})
```

## ⚛️ Frontend Usage Example (Next.js)

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

### With Custom Styling Example

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

### Environment Variables Example (.env.local)

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

#### Image Fields
- **Main Image** - Image upload with alt text support

#### Styling Options  
- **Image Position** - Choose left or right positioning
- **Content Alignment** - Top or center vertical alignment
- **Background Color** - Content area background color picker
- **Text Color** - Universal text color for all content

#### Button Configuration
- **Button Text** - Call-to-action button label
- **Button Link** - URL destination
- **Open in New Tab** - External link behavior
- **Button Background Color** - Custom button background
- **Button Text Color** - Custom button text color


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

## 📷 Screenshots

### Studio Interface
![Studio Interface](https://github.com/benazeerhassan1909/sanity-image-text-plugin/blob/main/public/studio-interface.png)
### Frontend Output
![Frontend Output Image Left](https://github.com/benazeerhassan1909/sanity-image-text-plugin/blob/main/public/image-left.png)
![Frontend Output Image Right](https://github.com/benazeerhassan1909/sanity-image-text-plugin/blob/main/public/image-right.png)

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

## Develop & test
This plugin uses @sanity/plugin-kit for build & watch scripts.

Local development tips:

- npm run link-watch or npm run watch to develop against a local Studio
- Publish with npm publish (build runs on prepublishOnly)

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
