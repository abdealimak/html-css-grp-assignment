# 👾 Space Invaders — Multi-Theme Landing Page

A production-grade, single-page frontend tribute to **Space Invaders** (originally created by Tomohiro Nishikado, Taito Corporation, 1978). Built as a university-level frontend assignment demonstrating advanced HTML, CSS, and vanilla JavaScript architecture across three completely distinct visual themes.

---

## 📁 Project Structure

```
space-invaders-landing/
├── index.html      # Semantic HTML structure + all SVG character variants
├── style.css       # Full theme system via CSS custom properties (1,780 lines)
├── script.js       # ES6 class-based JS — animation engine + all interactivity (1,015 lines)
└── README.md       # This file
```

> **No frameworks. No build tools. No external dependencies beyond Google Fonts.**  
> Open `index.html` directly in any modern browser — it works offline.

---

## 🎨 Three Visual Themes

Switch themes instantly using the buttons in the navigation bar or footer. The page never reloads — all transitions are animated in-place.

| Theme | Trigger | Aesthetic |
|---|---|---|
| 🎮 **Retro Arcade** | Default | Green-on-black pixel art, CRT scanlines, flicker animation |
| 🧊 **3D Futuristic** | `FUTURE` button | Glassmorphism, neon cyan, metallic gradients, node-graph background |
| 🌸 **Anime Cyberpunk** | `ANIME` button | Pink/purple gradients, kawaii rounded characters, floating particles |

### Theme Architecture

Themes are driven by a single `data-theme` attribute on the `<html>` element:

```html
<html data-theme="retro">      <!-- or "futuristic" or "anime" -->
```

Every component consumes only CSS custom properties — zero hardcoded colours anywhere in the stylesheet:

```css
:root { --clr-primary: #00ff00; }

[data-theme="futuristic"] { --clr-primary: #00e5ff; }
[data-theme="anime"]      { --clr-primary: #ff2dca; }
```

---

## 🖼️ SVG Character System

All characters — aliens, ships, and the UFO — are **inline SVG**. No raster images are used anywhere in the project.

### Per-Theme Character Variants

Each character slot contains **three SVG variants** inside a `.char-slot` wrapper. CSS shows only the active theme's variant using `display` toggling:

```html
<div class="char-slot alien alien-squid">
  <svg class="v-retro">    <!-- Pixel art squares (rect elements) --></svg>
  <svg class="v-futuristic"><!-- 3D mech with gradients + geometry --></svg>
  <svg class="v-anime">    <!-- Kawaii blob with circles + paths   --></svg>
</div>
```

```css
.v-retro, .v-futuristic, .v-anime { display: none; }   /* all hidden */
[data-theme="retro"]      .v-retro       { display: block; }
[data-theme="futuristic"] .v-futuristic  { display: block; }
[data-theme="anime"]      .v-anime       { display: block; }
```

### Character Designs by Theme

#### 🎮 Retro — Classic Pixel Art
Built entirely from `<rect>` elements on a pixel grid, faithfully recreating the original 1978 sprites:

| Character | Technique |
|---|---|
| Squid (30 pts) | 8×6 pixel grid with `<rect>` tiles |
| Crab (20 pts) | 8×6 pixel grid with `<rect>` tiles |
| Octopus (10 pts) | 8×6 pixel grid with `<rect>` tiles |
| Mystery UFO | 12×5 pixel grid with `<rect>` tiles |
| Player Cannon | 12×8 pixel grid with `<rect>` tiles |

#### 🧊 3D Futuristic — Mechanical Characters
Uses `<polygon>`, `<ellipse>`, `<line>`, `<circle>`, and SVG `linearGradient` / `radialGradient` fills to create a physically plausible mechanical aesthetic:

| Character | Design |
|---|---|
| Squid | Armoured mech spider-walker — trapezoidal hull, dual visor eyes, 4 articulated legs with joint nodes, shoulder pods, antenna |
| Crab | Tank chassis — wide panoramic scanner visor, mechanical pincer arms with claw splits, armour rivets, 4 bottom legs |
| Octopus | Surveillance drone — circular disc body, 8 radiating arm struts, large central optical sensor with dashed orbit ring |
| Mystery UFO | Chrome flying saucer — metallic dome, 6 rim light panels, gradient body, top antenna |
| Player Ship | Delta-wing fighter — swept wings, glowing engine nacelles (`radialGradient`), tinted cockpit dome, fuselage panel lines |

#### 🌸 Anime — Kawaii Cartoon Characters
Built with `<ellipse>`, `<circle>`, `<path>` curves, and `<text>` elements for emoji/symbol decorations. All characters feature oversized eyes with highlights, blush marks, and expressive mouths:

| Character | Design |
|---|---|
| Squid | Pink blob body, curly antenna with ball tips, huge sparkling eyes, blush marks, 5 rounded tentacle bumps |
| Crab | Chubby orange chibi with round harmless claws, bulging eyestalks, happy smile, stubby round legs |
| Octopus | Round purple head with bow accessory, giant `wh`ite-iris eyes, wide smile, 7 wavy thick tentacle strokes |
| Mystery UFO | Pastel rounded saucer with coloured rim lights, cute alien face in dome, ♥ antennae |
| Player Ship | Oval pink/purple body, large "face" porthole (pupil + highlight), rounded wings, ★ decorations, sparkle exhaust |

### SVG Gradient Definitions

All shared gradients are declared once in a hidden `<svg class="svg-global-defs">` block at the top of `<body>`, guaranteeing they are always available in the DOM regardless of which character variants are currently hidden:

```html
<svg class="svg-global-defs" aria-hidden="true">
  <defs>
    <linearGradient id="mech-body">   <!-- 3D dark steel blue body  --></linearGradient>
    <linearGradient id="mech-eye">    <!-- 3D cyan visor glow       --></linearGradient>
    <linearGradient id="ship-3d-body"><!-- 3D ship fuselage         --></linearGradient>
    <radialGradient id="ship-3d-engine"><!-- 3D engine exhaust glow --></radialGradient>
    <linearGradient id="ufo-3d-rim">  <!-- 3D saucer rim            --></linearGradient>
    <linearGradient id="anime-ship-body"><!-- Anime pink→purple ship --></linearGradient>
    <linearGradient id="anime-ufo-body"> <!-- Anime UFO body        --></linearGradient>
  </defs>
</svg>
```

---

## 🎞️ CSS Effects & Techniques

### Retro Theme
| Effect | Implementation |
|---|---|
| CRT Scanlines | `repeating-linear-gradient` overlay fixed to viewport |
| Screen flicker | `@keyframes crt-flicker` with opacity steps, 4s interval |
| Pixel font | `Press Start 2P` (Google Fonts) |
| Terminal glow | `text-shadow` multi-layer green glow on all headings |
| Blinking cursor | `@keyframes blink-text` on hero kicker text |
| Glitch effect | JS character-scramble on hero title (every ~3.5s, retro only) |

### 3D Futuristic Theme
| Effect | Implementation |
|---|---|
| Glassmorphism | `backdrop-filter: blur(16px)` + semi-transparent `rgba` backgrounds |
| 3D card tilt | `perspective(800px) rotateY() rotateX()` on `.glass-card:hover` |
| Neon glow | Multi-layer `box-shadow` and `drop-shadow` filter on SVGs |
| Node graph BG | Canvas: 60 moving orbs connected by lines when within 160px |
| Visor scan | `@keyframes visor-scan` animates brightness on gradient-filled rects |
| Ship 3D hover | `@keyframes ship-3d-hover` applies `rotateY` as ship moves |
| Futuristic font | `Orbitron` (Google Fonts) |

### Anime Theme
| Effect | Implementation |
|---|---|
| Gradient text | `background-clip: text` + `linear-gradient` on heading elements |
| Animated gradient border | `@keyframes gradient-border-spin` on `::after` pseudo-element using `mask-composite: exclude` to create border-only gradient |
| Floating particles | Canvas: 80 rising particles with sinusoidal wobble |
| Bouncy float | `cubic-bezier(0.36, 0.07, 0.19, 0.97)` timing on float animations |
| Character sway | `@keyframes alien-anime-sway` adds rotation to float movement |
| Soft font | `Rajdhani` (Google Fonts) |
| Warm overlay | `radial-gradient` body pseudo-elements in pink and purple |

---

## ⚙️ JavaScript Architecture

The JS is split into **11 ES6 classes**, each with a single responsibility:

```
DOMContentLoaded
├── ThemeManager      — data-theme switching + flash-veil transition sequence
├── CanvasEngine      — requestAnimationFrame background animation loop
│   ├── StarfieldRenderer   (retro)   — falling pixel stars + shooting stars
│   ├── OrbRenderer         (future)  — node-graph with proximity connections
│   └── ParticleRenderer    (anime)   — rising bubbly particles with wobble
├── UIBuilder         — JS-generated mini alien grid + simulated game score
├── ScrollReveal      — IntersectionObserver for staggered section reveals
├── StatCounter       — easeOutExpo counter animation on stat numbers
├── MobileMenu        — accessible hamburger nav with outside-click close
├── NavScroll         — scroll-aware nav shadow
├── HeroParallax      — mouse-follow lerp parallax on hero aliens + ship
├── ButtonRipple      — material-style click ripple on primary buttons
├── KonamiCode        — ↑↑↓↓←→←→BA easter egg
└── GlitchText        — retro-only periodic character scramble on title
```

### Canvas Engine — Theme-Adaptive Rendering

The single `<canvas id="bg-canvas">` element renders different effects per theme with no restart — just a state switch:

```js
setTheme(theme) {
  this.theme = theme;
  this._buildPools(); // Rebuild particle pools for new theme
}

_loop(now) {
  if (this.theme === 'retro')       this._drawStars();
  else if (this.theme === 'futuristic') this._drawOrbs();
  else if (this.theme === 'anime')  this._drawParticles();
  requestAnimationFrame(ts => this._loop(ts));
}
```

### Theme Transition Sequence

Switching themes triggers a choreographed 4-step sequence to avoid jarring visual cuts:

```
1. body.classList.add('theme-transitioning')   — all CSS props now interpolate
2. theme-veil flashes (opacity: 0.6)            — full-screen colour burst
3. html.dataset.theme = newTheme               — DOM update during flash
4. Veil fades out over 300ms                   — reveal new aesthetic
5. 'theme-transitioning' removed at 1100ms    — stop forcing transitions
```

### Scroll Reveal

All cards and section titles use a single `IntersectionObserver` instance with `threshold: 0.12`. Elements receive `.reveal` (invisible + translated down) on mount, then `.visible` on entry:

```css
.reveal          { opacity: 0; transform: translateY(30px); transition: ... }
.reveal.visible  { opacity: 1; transform: translateY(0); }
.reveal:nth-child(1) { transition-delay: 0.0s; }
.reveal:nth-child(2) { transition-delay: 0.1s; }
/* ... staggered per child */
```

---

## 🏗️ HTML Semantic Structure

```html
<body>
  <svg class="svg-global-defs">  <!-- Shared SVG gradient defs -->
  <canvas id="bg-canvas">        <!-- Theme-adaptive background -->
  <div class="scanlines">        <!-- CRT overlay (retro only) -->
  <div class="theme-veil">       <!-- Transition flash layer -->

  <nav class="site-nav">         <!-- Fixed navigation + theme switcher -->

  <header class="hero">          <!-- Hero: aliens + text + ship -->
    <div class="hero-aliens">    <!-- 3× .char-slot (squid/crab/octopus) -->
    <div class="hero-content">   <!-- Title, subtitle, score, CTAs -->
    <div class="hero-ship-wrap"> <!-- 1× .char-slot (player ship) -->

  <section id="story">           <!-- Origin story — 3-column card grid -->
  <section id="gameplay">        <!-- Mechanics — animated screen + list -->
  <section id="aliens">          <!-- Alien showcase — 4 cards -->
  <section id="legacy">          <!-- Stats counters + quote -->
  <section class="section-cta">  <!-- Final CTA + scrolling ticker -->

  <footer class="site-footer">   <!-- Brand + nav links + theme switcher -->
```

---

## 📐 Responsive Design

Breakpoints are handled with `clamp()` for fluid type scaling and CSS Grid with `auto-fit` / `minmax` for layout:

```css
/* Fluid type — no breakpoints needed for sizes */
--font-size-hero: clamp(2rem, 8vw, 5.5rem);
--font-size-h2:   clamp(1.2rem, 4vw, 2.5rem);

/* Auto-reflow grid — wraps naturally at any width */
grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
```

At `≤768px`: hero switches to single-column, nav links collapse to hamburger menu, footer becomes single-column.

---

## ♿ Accessibility

- All SVG graphics have `aria-hidden="true"` — decorative only
- Theme buttons use `aria-pressed` for screen reader state
- Nav buttons use `aria-expanded` and `aria-controls`
- Section headings have explicit `aria-labelledby` on their `<section>` elements
- Fully keyboard navigable with visible `:focus-visible` outlines
- `@media (prefers-reduced-motion: reduce)` disables all animations globally

---

## 🎮 Easter Egg

Enter the **Konami Code** on your keyboard:

```
↑  ↑  ↓  ↓  ←  →  ←  →  B  A
```

All aliens on the page flash through rainbow colours and a retro pop-up message appears.

---

## 🛠️ Technologies Used

| Technology | Usage |
|---|---|
| **HTML5** | Semantic structure, inline SVG, ARIA attributes |
| **CSS3** | Custom properties, Grid, Flexbox, `clip-path`, `backdrop-filter`, `mask-composite`, `background-clip: text`, `@keyframes`, `clamp()` |
| **Vanilla JavaScript (ES2020)** | ES6 classes, `requestAnimationFrame`, `IntersectionObserver`, `localStorage`, `performance.now()` |
| **SVG** | Inline character graphics — `rect`, `polygon`, `ellipse`, `circle`, `path`, `line`, `linearGradient`, `radialGradient` |
| **Google Fonts** | `Press Start 2P` (retro), `Orbitron` (futuristic), `Rajdhani` (anime) |
| **Canvas 2D API** | Background particle/starfield/orb animations |

---

## 🚀 Getting Started

No installation, no build step, no dependencies to install.

```bash
# Clone or download the three files, then:
open index.html
# or serve locally:
python3 -m http.server 8080
```

**Browser support:** All modern browsers (Chrome 90+, Firefox 88+, Safari 14+, Edge 90+).  
`backdrop-filter` requires a Chromium or WebKit engine — Firefox users will see fallback opaque backgrounds.

---

## 👤 Credits

- **Original Game:** Space Invaders® — Tomohiro Nishikado / Taito Corporation, 1978
- Space Invaders® is a registered trademark of Taito Corporation. This project is a non-commercial educational tribute.

<h4>🧑🏻‍💻Developer:<br> Abdeali Makda</h4>
