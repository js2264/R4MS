# Migration from bookdown

This book was converted from the bookdown project `/tmp/claude-0/-home-user/72636713-12aa-53c5-b68e-650964832ea2/scratchpad/r4ms/src` by `BiocBook::from_bookdown()` (BiocBook 1.11.2), with the `msmbstyle` style (output format: `msmbstyle::msmb_html_book`).

## Converted files

| bookdown | BiocBook |
|---|---|
| `index.Rmd` | `inst/index.qmd` |
| `05-intro.Rmd` | `inst/pages/05-intro.qmd` |
| `10-raw.Rmd` | `inst/pages/10-raw.qmd` |
| `20-id.Rmd` | `inst/pages/20-id.qmd` |
| `30-quant.Rmd` | `inst/pages/30-quant.qmd` |
| `40-ex.Rmd` | `inst/pages/40-ex.qmd` |
| `95-annex.Rmd` | `inst/pages/95-annex.qmd` |
| `99-si.Rmd` | `inst/pages/99-si.qmd` |

Assets copied: `img/`, `packages.bib`, `refs.bib`, `skeleton.bib`, `style.css`.

## Automatic rewrites

| Rule | Applied |
|---|---:|
| `Chapter \@ref(x)` -> `@sec-x` | 3 |
| `fig.margin=TRUE` -> `#| column: margin` | 1 |
| `\@ref(x)` -> `[-@sec-x]` | 3 |
| `question_begin()` -> question callout | 40 |
| `question_end()` -> end of callout | 40 |
| `solution_begin()` -> collapsed answer callout | 35 |
| `solution_end()` -> end of callout | 35 |
| `fig.fullwidth=TRUE` -> `#| column: page` | 1 |
| `fig.margin=FALSE` removed | 1 |
| setup calls of `index.Rmd` repeated at the top of each chapter | 7 |

Dependencies added to `Imports`: `Biostrings`, `MSnID`, `MsCoreUtils`, `MsDataHub`, `PSMatch`, `QFeatures`, `Spectra`, `cleaver`, `dplyr`, `factoextra`, `ggplot2`, `gplots`, `limma`, `magrittr`, `mzID`, `mzR`, `patchwork`, `pheatmap`, `rpx`, `tidyr`, `tidyverse`.

Setup repeated at the top of each chapter:

```r
options(bitmapType="cairo")
suppressPackageStartupMessages(library("BiocStyle"))
suppressPackageStartupMessages(library("mzR"))
suppressPackageStartupMessages(library("Spectra"))
suppressPackageStartupMessages(library("QFeatures"))
suppressPackageStartupMessages(library("MsCoreUtils"))
```

## To do by hand

### Shared R session

- [ ] bookdown ran every chapter in a single R session, quarto renders each in its own: the setup calls of `index.Rmd` are now repeated at the top of each chapter, but objects created in a chapter and used in a later one must be recreated there. The first full render shows which.

### Unknown options

- [ ] Output format `msmbstyle::msmb_html_book`: its look is not migrated, the book uses the BiocBook theme
- [ ] Output option `highlight: tango` has no quarto equivalent and was not migrated
- [ ] `language` (`_bookdown.yml`): translate the labels with quarto's `lang`/`language` options

### Bibliographies written while the book builds

- [ ] `inst/index.qmd` line 80: generate the `.bib` file once, commit it to `inst/assets/` and drop the call
- [ ] `inst/pages/99-si.qmd` line 90: generate the `.bib` file once, commit it to `inst/assets/` and drop the call

### Network access while the book builds

- [ ] `inst/pages/05-intro.qmd` line 151: `mzf <- pxget(px, fn)`
- [ ] `inst/pages/20-id.qmd` line 676: `(fas <- pxget(px, grep("fasta", pxfiles(px))))`
- [ ] `inst/pages/40-ex.qmd` line 39: `(mzids <- pxget(PXD022816, grep("mzID", pxfiles(PXD022816))[1:3]))`
- [ ] `inst/pages/40-ex.qmd` line 40: `(mzmls <- pxget(PXD022816, grep("mzML", pxfiles(PXD022816))[1:3]))`

### Cached chunks

- [ ] `inst/pages/30-quant.qmd` line 763: remove `cache = TRUE`, or keep `*_cache/` out of git and of the package
- [ ] `inst/pages/30-quant.qmd` line 797: remove `cache = TRUE`, or keep `*_cache/` out of git and of the package

### Dependencies

- [ ] `RforMassSpectrometry/SpectraVis` is installed from GitHub: add it to `Remotes:` in DESCRIPTION if the book needs it to build, or drop it
- [ ] `lgatto/msmbstyle` is installed from GitHub: add it to `Remotes:` in DESCRIPTION if the book needs it to build, or drop it

### DESCRIPTION

- [ ] Replace the placeholder email of the maintainer (`cre`) in `Authors@R`
- [ ] Check `Title`, `Description` and `License`: the template's MIT licence may not be the book's

