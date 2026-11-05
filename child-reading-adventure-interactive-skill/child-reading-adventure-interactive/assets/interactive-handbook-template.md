# Interactive Handbook Template

> This template guides the generation of a self-contained HTML file. Do not copy this prose into the HTML; use it as a structural specification.

## File requirements

- Single `.html` file, all CSS and JS inline.
- No external dependencies, no CDN, no build step, no fetch/XHR/WebSocket.
- Works at `file://` without internet.
- Total file size target: under 200KB (no base64 images over 50KB total).
- Language: match the user's request (default: zh-CN).

## HTML structure

```
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>{adventure_name} — 互动手册</title>
  <style>/* All CSS inline here */</style>
</head>
<body>
  <!-- Nav bar -->
  <!-- Cover page -->
  <!-- Start Now page -->
  <!-- Adventure Map page -->
  <!-- Station pages (one per station) -->
  <!-- Night Sky page -->
  <!-- Any Story Door page -->
  <!-- Tool Cards (end) -->
  <script>/* All JS inline here */</script>
</body>
</html>
```

## CSS specification

### Custom properties

Define a coherent palette via CSS custom properties:

```css
:root {
  --sky / --forest / --ocean / --sunset / --night:  /* scene backgrounds */
  --accent / --accent2 / --accent3 / --accent4:     /* action colors */
  --text / --text-light:                             /* text colors */
  --card-shadow / --radius:                          /* layout tokens */
}
```

### Layout

- Max content width: 800px, centered.
- Nav bar: fixed top, horizontally scrollable, one button per section.
- Station pages: white card on colored background, rounded corners, shadow.
- Responsive breakpoints: 375px (phone), 768px (tablet), 1024px+ (laptop).
- Min touch target: 44px × 44px.
- Min body font: 16px; child-facing headings: 24px+.

### Component styles

- **Cover**: gradient background, large title, name input, start button.
- **Read prompt**: blue left-border box.
- **Play zone**: dashed yellow border box.
- **Clue item**: flex row, input with bottom border, fills green on input.
- **Choice card**: bordered card, scales on hover, highlights on select.
- **Sound item**: draggable row with icon, input, volume slider, play button.
- **Mind node**: absolutely positioned, rounded, draggable, colored gradient.
- **Rule card**: bordered card with inputs, consequence reveal.
- **Role avatar**: circle with emoji, pulse animation on speak.
- **Speech bubble**: rounded box with CSS triangle.
- **Night sky**: dark gradient container, absolutely positioned stars.
- **Stamp**: small circle, toggles to filled star on click.
- **Next doors**: flex row of pill buttons at section bottom.
- **Timer**: inline flex with display, start/pause button.
- **Just read button**: dashed border, centered, always visible.

### Animations

- Page fade-in on navigation.
- Hover lift on cards and buttons.
- Pulse on role avatar when speaking.
- Twinkle on night-sky stars.
- No auto-play animations that could distract from reading.

## JavaScript specification

### Navigation

```js
function goTo(pageId)   // hide all pages, show target, mark visited
function updateNav()     // sync nav-bar active/visited states
```

- Track visited pages in a `Set`.
- Scroll to top on navigation.
- Smooth scroll behavior.

### Timer

```js
function toggleTimer(spanId, btnId)  // start/pause timer, update display
```

- Per-station independent timers.
- Display format: MM:SS.
- Never auto-start.

### Interactive mechanics

#### Clue collector
- Text inputs with `oninput` handler that toggles `.filled` class.
- Visual feedback: border color change on fill.

#### Path chooser
- Clickable `.choice-card` elements.
- On selection: highlight selected, reveal consequence panel with `.show` class.
- Multiple choice groups supported via data attributes.

#### Sound track
- Draggable `.sound-item` elements using HTML5 drag-and-drop.
- `dragstart`, `dragover`, `drop`, `dragend` handlers.
- Volume range sliders.
- Play button with brief visual feedback (icon change for 800ms).
- Add-sound button appends new draggable item with random emoji icon.

#### Mind map
- Absolutely positioned `.mind-node` elements.
- Mouse drag via `mousedown/mousemove/mouseup`.
- SVG overlay for connection lines, redrawn on every drag.
- Double-click to edit node text (prompt() acceptable here only).
- Add-node button with random position and rotating color palette.
- Root node centered, draggable.

#### Rule lab
- Two input fields: old rule and new rule.
- When both have content, reveal consequence panel.
- Consequence panel has additional inputs for "world becomes" and "new problem".

#### Role stage
- Avatar circle that changes content based on name input.
- Pulse animation class toggled when speech input receives content.
- Speech bubble with input.

#### Stamp trail
- Clickable `.stamp` circles that toggle `.collected` class.
- After toggle, show ⭐ emoji.
- Per-station, 2–3 stamps.

#### Star collection (Night Sky)
- Absolutely positioned `.star` elements on dark gradient.
- Click to label: prompt for text, then show label on hover and glow effect.
- Add-star button with random position.

### Session storage (optional)

- May use `sessionStorage` to preserve in-session state (visited pages, input values, stamp states, star labels).
- Must NOT use `localStorage` or any persistent storage.
- Must NOT store any personal data.

### Data safety

- No `fetch`, `XMLHttpRequest`, `WebSocket`, `navigator.sendBeacon`.
- No `<img>` with external `src`.
- No `<script src="...">`.
- No `<a href="...">` to external sites (internal `#` anchors only).
- No form submissions.
- No `eval()` or `Function()` constructor.

## Content injection rules

When generating the HTML from a specific reading adventure design:

1. **Station names, icons, and colors**: Inject from the adventure design. Each station gets a unique emoji icon and accent color.
2. **Reading prompts**: Use verified chapter/section/event cues. Never inject unverified page numbers.
3. **Play invitations**: Use the text-grounded input and play effect from each activity design.
4. **Hint examples**: Include the optional example from each activity, hidden behind a toggle.
5. **Next doors**: Match the route from the adult plan. Names must be identical.
6. **Adventure map**: Draw a simple node-and-line map matching the station sequence, but allow non-linear navigation.

## Accessibility checklist

Before delivery, verify:

- [ ] All interactive elements have `:focus-visible` styles
- [ ] Tab order follows visual order
- [ ] Color is not the only differentiator (use shapes, labels, icons)
- [ ] Font size >= 16px for body text
- [ ] Touch targets >= 44px
- [ ] `lang` attribute set on `<html>`
- [ ] No `alert()`/`confirm()` for core gameplay (only `prompt()` for mind-map node editing)
- [ ] "今天只读" button is always visible and prominent
