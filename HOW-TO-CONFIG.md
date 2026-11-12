# How to configure Form Canvas

Form Canvas reads two data sources (sections, fields), shows a palette and calls your actions. Before each action it
writes the **drop context** into attributes of the data view object it sits in; your microflow reads them.

## 1. The designer context

A non-persistent entity on the designer page, e.g.:

| Attribute | Type | Written before | Means |
|---|---|---|---|
| SectionKey | String | On add, On move, On section move, On section rules | the target section's key |
| Position | Integer / Long | On add, On move, On section move, On add section | 0-based place in the target (section, table or section list), counted without the moved item |
| TableKey | String | On add, On move | the table the field goes into ('' = the section itself) |
| TypeKey | String | On add | the palette item's type key |
| Width | String or Enum | On resize | the new width key |
| FieldKey | String | every field action | the field concerned |
| SelectedKey | String | On select (two-way) | the selected field; set it yourself to select from your own UI |
| SelectedSectionKey | String | On section select (two-way) | the selected section (highlighted, shows its tools) |
| ColumnKey | String | On access cell, On preview as | the stage / role column; for Preview as, '' = everyone |
| Level | String | On access cell | the picked level, one of the Level keys |

Put the widget **inside a data view** of this object. It also works inside a snippet (no action variables are used).

## 2. Properties

### Sections
| Property | Kind | Notes |
|---|---|---|
| Sections | data source | any source: database, association, microflow |
| Section key | expression (String) | unique; fields name their section by it |
| Title / Introduction | text template | |
| Order | expression (Integer) | empty: the data source's order |
| Starts a step | expression (Boolean) | the first section always starts step 1 |
| Step title | text template | empty: the section title |
| Step description | text template | a short line under the step's name in the step bar |
| Step view | All steps / One step at a time | |
| Step bar | Horizontal / Vertical / Hidden | vertical: a column of steps beside the form (above it when narrow); many steps scroll in the bar |
| Step descriptions / Step numbers | booleans | show the step description / number in each chip |
| Title in another language | text template | shown muted under the title (e.g. Arabic under English) |
| Shown by a rule / Read-only / Rule summary | expressions / text template | section markers, like a field's |
| Access | text template | the section's levels in your words |
| Problem | text template | red message and outline on the section; counted in the toolbar's problems button |
| Section rule count | expression (Integer) | rule chip on the section header |

### Fields
| Property | Kind | Notes |
|---|---|---|
| Fields | data source | every field, table columns included |
| Field key / Section key | expression (String) | Section key = the key of the field's section |
| Table key | expression (String) | for a table column: its table's field key |
| Label | text template | per language, e.g. English template `{1}` = LabelEN, Arabic template `{2}` = LabelAR |
| Type key | expression (String) | free key; the skeleton is chosen by name (see Type mapping) |
| Order / Width / Required | expressions | Width: one of the Width keys; empty = Full |
| Help text / Badge | text templates | badges separated by " · " |
| Options | text template | choice options separated by " · " (enum captions, lookup values, a fixed list) |
| Options come from | text template | e.g. "Lookup: Leave types" |
| Rule count / Rule summary | expression / text template | your rules in your words, separated by " · " |
| Depends on | expression (String) | comma-separated field keys this field depends on |
| Shown by a rule / Read-only | expressions (Boolean) | markers |
| Access | text template | e.g. "Edit: HR · See: Manager" |
| Label in another language | text template | shown muted next to the label |
| Required by a rule / Read-only by a rule | expressions (Boolean) | a ruled asterisk / lock, distinct from always required / read-only |
| Formula / Starting value / Limits | text templates | facts under the skeleton, in your words ("= Working days between First day, Last day") |
| Flags | expression (String) | flag keys separated by commas; Flag chips map each key to an icon, caption and tone |
| Problem | text template | your validation's message for the field: red text and outline; the toolbar's "n problems" walks them |
| Change | expression (String) | added / changed / removed, for comparing versions (coloured edge; removed is struck through) |
| Custom content | widgets | your own content instead of the skeleton |

### Logic and access
| Property | Kind | Notes |
|---|---|---|
| Form rules | data source + text template + kind | rules on the whole form (variant, priority, route, stop): the side panel's Form logic tab; On form rule select / On add form rule |
| Columns | data source + key + title | workflow stages or roles: the Access view's columns and the Preview as choices |
| Cells | data source + row key + column key + level | one object per field or section and column that has a level; none = Editable |
| Level keys | strings | your keys for Hidden / Shown / Editable / Required |
| On access cell / On preview as | actions | written context: FieldKey or SectionKey, ColumnKey, Level |

### Inspector and preview
| Property | Kind | Notes |
|---|---|---|
| Inspector | widgets | your own editor of the selected field / section, docked in the side panel (Properties tab) |
| Preview | widgets | your own live rendering of the form (Preview tab); open by default on wide screens |
| Layout height | string | e.g. `calc(100vh - 210px)`: the palette, canvas and side panel scroll on their own |

### Widths, type mapping, palette
- **Width keys** — your keys for Full, Half, Third and Cards (Cards: tables only; empty = not offered). Allow resize;
  Stack below (px) stacks every field when the canvas is narrower (a phone).
- **Type mapping** — optional: map a type key to one of 19 skeletons. Unlisted keys are matched by name (Text,
  LongText, Integer, Decimal, Date, DateTime, YesNo, SingleSelect, MultiSelect, Person, OrgUnit, Attachment,
  Reference, Computed, Grid, RichText, Rating, … and common synonyms).
- **Palette** — Static (type key, caption, icon, group per item) or Data source (type key, caption, icon class, group
  expressions); position beside or above the canvas, or hidden.

### Actions
On add (context) · On move · On resize · On select · On delete · On duplicate · On rules · On access (fields) ·
On section move · On section select · On section rules (sections) · On add section (context).
An action you leave empty switches the gesture off (no palette without On add, no drag without On move, …).

### General
Live preview (widgets) and its position; Read-only (expression — e.g. not a draft); Direction (Auto / LTR / RTL);
Colour scheme; Density; Canvas max width; Height; Rail below (px, default 1280: under it the palette folds to a rail
of icons) and Narrow below (px, default 960: under it the palette moves above the canvas and the side panel below it;
both are measured inside the designer's frame, the widget's width less 40 px, not on the screen); Animations; interface and accessibility texts (all translatable).

Preview as: the widget shows the role or stage in **Previewed column key** when you set it (written only before On
preview as); without it, Preview as reads **Target column key**, which On access cell writes too, so the Preview as
choice follows the last access cell clicked.

## 3. Microflow patterns

**On move** — `ACT_Field_Move($Field, $Context)`:
1. retrieve the target section by `$Context/SectionKey` (and the table by `$Context/TableKey`, may be empty);
2. retrieve the target's other fields (`[id != $Field]`, sorted);
3. renumber them 10 apart, leaving the gap at `$Context/Position`; set the field's order, section and table;
4. commit (refresh in client) — the canvas reloads and shows the server's state.

**On add** — `ACT_Field_Add($Context)`: create the field of `$Context/TypeKey` in the target, place it as above, then
open your field dialog (or not).

**On resize** — store `$Context/Width` on the field.

**Refusing** — just don't change anything (e.g. a published version): the canvas puts the field back after the reload
or within 4 seconds.

The demo module (`FormCanvasDemo.mpk`) contains all of these, plus versions, typed rules, roles and lookups.

## 4. Mapping an existing form model (an example)

| Form Canvas | An app with its own form catalogue |
|---|---|
| Sections | `FormSection` of the version being edited |
| Section key / Title / Order / Starts a step | `SectionKey` / `TitleEN`·`TitleAR` (per-language template) / `SortOrder` / `StartsStep` |
| Fields | `FormField` of the version (a view entity or DS microflow that also carries the width from `FieldLayout`) |
| Field key / Section key / Table key | `FieldUid` / the section's `SectionKey` / the grid field's `FieldUid` |
| Type key / Label / Order / Required | `getKey(FieldType)` / `LabelEN`·`LabelAR` / `SortOrder` / `IsRequired` |
| Width | `getKey(FieldLayout.Width)` (no row = Full); keys Full / Half / Third / Cards |
| Options / Options come from | the lookup type's values / "Lookup: …" from `FieldSpec.SourceCode` |
| Rule count / summary / depends on | counts and words from `FieldRule` + `RuleCondition`; source fields' keys; `FieldSpec_ParentField` |
| Access | words from the field's privacy and role settings |
| Read-only | the version is not a draft, or the user may not keep the catalogue |
| Actions | the app's governed `ACT_` microflows (they check, write, renumber and refuse) |

## 5. Tips

- Place the canvas in a container that can shrink (`min-width: 0`), e.g. a layout grid column.
- Keys may contain any character (`CODE|v3|S|S2` works).
- A field whose section key matches no section appears in the *Not placed* tray — never lost.
