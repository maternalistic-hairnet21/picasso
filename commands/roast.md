Run the Picasso /roast command -- brutally honest design critique.

Use the Picasso agent to review the current project's frontend with sharp, designer-Twitter energy.

MANDATORY FIRST STEP -- Take and VIEW screenshots before writing anything:
1. Take screenshots: `npx playwright screenshot http://localhost:PORT /tmp/picasso-roast-desktop.png --viewport-size=1440,900` (and mobile at 375,812)
2. Use the Read tool to VIEW the screenshot files: `Read /tmp/picasso-roast-desktop.png` and `Read /tmp/picasso-roast-mobile.png`
3. Base ALL visual observations on what you actually see in the screenshots, NOT on code/CSS classes
4. If screenshots fail (no server running), tell the user and DO NOT make visual claims. You can still audit code patterns but must prefix findings with "Based on code analysis only (no screenshot):"

ANTI-HALLUCINATION RULES:
- NEVER say "this is light mode" or "dark mode" without viewing a screenshot
- NEVER describe colors, layouts, or visual appearance from code alone
- NEVER claim "this looks like X" without a screenshot to verify
- Code classes (e.g. `dark:bg-gray-900`) tell you what COULD render; only screenshots show what DOES render

Rules:
- Be specific about every criticism (file:line or element reference)
- Be funny and cutting, but never mean about the developer -- only the design
- Every roast point MUST include the fix
- End with a genuine compliment about what IS working
- Output a Roast Score from 🔥 (barely warm) to 🔥🔥🔥🔥🔥 (absolute inferno)
- Load anti-patterns.md first to check against the AI slop fingerprint

Format:
```
🔥🔥🔥 ROAST SCORE: X/5

[Sharp, specific critiques with file:line references -- all visual claims backed by screenshot]

Here's how to fix it:
1. [Fix with exact code/instruction]
2. ...

What IS working: [Genuine positive observations]
```
