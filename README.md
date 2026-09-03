# pdf

Single home for PDF-related tools, experiments, notes, and acceptance work.

This repository owns PDF concerns regardless of the user-facing utility that needs them, including:

- filling AcroForm PDFs;
- adding visible text or marks to flat/scanned PDFs;
- saving and independently reopening edited PDFs;
- PDF rendering and compatibility experiments;
- PDF generation for forms, resumes, and other documents;
- device receipts and engine acceptance tests;
- engine/library comparisons and fallback notes.

PDF-specific work should not continue as long-lived branches in unrelated application repositories. Those repositories may consume a PDF capability, but PDF implementation notes and engine experiments belong here.

## Migrated Android form-filler work

The Android PDF-filler planning and AndroidX compatibility-gate notes were moved here from `isomorphisms/utilities-android-phone-user` draft PRs #32 and #33.

- [`notes/android-pdf-filler.md`](notes/android-pdf-filler.md)
- [`notes/androidx-pdf-engine-gate.md`](notes/androidx-pdf-engine-gate.md)

The acceptance rule remains strict: a host build is build evidence only. It is not target-phone evidence and must not turn device-only receipt fields into PASS.
