# Real PDF fixture corpus

Migrated from `isomorphisms/harvest-images-from-pdf`, branch `fixtures-real-pdfs`.

These are real PDFs selected from links previously supplied in project discussions. The point is to exercise parser and image/caption harvesting against ordinary published PDFs, not only synthetic byte strings.

Source review date: **2026-08-23**.

## Checked-in fixture candidates from the old branch

### `origami-in-n-dimensions-2203.11355.pdf`

- Title: *Origami in N dimensions: How feed-forward networks manufacture linear separability*
- Authors: Christian Keup, Moritz Helias
- Source: https://arxiv.org/abs/2203.11355
- PDF: https://arxiv.org/pdf/2203.11355
- Submitted: 2022-03-21
- License shown by arXiv: CC BY-SA 4.0
- License: https://creativecommons.org/licenses/by-sa/4.0/
- SHA-256 observed 2026-08-23: `5cbcfdaff46b8ce38bf54e9f70aa8e92e66edb232e0763417ac99daa0f642770`
- Why useful: figure-heavy mathematical paper; useful for image objects, page resources, figure/caption association, and mixed text/graphics layouts.

### `complexity-of-linear-regions-in-deep-networks.pdf`

- Title: *Complexity of Linear Regions in Deep Networks*
- Authors: Boris Hanin, David Rolnick
- Original publication: Proceedings of the 36th International Conference on Machine Learning, PMLR 97:2596-2604, 2019
- Publication page: https://proceedings.mlr.press/v97/hanin19a.html
- PDF: https://proceedings.mlr.press/v97/hanin19a/hanin19a.pdf
- Related arXiv source: https://arxiv.org/abs/1901.09021
- PMLR publication agreement licenses articles to the public under CC BY 4.0: https://proceedings.mlr.press/pmlr-license-agreement.html
- License: https://creativecommons.org/licenses/by/4.0/
- SHA-256 observed 2026-08-23: `951f8f5c92c3d6b2da046168d02bdc058c12dacffe518c023fea60ca13ec4e25`
- Why useful: nine-page two-column paper with figures and captions beginning on page 1; useful for testing spatial caption association as well as extraction.

### `on-the-expressive-power-of-deep-neural-networks.pdf`

- Title: *On the Expressive Power of Deep Neural Networks*
- Authors: Maithra Raghu, Ben Poole, Jon Kleinberg, Surya Ganguli, Jascha Sohl-Dickstein
- Original publication: Proceedings of the 34th International Conference on Machine Learning, PMLR 70, 2017
- Publication page: https://proceedings.mlr.press/v70/raghu17a.html
- PDF: https://proceedings.mlr.press/v70/raghu17a/raghu17a.pdf
- Related arXiv source: https://arxiv.org/abs/1606.05336
- PMLR publication agreement licenses articles to the public under CC BY 4.0: https://proceedings.mlr.press/pmlr-license-agreement.html
- License: https://creativecommons.org/licenses/by/4.0/
- SHA-256 observed 2026-08-23: `865fece7060b79b2a47db57a1a7842759337a894facd6eb932bf62241920364b`
- Why useful: eight-page two-column scientific paper; a useful contrast with the more image-heavy fixtures and another independently produced real PDF structure.

## Indexed remote PDFs

These are useful parser targets, but their arXiv pages showed only arXiv's non-exclusive distribution license rather than a general redistribution license. Keep them indexed rather than copying them into a public repository unless a redistributable publication copy is identified.

- https://arxiv.org/abs/1312.6098 — *On the number of response regions of deep feed forward networks with piece-wise linear activations* — Razvan Pascanu, Guido Montufar, Yoshua Bengio
- https://arxiv.org/abs/1803.01719 — *How to Start Training: The Effect of Initialization and Architecture* — Boris Hanin, David Rolnick
- https://arxiv.org/abs/2305.00241 — *When Deep Learning Meets Polyhedral Theory: A Survey* — Joey Huchette, Gonzalo Muñoz, Thiago Serra, Calvin Tsay

The arXiv links `1606.05336` and `1901.09021` are represented above by redistributable PMLR publication copies.

arXiv non-exclusive distribution license: https://arxiv.org/licenses/nonexclusive-distrib/1.0/

## Indexed Internet Archive source

- https://archive.org/details/hegelmythslegendOOOOunse — *Hegel Myths and Legends*, edited by Jon Stewart.

Keep this remote unless a general redistribution license for copying the complete PDF into the public repository is established.

## Fixture policy

Repository size is not a reason to avoid real fixtures. When a previously supplied PDF has clear redistribution permission, prefer checking the complete PDF into the fixture corpus rather than replacing it with a tiny generated stand-in.

For extraction tests, figures and their captions belong together: a successful figure fixture should eventually assert both the extracted visual material and its associated caption/layout text where the PDF provides one.
