# Mobile Prototype Drawing Rules

## Single-File Rule

Default to one HTML file. Use the HTML file that the user currently has open or explicitly names; otherwise prefer `index.html`.

Do not create additional HTML files unless the user explicitly asks for a separate file.

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

- Main prototype: the current user-opened HTML or `index.html`
- Final flow diagrams and explicitly requested screenshots: `preview-screenshots/`
- Project policy: `AGENTS.md`, if present

Never leave generated screenshots or temporary preview images in the project root.

## Screenshot Policy

Do not generate standalone page preview screenshots after every prototype edit by default.

Use browser automation for verification, but avoid saving one screenshot per page/state unless the user explicitly asks for screenshots.

When the user asks for a business flow diagram, page-flow overview, or final prototype overview:

- Capture the current HTML page states at that moment.
- Hide temporary review controls such as the interaction hint toggle.
- Generate one integrated image that shows the relevant states and transitions.
- Save that final diagram into `preview-screenshots/`.

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
- buttons with explicit behavior such as click handlers that open modals, change state, submit, or navigate

Do not mark:

- Visual-only buttons without navigation or event logic
- Decorative icons
- Static cards without links
- Disabled or placeholder controls
- Browser-shell display controls that do not do anything, such as a static `...` menu in a simulated external browser

If an element is only visual, implement it as a static element such as `span` or `div`, not as `a[href]` or an empty click handler.

The marker layer should be optional and should not interfere with reviewing the prototype. A draggable "交互提示" tag is acceptable.

### Hotspot Mask Behavior

- Use subtle visual masks or outlines; avoid adding persistent text labels over the prototype unless the user asks for them.
- The mask layer must not intercept pointer events.
- Anchor masks by document/page coordinates, not only viewport coordinates, so masks stay attached to top-bar controls during scroll.
- Clip masks to the page width and height to avoid creating horizontal scroll.
- Re-render masks after route changes, state changes, modal open/close, resize, and scroll.

### Modal Hotspots

When a modal is open:

- Mark only the real interactive elements inside the modal.
- Do not mark controls behind the modal overlay.
- Ensure the mask layer is visually above the modal card but still does not intercept clicks.
- After the modal closes, rescan interactions so modal button masks do not remain.

## External Link and Browser-Shell Flows

For external-link handoffs:

- Keep confirmation pages, browser-open pages, and external-form pages inside the same HTML unless the user asks otherwise.
- Treat simulated browser controls as business interactions only when they have a defined behavior.
- If the user states a close/back behavior, implement the exact hash target. For example, an external browser close button may return to the questionnaire list rather than the external-link confirmation page.

## Business Flow Diagrams

When generating page-flow overview images:

- Use fresh screenshots from the current HTML.
- Capture required page states only at diagram-generation time; do not rely on old per-page screenshots from earlier prototype edits.
- Hide the interaction hint control while capturing screenshots.
- Save the diagram into `preview-screenshots/`.
- Prefer orange orthogonal connectors.
- Draw connectors from the concrete triggering button or link.
- Use orange outlines to identify the trigger target.
- Use concise action labels such as `点击“调研问卷”`.
- Do not copy unrelated layout or styling from a user's example unless requested; only borrow the connection style the user names.

## Verification Checklist

After meaningful changes, verify:

- The intended hash routes work.
- The main clickable path works end to end.
- Modal buttons open, navigate, or close as intended.
- External browser close/back behavior matches the latest business rule.
- The page has no horizontal scroll at a mobile viewport.
- Text does not obviously overlap.
- Hotspot hints mark only true interactions, including modal-only behavior.
- Do not save standalone page screenshots just because visual behavior changed.
- Flow diagrams are updated when the user asks for a business/page-flow overview.

Prefer browser automation or local Chrome/Edge screenshots when available.
