# A Very Bad Idea — Design System & Style Bible

A complete reference for building UIs that match the ABI aesthetic. Hand this to Claude at the start of any front-end project to get consistent results across all personal tools and sites.

---

## Philosophy

Dark. Industrial. Monospaced. Every surface looks like it was built, not designed — precise, functional, with just enough craft to show it was cared about. No gradients for decoration's sake, no rounded corners trying to be friendly. The accent color is the only warmth in the room, and it earns its place.

**Rules of thumb:**
- Restraint over decoration. If you're about to add something, ask if it earns its keep.
- Monospace everywhere. No fallback to a sans-serif for "readability" — the mono IS the aesthetic.
- Animations should feel like a physical response, not a performance.
- Labels and metadata in uppercase with generous letter-spacing. It signals system, not marketing.
- Dark surfaces should feel deep, not flat. Layer them.

---

## Color Tokens

```css
:root {
  /* Backgrounds — layered dark surfaces */
  --bg:          #08080a;   /* page background, deepest layer */
  --surface:     #0f0f12;   /* cards, panels, alt sections */
  --surface-2:   #161619;   /* card bodies, nested surfaces */

  /* Borders */
  --border:      #222228;   /* primary border */
  --border-soft: #1a1a1f;   /* subtle dividers */

  /* Text */
  --text:        #b8b8c4;   /* body copy — main readable text */
  --text-dim:    #7c7c8a;   /* secondary, labels, metadata (min WCAG AA: 5.1:1 on --bg) */
  --text-bright: #e4e4f0;   /* headings, emphasis, interactive states */

  /* Accent — electric mint green */
  --accent:      #3dffaa;               /* primary accent — use sparingly */
  --accent-dim:  rgba(61,255,170,0.10); /* tinted backgrounds, tag fills */
  --accent-glow: 0 0 24px rgba(61,255,170,0.25); /* box-shadow glow effect */
}
```

**Usage rules:**
- `--bg` is the base. Every surface lifts slightly from it.
- Never use `--accent` for body text. It's a signal color — pips, borders on hover, keywords, active states.
- `--text-dim` is the floor for readable text (passes WCAG AA). Don't go darker for text that needs to be read.
- `--text-bright` on headings and anything that needs to snap forward.

**Semantic usage patterns:**
```
Page background      → --bg
Section (alt)        → --surface
Card / Panel         → --surface-2
Body copy            → --text
Labels / Meta        → --text-dim
Headings / Active    → --text-bright
Accent / Active pip  → --accent
Tag fill             → --accent-dim background + --accent border + --accent text
```

---

## Typography

**Font:** JetBrains Mono — monospaced throughout, no exceptions.

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;1,400&display=swap" rel="stylesheet">
```

```css
--font: 'JetBrains Mono', ui-monospace, 'Cascadia Code', monospace;
```

### Type Scale

| Role | Size | Weight | Letter-spacing | Case | Color |
|------|------|--------|----------------|------|-------|
| Hero / Display | `clamp(3.2rem, 7.5vw, 6.5rem)` | 700 | `-0.03em` | Mixed | `--text-bright` |
| Section title | `clamp(1.6rem, 2.8vw, 2.4rem)` | 600 | `-0.015em` | Mixed | `--text-bright` |
| Card title | `1rem` | 600 | `0.02em` | Mixed | `--text-bright` |
| Body / Description | `13px` | 300 | `0.04em` | Mixed | `--text` or `--text-dim` |
| Eyebrow / Label | `11px` | 400 | `0.3em` | UPPERCASE | `--accent` |
| Tag | `10px` | 400 | `0.15em` | UPPERCASE | `--accent` |
| Micro / Nav label | `10px` | 400 | `0.25em` | UPPERCASE | `--text-dim` |
| Tiny / Footer | `10px` | 400 | `0.15em` | UPPERCASE | `--text-dim` |

### Eyebrow Pattern
Used above section titles and in the hero. Brackets signal system/code:

```html
<span class="eyebrow">3D Work</span>
```
```css
.eyebrow {
  font-size: 11px;
  letter-spacing: 0.3em;
  text-transform: uppercase;
  color: var(--accent);
}
.eyebrow::before { content: '[ '; }
.eyebrow::after  { content: ' ]'; }
```

### Tag / Pill Pattern
```css
.tag {
  font-size: 10px;
  letter-spacing: 0.15em;
  text-transform: uppercase;
  color: var(--accent);
  border: 1px solid rgba(61,255,170,0.18);
  background: var(--accent-dim);
  padding: 3px 9px;
}
```

---

## Spacing & Layout

### Section padding
```css
.section { padding: 110px 7vw; }           /* desktop */
.section { padding: 80px 5vw; }            /* tablet ≤900px */
.section { padding: 60px 20px; }           /* mobile ≤599px */
```

### Content max-width
No explicit max-width on sections — layout breathes at 7vw gutter. Modal max-width: `900px`.

### Grid systems

**Bento (12-col, 3D work section):**
```css
.bento { display: grid; grid-template-columns: repeat(12, 1fr); gap: 14px; }
.bento-card--wide  { grid-column: span 7; }  /* feature card */
.bento-card--med   { grid-column: span 5; }
.bento-card--third { grid-column: span 4; }
/* Tablet: 2-col. Mobile: 1-col. */
```

**Square grid (design / equal items):**
```css
.design-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 14px; }
/* Tablet: 2-col. Mobile: 2-col (1-col only at ≤380px). */
```

**Two-column content (about / contact):**
```css
.two-col { display: grid; grid-template-columns: 1fr 1fr; gap: 80px; align-items: start; }
/* Tablet + mobile: single column. */
```

---

## Surfaces & Cards

```css
.card {
  background: var(--surface-2);
  border: 1px solid var(--border-soft);
  overflow: hidden;
  transition: border-color 0.3s ease, transform 0.3s ease, box-shadow 0.3s ease;
}
.card:hover {
  border-color: rgba(61,255,170,0.25);
  transform: translateY(-3px);
  box-shadow: 0 12px 48px rgba(0,0,0,0.5);
}
```

**Corner mark** — a small L-bracket in the top-left of any media/image container:
```css
.card-thumb::before {
  content: '';
  position: absolute;
  top: 14px; left: 14px;
  width: 16px; height: 16px;
  border-top: 1px solid rgba(61,255,170,0.3);
  border-left: 1px solid rgba(61,255,170,0.3);
  z-index: 3;
}
```

**Image zoom on hover** — wrap the image/gradient in an inner div, scale the inner:
```css
.card-thumb { overflow: hidden; position: relative; }
.card-thumb-inner { position: absolute; inset: 0; transition: transform 0.55s cubic-bezier(0.16,1,0.3,1); }
.card:hover .card-thumb-inner { transform: scale(1.05); }
```

**Hover reveal overlay:**
```css
.card-expand {
  position: absolute; inset: 0;
  background: rgba(8,8,10,0.65);
  display: flex; align-items: center; justify-content: center;
  opacity: 0;
  transition: opacity 0.22s ease;
  font-size: 10px; letter-spacing: 0.3em; text-transform: uppercase;
  color: var(--accent);
}
.card-expand::before { content: '+ '; }
.card:hover .card-expand { opacity: 1; }
```

---

## Animation System

### Easing curves
```css
--ease-out-expo:  cubic-bezier(0.16, 1, 0.3, 1);   /* primary — snappy settle */
--ease-standard:  ease;                              /* opacity fades */
--ease-in-out:    ease-in-out;                       /* loops, pulses */
```

### Duration scale
| Name | Value | Use |
|------|-------|-----|
| Instant | `0ms` | State toggles with no transition |
| Fast | `120–220ms` | Hover color, opacity micro-interactions |
| Standard | `300–350ms` | Modal open, card border |
| Deliberate | `550–650ms` | Card zoom, reveal fade |
| Slow | `900ms–1.2s` | Section rule draw, skill bar fill |

### Scroll reveal
Elements start invisible, fade up on intersection:
```css
.reveal {
  opacity: 0;
  transform: translateY(22px);
  transition: opacity 0.65s ease, transform 0.65s ease;
}
.reveal.in-view { opacity: 1; transform: none; }
```
```js
const obs = new IntersectionObserver(entries => {
  entries.forEach(e => { if (e.isIntersecting) e.target.classList.add('in-view'); });
}, { threshold: 0.1, rootMargin: '0px 0px -60px 0px' });
document.querySelectorAll('.reveal').forEach(el => obs.observe(el));
```

### Stagger grid
Cards fan in one by one after their container enters the viewport:
```css
.stagger-grid { opacity: 0; }
.stagger-grid.in-view { opacity: 1; }
.stagger-grid .card { opacity: 0; transform: translateY(18px); transition-property: opacity, transform; }
.stagger-grid .card:nth-child(1) { transition-delay: 0s; }
.stagger-grid .card:nth-child(2) { transition-delay: 0.07s; }
.stagger-grid .card:nth-child(3) { transition-delay: 0.14s; }
.stagger-grid .card:nth-child(4) { transition-delay: 0.21s; }
.stagger-grid .card:nth-child(5) { transition-delay: 0.28s; }
.stagger-grid .card:nth-child(6) { transition-delay: 0.35s; }
.stagger-grid.in-view .card { opacity: 1; transform: none; }
```

### Section rule draw
A horizontal rule that scales in from the left when the section header enters view:
```css
.section-rule {
  flex: 1; height: 1px;
  background: linear-gradient(to right, var(--border), transparent);
  transform: scaleX(0);
  transform-origin: left;
  transition: transform 0.9s cubic-bezier(0.16,1,0.3,1) 0.35s;
}
.section-head.in-view .section-rule { transform: scaleX(1); }
```

### Pulse (status dot, reel indicator)
```css
@keyframes pulse {
  0%, 100% { opacity: 0.4; transform: scale(1); }
  50%       { opacity: 1;   transform: scale(1.3); }
}
.pulse { animation: pulse 2s ease-in-out infinite; }
```

### Blink (cursor, active indicators)
```css
@keyframes blink {
  0%, 100% { opacity: 1; }
  50%       { opacity: 0; }
}
.blink { animation: blink 1.1s step-end infinite; }
```

### Typewriter (hero name)
Types each character with a blinking `|` cursor, then fades it out:
```js
async function typeLine(el, text, speed = 85) {
  const cursor = document.createElement('span');
  cursor.style.cssText = 'color:var(--accent);animation:blink 0.55s step-end infinite;';
  cursor.textContent = '|';
  el.appendChild(cursor);
  for (let i = 0; i <= text.length; i++) {
    const node = document.createTextNode(text.slice(0, i));
    el.insertBefore(node, cursor);
    if (el.childNodes.length > 2) el.removeChild(el.childNodes[0]);
    await new Promise(r => setTimeout(r, speed + Math.random() * 22));
  }
  el.textContent = text;
  el.appendChild(cursor);
  return cursor;
}
```

---

## Custom Cursor

**Two-element dot + ring.** Dot tracks instantly; ring lerps with slight weight.

```html
<div class="cursor-dot"  id="cursor-dot"></div>
<div class="cursor-ring" id="cursor-ring"></div>
```

```css
.cursor-dot {
  position: fixed; width: 3px; height: 3px;
  border-radius: 50%; background: var(--accent);
  pointer-events: none; z-index: 9999;
  transform: translate(-50%,-50%);
}
.cursor-ring {
  position: fixed; width: 20px; height: 20px;
  border-radius: 50%; border: 1px solid var(--accent);
  pointer-events: none; z-index: 9998;
  transform: translate(-50%,-50%); opacity: 0.65;
  transition: transform 0.22s cubic-bezier(0.16,1,0.3,1), opacity 0.22s ease;
}
body.cursor-hover .cursor-ring { transform: translate(-50%,-50%) scale(1.65); opacity: 1; }
@media (hover: none) {
  .cursor-dot, .cursor-ring { display: none; }
  body, a, button, [role="button"], input, textarea { cursor: auto; }
}
```

```js
const dot = document.getElementById('cursor-dot');
const ring = document.getElementById('cursor-ring');
let mx = 0, my = 0, rx = 0, ry = 0;

document.addEventListener('mousemove', e => {
  mx = e.clientX; my = e.clientY;
  dot.style.left = mx + 'px'; dot.style.top = my + 'px';
});

(function lerp() {
  rx += (mx - rx) * 0.30;  // 0.30 = tight follow with whisper of weight
  ry += (my - ry) * 0.30;
  ring.style.left = rx + 'px'; ring.style.top = ry + 'px';
  requestAnimationFrame(lerp);
})();

document.querySelectorAll('a, button, .card, [role="button"]').forEach(el => {
  el.addEventListener('mouseenter', () => document.body.classList.add('cursor-hover'));
  el.addEventListener('mouseleave', () => document.body.classList.remove('cursor-hover'));
});
```

---

## Buttons & Interactive Elements

**Primary action (outline, accent border):**
```css
.btn {
  background: transparent;
  border: 1px solid var(--accent);
  color: var(--accent);
  font-family: var(--font);
  font-size: 10px;
  letter-spacing: 0.22em;
  text-transform: uppercase;
  padding: 13px 28px;
  cursor: none;
  transition: background 0.22s ease, color 0.22s ease;
}
.btn:hover { background: var(--accent); color: var(--bg); }
```

**Ghost / secondary (dim border):**
```css
.btn-ghost {
  background: none;
  border: 1px solid var(--border);
  color: var(--text-dim);
  font-family: var(--font);
  font-size: 11px;
  letter-spacing: 0.1em;
  padding: 6px 14px;
  transition: border-color 0.2s ease, color 0.2s ease;
}
.btn-ghost:hover { border-color: var(--accent); color: var(--accent); }
```

**Link with arrow nudge:**
```css
.link-arrow {
  display: flex; align-items: center; gap: 12px;
  font-size: 13px; letter-spacing: 0.06em;
  color: var(--text-dim); text-decoration: none;
  transition: color 0.2s ease;
}
.link-arrow::before { content: '→'; color: var(--accent); transition: transform 0.2s ease; }
.link-arrow:hover { color: var(--accent); }
.link-arrow:hover::before { transform: translateX(5px); }
```

---

## Form Elements

```css
.form-field, .form-textarea {
  background: var(--bg);
  border: 1px solid var(--border);
  color: var(--text);
  font-family: var(--font);
  font-size: 13px;
  letter-spacing: 0.04em;
  padding: 12px 14px;
  outline: none;
  width: 100%;
  transition: border-color 0.2s ease, background 0.2s ease;
}
.form-field:focus, .form-textarea:focus {
  border-color: rgba(61,255,170,0.35);
  background: rgba(61,255,170,0.02);
}
.form-label {
  font-size: 10px; letter-spacing: 0.22em;
  text-transform: uppercase; color: var(--text-dim);
}
```

No border-radius on inputs. Sharp corners, consistent with the industrial feel.

---

## Navigation Patterns

### Dot nav (right-side vertical, desktop)
Dots indicate current section via IntersectionObserver. Labels reveal on hover with stagger.

```css
.nav { position: fixed; right: 1.75rem; top: 50%; transform: translateY(-50%); z-index: 900; }
.nav-pip { width: 5px; height: 5px; border-radius: 50%; background: var(--border); }
.nav-pip.active { background: var(--accent); box-shadow: var(--accent-glow); }
.nav-label { font-size: 10px; letter-spacing: 0.25em; text-transform: uppercase; color: var(--text-dim); opacity: 0; transform: translateX(10px); transition: opacity 0.25s ease, transform 0.25s ease; }
.nav-track:hover .nav-label { opacity: 1; transform: translateX(0); }
```

### Bottom tab bar (mobile ≤599px)
```css
@media (max-width: 599px) {
  .nav {
    position: fixed; bottom: 0; left: 0; width: 100%;
    background: rgba(8,8,10,0.96);
    backdrop-filter: blur(20px);
    border-top: 1px solid var(--border);
    padding-bottom: env(safe-area-inset-bottom, 0px);
  }
  .nav-track { flex-direction: row; justify-content: space-around; padding: 10px 0; }
  .nav-item { flex-direction: column; align-items: center; gap: 4px; }
  .nav-label { opacity: 1; transform: none; font-size: 8px; }
}
```

---

## Modal System

Single modal element populated by a JS data object — no duplicate markup.

```css
.modal-overlay {
  position: fixed; inset: 0;
  background: rgba(4,4,6,0.88);
  backdrop-filter: blur(12px);
  z-index: 1000;
  display: flex; align-items: center; justify-content: center; padding: 2rem;
  opacity: 0; pointer-events: none;
  transition: opacity 0.3s ease;
}
.modal-overlay.open { opacity: 1; pointer-events: all; }
.modal-panel {
  background: var(--surface); border: 1px solid var(--border);
  width: 100%; max-width: 900px; max-height: 90vh; overflow-y: auto;
  transform: translateY(20px) scale(0.97);
  transition: transform 0.35s cubic-bezier(0.16,1,0.3,1);
}
.modal-overlay.open .modal-panel { transform: translateY(0) scale(1); }

/* Mobile: bottom sheet */
@media (max-width: 599px) {
  .modal-overlay { padding: 0; align-items: flex-end; }
  .modal-panel { max-height: 93dvh; border-radius: 12px 12px 0 0; }
}
```

Open/close via class toggle, close on overlay click and Escape key.

---

## Background Textures & Atmosphere

### Hero animated gradient
Three radial blobs that slowly shift hue over 12s:
```css
.hero-bg {
  background:
    radial-gradient(ellipse at 65% 45%, rgba(22,12,60,0.9) 0%, transparent 55%),
    radial-gradient(ellipse at 20% 75%, rgba(0,55,45,0.5) 0%, transparent 45%),
    radial-gradient(ellipse at 85% 15%, rgba(45,8,60,0.4) 0%, transparent 40%),
    #08080a;
  animation: heroFloat 12s ease-in-out infinite alternate;
}
@keyframes heroFloat {
  0%   { filter: hue-rotate(0deg)   brightness(1);    }
  50%  { filter: hue-rotate(18deg)  brightness(1.04); }
  100% { filter: hue-rotate(-10deg) brightness(0.97); }
}
```

### Dot grid overlay
```css
.dot-grid::before {
  content: '';
  position: absolute; inset: 0;
  background-image: radial-gradient(circle, rgba(255,255,255,0.06) 1px, transparent 1px);
  background-size: 28px 28px;
  mask-image: radial-gradient(ellipse at 50% 50%, black 30%, transparent 80%);
}
```

### Scanline overlay
```css
.scanlines::after {
  content: '';
  position: absolute; inset: 0;
  background: repeating-linear-gradient(
    0deg, transparent, transparent 3px,
    rgba(0,0,0,0.04) 3px, rgba(0,0,0,0.04) 4px
  );
  pointer-events: none;
}
```

---

## Placeholder Gradients
For use before real images are ready. Each is a distinct dark-toned color story:

```css
.ph-1  { background: linear-gradient(135deg, #080814 0%, #16103a 45%, #260e50 75%, #3a1870 100%); }
.ph-2  { background: linear-gradient(145deg, #080c14 0%, #0c1a30 40%, #122240 70%, #1a3055 100%); }
.ph-3  { background: linear-gradient(125deg, #0e0808 0%, #2a0c0c 45%, #3e1010 75%, #5a1414 100%); }
.ph-4  { background: linear-gradient(150deg, #080c0a 0%, #0c1e12 45%, #101e14 70%, #182a1e 100%); }
.ph-5  { background: linear-gradient(130deg, #08080e 0%, #10101e 40%, #16162a 70%, #20203a 100%); }
```

---

## Responsive Breakpoints

| Breakpoint | Target | Key changes |
|-----------|--------|-------------|
| `≤900px` | Tablet | 2-col grids, stacked about/contact, section padding 80px 5vw |
| `≤599px` | Mobile | 1-col bento, bottom tab nav, full-screen modals, padding 60px 20px |
| `≤380px` | Small phone | 1-col design grid, nav labels shrink to 7px |

Always include safe area insets for notched phones:
```css
padding-bottom: env(safe-area-inset-bottom, 0px);
```

---

## Accessibility

- Text contrast floor: `--text-dim` (#7c7c8a on #08080a) = **5.1:1** — passes WCAG AA
- All interactive elements get `:focus-visible` styles
- Custom cursor is hidden on `(hover: none)` devices, native cursor restored
- Modal traps focus and responds to Escape
- `touch-action: manipulation` on cards removes 300ms tap delay on mobile
- Always include `prefers-reduced-motion`:

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    transition-duration: 0.01ms !important;
  }
}
```

---

## Favicon

Inline SVG — no external file needed:
```html
<link rel="icon" type="image/svg+xml" href="data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 32 32'><rect width='32' height='32' fill='%2308080a'/><text x='2' y='22' font-family='monospace' font-size='11' font-weight='700' fill='%233dffaa'>[ABI]</text></svg>">
```

Swap `[ABI]` for the project's short identifier and adjust `font-size` to fit.

---

## Quick-start Prompt for Claude

When starting a new UI project, paste this at the top of your message:

> Use the A Very Bad Idea design system. Dark industrial aesthetic: background `#08080a`, surfaces `#0f0f12` / `#161619`, accent `#3dffaa`, text `#b8b8c4` / `#7c7c8a` / `#e4e4f0`, borders `#222228`. Font: JetBrains Mono monospace throughout. No border-radius except modals (12px top corners on mobile). Labels in uppercase with 0.2–0.35em letter-spacing. Animations use `cubic-bezier(0.16,1,0.3,1)` for settles, standard `ease` for fades. Cards lift on hover with `translateY(-3px)` and accent border glow. Custom cursor: 3px dot (instant) + 20px ring (lerp 0.30). Refer to ABI-STYLE-GUIDE.md for full spec.
