---
name: mobile-prototype-drawing
description: Create or update mobile H5 static HTML prototypes from reference screenshots, business rules, workflow descriptions, field lists, approval flows, interaction requirements, hotspot hints, and page-flow diagrams. Use when Codex needs 手机端原型绘制, mobile prototype drawing, H5/mobile HTML prototypes, screenshot-to-static-HTML mobile pages, business-rule-driven mobile prototypes, interactive mobile page flows, prototype screenshots, hotspot/interaction marking, or page-flow overview diagrams.
---

# 手机端原型绘制

## Overview

Use this skill to produce reviewable mobile H5 prototypes as static HTML. The input can be reference screenshots, business rules, field definitions, page-flow descriptions, state rules, interaction requirements, or a mix of these.

Default to a single-file prototype unless the user explicitly requests separate HTML files.

## Required Reference

Before editing or creating the prototype, read `references/mobile-prototype-rules.md`. It contains the detailed conventions for single-file routing, screenshot storage, real-interaction marking, modal hotspot behavior, browser-shell static elements, and page-flow diagrams.

## Workflow

1. Identify the requested prototype mode:
   - Screenshot restoration: reproduce the supplied mobile screen visually.
   - Business-rule generation: infer screens, fields, states, and interactions from the user's rules.
   - Flow extension: add new pages, routes, modal states, or interaction states to an existing prototype.
   - Flow documentation: generate a single image showing how pages connect.

2. Inspect existing project files before editing:
   - Prefer the current user-opened HTML or `index.html` as the main prototype file.
   - Check for `AGENTS.md`; if present, follow it as project-level policy.
   - Keep generated screenshots in `preview-screenshots/`.

3. Build or update the prototype:
   - Use a realistic mobile viewport.
   - Keep visual density close to the user's screenshot or business context.
   - For multiple pages, use hash-based views inside one HTML file.
   - Preserve existing user work and avoid unrelated refactors.

4. Add interactions:
   - Use real HTML controls and links where behavior exists.
   - Keep visual-only controls static; do not make them links or bind empty events just for marking.
   - If the prototype needs discoverable click targets, use an optional hotspot layer based on real interactive elements.

5. Verify:
   - Open the prototype with a browser when possible.
   - Check hash navigation, modal behavior, mobile width, no horizontal scroll, and main clickable paths.
   - Confirm hotspot hints mark only true interactions, including modal-only marking when a modal is open.
   - Do not generate standalone page screenshots after every prototype edit unless the user explicitly asks for them.
   - When the user asks for a business/page-flow overview, capture all needed current page states at that point and generate one integrated flow image.

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
- Confirmation pages, external-link handoffs, modal prompts, and exception states
- Review needs such as page-flow diagrams or clickable hotspot markers

Ask a concise clarification only if the missing rule would materially change the page structure. Otherwise make a reasonable assumption and continue.

## Output Standards

- Make the first screen usable, not a landing page.
- Keep controls familiar: links for navigation, inputs for fields, radio/checkbox controls for choices.
- Keep copy in Chinese when the source material is Chinese.
- Use concise visible labels suitable for business review.
- Store generated flow diagrams and explicitly requested screenshots under `preview-screenshots/`.
