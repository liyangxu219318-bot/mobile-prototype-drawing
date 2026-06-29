# Mobile Prototype Drawing Rules

## Single-File Rule

Default to one HTML file, usually `index.html`. Do not create additional HTML files unless the user explicitly asks for a separate file.

For multi-page prototypes, use hash routes such as:

- `#home`
- `#list`
- `#detail`
- `#confirm`
- `#external-opened`

Represent each page with a `.view` container and toggle `.view.active` in JavaScript.

## Inputs

Support both kinds of input:

- Reference screenshots: reproduce layout, proportions, visual hierarchy, colors, and page states.
- Business rules: derive pages, fields, buttons, status labels, validation hints, confirmations, and navigation.

When both are present, use screenshots for visual form and business rules for behavior.

## File Organization

- Main prototype: `index.html`
- Preview screenshots and diagrams: `preview-screenshots/`
- Project policy: `AGENTS.md`, if present

Never leave generated screenshots or temporary preview images in the project root.

## Mobile UI Standards

- Use a realistic phone canvas and mobile proportions.
- On desktop, center the phone canvas; on narrow screens, let it fill the viewport.
- Preserve compact mobile density when the source is an enterprise H5 screen.
- Avoid marketing-style landing pages unless the user explicitly asks for one.
- Use real HTML controls for business fields:
  - `input` / `textarea` for text
  - `radio` for mutually exclusive choices
  - `checkbox` for multi-select
  - `select` for option lists when appropriate
  - `a[href]` for navigation

## Interaction Marking

If an interaction hint layer is needed, generate markers from real interactive HTML, not manual assumptions.

Mark:

- `a[href]`
- `input`, `textarea`, `select`
- `label` that contains a real input
- submit/reset buttons inside a real form
- buttons with explicit behavior such as `onclick`

Do not mark:

- Visual-only buttons without navigation or event logic
- Decorative icons
- Static cards without links
- Disabled or placeholder controls

The marker layer should be optional and should not interfere with reviewing the prototype. A draggable "交互提示" tag is acceptable.

## Business Flow Diagrams

When generating page-flow overview images:

- Use fresh screenshots from the current `index.html`.
- Hide the interaction hint control while capturing screenshots.
- Save the diagram into `preview-screenshots/`.
- Prefer orange orthogonal connectors.
- Draw connectors from the concrete triggering button or link.
- Use orange outlines to identify the trigger target.
- Use concise action labels such as `点击「调研问卷」`.
- Do not copy unrelated layout or styling from a user's example unless requested; only borrow the connection style the user names.

## Verification Checklist

After meaningful changes, verify:

- The intended hash routes work.
- The main clickable path works end to end.
- The page has no horizontal scroll at a mobile viewport.
- Text does not obviously overlap.
- Screenshots or flow diagrams are updated when visual behavior changed.

Prefer browser automation or local Chrome screenshots when available.
