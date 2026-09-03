# Real World Haskell parser lessons for this PDF work

Migrated from `isomorphisms/harvest-images-from-pdf`, branch `rwh-binary-parser`.

The useful reference is **Real World Haskell, Chapter 10: “Code case study: parsing a binary data format.”** Chapter 16 introduces Parsec, but the smaller Chapter 10 parser is the better design reference for this PDF work.

This is not a Haskell implementation. The chapter supplies a parser-design lesson to translate into the project language.

The chapter starts with nested case expressions over a binary format, then factors repeated plumbing into a parser whose shape is essentially:

```text
state → Either error (value, state)
```

That state/error shape is the useful model.

## What carries over directly

- Parser state contains unread bytes and the current byte offset.
- Failure is a value, not an exception.
- Tiny parsers are composed into larger parsers.
- The caller does not manually thread the remaining input or offset through every operation.
- Errors report the byte offset. This matters especially in PDF because cross-reference data is explicitly offset-based.

## Typed improvement

A fixed-length byte read should make a short input a parse failure rather than a shorter successful value. The parser core should remain pure. File I/O should read only the slices needed at known PDF offsets and pass those slices to the pure parser while preserving absolute file offsets for useful errors.

## PDF-specific consequence

Do not search blindly for strings such as `/Subtype /Image`, `stream`, or `endstream` across the whole file. Stream bodies are arbitrary binary data and may contain those byte sequences by accident. The parser should learn enough PDF object structure to know when bytes are syntax and when they are opaque stream payload.

The next useful layer is:

1. parse the PDF header;
2. locate `startxref` and parse either a classic cross-reference table or a cross-reference stream;
3. seek to indirect objects by byte offset;
4. parse dictionaries far enough to identify `/Subtype /Image` and `/Length`;
5. consume exactly the declared stream length;
6. hand the extracted stream plus `/Filter`, `/Width`, `/Height`, `/ColorSpace`, and `/BitsPerComponent` metadata to the image-decoding/output layer.

The useful lesson is to make state, failure, and byte consumption explicit once, then describe PDF syntax by composing small parsers.
