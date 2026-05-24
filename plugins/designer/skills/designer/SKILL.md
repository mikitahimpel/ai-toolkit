---
name: designer
description: Defines the aesthetic taste and design principles to apply to any UI work — critique, implementation, or decision-making. Invoke whenever the work needs an opinionated voice instead of generic defaults.
---

This skill is the source of truth for design taste — what good looks like and what to reject. It is stack-agnostic by design; implementation details belong to the calling context.

## The lens

Design with restraint, conviction, and craft. The work should feel composed by someone with strong opinions who said no to most of their first ideas. Confidence reads as quiet — loud designs are usually anxious. The interface should disappear into the content while still having a recognizable point of view.

If a design feels like a template, it has failed. If it looks like every other product in its category, it has failed. If the user can tell within a second what the page is *about* and what the most important action is, it has succeeded.

## Core principles

### Hierarchy is the whole game
A page works when the eye lands on one thing first, then a second, then optional thirds. Three primary actions = no primary action. If everything is bold, nothing is bold. Use size, weight, color, and whitespace to enforce a single visual order. Every element should know where it ranks.

### Restraint over decoration
Fewer elements, more thinking per element. Remove until removing the next thing hurts the meaning — then stop. The page that "looks empty" to a novice usually looks composed to a designer. Negative space is a deliberate choice, not absence.

### Typography carries the aesthetic
- Pair a distinctive display face with a refined body face. The display face is the personality.
- Reject defaults that are everyone's choice. Generic system or web-default fonts as the *intentional design* signal lack of decision.
- Type scale should feel composed (1.125–1.333 ratio), not random. Line-height ~1.4–1.6 for body, ~1.05–1.2 for display.
- Tighten letter-spacing on large display type; leave body alone. Optical correction matters.
- Treat the type ladder as a system — a reader should sense the hierarchy without reading.

### Color discipline
- A foundation (one warm-neutral or cool-neutral) plus 1–2 accents. Not a palette of six.
- Accent is rare and reserved for emphasis (primary action, key data). Spent everywhere, it stops meaning anything.
- High contrast where it matters; muted everywhere else.
- Dark mode is not "invert the colors" — it's its own composition, with re-considered surface levels and accent saturation.
- Avoid the AI/SaaS default palette (purple/violet gradient on white). Pick something the category isn't already wearing.

### Space as composition
- A clear rhythm of spacing tokens (e.g. 4 / 8 / 16 / 24 / 32 / 48 / 64) — and commit to it. Ad-hoc gaps (13px, 17px, 22px) read as careless.
- Tight grouping for related items; generous separation between sections. The grouping itself communicates structure.
- Intentional asymmetry and grid-breaks beat mechanically-centered everything.
- Generous margins around a feature image signal confidence; tight crops signal anxiety.

### Materiality and surface
- Containers should have considered weight: subtle shadows, soft borders, or layered backgrounds — chosen, not defaulted.
- Atmospheric depth (subtle gradients, noise, soft layering) lifts flat blocks of color without becoming decorative.
- Surfaces should imply level: a card sits on a section sits on a page. One container layer per concern — not three nested.
- Edges and corners are a stylistic choice: hard right angles, generous radii, or precise small radii — pick one and use it everywhere.

### Motion serves meaning
- One well-orchestrated moment beats five scattered micro-interactions. A single staggered entrance is more delightful than every button bouncing on hover.
- Easing should feel physical: ease-out for entrances, custom cubic-bezier or spring physics for anything substantial. Linear easing reads as robotic.
- Duration: ~150–250ms for interactions, ~300–700ms for entrances, longer only for cinematic moments.
- Animation should explain a state change or guide attention, not entertain. Decorative motion on functional surfaces (settings, forms) is noise.
- Respect prefers-reduced-motion without exception.

### Iconography has voice
- Icons are part of the type system, not clip art. They should share weight, stroke width, and visual rhythm.
- A consistent icon family beats a mix-and-match grab bag, even if individual icons are nicer in isolation.
- Custom icons signal product care; off-the-shelf sets signal "we shipped".

### Content leads, chrome recedes
- Navigation, headers, and toolbars exist to serve the payload — they shouldn't compete with it.
- When in doubt, make the UI quieter and let the data, copy, or imagery be the loudest thing on the page.
- Marketing surfaces invert this: there the *page itself* is the payload, so composition can be bolder.

### Adapt density to context
- Sparse for marketing and onboarding; denser for power tools and dashboards. The same product can shift density across surfaces.
- Density without crowding requires real attention to alignment, baseline grids, and consistent gutters.

## Anti-patterns (auto-fail)

Flag any of these on sight:

- **Card-in-card-in-card** — nested containers stacking borders/shadows. Pick one container level per concern.
- **Image cropped to nonsense** — hero/feature images where the subject is cut, the aspect is wrong, or `overflow:hidden` is doing damage.
- **Image rendered as thumbnail** — visuals that should anchor a section sized like favicons.
- **Generic template silhouette** — centered hero + three feature cards + footer, with no point of view.
- **The AI-slop trifecta** — default sans-serif + purple/violet gradient + frosted-glass card. Run away.
- **Mobile as a shrunken desktop** — same hierarchy mechanically scaled down instead of re-composed for the viewport.
- **Inconsistent spacing rhythm** — gaps off the token scale, signaling no system underneath.
- **Low-contrast body text** — body copy below 4.5:1 on its background; non-decorative copy below 3:1.
- **Toy-store color** — saturated rainbow palettes outside explicitly playful contexts.
- **Flat empty planes** — large solid-color regions that needed atmosphere (gradient, noise, image, structure) and got none.
- **CTA confusion** — multiple buttons of equal visual weight competing as primary.
- **Decorative animation on functional pages** — bouncy hover effects on a settings screen or a form.
- **Inconsistent corner radii** — pills, sharp corners, and rounded corners mixed on the same surface without intent.
- **Borrowed-icon-set look** — clearly off-the-shelf, mismatched weights, no system.
- **Empty states left blank** — every empty state is a design surface, not a "todo".

## How to apply

When critiquing existing UI: walk the work top-to-bottom. Name the first hierarchy violation, then the second. Flag any anti-pattern on sight. Tie each finding to a principle here rather than to personal opinion — that keeps critique objective and the fix obvious.

When making new UI: before producing anything, commit out loud to one direction in a single sentence (e.g. "editorial-minimal with one hero gradient and tight ink-on-paper typography"). Then execute with restraint *at the chosen intensity*. Maximalism done well demands more discipline, not less.

When making a small judgement call (one component, one color choice, one spacing decision): pick the choice that aligns with the most principles above. If two choices tie, pick the more restrained one.

When in doubt, ask: *would someone who designs for a living be proud to ship this?* If no, iterate.
