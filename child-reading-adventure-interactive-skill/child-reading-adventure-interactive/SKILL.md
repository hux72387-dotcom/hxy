---
name: child-reading-adventure-interactive
description: Enhanced child reading adventure designer. In addition to the original Markdown/DOCX/PDF output, generates interactive HTML handbooks with drag-and-drop, clickable choices, mind maps, sound tracks, rule labs, role stages, and a night-sky collection — all playable in a browser with no installation. Use when a user asks for children's reading methods, book-based projects, activity cards, reading challenges, interactive handbooks, or digital reading adventures, especially when the child will use the output on a screen.
---

# Design Child Reading Adventures — Interactive Edition

This skill extends the original `design-child-reading-adventures` with an **interactive HTML output mode**. All original content-grounding, safety, child-agency, and quantitative-gate rules remain in force. The interactive layer adds browser-playable mechanics on top of the same text-dependent, child-owned design philosophy.

## Core rules (inherited)

Read `references/content-grounding.md`, `references/activity-library.md`, `references/safety-and-tools.md`, and `references/design-principles.md` before designing. Apply all constraints from the original skill:

1. physical safety, privacy, lawful access, and copyright;
2. content grounding and no invented book details;
3. explicit user constraints and the selected output mode;
4. child agency and independent completion;
5. protected reading time and text-connected activity design;
6. layout, variety, ratio, and visual enhancements.

Never override a child's stated preference solely to satisfy a lower-priority quota.

## Select the output mode

Choose one mode from the request; do not ask a question only to choose a mode.

- `ideas`: Return a bounded set of ideas (same as original).
- `plan`: Return the adult-facing reading adventure plan (same as original).
- `full`: Return the adult-facing plan and child-facing handbook (same as original).
- **`interactive`** (new): Return the adult-facing plan **plus** a self-contained interactive HTML file that the child can open in any browser with no installation.
- `full+interactive`: Return all three — adult plan, Markdown handbook, and interactive HTML.

When the request is ambiguous, choose `interactive` (it subsumes the Markdown handbook) and mention that a printable version is available on request.

## Scale the parent-set intake to the mode

Use the same intake rules as the original skill. When the mode is `interactive` or `full+interactive`, also collect:

- preferred screen: tablet, laptop, or phone (affects layout width and touch targets);
- whether the child will use the file offline (default: yes — all assets inline, no CDN).

## Content-grounding gate

Apply the original content-grounding gate unchanged. The interactive format does not relax evidence requirements. All book-specific content in the HTML must meet the same Level A/B/C/D constraints.

## Build the interactive handbook

### Overall structure

Generate a single self-contained HTML file that includes all CSS and JavaScript inline. No external dependencies, no CDN links, no build step. The file must work when opened directly from the filesystem (`file://`).

### Page architecture

Use a single-page app pattern with section-based navigation:

1. **Cover** — adventure name, player name input, "Start now" button.
2. **Start Now** — immediate 3-minute story-door entry (same as original).
3. **Adventure Map** — interactive visual map serving as both contents and navigation; each location is clickable.
4. **Station pages** — one per adventure station, each following the two-page packet pattern.
5. **Night Sky** — star-collection page for optional traces.
6. **Any Story Door** — generic station for switching books.
7. **Tool Cards** — optional help cards (dictionary, audiobook, timer, ask adult).

### Station page layout (interactive two-page packet)

Each station must implement the **choose-read-play-trace-bridge-next** loop:

**Page A — Story scene (door opens)**

1. **Scene title** — short, action-driven, from verified content.
2. **Reading prompt** — enter reading via verified chapter/section/event cue or generic child-controlled entry; never show unverified page numbers.
3. **Timer** — optional reading timer with start/pause/reset; never auto-start.
4. **Play invitation** — "Now make it move" with one text-grounded input that changes the play.

**Page B — Playable surface (this page is yours)**

Implement one or more interactive mechanics (see below). The child must be able to make the first meaningful interaction within 90 seconds.

### Interactive mechanics catalog

Select mechanics that match the activity's primary interaction grammar. Do not repeat the same mechanic in adjacent stations; use at least four distinct mechanics when n >= 4.

| Mechanic | Interaction grammar | HTML implementation |
|----------|--------------------|--------------------|
| **Clue collector** | Inferring from clues | Text inputs with visual fill-state; auto-highlight when content entered |
| **Path chooser** | Choosing a route with consequences | Clickable choice cards; reveal consequence panel on selection |
| **Sound track** | Encoding/decoding with sound | Draggable sound items; volume sliders; play buttons; add/remove sounds |
| **Mind map** | Allocating and connecting | Draggable nodes; SVG lines; double-click to edit; add-node button |
| **Rule lab** | Revising a rule and testing consequences | Old-rule / new-rule inputs; consequence reveal when both filled |
| **Role stage** | Performing a role response | Avatar that reacts to input; speech bubble; optional new-character input |
| **Stamp trail** | Multiple grammars (light) | Clickable stamp circles per station; collected state persists in session |
| **Star collection** | Reviewing and choosing | Clickable stars on a night sky; label on click; add-star button |

### Interactive design rules

1. **Child control**: Every interactive element must preserve at least three child-controlled dimensions (goal, method, rule, order, outcome, medium, or whether to leave a trace). The interactive wrapper must not lock goal, method, rules, and result simultaneously just because the UI has defined input fields.
2. **Just read**: Every station page must include a visible "📖 今天只读" button that is a complete, valid choice.
3. **No grading**: Never auto-score, auto-correct, show progress bars tied to completion, or require all fields filled before navigation.
4. **No streaks**: Stamp and star collection must not show streaks, counts, comparisons, or "you missed X" messages.
5. **No external calls**: All JavaScript runs locally. No fetch, no XMLHttpRequest, no WebSocket, no external images, no analytics.
6. **No personal data**: Do not ask for real name (use placeholder like "我的冒险名字"). Do not collect, store, or transmit any personal data. SessionStorage may be used for in-session state only.
7. **Offline-first**: All CSS, JS, and assets are inline. The file works without internet.
8. **Accessibility**: Use large touch targets (min 44px), sufficient color contrast (WCAG AA), non-color navigation cues, and keyboard-navigable focus order. All interactive elements must have visible focus states.
9. **Safety in UI**: If an activity would require adult safety help in the physical version (loose parts, string, etc.), mark it `🟠 求助卡` in the HTML and provide a safe/no-material alternative toggle.
10. **Responsive**: Layout must work on phones (375px), tablets (768px), and laptops (1024px+).

### Navigation

- **Top nav bar**: Fixed, horizontally scrollable, with one button per station. Active station highlighted; visited stations marked with a star icon.
- **Next doors**: Every station ends with 2–3 named next-door buttons plus "任意故事门" and "今天只读".
- **Map as home**: The adventure map is both the table of contents and the primary navigation surface. Clicking a location scrolls to or shows that station.

### Visual design

- Use CSS custom properties for a limited, coherent color palette.
- Use large, readable font sizes (min 16px body, 24px+ headings for child-facing content).
- Use emoji as icons (no external icon libraries needed for offline use).
- Use CSS gradients and shadows for depth — no images required.
- The overall aesthetic should feel like a picturebook world, not a dashboard or form.
- Keep the adult-facing plan in a separate file (Markdown or DOCX). The HTML is child-only.

## Deliver the selected mode

### `interactive`

Return:

1. an adult-facing plan (Markdown) adapted from `assets/parent-reading-plan-template.md`;
2. a self-contained interactive HTML file adapted from `assets/interactive-handbook-template.md`.

### `full+interactive`

Return all three:

1. adult-facing plan (Markdown);
2. child-facing Markdown handbook;
3. interactive HTML file.

### Other modes (`ideas`, `plan`, `full`)

Deliver exactly as the original skill specifies.

## Interactive delivery gate

In addition to the original delivery gate, reject or repair the HTML if it:

- requires internet access, external resources, or a build step;
- auto-scores, auto-corrects, or prevents navigation when fields are empty;
- stores or transmits personal data;
- uses `<img>` tags with external `src` or inline base64 images larger than 50KB total;
- does not work at 375px viewport width;
- lacks keyboard focus states on interactive elements;
- hides the "今天只读" button or makes it less prominent than play options;
- allows the same interactive mechanic in adjacent core activities without a justified exception in the adult plan;
- includes a progress bar, streak counter, completion percentage, or ranking visible to the child;
- uses `alert()`, `confirm()`, or `prompt()` for core gameplay (only for optional node editing in the mind map is `prompt()` acceptable as a lightweight input method).

Confirm that the child can open the file, find the Start Now entry, make a first interaction within 90 seconds, navigate to any station, stop at any point, and leave no data trace — all without adult performance management.
