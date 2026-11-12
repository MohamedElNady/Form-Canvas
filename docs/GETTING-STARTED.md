# Getting started: put Form Canvas in your app

For Mendix developers. Form Canvas is the canvas only: it draws your form model and tells your microflows what the
designer did. Your app keeps the data, the rules, the checks and the versions. Start small; every part after step 3 is
optional.

## 1. The smallest working designer (15 minutes)

You need two entities you probably already have, and one helper:

| Entity | Needs | Example |
|---|---|---|
| Section | a key, a title, an order | `Section (SectionKey, Title, SortOrder)` |
| Field | a key, its section's key, a label, a type key, an order, a width | `Field (FieldKey, Label, TypeKey, SortOrder, Width)` + `Field_Section` |
| Designer context (non-persistent) | string attributes the canvas writes before each action | `DesignerContext (SectionKey, Position, TableKey, TypeKey, Width, FieldKey, SelectedKey)` |

1. Page with a data view on a `DesignerContext` (a microflow data source that creates one).
2. Put **Form Canvas** in it. Set **Sections** and **Fields** (any data source) and their key / title / label / order /
   type / width expressions. Leave everything else empty.
3. Set the **Drop context** attributes (Target section key, Target position, Target table key, Target type key, Target width, Field key) and the actions:

| Action | Your microflow does |
|---|---|
| On add (palette drop) | create a field of `TypeKey` in section `SectionKey` at `Position` (in table `TableKey` if set) |
| On move | put the field (the action's object) in `SectionKey` / `TableKey` at `Position`; renumber the others |
| On resize | set the field's width to `Width` |

That is a working drag-and-drop designer. Studio Pro's consistency checks tell you when an action needs a context
attribute you did not set.

## 2. Add what you need, one group at a time

| You want | Set | Your app does |
|---|---|---|
| select and edit a field | Selected key + On select, **Inspector** (your own widgets) | show the selected field's editor in the docked panel |
| delete / duplicate | On delete, On duplicate | remove / copy the field |
| sections that move, delete, duplicate, start a step | On section move / delete / duplicate / step | the same on sections |
| a wizard | Starts a step, Step title / description, Step view, Step bar (expressions: one setting per form) | store which sections start a step and the form's step settings |
| two languages on the canvas | Label in another language, Title in another language | pick the texts of the edit language and the one beside it |
| rules shown on the canvas | Rule count / summary, Depends on, Shown by a rule, Required / read-only by a rule | count and describe *your* rule rows in words |
| form rules (route, priority, variant, stop) | Form rules data source + On form rule select / add | list rules without a field or section target |
| access per stage or role | Columns + Cells + Level keys + On access cell | store one level per field (or section) and column |
| Preview as | On preview as (writes the column key) | render your preview for that stage or role |
| problems on fields | Problem (field / section) | write your validation's message per field; the toolbar walks them |
| version compare | Change (added / changed / removed) | compare the draft with the published version |
| a live preview | **Preview** (your own widgets) | render the form from the same data |

## 3. Rules stay in your app

The canvas never evaluates anything and never stores expression text. Keep your rules as rows (effect, target,
conditions with field / operator / value) and give the canvas their words. The demo's `FormCanvasDemo` module shows a
complete set: typed rules with AND / OR groups, access per stage, versions, checks, texts per language and a preview
engine that applies the rules. Copy what you need from it.

## 4. Where to look next

- [HOW-TO-CONFIG.md](../HOW-TO-CONFIG.md): every property, the context attributes, microflow patterns, mapping an existing form model.
- [BUILD-A-FORM.md](BUILD-A-FORM.md): how a form designer uses the demo, step by step.
- [HOW-TO-TEST.md](../HOW-TO-TEST.md): what to check before you ship.
- The demo app: **Form Canvas → Forms** (a list of forms with New form), the designer, **Form settings**.
