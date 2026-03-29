# Changelog

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
