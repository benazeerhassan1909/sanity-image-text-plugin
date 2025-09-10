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

### 1. Add Field to Your Query

First, add the `imageTextBlock` field to your page or post query:

```tsx
// lib/sanity/queries.ts
export const getPageQuery = defineQuery(`
  *[_type == 'page' && slug.current == $slug][0]{
    _id,
    _type,
    name,
    slug,
    imageTextBlock,
  }
`);
```

### 2. Create ImageText Wrapper Component

Create a wrapper component to handle the data transformation:

```tsx
// components/ImageText.tsx
'use client'

import { ImageTextBlock } from "@multidots/sanity-plugin-image-text-block"
import { PortableTextBlock } from "sanity"
import type { SanityImageSource } from '@sanity/image-url/lib/types/types'
import { client } from '@/lib/sanity/client/client'

interface ImageTextProps {
    title?: string
    subTitle?: string
    description?: PortableTextBlock[] | string | null
    mainImage?: SanityImageSource
    imagePosition?: "left" | "right"
    backgroundColor?: {
        hex: string
    }
    textColor?: {
        hex: string
    }
    contentAlignment?: 'top' | 'center'
    button?: {
        text?: string
        link?: string
        openInNewTab?: boolean
        buttonBackgroundColor?: {
            hex: string
        }
        buttonTextColor?: {
            hex: string
        }
    }
}

function portableTextToPlainText(value: PortableTextBlock[] | string | null | undefined): string {
    if (!value) return ''
    if (typeof value === 'string') return value
    try {
        return value
            .map(block => {
                if (block?._type !== 'block' || !('children' in block)) return ''
                // @ts-expect-error: children exists on block content
                return (block.children || []).map((child: any) => child.text).join('')
            })
            .join('\n')
    } catch {
        return ''
    }
}

export const ImageText = ({ mainImage, title, subTitle, description, imagePosition, button, backgroundColor, textColor, contentAlignment }: ImageTextProps) => {
    const descriptionText = portableTextToPlainText(description)
    const normalizedButton = {
        text: button?.text ?? '',
        link: button?.link ?? '',
        openInNewTab: button?.openInNewTab ?? false,
        buttonBackgroundColor: button?.buttonBackgroundColor ?? { hex: '#667eea' },
        buttonTextColor: button?.buttonTextColor ?? { hex: '#ffffff' },
        }
    return (
        <ImageTextBlock
            title={title || ''}
            subTitle={subTitle || ''}
            description={descriptionText}
            mainImage={mainImage}
            imagePosition={imagePosition}
            button={normalizedButton}
            backgroundColor={backgroundColor}
            textColor={textColor}
            contentAlignment={contentAlignment}
            sanityClient={client}
        />
    )
}
```

### 3. Use in Your Page Component

```tsx
// app/[slug]/page.tsx
import { ImageText } from "@/components/ImageText";

export default function Page({ page }) {
  return (
    <main>
      {page?.imageTextBlock && (
        <ImageText
          title={page.imageTextBlock.title}
          subTitle={page.imageTextBlock.subTitle}
          description={page.imageTextBlock.description}
          mainImage={page.imageTextBlock.mainImage}
          imagePosition={page.imageTextBlock.imagePosition}
          button={page.imageTextBlock.button}
          backgroundColor={page.imageTextBlock.backgroundColor?.hex ? { hex: page.imageTextBlock.backgroundColor.hex } : undefined}
          textColor={page.imageTextBlock.textColor?.hex ? { hex: page.imageTextBlock.textColor.hex } : undefined}
          contentAlignment={page.imageTextBlock.contentAlignment}
        />
      )}
    </main>
  )
}
```

### 4. Environment Variables (.env.local)

Make sure to set up your environment variables:

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

Backend Settings: https://share.cleanshot.com/mfSSvb0FXGR7TmVspQSM

Frontend Output Image Left: https://share.cleanshot.com/jV7DnbbKvw3bmTsWXDNB 

Frontend Output Image Right: https://share.cleanshot.com/Dqkrdrd2ywkLCkzzwFCS 

### Studio Interface
![Studio Interface](https://github.com/benazeerhassan1909/sanity-image-text-plugin/blob/main/public/backend-settings.jpeg)
### Frontend Output
![Frontend Output Image Left](https://github.com/benazeerhassan1909/sanity-image-text-plugin/blob/main/public/image-left.png)
![Frontend Output Image Right](https://github.com/benazeerhassan1909/sanity-image-text-plugin/blob/main/public/image-right.png)

## Demo Video

[![Demo Video](https://github.com/benazeerhassan1909/sanity-image-text-plugin/blob/main/public/Image-Text-Block-Plugin.mp4)](https://github.com/benazeerhassan1909/sanity-image-text-plugin/blob/main/public/Image-Text-Block-Plugin.mp4)


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

[Visit Multidots's profile](https://multidots.com)

---

## 📄 License

[MIT](LICENSE) © [Multidots](https://multidots.com)
