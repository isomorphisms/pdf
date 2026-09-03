# AndroidX PDF engine gate

Migrated from `isomorphisms/utilities-android-phone-user` draft PR #33 / branch `pdf-filler-androidx-spike`.

This is the smallest executable compatibility spike for the five questions in the Android PDF filler plan. It is a gate, not the editor.

```text
OPEN      AndroidX reports its first visible page bitmap for a generated PDF
FORM      a generated AcroForm text field is enumerated, filled, saved, and read after reopen
SAVE      another installed PDF viewer visibly shows the saved AcroForm value
FLAT_TEXT another installed PDF viewer visibly shows typed text stored as glyph outlines
CHECK     another installed PDF viewer visibly shows the saved X mark
```

The last three gates do not become `PASS` merely because AndroidX can reopen its own output. The spike may record that internal check as evidence but leaves the gate at `SKIP` until the operator opens the result in another viewer and records what was actually visible.

`FLAT_TEXT` is deliberately narrow. The tested AndroidX beta did not expose a native free-text annotation through the chosen path. The spike converted the typed marker into glyph outlines and stored those paths in a stamp annotation. A pass proves a persistent visible mark, not searchable text or a text object that can later be edited character-by-character.

## Device run

1. Install the debug APK on the target phone.
2. Run **FORM**. The app creates its own tiny AcroForm, fills the marker, saves it, and reopens it.
3. Choose **View form result**, use a different PDF viewer, return, and record `SAVE` as `PASS` or `FAIL`.
4. Run **FLAT_TEXT + CHECK**. The app creates a flat PDF and writes the marks.
5. Choose **View flat result**, inspect it in a different viewer, then record `FLAT_TEXT` and `CHECK` separately.
6. Export the JSON receipt and commit it under `receipts/` without editing its outcomes.

On devices where the editable fragment is unavailable, `OPEN` may still be tested through a read-only viewer while editing gates remain `SKIP`. An unavailable operation was not run and is not truthfully a failed PDF operation.

## Decision rule

- Keep AndroidX only if the important target phones can run the editable path and exported receipts show all five gates passing.
- Treat `FORM=PASS` with `SAVE=SKIP` as incomplete: the same implementation read its own output, but no independent implementation confirmed it.
- Treat `FLAT_TEXT=PASS` only as acceptance of the stated persisted glyph-outline representation. It does not establish searchable or character-editable PDF text.
- An unavailable editable path on an important phone is evidence for evaluating a bundled PDFium fallback, even though the unavailable editing gates correctly say `SKIP` rather than `FAIL`.

## Receipt contract

Every exported receipt should contain:

- Android release;
- API level;
- relevant SDK extension level;
- manufacturer and model;
- ABIs;
- exact PDF-engine/library version;
- timestamp;
- one `PASS`, `FAIL`, or `SKIP` record for each gate;
- a reason and concrete evidence field for every gate.

`PASS` never means more than its stated evidence.

## Host/device boundary

A host build can prove things such as source consistency, dependency resolution, compilation, packaging, and host-side checks. It cannot prove that the APK ran on the target phone, that Android supplied the required runtime PDF capability, that a saved PDF reopened correctly on that phone, or that an independent viewer displayed the persisted result.

Therefore a checked-in not-run receipt should remain `SKIP` for device-only gates until real phone evidence exists. Do not infer device verification from a green host CI build.
