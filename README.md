# pdf

Single home for PDF-related tools, experiments, notes, and acceptance work.

This repository owns PDF concerns regardless of the user-facing utility that needs them, including:

- filling AcroForm PDFs;
- adding visible text or marks to flat/scanned PDFs;
- saving and independently reopening edited PDFs;
- PDF rendering and compatibility experiments;
- PDF generation for forms, résumés, and other documents;
- device receipts and engine acceptance tests;
- PDF parsing and object modeling;
- image/figure extraction and caption association;
- real-PDF fixture and corpus policy;
- engine/library comparisons and fallback notes.

PDF-specific work should not continue as long-lived branches in unrelated repositories. Those repositories may consume a PDF capability, but PDF implementation notes, parser work, engine experiments, acceptance receipts, and document-generation work belong here.

## Migrated Android form-filler work

The Android PDF-filler planning and AndroidX compatibility-gate notes were moved here from `isomorphisms/utilities-android-phone-user` draft PRs #32 and #33. Both drafts were closed unmerged because the work was reorganized, not because the engine gate passed.

- [`notes/android-pdf-filler.md`](notes/android-pdf-filler.md)
- [`notes/androidx-pdf-engine-gate.md`](notes/androidx-pdf-engine-gate.md)

The acceptance rule remains strict: a host build is build evidence only. It is not target-phone evidence and must not turn device-only receipt fields into PASS.

## Migrated PDF harvester notes

Durable notes from the former `isomorphisms/harvest-images-from-pdf` branch set now live here:

- [`notes/harvest/extraction-policy.md`](notes/harvest/extraction-policy.md)
- [`notes/harvest/pdf-data-types.md`](notes/harvest/pdf-data-types.md)
- [`notes/harvest/pdf-type-source.md`](notes/harvest/pdf-type-source.md)
- [`notes/harvest/real-world-haskell.md`](notes/harvest/real-world-haskell.md)
- [`notes/harvest/figure-captions.md`](notes/harvest/figure-captions.md)
- [`notes/harvest/real-pdf-corpus-test-2026-08-23.md`](notes/harvest/real-pdf-corpus-test-2026-08-23.md)
- [`notes/harvest/real-pdf-fixtures.md`](notes/harvest/real-pdf-fixtures.md)

The old harvester repository is retained only as historical implementation material and now points here as its successor.
