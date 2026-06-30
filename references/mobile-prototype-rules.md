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
- Hide temporary review controls such as the review tool, interaction hint toggle, waterdrop annotation toolbar, and annotation popovers.
- Generate the required final diagram image or images that show the relevant states and transitions.
- Save final diagrams into `preview-screenshots/`.

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

## Review Assistant And Waterdrop Annotations

Add review-assistant controls only when the user asks for interaction hints, waterdrop notes, content annotation, review annotation, or persistent review notes. Do not add these controls by default to every mobile prototype.

Use one draggable "评审工具" control instead of multiple always-visible buttons. When opened, it may expose:

- `交互提醒`: toggle the real-interaction hotspot layer.
- `水滴标注`: enter annotation mode.
- `清空标注`: clear persisted waterdrop notes after a confirmation.

Do not mark the review tool, its menu, annotation dialog, annotation droplets, or annotation popovers as business interaction hotspots.

### Waterdrop Creation

In waterdrop annotation mode:

- Let the user tap/click a point in the phone canvas, then open an annotation input dialog.
- Save only after the user enters non-empty content and confirms.
- Store droplets as page-relative coordinates. Prefer percentages over pixels so notes survive viewport changes.
- Let droplets be created on real interactive controls such as buttons, links, inputs, and modal actions when annotation mode is active.
- Intercept annotation-mode taps/clicks before the underlying control handles them. Creating a droplet must not trigger the annotated button, close a modal, navigate, submit, or run other prototype interactions.
- After annotation mode is turned off, restore normal prototype interactions without requiring a refresh.
- Allow droplets on modal content when the modal is the visible page state. Store enough state, such as `targetState` or `route`, so droplets created on a modal appear only with that modal state and hide when the modal is closed.
- Do not create droplets on the review tool, review menus, existing droplets, annotation dialogs, note popovers, or temporary masks outside the visible content state.
- If the prototype uses hash routes, record the route with each note and show only notes for the active route.

Use a compact data shape such as:

- `id`
- `xPercent`
- `yPercent`
- `text`
- `createdAt`
- optional `pageKey` or `route`
- optional `targetState` such as `page`, `modal:<id>`, or another visible-state key

### Waterdrop Viewing

Support both desktop and phone review:

- On desktop, show the note on `hover` and `focus`.
- On touch/mobile, show the note when the droplet is tapped.
- Provide a per-droplet delete action in the note popover when the user asks for removable annotations or when a clear-all control exists.
- Close the note popover when the user taps outside it, changes route, closes the related modal state, or turns off the review tool panel.
- Keep popovers inside the phone canvas and prevent horizontal scroll.

### Persistence

Persist notes in `localStorage` for the current prototype. Use a stable key based on the prototype name or page identity, for example:

```text
mobilePrototypeReview.<safePageName>.droplets.v1
```

Never store secrets, credentials, phone numbers, or customer data in the annotation payload. Treat waterdrop text as local review notes for the current browser only; do not imply cross-device sync.

### Review Tool And Hotspot Coexistence

When the review tool includes both interaction hints and waterdrop annotations:

- The review tool menu should open on hover/focus for desktop review and stay open while the pointer is over the button or the menu; close only after the pointer leaves the whole review tool area. A tap/click fallback may be kept for touch devices.
- Keep the hotspot mask layer non-intercepting with `pointer-events: none`.
- Keep droplet buttons interactive so notes can be viewed, but exclude droplets from hotspot scans.
- While waterdrop annotation mode is active, intercept taps/clicks in capture phase or an equivalent early handler so business controls underneath are not activated.
- If a modal is open, hotspot scans should inspect only real interactive elements inside the modal.
- After modal open/close, hash route changes, resize, and scroll, re-render hotspot masks, filter droplets to the current visible state, and reposition visible note popovers.
- When exporting screenshots or flow diagrams, hide the review tool, hotspot masks, droplets, annotation dialogs, and note popovers unless the user explicitly asks for an annotated review screenshot.

## External Link and Browser-Shell Flows

For external-link handoffs:

- Keep confirmation pages, browser-open pages, and external-form pages inside the same HTML unless the user asks otherwise.
- Treat simulated browser controls as business interactions only when they have a defined behavior.
- If the user states a close/back behavior, implement the exact hash target. For example, an external browser close button may return to the questionnaire list rather than the external-link confirmation page.

## Business Flow Diagrams

When generating page-flow overview images:

- Use fresh screenshots from the current HTML.
- Capture required page states only at diagram-generation time; do not rely on old per-page screenshots from earlier prototype edits.
- If the prototype has mutually exclusive business states, such as first sign-in versus already signed-in, do not merge those states into one flow diagram. Generate a separate flow diagram for each state, and include only the buttons, modals, and transitions that truly exist in that state.
- Hide the review tool, interaction hint control, waterdrop annotations, and annotation popovers while capturing screenshots.
- Save the diagram into `preview-screenshots/`.
- Prefer orange orthogonal connectors.
- Draw connectors from the concrete triggering button or link.
- Use orange outlines to identify the trigger target.
- Start and end connector lines at the outside edge of the orange target outline; do not let connector lines pass through the outlined button or hotspot box.
- Use concise action labels such as `点击“调研问卷”`.
- Place action labels in whitespace between screenshots or above/below screenshots. Labels must not cover prototype content, buttons, modals, fields, or other important UI. Reroute connector lines around screenshots when needed to keep labels off the prototype.
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
- Review tools do not mark themselves as interaction hotspots.
- Waterdrop annotations can be created on static content, real controls, and visible modal content without triggering the underlying control while annotation mode is active.
- Waterdrop annotations can be viewed by hover/focus or tap, deleted individually, cleared together, and restored after refresh when requested.
- Droplets created in modal states hide with that modal and reappear when that same state is visible again.
- Do not save standalone page screenshots just because visual behavior changed.
- Flow diagrams are updated when the user asks for a business/page-flow overview.

Prefer browser automation or local Chrome/Edge screenshots when available.
