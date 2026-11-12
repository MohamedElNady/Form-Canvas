# Changelog

All notable changes to this widget are documented here (format: Keep a Changelog, versions: SemVer).

## [1.0.0] - unreleased

### Added
- Form designer canvas: sections and fields from two data sources at their real widths (Full / Half / Third, Cards
  for tables) on a 6-track grid, tables with their columns, steps (all steps or one at a time), Not placed tray.
- Palette (static list or data source, groups, search from 13 items) with drag to the canvas and keyboard add.
- Move fields within and across sections and into / out of tables, reorder sections, drop on a step chip.
- Resize by the field's edge, the width chip, or Ctrl+←/→.
- Keyboard drag with translatable announcements; Enter selects, Delete and letter shortcuts.
- Drop context written to the data view object before every action (Mendix 10.6+, works in snippets).
- Rule chips with the app's own rule words, dependency highlight, shown-by-a-rule / read-only / access markers,
  On rules / On access / On section rules actions.
- Choice options and their source drawn in the skeletons.
- Read-only mode, live preview slot (beside, below, behind a button), phone-width toggle, responsive layout.
- Right-to-left, dark mode tokens, English and Arabic default texts, Studio Pro checks and previews.
- Logic and access without evaluating anything: required / read-only by a rule next to always, a "depends on" chip,
  a Form logic panel (form rules), an Access view (stages or roles × sections and fields, four levels) and
  Preview as.
- Docked side panel: the app's inspector, its live preview and the form logic, with three columns from 1280 px and a
  palette rail from 960 px; Layout height for independent scrolling.
- Per-field and per-section problems with a toolbar button that walks them; Change marks for comparing versions.
- Labels and titles in a second language, flag chips, formula / starting value / limits facts.
- Section tools: delete, duplicate, "starts a step" toggle, selected-section highlight.
- Step bar: horizontal, vertical or hidden, with optional step descriptions and numbers.
- Studio Pro toolbox icon and tile (light and dark), help link.
- **Rail below** and **Narrow below** (px, defaults 1280 and 960): where the palette folds to a rail and
  where the layout goes to one column, measured inside the designer's frame (the widget less 40 px). An app with its
  own navigation rail can keep the side panel beside the canvas (for example Narrow below 760). Studio Pro warns when
  Narrow below is not lower.
- **Previewed column key** (optional attribute): Preview as reads it and On preview as writes it (and Target column
  key). An access cell writes only Target column key, so it no longer moves the Preview as choice. Without
  the attribute nothing changes.
- Arabic (ar_SA, ar_EG, ar_AE) drag instructions that read the R / A / D keys too.

### Changed
- Shortcuts: R (rules), A (access), D (duplicate) on the focused field, so no browser shortcut is taken over.
- The field's tools are a row of their own on the selected field: they never cover a label or the field above.
