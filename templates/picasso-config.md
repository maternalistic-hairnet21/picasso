# .picasso.md -- Project Design Configuration

<!-- Drop this file in your project root. The Picasso agent reads it before every
     audit to understand your project's design preferences. Any field left as the
     default placeholder (brackets) will be ignored and Picasso defaults apply. -->

## Brand
<!-- Core identity. Guides tone-of-voice checks and overall aesthetic expectations. -->
- **Name:** [Your product name]
- **Tone:** [e.g., professional, playful, minimal, bold]
- **Industry:** [e.g., fintech, health, creative, education, developer tools]

## Typography
<!-- Picasso flags generic fonts (Inter, Roboto, Arial) as AI-slop by default.
     If your project intentionally uses one of those, declare it here and it
     will be treated as an approved choice. -->
- **Display Font:** [e.g., Satoshi, Cabinet Grotesk]
- **Body Font:** [e.g., DM Sans, Outfit]
- **Mono Font:** [e.g., Geist Mono, JetBrains Mono]
- **Type Scale Ratio:** [e.g., 1.25 (Major Third)]

## Color
<!-- Picasso uses OKLCH as its default color model.
     Declaring your palette here prevents false positives when auditing
     color usage across the codebase. -->
- **Primary Accent:** [e.g., oklch(0.55 0.25 250)]
- **Neutral Tint:** [e.g., cool (blue), warm (amber), neutral]
- **Dark Mode:** [yes/no/both]

## Component Library
<!-- Tells Picasso which component primitives and CSS framework to expect
     so it can tailor advice and auto-fixes accordingly. -->
- **Framework:** [e.g., shadcn/ui + Radix, Chakra, custom]
- **CSS:** [e.g., Tailwind v4, CSS Modules, Panda CSS]

## Design Settings
<!-- These knobs control how aggressively Picasso suggests changes.
     Scale is 1-10 for each. -->
- **DESIGN_VARIANCE:** [1-10, default 6]
  <!-- How far Picasso can deviate from safe/conventional choices.
       1 = strictly conservative, 10 = experimental and opinionated. -->
- **MOTION_INTENSITY:** [1-10, default 5]
  <!-- Controls animation suggestions.
       1 = almost no motion, 10 = rich choreographed animations. -->
- **VISUAL_DENSITY:** [1-10, default 5]
  <!-- How packed the UI should feel.
       1 = lots of whitespace, 10 = information-dense dashboard style. -->

## Constraints
<!-- Hard requirements that override all other Picasso recommendations.
     Add one bullet per constraint. Remove the examples below and add your own. -->
- [e.g., "Must support IE11", "No animations", "508 compliance required"]
- [e.g., "Match existing app at app.example.com"]
