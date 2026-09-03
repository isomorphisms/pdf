# Android PDF filler

Migrated from `isomorphisms/utilities-android-phone-user` draft PR #32 / branch `pdf-filler-plan`.

A deliberately small Android utility for the ordinary task: open a PDF on the phone, put information into it, and save a usable PDF.

The product goal is not a general PDF editor. It is a free, local, no-ads form filler that handles the PDFs people actually get sent.

## Product promise

Version 1 should do these things well:

1. Open a local PDF through Android's document picker.
2. Display it with normal pan/zoom.
3. If the PDF contains ordinary AcroForm fields, let the user fill text fields, checkboxes, radio buttons, combo boxes, and list boxes.
4. If the PDF is flat or scanned, let the user tap a location and place text there.
5. Let the user place an X/checkmark on a flat form.
6. Save the edited document as a new PDF through Android's document picker.
7. Work entirely on-device.

Explicitly not required for v1:

- full PDF editing
- page rearrangement
- OCR-driven automatic form reconstruction
- cloud storage accounts
- Adobe accounts
- digital certificate signatures
- XFA compatibility
- collaboration
- subscriptions
- ads
- analytics/tracking

A handwritten signature may be added if it falls out cheaply from the chosen library, but it is not a release blocker.

## First engine choice: AndroidX PDF

The initial plan chose AndroidX PDF as the first implementation spike rather than an irreversible engine decision.

Start with:

```text
androidx.pdf:pdf-viewer-fragment:1.0.0-beta01
androidx.pdf:pdf-ink:1.0.0-beta01
```

Use `EditablePdfViewerFragment` for the first executable version.

Reasons recorded in the original plan:

- it supplies the PDF viewer UI;
- it understands real PDF form widgets;
- it supports text fields, drop-downs, checkboxes, and radio buttons;
- it has an edit/apply/save workflow through `PdfWriteHandle`;
- it minimizes the amount of PDF-specific code we must own.

This route uses the ordinary Android/Kotlin/Gradle stack. For this utility, getting a correct PDF writer quickly matters more than avoiding DEX.

## Compatibility spike

Before building the complete UI, make one tiny executable that answers these questions on the actual target phones:

```text
OPEN      can we open and render a PDF?
FORM      can we enumerate/fill an AcroForm text field?
SAVE      does the saved file reopen with the value visible?
FLAT_TEXT can we place typed text on a non-form PDF and persist it?
CHECK     can we place a check/X and persist it?
```

Record each as PASS / FAIL / SKIP with Android version and SDK extension level.

This spike is the engine gate.

### If AndroidX editing works on the target devices

Keep AndroidX PDF for v1.

### If AndroidX editing is unavailable on too many target devices

Move to a bundled PDFium path rather than trying to reimplement PDF.

The original note identified MJ PDF as a useful reference implementation and PDFium as the underlying Chromium PDF engine. A direct PDFium/JNI path gives broader OS compatibility and more control, but costs substantially more integration work than AndroidX.

MuPDF was not the default fallback because its licensing would complicate this app.

## PDF cases

The app should classify the document internally into practical cases rather than pretending every blank is a form field.

### A. AcroForm

Use the existing field widgets.

Expected v1 support:

- text
- checkbox
- radio button
- combo/dropdown
- list

Save the field values and verify their visible appearances in at least two independent PDF viewers.

### B. Flat PDF

There is no field to fill.

Interaction:

```text
tap page
-> text cursor / small text box
-> type
-> drag if necessary
-> resize/font-size if necessary
-> save
```

Internally, store placement in page coordinates:

```text
page
x
y
width
height
text
font_size
alignment
```

Persist this using an engine-supported PDF object such as a free-text annotation, stamp annotation, or equivalent. Do not rely on a screen-only overlay that disappears when the PDF is opened elsewhere.

### C. Scanned PDF

Treat it exactly like a flat PDF for v1. The page image is just the background.

OCR is optional assistance later; it is not necessary for a person to tap where text belongs.

### D. XFA / unsupported interactive PDF

Detect it and say clearly that this form type is unsupported. Do not silently corrupt it.

## UX

Keep the first UI small.

```text
Open PDF

[page viewer]

Fill mode:
- automatic real form fields when present
- Text
- Check / X
- optional Signature

Undo
Save a copy
```

No library screen, document management system, login, tutorial carousel, AI, or cloud sync in v1.

The important user story is:

> Someone emails me a PDF form. I open it on my phone, fill the blanks, save it, and send it back without creating an account or fighting Adobe software.

## Privacy and permissions

Default policy:

- no `INTERNET` permission;
- no advertising SDK;
- no analytics SDK;
- no crash-reporting network SDK;
- no account;
- no storage-wide permission when the Storage Access Framework is sufficient;
- source repository public;
- privacy policy can truthfully state that documents are processed locally and are not collected by the app.

## Acceptance corpus

Keep small public/test PDFs in a test-fixtures directory where licensing permits, or generate fixtures during tests.

Minimum corpus:

1. one simple text-field AcroForm;
2. checkbox + radio form;
3. dropdown/list form;
4. multipage AcroForm;
5. flat form;
6. scanned/image-only form;
7. password-protected PDF;
8. XFA sample for correct refusal;
9. malformed-but-viewable PDF;
10. a PDF filled by the app and reopened in another viewer.

The crucial acceptance test is not merely that our own viewer displays the edit. The saved output must reopen correctly in another independent PDF implementation.

## Build sequence

### Milestone 0 — engine receipt

Build the five-test compatibility spike: OPEN / FORM / SAVE / FLAT_TEXT / CHECK.

Do this before polishing UI.

### Milestone 1 — useful APK

- document picker
- editable viewer
- real form filling
- save-as
- clear unsupported-file errors

At this point the app is already useful for genuine AcroForms.

### Milestone 2 — broadly useful flat-PDF editing

- tap-to-place typed text on flat/scanned PDFs
- move/resize text
- X/checkmark placement
- undo/remove placement

This matters because many forms that visually look fillable are not interactive PDFs.

### Milestone 3 — release hardening

- filename/save behavior
- rotation/process-death testing
- large PDF sanity check
- corrupted PDF failure behavior
- accessibility pass
- test on low-memory phone
- verify no network permission or trackers
- build release AAB
- store listing/privacy text/screenshots

### Later, not v1

- remembered signature
- OCR-assisted blank detection
- automatic field detection for flat forms
- flatten edits into page content
- encrypted PDF editing
- better XFA handling
- desktop/web ports

## Decision rule

Do not spend a week debating engines before running the spike.

```text
AndroidX passes target devices
    -> build v1 on AndroidX

AndroidX fails because SDK extensions exclude important devices
    -> evaluate MJ PDF/PDFium as bundled fallback

PDFium integration becomes larger than the app
    -> ship AndroidX-compatible v1 first, state minimum requirements clearly,
       then add the compatibility backend separately
```

The first engineering task is therefore not "build a PDF editor." It is:

> Produce an APK that opens one AcroForm and one flat PDF, makes one persistent edit to each, saves both, and proves the files reopen correctly elsewhere.

## References retained from the original note

- AndroidX PDF releases: https://developer.android.com/jetpack/androidx/releases/pdf
- EditablePdfViewerFragment: https://developer.android.com/reference/androidx/pdf/ink/EditablePdfViewerFragment
- Android platform PdfRenderer.Page form APIs: https://developer.android.com/reference/android/graphics/pdf/PdfRenderer.Page
- Android FreeTextAnnotation: https://developer.android.com/reference/android/graphics/pdf/component/FreeTextAnnotation
- PDFium: https://github.com/chromium/pdfium
- MJ PDF: https://github.com/mudlej/mj_pdf

Store-policy dates and version requirements in the old note are historical planning inputs, not permanent acceptance facts; re-check them when release work resumes.
