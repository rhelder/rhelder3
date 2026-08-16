## [0.1.0] - 2026-08-09

### Bug Fixes

* Load packages as late as possible
  - To ensure that necessary things (e.g., `\normalsize`) that packages
    might expect are loaded.
* Set font features for `\headfamily`
* Preserve leading between footnotes
* Align bottom of footnotes with bottom of text block
* Prevent skip from being gobbled by `\addvspace`
  - The subsequent `\addvspace` before the section heading consumes the
    `\skip_vertical:n {-#2}`. Add a penalty so that the `\addvspace` doesn't
    see the previous skip.
* Check for when properties are not defined
  - Issue a warning, which needs to be done manually because `\RefProperty`
    is not expandable (see `clsguide`). And check if one of the integers set
    from the property is zero, and assume that means that the property is
    not yet defined.
* Ensure page break before inserting space
  - If there is enough space at the bottom of the previous page, LaTeX will
    insert \rule and the vertical skips (which will be lost, since at the
    end of the page) at the bottom of the previous page.
* Use `l3doc` class
  - So that `expl3` macros can be documented with `macro` environment and
    friends.
* Use correct environment name
* Rename commands related to package options
  - Use `Option` and `optionenv` to be consistent with `l3doc`.
* Correctly describe `\skip\footins`
  - It's a skip, not a dimen, and the `macro` environment expands `\footins`
    to something weird. `\skip\bslash footins` makes things come out right
    in the margin and in the index.
* Defensively set `\normalfont`
* Use proper variable naming convention
  - And include var type at end of name
* Fix typo in command name
* [**breaking**] Simplify to more reliably return to baseline
  - Lists were separated from the main text by an appealing half-lead, which
    works nicely if the list begins and ends on the same page, but throughs
    the text off-baseline otherwise. So separate lists from the main text by
    one lead, which looks fine and always works.

    Likewise, the mechanism for pushing section headers down if they are at
    the top of the page worked pretty well, but not always (and the
    limitations don't seem superable), so design section headers so that the
    text returns to baseline after them, at least most of the time. There
    will always be some edge cases, and these shouldn't be overzealously
    weeded out with elaborate programming. The rare deviant can be corrected
    by hand at the end, along with the widows and orphans.
* Make local variables local, and rename others
  - 1. Enclose all variables that can and should be local in groups, if they
       weren't already.
    2. Call all variables that needs to be global global.
    3. Rename global variables that are more specifically constants as
       constants.
* Protect commands that are not expandable
  - And use expl3 syntax in place of equivalent LaTeX2e commands
* Set `\maxdepth`
* Set `\partopsep`
* Remove outdated comment
* Don't set `\parindent` with size-changing commands
  - This caused unexpected indentations in environments where `\parindent`
    is set to 0, e.g. in a `center` environment. It also meant that
    `\parindent` declarations in e.g. a group are useless for text after a
    size-changing command.
* Fix typo
  - This macro isn't supposed to have a parameter
* Load some packages at the end of the class
  - Distinguish between packages that are dependencies of subsequent class
    code and packages that are typographically useful. Load the latter at
    the end of the class.
* Only ever use leading to determine interline glue
  - For the sake of aligning the text to the baseline
* Reduce letterspacing between small caps
  - Small caps are apparently already pretty well letterspaced in the font
    design
* Don't set `\lineskiplimit` to `-\maxdimen`
  - This causes problems in environments like `tabular`, where
    `\baselineskip` is set to 0. If no `\lineskip` is added in these
    environments, all of the text collapses to one line. These environments
    could be patched on an ad hoc basis, but with generous leading, setting
    `\lineskiplimit` to `-\maxdimen` will have almost no practical benefit
    for grid typesetting. So KISS.
* Obtain correct spacing between author and date
  - Even when author spans multiple lines
* Don't use the heading font for normal size date
  - It is too light at that size
* Avoid rounding error when shifting to baseline
* Don't set list parameters globally
* Improve centering of author name with `\thanks`
* Reduce space between `abstract` and body text
* Remove accidentally leftover debugging commands
* Use oldstyle numerals in display font
* Increase roman small caps letterspacing
  - Some glyphs almost overlap with current line spacing.
* Use display font for `\section` numbers
* Set itemize/enumerate item font in `\makelabel`
  - Rather than in the `\label` commands, which need to be safe in an
    expansion-only context (e.g., enumitem tries to expand their definition
    with `\xdef`).

    To do this, rewrite the `enumerate` and `itemize` definitions to contain
    a `\format` hook that can be defined to take formatting commands.
* Add `\baselineskip` before first line of title
  - By adding a `\rule` at the top of the `\vbox` – otherwise, no
    `\baselineskip` is inserted. This is not particularly cosmetically
    necessary, but where the first baseline of a box is should not
    depend on what the height of the first `\hbox` in the box (which could
    vary) happens to be. On page, for example, `\topskip` is inserted so
    that the first baseline is always at the same place on the page. (Here,
    the fact that the position of the first baseline depends on the height
    of the first `\hbox` makes it harder to determine the offset if e.g.
    `\ShowGrid` is used.
* Shift title box by least possible absolute value
  - It's just as well for the title to be shifted up as down, so aim for the
    least possible intervention, whether that means shifting up or shifting
    down.
* Load `ellipsis` after `hyperref`
  - `hyperref` redefines `\textellipsis`, so `ellipsis` needs to be loaded
    after `hyperref` to ensure its definition doesn't get overwritten.
* Make spacing around math displays more flexible
  - Grid typesetting can't be guaranteed around math displays without
    measuring the display box before insertion. I'm not willing to do that
    now, so give LaTeX broad latitude to choose good spacing for that page
    (anywhere between half a lead and a lead is fine).
* Declare module to which variable belongs
* Use `\normalfont` more defensively
  - `\normalfont` does what `\rmfamily` does, and then some.

    This doesn't fix a bug, really – but it might prevent future bugs.

    In general, select the font, then select the size. This generally
    doesn't matter, but it's nice to have a rule of thumb.

### Documentation

* Describe all defined or modified elements
* Use indicative, not imperative
* Make doc elements play nice with `l3doc`
  - In particular, use `imacro` and `lmacro` environments, rather than
    `function` or `macro` defined by `l3doc`, to describe LaTeX2e-style
    macros. This has the advantage both of accuracy (these should not be
    described as functions) and of avoiding 'undefined references warnings'
    (if the `l3doc` `macro` environment is used – it expects a corresponding
    `function` environment providing user-facing documentation of the
    macro).
* Differentiate between LaTeX2e and LaTeX3 commands
* Various small changes
* Small language change
  - We might not provide all of these sizes, if they weren't provided by the
    standard classes (and therefore expected by some packages)
* Document some harder aspects of `\@maketitle`
* Use correct comment character
* Add disclaimer about fourth list level
* Remove some various small detritus
* Give example of use of `\ShowGrid`
* Add some things that were forgotten
* Make various corrections/improvements
* Describe reasons for display skip values
* Print index
* Mark where `\alialingua` is defined
  - Forgot to do this when `\alialingua` was added.
* Mark as internal LaTeX macros
  - Because although they can be set by the user to customize things, they
    are not really meant to be used for this.
* Distinguish between class and LaTeX internal commands
* Avoid indexing some unwanted macros
  - When you don't want something in the doc text indexed, avoid using `\cs`
    or `\tn`, which will add the command to the index even if the command
    has been passed to `\DoNotIndex`. Use `\verb` to typeset the command
    instead.
* Use `sample` test also as sample of class

### Features

* Initial commit of minimal document class
* Add option to show baseline grid
* Set paragraph spacing
* Set generic list parameters
* Define environments for block quotations
* Letterspace caps and small caps
* Define section headings
* Use semi-bold instead of bold
* Shift heading if at top of page
  - So that subsequent text snaps to baseline.
* Make small caps Swash available
* Use small caps even for uppercase letters
* Support footnotes
* Place footnote marker in left margin
* Suppress the footnote rule
  - For a less modern/flash and more minimal look
* Typeset on letter-sized paper
* Def `\@list` for all levels allowed by `\list`
* Add support for `enumerate` environment
* Check for widows and orphans
* Add support for `itemize` and `description`
* Add `kant` option and `\AutoKant` command
  - To allow easier generation of complex sample texts.
* Use tabular figures in `enumerate`
  - For clearer alignment, especially since we are not using any punctuation
    to emphasize the alignment.
* Provide other size-changing commands
* Set display skips
* Provide horizontal spacing commands
* Provide macro to letterspace digits
* Set a unicode math font
* Implement `\maketitle`
* Define `\@ptsize` for compatibility
  - With any packages that use it.
* Load `microtype`
* Define intervals for vertical spacing
* Set penalties for line and page breaks
* Add macros to mark up initials and acronyms
* Set list-related penalties
* Implement `abstract` environment
* Call `\@listi` at start of `\trivlist`
* Define small, med, and large skips by size
* Load `ragged2e` for better unjustified text
* Implement `showgrid` option without `tikz`
* [**breaking**] Implement `\ShowGrid` macro for any vbox
  - This does mean that the `showgrid` option, which is for the whole page,
    only shows the grid over the size of `\l_shipout_box`, which doesn't
    take up the whole page. It is perfectly serviceable though.
* Make horizontal spacing commands configurable
* Configure ellipses, in and out of quotations
* Set a matching sans serif font
* Enable multilingual typesetting
  - In particular, set up Latin and Greek.
* Set a typewriter font
* Add Latin and Greek `csquotes` quotation styles
* Use `\autocite` as `csquotes` cite command
  - If `biblatex` is loaded
* Disable page numbering if only one page
* Load and configure `hyperref`
* Add some additional markup commands
* Improve index group names in various ways
  - Use plurals instead of singulars, and always prefix with 'class' or
    'LaTeX' where relevant (as opposed to e.g. 'internal', which weirdly
    puts 'class commands' under 'c' and internal class commands under 'i').
* Explicitly require LuaTeX
  - Although this should almost work for XeTeX, I think that because the
    typewriter font is loaded by name, not by file, it will only work for
    LuaTeX.

### Refactor

* [**breaking**] Directly specify font settings
  - And rename many of the commands to match what is described as common in
    the LaTeX Companion (3rd ed.)
* Use `xparse` command for consistency
* Use `expl3` syntax
* Use `expl3` syntax more consistently
* Specify dimensions with predefined constants
* Use `\dim_set_eq:NN` where possible
* Move `showgrid` code into option declaration
  - In general, don't define code unconditionally that requires a dependency
    that's loaded only conditionally – so move this code, which requires
    `tikz`, into the option declaration where `tikz` is loaded.
* Move essential code to front of class
* Consolidate related material
  - Gather all text-resizing macros together, and gather all list
    environments together
* Don't define unnecessary functions
  - Instead, just put code straight in `\NewDocumentCommand`, which is not
    uncommon in `expl3` packages.
* Enclose contents of `\@maketitle` in group
* Use `\baselineskip` instead of constant
  - For consistency and simplicity
* Don't use `\addfontfeature` alias
  - For consistency
* Use higher-level `\use:c` function
* Handle options after initialization

