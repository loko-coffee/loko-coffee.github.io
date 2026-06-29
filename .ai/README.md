# .ai — Steering Docs

This folder holds steering docs for L'oko Coffee public menu gateway repo: shared,
durable context that explains what repo does today, how it is structured, how to
change it safely. Read before edits; update when direction changes.

> Steering docs describe intent and conventions, not every line of implementation.
> If code is source of truth, point at code instead of duplicating.

## Documents

| File | What it answers | Read it when... |
|------|-----------------|-----------------|
| [product.md](./steering/product.md) | What repo is for, who uses it, what is in/out of scope. | Deciding whether work belongs here. |
| [tech.md](./steering/tech.md) | Hosting model, redirect architecture, client-side runtime, URL handoff points. | Changing deploy flow, redirects, QR targets, or external host linkage. |
| [structure.md](./steering/structure.md) | Actual repo layout and purpose of each file. | Adding, moving, or deleting files. |
| [design.md](./steering/design.md) | Visual language for repo-owned pages: QR cards and redirect fallback. | Editing UI, branding, typography, or spacing. |
| [convention.md](./steering/convention.md) | Plain HTML/CSS/JS conventions, naming, copy, Git rules. | Writing any code or docs. |

## Reading order
`product.md` -> `tech.md` -> `structure.md` -> `design.md` -> `convention.md`

## Status
Repo is lightweight static gateway on `loko-coffee.github.io`.

- `index.html` redirects root traffic to `/menu`.
- `menu/index.html` is domain-owned iframe shell that embeds live menu app hosted on
  Vercel from separate repository.
- `menu/qr.html` and `menu/qr2.html` generate printable branded QR cards with
  configurable destination URLs.
- `menu/menu.json` is local structured menu data kept in repo, but current public
  experience is iframe-hosted, not repo-rendered.

Current product direction: keep this repo as stable user-accessed domain and QR
surface; actual menu/content app is built with Svelte + Supabase in separate repo
and deployed on Vercel.

## Maintaining docs
- Update relevant doc in same PR as decision change.
- Prefer editing existing doc over adding new doc.
- If docs disagree: `tech.md` wins for architecture, `design.md` for visuals,
  `convention.md` for coding/style rules.
