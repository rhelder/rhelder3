# `rhelder3` Document Class

## Goals of the Document Class

My personal document class for typesetting articles and other self-contained
papers. The first goal of `rhelder3` is to get the typographic basics right.
Respects in which `rhelder3` tries to do better than the standard classes
include (but are not limited to) the following:

*   Good leading
*   Easy-to-read measure
*   Appealing text block proportions
*   Rhythmic and logical vertical spacing
*   Subtly designed headings and subheads

Another goal of `rhelder3` is to support grid typesetting to some extent by
means of the following:

*   Making most vertical spaces rigid
*   Defining vertical spaces around section headings, block quotes, lists,
    etc. that cause the subsequent text to return to baseline
*   Setting `\flushbottom`. Because vertical spaces are specified in units of
    one lead (or sum to one lead), forcing LaTeX to align text with the bottom
    of the text block will incentivize LaTeX, in those cases where there is
    stretchable space, to align the preceding text with the baseline.

`rhelder3` does not intend to guarantee that text will be aligned with the
baseline. When designing a class that guarantees baseline alignment, there are
simply too many edge cases, and then even those edge cases have edge cases. If
perfect alignment with the baseline grid is desired, some manual intervention
may be required at the last stage of document preparation (just as manual
intervention will be required at that stage to e.g. deal with widows and
orphans).

## To-Do

`rhelder3` is a work in progress. The goal is to eventually implement most of
the functionality offered by the `article` standard class, but at least the
following still have not been implemented:

*   Two-column mode
*   Title page
*   Verse environment
*   Additional page styles
*   Marginal notes
*   Parameters for existing environments (see section 7.5 of `classes.dtx`)
*   Floats
*   Table of contents

These are not yet supported mainly because I don't use them often, so I don't
have strong opinions about how they should be designed. (The `description`
environment is also just borrowed straight from the standard classes.)

Other goals include:

1.  Use LaTeX's new mark mechanism with headings and subheads.

## Installation

Clone this repository:

```
git clone https://github.com/rhelder/rhelder3.git
```

Change directories into the clone, and install using the `l3build` utility,
which should be installed on your system with TeXLive. To install `rhelder3`
to your local tree, run

```
l3build install
```

To install `rhelder3` and its source files and documentation to your local
tree, run

```
l3build install --full
```
