# Design Spec — DMS v2 (Phase 1)

Source image: University Admissions Portal "My profile" screen. Adapted to the DMS app (staff-facing document management). Tokens govern every screen.

## 1. Design tokens

### Colors
| Token | Hex | Role |
|---|---|---|
| `--color-primary` | `#1F7A46` | Brand green: banner, primary buttons, active nav |
| `--color-primary-hover` | `#155E34` | Darker green for hover |
| `--color-surface-sidebar` | `#F4F7F4` | Sidebar background |
| `--color-surface` | `#FFFFFF` | Cards, inputs |
| `--color-page-bg` | `#F9FAF9` | Page background (faded image at top) |
| `--color-text-primary` | `#1A1A1A` | Headings, primary labels |
| `--color-text-secondary` | `#6B7280` | Subtitles, hints, breadcrumbs |
| `--color-text-on-green` | `#FFFFFF` | Text on green surfaces |
| `--color-border` | `#E5E7EB` | 1px borders/dividers |
| `--color-status-draft-bg` | `#FDF3E7` | Draft badge background |
| `--color-status-draft-text` | `#B45309` | Draft badge text |
| `--color-status-draft-dot` | `#D97706` | Draft badge dot |
| `--color-badge-neutral` | `#E5E7EB` | Count pills (0/3) |
| `--color-focus-ring` | `rgba(31,122,70,0.2)` | Input focus ring (INFERRED) |
| `--color-error` | `#DC2626` | Error border/text (INFERRED) |
| `--color-success` | `#16A34A` | Success (INFERRED) |
| `--color-warning` | `#D97706` | Warning (INFERRED) |

### Typography
- Headings font: serif (Playfair Display / Merriweather / Georgia) — academic feel.
- Body font: sans (Inter / Roboto / System UI).
- Scale: H1 serif 32px bold #1A1A1A; H2 (banner name) serif 28px bold white; H3 (card heading) sans 18px semibold #1A1A1A; Body/labels sans 14px regular/medium; Small/captions sans 12px.

### Spacing & layout
- Base unit: 4px (scale: 4, 8, 16, 24, 32, 48).
- Sidebar: fixed ~280px (light green surface).
- Main content: fluid, max-width ~1200px, centered, padded.
- Below banner: 2-column (left ~65%, right ~35%, gap ~24px).

### Shapes & effects
- Card radius: 12px; Input/Button radius: 8px; Pills/badges: 9999px.
- Card shadow: `0 4px 6px -1px rgba(0,0,0,0.05)`.

## 2. Component library (shared kit)

- **Primary button**: solid #1F7A46, white text, 8px radius, padding 12px 24px, weight 600.
- **Secondary/outline button**: white bg, 1px #E5E7EB border, #1A1A1A text, 8px radius, padding 12px 24px. Icon left.
- **Icon button**: no bg; hover adds light gray bg.
- **Inputs**: white bg, 1px #D1D5DB border, 8px radius, padding 10px 14px, text #1A1A1A; placeholder #9CA3AF; disabled bg #F3F4F6 text #6B7280; helper text 12px #6B7280 below.
- **Cards**: white bg, 12px radius, subtle shadow, padding 24/32px; inner dividers 1px #E5E7EB with 16px vertical spacing.
- **Badges/pills**: sidebar counts (light gray, pill, right-aligned); Draft status (warm yellow/orange pill with dot).
- **Sidebar nav**: muted gray #4B5563 links with left icon; hover darkens + light bg; active #1F7A46 + heavier weight.
- **Top bar**: breadcrumb left, ID pill + user dropdown right.
- **Hero header**: faded background image, eyebrow (uppercase, wide tracking, gray), H1, subtitle.
- **Green identity banner**: full-width, #1F7A46, 12px radius, avatar left, name/email/role middle, crest watermark + DRAFT badge right.
- **Two-column layout**: left 65% (Contact details card), right 35% (Application card).
- **Empty/loading/error states**: one pattern app-wide (INFERRED — to be defined in Phase 2).

## 3. Screen composition (adapted to DMS)

App shell: left sidebar (280px, light green) + main content (max 1200px). Top bar with breadcrumb + ID pill + user dropdown. Hero header with faded bg image. Green identity banner (user). Two-column below banner.

For DMS, nav links map to: Overview, Documents, History, Snapshots, Trash (per source). Admin gets extra: Accounts, Connectors, Scans, Audit, Workspaces, Legal holds.

## 4. UX rules (app-wide)

- Primary vs secondary vs destructive: solid green / outline gray / outline red.
- Status: color + icon + text (status-badge already does this).
- Forms: label above input, helper below, error = red border + red helper text.
- Tables: overflow scroll, zebra striping (gray/white per README), actions cell right-aligned.
- Nav: current location highlighted (green + weight).
- Destructive actions: confirm via existing jQuery UI/Bootstrap modal (do NOT replace).
- Empty/loading/error: one pattern everywhere.

## 5. Inferred / unspecified (needs sign-off)

- Responsive: stack 2-col → 1-col below 1024px; sidebar → hamburger below 768px. [INFERRED — needs sign-off]
- Dark mode: not specified → NOT included. [INFERRED — needs sign-off]
- Motion: subtle transitions (150–200ms ease) for hover/focus. [INFERRED — needs sign-off]
- Focus ring: `0 0 0 3px rgba(31,122,70,0.2)` on inputs. [INFERRED — needs sign-off]
- Error copy: keep existing Laravel validation messages. [INFERRED]
- Long-content truncation: ellipsis + tooltip. [INFERRED]
- RTL: not supported. [INFERRED]
- Keyboard nav order: tab order follows visual order. [INFERRED]

## 6. jQuery UI compatibility (top priority)

These widgets MUST survive the redesign (DOM contracts preserved):

| Widget | Selector | Contract |
|---|---|---|
| Draggable | `.document-row[data-can-move="true"]` | keep class + data attr |
| Droppable | `.folder-card[data-folder-id]` | keep class + data attr |
| Datepicker | `.datepicker` | keep class |
| Autocomplete | `#share-search` | keep id |
| Tabs | `#document-tabs` | keep `ul>li>a` + sibling panels |
| Resizable | `.preview-panel` | keep class |

Restyle jQuery UI widgets via CSS only (`.ui-tabs`, `.ui-accordion`, `.ui-dialog`, `.ui-datepicker`, `.ui-autocomplete`) — do NOT touch init code or DOM contracts. No new widget markup restructuring.

## GATE 2
Spec complete. Please review and reply "spec approved" (with any corrections) before Phase 2 implementation.