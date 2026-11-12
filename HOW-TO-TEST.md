# How to test Form Canvas

This guide shows two ways to try Form Canvas, from the quickest to the most complete, and the checks to run
before you ship a designer built on it.

## Contents

1. [Try the online demo](#1-try-the-online-demo)
2. [Import the demo module into your app](#2-import-the-demo-module-into-your-app)
3. [Manual test matrix](#3-manual-test-matrix)
4. [Troubleshooting](#4-troubleshooting)

## 1. Try the online demo

Open <https://aiwidgets-sandbox.mxapps.io/p/home>, then **Form Canvas → Forms** and **Open the designer** on
**Leave request**. Leave request is a finished example of every feature (steps, dependent lists, rules, access per
stage and role, three languages, three versions). **Reset demo data** brings it back to its starting state.

## 2. Import the demo module into your app

`FormCanvasDemo.mpk` (a release asset) is the demo as a Mendix module, with the widget package inside it.

1. Studio Pro **10.24.10 or later** (module packages import into the same Studio Pro version or a newer one; the
   widget itself works from Mendix 10.6).
2. **File → Import module package…** and pick `FormCanvasDemo.mpk`. This adds the `FormCanvasDemo` module,
   `widgets/elnady.FormCanvas.mpk` and the module's styles (`themesource/formcanvasdemo`).
3. Add the pages to your navigation: `FormCanvasDemo.Forms_Web` (the forms list), `FormCanvasDemo.FormDesigner_Web`
   and `FormCanvasDemo.Settings_Web`.
4. The module has no module roles. Run it in an app with security off, or add a module role and give it access to the
   module's pages, microflows and entities.
5. Optional: add Arabic (`ar_EG`) as an app language to see the right-to-left designer; the module carries its
   Arabic texts.
6. Run the app and open the forms list. The demo seeds itself on first use.

The module uses only `Atlas_Core` layouts and `System.Language` from outside itself. Checked on 4 Nov 2026: imported
with `mx module-import` into a new blank Mendix 10.24.10 app, `mx check` reports 0 errors.

## 3. Manual test matrix

Done for 1.0.0 on Mendix 10.24.10 and 11.15.0 with the demo; on 10.6.11 the model check and the drop-context write.

| Area | Steps | Expect |
|---|---|---|
| Add | drag a palette item into a section, a table, an empty section; focus a palette item, Space, arrows, Space | field created at that place and selected in the inspector |
| Move | drag within a row, to another row, another section, into / out of a table, onto a step chip | rows reflow live at real widths; the server's order after the reload matches |
| Keyboard | Tab to a field, Space, ←/→/↑/↓, Space; Escape | moves as the arrows point; announcements; Escape puts it back; focus kept |
| Shortcuts | on a focused field: Enter, D, R, A, Delete, Ctrl+←/→ | selects, duplicates, opens rules, opens access, deletes, resizes |
| Resize | pull the edge (incl. a field starting mid-row), width chip, Ctrl+←/→ | snaps to Third / Half / Full; Cards only on tables |
| Sections | drag the handle, Space + ↑/↓; Starts a step; duplicate; delete | order and steps saved |
| Rules | field → **Logic** → Add a rule; a rule chip; **Form logic** tab | chip count and words update; the selected field's sources and dependants are highlighted |
| Access | **Access** above the canvas → click a cell | Hidden / Shown / Editable / Required per stage or role; the field's access chip updates |
| Preview as | **Preview as** → a stage or role | the preview shows the form as that stage or role sees it |
| Options | field → **Options**: its own list, a lookup list, a parent option; **Form settings → Lookup lists** | the skeleton shows the options and where they come from |
| Languages | **Form** → **Languages**: Edit in / Show beside it | labels in the edit language, the other one muted beside it; *Not yet in …* chips |
| Versions | **Check**, **Publish**; **Version** tab → **Open** a published version | problems on their fields; a new draft; the published version is read-only (no palette, no drag) |
| RTL | switch to Arabic with the language selector (home page) | mirrored; ← moves forward; edge resize from the left |
| Dark | add `dark` to `<html>` | dark tokens, readable markers |
| Narrow | 1100 px window; phone-width toggle | palette rail / above, side panel below; fields stack |

## 4. Troubleshooting

| Symptom | Cause and fix |
|---|---|
| CE0463 *"The definition of this widget has changed"* after an mxcli write | run *Update widget* (or `mxcli widget sync --widget elnady.formcanvas.FormCanvas`) |
| Studio Pro names a missing *Target …* attribute | the action needs that drop-context attribute: select it in the **Drop context** group |
| No palette, or fields do not drag | On add / On move is empty: an empty action switches its gesture off |
| A dropped field jumps back | your microflow did not change anything (by design for a refused change), or it did not commit with *refresh in client* |
| A field is in the *Not placed* tray | its Section key matches no section key |
| The side panel goes below the canvas next to your app's own navigation rail | lower **Narrow below** (e.g. 760) and **Rail below** |
| `mx update-widgets` turned an MPR v2 app into one `.mpr` file | use Studio Pro's *Update widget* or `mxcli widget sync --widget …` on MPR v2 apps instead |
