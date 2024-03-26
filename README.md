# Quarto Website project template for INRAE <img src="images/extension-logo.png" align="right" width="20%"/>

The aim of this quarto extension is to provide a template for creating a project website for INRAE researchers and engineers. This is an __unofficial__ and __opiniated__ template.

## Related work

-   The [quarto-inrae-extension](https://github.com/davidcarayon/quarto-inrae-extension)

-   The [{InraeThemes}](https://github.com/davidcarayon/InraeThemes) R package for ggplot2 and bootstrap themes

## Prerequisites

To make the full use of these templates, you will need :

* 2 fonts defined in INRAE's design system : `Raleway` and `Avenir Next Pro Cn` that can be downloaded [here](https://charte-identitaire.intranet.inrae.fr/content/download/3007/30036?version=5)
* A LaTeX installation if you are using the `beamer` format 
* Of course, [Quarto](https://quarto.org/) installed (**> 1.2.0**)

## Installing in a new project

You will need to do this to get all the folders with all the templates, assets and prefilled quarto templates :

```bash
quarto use template xxxxxx
```

## Installing for an existing project

You may also use this format with an existing project to download only the `_extensions` folder (**not recommended**).

```bash
quarto add xxxxxx
```

## How to use it

TO-DO

### Publishing

#### Gitlab Pages using CI/CD

You just need to add a `.gitlab-ci.yml` file to the repository root with this content (you may need to update the package list if you add more R-based content) :

```bash
# The Docker image that will be used to build your app
image: rocker/verse:4.3.1
pages:
  stage: deploy
  script:
    -  R -e "install.packages('pak')"
    -  R -e "pak::pak(c('wordcloud2','data.table','bib2df','magick'))"
    - quarto render
  publish: docs
  artifacts:
    paths:
      - docs
  rules:
    - if: $CI_COMMIT_BRANCH == $CI_DEFAULT_BRANCH
```

#### Github Pages using Github Actions

The following `publish.yml` file has to be placed in `.github/workflow/` :

```bash
on:
  on:
  schedule:
    - cron: '* * 1 * *'
  workflow_dispatch:
  push:
    branches: main

name: Quarto Publish

jobs:
  build-deploy:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - name: Check out repository
        uses: actions/checkout@v3

      - name: Set up Quarto
        uses: quarto-dev/quarto-actions/setup@v2

      - name: Render and Publish
        uses: quarto-dev/quarto-actions/publish@v2
        with:
          target: gh-pages

      - name: Install R Dependencies
        uses: r-lib/actions/setup-renv@v2
        with:
          cache-version: 1

        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

More about Quarto publishing here : <https://quarto.org/docs/publishing/>

## Real-life examples
