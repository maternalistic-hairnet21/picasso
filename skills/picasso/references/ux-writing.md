# UX Writing Reference

Microcopy and content design rules for frontend interfaces. Every string in the UI is a design decision.

## Table of Contents
1. Principles
2. Button Labels
3. Headlines and Hero Copy
4. Error Messages
5. Empty States
6. Loading States
7. Confirmation and Success
8. Placeholder Content
9. Banned Words
10. Common Mistakes

---

## 1. Principles

- Voice is consistent (who you are). Tone shifts with context (how you say it now).
- Every word is a UX decision. "Submit" tells the user nothing. "Publish article" tells them exactly what happens.
- Specificity builds trust. Vagueness creates anxiety. When in doubt, be specific.

---

## 2. Button Labels

Always verb-first and specific to the action. The user must know what will happen before they click.

**Banned labels:** "Submit", "OK", "Click here", "Yes", "No", "Proceed", "Go", "Done" (when ambiguous).

| Bad | Good | Context |
|---|---|---|
| Submit | Save changes | Form edit |
| Submit | Create account | Registration |
| Submit | Send message | Contact form |
| OK | Got it | Info dialog |
| Yes / No | Delete project / Keep project | Delete confirm |
| Cancel | Discard changes | Unsaved edits |
| Send | Send invitation | Invite flow |
| Save | Publish article | Publish flow |
| Continue | Review order | Checkout |
| Continue | Start free trial | Onboarding |
| Click here | Download report | Export |
| Go | Search products | Search |
| Add | Add team member | Team mgmt |
| Remove | Remove from project | Membership |
| Delete | Delete account permanently | Destructive |
| Update | Update billing info | Settings |

Destructive buttons must state consequences: "Delete project and all files" not "Delete". "Cancel subscription (ends Jan 15)" not "Cancel".

---

## 3. Headlines and Hero Copy

**Banned phrases:** "Build the future of work", "Your all-in-one platform", "Scale without limits", "Empowering teams", "Reimagine how you work", "Seamless experience", "Unlock the power of [anything]", "Everything you need to succeed", "Work smarter, not harder". These could describe anything from a CRM to a toaster.

Headlines must make a specific, verifiable claim about what the product does.

| Before (Generic) | After (Specific) |
|---|---|
| "Your all-in-one project platform" | "Ship projects with half the meetings" |
| "Empowering teams to collaborate" | "Comment on any design file without leaving Slack" |
| "Scale your business effortlessly" | "Handle 10x more orders without hiring ops staff" |
| "The future of document management" | "Find any contract clause in 3 seconds" |
| "Unlock the power of AI" | "Turn customer calls into action items automatically" |

Subheadline = how. Example: "Deploy to 30 regions in one click" / "Push to main. We handle containers, DNS, SSL, and rollbacks."

---

## 4. Error Messages

Pattern: **What happened + Why + What to do next.** Example: "We couldn't save your document. The connection was interrupted. Check your internet and try again."

**Banned:** Raw error codes ("Error 500", "404"), stack traces in the UI, "Something went wrong" alone, "Invalid input" without specifics, "Please try again later" as only instruction.

| Bad | Good |
|---|---|
| "404 Not Found" | "We couldn't find that page. It may have been moved or deleted." |
| "Invalid input" | "Email must include an @ symbol" |
| "Error: field required" | "First name is required to create your account" |
| "File upload failed" | "That file is 25MB. The limit is 10MB. Try compressing it." |
| "Authentication error" | "Wrong password. Reset it from the login page." |
| "Something went wrong" | "We couldn't load your dashboard. Refresh, or contact support." |

Validate inline. Show errors next to the field, not in a top banner.

---

## 5. Empty States

Every empty state needs: (1) what this area will contain, (2) how to get started, (3) a clear CTA. **Banned:** blank pages, raw "null"/"undefined"/"[]", "Nothing to see here", sad emoji alone.

| Screen | Copy | CTA |
|---|---|---|
| Empty inbox | "No messages yet. Messages you receive appear here." | [Compose a message] |
| Empty projects | "No projects yet. Projects organize tasks and files." | [Create your first project] |
| Empty search | "No results for 'quarterly report'. Try a different term." | [Browse all documents] |
| First-time dashboard | "Welcome. Metrics and activity will live here." | [Connect data source] |
| Empty activity | "No activity yet. Team updates appear as work happens." | [Go to your first project] |

---

## 6. Loading States

- **Skeleton screens** for content areas (feeds, lists, cards). Skeletons > spinners when layout is predictable.
- **Spinners** for isolated actions (button clicks, small fetches). Keep small and inline.
- **Progress indicators** for 5+ second operations. Show percentage or step count.
- **Optimistic updates** for actions where failure is rare (likes, toggles, sends). Update UI immediately, roll back on failure.

| Duration | Copy |
|---|---|
| Under 2s | No text, skeleton or spinner only |
| 2-5s | "Loading your projects..." |
| 5-30s | "Processing 847 transactions. About 30 seconds." |
| 30s+ | Progress + ETA + notification option |

Never block the entire page when only one section is loading.

---

## 7. Confirmation and Success

Pattern: **State what happened + optional next step.**

| Bad | Good |
|---|---|
| "Success!" | "Project created. Add your first task?" |
| "Done." | "Invoice sent to maria@acme.co" |
| "Saved." | "Changes saved. Preview your updated profile?" |
| "Deleted." | "3 files moved to trash. Undo?" |

Delete confirmations must state consequences: "Delete 'Q4 Marketing Plan'? This removes the document and its 12 comments permanently." [Delete document] [Keep document]. Never "Are you sure?" [Yes] [No].

For reversible actions, skip the modal -- use an undo toast with a 5-10 second window.

---

## 8. Placeholder Content

**Banned:** "John Doe", "Jane Smith", "Acme Corp", "Lorem ipsum", "test@test.com", broken Unsplash links, watermarked stock photos.

Use realistic content: diverse names ("Priya Mehta", "Carlos Rivera", "Amina Osei"), matching emails ("priya@meridian.io"), plausible companies ("Meridian Analytics", "Strand & Co"), messy metrics ($47,291 not $100,000, 12.3% not 99.99%), recent dates, and domain-specific task names ("Finalize vendor contract" not "Task 1").

---

## 9. Banned Words

**AI-telltale words** -- never use in UI copy:
"delve" (say "explore"), "pivotal" (say "important"), "showcasing" (say "showing"), "fostering" (say "building"), "vibrant" (say "active"), "enhance"/"elevate" (say "improve" or be specific), "leverage" (say "use"), "synergy" (describe the benefit), "seamless" (describe what works well), "cutting-edge" (name the tech), "revolutionary" (describe the change), "game-changing" (describe the impact).

**Hedge language** -- banned: "may help you", "might improve", "can potentially", "designed to help", "aims to provide". State what it does or do not say it.

**Marketing fluff** -- banned: "world-class", "best-in-class", "robust", "powerful", "intuitive".

---

## 10. Common Mistakes

- Using "Submit" as a button label anywhere
- Error messages that blame the user ("You entered an invalid email" vs "Email must include @")
- Empty states with no explanation or CTA
- "Click here" as link text (inaccessible to screen readers)
- Headlines that could describe any product in the industry
- "Lorem ipsum" in demos or external screenshots
- Full-page spinner when only one section loads
- "Success!" without stating what succeeded
- "Are you sure?" without stating consequences
- Inconsistent voice across screens
- "John Doe" or "Acme Corp" in customer-visible data
- Raw error codes (500, 404) in the UI
- Hedge language ("might", "may") in feature copy
- Button labels that do not start with a verb
