# Conventions

How to write code in this repo. Match surrounding files unless strong reason not to.

## Scope
These rules apply to this gateway repo only. They do **not** define conventions for
separate Svelte + Supabase app repo on Vercel.

## Language and stack
- Plain HTML, CSS, and vanilla JavaScript.
- No framework assumptions in this repo.
- Keep static files browser-native and dependency-light.

## Files and naming
| Kind | Convention | Example |
|------|-----------|---------|
| HTML page | lowercase filename | `index.html`, `qr2.html` |
| Data file | lowercase filename | `menu.json` |
| JS constant | `UPPER_SNAKE` | `APP_URL`, `MENU_URL` |
| CSS class | short readable lowercase | `card`, `brand`, `qr-tile` |
| Asset file | lowercase filename | `lokocoffee.svg`, `qrcode.js` |

## HTML
- Use semantic HTML where practical.
- Keep `<html lang="id">` for user-facing pages.
- Include fallback links on gateway pages that depend on JS or external embed.
- Add accessible text for images and inline SVG used as meaningful brand marks.
- Keep script blocks near use site for simple one-page behavior.

## JavaScript
- Keep logic tiny and obvious.
- Prefer one obvious constant for target URLs near bottom script block.
- Use `const` by default; use `var` only when matching existing legacy file style or
  when patching file with deliberate consistency choice.
- Prefer tiny, direct embed/redirect logic. Avoid multi-step client scripting unless needed.
- Do not introduce build tooling only to solve trivial static-page logic.

## CSS
- Co-locate styles inside page for small standalone files.
- Preserve existing brand palette and typography from `design.md`.
- Avoid decorative complexity; these are utility pages with brand polish.
- Optimize for mobile viewport first and clean print output on QR pages.

## Data
- Treat `menu/menu.json` as structured menu reference/snapshot.
- Preserve object shape unless coordinated with downstream Svelte app expectations.
- Keep stable ids like `section.id` intact when editing content.
- If repo stops owning menu snapshot, document that removal in `tech.md`.

## Copy and language
- User-facing copy stays Indonesian unless specific brand phrase is already English
  by design, like `Scan for Menu`.
- Code, identifiers, comments, commit messages, and steering docs stay English.
- Keep fallback/loading copy short and clear.

## Third-party code
- `menu/qrcode.js` is vendored library code.
- Avoid style-only edits inside vendored library.
- If upgrading library, note source/version in PR or commit context.

## Quality bar
- Redirect flow must still work after edits.
- QR output must remain scannable on phone camera and printable at reasonable size.
- Keep no-JS fallback usable when page is meant to redirect.
- Avoid broken brand assets or missing external links.

## Git
- Branch off `main`; do not commit directly to `main`.
- Use Conventional Commits: `feat:`, `fix:`, `docs:`, `chore:`, `refactor:`, `style:`.
- Keep commits focused.
- Update relevant steering docs in same PR when architectural direction changes.
- End commit messages with trailer:
  `Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>`
