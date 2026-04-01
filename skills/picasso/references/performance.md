# Performance Reference

## Table of Contents
1. Core Web Vitals
2. Image Optimization
3. Font Loading
4. Code Splitting
5. Rendering Performance
6. Loading Patterns
7. Bundle Size
8. Measurement
9. Common Mistakes

---

## 1. Core Web Vitals

### Largest Contentful Paint (LCP) -- Target: < 2.5s
Time until the largest visible element renders (hero image, heading, video poster). Affected by image format/compression, font loading strategy, and resource prioritization.

### Cumulative Layout Shift (CLS) -- Target: < 0.1
Visual stability score. Increases when elements shift after becoming visible. Caused by images without dimensions, font swaps with different metrics, and dynamically injected content above existing elements.

### Interaction to Next Paint (INP) -- Target: < 200ms
Worst interaction latency during a visit. Worsened by heavy main-thread JS, complex DOM structures, and JS-driven animations instead of CSS compositing.

---

## 2. Image Optimization

Always set dimensions to prevent CLS. Use `aspect-ratio` with `object-fit` for fluid layouts:
```html
<img src="hero.webp" width="1200" height="630" alt="Hero" />
```
```css
.hero-img {
  width: 100%;
  aspect-ratio: 1200 / 630;
  object-fit: cover;
}
```

Use next-gen formats (prefer AVIF, then WebP, then JPEG) via `<picture>` with format fallbacks. Loading strategy -- `fetchpriority="high"` for hero, `loading="lazy"` for below-fold:
```html
<img src="hero.webp" fetchpriority="high" width="1200" height="630" alt="Hero" />
<img src="card.webp" loading="lazy" width="400" height="300" alt="Card" />
```

Use `srcset` and `sizes` for responsive images:
```html
<img
  srcset="hero-480.webp 480w, hero-800.webp 800w, hero-1200.webp 1200w"
  sizes="(max-width: 640px) 100vw, (max-width: 1024px) 50vw, 1200px"
  src="hero-1200.webp"
  alt="Hero"
/>
```

---

## 3. Font Loading

Always use `font-display: swap` so text is visible immediately:
```css
@font-face {
  font-family: 'Inter';
  src: url('/fonts/inter-var.woff2') format('woff2');
  font-display: swap;
  font-weight: 100 900;
}
```

Preload the one or two critical fonts used above the fold:
```html
<link
  rel="preload"
  href="/fonts/inter-var.woff2"
  as="font"
  type="font/woff2"
  crossorigin
/>
```

Self-host fonts to eliminate third-party DNS lookups. Subset to needed characters (Latin-only subsets are 50-70% smaller). Use variable fonts to replace multiple weight files with a single request.

---

## 4. Code Splitting

### React.lazy + Suspense
```tsx
'use client';
import { lazy, Suspense } from 'react';
const RichTextEditor = lazy(() => import('./rich-text-editor'));

export function EditorPanel() {
  return (
    <Suspense fallback={<EditorSkeleton />}>
      <RichTextEditor />
    </Suspense>
  );
}
```

### Dynamic imports for heavy components
Charts, maps, editors, and PDF viewers should never be in the initial bundle:
```tsx
import dynamic from 'next/dynamic';
const Chart = dynamic(() => import('./analytics-chart'), {
  loading: () => <ChartSkeleton />,
  ssr: false,
});
```

### Route-based splitting
Next.js App Router automatically code-splits by route segment. Each `page.tsx` is a separate chunk. No configuration needed.

---

## 5. Rendering Performance

Only animate `transform` and `opacity` (GPU-composited, no layout/paint):
```css
.card { transition: transform 200ms ease, opacity 200ms ease; }
.card:hover { transform: translateY(-4px); opacity: 0.95; }

/* Bad: triggers layout every frame */
.card:hover { top: -4px; width: 102%; }
```

Use `will-change` sparingly. Apply only before animation, release after:
```css
.modal-enter { will-change: transform, opacity; }
.modal-idle  { will-change: auto; }
```

Skip rendering off-screen content with `content-visibility`. Always pair with `contain-intrinsic-size`:
```css
.faq-section {
  content-visibility: auto;
  contain-intrinsic-size: auto 500px;
}
```

Isolate components with `contain: layout style paint` so their internals do not trigger parent recalculation. Prefer CSS animations over JS -- CSS runs on the compositor thread and does not block interactions.

---

## 6. Loading Patterns

Skeleton screens over spinners. Skeletons communicate the shape of incoming content:
```tsx
function CardSkeleton() {
  return (
    <div className="animate-pulse space-y-3">
      <div className="h-48 rounded-lg bg-gray-200" />
      <div className="h-4 w-3/4 rounded bg-gray-200" />
      <div className="h-4 w-1/2 rounded bg-gray-200" />
    </div>
  );
}
```

Progressive loading with Suspense boundaries -- each section streams independently:
```tsx
export default function DashboardPage() {
  return (
    <div className="grid grid-cols-3 gap-6">
      <Suspense fallback={<StatsSkeleton />}><StatsPanel /></Suspense>
      <Suspense fallback={<ChartSkeleton />}><RevenueChart /></Suspense>
      <Suspense fallback={<TableSkeleton />}><RecentOrders /></Suspense>
    </div>
  );
}
```

Optimistic updates for user actions -- update UI instantly, revert on failure. RSC streams HTML as data resolves, eliminating client-side fetch waterfalls.

---

## 7. Bundle Size

Tree-shake by using named imports. Never import entire libraries:
```tsx
import { Button } from '@/components/ui/button';  // Good
import { Search, Menu } from 'lucide-react';       // Good: ~2KB
import * as Icons from 'lucide-react';              // Bad: ~200KB+
import _ from 'lodash';                             // Bad: entire library
```

Monitor with `next build` output or `webpack-bundle-analyzer`. Target < 100KB first-load JS. If exceeded, audit shared dependencies and lazy-load.

---

## 8. Measurement

Field data with `web-vitals`:
```tsx
import { onLCP, onCLS, onINP } from 'web-vitals';
onLCP(console.log); onCLS(console.log); onINP(console.log);
```

Lab data with Lighthouse (DevTools or CI). Use both -- lab shows potential, field shows real impact. Performance tab reveals long tasks (red bars > 50ms).

Break long tasks with `scheduler.yield()`:
```tsx
async function processLargeList(items: Item[]) {
  for (const item of items) {
    processItem(item);
    if (navigator.scheduling?.isInputPending?.()) {
      await scheduler.yield();
    }
  }
}
```

---

## 9. Common Mistakes

- **Lazy-loading above-fold images**: Hero images need `fetchpriority="high"` and eager loading. `loading="lazy"` delays LCP by waiting for viewport intersection.
- **No explicit image dimensions**: Omitting `width`/`height` or `aspect-ratio` causes the browser to allocate zero space, then shift everything when the image loads. Most common CLS source.
- **Importing full icon libraries**: `import { Icon } from 'react-icons/fa'` is fine. `import * as Icons from 'react-icons'` ships hundreds of unused SVGs.
- **No code splitting**: A monolithic bundle forces downloading JS for pages never visited. Use route-based splitting and `React.lazy` for heavy components.
- **Animating layout properties**: `width`, `height`, `top`, `left`, `margin`, `padding` trigger layout recalculation every frame. Use `transform` and `opacity` instead.
- **`content-visibility` without `contain-intrinsic-size`**: Page height collapses, creating massive layout shifts as the user scrolls and sections render in.
