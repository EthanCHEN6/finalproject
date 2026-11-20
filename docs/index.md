# finalproject

Yitian(Ethan) Chen · BST260 · Fall 2026

For my final project, I plan to study BMI and systolic blood pressure in
NHANES and compare foodborne and waterborne outbreak sizes in CDC data.
The proposal describes the questions, datasets, and analyses I plan to
use.

- [Proposal](https://EthanCHEN6.github.io/finalproject/articles/proposal.html)
- [Project website](https://EthanCHEN6.github.io/finalproject/)

## Install

With R 4.5.0 or later:

``` r

devtools::install_github("EthanCHEN6/finalproject")
data("waterborne", package = "finalproject")
```

## Build the website

Install Quarto and the R packages `pkgdown`, `quarto`, `knitr`, and
`rmarkdown`. Open `finalproject.Rproj` and run:

``` r

pkgdown::build_site()
```

GitHub Pages publishes the `docs/` folder on `main`.

## Course template

This repository is forked from [Vincent Carey’s BST260
template](https://github.com/vjcitn/bst260project-template). It retains
the course datasets, instructions, and project suggestions. My proposal
is in `vignettes/proposal.qmd`.
