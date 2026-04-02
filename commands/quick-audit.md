---
name: quick-audit
description: "5-minute fast design audit: 6 binary checks (font, color, layout, spacing, a11y, anti-slop)"
---

Run the Picasso /quick-audit command. Check exactly 6 things and report pass/fail for each. Takes 5 minutes, not 30.

MANDATORY FIRST STEP: Take a desktop screenshot (`npx playwright screenshot http://localhost:PORT /tmp/picasso-quick-audit.png --viewport-size=1440,900`) and VIEW it with the Read tool before assessing Layout, Spacing, or Anti-Slop visually. Font and Color checks can be done from code. Accessibility can be done from code. But layout/spacing/anti-slop require visual verification.

If screenshots fail, you can still check Font, Color, and Accessibility from code. Mark Layout, Spacing, and Anti-Slop as "SKIPPED (no screenshot available)" instead of guessing.
