# CI/CD pro OT/IT Docs (MkDocs → GitHub Pages)

*Proč jednou větou:* Každý push do `main` automaticky přebuildí web a nasadí ho na GitHub Pages. Bez ručního nahrávání.

## Účel
- Automaticky nasazovat web z Markdownu při každém pushi.
- Mít opakovatelné kroky buildu v CI.
- Udržet repo jako jediný zdroj pravdy.

## Předpoklady
- Repo `ot-it-docs` (public), zapnuté GitHub Pages (Source: GitHub Actions).
- Složka `docs/` a soubor `mkdocs.yml`.
- Workflow `.github/workflows/deploy.yml` (viz níže).

## Kroky

### 1) Workflow pro deploy
Soubor: `.github/workflows/deploy.yml`
```yaml
name: Deploy MkDocs to GitHub Pages

on:
  push:
    branches: [ "main" ]
  workflow_dispatch:

permissions:
  contents: write

jobs:
  build-deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.x'

      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install mkdocs-material

      - name: Configure Git for GitHub Actions
        run: |
          git config user.name "github-actions[bot]"
          git config user.email "41898282+github-actions[bot]@users.noreply.github.com"
          git remote set-url origin https://x-access-token:${GITHUB_TOKEN}@github.com/${{ github.repository }}.git
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}

      - name: Build & deploy to gh-pages
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        run: |
          mkdocs build --strict
          mkdocs gh-deploy --force --clean --remote-branch gh-pages
