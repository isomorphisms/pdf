# Figure captions

Migrated from `isomorphisms/harvest-images-from-pdf`, branch `figure-caption-association`.

## Decision

The normal harvested output should keep a figure together with its caption when a caption can be identified. The image bytes alone are not the complete useful unit.

This does not mean that caption parsing belongs inside image-stream decoding. A PDF image XObject normally contains image data and image-related dictionary entries; the visible caption is usually separate page text positioned near the image. Therefore keep these stages separate:

1. identify and decode/extract image streams;
2. interpret page content enough to learn where each image is painted;
3. obtain positioned page text fragments;
4. associate likely caption text with each placed image;
5. emit the image plus associated caption metadata.

Keeping stage 1 independent means raw image extraction still works even when page layout or caption recognition fails.

## First association rule

A deliberately conservative first pass:

A text fragment is a caption candidate only when all of these hold:

- it is on the same page as the image;
- its page rectangle overlaps the image horizontally;
- it begins with an explicit numbered `Figure`, `FIGURE`, `Fig.`, or `FIG.` label;
- it lies entirely below or entirely above the image;
- the vertical gap is no greater than a caller-supplied maximum.

Selection is deterministic:

1. choose the nearest qualifying caption below the image;
2. only if there is none, choose the nearest qualifying caption above it;
3. if no candidate qualifies, preserve the image as `image_without_identified_caption` rather than inventing a caption.

This intentionally does not yet infer captions from typography alone, merge several text fragments into a multiline caption, understand tables, or resolve one caption spanning several subfigures.

## Layout representation

The first layout slice used integer coordinates. That was not a claim that PDF geometry is integral; it kept the association rule independent of a premature floating-point representation choice. A future page-content interpreter can normalize PDF user-space numbers/transforms into whatever layout unit this layer accepts.

A page rectangle `left bottom right top` follows ordinary PDF page coordinates: y increases upward. Thus a caption below an image has `caption_top <= image_bottom`.

## Acceptance cases

Check at least:

- explicit numbered label recognition;
- unlabeled nearby body text is ignored;
- the nearest qualifying caption below wins;
- below is preferred to above;
- above is accepted as a fallback;
- wrong-page and horizontally disjoint text is rejected;
- the maximum vertical gap is enforced.

## Next integration work

The byte parser does not itself provide the two layout inputs this stage needs: image placements and positioned text fragments. After cross-reference/object parsing and `/Subtype /Image` extraction, the page-content interpreter should record image paint operations and text geometry, then pass those values to the caption-association stage.
