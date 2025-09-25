# Astro Static Site Generator Migration Specification

## Project Overview

Convert the existing static HTML site with multiple style versions into an Astro-powered static site generator that allows users to select their preferred style from the home page and applies it across all pages while maintaining a single source of truth for content updates.

## Current State Analysis

### Existing Style Variants
- **v1-cyberpunk** - External CSS + test file
- **v2-nature** - External CSS + test file  
- **v3-ocean** - External CSS + test file
- **v4-sunset** - External CSS + test file
- **v5-minimal** - External CSS + test file
- **v6-neon** - External CSS + test file
- **v7-geometric** - Embedded styling in HTML
- **v8-particles** - Embedded styling in HTML
- **v9-matrix** - Embedded styling in HTML
- **v10-corporate** - External CSS + test file
- **v11-starfield** - Embedded styling in HTML
- **v12-parallax** - Embedded styling in HTML

### Current Issues
- Content duplication across multiple HTML files
- Manual updates required for each style variant
- Inconsistent styling implementation (external CSS vs embedded)
- No centralized content management

## Target Architecture

### Project Structure
```
elmstop/
├── src/
│   ├── components/
│   │   ├── Header.astro
│   │   ├── Services.astro
│   │   ├── WhyChoose.astro
│   │   ├── Contact.astro
│   │   ├── Footer.astro
│   │   └── StyleSelector.astro
│   ├── layouts/
│   │   └── BaseLayout.astro
│   ├── pages/
│   │   ├── index.astro (home with style selector)
│   │   ├── [style].astro (dynamic style pages)
│   │   └── tid-bits.astro
│   ├── data/
│   │   └── content.ts
│   └── styles/
│       ├── cyberpunk.css
│       ├── nature.css
│       ├── ocean.css
│       ├── sunset.css
│       ├── minimal.css
│       ├── neon.css
│       ├── geometric.css
│       ├── particles.css
│       ├── matrix.css
│       ├── corporate.css
│       ├── starfield.css
│       └── parallax.css
├── public/
├── astro.config.mjs
└── package.json
```

## Implementation Plan

### Phase 1: Project Setup
- [ ] Initialize Astro project
- [ ] Configure TypeScript support
- [ ] Set up build configuration
- [ ] Install necessary dependencies

### Phase 2: Content Migration
- [ ] Extract all content into `src/data/content.ts`
- [ ] Create centralized data structure for:
  - Site metadata (title, description)
  - Services information
  - Why Choose content
  - Contact information
  - Footer content

### Phase 3: Component Development
- [ ] Create reusable Astro components:
  - `Header.astro` - Site header with title and subtitle
  - `Services.astro` - Services grid section
  - `WhyChoose.astro` - Why choose us section
  - `Contact.astro` - Contact information section
  - `Footer.astro` - Footer with copyright
  - `StyleSelector.astro` - Style selection interface

### Phase 4: Style System Migration
- [ ] Extract embedded styles from HTML files (v7-v12) into separate CSS files
- [ ] Normalize all style implementations to use external CSS
- [ ] Create consistent naming convention for all styles
- [ ] Ensure all styles work with the same HTML structure

### Phase 5: Layout and Routing
- [ ] Create `BaseLayout.astro` with:
  - Dynamic style loading based on route parameter
  - Meta tags and SEO optimization
  - Common page structure
- [ ] Implement dynamic routing with `[style].astro`
- [ ] Configure static path generation for all style variants

### Phase 6: Style Selection System
- [ ] Implement home page style selector with:
  - Preview thumbnails/cards for each style
  - Style descriptions
  - Navigation to style-specific pages
- [ ] Add localStorage persistence for user preferences
- [ ] Implement smooth transitions between styles

### Phase 7: Static Site Generation
- [ ] Configure `getStaticPaths()` to generate all style combinations
- [ ] Optimize build process for multiple style variants
- [ ] Ensure SEO-friendly URLs and meta tags for each variant

## Technical Requirements

### URL Structure
```
/ - Home page with style selector
/cyberpunk/ - Cyberpunk styled version  
/nature/ - Nature styled version
/ocean/ - Ocean styled version
/sunset/ - Sunset styled version
/minimal/ - Minimal styled version
/neon/ - Neon styled version
/geometric/ - Geometric styled version
/particles/ - Particles styled version
/matrix/ - Matrix styled version
/corporate/ - Corporate styled version
/starfield/ - Starfield styled version
/parallax/ - Parallax styled version
/tid-bits/ - Tid bits page (inherits user's selected style)
```

### Data Structure
```typescript
// src/data/content.ts
export interface SiteContent {
  meta: {
    title: string;
    subtitle: string;
    description: string;
  };
  services: Service[];
  whyChoose: WhyChooseItem[];
  contact: ContactInfo;
  footer: FooterInfo;
}

export interface StyleConfig {
  id: string;
  name: string;
  description: string;
  cssFile: string;
  previewImage?: string;
}
```

### Style Configuration
```typescript
export const AVAILABLE_STYLES: StyleConfig[] = [
  { id: 'cyberpunk', name: 'Cyberpunk', description: 'Futuristic neon aesthetics', cssFile: 'cyberpunk.css' },
  { id: 'nature', name: 'Nature', description: 'Earth tones and organic feel', cssFile: 'nature.css' },
  { id: 'ocean', name: 'Ocean', description: 'Blue depths and flowing design', cssFile: 'ocean.css' },
  { id: 'sunset', name: 'Sunset', description: 'Warm colors and gradients', cssFile: 'sunset.css' },
  { id: 'minimal', name: 'Minimal', description: 'Clean and simple design', cssFile: 'minimal.css' },
  { id: 'neon', name: 'Neon', description: 'Bright electric colors', cssFile: 'neon.css' },
  { id: 'geometric', name: 'Geometric', description: 'Sharp angles and patterns', cssFile: 'geometric.css' },
  { id: 'particles', name: 'Particles', description: 'Animated particle effects', cssFile: 'particles.css' },
  { id: 'matrix', name: 'Matrix', description: 'Green code rain aesthetic', cssFile: 'matrix.css' },
  { id: 'corporate', name: 'Corporate', description: 'Professional business look', cssFile: 'corporate.css' },
  { id: 'starfield', name: 'Starfield', description: 'Space and stars theme', cssFile: 'starfield.css' },
  { id: 'parallax', name: 'Parallax', description: 'Layered scrolling effects', cssFile: 'parallax.css' }
];
```

## User Experience Features

### Style Selection Interface
- Grid layout showing all available styles
- Preview cards with style names and descriptions
- Instant navigation to selected style
- Breadcrumb navigation showing current style

### Style Persistence
- localStorage to remember user's preferred style
- Automatic redirect to preferred style on return visits
- Style-aware internal navigation

### Performance Considerations
- Lazy loading of non-critical styles
- Optimized CSS delivery for each style variant
- Minimal JavaScript for style switching
- Static generation for optimal loading speeds

## SEO and Accessibility

### SEO Optimization
- Unique meta tags for each style variant
- Canonical URLs for each style page
- Structured data markup
- XML sitemap including all style variants

### Accessibility
- ARIA labels for style selection interface
- Keyboard navigation support
- High contrast support across all styles
- Screen reader compatible descriptions

## Migration Benefits

1. **Single Source Updates**: Content changes propagate to all style variants automatically
2. **Maintainability**: Clean separation of content, styling, and structure
3. **Performance**: Static generation with optional client-side enhancements
4. **SEO**: Each style variant gets its own optimized static page
5. **User Experience**: Persistent style selection and smooth transitions
6. **Scalability**: Easy addition of new styles or content sections
7. **Developer Experience**: Modern tooling with Astro's component system

## Success Criteria

- [ ] All existing style variants faithfully reproduced
- [ ] Content updates require changes in only one location
- [ ] Style selection works seamlessly from home page
- [ ] All pages maintain consistent functionality across styles
- [ ] Build generates optimized static files for all variants
- [ ] SEO performance maintained or improved
- [ ] Site loads faster than current implementation

## Timeline Estimate

- **Phase 1-2**: 1-2 days (Setup and content migration)
- **Phase 3-4**: 2-3 days (Component and style system development)
- **Phase 5-6**: 2-3 days (Routing and selection interface)
- **Phase 7**: 1 day (Build optimization and testing)

**Total Estimated Time**: 6-9 days

---

*This specification serves as the complete blueprint for migrating the Elmstop website to Astro with unified style selection capabilities.*