# R for Mass Spectrometry

Applications in Proteomics and Metabolomics

Authors

Laurent Gatto

Sebastian Gibb

Johannes Rainer

Published

October 6, 2026

![](assets/cover.png "R for Mass Spectrometry")

**Package:** R4MS\
**Authors:** Laurent Gatto \[aut, cre\], Sebastian Gibb \[aut\], Johannes Rainer \[aut\]\
**Compiled:** 2026-10-06\
**Package version:** 0.98.0\
**R version:** **R version 4.6.1 (2026-06-24)**\
**BioC version:** **3.24**\
**License:** CC BY-SA 4.0\

# Preamble

The aim of the [R for Mass Spectrometry](https://www.rformassspectrometry.org/) initiative is to provide efficient, thoroughly documented, tested and flexible R software for the analysis and interpretation of high throughput mass spectrometry assays, including proteomics and metabolomics experiments. The project formalises the longtime collaborative development efforts of its core members under the RforMassSpectrometry organisation to facilitate dissemination and accessibility of their work.

![](https://github.com/rformassspectrometry/stickers/raw/master/sticker/RforMassSpectrometry.png)

The *R for Mass Spectrometry* intiative sticker, designed by Johannes Rainer.

This material introduces participants to the analysis and exploration of mass spectrometry (MS) based proteomics data using R and Bioconductor. The course will cover all levels of MS data, from raw data to identification and quantitation data, up to the statistical interpretation of a typical shotgun MS experiment and will focus on hands-on tutorials. At the end of this course, the participants will be able to manipulate MS data in R and use existing packages for their exploratory and statistical proteomics data analysis.

## Targeted audience and assumed background

The course material is targeted to either proteomics practitioners or data analysts/bioinformaticians that would like to learn how to use R and Bioconductor to analyse proteomics data. Familiarity with MS or proteomics in general is desirable, but not essential as we will walk through and describe a typical MS data as part of learning about the tools. For approachable introductions to sample preparation, mass spectrometry, data interpretation and analysis, readers are redirected to:

- *A beginner’s guide to mass spectrometry–based proteomics* ([Sinha and Mann 2020](#ref-Sinha:2020))
- *The ABC’s (and XYZ’s) of peptide sequencing* ([Steen and Mann 2004](#ref-Steen:2004))
- *How do shotgun proteomics algorithms identify proteins?* ([Marcotte 2007](#ref-Marcotte:2007))
- *An Introduction to Mass Spectrometry-Based Proteomics* ([Shuken 2023](#ref-Shuken:2023))

A working knowledge of R (R syntax, commonly used functions, basic data structures such as data frames, vectors, matrices, … and their manipulation) is required. Familiarity with other Bioconductor omics data classes and the tidyverse syntax is useful, but not necessary.

## Setup

This material uses the latest version of the R for Mass Spectrometry package and their dependencies. It might thus be possible that even the latest Bioconductor stable version isn’t recent enough.

To install all the necessary package, please use the latest release of R and execute:

``` downlit
if (!requireNamespace("BiocManager", quietly = TRUE))
    install.packages("BiocManager")

BiocManager::install("remotes")
BiocManager::install("tidyverse")
BiocManager::install("factoextra")
BiocManager::install("MsDataHub")
BiocManager::install("mzR")
BiocManager::install("rhdf5")
BiocManager::install("rpx")
BiocManager::install("MsCoreUtils")
BiocManager::install("QFeatures")
BiocManager::install("Spectra")
BiocManager::install("ProtGenerics")
BiocManager::install("PSMatch")
BiocManager::install("PTMods")
BiocManager::install("pheatmap")
BiocManager::install("limma")
BiocManager::install("MSnID")
BiocManager::install("Biostrings")
BiocManager::install("cleaver")
BiocManager::install("RforMassSpectrometry/SpectraVis")
```

After installation, you can download some data that will be used in the latter chapter running the following:

``` downlit
library(rpx)
px <- PXDataset("PXD000001") ## answer yes if asked to create a cache directory
fn <- "TMT_Erwinia_1uLSike_Top10HCD_isol2_45stepped_60min_01-20141210.mzML"
mzf <- pxget(px, fn)
px <- PXDataset("PXD022816")
pxget(px, grep("mzID", pxfiles(px))[1:3])
pxget(px, grep("mzML", pxfiles(px))[1:3])
```

All software versions used to generate this document are recoded at the end of the book in [sec-si](#sec-si).

## Questions and help

For questions about specific software or their usage, please refer to the software’s github issue page, or use the [Bioconductor support site](http://support.bioconductor.org/).

## Citation

If you need to cite this book, please use the following reference:

[![](https://zenodo.org/badge/349528091.svg)](https://doi.org/10.5281/zenodo.15180829)

DOI

Laurent Gatto, Sebastian Gibb and Johannes Rainer, *R for Mass Spectrometry* (2025) [DOI:10.5281/zenodo.15180830](https://doi.org/10.5281/zenodo.15180829).

## Acknowledgments

Thank you to [Charlotte Soneson](https://github.com/csoneson) for fixing many typos in a previous version of this book.

## License

[![Creative Commons Licence](https://i.creativecommons.org/l/by-sa/4.0/88x31.png)](http://creativecommons.org/licenses/by-sa/4.0/)\
This material is licensed under a [Creative Commons Attribution-ShareAlike 4.0 International License](http://creativecommons.org/licenses/by-sa/4.0/). You are free to **share** (copy and redistribute the material in any medium or format) and **adapt** (remix, transform, and build upon the material) for any purpose, even commercially, as long as you give appropriate credit and distribute your contributions under the same license as the original.

# Docker image

A `Docker` image built from this repository is available here:

👉 [ghcr.io/js2264/r4ms](https://ghcr.io/js2264/r4ms) 🐳

> **TIP:**
>
> You can get access to all the packages used in this book in \< 1 minute, using this command in a terminal:
>
> ``` sh
> docker run -it ghcr.io/js2264/r4ms:devel R
> ```

# RStudio Server

An RStudio Server instance can be initiated from the `Docker` image as follows:

``` sh
docker run \
    --volume <local_folder>:<destination_folder> \
    -e PASSWORD=OHCA \
    -p 8787:8787 \
    ghcr.io/js2264/r4ms:devel
```

The initiated RStudio Server instance will be available at <https://localhost:8787>.

# `python` support

The `Docker` image ships the book’s `conda` environment, holding everything listed in `inst/requirements.yml`. It is created at build time by [`BiocBook::setup_python()`](https://rdrr.io/pkg/BiocBook/man/BiocBook-python.html), which fetches a pinned `micromamba` if the machine has no `conda` at all.

`reticulate` picks it up automatically, so `python` just works in any `R` session started from the image:

``` sh
docker run -it ghcr.io/js2264/r4ms:devel R
```

``` downlit
library(reticulate)
py_config()          # already points at the book's environment
```

To execute `python` in a book page, open the page with an `R` chunk calling [`BiocBook::setup_python()`](https://rdrr.io/pkg/BiocBook/man/BiocBook-python.html) and write ```` ```{python} ```` chunks after it. Keep at least one `R` chunk on the page: that is what keeps `quarto` on the `knitr` engine, and therefore on `reticulate`, rather than on `jupyter`.

# Session info

> **NOTE:**
>
> ``` downlit
> sessioninfo::session_info(
>     installed.packages()[,"Package"], 
>     include_base = TRUE
> )
> ##  ─ Session info ────────────────────────────────────────────────────────────
> ##   setting  value
> ##   version  R version 4.6.1 (2026-06-24)
> ##   os       Ubuntu 24.04.4 LTS
> ##   system   x86_64, linux-gnu
> ##   ui       X11
> ##   language (EN)
> ##   collate  C
> ##   ctype    en_US.UTF-8
> ##   tz       Etc/UTC
> ##   date     2026-10-06
> ##   pandoc   3.11 @ /usr/bin/ (via rmarkdown)
> ##   quarto   1.11.5 @ /usr/local/bin/quarto
> ##  
> ##  ─ Packages ────────────────────────────────────────────────────────────────
> ##   package              * version    date (UTC) lib source
> ##   abind                  1.4-8      2024-09-12 [2] RSPM (R 4.6.0)
> ##   affy                   1.91.0     2026-04-28 [2] Bioconductor 3.24 (R 4.6.1)
> ##   affyio                 1.83.0     2026-04-28 [2] Bioconductor 3.24 (R 4.6.1)
> ##   AnnotationDbi          1.75.2     2026-07-21 [2] Bioconductor 3.24 (R 4.6.1)
> ##   AnnotationFilter       1.37.0     2026-04-28 [2] Bioconductor 3.24 (R 4.6.1)
> ##   AnnotationHub          4.3.2      2026-06-30 [2] Bioconductor 3.24 (R 4.6.1)
> ##   askpass                1.2.1      2024-10-04 [2] RSPM (R 4.6.0)
> ##   backports              1.5.1      2026-04-03 [2] RSPM (R 4.6.0)
> ##   base                 * 4.6.1      2026-09-11 [3] local
> ##   base64enc              0.1-6      2026-02-02 [2] RSPM (R 4.6.0)
> ##   BH                     1.90.0-1   2025-12-14 [2] RSPM (R 4.6.0)
> ##   Biobase              * 2.73.2     2026-07-29 [2] Bioconductor 3.24 (R 4.6.1)
> ##   BiocBaseUtils          1.15.1     2026-05-10 [2] Bioconductor 3.24 (R 4.6.1)
> ##   BiocBook               1.11.1     2026-09-06 [2] Bioconductor 3.24 (R 4.6.1)
> ##   BiocFileCache          3.3.0      2026-04-28 [2] Bioconductor 3.24 (R 4.6.1)
> ##   BiocGenerics         * 0.59.12    2026-08-11 [2] Bioconductor 3.24 (R 4.6.1)
> ##   biocmake               1.5.1      2026-08-21 [2] Bioconductor 3.24 (R 4.6.1)
> ##   BiocManager            1.30.27    2025-11-14 [2] CRAN (R 4.6.1)
> ##   BiocParallel         * 1.47.0     2026-04-28 [2] Bioconductor 3.24 (R 4.6.1)
> ##   BiocStyle            * 2.41.0     2026-04-28 [2] Bioconductor 3.24 (R 4.6.1)
> ##   BiocVersion            3.24.0     2026-04-28 [2] Bioconductor 3.24 (R 4.6.1)
> ##   Biostrings             2.81.9     2026-09-06 [2] Bioconductor 3.24 (R 4.6.1)
> ##   bit                    4.6.0      2025-03-06 [2] RSPM (R 4.6.0)
> ##   bit64                  4.8.6      2026-09-01 [2] RSPM (R 4.6.0)
> ##   bitops                 1.1-0      2026-07-30 [2] RSPM (R 4.6.0)
> ##   blob                   1.3.0      2026-01-14 [2] RSPM (R 4.6.0)
> ##   bookdown               0.48       2026-08-28 [2] RSPM (R 4.6.0)
> ##   boot                   1.3-32     2025-08-29 [3] CRAN (R 4.6.1)
> ##   brew                   1.0-10     2023-12-16 [2] RSPM (R 4.6.0)
> ##   brio                   1.1.5      2024-04-24 [2] RSPM (R 4.6.0)
> ##   broom                  1.0.13     2026-05-14 [2] RSPM (R 4.6.0)
> ##   bslib                  0.12.0     2026-08-04 [2] RSPM (R 4.6.0)
> ##   cachem                 1.1.0      2024-05-16 [2] RSPM (R 4.6.0)
> ##   callr                  3.8.0      2026-06-05 [2] RSPM (R 4.6.0)
> ##   car                    3.1-5      2026-02-03 [2] RSPM (R 4.6.0)
> ##   carData                3.0-6      2026-01-30 [2] RSPM (R 4.6.0)
> ##   caTools                1.18.4     2026-07-20 [2] RSPM (R 4.6.0)
> ##   cellranger             1.1.0      2016-07-27 [2] RSPM (R 4.6.0)
> ##   class                  7.3-24     2026-08-03 [2] RSPM (R 4.6.0)
> ##   cleaver                1.51.0     2026-04-28 [2] Bioconductor 3.24 (R 4.6.1)
> ##   cli                    3.6.6      2026-04-09 [2] RSPM (R 4.6.0)
> ##   clipr                  0.8.1      2026-05-25 [2] RSPM (R 4.6.0)
> ##   clue                   0.3-68     2026-03-26 [2] RSPM (R 4.6.0)
> ##   cluster                2.1.8.3    2026-07-30 [2] RSPM (R 4.6.0)
> ##   codetools              0.2-20     2024-03-31 [3] CRAN (R 4.6.1)
> ##   colorspace             2.1-3      2026-07-12 [2] RSPM (R 4.6.0)
> ##   commonmark             2.0.0      2025-07-07 [2] RSPM (R 4.6.0)
> ##   compiler               4.6.1      2026-09-11 [3] local
> ##   conflicted             1.2.0      2023-02-01 [2] RSPM (R 4.6.0)
> ##   corrplot               0.95       2024-10-14 [2] RSPM (R 4.6.0)
> ##   cowplot                1.2.0      2025-07-07 [2] RSPM (R 4.6.0)
> ##   cpp11                  0.5.5      2026-05-06 [2] RSPM (R 4.6.0)
> ##   crayon                 1.5.3      2024-06-20 [2] RSPM (R 4.6.0)
> ##   credentials            2.0.3      2025-09-12 [2] RSPM (R 4.6.0)
> ##   crosstalk              1.2.2      2025-08-26 [2] RSPM (R 4.6.0)
> ##   curl                   8.0.0      2026-08-25 [2] RSPM (R 4.6.0)
> ##   data.table             1.18.6.1   2026-08-24 [2] RSPM (R 4.6.0)
> ##   datasets             * 4.6.1      2026-09-11 [3] local
> ##   DBI                    1.3.0      2026-02-25 [2] RSPM (R 4.6.0)
> ##   dbplyr                 2.6.0      2026-06-17 [2] RSPM (R 4.6.0)
> ##   DelayedArray           0.39.8     2026-09-30 [2] Bioconductor 3.24 (R 4.6.1)
> ##   dendextend             1.19.1     2025-07-15 [2] RSPM (R 4.6.0)
> ##   Deriv                  4.3.5      2026-09-03 [2] RSPM (R 4.6.0)
> ##   desc                   1.4.3      2023-12-10 [2] RSPM (R 4.6.0)
> ##   devtools               2.5.2      2026-04-30 [2] RSPM (R 4.6.0)
> ##   diffobj                0.3.9      2026-09-11 [2] RSPM (R 4.6.0)
> ##   digest                 0.6.39     2025-11-19 [2] RSPM (R 4.6.0)
> ##   dir.expiry             1.21.0     2026-04-28 [2] Bioconductor 3.24 (R 4.6.1)
> ##   doBy                   4.7.2      2026-07-01 [2] RSPM (R 4.6.0)
> ##   docopt                 0.7.2      2025-03-25 [2] RSPM (R 4.6.1)
> ##   doParallel             1.0.17     2022-02-07 [2] RSPM (R 4.6.0)
> ##   downlit                0.4.5      2025-11-14 [2] RSPM (R 4.6.0)
> ##   dplyr                  1.2.1      2026-04-03 [2] RSPM (R 4.6.0)
> ##   DT                     0.34.0     2025-09-02 [2] RSPM (R 4.6.0)
> ##   dtplyr                 1.3.3      2026-02-11 [2] RSPM (R 4.6.0)
> ##   edgeR                  4.99.6     2026-09-13 [2] Bioconductor 3.24 (R 4.6.1)
> ##   ellipse                0.5.0      2023-07-20 [2] RSPM (R 4.6.0)
> ##   ellipsis               0.3.3      2026-04-04 [2] RSPM (R 4.6.0)
> ##   emmeans                2.0.4      2026-07-15 [2] RSPM (R 4.6.0)
> ##   estimability           2.0.0      2026-06-26 [2] RSPM (R 4.6.0)
> ##   evaluate               1.0.5      2025-08-27 [2] RSPM (R 4.6.0)
> ##   ExperimentHub          3.3.2      2026-08-12 [2] Bioconductor 3.24 (R 4.6.1)
> ##   factoextra             2.2.0      2026-07-24 [2] RSPM (R 4.6.0)
> ##   FactoMineR             2.17       2026-09-08 [2] RSPM (R 4.6.0)
> ##   fansi                  1.0.7      2025-11-19 [2] RSPM (R 4.6.0)
> ##   farver                 2.1.2      2024-05-13 [2] RSPM (R 4.6.0)
> ##   fastmap                1.2.0      2024-05-15 [2] RSPM (R 4.6.0)
> ##   filelock               1.0.3      2023-12-11 [2] RSPM (R 4.6.0)
> ##   flashClust             1.1-4      2026-03-03 [2] RSPM (R 4.6.0)
> ##   fontawesome            0.5.3      2024-11-16 [2] RSPM (R 4.6.0)
> ##   forcats                1.0.1      2025-09-25 [2] RSPM (R 4.6.0)
> ##   foreach                1.5.2      2022-02-02 [2] RSPM (R 4.6.0)
> ##   forecast               9.0.2      2026-03-18 [2] RSPM (R 4.6.0)
> ##   foreign                0.8-91     2026-01-29 [3] CRAN (R 4.6.1)
> ##   formatR                1.14       2023-01-17 [2] RSPM (R 4.6.0)
> ##   Formula                1.2-6      2026-08-03 [2] RSPM (R 4.6.0)
> ##   fracdiff               1.5-4      2026-04-28 [2] RSPM (R 4.6.0)
> ##   fs                     2.1.0      2026-04-18 [2] RSPM (R 4.6.0)
> ##   futile.logger          1.4.9      2025-12-29 [2] RSPM (R 4.6.0)
> ##   futile.options         1.0.1      2018-04-20 [2] RSPM (R 4.6.0)
> ##   gargle                 1.6.1      2026-01-29 [2] RSPM (R 4.6.0)
> ##   generics             * 0.1.4      2025-05-09 [2] RSPM (R 4.6.0)
> ##   GenomicRanges        * 1.65.4     2026-09-02 [2] Bioconductor 3.24 (R 4.6.1)
> ##   gert                   2.4.1      2026-08-19 [2] RSPM (R 4.6.0)
> ##   ggplot2                4.0.3      2026-04-22 [2] RSPM (R 4.6.0)
> ##   ggpubr                 1.0.0      2026-07-06 [2] RSPM (R 4.6.0)
> ##   ggrepel                0.9.8      2026-03-17 [2] RSPM (R 4.6.0)
> ##   ggsci                  5.2.0      2026-07-30 [2] RSPM (R 4.6.0)
> ##   ggsignif               0.6.4      2022-10-13 [2] RSPM (R 4.6.0)
> ##   gh                     1.6.1      2026-07-20 [2] RSPM (R 4.6.0)
> ##   gitcreds               0.1.2      2022-09-08 [2] RSPM (R 4.6.0)
> ##   glue                   1.8.1      2026-04-17 [2] RSPM (R 4.6.0)
> ##   googledrive            2.1.2      2025-09-10 [2] RSPM (R 4.6.0)
> ##   googlesheets4          1.1.2      2025-09-03 [2] RSPM (R 4.6.0)
> ##   gplots                 3.3.0      2025-11-30 [2] RSPM (R 4.6.0)
> ##   graphics             * 4.6.1      2026-09-11 [3] local
> ##   grDevices            * 4.6.1      2026-09-11 [3] local
> ##   grid                   4.6.1      2026-09-11 [3] local
> ##   gridExtra              2.3.1      2026-06-25 [2] RSPM (R 4.6.0)
> ##   gtable                 0.3.6      2024-10-25 [2] RSPM (R 4.6.0)
> ##   gtools                 3.9.5      2023-11-20 [2] RSPM (R 4.6.0)
> ##   haven                  2.5.5      2025-05-30 [2] RSPM (R 4.6.0)
> ##   here                   1.0.2      2025-09-15 [2] RSPM (R 4.6.0)
> ##   highr                  0.12       2026-03-06 [2] RSPM (R 4.6.0)
> ##   hms                    1.1.4      2025-10-17 [2] RSPM (R 4.6.0)
> ##   htmltools              0.5.9      2025-12-04 [2] RSPM (R 4.6.0)
> ##   htmlwidgets            1.6.4      2023-12-06 [2] RSPM (R 4.6.0)
> ##   httpuv                 1.6.17     2026-03-18 [2] RSPM (R 4.6.0)
> ##   httr                   1.4.9      2026-09-01 [2] RSPM (R 4.6.0)
> ##   httr2                  1.3.0      2026-07-13 [2] RSPM (R 4.6.0)
> ##   ids                    1.0.1      2017-05-31 [2] RSPM (R 4.6.0)
> ##   igraph                 2.3.4      2026-09-30 [2] RSPM (R 4.6.0)
> ##   impute                 1.87.0     2026-04-28 [2] Bioconductor 3.24 (R 4.6.1)
> ##   ini                    0.3.1      2018-05-20 [2] RSPM (R 4.6.0)
> ##   IRanges              * 2.47.5     2026-08-27 [2] Bioconductor 3.24 (R 4.6.1)
> ##   irlba                  2.4.1      2026-10-05 [2] RSPM (R 4.6.0)
> ##   isoband                0.3.0      2025-12-07 [2] RSPM (R 4.6.0)
> ##   iterators              1.0.14     2022-02-05 [2] RSPM (R 4.6.0)
> ##   jquerylib              0.1.4      2021-04-26 [2] RSPM (R 4.6.0)
> ##   jsonlite               2.0.0      2025-03-27 [2] RSPM (R 4.6.0)
> ##   KEGGREST               1.53.6     2026-07-23 [2] Bioconductor 3.24 (R 4.6.1)
> ##   KernSmooth             2.23-27    2026-08-12 [2] RSPM (R 4.6.0)
> ##   knitr                  1.52       2026-09-06 [2] RSPM (R 4.6.0)
> ##   labeling               0.4.3      2023-08-29 [2] RSPM (R 4.6.0)
> ##   lambda.r               1.2.4      2019-09-18 [2] RSPM (R 4.6.0)
> ##   later                  1.4.8      2026-03-05 [2] RSPM (R 4.6.0)
> ##   lattice                0.23-1     2026-08-12 [2] RSPM (R 4.6.0)
> ##   lazyeval               0.2.3      2026-04-04 [2] RSPM (R 4.6.0)
> ##   leaps                  3.2        2024-06-10 [2] RSPM (R 4.6.0)
> ##   lifecycle              1.0.5      2026-01-08 [2] RSPM (R 4.6.0)
> ##   limma                  3.99.0     2026-08-31 [2] Bioconductor 3.24 (R 4.6.1)
> ##   littler                0.3.23     2026-04-12 [2] RSPM (R 4.6.1)
> ##   lme4                   2.0-6      2026-07-16 [2] RSPM (R 4.6.0)
> ##   lmtest                 0.9-40     2022-03-21 [2] RSPM (R 4.6.0)
> ##   locfit                 1.5-9.12   2025-03-05 [2] RSPM (R 4.6.0)
> ##   lubridate              1.9.5      2026-02-04 [2] RSPM (R 4.6.0)
> ##   magrittr               2.0.5      2026-04-04 [2] RSPM (R 4.6.0)
> ##   MALDIquant             1.22.3     2024-08-19 [2] RSPM (R 4.6.0)
> ##   MASS                   7.3-66     2026-07-15 [2] RSPM (R 4.6.0)
> ##   Matrix                 1.7-6      2026-07-25 [2] RSPM (R 4.6.0)
> ##   MatrixGenerics       * 1.25.0     2026-04-28 [2] Bioconductor 3.24 (R 4.6.1)
> ##   MatrixModels           0.5-4      2025-03-26 [2] RSPM (R 4.6.0)
> ##   matrixStats          * 1.5.0      2025-01-07 [2] RSPM (R 4.6.0)
> ##   memoise                2.0.1      2021-11-26 [2] RSPM (R 4.6.0)
> ##   MetaboCoreUtils        1.21.1     2026-04-30 [2] Bioconductor 3.24 (R 4.6.1)
> ##   methods              * 4.6.1      2026-09-11 [3] local
> ##   mgcv                   1.9-4      2025-11-07 [3] CRAN (R 4.6.1)
> ##   mime                   0.13       2025-03-17 [2] RSPM (R 4.6.0)
> ##   miniUI                 0.1.2      2025-04-17 [2] RSPM (R 4.6.0)
> ##   minqa                  1.2.8      2024-08-17 [2] RSPM (R 4.6.0)
> ##   modelr                 0.1.11     2023-03-22 [2] RSPM (R 4.6.0)
> ##   MsCoreUtils          * 1.25.4     2026-05-11 [2] Bioconductor 3.24 (R 4.6.1)
> ##   MsDataHub              1.13.3     2026-10-02 [2] Bioconductor 3.24 (R 4.6.1)
> ##   msmsEDA                1.51.0     2026-07-14 [2] Bioconductor 3.24 (R 4.6.1)
> ##   msmsTests              1.51.0     2026-07-14 [2] Bioconductor 3.24 (R 4.6.1)
> ##   MSnbase                2.39.5     2026-08-09 [2] Bioconductor 3.24 (R 4.6.1)
> ##   MSnID                  1.47.0     2026-07-14 [2] Bioconductor 3.24 (R 4.6.1)
> ##   multcompView           0.1-12     2026-07-26 [2] RSPM (R 4.6.0)
> ##   MultiAssayExperiment * 1.39.1     2026-08-31 [2] Bioconductor 3.24 (R 4.6.1)
> ##   mvtnorm                1.4-2      2026-07-12 [2] RSPM (R 4.6.0)
> ##   mzID                   1.51.0     2026-04-28 [2] Bioconductor 3.24 (R 4.6.1)
> ##   mzR                  * 2.47.1     2026-09-02 [2] Bioconductor 3.24 (R 4.6.1)
> ##   ncdf4                  1.24       2025-03-25 [2] RSPM (R 4.6.0)
> ##   nlme                   3.1-171    2026-09-01 [2] RSPM (R 4.6.0)
> ##   nloptr                 2.2.1      2025-03-17 [2] RSPM (R 4.6.0)
> ##   nnet                   7.3-21     2026-08-03 [2] RSPM (R 4.6.0)
> ##   numDeriv               2016.8-1.1 2019-06-06 [2] RSPM (R 4.6.0)
> ##   openssl                2.4.2      2026-06-09 [2] RSPM (R 4.6.0)
> ##   otel                   0.2.0      2025-08-29 [2] RSPM (R 4.6.0)
> ##   pak                    0.11.1     2026-07-22 [2] RSPM (R 4.6.0)
> ##   parallel               4.6.1      2026-09-11 [3] local
> ##   patchwork              1.3.2      2025-08-25 [2] RSPM (R 4.6.0)
> ##   pbkrtest               0.5.5      2025-07-18 [2] RSPM (R 4.6.0)
> ##   pcaMethods             2.5.0      2026-04-28 [2] Bioconductor 3.24 (R 4.6.1)
> ##   pheatmap               1.0.13     2025-06-05 [2] RSPM (R 4.6.0)
> ##   pillar                 1.11.1     2025-09-17 [2] RSPM (R 4.6.0)
> ##   pkgbuild               1.4.8      2025-05-26 [2] RSPM (R 4.6.0)
> ##   pkgconfig              2.0.3      2019-09-22 [2] RSPM (R 4.6.0)
> ##   pkgdown                2.2.1      2026-07-07 [2] RSPM (R 4.6.0)
> ##   pkgload                1.5.3      2026-06-15 [2] RSPM (R 4.6.0)
> ##   plotly                 4.12.1     2026-07-22 [2] RSPM (R 4.6.0)
> ##   plyr                   1.8.9      2023-10-02 [2] RSPM (R 4.6.0)
> ##   png                    0.1-9      2026-03-15 [2] RSPM (R 4.6.0)
> ##   polynom                1.4-1      2022-04-11 [2] RSPM (R 4.6.0)
> ##   praise                 1.0.0      2015-08-11 [2] RSPM (R 4.6.0)
> ##   preprocessCore         1.75.1     2026-08-31 [2] Bioconductor 3.24 (R 4.6.1)
> ##   prettyunits            1.2.0      2023-09-24 [2] RSPM (R 4.6.0)
> ##   processx               3.9.0      2026-04-22 [2] RSPM (R 4.6.0)
> ##   profvis                0.4.0      2024-09-20 [2] RSPM (R 4.6.0)
> ##   progress               1.2.3      2023-12-06 [2] RSPM (R 4.6.0)
> ##   promises               1.5.0      2025-11-01 [2] RSPM (R 4.6.0)
> ##   ProtGenerics           1.45.0     2026-04-28 [2] Bioconductor 3.24 (R 4.6.1)
> ##   ps                     1.9.3      2026-04-20 [2] RSPM (R 4.6.0)
> ##   PSMatch                1.17.0     2026-04-28 [2] Bioconductor 3.24 (R 4.6.1)
> ##   PTMods                 1.1.0      2026-04-28 [2] Bioconductor 3.24 (R 4.6.1)
> ##   purrr                  1.2.2      2026-04-10 [2] RSPM (R 4.6.0)
> ##   QFeatures            * 1.23.2     2026-09-04 [2] Bioconductor 3.24 (R 4.6.1)
> ##   quantreg               6.1        2025-03-10 [2] RSPM (R 4.6.0)
> ##   quarto                 1.5.1      2025-09-04 [2] RSPM (R 4.6.0)
> ##   qvalue                 2.45.0     2026-04-28 [2] Bioconductor 3.24 (R 4.6.1)
> ##   R.cache                0.17.0     2025-05-02 [2] RSPM (R 4.6.0)
> ##   R.methodsS3            1.8.2      2022-06-13 [2] RSPM (R 4.6.0)
> ##   R.oo                   1.27.1     2025-05-02 [2] RSPM (R 4.6.0)
> ##   R.utils                2.13.0     2025-02-24 [2] RSPM (R 4.6.0)
> ##   R4MS                   0.98.0     2026-10-06 [1] local
> ##   R6                     2.6.1      2025-02-15 [2] RSPM (R 4.6.0)
> ##   ragg                   1.5.2      2026-03-23 [2] RSPM (R 4.6.0)
> ##   rappdirs               0.3.4      2026-01-17 [2] RSPM (R 4.6.0)
> ##   rbibutils              2.4.1      2026-01-21 [2] RSPM (R 4.6.0)
> ##   rcmdcheck              1.4.0      2021-09-27 [2] RSPM (R 4.6.0)
> ##   RColorBrewer           1.1-3      2022-04-03 [2] RSPM (R 4.6.0)
> ##   Rcpp                 * 1.1.2      2026-07-05 [2] RSPM (R 4.6.0)
> ##   RcppArmadillo          15.6.0-1   2026-09-08 [2] RSPM (R 4.6.0)
> ##   RcppEigen              0.3.4.0.2  2024-08-24 [2] RSPM (R 4.6.0)
> ##   RcppTOML               0.2.3      2025-03-08 [2] RSPM (R 4.6.0)
> ##   RCurl                  1.98-1.20  2026-08-21 [2] RSPM (R 4.6.0)
> ##   Rdpack                 2.6.6      2026-02-08 [2] RSPM (R 4.6.0)
> ##   rdtools                0.1.0      2026-07-16 [2] RSPM (R 4.6.0)
> ##   readr                  2.2.0      2026-02-19 [2] RSPM (R 4.6.0)
> ##   readxl                 1.5.0.1    2026-09-16 [2] RSPM (R 4.6.0)
> ##   reformulas             0.4.4      2026-02-02 [2] RSPM (R 4.6.0)
> ##   rematch                2.0.0      2023-08-30 [2] RSPM (R 4.6.0)
> ##   rematch2               2.1.2      2020-05-01 [2] RSPM (R 4.6.0)
> ##   remotes                2.5.0      2024-03-17 [2] RSPM (R 4.6.0)
> ##   renv                   1.3.0      2026-09-29 [2] RSPM (R 4.6.0)
> ##   reprex                 2.1.1      2024-07-06 [2] RSPM (R 4.6.0)
> ##   reshape2               1.4.5      2025-11-12 [2] RSPM (R 4.6.0)
> ##   reticulate             1.47.0     2026-09-03 [2] RSPM (R 4.6.0)
> ##   rhdf5                  2.57.18    2026-09-23 [2] Bioconductor 3.24 (R 4.6.1)
> ##   rhdf5filters           1.25.4     2026-08-06 [2] Bioconductor 3.24 (R 4.6.1)
> ##   Rhdf5lib               2.1.0      2026-04-28 [2] Bioconductor 3.24 (R 4.6.1)
> ##   rlang                  1.3.0      2026-07-05 [2] RSPM (R 4.6.0)
> ##   rmarkdown              2.32       2026-09-01 [2] RSPM (R 4.6.0)
> ##   roxygen2               8.1.0      2026-08-04 [2] RSPM (R 4.6.0)
> ##   rpart                  4.1.27     2026-03-27 [3] CRAN (R 4.6.1)
> ##   rprojroot              2.1.1      2025-08-26 [2] RSPM (R 4.6.0)
> ##   rpx                    2.21.2     2026-09-29 [2] Bioconductor 3.24 (R 4.6.1)
> ##   RSQLite                3.53.3     2026-06-30 [2] RSPM (R 4.6.0)
> ##   rstatix                1.1.0      2026-07-23 [2] RSPM (R 4.6.0)
> ##   rstudioapi             0.19.0     2026-06-11 [2] RSPM (R 4.6.0)
> ##   RUnit                  0.4.33.1   2025-06-17 [2] RSPM (R 4.6.0)
> ##   rversions              3.0.0      2025-10-09 [2] RSPM (R 4.6.0)
> ##   rvest                  1.0.5      2025-08-29 [2] RSPM (R 4.6.0)
> ##   S4Arrays               1.13.2     2026-09-30 [2] Bioconductor 3.24 (R 4.6.1)
> ##   S4Vectors            * 0.51.10    2026-09-16 [2] Bioconductor 3.24 (R 4.6.1)
> ##   S7                     0.2.2      2026-04-22 [2] RSPM (R 4.6.0)
> ##   sass                   0.4.10     2025-04-11 [2] RSPM (R 4.6.0)
> ##   scales                 1.4.0      2025-04-24 [2] RSPM (R 4.6.0)
> ##   scatterplot3d          0.3-45     2026-02-23 [2] RSPM (R 4.6.0)
> ##   selectr                0.8-0      2026-09-27 [2] RSPM (R 4.6.0)
> ##   Seqinfo              * 1.3.2      2026-08-27 [2] Bioconductor 3.24 (R 4.6.1)
> ##   sessioninfo            1.2.4      2026-06-04 [2] RSPM (R 4.6.0)
> ##   shiny                  1.14.0     2026-06-21 [2] RSPM (R 4.6.0)
> ##   showtext               0.9-8      2026-03-21 [2] RSPM (R 4.6.0)
> ##   showtextdb             3.0        2020-06-04 [2] RSPM (R 4.6.0)
> ##   snow                   0.4-4      2021-10-27 [2] RSPM (R 4.6.0)
> ##   sourcetools            0.1.7-2    2026-03-28 [2] RSPM (R 4.6.0)
> ##   SparseArray            1.13.4     2026-09-30 [2] Bioconductor 3.24 (R 4.6.1)
> ##   SparseM                1.84-2     2024-07-17 [2] RSPM (R 4.6.0)
> ##   spatial                7.3-19     2026-08-03 [2] RSPM (R 4.6.0)
> ##   Spectra              * 1.23.5     2026-09-14 [2] Bioconductor 3.24 (R 4.6.1)
> ##   splines                4.6.1      2026-09-11 [3] local
> ##   statmod                1.5.2      2026-05-17 [2] RSPM (R 4.6.0)
> ##   stats                * 4.6.1      2026-09-11 [3] local
> ##   stats4               * 4.6.1      2026-09-11 [3] local
> ##   stringi                1.8.9      2026-08-04 [2] RSPM (R 4.6.0)
> ##   stringr                1.6.0      2025-11-04 [2] RSPM (R 4.6.0)
> ##   SummarizedExperiment * 1.43.0     2026-04-28 [2] Bioconductor 3.24 (R 4.6.1)
> ##   survival               3.8-12     2026-09-09 [2] RSPM (R 4.6.0)
> ##   sys                    3.4.3      2024-10-04 [2] RSPM (R 4.6.0)
> ##   sysfonts               0.8.9      2024-03-02 [2] RSPM (R 4.6.0)
> ##   systemfonts            1.3.2      2026-03-05 [2] RSPM (R 4.6.0)
> ##   tcltk                  4.6.1      2026-09-11 [3] local
> ##   testthat               3.3.2      2026-01-11 [2] RSPM (R 4.6.0)
> ##   textshaping            1.0.5      2026-03-06 [2] RSPM (R 4.6.0)
> ##   tibble                 3.3.1      2026-01-11 [2] RSPM (R 4.6.0)
> ##   tidyr                  1.3.2      2025-12-19 [2] RSPM (R 4.6.0)
> ##   tidyselect             1.2.1      2024-03-11 [2] RSPM (R 4.6.0)
> ##   tidyverse              2.0.0      2023-02-22 [2] RSPM (R 4.6.0)
> ##   timechange             0.4.0      2026-01-29 [2] RSPM (R 4.6.0)
> ##   timeDate               4052.112   2026-01-28 [2] RSPM (R 4.6.0)
> ##   tinytex                0.61       2026-09-17 [2] RSPM (R 4.6.0)
> ##   tools                  4.6.1      2026-09-11 [3] local
> ##   tzdb                   0.5.0      2025-03-15 [2] RSPM (R 4.6.0)
> ##   urca                   1.3-4      2024-05-27 [2] RSPM (R 4.6.0)
> ##   urlchecker             2.0.0      2026-07-08 [2] RSPM (R 4.6.0)
> ##   usethis                3.2.2      2026-09-10 [2] RSPM (R 4.6.0)
> ##   utf8                   1.2.6      2025-06-08 [2] RSPM (R 4.6.0)
> ##   utils                * 4.6.1      2026-09-11 [3] local
> ##   uuid                   1.2-2      2026-01-23 [2] RSPM (R 4.6.0)
> ##   vctrs                  0.7.3      2026-04-11 [2] RSPM (R 4.6.0)
> ##   viridis                0.6.5      2024-01-29 [2] RSPM (R 4.6.0)
> ##   viridisLite            0.4.3      2026-02-04 [2] RSPM (R 4.6.0)
> ##   vroom                  1.7.1      2026-03-31 [2] RSPM (R 4.6.0)
> ##   vsn                    3.81.1     2026-09-23 [2] Bioconductor 3.24 (R 4.6.1)
> ##   waldo                  0.6.2      2025-07-11 [2] RSPM (R 4.6.0)
> ##   whisker                0.4.1      2022-12-05 [2] RSPM (R 4.6.0)
> ##   withr                  3.0.3      2026-06-19 [2] RSPM (R 4.6.0)
> ##   xfun                   0.61       2026-09-16 [2] RSPM (R 4.6.0)
> ##   XML                    3.99-0.25  2026-09-27 [2] RSPM (R 4.6.0)
> ##   xml2                   1.6.0      2026-06-22 [2] RSPM (R 4.6.0)
> ##   xopen                  1.0.1      2024-04-25 [2] RSPM (R 4.6.0)
> ##   xtable                 1.8-8      2026-02-22 [2] RSPM (R 4.6.0)
> ##   XVector                0.53.0     2026-04-28 [2] Bioconductor 3.24 (R 4.6.1)
> ##   yaml                   2.3.12     2025-12-10 [2] RSPM (R 4.6.0)
> ##   zip                    3.0.2      2026-08-04 [2] RSPM (R 4.6.0)
> ##   zoo                    1.9-1      2026-09-25 [2] RSPM (R 4.6.0)
> ##  
> ##   [1] /tmp/RtmpSfuPi0/Rinstb7ff10afe
> ##   [2] /usr/local/lib/R/site-library
> ##   [3] /usr/local/lib/R/library
> ##   * ── Packages attached to the search path.
> ##  
> ##  ─ Python configuration ────────────────────────────────────────────────────
> ##   Python is not available
> ##  
> ##  ───────────────────────────────────────────────────────────────────────────
> ```

Marcotte, Edward M. 2007. “How Do Shotgun Proteomics Algorithms Identify Proteins?” *Nat. Biotechnol.* 25 (7): 755–57.

Shuken, Steven R. 2023. “An Introduction to Mass Spectrometry-Based Proteomics.” *J. Proteome Res.*, June.

Sinha, Ankit, and Matthias Mann. 2020. “A beginner’s guide to mass spectrometry–based proteomics.” *The Biochemist*, ahead of print, September. <https://doi.org/10.1042/BIO20200057>.

Steen, Hanno, and Matthias Mann. 2004. “The ABC’s (and XYZ’s) of Peptide Sequencing.” *Nat. Rev. Mol. Cell Biol.* 5 (9): 699–711.
