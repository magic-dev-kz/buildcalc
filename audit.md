# BuildCalc Audit Report
**Auditor:** House (OpenClaw)
**Date:** 2026-03-29
**File:** `/Volumes/OpenClaw/sandbox/projects/buildcalc/index.html`

---

SCORE: 6.8/10

---

### Kritical bugs (blockers)

1. **XSS via `showResult` detail parameter uses `innerHTML` with `escapeHtml` -- but `renderProjects` injects `onclick` with `escapeHtml(p.id)` which is NOT safe.** Line 944: `'<button onclick="window._deleteProject(\''+escapeHtml(p.id)+'\')"'` -- `escapeHtml` escapes HTML entities but NOT JavaScript string context. A project ID crafted as `'); alert('xss` would break out of the string. Since `p.id` is generated internally via `Date.now().toString(36)+Math.random()...` (line 915), the actual risk is low, but this is still an architectural anti-pattern: HTML-escaping is NOT JS-escaping. If project data ever comes from an external source (share feature, import), this is exploitable.

2. **`getNum()` rejects zero values (line 601): `return isNaN(v)||v<0?null:v;` -- but the check on callers is `if(!l||!w)` which treats `0` as falsy/null.** This means entering `0` for any dimension silently fails with "Please enter..." toast instead of calculating. Zero IS a valid input that should produce 0 result (or explicit "value must be > 0" message). This affects ALL calculators.

3. **`getNum()` rejects negative numbers but `min="0"` on inputs doesn't prevent typing negatives.** A user can type `-5` -- getNum returns null, toast says "Please fill fields" without explaining WHY. No field-level validation feedback.

---

### Serious bugs

1. **Paint calculator: door/window area constants hardcoded for feet are wrong.** Line 773: `doorAreaM=unit==='ft'?17.22:1.6` -- 1.6 m^2 = 17.22 sq.ft? Let's check: 1.6 * 10.7639 = 17.22. OK, the conversion is correct. However, the naming `doorAreaM` is misleading -- when unit is `ft`, this value is in sq.ft, not in meters. This is confusing but not a calculation bug. **The real issue: coverage is ALWAYS in m^2 (line 768, options say "10 m^2", "40 m^2", "65 m^2"), but `netAreaM` is converted to m^2 before multiplication by coats (line 778). This is correct.** No calculation bug here after deeper analysis.

2. **L-shape area: formula is `totalWidth * totalHeight - cutoutWidth * cutoutHeight`.** This is ONLY correct for a specific L-shape configuration where the cutout is in one corner. The visual (lines 244-247) shows cells A, B, C with one empty corner, which matches. However, there's no validation that cutout dimensions make geometric sense -- `cutoutWidth` could be larger than `totalWidth` (line 720 checks `cw>=tw` which is good) but there's no check that cutout height + remaining height = total height. The formula itself is mathematically correct for the depicted shape.

3. **Tile calculator mixes units unsafely.** Room area is entered in user-selected units (m or ft), but tile size is ALWAYS in cm. Line 746: `roomAreaM=unit==='ft'?toSqMeters(roomArea,'ft'):roomArea` converts room to m^2, then `tileArea=(tl/100)*(tw/100)` converts cm to m^2. This is correct. BUT: if user enters room in meters and tile in cm, and tile dimensions are e.g. 600 (meaning 600mm, user confused cm vs mm), there's no sanity check. A tile of 600cm = 6m seems absurd but is accepted silently.

4. **Save functionality only exists for Area calculator.** Only `save-form-area` exists in HTML (line 281). No save forms for Tile, Paint, Flooring, or Concrete. The `showSaveForm` function (line 952) is called only from `calcArea`. **Users cannot save Tile/Paint/Floor/Concrete calculations.** This is a significant feature gap given that "Save Projects" is promoted in onboarding.

5. **`window._deleteProject` is exposed globally (line 987).** The entire app is wrapped in IIFE for encapsulation, but then a delete function is leaked to global scope via `window._deleteProject`. This is needed because `onclick` inline handlers are used in `renderProjects`. Should use event delegation instead.

6. **Onboarding dialog: no Escape key handler.** The onboarding overlay (lines 680-689) can only be dismissed by clicking "Start Building". Pressing Escape does nothing. The Escape handler on line 971 only targets `projects-panel`. Focus trap is also missing -- Tab can escape the onboarding dialog.

---

### Minor bugs

1. **`toSqMeters` function is redundant/confusing.** Line 591: `toSqMeters(val,unit)` takes a value already in sq.ft and converts to m^2. But the name suggests it converts any unit to sq.meters. Meanwhile `toMeters` (line 586) takes a linear value. The inconsistency is confusing for maintenance.

2. **Tab navigation: arrow keys switch and click tabs.** Line 664: `tabs[idx+1].focus();tabs[idx+1].click();` -- per WAI-ARIA tabs pattern, arrow keys should move focus, but activation should happen on focus (automatic activation) OR on Enter/Space (manual activation). Current behavior is automatic activation, which is fine, but there's no `tabindex` management: all tabs are focusable via Tab key, but the ARIA pattern says only the active tab should be in tab order (`tabindex="0"`) and others should be `tabindex="-1"`.

3. **Area calculator: `areaSqFt` conversion for meters input.** Line 713: `areaSqFt=unit==='m'?area/0.092903:area` -- dividing by 0.092903 to get sq.ft from m^2. Correct (1 m^2 = 10.7639 sq.ft, and 1/0.092903 = 10.7639). OK, this is fine.

4. **Concrete calculator doesn't suggest waste/spillage in the actual calculation.** Line 849 says "Add 5-10% for spillage" as text, but doesn't add it to the bag count. Users might buy exactly the calculated amount and run short.

5. **Unit converter: negative values are accepted.** `getNum` is NOT used in converter -- `parseFloat` is used directly (lines 883, 890). Negative feet/meters are technically meaningless for construction but the converter happily converts them.

6. **`lastAreaResult` is declared with `var` at module scope (line 701) but outside any function.** It's inside the IIFE so not truly global, but it's a module-level mutable variable with no clear lifecycle management.

7. **CSS: `.form-row-3` collapses to 1 column on mobile (line 158)** which makes the Concrete calculator's L/W/Thickness stack vertically. This is fine for usability but the labels don't indicate units inline, so the user has to scroll up to see the unit selector.

8. **No `<meta name="apple-mobile-web-app-capable">` or manifest.json** -- onboarding claims "Works Offline" but there's no Service Worker or PWA manifest. Offline claim is misleading. The page will work from browser cache, but there's no guaranteed offline support.

---

### UX remarks

1. **No input validation feedback on individual fields.** When a value is missing, only a generic toast appears. The offending field is not highlighted, not focused, no red border. User has to guess which field is the problem.

2. **No real-time calculation.** User must click "Calculate" button every time. Modern calculators update results as you type (debounced). The converter does this (line 865: `'input'` event), but all other calculators require explicit button press.

3. **Results disappear when switching tabs.** If user calculates area, switches to Tile tab, then back -- the result is still visible (DOM persists). Good. But `lastAreaResult` persists only in memory, not across page reloads.

4. **No "Copy result" button.** Users on job sites often need to text or message results to someone. A copy button would be very useful.

5. **No unit persistence.** If user selects "Feet" in Area calculator, they have to re-select it in Tile, Paint, etc. Each calculator has its own unit selector with no synchronization.

6. **Footer is inside `<main>` (line 522-526).** Semantically, footer should be outside main. Minor HTML semantics issue.

7. **Tile waste defaults to 10% but allows 0%.** 0% waste is unrealistic for any tile job. Could at minimum show a warning.

8. **No "What is this?" tooltips.** New users may not understand "Cutout Width (B)" in L-shape, or what "Pack Coverage" means for flooring.

9. **Project delete has no confirmation dialog.** Clicking delete immediately removes the project. One accidental tap = data gone.

10. **Color contrast in dark mode.** `--text-muted:#887060` on `--bg:#1A1210` -- the muted text (#887060) on dark background (#1A1210) has contrast ratio of approximately 3.2:1, which fails WCAG AA for normal text (requires 4.5:1).

11. **Onboarding is skipped on second visit** even if user cleared cache or uses different browser profile. Not a bug per se, but the `localStorage` flag means a user on the same browser who clears data sees it again (intended), while a user on a new device never sees it (also intended).

---

### What's excellent

1. **Clean, warm design.** The color palette (#E8722A orange, warm beige #FDF6F0) is perfect for a construction tool. Feels premium, not generic.

2. **Dark mode is well-implemented.** CSS custom properties, smooth transitions, proper contrast for primary elements. The toggle is elegant.

3. **IIFE + 'use strict'.** Proper JavaScript encapsulation (line 571). No global pollution except the intentional `window._deleteProject`.

4. **`escapeHtml()` exists and is used consistently** in `showResult` output. The DOM-based escaping approach (createElement + textContent) is robust.

5. **`localStorage` wrapped in try/catch** (lines 617, 622). Handles private browsing mode and quota exceeded gracefully.

6. **ARIA attributes are present and mostly correct.** `role="tablist"`, `role="tab"`, `aria-selected`, `role="tabpanel"`, `aria-controls`, `aria-labelledby`, `aria-live="polite"` on results, `aria-modal` on dialogs, `aria-label` on buttons.

7. **Skip link** (line 172-173) for keyboard/screen reader users. Nice touch.

8. **Enter key triggers calculation** (lines 1009-1017) from any input in the active section. Good keyboard UX.

9. **Responsive design is solid.** Works from 360px mobile (tested via CSS review: `@media(max-width:480px)` collapses grids). Horizontal tab scrolling with `-webkit-overflow-scrolling:touch`.

10. **Single HTML file, zero dependencies.** No build step, no CDN, no framework. Loads instantly. This is a genuine advantage over bloated competitors.

11. **Onboarding screen** is welcoming and clearly explains value proposition. Good first impression.

12. **Conversion factors are accurate.** Checked: ft->m (0.3048), in->cm (2.54), sqft->m^2 (0.092903), yd->m (0.9144), cuft->m^3 (0.0283168). All correct.

---

### Recommendations

1. **[P0] Fix zero-value handling.** Change `getNum` to distinguish between "empty/invalid" and "zero". Either: accept 0 and calculate (result will be 0), or reject 0 with explicit message "Value must be greater than 0". Current behavior where 0 is silently rejected is confusing.

2. **[P0] Add save functionality to ALL calculators**, not just Area. This is a headline feature from onboarding that's 80% missing.

3. **[P1] Add Service Worker for true offline support.** The onboarding promises "Works Offline" -- deliver on that promise. A simple SW that caches the single HTML file is ~20 lines of code.

4. **[P1] Replace inline `onclick` in renderProjects with event delegation.** Remove `window._deleteProject` global. Use `data-id` attributes and a single click listener on `#projects-list`.

5. **[P1] Fix dark mode text contrast.** Increase `--text-muted` in dark theme from `#887060` to at least `#A09080` for WCAG AA compliance.

6. **[P2] Add focus trap to onboarding dialog.** Also handle Escape key to dismiss it.

7. **[P2] Add field-level validation.** Red borders on empty/invalid fields, not just a toast. Focus the first invalid field.

8. **[P2] Add "Copy result" button** next to each result.

9. **[P2] Synchronize units across calculators.** When user picks "Feet" in one calc, offer to set it globally.

10. **[P2] Add confirmation dialog for project deletion.**

11. **[P3] Fix tab ARIA pattern.** Set `tabindex="-1"` on inactive tabs, `tabindex="0"` on active tab.

12. **[P3] Move footer outside `<main>`.** Minor semantic fix.

13. **[P3] Add input sanity warnings.** E.g., tile dimensions > 200cm, room dimensions > 100m, negative values in converter.

14. **[P3] Consider real-time calculation** (on input event with debounce) for all calculators, not just the converter.

---

### Competitor comparison: BuildCalc vs Construction Master ($36/yr)

| Feature | BuildCalc | Construction Master |
|---------|-----------|-------------------|
| Price | Free | $35.99/year |
| Platforms | Web (any device) | iOS, Android |
| Offline | Partial (cache) | Full native |
| Calculators | 6 | 20+ |
| Materials database | No | Yes |
| Cost estimation | No | Yes |
| Blueprint/drawing | No | Yes |
| Save projects | Partial (1 of 6) | Full |
| Unit conversion | Yes | Yes |
| Stair/rafter calc | No | Yes |

**Verdict:** BuildCalc is NOT a replacement for Construction Master for professionals. It covers ~30% of CM's functionality. However, for a homeowner doing a kitchen renovation, BuildCalc gives exactly what they need for $0. The value proposition is clear: "good enough for DIY, free forever."

**To win:** Focus on the DIY/homeowner niche. Add cost estimation (price per tile/can/bag), a materials shopping list, and photo measurement (upload room photo -> get dimensions). Those three features would make BuildCalc genuinely superior for its target audience.

---

*End of audit. No courtesy points were given.*
