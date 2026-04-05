# Dashboard Redesign — UX Research & Recommendations

Research compiled: 2026-04-03
Purpose: Redesign the dark-themed personal project tracker dashboard with best-in-class UX patterns.

---

## Part 1: Best-in-Class Dashboard Analysis

### Linear
**What makes it feel premium:**
- Instant optimistic UI — actions feel immediate because state updates before server confirms
- Keyboard-first design with `Cmd+K` command palette as the primary navigation layer
- Minimal chrome — the content IS the interface, almost no decorative UI
- Micro-animations on state transitions (issue status changes, drag reorder) are ~150ms with easing, never blocking
- Views (board/list/timeline) share identical data but reshape layout — no page reload, no state loss

**Project views:**
- **List view:** Dense, scannable rows. Each row: icon + title + assignee avatar + status pill + priority icon + date. No description shown inline.
- **Board view:** Kanban columns by status. Cards are minimal — title, assignee, priority dot. No card borders, just subtle elevation on hover.
- **Timeline view:** Gantt-style horizontal bars. Status encoded as bar color. Hover reveals detail.
- Key insight: All three views are just CSS layout changes on the same data. Toggle is instant.

**Status visualization:**
- Colored dots (not pills, not badges) — 6px circles: purple (backlog), yellow (in progress), green (done), gray (cancelled)
- Status is always the leftmost element — eye anchors there first
- No text labels on status in dense views, just color. Text appears on hover/expanded.

**Navigation:**
- Left sidebar: workspace → team → project hierarchy (collapsible)
- Top: view toggle (list/board/timeline) + filter bar
- `Cmd+K`: global search + command palette — this is where power users live
- No tabs. Sidebar + command palette eliminates tab sprawl.

---

### Vercel Dashboard
**What makes it feel premium:**
- Monochrome palette with one accent color (status-dependent)
- Typography does all the heavy lifting — Geist font family, clear size hierarchy
- Real-time status without polling UI — deployments update live via websockets
- Generous whitespace. Each deployment row breathes.

**Deployment status visualization:**
- Three states shown with icon + color: ● green (ready), ● yellow (building), ● red (error)
- The dot animates (pulse) during building state — movement draws attention without being distracting
- Timestamp is relative ("2m ago") not absolute — reduces cognitive load
- Commit message truncated to ~60 chars with ellipsis

**Layout:**
- No sidebar. Top nav only: project selector dropdown → tabs (deployments, analytics, settings)
- Each deployment is a full-width row: status dot + branch + commit + timestamp + author avatar
- Click expands inline (no page navigation) to show build logs
- Mobile: rows stack vertically, status dot stays left-anchored

---

### Raycast
**What makes it feel premium:**
- Single-input driven. Type to filter everything.
- Results appear in <50ms — feels like the UI is reading your mind
- Dark theme with frosted glass (vibrancy) background
- List items: icon + title + subtitle + right-aligned metadata + keyboard shortcut hint
- Zero learning curve — it's just a search bar that does everything

**Key pattern:**
- Command palette IS the entire UI — not a secondary nav, THE nav
- Sections group results (Applications, Files, Commands) with subtle headers
- Selection state: single highlighted row with accent background, everything else recedes

---

### Arc Browser Command Bar
**What makes it feel premium:**
- Convergent search — URLs, tabs, bookmarks, history, actions all in one input
- Inline previews reduce the need to "go" anywhere
- Spatial organization — sidebar groups (spaces) use color coding

**Key pattern:**
- Spaces (color-coded groups) as top-level organization — maps perfectly to project grouping
- Pinned items at top, ephemeral below — clear hierarchy of permanence
- Sidebar collapses to icons on narrow screens

---

### Notion Databases
**What makes it feel premium:**
- View flexibility — same data as table, board, calendar, gallery, list, timeline
- Filter/sort/group is inline and visual — not hidden in a menu
- Properties are first-class — each one gets its own column/field type with appropriate UI

**Filter/view handling:**
- Saved views as tabs across the top of the database
- Each view remembers its own filter, sort, group, and visible properties
- Filter builder: property → operator → value, shown as pills that can be removed
- Group by: any select/status property creates collapsible sections

**What doesn't work:**
- Information overload — Notion lets you show too many properties, making cards noisy
- Mobile Notion databases are clunky — horizontal scroll on tables, cards too tall on boards
- Lesson: constrain what's visible per view, especially on mobile

---

### Things 3
**What makes it feel premium:**
- Opinionated simplicity — you can't customize everything, and that's the point
- Natural language date parsing ("tomorrow", "next tuesday")
- Drag to reorder is the primary organization method — no priority numbers
- Headings within lists create visual sections without nested complexity

**Effortless task management:**
- Inbox → organized list flow. Capture fast, organize later.
- Today view: only what matters now. No overdue guilt trips.
- Checkbox completion animation: satisfying circle fill + strikethrough + fade out (~300ms)
- Areas → Projects → Tasks hierarchy. Three levels max. No deeper nesting.

**Key patterns:**
- Single-column layout. No sidebars, no multi-pane. One list, full attention.
- Upcoming view: grouped by date headers, not a calendar grid
- Tags are minimal — just colored dots, no text labels in list view

---

### Superlist / Height.app
**Superlist:**
- Hybrid notes + tasks. Each item can be a task OR rich text.
- Collapsible sections with drag reorder
- Status shown as left-border color on each item
- Mobile: full-width cards, swipe for actions (complete, delete, move)

**Height.app:**
- Spreadsheet-like density with app-like interactions
- Attributes (custom fields) shown as compact pills in list view
- Chat-style activity feed per task — feels conversational, not bureaucratic
- Status workflow: customizable stages with auto-transitions

---

## Part 2: Dark Theme Dashboard Patterns

### Color System
- **Background layers:** 3 tiers minimum
  - Base: `#0A0A0A` to `#111111` (true dark, not gray)
  - Surface: `#1A1A1A` to `#1E1E1E` (cards, panels)
  - Elevated: `#252525` to `#2A2A2A` (modals, dropdowns, hover states)
- **Never pure black (#000)** for large surfaces — causes halation on OLED and feels harsh
- **Never pure white (#FFF)** for text — use `#E5E5E5` or `#EBEBEB` for primary, `#888` for secondary
- **Accent color:** One. Pick one. Linear uses purple. Vercel uses white. You need exactly one brand accent for interactive elements.
- **Status colors on dark backgrounds:**
  - Active/Running: `#22C55E` (green, slightly desaturated)
  - Paused/Waiting: `#F59E0B` (amber)
  - Blocked/Error: `#EF4444` (red)
  - Done/Archived: `#6B7280` (gray)
  - Use these at ~80% opacity on dark backgrounds to avoid neon glare

### Typography
- **Font:** Inter, Geist, or SF Pro. System fonts are fine — don't overthink this.
- **Scale (mobile-first):**
  - Page title: 20px / 600 weight
  - Section header: 14px / 600 weight / uppercase tracking 0.05em (sparingly)
  - Card title: 15-16px / 500 weight
  - Card metadata: 12-13px / 400 weight / secondary color
  - Timestamps: 12px / 400 weight / tertiary color
- **Line height:** 1.4 for titles, 1.5 for body text
- **Letter spacing:** Slightly positive (0.01em) for small text on dark backgrounds improves readability

### Spacing System
- **Base unit:** 4px grid
- **Card internal padding:** 16px (mobile), 20px (desktop)
- **Gap between cards:** 8px (tight/list), 12px (grid)
- **Section spacing:** 32px between groups
- **Touch targets:** Minimum 44px height for any tappable element (Apple HIG)

### Card Design
- **Border:** None, or 1px `rgba(255,255,255,0.06)` — barely visible separator
- **Border-radius:** 8-12px (modern, not bubbly)
- **Shadow:** Skip it on dark themes — use border or background differentiation instead
- **Hover state:** Background lightens by one tier + subtle scale(1.005) transform
- **Active/pressed:** Background darkens slightly, scale(0.98) for tactile feedback

### Status Indicators
- **Dots > Pills > Badges** for compact views
  - 8px circle, status-colored, left of title
  - On hover/expanded: dot + text label
- **Progress:** Thin horizontal bar (2-3px) at card bottom or top, percentage fill with status color
- **Pulsing dot** for actively running items (subtle, 2s animation cycle)
- **No traffic lights** (red/yellow/green side by side) — colorblind-hostile and visually noisy

---

## Part 3: Top 10 UX Principles for This Dashboard

1. **Optimistic UI everywhere.** Every interaction should feel instant. Update the UI before the server confirms. Roll back on failure (rare). This is the #1 thing that makes Linear feel fast.

2. **One accent color. Status colors are functional, not decorative.** Don't rainbow the interface. Pick one brand color for interactive elements (buttons, links, focus rings). Status colors (green/amber/red/gray) are reserved for meaning.

3. **Command palette as first-class navigation.** `Cmd+K` (desktop) or search bar (mobile) should reach any project, any action, any view. Sidebar is for browsing; command palette is for doing.

4. **Progressive disclosure.** Cards show 3-4 data points max by default. Expand to see more. Desktop hover reveals secondary actions. Mobile swipe reveals actions. Don't front-load everything.

5. **Consistent spatial rhythm.** 4px grid. 16px card padding. 8-12px gaps. 32px section breaks. Once established, never deviate. The eye learns the rhythm and reads faster.

6. **Status is always position-zero.** First visual element on every card/row is the status indicator. The eye should know the state of an item before reading its name.

7. **Views, not pages.** Board/list/timeline/calendar should be view toggles on the same data, not separate pages. Toggle is instant (CSS layout change, not data refetch). Each view remembers its own filter/sort.

8. **Motion is communication, not decoration.** Every animation must convey state change. Completion: check + fade. Reorder: item slides to new position. Status change: dot color transition. Nothing should move just to look cool. Keep all transitions under 200ms.

9. **Mobile is the primary reading device; desktop is the primary editing device.** Design cards, layout, and information density for phone-first scanning. Layer on desktop-only features (keyboard shortcuts, multi-select, bulk actions, wider info columns) as progressive enhancement.

10. **Silence is a feature.** Empty states, completed items, and low-priority projects should visually recede. Use opacity, size, and position to create a natural focus hierarchy. The dashboard should scream "here's what needs attention" and whisper everything else.

---

## Part 4: Specific Layout Recommendation

### Desktop (>768px)

```
┌─────────────────────────────────────────────────┐
│  [Logo]  [Search / Cmd+K]            [Settings] │  ← Top bar, 48px
├──────┬──────────────────────────────────────────┤
│      │  [View: List | Board | ...]   [Filters]  │  ← View controls
│  S   │──────────────────────────────────────────│
│  I   │                                          │
│  D   │  ● Active Project Title          2d ago  │  ← Row item
│  E   │  ● Another Project               today   │
│  B   │                                          │
│  A   │  — Paused ——————————————————————————————│  ← Status group header
│  R   │                                          │
│      │  ◐ Waiting on X                  1w ago  │
│      │  ◐ Parked idea                   2w ago  │
│      │                                          │
├──────┴──────────────────────────────────────────┤
│  [Keyboard shortcut hints]                      │  ← Footer, optional
└─────────────────────────────────────────────────┘
```

**Sidebar (200px, collapsible to 48px icons):**
- Inbox / Today / All Projects
- Custom filters (saved views)
- Areas/categories
- Archive

**Main area:**
- Default: List view, grouped by status
- Each group (Active, Paused, Waiting, Done) is collapsible
- Active group expanded by default, others collapsed

### Mobile (<768px)

```
┌───────────────────────────┐
│  [☰]   Dashboard    [🔍]  │  ← Top bar, 48px
├───────────────────────────┤
│  [Today] [Active] [All]   │  ← Horizontal scroll tabs
├───────────────────────────┤
│                           │
│  ┌───────────────────────┐│
│  │ ● Project Title       ││  ← Full-width card
│  │   Last update · 2d    ││
│  │   Next: action item   ││
│  └───────────────────────┘│
│                           │
│  ┌───────────────────────┐│
│  │ ● Another Project     ││
│  │   Updated · today     ││
│  │   Next: ship v2       ││
│  └───────────────────────┘│
│                           │
│  — Paused (3) ──────────  │  ← Collapsed group
│                           │
│  — Waiting (2) ─────────  │  ← Collapsed group
│                           │
├───────────────────────────┤
│  [+]        [Home] [Menu] │  ← Bottom tab bar
└───────────────────────────┘
```

**Mobile-specific:**
- Bottom tab bar: Home, Today, Add (+), Search, Menu
- Swipe card left → actions (pause, archive, edit)
- Swipe card right → mark as done
- Pull to refresh
- Hamburger opens full sidebar as overlay
- FAB or bottom-bar "+" for quick add

---

## Part 5: Card Redesign Specs

### Compact Card (List View — Default)

```
┌─────────────────────────────────────────────┐
│  ●  Project Title                    2d ago │
└─────────────────────────────────────────────┘
```

- Height: 44-48px
- Status dot (8px) left-aligned
- Title: 15px/500, primary text color, truncate with ellipsis
- Timestamp: 12px/400, tertiary color, right-aligned
- No border between items — use alternating background (`rgba(255,255,255,0.02)`) or consistent gap
- Hover: row background → elevated tier + show inline action icons (edit, archive)

### Expanded Card (Detail View)

```
┌─────────────────────────────────────────────┐
│  ● Project Title                            │
│                                             │
│  Status: Active        Updated: 2 days ago  │
│  Area: Ventures        Priority: High       │
│                                             │
│  Next action:                               │
│  Ship the landing page by Friday            │
│                                             │
│  ┌─ Notes ────────────────────────────────┐ │
│  │ Last meeting discussed pricing...      │ │
│  └────────────────────────────────────────┘ │
│                                             │
│  [Pause]  [Archive]  [Edit]                 │
└─────────────────────────────────────────────┘
```

- Padding: 20px
- Section dividers: 1px `rgba(255,255,255,0.06)` or 16px space
- Metadata: 2-column grid on desktop, stacked on mobile
- "Next action" is the hero field — most prominent after title
- Notes: collapsible, shown truncated (3 lines) with "Show more"
- Actions: bottom of card, ghost buttons, icon + label

### Card Information Hierarchy (What to Show Where)

| Level | Data | When |
|-------|------|------|
| Always visible | Status dot, title, last updated | List view |
| On hover/tap | Next action, area/category | Expanded row |
| On open | Full notes, history, all metadata | Detail panel |

**Rule: Max 3 data points in compact view. Max 6 in expanded. Everything else in detail panel.**

---

## Part 6: Mobile-First Patterns

1. **Touch targets: 44px minimum.** Every button, every tappable row, every interactive element.

2. **Swipe gestures for primary actions.** Left-swipe: contextual actions (pause, edit, archive). Right-swipe: complete/primary action. Match iOS system patterns — users already know them.

3. **Bottom sheet > Modal.** For editing, filtering, and detail views on mobile, slide up a bottom sheet (half-screen initially, drag to full). Never use centered modals on mobile.

4. **Sticky filter bar.** Horizontal scrolling filter pills that stick below the header on scroll. Active filters shown as filled pills, inactive as outlined.

5. **Pull to refresh.** Standard. Include a subtle haptic on trigger threshold.

6. **Thumb-zone aware layout.** Primary actions (add, complete) in bottom 1/3 of screen. Navigation in bottom bar. Search in top bar (reachable via bottom bar shortcut).

7. **Offline-capable.** Cache the project list locally. Show stale data with a "last synced" indicator rather than a loading spinner.

8. **Keyboard avoidance.** When adding/editing, the input should never be hidden behind the keyboard. Use `scrollIntoView` or adjust padding dynamically.

---

## Part 7: What to REMOVE

1. **Remove verbose status text from list view.** A colored dot is enough. "Active," "Paused," "Waiting" are redundant when you're already grouping by status.

2. **Remove card borders.** On dark themes, borders add visual noise. Use spacing and background differentiation instead.

3. **Remove timestamps with full dates.** Use relative time ("2d ago", "just now") everywhere except detail views. Nobody needs to know it was "March 31, 2026 at 2:47 PM" at a glance.

4. **Remove multi-level nesting.** Max two levels: Area → Project. No sub-sub-projects. If it needs that, it needs its own project.

5. **Remove settings from the main UI.** Settings, preferences, and configuration belong behind a gear icon or in a separate page. They should never compete for attention with actual projects.

6. **Remove pagination.** Infinite scroll or "load more" for lists. Pagination is a web 1.0 pattern that breaks flow.

7. **Remove confirmation dialogs for reversible actions.** Archive, pause, status changes — do them instantly with an undo toast (3-5 seconds). Only confirm destructive, irreversible actions (permanent delete).

8. **Remove empty columns on board view.** If a status has zero items, collapse or hide it. Don't show empty lanes — they waste space and look sad.

9. **Remove "last edited by" on a personal dashboard.** It's YOUR dashboard. You know who edited it. Remove any multi-user artifacts that don't apply to single-player mode.

10. **Remove loading skeletons for cached data.** Show the last-known state immediately. Add a subtle refresh indicator if you're fetching updates. Skeleton screens should only appear on first load.

---

## Part 8: Implementation Priority

### Phase 1 — Foundation (ship first)
- [ ] Dark color system (3-tier backgrounds, status colors, one accent)
- [ ] Typography scale (Inter/Geist, defined sizes)
- [ ] Spacing system (4px grid, consistent padding)
- [ ] List view with status grouping
- [ ] Command palette (`Cmd+K`)

### Phase 2 — Interaction
- [ ] Card expand/collapse
- [ ] Swipe actions (mobile)
- [ ] Optimistic UI for status changes
- [ ] Undo toasts instead of confirmations
- [ ] Filters with saved views

### Phase 3 — Polish
- [ ] Board view toggle
- [ ] Micro-animations (status transitions, completion)
- [ ] Keyboard shortcuts throughout
- [ ] Offline cache
- [ ] Bottom sheet detail view (mobile)

---

## Reference Palette (Dark Theme Starter)

```css
:root {
  /* Backgrounds */
  --bg-base: #0C0C0C;
  --bg-surface: #1A1A1A;
  --bg-elevated: #262626;
  --bg-hover: #2E2E2E;

  /* Text */
  --text-primary: #EBEBEB;
  --text-secondary: #A0A0A0;
  --text-tertiary: #666666;

  /* Status */
  --status-active: #22C55E;
  --status-paused: #F59E0B;
  --status-waiting: #3B82F6;
  --status-blocked: #EF4444;
  --status-done: #6B7280;

  /* Accent */
  --accent: #8B5CF6;  /* or your brand color */
  --accent-hover: #7C3AED;

  /* Borders */
  --border-subtle: rgba(255, 255, 255, 0.06);
  --border-medium: rgba(255, 255, 255, 0.12);

  /* Radius */
  --radius-sm: 6px;
  --radius-md: 10px;
  --radius-lg: 16px;

  /* Spacing */
  --space-xs: 4px;
  --space-sm: 8px;
  --space-md: 16px;
  --space-lg: 24px;
  --space-xl: 32px;
}
```

---

*Research based on analysis of Linear, Vercel, Raycast, Arc Browser, Notion, Superlist, Height.app, and Things 3 design patterns as of 2026.*
