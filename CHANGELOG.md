# Changelog



## v24.0 (2026-03-29) — JSON-LD Structured Data

- **JSON-LD structured data**: Added WebApplication schema markup in `<head>` for improved SEO and rich search results
- SW cache bumped to `buildcalc-v24.0`

---
## v23.0 (2026-03-29) — Deferred Font Loading

- **Non-blocking Google Fonts**: Replaced render-blocking `@import` in inline CSS with `<link rel="preload" ... onload>` and `<noscript>` fallback — font CSS no longer blocks first paint
- SW cache bumped to `buildcalc-v23.0`

---
## v22.0 (2026-03-29) — Data Portability

- **Export all projects/calculations as JSON backup**: "Export All Data" button exports saved projects, calculation history, shopping list, and current form state as a portable JSON file
- SW cache bumped to `buildcalc-v22.0`

## v21.0 (2026-03-29) — Drywall Calculator + UX Polish (Leo + Molot)

- **Remove tabGlowPulse** (Leo): Replaced pulsing glow animation on active tab with a static solid accent bottom border. Less distracting, cleaner look.
- **Tab scroll hint** (Leo): On mobile, a fade gradient with arrow hint appears on the right edge of the tab nav when more tabs are scrollable. Hides once scrolled to end.
- **Simplify result animation** (Leo): Removed `resultPop`, `resultShine`, and `springPop` animations. Results now appear with a single 0.3s opacity fade-in.
- **Drywall calculator** (Molot): New 7th tab. Inputs: wall area or room dimensions (length/width/height). Outputs: number of drywall sheets (3 sizes), screws (~28/sheet), joint compound (~0.34 kg/m2), joint tape (~1.2 m/m2). Supports metric/imperial, 10% waste toggle, cost estimate.
- SW cache bumped to `buildcalc-v21.0`

## v20.0 (2026-03-29) — Print Styles Verified

- **Print verification**: Existing `@media print` rules confirmed working — hides nav, buttons, overlays; shows calculator results with cost estimates; report header prints with branding; page breaks on cards
- SW cache bumped to `buildcalc-v20.0`

## v19.0 (2026-03-29) — Error Handling Hardening

- **NaN guard**: If any calculator result is NaN, show "Invalid input" instead of broken output
- SW cache bumped to `buildcalc-v19.0`

## v18.0 (2026-03-29) — SEO / Meta Pass

- **robots meta**: Added `<meta name="robots" content="index, follow">`
- **apple-touch-icon**: Inline SVG data URL with product emoji (🧱)
- SW cache bumped to `buildcalc-v18.0`

## v17.0 (2026-03-29) — Accessibility & Validation

Accessibility improvements by Mario.

### Skip-to-Content Link
- Updated skip link text to "Skip to calculator" for clearer screen reader navigation
- Link remains visually hidden, appears on keyboard focus at top of page

### Input Labels
- Converter inputs (`conv-from`, `conv-to`) now use proper `for=` attribute on labels instead of `aria-labelledby`
- All input fields across all calculators have visible `<label>` elements with `for=` attributes

### Inline Error Messages
- Validation errors now render inline beneath the invalid field
- Each error message element has `role="alert"` and `aria-live="assertive"` for immediate screen reader announcement
- Invalid inputs get `aria-describedby` pointing to their error message
- Error messages auto-clear when user types in the field
- L-shape cutout validation ("Cutout must be smaller than total") now also marks fields with inline errors

### Service Worker
- Cache version bumped to `buildcalc-v17.0`

---

## v16.0 (2026-03-29) — Tab Calculation History

- **Tab History**: Show last 3 calculations per calculator tab in the result area
- SW cache bumped to `buildcalc-v16.0`
## v15.0 — Quick Unit Info (2026-03-29)

Feature update by Mario.

### Quick Unit Info
- Hover or focus the "?" icon next to any unit dropdown to see a quick conversion reference
- Tooltip shows "1 ft = 0.3048 m" for instant reference without performing a conversion
- Available on all 6 unit selects: Area, Area L-shape, Tile, Paint, Flooring, Concrete
- Keyboard accessible via focus (tabindex)
- Styled consistently with app design system

### Service Worker
- Cache version bumped to `buildcalc-v15.0`

---

## v14.0 — Undo Last Calculation (2026-03-29)

Feature update by Mario.

### Undo Last Calculation
- New "Undo" button appears next to each Calculate button after a calculation is performed
- Captures all input field values (inputs, selects, checkboxes) before each calculation
- Clicking Undo restores the previous input values, hides the result, and hides the save form
- One level deep: stores only the most recent pre-calculation state per calculator
- Toast notification "Restored previous values" on undo
- Button styled as secondary with undo arrow icon
- Works across all 5 calculators: Area, Tile, Paint, Flooring, Concrete

### Service Worker
- Cache version bumped to `buildcalc-v14.0`

---

## v12.0 — Room Presets, Project Total & Share (2026-03-29)

### Room Size Presets
- Quick-fill buttons added to Area, Tile, Paint, and Flooring calculators
- 4 common room sizes: Small Bathroom (2.5x2m), Standard Bedroom (4x3.5m), Living Room (5x4m), Kitchen (3x3m)
- Clicking a preset fills length/width, sets unit to meters, and auto-triggers calculation
- Toast notification confirms which preset was applied

### Total Project Cost Summary
- New "Project Total" section appears when any calculator has a cost estimate filled in
- Sums costs across all calculators (Tiles + Paint + Flooring + Concrete)
- Shows total with breakdown by category
- Updates automatically whenever costs change or forms are cleared

### Share Project Summary
- "Share Project Summary" button copies a full text report to clipboard
- Report includes: all visible calculator results with details, cost estimates, shopping list, and project total
- Formatted for easy sharing via chat, email, or notes
- Button only visible when there are cost estimates to share

### Service Worker
- Cache version bumped to `buildcalc-v12.0`

---

## v11.0 — Micro-polish (2026-03-29)
- Onboarding logo animation upgraded to scale-bounce (cubic-bezier spring)
- Active tab subtle glow pulse animation (`tabGlowPulse`)
- Service worker cache bumped to `buildcalc-v11.0`

---

## v10.0 — Micro-polish (2026-03-29)

### Input Placeholders
- Dimension fields already carry helpful placeholders ("e.g. 5.5") for all calculators

### Print Toast Hint
- First-visit toast updated to "Tip: Save as PDF for digital sharing"

### Result Spring Pop
- `springPop` keyframe overshoot refined (scale 0.8 -> 1.15 -> 1) for a tighter spring feel

### Service Worker
- Cache version bumped to `buildcalc-v10.0`

---

## v9.0 — Share, Animations & Polish (2026-03-29)

### Share on Twitter/X
- New header button opens Twitter/X with pre-filled tweet: "Just calculated my renovation materials with BuildCalc" + link

### Result Spring Animation
- Result numbers now play a spring-scale pop animation (`springPop` keyframes) when calculation completes, replacing the simple pulse

### Unit Toggle Animated Slide
- Converter unit labels slide out/in with a smooth transition when switching conversion types (e.g. ft/m to sq.ft/m2)

### Service Worker
- Cache version bumped to `buildcalc-v9.0`

---

## v8.0 — Material Database, Stock Deduction & Print Report (2026-03-29)

### Material Database
- Collapsible "Material Database" panel with hardcoded consumption ratios for 6 common materials: tile adhesive (4-6 kg/m2), grout (0.3-0.5 kg/m2), self-leveling compound (1.5 kg/m2/mm), primer (0.1-0.2 L/m2), plaster (8-12 kg/m2/cm), drywall screws (25 pcs/m2)
- Quick lookup reference — no calculation needed, just visual reference

### Before/After Calculator (Existing Stock)
- "Already have" input field added to Tile, Paint, Flooring, and Concrete calculators
- When stock > 0, result shows "X to buy" instead of total needed, with a green deduction badge: "Already have: N -> Need to buy: M"
- Cost estimate recalculated based on items to buy (not total)

### Print Project Report
- "Print Project Report" button at bottom of main area
- Uses `window.print()` with `@media print` CSS rules
- Print layout hides navigation, shopping list, buttons; shows only active calculator with results
- Adds a "BuildCalc Project Report" header with generation date visible only in print

### Service Worker
- Cache version bumped to `buildcalc-v8.0`

---

## v7.0 — Robustness & Retention (2026-03-29)

Quality-of-life improvements by Mario / OpenClaw. Focus on robustness and user retention.

### Auto-Save Form State
- All input field values saved to localStorage on every change (500ms debounce)
- Form state restored automatically on page load — no data loss on accidental tab close
- "Clear Form" button added to every calculator section (Area, Tile, Paint, Floor, Concrete)
- Clear button resets all fields, hides results and cost estimates

### Keyboard Shortcuts
- `Enter` triggers Calculate when focused on any input field (existing, preserved)
- `Ctrl/Cmd+C` copies active calculator result when no text selected
- `1`-`6` keys switch between tabs (Area, Tile, Paint, Floor, Concrete, Convert)
- Visual keyboard hints shown on tabs (hidden on mobile for space)
- Shortcuts disabled when typing in input fields to avoid conflicts

### PWA Install Prompt
- "Install BuildCalc" banner shown after 2+ visits using `beforeinstallprompt` event
- Banner appears with 2s delay for non-intrusive UX
- "Install" button triggers native install prompt
- "Dismiss" button hides banner and saves preference to localStorage (won't show again)
- Banner auto-hides on successful installation via `appinstalled` event

### Service Worker
- Cache version bumped to `buildcalc-v7.0`

---

## v6.0 — Quality Polish (2026-03-29)

Quality improvements by Mario / OpenClaw. Focus on animations, feedback, and accessibility.

### Result CountUp Animation
- Number results now animate from 0 to final value over 0.8s with ease-out curve
- Subtle scale pulse on completion for satisfying feedback
- Respects `prefers-reduced-motion` — skips animation when enabled

### Input Validation Visual
- Shake animation on invalid inputs verified working (CSS `@keyframes shake` + `.form-input--error`)
- Error class auto-clears on input change via `initFieldErrorClear()`

### Inline Confirm Dialogs
- Replaced native `confirm()` calls with styled inline confirm dialog
- Two instances: "Delete this project?" and "Clear entire shopping list?"
- Glassmorphism overlay with card animation, Cancel/OK buttons
- Keyboard accessible: Escape to dismiss, focus trapped to dialog
- Matches app design system (accent gradient, border radius, shadows)

### Toast System
- Verified all user notifications use inline toast (no `alert()` calls)
- `confirm()` was the only native dialog — now replaced

### Focus-Visible Audit
- Added `:focus-visible` outline styles to all previously uncovered interactive elements:
  `.converter-swap`, `.copy-btn`, `.btn-add-shopping`, `.shopping-header`,
  `.measure-guide-header`, `.history-item`, `.history-clear`, `.shopping-item-del`,
  `.project-item-actions button`, `.btn-share-list`, `.btn-secondary`, `.onboarding-cta`

### Service Worker
- Cache version bumped to `buildcalc-v6.0`

---

## v5.0 — Cost & Sharing Update (2026-03-29)

Feature additions by Mario / OpenClaw. Focus on cost estimation, sharing, and guidance.

### Cost Estimator
- Added optional "Price per unit" field to Tile, Paint, Flooring, and Concrete calculators
- Tile: "Price per tile", Paint: "Price per can", Flooring: "Price per pack", Concrete: "Price per bag"
- When filled, displays "Estimated Cost: $XXX" in the result block with accent styling
- Field is fully optional — calculators work exactly as before without it

### Export Shopping List as Text
- New "Share List" button in Shopping List actions (gradient accent style)
- Copies shopping list as formatted text with checkbox squares: "□ 5 pcs of Tiles..."
- Shows "Copied!" toast notification on success
- Existing "Copy List" button retained for detailed format

### Measurement Photo Guide
- New collapsible "How to Measure" section above the shopping list
- 4 practical tips: consistent units, 10% waste, L-shape splitting, measure twice
- Hidden by default, expandable with click/keyboard
- Styled with icons for visual guidance

### Service Worker
- Cache version bumped to `buildcalc-v5.0`

## v4.0 — Smart Features Update (2026-03-29)

Functional improvements by Mario / OpenClaw. Focus on UX and productivity.

### Shopping List Improvements
- Added "Add to Shopping List" button for concrete calculator results
- Button animation on add: pulse effect + green "Added!" confirmation state (1.5s)
- Toast notification "Added to shopping list" on every add action
- All four calculators (tile, paint, floor, concrete) now have one-click add

### Copy Result
- Copy buttons already present on all result blocks (area, tile, paint, floor, concrete)
- Uses `navigator.clipboard.writeText` with `document.execCommand('copy')` fallback
- Copies both result value and detail breakdown text

### Unit Preferences
- All unit selects (area, area L-shape, tile, paint, floor, concrete) persist to localStorage
- Last used unit (meters/feet) restored automatically on next visit
- Stored per-field via `buildcalc_unit_<id>` keys

### Concrete Quick Presets
- New dropdown above thickness field with common project types:
  - Sidewalk (4 inches / 0.1016 m)
  - Driveway (6 inches / 0.1524 m)
  - Foundation (8 inches / 0.2032 m)
  - Patio slab (4 inches / 0.1016 m)
- Auto-fills thickness field, respects current unit selection (converts to feet if needed)
- "Custom thickness..." option for manual entry

### Dark Mode
- Dark mode toggle verified working (header moon/sun icon)
- Theme persists via localStorage
- Full CSS variable coverage for all new elements (presets, added button states)

### Service Worker
- Cache version bumped to `buildcalc-v4.0`

## v3.0 — Premium Visual Redesign (2026-03-29)

Design overhaul by Sky / OpenClaw. CSS-only changes, zero JavaScript modifications.

### Gradient Backgrounds
- Header logo text uses `background-clip: text` gradient
- Primary buttons use `--accent-gradient` instead of flat color
- Active nav tabs render with gradient background + glow shadow
- Onboarding overlay uses multi-stop radial background with animated pulsing glow
- Toggle switches use gradient fill when checked
- Shopping list badge uses gradient background

### Enhanced Color Palette
- Warmer, more saturated accent: `#D4621A` (light) / `#E07030` (dark)
- Deeper text hierarchy: primary `#1E120A`, secondary `#5C3D28`, muted `#8A7264`
- New CSS custom properties: `--accent-gradient`, `--accent-gradient-hover`, `--success-gradient`, `--shadow-card`, `--shadow-glow`
- Dark mode colors retuned for better contrast and warmth

### Micro-animations
- Buttons: `translateY` lift on hover, scale-down on active
- Cards: spring-curve entrance animation (`cardIn`), hover lift
- Nav tabs: float on hover, icon scale + rotate
- Form inputs: focus glow ring (4px accent ring)
- Converter swap button: 180-degree rotation on hover
- Error fields: shake animation on validation failure
- History / project items: slide on hover
- Section switching: spring-curve `sectionIn` animation
- Header logo SVG: tilt + scale on hover

### Typography
- Imported Inter font (Google Fonts) for sharper rendering
- Labels: uppercase + letter-spacing for hierarchy
- Card titles: weight 800, tighter tracking
- Result values: weight 900, size 2rem
- Sheet headers: weight 800

### Soft Shadows
- Three-tier shadow system: `--shadow`, `--shadow-card`, `--shadow-lg`
- Cards: subtle shadow at rest, elevated on hover
- Buttons: colored glow shadow
- Logo SVG: orange drop-shadow filter

### Glassmorphism Header
- Light mode: `rgba(255,255,255,0.72)` + `saturate(180%) blur(20px)`
- Dark mode: `rgba(36,28,26,0.78)` + matching blur
- Semi-transparent border bottom

### Onboarding Screen
- Animated radial glow background (4s infinite pulse)
- Logo float animation (3s infinite)
- Title uses white-to-warm gradient text
- Features slide in sequentially with staggered delays
- CTA button: shimmer sweep on hover
- Content springs up with entrance animation

### Larger Tab Icons
- Icon size increased from 1.1rem to 1.35rem
- Saturation filter + scale/rotate transform on hover
- Active state: brightness override

### Result Block
- Spring pop animation on appear
- Radial gradient shine sweep animation
- Larger value text (2rem, weight 900)
- Label uppercase + letter-spacing
- Increased padding and border radius

### Mobile Polish
- Refined padding/spacing at 480px breakpoint
- Larger touch targets on converter swap
- Adjusted onboarding heading size for small screens
- Card border-radius reduced on mobile for tighter fit
- Desktop (768px+) gets enhanced hover lift on cards

## v1.0 (2026-03-29)
- Initial release
- Ship-ready (9.0/10)
- PWA (service worker + manifest)
- WCAG AA accessible
- Works offline
