# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Static, no-build site for **مكتب القاسم** (Al-Qasem Law Office, Amman, Jordan) — a lawyer/legal-consultancy office. There is no package.json, build tool, or test framework; every page is a single self-contained HTML file with inline `<style>` and `<script>`. The live page is **bilingual (Arabic default RTL / English LTR)** and **dual-theme (dark default / light)** — both toggled client-side and persisted in `localStorage`.

## Files

- `index.html` — **the live site.** A complete single-page site built on the client-approved "ق · The Monogram" direction (near-black / saffron / sand, Cairo + Space Grotesk fonts). Sections (anchor-linked, smooth-scroll): nav → hero (`#home`) → stats bar (2014 / +10 / 7) → services (`#services`, 7 real practice areas in a horizontal scroll rail `.svcF-rail`) → why-us (`#why`, editorial two-column list) → about (`#about`, lawyer bio + vision/mission) → contact (`#contact`, consultation form + office info + working hours + embedded Google Map) → footer. Includes a mobile hamburger nav and a floating WhatsApp button (`wa.me/962799574933`). The real firm logo is `removebg-logo.png` (transparent gold scales-of-justice + laurel wreath) — used directly in the nav, as a large faint hero watermark (`.hero-logo`, higher opacity in light mode), and as the favicon. `logo.png` is a byte-identical spare copy and is not referenced.

  **Theming:** all colors are CSS custom properties scoped to `[data-theme="dark"]` / `[data-theme="light"]` on `<html>`. To stay theme-safe, use the existing variables (`--bg`, `--card`, `--gold`, `--sand`, `--cream`, `--text-soft`, `--nav-bg`, `--ghost`, `--input-bg`, etc.) — **do not hardcode colors** in new CSS, or light mode will break. The theme toggle button swaps a moon/sun icon and updates `<meta name="theme-color">`.

  **i18n:** every translatable node carries `data-i18n="key"` (or `data-i18n-ph` for placeholders); the `I18N` object in the bottom `<script>` holds parallel `ar`/`en` dictionaries. `applyLang()` sets `<html>` `lang`/`dir` and rewrites each node via `innerHTML` (values may contain markup like `<br>`/`&amp;`). **When adding any visible text, you must add a `data-i18n` key and supply both `ar` and `en` strings** — the key sets are kept identical. The bilingual section headers intentionally swap languages (big title in the active language, small label in the other).

  **Form delivery:** wired to [Web3Forms](https://web3forms.com) for email to the office inbox. Config constants at the top of the form IIFE: `WEB3FORMS_ACCESS_KEY` (empty by default — paste the free key for `alqasemlegal@gmail.com` to enable email) and `WHATSAPP_NUMBER`. **Fallback:** while the key is empty the form opens WhatsApp pre-filled with the submission, so it works with zero setup. Status messages are localized via the `form.*` i18n keys.

  **SEO / Analytics:** `<head>` carries full meta (description, keywords, canonical, Open Graph, Twitter, geo) plus a `LegalService` JSON-LD block (name, address, geo `31.9701366,35.8943148`, phone, email, founder, opening hours, 7 service offers) that backs the Google Business Profile / local SEO. The Web3Forms access key is live and the GA4 snippet is guarded: set `GA_MEASUREMENT_ID` (e.g. `G-XXXXXXXXXX`) to enable analytics — it no-ops while empty. Canonical/OG URLs assume the domain `alqasemlaw.com`; update them (and the OG/JSON-LD `removebg-logo.png` image URLs) once the real domain is set.

## Working in this repo

- `index.html` is the source of truth for the live site. Edit it directly for copy/content/style changes going forward.
- Deployment is automatic: the repo is hosted on GitHub Pages via `.github/workflows/static.yml`, so any push to `main` redeploys the live site.
- There is no build/lint/test tooling — verify changes by opening `index.html` directly in a browser (or via a local static server, e.g. `python3 -m http.server`).
- Layout uses CSS logical properties (`padding-inline`, `border-inline-start`, `inset-inline`) so it mirrors correctly between RTL (Arabic) and LTR (English) — prefer these over physical left/right when adding styles.
- The two checklists when editing the live page: (1) any new text needs a `data-i18n` key with both `ar`+`en`; (2) any new color must use a theme variable, never a literal.
