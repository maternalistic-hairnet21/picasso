# Modern CSS Reference

## Table of Contents
1. `:has()` Selector
2. CSS Nesting
3. Scroll-Driven Animations
4. View Transitions API
5. Anchor Positioning
6. `light-dark()` Function
7. `color-mix()` and Relative Color Syntax
8. `@layer` Cascade Layers
9. `@scope`
10. Other Modern Features
11. Common Mistakes

---

## 1. `:has()` Selector

The parent selector. `:has()` selects an element based on what it contains, eliminating many JS class-toggle patterns.

```css
/* Disable submit when form has invalid fields */
form:has(:invalid) button[type="submit"] {
  opacity: 0.5;
  pointer-events: none;
}

/* Style cards that contain an image differently */
.card:has(img) { grid-template-rows: 200px 1fr; }

/* Lock body scroll when modal is open -- no JS class needed */
body:has(.modal-open) { overflow: hidden; }

/* Highlight label when sibling input is focused */
.field:has(input:focus) label { color: var(--color-primary); }
```

Reach for `:has()` whenever you would otherwise add a JS listener solely to toggle a class on a parent. Supported in all modern browsers since late 2023.

---

## 2. CSS Nesting

Native nesting without preprocessors. The `&` is required for pseudo-classes and compound selectors; implicit for descendant selectors.

```css
.card {
  padding: 1.5rem;
  h2 { font-size: 1.25rem; }
  .body { color: var(--text-secondary); }
  &:hover { box-shadow: 0 4px 12px rgba(0 0 0 / 0.1); }
  &.featured { border: 2px solid var(--color-primary); }
  @media (min-width: 768px) { padding: 2rem; }
}
```

**Nesting vs. Tailwind:** Use nesting for global/base styles and component library internals. Use Tailwind utilities for one-off styling in markup. They pair well: Tailwind handles the 90% case, nesting handles the rest in `@layer components`.

---

## 3. Scroll-Driven Animations

Replace IntersectionObserver and scroll listeners with pure CSS.

### Complete Progress Bar Example
```css
@keyframes fill {
  from { transform: scaleX(0); }
  to   { transform: scaleX(1); }
}
.progress-bar {
  position: fixed; top: 0; left: 0;
  width: 100%; height: 3px;
  background: var(--color-primary);
  transform-origin: left;
  animation: fill linear;
  animation-timeline: scroll();
}
```

### Reveal on Scroll
```css
@keyframes slide-in {
  from { opacity: 0; transform: translateY(40px); }
  to   { opacity: 1; transform: translateY(0); }
}
.reveal {
  animation: slide-in linear both;
  animation-timeline: view();
  animation-range: entry 0% entry 100%;
}
```

`scroll()` ties to container progress. `view()` ties to element visibility. `animation-range` controls which portion of the timeline runs.

---

## 4. View Transitions API

Smooth transitions between DOM states without animation libraries.

```js
document.startViewTransition(() => { container.innerHTML = newContent; });
```

```css
::view-transition-old(root) { animation: fade-out 200ms ease-out; }
::view-transition-new(root) { animation: fade-in 200ms ease-in; }
.product-image { view-transition-name: product-hero; } /* shared-element morph */
```

**Next.js:** wrap `router.push()` in `document.startViewTransition()`. **Astro:** add `<ViewTransitions />` to layout head.

---

## 5. Anchor Positioning

Position elements relative to any other element in the DOM without JS.

```css
.trigger { anchor-name: --tooltip-anchor; }
.tooltip {
  position: fixed;
  position-anchor: --tooltip-anchor;
  top: anchor(bottom);
  left: anchor(center);
  translate: -50% 8px;
  position-try-fallbacks: flip-block, flip-inline; /* auto-flip on overflow */
}
```

`position-try-fallbacks` flips the element if it overflows the viewport. Replaces Floating UI/Popper.js for tooltips, dropdowns, and popovers.

---

## 6. `light-dark()` Function

Resolves to one of two values based on color scheme. Requires `color-scheme` on root.

```css
:root { color-scheme: light dark; }
body {
  color: light-dark(#1a1a1a, #e5e5e5);
  background: light-dark(#ffffff, #0a0a0a);
}
.card {
  background: light-dark(#f5f5f5, #1a1a1a);
  border: 1px solid light-dark(#e0e0e0, #2a2a2a);
}
```

Eliminates `prefers-color-scheme` media queries for most cases. Components can force a scheme locally by setting `color-scheme` on an ancestor.

---

## 7. `color-mix()` and Relative Color Syntax

Generate entire palettes from a single brand color.

```css
:root {
  --brand: #3b82f6;
  --brand-light: color-mix(in oklch, var(--brand), white 30%);
  --brand-dark: color-mix(in oklch, var(--brand), black 30%);
}
```

Always use `oklch` or `oklab` for interpolation to avoid muddy midpoints.

```css
/* Relative color syntax: full theme from one color */
:root {
  --brand: oklch(0.55 0.2 260);
  --brand-vivid: oklch(from var(--brand) l calc(c * 1.3) h);
  --surface: oklch(from var(--brand) 0.98 0.01 h);
  --text-primary: oklch(from var(--brand) 0.15 0.02 h);
  --accent: oklch(from var(--brand) 0.55 0.22 calc(h + 30));
}
```

Colors stay harmonious because they share the same hue origin.

---

## 8. `@layer` Cascade Layers

Explicit control over which styles win, independent of specificity or source order.

```css
@layer reset, base, components, utilities;

@layer reset { *, *::before, *::after { box-sizing: border-box; margin: 0; } }
@layer base { body { font-family: var(--font-sans); line-height: 1.6; } }
@layer components { .btn { padding: 0.5rem 1rem; border-radius: 0.5rem; } }
```

Layers listed first have lowest priority. Layer order trumps specificity: `.btn` in `utilities` beats `#main .content .btn` in `base`.

**Tailwind v4** emits utilities inside `@layer utilities` automatically. Your component styles in `@layer components` will never accidentally override a Tailwind class. Wrap all custom CSS in the correct layer.

---

## 9. `@scope`

Scoped styles without Shadow DOM or CSS Modules.

```css
@scope (.card) {
  h2 { font-size: 1.25rem; }
  p  { color: var(--text-secondary); }
}

/* Lower boundary: styles apply inside .card but NOT inside .card-content */
@scope (.card) to (.card-content) {
  color: var(--text-muted);
  font-size: 0.875rem;
}
```

When two `@scope` rules target the same element, the scope whose root is closer in the DOM wins. This gives natural component-level specificity without fighting selector weight.

---

## 10. Other Modern Features

**`@property`** -- Typed custom properties enabling animated gradients:
```css
@property --angle { syntax: "<angle>"; initial-value: 0deg; inherits: false; }
.gradient { animation: spin 3s linear infinite; }
@keyframes spin { to { --angle: 360deg; } }
```

**`@starting-style`** -- Enter animations from `display: none`:
```css
dialog[open] {
  opacity: 1; transition: opacity 200ms;
  @starting-style { opacity: 0; }
}
```

**`interpolate-size: allow-keywords`** -- Animate to `height: auto`:
```css
:root { interpolate-size: allow-keywords; }
.collapsible { height: 0; transition: height 300ms; }
.collapsible.open { height: auto; }
```

**`field-sizing: content`** -- Auto-growing textareas: `textarea { field-sizing: content; min-height: 3lh; }`

**`text-wrap: balance/pretty`** -- `h1, h2, h3 { text-wrap: balance; }` for even line lengths. `p { text-wrap: pretty; }` to avoid orphans.

**`align-content` on block elements** -- `align-content: center` for vertical centering without flexbox or grid.

---

## 11. Common Mistakes

### Unnecessary Vendor Prefixes
`border-radius`, `box-shadow`, `transition`, `transform`, `flexbox`, `grid`, and `filter` have not needed prefixes for years. Only exceptions: `-webkit-line-clamp` and some `-webkit-appearance` values.

### Not Using @layer with Utility Frameworks
Custom CSS alongside Tailwind without `@layer components` causes specificity collisions. Unlayered styles beat layered ones. Always declare layers and assign all styles to them.

### Missing Fallback Strategies
Not every feature here has universal support:
- **Safe everywhere:** `:has()`, nesting, `@layer`, `color-mix()`, `light-dark()`, `text-wrap: balance`
- **Wide support (test on targets):** View Transitions, `@property`, `@starting-style`, scroll-driven animations
- **Emerging (use with fallback):** Anchor positioning, `@scope`, `field-sizing`, `interpolate-size`

For emerging features, use `@supports`:
```css
@supports (position-anchor: --x) {
  .tooltip { position: fixed; position-anchor: --trigger; top: anchor(bottom); }
}
```

### Overusing Nesting Depth
Keep nesting to two or three levels maximum. Deeper nesting creates brittle selectors tied to DOM structure and high-specificity rules that are hard to override.

### Ignoring color-scheme Declaration
Using `light-dark()` without `color-scheme: light dark` on the root means the function always returns the light value. The `color-scheme` property is what tells the browser which mode is active.
