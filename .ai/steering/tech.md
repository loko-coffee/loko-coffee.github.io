# Tech

## Stack at glance

| Layer | Choice | Notes |
|-------|--------|-------|
| Public domain repo | **Static HTML/CSS/JS on GitHub Pages** | Owns `https://loko-coffee.github.io`. |
| Gateway layer | **Root redirect + iframe shell** | `index.html` -> `/menu`, then `/menu` embeds external app URL. |
| QR generation | **Vanilla JS + bundled `qrcode.js`** | Generates printable SVG QR codes in-browser. |
| External menu app | **Svelte app in separate repo** | Real user-facing menu UI lives there. |
| External backend | **Supabase** | External Svelte app reads/writes here, not this repo. |
| External hosting | **Vercel** | Separate repo deployment target for live app. |

## Architecture
This repo is **not** primary menu app codebase. It is stable domain gateway in front
of another deployment, and `/menu` hosts that app inside iframe.

```
Customer / QR scan
        |
        v
https://loko-coffee.github.io
        |
        +--> /index.html       -> redirect to /menu
        |
        +--> /menu/index.html  -> full-height, phone-width iframe
        |                         embedding Vercel app URL
        |
        +--> /menu/qr*.html    -> printable QR card pages
                                   with configurable target URL

Separate repo on Vercel
        |
        v
Svelte menu app <-> Supabase
```

Boundary:
- This repo owns domain, redirect flow, QR print assets, minimal fallback UI.
- Separate repo owns actual menu application, Supabase client logic, data fetching,
  auth, admin editing, and app deployment.

## Gateway model
Current flow:

1. `https://loko-coffee.github.io` loads root `index.html`.
2. Root page sends browser to `https://loko-coffee.github.io/menu`.
3. `menu/index.html` renders centered shell with real `100dvh` height.
4. Inside that shell, iframe loads Vercel app URL.

Rules:
- Keep downstream app URL in one obvious constant when possible.
- Give iframe real layout height on GitHub Pages side. Downstream app uses `100dvh`,
  which measures iframe viewport, not device screen.
- Center iframe in page with max width about `430px` so embedded app behaves like
  phone-width shell.
- Keep noscript/fallback link for direct open when iframe cannot render.

## External app contract
Downstream app assumptions:
- Built with Svelte.
- Uses Supabase for menu data and any admin/auth flows.
- Hosted on Vercel in separate repository.
- Must be safe to deep-link from this repo's public domain and QR materials.

This repo should not duplicate downstream app code, env vars, or Supabase secrets.
If downstream stack changes, update docs and redirect target here.

## Data in this repo
`menu/menu.json` exists as structured menu snapshot/reference data. Today it is not
served as primary customer experience because `/menu` embeds Vercel app instead.

Document shape:
```jsonc
{
  "menus": [
    {
      "id": "minuman",
      "title": "Minuman",
      "caption": "DRINKS",
      "sections": [
        {
          "id": "signature",
          "title": "Signature",
          "items": [
            { "name": "Kopi Susu L'oko", "price": "19K" },
            { "name": "Espresso", "price": "15K | 15K", "variants": "Hot | Ice" },
            { "name": "+ Japanese", "price": "2K", "isAddOn": true }
          ]
        }
      ]
    }
  ]
}
```

Conventions in data:
- `price` and `variants` use `" | "` for paired options.
- `isAddOn: true` marks muted subordinate item.
- `section.id` is stable slug; preserve when syncing with downstream app.

## Configuration points
This repo currently uses hard-coded URL constants inside HTML, not env vars.

Known constants:
- Root redirect in `index.html`.
- Downstream app target in `menu/index.html` as iframe `src`.
- QR destination in `menu/qr.html` and `menu/qr2.html` as `MENU_URL`.

If redirect target and QR target should stay aligned, update both together or
extract shared generation/build step before adding more pages.

## Performance + reliability
- Gateway pages should stay tiny and dependency-light.
- QR pages may use Google Fonts and embedded branding because they are printed/shared,
  but runtime logic should remain minimal.
- External app cold starts or Supabase latency belong to separate repo/runtime.
- Iframe shell must provide correct height and centered width so downstream `100dvh`
  layout behaves predictably.
- Embedded page should still fail soft: keep human-readable fallback link.

## Security model
This repo holds no Supabase credentials and no private server logic.

- No `service_role` key here.
- No Supabase anon key needed here unless gateway responsibilities expand.
- Secrets for downstream Svelte + Supabase app belong in separate Vercel repo/env.

## Change rules
- If public domain path changes, treat as breaking product change.
- If downstream Vercel URL changes, update iframe `src` and any QR destinations.
- If downstream app architecture changes away from Svelte + Supabase, update this
  doc and keep gateway role unchanged unless product direction changes.
