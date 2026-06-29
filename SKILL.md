---
name: mobile-prototype-drawing
description: Create or update mobile H5 static HTML prototypes from user-provided reference screenshots, business rules, workflow descriptions, field lists, approval flows, or page interaction requirements. Use when the user asks for 手机端原型绘制, mobile prototype drawing, H5/mobile HTML prototype, screenshot-to-static-HTML mobile page, business-rule-driven mobile prototype, interactive mobile page flow, prototype screenshots, hotspot/interaction marking, or page-flow overview diagrams.
---

# 手机端原型绘制

## Overview

Use this skill to produce reviewable mobile H5 prototypes as static HTML. The input can be either reference screenshots, business rules, page-flow descriptions, field definitions, or a mix of these.

Default to a single-file prototype unless the user explicitly requests separate HTML files.

## Required Reference

Before editing or creating the prototype, read `references/mobile-prototype-rules.md`. It contains the project conventions for single-file routing, screenshot storage, real-interaction marking, and page-flow diagrams.

## Workflow

1. Identify the requested prototype mode:
   - Screenshot restoration: reproduce the supplied mobile screen visually.
   - Business-rule generation: infer screens, fields, states, and interactions from the user's rules.
   - Flow extension: add new pages or interaction states to an existing prototype.
   - Flow documentation: generate a single image showing how pages connect.

2. Inspect existing project files before editing:
   - Prefer `index.html` as the main prototype file.
   - Check for `AGENTS.md`; if present, follow it as project-level policy.
   - Keep generated screenshots in `preview-screenshots/`.

3. Build or update the prototype:
   - Use a realistic mobile viewport.
   - Keep visual density close to the user's screenshot or the business context.
   - For multiple pages, use hash-based views inside one HTML file.
   - Preserve existing user work and avoid unrelated refactors.

4. Add interactions:
   - Use real HTML controls and links where behavior exists.
   - Do not mark purely visual buttons as interactive unless they have actual behavior.
   - If the prototype needs discoverable click targets, use an optional hotspot layer based on real interactive elements.

5. Verify:
   - Open the prototype with a browser when possible.
   - Check hash navigation, mobile width, no horizontal scroll, and main clickable paths.
   - Generate updated screenshots for visual changes.

6. Deliver:
   - Link to the edited HTML file.
   - Link to relevant screenshots or flow diagrams.
   - Briefly state what interactions were implemented or left as visual-only.

## Business-Rule Input

When the user provides business rules instead of screenshots, infer a mobile page set from:

- Actors and entry points
- Required fields and validation hints
- Statuses and state transitions
- Primary and secondary actions
- Confirmation pages, external-link handoffs, and exception states
- Review needs such as page-flow diagrams or clickable hotspot markers

Ask a concise clarification only if the missing rule would materially change the page structure. Otherwise make a reasonable assumption and continue.

## Output Standards

- Make the first screen usable, not a landing page.
- Keep controls familiar: links for navigation, inputs for fields, radio/checkbox controls for choices.
- Keep copy in Chinese when the source material is Chinese.
- Use concise visible labels suitable for business review.
- Store generated preview images under `preview-screenshots/`.
