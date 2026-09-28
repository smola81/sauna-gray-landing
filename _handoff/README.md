# Handoff: Sauna Gray landing page refresh

Target: `smola81/sauna-gray-landing` (Jekyll on GitHub Pages, served at https://smola81.github.io/sauna-gray-landing/).

## How to use this with Claude Code
```
cd sauna-gray-landing
unzip ~/Downloads/design_handoff_landing_page.zip -d _handoff
claude
> Read _handoff/README.md and implement it. Work on a branch called redesign. Run `bundle exec jekyll serve` and check the result at 375px and 1280px before committing.
```

## Goal
Replace the default-theme page (a long feature list and ~70 device names in bullets) with a short, dark, product-led page that matches the watch app redesign. The page should have one clear call to action: **Get it on Connect IQ**.

## Constraints
- Keep Jekyll and GitHub Pages. Use no build tools and no JS frameworks. Put everything in `index.html`, `_layouts/default.html`, `assets/css/site.css` and `_data/devices.yml`.
- Drop the remote/default theme from `_config.yml` (use `theme: null` or remove `remote_theme`) so the page ships its own CSS.
- Keep the page under 400 KB total. The only fonts are Barlow 500/600/700 and Barlow Condensed 600 (Google Fonts, `display=swap`, preconnect).
- Meet WCAG AA contrast. All images need `alt`, `width` and `height`, and every image except the hero uses `loading="lazy"`.

## Design tokens (`:root` in `site.css`)
```
--bg:#0B0A09; --surface:#161412; --line:#2A2724;
--text:#F2EEE8; --text-2:#C9C2B7; --muted:#8F887D;
--heat:#FF8A3D; --cool:#46C2F0; --danger:#FF4D4D; --gold:#F2C14E; --go:#5BD68A;
--radius:14px; --max:1120px;
```
- Type: H1 Barlow Condensed 600, `clamp(56px,9vw,112px)`, line-height .9. H2 Barlow Condensed 600, `clamp(34px,5vw,52px)`. Body Barlow 18px/1.55 in `--text-2`. Eyebrow Barlow 700 13px, letter-spacing .22em, uppercase, in `--heat`.
- Links: `--heat`, hover `#FFB07A`. Buttons: filled `--heat` with `#0B0A09` text, 52px tall, radius 999px, 600 weight.
- Section padding: `clamp(64px,10vw,128px)` vertical. Content max-width `--max`, gutters 24px.

## Page structure (in order)
1. **Header** (sticky, 64px, `--bg` at 85% with `backdrop-filter: blur(12px)`, bottom border `--line`): wordmark "Sauna Gray" in Barlow Condensed 24px on the left, "Get the app" button on the right.
2. **Hero**: a two-column grid (stacks below 860px). Left: eyebrow "For Garmin watches", H1 "Sauna Gray", sub "Rounds, heat and heart rate on your wrist — with safety alerts built in.", primary button "Get it on Connect IQ" (https://apps.garmin.com/apps/99fa9fae-74ea-4f3e-b27f-c5571c99a9b8), and a secondary text link "See compatible watches ↓". Right: `assets/images/1-screen.png` shown as a 420px circle with a 14px `#1C1B1A` bezel ring and a large soft shadow. Background: a subtle radial `#2A1A0F → --bg` behind the watch only.
3. **How a session works**: a three-step row, where each card is `--surface` with a `--line` border and radius 14px:
   - Sauna (`1-screen`): heat dot. Copy: "Live timer, temperature and heart rate with zones. The ring fills to 15 min."
   - Cool (`2-screen`): cool dot. Copy: "Cooling break timer and temperature drop. Buzzes when you're ready for the next round."
   - Save (`4-screen`): go dot. Copy: "Summary of rounds, max temp, average HR and calories. Saves to Garmin Connect and Strava."
   Each card holds a 240px round screenshot.
4. **Safety first**: a split layout with `3-screen.png` and a short list. Alerts at 15, 18 and 20 min; temperature over 80/85 °C; HR over 140/150 bpm; vibration and a full-screen alert.
5. **Features**: a 2×3 grid (1 column on mobile) with title and one line each: Multi-round tracking · History of 50 sessions · Personal records (show `5-screen.png` as a small circle) · GPS location · Temperature editing after a session · Adaptive layouts for every screen. No icons; use a coloured 8px dot only.
6. **Compatible watches**: move the device list into `_data/devices.yml` (grouped by family) and render the families as `<details>` accordions, closed by default. Add a search input that filters by name with ~15 lines of vanilla JS. Show the count: "Works on 70+ Garmin watches".
7. **Final CTA**: centred H2 "Start your next session", the primary button, and `cover-image.png` at 120px above it.
8. **Footer**: a muted small line with the app name, © year, a Connect IQ link, and a privacy link if one exists.

## SEO / meta
- `<title>Sauna Gray — Sauna tracker for Garmin watches</title>`, with a meta description of 150 characters or fewer.
- Open Graph image: `assets/images/hero-image.png` (1440×720). Set `twitter:card=summary_large_image`.
- Theme colour `#0B0A09`, and use `cover-image.png` for the favicon/apple-touch-icon.
- Add JSON-LD `SoftwareApplication` with `operatingSystem: "Garmin Connect IQ"` and `applicationCategory: "HealthApplication"`.

## Interaction
- `prefers-reduced-motion` is respected. The only motion is screenshots fading and rising 12px on scroll via IntersectionObserver (300ms ease-out).
- Visible focus rings: 2px `--heat`, offset 3px.

## Assets (in `assets/images/`)
`hero-image.png`, `cover-image.png`, `1-screen.png` … `5-screen.png`. They are the same files as the store asset pack. Replace the existing images of the same name.

## Acceptance checklist
- [ ] Lighthouse mobile scores are ≥95 for Performance, Accessibility, Best Practices and SEO.
- [ ] There is no horizontal scroll at 360px.
- [ ] The hero CTA is visible above the fold on a 375×667 screen.
- [ ] The device list is collapsed by default and the search works.
