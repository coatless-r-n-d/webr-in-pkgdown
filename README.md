
<!-- README.md is generated from README.Rmd. Please edit that file -->

# WebR in pkgdown Documentation Website

<!-- badges: start -->

[![R-CMD-check](https://github.com/coatless-r-n-d/webr-in-pkgdown/actions/workflows/R-CMD-check.yaml/badge.svg)](https://github.com/coatless-r-n-d/webr-in-pkgdown/actions/workflows/R-CMD-check.yaml)
[![R Wasm & pkgdown
deploy](https://github.com/coatless-r-n-d/webr-in-pkgdown/actions/workflows/rwasm-binary-and-pkgdown-site.yml/badge.svg)](https://github.com/coatless-r-n-d/webr-in-pkgdown/actions/workflows/rwasm-binary-and-pkgdown-site.yml)
<!-- badges: end -->

> \[!NOTE\]
>
> This repository is still a **work in progress (WIP)**. The
> documentation and examples are subject to change as we refine the
> integration of webR with pkgdown.

This repository demonstrates how to create interactive R documentation
by combining [pkgdown](https://pkgdown.r-lib.org/) with
[webR](https://docs.r-wasm.org/webr/latest/) (WebAssembly Distribution
of R). The approach enables users to run R code directly in their
browser within the documentation website, removing the need for local R
installation while exploring package functionality.

## Live Demo

Visit the [live documentation
website](https://rd.thecoatlessprofessor.com/webr-in-pkgdown) to see
interactive R code in action. The site includes interactive examples
for:

- [`in_webr()`](https://rd.thecoatlessprofessor.com/webr-in-pkgdown/reference/in_webr.html):
  Detect if R is running in WebAssembly
- [`residual_surrealism`](https://rd.thecoatlessprofessor.com/webr-in-pkgdown/reference/residual_surrealism.html):
  Interactive data visualization with surreal residuals

## Installation

You can install the package directly from GitHub using the `remotes`
package:

``` r
# install.packages("remotes")
remotes::install_github("coatless-r-n-d/webr-in-pkgdown")
```

Then, load the package:

``` r
library(pkgdownwebrdemo)
```

## Implementation Overview

The integration combines standard R package development with WebAssembly
compilation and automated deployment. WebR is a distribution of R
compiled for web browsers using WebAssembly technology. Unlike standard
R packages, webR packages require compilation into WebAssembly binaries
using the [`rwasm`](https://github.com/r-wasm/rwasm) package.

This repository uses the
[`r-wasm/actions`](https://github.com/r-wasm/actions) GitHub Action to
automate the WebAssembly compilation process. The workflow file
`.github/workflows/rwasm-binary-and-pkgdown-site.yml` handles
documentation generation, webR binary creation, package repository
setup, and deployment to GitHub Pages, ensuring that both documentation
and webR binaries are available at the same domain.

## Implementation Guide

To implement interactive webR documentation in your own R package,
follow the next steps closely. This guide assumes you have a
understanding of R package development and familiarity with GitHub
Actions.

### Step 1: Add the GitHub Actions Workflow

Obtain and configure the specialized workflow for building both pkgdown
documentation and webR binaries:

``` r
usethis::use_github_action(
  url = "https://github.com/r-wasm/actions/blob/main/examples/rwasm-binary-and-pkgdown-site.yml"
)
```

This workflow was developed through collaborative discussion documented
in [r-wasm/actions#15](https://github.com/r-wasm/actions/issues/15).

### Step 2: Configure GitHub Pages

Enable GitHub Pages for your repository:

1.  Navigate to your repository’s **Settings** tab
2.  Select **Pages** from the sidebar under “Code and automation”
3.  Set **Source** to “GitHub Actions”
4.  Enable **Enforce HTTPS** in the “Custom Domain” settings

![](https://github.com/user-attachments/assets/6d2cf5e2-4a5c-4447-ac35-8ab0a6a153e9)

### Step 3: Update Package Documentation

To enable interactive examples in your package documentation, use the
`@examplesWebR` tag from
[`rocleteer`](https://github.com/coatless-rpkg/rocleteer) in your
roxygen comments. This tag allows you to specify code that should be
executed in a webR environment.

#### Package Configuration

> \[!IMPORTANT\]
>
> You will need to use a developmental version of `rocleteer`.
>
> ``` r
> # install.packages("remotes")
> remotes::install_github("coatless-rpkg/rocleteer")
> ```

To use the `@examplesWebR` tag in your package, include the following in
your `DESCRIPTION` file:

    Suggests:
        rocleteer
    Remotes: coatless-rpkg/rocleteer

    Roxygen: list(packages = c("rocleteer"))

#### WebR Repository Configuration

Configure where `rocleteer` should find your webR-compatible package
binaries. You have two options:

**Option 1: GitHub Pages (recommended for this workflow)**

    Config/rocleteer/webr-repo: https://username.github.io/repository

**Option 2: R-Universe**

    Config/rocleteer/webr-repo: https://username.r-universe.dev

If not specified, `rocleteer` attempts automatic detection based on the
`URL` field in your `DESCRIPTION` file, with a warning if no compatible
repository is found.

#### Update Documentation

Prepare your package for webR integration:

- **Test locally**: Verify that your standard pkgdown site builds
  correctly before adding webR functionality
- **Add interactive examples**: Use `@examplesWebR` tags in your roxygen
  documentation instead of or alongside standard `@examples`
- **Update DESCRIPTION file**: Ensure the `Config/rocleteer/webr-repo`
  field or `URL` field lists your GitHub Pages site (e.g.,
  `https://username.github.io/repository`) or R-Universe account (e.g.,
  `https://username.r-universe.dev`)

### Step 4: Deploy and Verify

After pushing your changes:

1.  Monitor the GitHub Actions workflow execution
2.  Verify that both jobs (webR compilation and pkgdown building)
    complete successfully  
3.  Check that your GitHub Pages site displays with interactive code
    examples functional

## Related Projects and Technologies

This implementation builds upon several key technologies and projects:

### Core Technologies

- [webR](https://docs.r-wasm.org/webr/latest/) - The WebAssembly
  distribution of R that enables R execution in browsers
- [pkgdown](https://pkgdown.r-lib.org/) - R package documentation
  website generator

### Extensions and Tools

- [`{rwasm}`](https://github.com/r-wasm/rwasm) - Package for compiling R
  packages to WebAssembly
- [`{quarto-live}`](https://github.com/r-wasm/quarto-live) - Posit’s
  official Quarto extension for live R and Python code execution in web
  browsers
- [`{altdoc}`](https://altdoc.etiennebacher.com/) - Alternative
  documentation generation tools for R packages
- [`{quarto-webr}`](https://github.com/coatless/quarto-webr/) -
  Community Quarto extension that provides webR integration for Quarto
  documents
- [`{quarto-panelize}`](https://github.com/coatless-quarto/panelize) -
  Quarto extension for creating tabset panels with rendered and
  interactive content.

### Related Demonstrations

- [`quarto-webr-in-altdocs`](https://github.com/coatless-r-n-d/quarto-webr-in-altdocs) -
  Integration of webR with altdoc-generated documentation using
  community Quarto extensions

## Contributing

Contributions to improve this demonstration are welcome. Please feel
free to submit issues for questions or suggestions, and pull requests
for enhancements.

## Acknowledgments

This work builds on contributions from:

- The [webR development team](https://github.com/r-wasm/webr) for
  creating the WebAssembly distribution of R
- The broader R community for ongoing innovation in package
  documentation and accessibility
