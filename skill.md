# Frontend Skill — RedBeryl Partner Communications Platform

## Overview

This is a **zero-build, mobile-first static web app** built with vanilla HTML, CSS, and JavaScript. There is no bundler, no framework, and no `node_modules`. Every page is a plain `.html` file that links directly to a shared stylesheet at `assets/style.css`.

---

## Project Structure

```
/
├── index.html                  # Landing page — tier selector (Red / Black)
├── assets/
│   ├── style.css               # Single shared design system for the entire site
│   ├── fonts/                  # Self-hosted OTF fonts (JuanaAlt-Medium, JuanaAlt-Light)
│   └── images/                 # Partner images (16:9 PNG/JPG)
├── red/
│   ├── index.html              # Red tier — month listing page
│   └── {month-year}/
│       └── index.html          # Red tier — monthly partner accordion page
├── black/
│   ├── index.html              # Black tier — month listing page
│   └── {month-year}/
│       └── index.html          # Black tier — monthly partner accordion page
└── galeries-lafayette/
    └── ...                     # One-off partner microsite
```

Image paths in partner pages always use `../../assets/images/` (two levels up from the month folder).  
CSS paths in partner pages use `../../assets/style.css`.  
CSS paths in tier-index pages (`red/index.html`, `black/index.html`) use `../assets/style.css`.

---

## Design System (`assets/style.css`)

All visual decisions live in a single file. Never use inline styles or ad-hoc values — use the CSS custom properties defined in `:root`.

### Brand Palette

| Variable | Value | Usage |
|---|---|---|
| `--rb-red` | `#981b23` | Primary accent — buttons, borders, active states |
| `--rb-red-light` | `#c0272f` | Hover tints |
| `--rb-red-dark` | `#6b1219` | Gradient darks |
| `--rb-red-glow` | `rgba(152,27,35,0.22)` | Shadows, selections |
| `--rb-red-subtle` | `rgba(152,27,35,0.07)` | Hover backgrounds, badge bg |
| `--rb-ivory` | `#FDF7F4` | Primary page background |
| `--rb-alabaster` | `#F0EBE3` | Secondary background, badges, footer |
| `--rb-stone` | `#8B7D72` | Muted body text, subtitles |
| `--rb-stone-light` | `#a89890` | Lighter muted text |
| `--rb-whatsapp` | `#25D366` | WhatsApp CTA button gradient start |
| `--rb-whatsapp-dark` | `#128C7E` | WhatsApp CTA button gradient end |

Black tier elements (`tier-card--black`, `tier-indicator--black`, `badge--tier`) use hardcoded dark gradients (`#2C2C2C → #1A1A1A`) rather than variables.

### Typography

| Variable | Font | Role |
|---|---|---|
| `--font-heading-brand` | JuanaAlt-Medium → Playfair Display | Logo / brand wordmark only |
| `--font-heading` | JuanaAlt-Light → Playfair Display | Partner names, month headings, content titles |
| `--font-display` | Playfair Display | Sub-headings, offer group headers |
| `--font-body` | Visby → Poppins → system-ui | Default body text |
| `--font-poppins` | Poppins → Visby | Partner descriptions, offer lists, footer |

JuanaAlt is self-hosted via `@font-face` in `style.css`. Poppins and Playfair Display are loaded via a Google Fonts `@import` at the top of `style.css`.

### Spacing (4px grid)

`--space-1` (4px) through `--space-10` (64px). Always use these variables; never write raw `px` values for layout spacing.

### Radius

| Variable | Value | Usage |
|---|---|---|
| `--radius-sm` | 8px | Badges, small interactive elements |
| `--radius-md` | 12px | Images, CTA buttons |
| `--radius-lg` | 16px | Partner cards, month cards |
| `--radius-xl` | 24px | Landing tier cards |

### Shadows

| Variable | Usage |
|---|---|
| `--shadow-card` | Default resting state of cards |
| `--shadow-card-hover` | Card on hover |
| `--shadow-elevated` | Open/expanded accordion card |

### Easing & Transitions

| Variable | Value |
|---|---|
| `--ease-out-expo` | `cubic-bezier(0.16, 1, 0.3, 1)` |
| `--ease-spring` | `cubic-bezier(0.34, 1.56, 0.64, 1)` |
| `--transition-fast` | `200ms var(--ease-out-expo)` |
| `--transition-med` | `350ms var(--ease-out-expo)` |
| `--transition-slow` | `500ms var(--ease-out-expo)` |

---

## Page Templates

### 1. Landing Page (`index.html`)

Tier selector with two `<a class="tier-card">` links — one red, one black. Styles are written inline in a `<style>` block inside this file (not in `style.css`) because they are unique to this page.

Key classes: `.landing-hero`, `.tier-cards`, `.tier-card--red`, `.tier-card--black`, `.tier-card-arrow`.

### 2. Tier Index Page (`red/index.html`, `black/index.html`)

Lists available months as `<a class="month-card">` links inside `<section class="month-list">`. Most recent month goes at the **top**.

Key classes: `.month-list`, `.month-card`, `.month-card-name`, `.month-card-count`, `.month-card-arrow`.

### 3. Monthly Partner Page (`red/{month}/index.html`, `black/{month}/index.html`)

The main content page. Contains:
- Sticky `<header class="site-header">` with back button, logo, and tier badge.
- `<section class="hero-section">` with month name and subtitle.
- `<p class="partner-count">` showing the number of partner experiences.
- `<section class="partners-list">` containing `<article class="partner-card">` accordion items.

---

## Partner Card (Accordion) Anatomy

Each partner is an `<article class="partner-card animate-in">` with a unique `id` (e.g. `id="partner-soka-bengaluru"`).

```html
<article class="partner-card animate-in" id="partner-{slug}">

  <!-- Accordion trigger -->
  <div class="partner-header"
       role="button" tabindex="0"
       aria-expanded="false"
       aria-controls="body-{slug}">
    <div class="partner-header-left">
      <h2 class="partner-name">Partner Name</h2>
      <div class="partner-meta">
        <span class="badge badge--location">
          <!-- location pin SVG -->
          City Name
        </span>
        <span class="badge badge--offers">N offers</span>
      </div>
    </div>
    <div class="partner-chevron">
      <!-- chevron-down SVG -->
    </div>
  </div>

  <!-- Accordion body (collapsed by default) -->
  <div class="partner-body" id="body-{slug}">
    <div class="partner-content">
      <div class="partner-image-wrap">
        <img src="../../assets/images/{image-name}.png"
             alt="Descriptive alt text"
             loading="lazy" decoding="async">
      </div>
      <p class="partner-description">...</p>
      <p class="offers-heading">Exclusive Privileges</p>
      <!-- Optional: <p class="offers-note"> for a fine-print note -->
      <!-- Optional: <p class="offers-subheading"> for grouped offer sections -->
      <ul class="offers-list">
        <li>Offer description.</li>
      </ul>
      <a class="book-btn"
         id="cta-{slug}"
         data-message="Hi, I'm Interested in the privileges for {Partner Name}."
         target="_blank" rel="noopener noreferrer">
        <!-- WhatsApp SVG -->
        Book via your Lifestyle Manager
      </a>
    </div>
  </div>

</article>
```

**Rules for IDs:** The `id` on the `<article>`, the `aria-controls` on `.partner-header`, and the `id` on `.partner-body` must all correspond: `partner-{slug}`, `body-{slug}`. The `id` on `.book-btn` must be `cta-{slug}`. Slugs must be unique per page.

---

## JavaScript Behaviours (inline `<script>` at bottom of partner pages)

All JS is vanilla, inline in a `<script>` tag at the bottom of the `<body>`. No external JS dependencies.

| Behaviour | How it works |
|---|---|
| **Accordion toggle** | Click on `.partner-header` toggles `.open` on the parent `.partner-card`. Only one card can be open at a time (others are closed first). After 450ms (mid-animation), the page smoothly scrolls to the open card. |
| **Scroll-reveal** | On load, `.animate-in` is swapped for `.reveal`. An `IntersectionObserver` adds `.visible` when a card enters the viewport, triggering a `fade-up` CSS transition. Each card gets a staggered `transitionDelay` of `i * 80ms`. |
| **Deep-link** | On `window.load`, if the URL has a `#hash`, the matching `.partner-card` is found, made visible, and its accordion header is clicked after 400ms. |
| **WhatsApp CTA** | Reads `?rm={phone}` from the URL. If present, constructs `https://wa.me/{phone}?text={encoded message}` from `data-message` on each `.book-btn`. If absent, hides all CTAs with `display: none`. |
| **Sticky header shadow** | An `IntersectionObserver` on a 1px sentinel `<div>` toggles `.scrolled` on `.site-header` when the user scrolls down, adding a border and box-shadow. |

---

## Animations

| Name | Definition | Used on |
|---|---|---|
| `fade-up` | `opacity: 0 → 1`, `translateY(20px → 0)` | `.animate-in` (staggered entry), landing page elements |
| `pulse-dot` | Pulsing opacity/scale | `.tier-indicator .dot` (live status dot) |
| `shimmer` | Horizontal sweep gradient | `.partner-image-wrap::before` (image loading skeleton) |
| `reveal` / `.visible` | JS-driven `fade-up` via class toggle | Partner cards (scroll-triggered) |

`@media (prefers-reduced-motion: reduce)` disables all animations and transitions site-wide.

---

## Responsive Breakpoints

| Breakpoint | Change |
|---|---|
| `min-width: 390px` | Slightly larger `partner-name` and `hero-month` font sizes |
| `min-width: 768px` | `--max-width` increases to 600px; larger hero text; more padding on partner header and content |
| `(hover: none) and (pointer: coarse)` | All hover effects (shadows, transforms, pseudo-element reveals) are suppressed on touch devices |

The layout is constrained to `--max-width: 540px` (desktop) via `max-width` on `.page-container` and `.header-inner`, keeping it phone-width on all screens.

---

## Key Conventions & Gotchas

- **No build step.** Editing `assets/style.css` or any `.html` file directly changes the live site (via GitHub Pages).
- **Image ratio.** All partner images must be saved at **16:9** and placed in `assets/images/`. The `<div class="partner-image-wrap">` enforces `aspect-ratio: 16 / 9` via CSS.
- **Font fallback chain.** Visby is not loaded via Google Fonts — it falls back to Poppins if not present locally on the device. This is intentional.
- **`data-message` attribute.** The WhatsApp pre-fill text lives here. It must be plain text (not URL-encoded) — the JS encodes it at runtime.
- **RM parameter.** The `?rm=` query param holds the Relationship Manager's WhatsApp number (digits only, country code included, no `+`). Without it, all CTAs are hidden. This means the page is designed to be shared as a deep-link with the RM number embedded.
- **Deep-link hash.** The URL fragment (`#partner-soka-bengaluru`) must exactly match the `id` of the `<article>`. The `RM` param and hash can be combined: `?rm=919999999999#partner-soka-bengaluru`.
- **Black tier gold.** The Black tier uses `--rb-gold` / `--rb-gold-light` in the README examples but the live `style.css` uses hardcoded dark surfaces for black-tier components. There is currently no `--rb-gold` variable in `:root`.
- **`Section.html` / `section.txt` / `spec_text.txt`** — these are scratch/reference files used during content drafting, not part of the live site.
