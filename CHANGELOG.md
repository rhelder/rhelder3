# `rhelder3` v0.2.0 (2026/08/16)

## Bug Fixes

* Include optional arg in `\MakeUppercase` spec
  - `\MakeUppercase` takes an optional argument, so include that in its
    `\RenewDocumentCommand`. The fact that the optional argument wasn't
    specified was, under bizarrely particular circumstances (e.g., when not
    preceded by `\headfamily` in `\maketitle`, only if `microtype` was
    loaded), causing `\MakeUppercaseCopy` to pick up the next token
    (`\group_end:`) as the optional argument at some point in its expansion,
    which led to kernel-level key/value errors that were not easy to trace.
* [**breaking**] Replace redefined `\MakeUppercase` with `\textcaps`
  - `\MakeUppercase` has many uses other than just making its argument all
    caps (e.g., it can capitalize the first letter of each word), whereas
    what we are trying to do is just letterspace full caps. So just define a
    wrapper around `\MakeUppercase`, where `\MakeUppercase` is used to put
    its argument in all caps, and letterspace those caps.
* [**breaking**] Don't use separate display font for 14pt font
  - 14pt is too close to 12pt for a separate display font. It's true that EB
    Garamond discolors a suprising amount even at 14pt, but using a display
    font at that size, although it evened out the color a bit, ended up
    having too striking an effect and thereby came off as showy.
* Always use same label sep and width for `enumerate`
  - The enumerate counter is set globally, so at the start of a new list
    `\labelenumi` could expand to any chance thing. Set to 0 locally to
    standardize.
* Expand to `\emph` rather than copying `\emph`
  - For some reason, making a copy caused the commands to not work in e.g.
    arguments to `\section`.
* [**breaking**] Make vertical skip amounts more useful
  - Adding as big an increment as 2 leads will probably not usually be very
    useful. But especially when not adhering to grid typesetting, a quarter
    or half of a lead could be very useful.
* Doc: Use correct `macro` env syntax
  - And add forgotten `\end{macro}`.
* Give correct names to section spacing constants
* Declare and set constants correctly
  - With `\dim_const:Nn` or `\skip_const:Nn`. This ensures global
    declaration, and also saves some lines (since the constant is declared
    and set on the same line).
* Correct various syntax errors in skip expressions
* Disable `microtype` features in some situations
  - Disable `microtype` altogether in `verbatim` environments, and disable
    expansion before calling `\RaggedRight`.

## Documentation

* Cv: Add sample CV
* Clarify choice of width for footnote label box

## Features

* [**breaking**] Design a `description` environment
* Letterspace year in `\today`
  - While doing our best to avoid this getting trambled over by `babel`.
* Provide class for typesetting CVs
  - Name the class `rhelder3cv`. The most essential features are:

    1. A different `\maketitle` command that typesets the author's name,
       title, affiliation, and contact information.
    2. An abandonment of grid typesetting, so not setting `\flushbottom`.
* Cv: [**breaking**] Redesign headings and subheads
  - Because `rhelder3cv` is not trying to align the text to a baseline grid,
    use more appealing proportions between the space before and after
    headings and subheads. Also allow this space to stretch and/or shrink.

    Don't number headings or subheads.

    Only provide `\section` and `\subsection`. Other subheads should not be
    necessary, and I don't want to design them.
* Cv: Use smaller `\topsep`, and allow stretch
* Cv: Set text ragged right
* Cv: Widen the measure
* Cv: Provide and set a `cv` page style
  - This simple page style prints the author's name and the date in the
    footer of the page, with the page number in the center
* [**breaking**] Throw error if any characters are missing

## Performance

* Calculate section skips only once

## Refactor

* [**breaking**] Make inline markup command names consistent
  - They should begin with the prefix `\text`.
* Don't create unnecessary `dim`s
  - Use scratch registers when it doesn't hurt the readability of the code,
    or do computations directly when it doesn't hurt the readability of the
    code.
* Define `\@listi` only once
  - Because size-dependent parameters are used in assignments in `\@listi`,
    each size-changing command was in practice re-declaring the same
    definition of `\@listi`. It was always confusing and hard to reason
    about to have so many definitions of `\@listi`, so just cut them.

    In theory, someone might want to make micro-adjustments to the list
    parameters depending on size. For now, A. that might not be good design,
    and B. we don't need it now, so for now favor simplicity.
* [**breaking**] Generate the size-changing commands
  - What is breaking about this change is that now *all* size-changing
    commands change display parameters and small/med/big skip amounts.
* Avoid small/med/bigskip language
  - It is less descriptive than just saying what interval of `\baselineskip`
    you intend to insert, and the LaTeX `\smallskip`, `\medskip`, and
    `\bigskip` use `\vspace`, which might have undesired results in
    combination with `\addvspace`. Leave `\smallskip`, `\medskip`, and
    `\bigskip` to users.
* Use more descriptive `\quad` to insert 1em
  - This is also more consistent, because `\quad` is used elsewhere (e.g.,
    section numbers).
* Generate `\@list` commands for nested lists
  - To cut down on more repetitive code

# `rhelder3` v0.1.0 (2026/08/08)

First release.
