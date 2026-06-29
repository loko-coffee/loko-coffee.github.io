# Design

Repo-owned UI is small, but still branded. Visual language stays **warm-dark,
typographic, and quiet**. It should feel like same brand family as downstream menu
app, even though this repo only owns redirect and QR surfaces.

## Applies to
- `menu/qr.html`
- `menu/qr2.html`
- `menu/index.html` iframe shell and fallback state

## Visual system

### Color
Keep current palette:

```css
#0c0a08   /* page background */
#15110e   /* card / surface */
#efe7da   /* light tile / primary contrast text */
#f2ecdf   /* strong display text */
#9a8c79   /* muted brand text */
#7e7263   /* faint mono copy */
#6f6456   /* dim footer copy */
#d6b26a   /* only accent */
rgba(255,255,255,.08-.10) /* hairline borders */
```

Rules:
- Gold is only accent color.
- Near-black background + warm cream contrast should dominate.
- Borders stay thin and quiet.

### Typography
Current pages use:
- **Instrument Serif** for headline/display.
- **Archivo** for body, brand labels, and action text.
- **Space Mono** for captions, meta, and footer copy.

Keep wide tracking on labels and mono captions. If letter-spacing is wide, add
matching left padding so centered text still looks visually centered.

### Shape and spacing
- Centered single-card composition.
- Card width about `360px` max.
- Large cream QR tile inside dark card.
- Soft corners: bigger card radius than QR/logo inner elements.
- Plenty of vertical breathing room; avoid cramped utility layouts.

## Component patterns

### Iframe host page
`menu/index.html` is functional first:
- Center phone-width frame on dark background.
- Give frame real full height (`100dvh`) so embedded app using `100dvh` measures
  useful viewport height.
- Keep shell visually quiet; downstream app is primary content.
- Keep fallback link visible for no-JS or failed embed cases.

Do not overdesign iframe host page; sizing and reliability matter more.

### QR card
QR pages are primary designed surfaces in this repo:
- Dark branded card centered on dark page background.
- Logo and brand line near top when layout uses them.
- Display headline in serif: "Scan for Menu".
- Small mono/localized helper copy under headline.
- Large cream QR tile with dark QR modules.
- Footer with location/brand copy in restrained mono style.

### QR logo overlay
`qr2.html` adds centered logo over QR.

Rules:
- Use higher QR error correction when center logo covers modules.
- Logo tile should contrast against QR enough for scanning reliability.
- Decorative overlay never takes precedence over scan success.

## Motion
- Almost none.
- No heavy animation, bounce, or parallax.
- Static printable pages should remain print-safe.

## Print behavior
QR pages must print cleanly:
- Preserve colors with print color adjustment.
- Remove unnecessary page chrome.
- Keep card centered and uncluttered.
- Ensure QR remains high contrast when printed.

## Do / don't
- Do keep repo-owned pages visually aligned with L'oko brand.
- Do favor typography, spacing, and contrast over extra decoration.
- Do size iframe host like real phone-width app shell.
- Do preserve scan reliability above aesthetic tweaks.
- Don't add extra accent colors, gradients, or photo backgrounds.
- Don't turn gateway pages into complex app shells.
