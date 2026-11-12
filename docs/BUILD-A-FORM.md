# Build a form, step by step

For people who design forms in the demo app (or in an app built the same way). No Mendix knowledge needed.
Ten minutes from an empty form to a wizard with dependent lists, rules and access per stage.

## The screen in one look

| Part | Where | What it is for |
|---|---|---|
| **Fields** palette | left | the field types; drag one onto the form, or focus it and press Enter |
| **Canvas** | middle | your form: sections, fields at their real widths, the step bar |
| **Side panel** | right | **Properties** (edit what you clicked), **Preview** (fill the form in), **Form logic** (rules on the whole form) |
| **Bar** | top | the form's name and version, **Check**, **Publish**, **Add a step**, **All forms** |

Click a field or a section: its settings open on the right. Click empty space, or **Form**, to get back to the form's own
settings.

## 1. Create the form

1. Menu **Form Canvas → Forms**, then **New form**.
2. Give it a name. Choose the layout:
   - **Steps (a wizard)**: say how many steps to start with (you can add more later), the step bar (**horizontal**
     above the form or **vertical** beside it), and whether the person sees **all steps on one page** or **one step at
     a time**. Step descriptions and numbers can be switched off.
   - **No steps**: everything on one page.
3. **Create and open**. The form starts with one empty section per step.

You can change all of this later: the **Form** tab on the right (nothing selected), or **Settings** on the forms list.

## 2. Add fields

- Drag a type from the palette into a section (or into a table). Or focus a palette item and press **Enter**.
- Drag a field to move it; drag its edge, or use the width chip, for **full / half / a third**.
- A **table** (Table of rows) takes fields as its columns; set **Each line as a card** for long lines.
- Keyboard: Space picks a field up, arrows move it, Space drops, Escape cancels; Delete, **D** duplicates, **R** rules,
  **A** access.

## 3. Name it in every language

Click the field → **Field** tab. There is one box per language (English, العربية, Español in the demo): the label and
the help. Empty boxes show on the canvas as **Not yet in …**; a *required* language must be complete before you can
publish. **Languages** (form tab) chooses the language you edit in and the one shown beside it.

Each form language can be mapped to one of the app's own languages (**Form settings → Languages → App language**, the
Mendix code such as `en_US` or `ar_EG`). The designer opens in the form language of your session; an app that shows the
form to people does the same for them, and an empty text falls back to the base language.

## 4. Options, values, limits

| Tab | Use it for |
|---|---|
| **Options** | where a choice field's options come from: **its own list**, a **lookup list** (shared, see Form settings) or an **external list** (a code your app answers). Give each option a **parent** to make a *dependent list*: the options then follow another field's answer. People fields offer the people of the unit field you pick. Tick **Asks for details** on an option such as *Other*: picking it shows a "Please say which" box, and Next will not go on while it is empty. |
| **Values** | **computed** fields (sum, difference, days or working days between two dates, …, with decimals), the **starting value** (fixed, today +N, the requester, the requester's unit) and **limits** (smallest / largest, characters, file types and size, number of files or lines). |

### Dependent lookup lists

A lookup list can hang under another list: **Cities** under **Countries**, each city naming its country as its
parent (Form settings → Lookup lists, *Under*). On the field: **Options → A lookup list → Cities**, and **Options follow
the answer of → Destination country**. The person then sees only the cities of the country they picked; a value without
a parent (*Other city*, with **Asks for details**) is offered for every country.

When something else than a details box should appear, use a rule instead: on the other field, **Show when Leave type
is Other** (any field, any number of fields, with their own rules and access).

## 5. Add logic (rules)

Click the field → **Logic** → **Add a rule**. A rule is *what happens* while *its conditions hold*:

- **When**: pick a field, a comparison (is, is not, more than, at least, contains, is one of, is empty, …) and a value,
  or compare with another field. **And another condition** = all must hold; **Or another group** = any group is enough.
  **Negate** turns a condition around. For a table column choose *every line*, *the total*, *the number of lines*,
  *a repeated value*, …
- **Then**: show, hide, make required or optional, read-only or editable, set a value, or **stop with a message**
  (in every language).
- On a **section**, the same rules show, hide, lock or require the whole section.
- On the **whole form** (**Form logic** tab): set the variant, set the priority, route the request, or stop it.

The canvas marks what rules do: a rule chip with the count, **depends on …** chips, a ruled asterisk for *required by a
rule*, a lock for *read-only*.

## 6. Who sees what (access)

**Access** (above the canvas) shows the matrix: sections and fields down, stages and roles across. Click a cell:
**Hidden**, **Shown** (read-only), **Editable** (the default) or **Required** at that stage. The stages and roles
themselves are kept in **Form settings → Stages and roles**.

## 7. Try it as the person who fills it in

**Preview** (right): real inputs, the rules applied as you type, computed fields, dependent lists, steps with **Next**
and **Back** that check each step. **Preview as** (above the canvas) shows the form as one stage or role sees it.
**Phone width** checks the narrow layout.

## 8. Check and publish

- **Check** writes each problem on its field or section in red; the **n problems** button walks through them.
- **Publish** checks again, publishes the version and starts a new draft. Published versions are read-only; the
  **Version** tab lists them all, opens any of them, and marks what changed in the draft.

## Shared settings

**Form Canvas → Form settings** holds what all forms share: **lookup lists** (lists of values with optional parent
values), **languages** (base, required, right to left) and **stages and roles** (the access matrix's columns).

## The demo form

**Leave request** shows every feature: dependent leave types per unit, people of a unit, working days computed from two
dates, rules with AND / OR groups and a stop message, a table with a dependent list in each line, dependants as cards,
access per stage and role, three languages (Spanish only partly translated), three versions. **Reset demo data**
brings it back.
