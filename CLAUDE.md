# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a personal portfolio website built with Gatsby v2.18.7 and React 16.10.2. It's a fork/adaptation of Brittany Chiang's portfolio site - **always maintain proper attribution** by linking to [brittanychiang.com](https://brittanychiang.com).

## Development Commands

### Core Commands
- `npm start` - Start development server on http://localhost:8000 with hot reload
- `npm run build` - Generate full static production build
- `npm run serve` - Preview production build locally
- `npm run clean` - Clear Gatsby cache (useful when encountering build issues)
- `npm run format` - Run Prettier formatting on all files

### Setup
1. Install Gatsby CLI: `npm install -g gatsby-cli`
2. Use Node 10.13.0: `nvm install` (reads from .nvmrc)
3. Install dependencies: `yarn`

## Architecture

### Tech Stack
- **Framework:** Gatsby v2.18.7 (static site generator)
- **UI Library:** React 16.10.2
- **Styling:** Styled-components v4.4.0 (CSS-in-JS)
- **Animation:** ScrollReveal v4.0.5, Anime.js v3.1.0
- **Content:** Markdown with YAML frontmatter
- **Code Highlighting:** PrismJS

### Key Directories
- `src/components/` - Reusable React components and page sections
- `src/pages/` - Auto-routed pages (index.js, 404.js, pensieve/)
- `src/templates/` - Dynamic templates for blog posts and tag archives
- `src/styles/` - Styled-components theme, mixins, and global styles
- `src/hooks/` - Custom React hooks (scroll detection, click outside, reduced motion)
- `content/` - Markdown content (posts, projects, jobs, featured)

### Module Aliases
Webpack aliases are configured in gatsby-node.js for cleaner imports:
- `@components` → `src/components`
- `@config` → `src/config`
- `@styles` → `src/styles`
- `@fonts` → `src/fonts`
- `@hooks` → `src/hooks`
- `@images` → `src/images`

### Content Management
- Blog posts live in `content/posts/{slug}/index.md`
- Required frontmatter: `title`, `date`, `slug`, `tags`, `description`
- Optional: `draft` (boolean to exclude from production)
- Dynamic pages and tag archives auto-generated via gatsby-node.js

### Page Structure (index.js)
The home page uses this component hierarchy:
```
Layout
  ├─ Hero (landing with name/title)
  ├─ About (bio section)
  ├─ Jobs (experience timeline)
  ├─ Featured (highlighted projects)
  ├─ Projects (full project grid)
  └─ Contact (CTA section)
```

## Styling Guidelines

### Theme & Colors
Colors are defined in `src/config.js` and used via styled-components theme:
- **Navy** (`#0a192f`) - Primary background
- **Dark Navy** (`#020c1b`) - Darker background
- **Green** (`#64ffda`) - Primary accent color
- **Slate** (`#8892b0`, `#a8b2d1`, `#ccd6f6`) - Text shades

### CSS Variables
Global CSS variables in `src/styles/GlobalStyle.js`:
- Font sizes: `var(--fz-xxs)` through `var(--fz-heading)`
- Fonts: `var(--font-sans)` (Calibre), `var(--font-mono)` (SF Mono)
- Colors: `var(--navy)`, `var(--green)`, etc.

### Styled-Components Mixins
Use helper mixins from `src/styles/mixins.js`:
- `mixins.flexCenter` / `mixins.flexBetween` - Flexbox utilities
- `mixins.link` / `mixins.inlineLink` - Link styling
- `mixins.button` / `mixins.smallButton` / `mixins.bigButton` - Button styles
- `mixins.fancyList` - Styled bullet lists

### Responsive Design
- Mobile-first approach using media queries
- Breakpoints defined in theme: `mobileS`, `mobileM`, `mobileL`, `tabletS`, `tabletL`, `desktopXS`, `desktopS`, `desktopM`, `desktopL`
- Access via: `@media (max-width: ${({ theme }) => theme.bp.tablet})`

## Code Quality

### Pre-commit Hooks
Husky runs lint-staged on commit via `.husky/pre-commit`:
- Prettier formats: `*.{js,css,json,md}`
- ESLint fixes: `*.js` files

### Linting & Formatting
- ESLint extends `@upstatement/eslint-config/react`
- Prettier uses `@upstatement/prettier-config`
- 2-space indentation (enforced via .editorconfig)

### Important Build Configuration
In gatsby-node.js, ScrollReveal and Anime.js are excluded from server-side rendering:
```javascript
if (stage === 'build-html' || stage === 'develop-html') {
  actions.setWebpackConfig({
    externals: {
      'scrollreveal': 'scrollreveal',
      'animejs': 'animejs',
    },
  });
}
```

## Deployment

- **Hosting:** Netlify (auto-deploys on push to main branch)
- **Build Command:** `gatsby build`
- **Publish Directory:** `/public`
- **Node Version:** 10.13.0+ (specified in .nvmrc)

## Accessibility Considerations

- Support for `prefers-reduced-motion` via `usePrefersReducedMotion` hook
- External links have proper `target="_blank"` and `rel` attributes
- Focus management for scroll navigation
- Semantic HTML structure in components

## Important Notes

- **No Testing Infrastructure:** No formal tests exist. If adding tests, consider Jest + React Testing Library
- **Legacy Dependencies:** Using older versions of Gatsby/React - be cautious when updating dependencies
- **Google Analytics:** Integrated with ID `UA-45666519-2` (configured in gatsby-config.js)
- **Attribution Required:** When forking, always credit Brittany Chiang and link to brittanychiang.com
