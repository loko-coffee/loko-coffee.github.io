# Product

## What this is
Public entrypoint repo for **L'oko Coffee & Roastery** digital menu.

Customers hit stable L'oko-owned URL or scan printed QR. This repo owns that first
touchpoint, then forwards users to menu/content app built with **Svelte** and
backed by **Supabase**, deployed on **Vercel** from separate repository. This repo
also holds branded printable QR pages and local menu snapshot data.

## Who uses it

| User | Context | Needs |
|------|---------|-------|
| **Customer** | Scans QR at table or opens `loko-coffee.github.io`. | Land on working menu fast, no confusion, no dead links. |
| **Owner / staff** | Reprinting QR cards or changing where menu lives. | Keep same public domain/QR surface even if actual content host changes. |
| **Maintainer** | Updates redirect targets, QR destinations, branded shell pages. | One obvious place to change URLs without rebuilding bigger app here. |

## Why it exists
Menu application lives in separate deployment unit from public domain. QR stickers
and public domain should not need replacement when downstream app is redeployed,
migrated, or iterated in its own repo.

This repo keeps **front-door stability**:
- stable public domain,
- stable repo-owned path structure,
- branded QR print assets,
- minimal client-side redirect logic.

## Core features
- **Stable domain gateway**: root domain sends visitors into repo-owned menu path.
- **External handoff**: `/menu` forwards customer to live externally hosted menu.
- **Printable QR cards**: branded pages generate scannable QR codes for target URL.
- **Local menu snapshot**: `menu/menu.json` preserves structured menu content in repo
  even though current public flow is redirect-first.

## Explicit non-goals
- No admin/editor UI in this repo.
- No authentication, private dashboard, or role system here.
- No checkout, ordering, payments, reservations, or inventory.
- No Svelte app implementation in this repo. Downstream app lives elsewhere.
- No complex app runtime inside this repo unless product direction changes.

## Product principles
1. **Stable first touchpoint.** User-facing domain should stay same even if content
   host changes.
2. **Tiny and reliable.** Repo-owned pages should be simple static files with minimal
   break surface.
3. **Easy URL swaps.** Changing downstream destination should mean editing obvious
   constants, not rebuilding architecture.
4. **Decouple domain from app.** Public entrypoint can stay fixed while Svelte +
   Supabase app evolves in separate repo.
5. **Brand still matters.** Even QR print pages and fallback screens should feel like
   L'oko, not generic utilities.

## Success looks like
- Customer scans QR and reaches live menu with one handoff.
- Maintainer can repoint downstream content by editing one URL constant.
- Domain remains stable while external content host can change independently.
