
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
browser within the documentation website, eliminating the need for local
R installation while exploring package functionality.

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

## Technical Implementation

This repository demonstrates the integration of several technologies to
create interactive R documentation within a pkgdown website. The
implementation combines standard R package development with WebAssembly
compilation and automated deployment.

### WebR and WebAssembly Compilation

WebR is a distribution of R compiled for web browsers using WebAssembly
technology. Unlike standard R packages, webR packages require
compilation into WebAssembly binaries using the
[`rwasm`](https://github.com/r-wasm/rwasm) package. While CRAN packages
typically have pre-built webR binaries available, developmental packages
require custom compilation.

This repository uses the
[`r-wasm/actions`](https://github.com/r-wasm/actions) GitHub Action to
automate the WebAssembly compilation process, ensuring that the package
is available for use in webR environments.

### GitHub Actions Workflow Configuration

The workflow file `.github/workflows/rwasm-binary-and-pkgdown-site.yml`
orchestrates:

- **Documentation Generation**: Building the pkgdown site with
  interactive examples
- **WebR Binary Creation**: Compiling package binaries for browser use  
- **Package Repository Setup**: Creating a webR-compatible package
  repository structure
- **Site Deployment**: Publishing the combined documentation and
  binaries to GitHub Pages

The workflow ensures that both the documentation and the necessary webR
binaries are available at the same domain, enabling seamless integration
of interactive examples within the documentation.

## Implementation Guide

To implement interactive webR documentation in your own R package,
follow these steps:

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
> remotes::install_github("coatless-rpkg/rocleteer#1")
> ```

To use the `@examplesWebR` tag in your package, add the required
dependencies:

In your `DESCRIPTION` file:

    Suggests:
        rocleteer
    Remotes: coatless-rpkg/rocleteer

    Roxygen: list(..., packages = c("rocleteer"))

#### Update Documentation

> \[!NOTE\]
>
> Still a WIP.

Prepare your package for webR integration:

- **Add interactive examples**: Use `@examplesWebR` tags in your roxygen
  documentation instead of or alongside standard `@examples`
- **Update DESCRIPTION file**: Ensure the `URL` field lists your GitHub
  Pages site as the first URL (e.g.,
  `https://username.github.io/repository`)
- **Test locally**: Verify that your standard pkgdown site builds
  correctly before adding webR functionality

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

- [`{quarto-webr}`](https://github.com/coatless/quarto-webr/) - Quarto
  extension that provides webR integration for Quarto documents
- [`{altdoc}`](https://altdoc.etiennebacher.com/) - Alternative
  documentation generation tools for R packages
- [`{quarto-panelize}`](https://github.com/coatless-quarto/panelize) -
  Quarto extension for creating tabset panels
- [`{rwasm}`](https://github.com/r-wasm/rwasm) - Package for compiling R
  packages to WebAssembly

### Related Demonstrations

- [`quarto-webr-in-altdocs`](https://github.com/coatless-r-n-d/quarto-webr-in-altdocs) -
  Integration of webR with altdoc-generated documentation using Quarto
  extensions

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
