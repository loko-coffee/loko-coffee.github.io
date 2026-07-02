# Structure

## Actual layout
Repo is intentionally small. Current filesystem:

```
.
├─ .ai/
│  ├─ README.md              # steering docs index
│  └─ steering/
│     ├─ product.md
│     ├─ tech.md
│     ├─ structure.md
│     ├─ design.md
│     └─ convention.md
├─ .github/
│  └─ workflows/
│     ├─ static.yml                  # deploy this repo to GitHub Pages on push
│     └─ supabase-keepalive.yml      # Mon/Fri ping to stop downstream Supabase pausing
├─ index.html                # root redirect -> /menu
└─ menu/
   ├─ index.html             # iframe shell -> external Vercel app
   ├─ menu.json              # structured local menu snapshot/reference
   ├─ qr.html                # branded QR card page
   ├─ qr2.html               # branded QR card variant with centered logo
   ├─ qrcode.js              # vendored QR generator library
   └─ lokocoffee.svg         # brand logo artwork
```

## File roles
- `index.html`: domain entrypoint. Keep tiny. Should do one job: send root visitors
  into repo-owned menu path.
- `menu/index.html`: downstream host page. Owns iframe container and external
  Vercel-hosted Svelte app URL.
- `menu/qr.html`: printable/shareable branded QR page with configurable `MENU_URL`.
- `menu/qr2.html`: second QR layout variant, currently with inline centered logo.
- `menu/menu.json`: menu content reference/snapshot. Useful for migration, syncing,
  or archival, but not current live render path.
- `menu/qrcode.js`: third-party library source. Avoid hand-editing unless fixing or
  upgrading library intentionally.
- `menu/lokocoffee.svg`: reusable brand asset for QR and other static pages.
- `.github/workflows/static.yml`: deploys this repo to GitHub Pages on push to `main`.
- `.github/workflows/supabase-keepalive.yml`: scheduled job (Mon/Fri) that reads one
  row from the downstream Supabase project to prevent free-tier auto-pause. See
  `tech.md` "Supabase keepalive job" for details and where its credentials live.

## Where new things go
- New gateway page: root or `menu/`, depending public URL path.
- New branded print asset: `menu/`.
- New shared static asset used by `menu/*`: `menu/`.
- New steering or process docs: `.ai/` or `.ai/steering/`.

Do not add downstream Svelte app source here unless product direction changes.

## Boundary with separate app repo
Keep responsibilities split:

- This repo: domain gateway, QR print assets, minimal static fallback UX.
- Separate Vercel repo: Svelte app source, Supabase integration, app routing, admin
  UI, deployment config, environment variables.

If you find yourself adding `src/`, `package.json`, Vite config, or Supabase client
code here, stop and confirm direction first.

## URL ownership
- `/` belongs to gateway entry.
- `/menu` belongs to gateway iframe host.
- `/menu/qr.html` and `/menu/qr2.html` belong to printable QR materials.

Preserve these paths unless owner explicitly approves breaking existing QR/domain
links.

## Change rules
- Prefer editing existing files over adding parallel gateway pages.
- Keep destination URLs easy to locate near bottom script blocks.
- Avoid duplicating same constant across many files without reason.
- Treat `qrcode.js` as vendored dependency, not normal app code.
