# Design System — DM Cheat Sheet

> Readability first. Aesthetic is secondary. This is a reference tool used under table conditions — dim lighting, fast lookups, phones at arm's length. Every decision should pass the test: "Can I find what I need in under two seconds?"

---

## Philosophy

- **Scan speed over visual richness.** Dense numeric data needs maximum contrast and consistent layout.
- **Character comes from typography and amber, not from background hue.** The palette is neutral charcoal — no blue-shift, no warmth. The D&D feel comes from Cinzel and the heat-map amber.
- **Numbers are first-class citizens.** Every stat, modifier, and score uses JetBrains Mono with tabular-nums so columns don't dance.
- **Identity colors are a separate semantic layer from heat maps.** Green/red/amber = value quality. Character colors = who this belongs to. Never mix these signals.

---

## Color Tokens

```css
:root {
  /* Base palette */
  --bg:       #141414;   /* page background — warm dark charcoal */
  --surface:  #1e1e1e;   /* card, panel, sticky column background */
  --surface2: #272727;   /* table header, nested elements */
  --border:   #383838;   /* borders, dividers */
  --text:     #f0f0f0;   /* primary text — 14.7:1 contrast on bg (AAA) */
  --muted:    #a8a8a8;   /* labels, secondary info — 5.9:1 on bg (AA) */
  --subtle:   #707070;   /* metadata, section chrome — low priority */

  /* Semantic: value quality (heat maps) */
  --amber:  #e8a820;   /* best-in-party highlight — 9.3:1 on bg (AAA) */
  --green:  #52b566;   /* heat map: high value — 5.5:1 on bg (AA) */
  --danger: #d04848;   /* heat map: low value — 4.6:1 on bg (AA) */

  /* Character identity — border/stripe color */
  --c-beren:   #4d9e6a;
  --c-dorian:  #8855cc;
  --c-nappa:   #b83a3a;
  --c-gareth:  #3a70a0;
  --c-og:      #9a7a20;
  --c-andrick: #28a090;

  /* Character identity — lightened for text on dark surfaces */
  --c-beren-lt:   #72c28e;
  --c-dorian-lt:  #aa7de8;
  --c-nappa-lt:   #d46868;
  --c-gareth-lt:  #5a96c8;
  --c-og-lt:      #c8a030;
  --c-andrick-lt: #5ac8b8;
}
```

### Contrast Reference

| Pairing | Ratio | Grade |
|---------|-------|-------|
| `--text` on `--bg` | 14.7:1 | AAA |
| `--muted` on `--bg` | 5.9:1 | AA |
| `--muted` on `--surface` | 5.4:1 | AA |
| `--amber` on `--bg` | 9.3:1 | AAA |
| `--green` on `--bg` | 5.5:1 | AA |
| `--danger` on `--bg` | 4.6:1 | AA |

---

## Typography

Three fonts, each with a specific job. Never swap them.

| Role | Font | Weight | Size | Use for |
|------|------|--------|------|---------|
| Display | Cinzel | 700 | 24–28px | Page title only (`DM Cheat Sheet`) |
| Display SM | Cinzel | 600 | 14–16px | Character names on cards |
| Section head | Inter | 600 | 11px / 0.1em tracking / uppercase | Table section labels (`COMBAT AT A GLANCE`) |
| Body | Inter | 400 | 15px | Descriptive text, special abilities |
| Label | Inter | 500–600 | 12px | Stat labels (HP, AC, INIT), column headers |
| Tag | Inter | 600 | 11px / uppercase | Tags, chips, badges |
| Numbers | JetBrains Mono | 600 | 18–22px | HP, AC, Initiative, Speed (big four) |
| Modifiers | JetBrains Mono | 500 | 14px | Saves, ability scores, spell stats |
| Small nums | JetBrains Mono | 400 | 12–13px | Skill values, passive senses |

**Rules:**
- Minimum label size: **12px** at weight 500 or 600. Never smaller. Weight carries more legibility than size.
- All numeric values: `font-variant-numeric: tabular-nums` — columns align without manual spacing.
- Cinzel only appears on the page title and character names. Everything else is Inter.

**Google Fonts import:**
```css
@import url('https://fonts.googleapis.com/css2?family=Cinzel:wght@600;700&family=Inter:wght@400;500;600;700&family=JetBrains+Mono:wght@400;500;600&display=swap');
```

---

## Character Identity System

Each character has two color values:
- **Border color** — used for left-border stripes on cards, top-border accents on table columns
- **Text color (lightened)** — used for character name text and focus chip color

| Character | Class | Border | Text |
|-----------|-------|--------|------|
| Beren | Ranger · Wood Elf | `#4d9e6a` | `#72c28e` |
| Dorian | Warlock · Elf | `#8855cc` | `#aa7de8` |
| Nappa | Barbarian · Orc | `#b83a3a` | `#d46868` |
| Gareth | Paladin · Dwarf | `#3a70a0` | `#5a96c8` |
| Óg | Bard · Half-Elf | `#9a7a20` | `#c8a030` |
| Andrick | Fighter · Dragonborn | `#28a090` | `#5ac8b8` |

### Application

**On cards:**
- 4px left border: `border-left: 4px solid var(--c-<name>)`
- Character name text: `color: var(--c-<name>-lt)`

**On table column headers:**
- Top border: `border-top: 3px solid var(--c-<name>)`
- Name text: `color: var(--c-<name>-lt)`

**On focus chips (mobile):**
- Border and text: `var(--c-<name>-lt)`
- Subtle background tint when active: `background: rgba(<name-rgb>, 0.08)`

---

## Heat Map System

Applied to table cells only. Represents value quality relative to the party. Completely separate from character identity colors.

```css
.ht5 { background: rgba(82, 181, 102, 0.35); }   /* high */
.ht4 { background: rgba(82, 181, 102, 0.20); }   /* above avg */
.ht3 { background: transparent; }                 /* average */
.ht2 { background: rgba(208, 72, 72, 0.18); }    /* below avg */
.ht1 { background: rgba(208, 72, 72, 0.32); }    /* low */

/* Best-in-party — applied in addition to .ht5 */
.best { color: var(--amber); font-weight: 600; }
```

Tiers are relative to the party max/min per stat. ht5 = highest, ht1 = lowest.

---

## Component Patterns

### Card

```
border: 1px solid var(--border)
border-left: 4px solid var(--c-<name>)   ← character identity
border-radius: 4px
background: var(--surface)
padding: 14–16px
```

Character name: Cinzel 600, 14–16px, `var(--c-<name>-lt)`
Class/race: Inter 500, 11px, `var(--muted)`, uppercase, `letter-spacing: 0.03em`

### Stat Block (big four: HP/AC/INIT/SPD)

```
4-column grid, centered
Value: JetBrains Mono 600, 18–22px, var(--text)
Label: Inter 600, 10px, var(--muted), uppercase, letter-spacing: 0.06em
Best-in-party value: color var(--amber)
```

### Table

```
border: 1px solid var(--border)
border-radius: 4px
overflow-x: auto
```

- First column: `position: sticky; left: 0; background: var(--surface)` — must be opaque (not transparent) to prevent content bleed-through during horizontal scroll
- Header row: `background: var(--surface2)`
- Character column headers: colored top border + colored name text (see identity system)
- Section rows: `background: var(--bg)`, Inter 600 10px uppercase subtle — visual break between Combat/Saves/Abilities/Skills

### Focus Chips (mobile)

Row of character chips above the table. Tapping a chip sets `HL` to that character index and calls `build()`. Non-selected columns get `opacity: 0.15`. "All" chip sets `HL = null` and restores full heat map.

```html
<div class="focus-bar">
  <button class="focus-chip all-active">All</button>
  <button class="focus-chip chip-beren">Beren</button>
  ...
</div>
```

---

## Spacing & Shape

- **Border radius:** 4px max. Rectangular, document-like. Not rounded, not pill-shaped.
- **Card padding:** 14–16px
- **Section gap:** 12–14px between sections within a card
- **Table cell padding:** 7px vertical, 10–12px horizontal
- **Grid gap (cards):** 10–12px

---

## What Not to Do

- No decorative gradients on backgrounds
- No colored backgrounds behind non-semantic elements
- No Cinzel below 13px (it loses its character)
- No Inter for numeric data (use JetBrains Mono)
- No labels below 12px / weight 400 (use 500 or 600 for secondary text)
- No character identity colors on heat-map cells (separate the two systems)
- No new Google Fonts or CDN dependencies — the three above are the complete set

---

*Generated via `/design-consultation` · 2026-03-27 · Approach B: Mobile-First Party View*
