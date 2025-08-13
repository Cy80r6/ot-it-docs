# CI/CD pro OT/IT Docs (MkDocs → GitHub Pages)

*Proč jednou větou:* Každý push do `main` automaticky přebuildí web a nasadí ho na GitHub Pages. Bez ručního nahrávání.

## Účel
- Automaticky nasazovat web z Markdownu při každém pushi.
- Mít opakovatelné kroky buildu v CI.
- Udržet repo jako jediný zdroj pravdy.

## Předpoklady
- Repo `ot-it-docs` (public), zapnuté GitHub Pages (Source: Deploy from a branch → gh-pages → /).
- Složka `docs/` a soubor `mkdocs.yml`.
- Workflow `.github/workflows/deploy.yml` (viz níže).

## Kroky

### 1. Workflow pro deploy
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

## Troubleshooting
## Problém: Actions zelené, ale web je starý

### Jak to vypadá
- Větve `gh-pages` přibyl nový commit, ale na URL vidím starý obsah.

### Nejčastější příčiny + rychlý fix
1) **Zdroj Pages neodpovídá způsobu deploye**
   - Deploy: `mkdocs gh-deploy` posílá do větve **`gh-pages`**.
   - Pages musí číst: **Deploy from a branch → gh-pages → /**.
   - **Fix:** Settings → Pages → Source přepnout na `gh-pages` a uložit.

2) **Cache prohlížeče / CDN**
   - **Fix:** tvrdý refresh (Ctrl/Cmd+F5) nebo přidej `?_cb=YYYYMMDDHHMM` za URL, např. `…/ot-it-docs/?_cb=20250813`.
   - Případně vyčisti uložená data pro daný web.

3) **Chybí `.nojekyll`**
   - MkDocs ho přidává automaticky. **Fix:** zkontroluj v rootu větve `gh-pages`, případně nasadit znovu.

4) **Míšmaš dvou strategií**
   - Buď nasazuj do **branch `gh-pages`**, nebo používej **Pages artifact**. Nemíchat.
   - Pro verzování s `mike` je lepší **`gh-pages`** (branch).

### Kontrolní checklist
- [ ] Settings → Pages: Source = Deploy from a branch, Branch = `gh-pages`, Folder = `/`.
- [ ] Větvi `gh-pages` přibyl čerstvý commit.
- [ ] Po přidání `?_cb=<něco>` na URL je vidět nová změna.

## Ověření
- **Actions**: poslední běh „Deploy MkDocs to GitHub Pages“ je zelený.
- **Větev `gh-pages`**: přibyl nový commit (čas teď).
- **Pages → Source**: `Deploy from a branch → gh-pages → /`.
- **Web**: `https://<user>.github.io/ot-it-docs/` se načte; lupa vyhledává; nová změna je vidět i s `?_cb=<timestamp>`.

## Rollback
- **Vrácení obsahu**: v GitHubu na posledním commitu klikni **Revert** → push do `main` (Actions nasadí zpět).
- **Dočasné zastavení deploye**: Actions → otevři workflow → **… → Disable workflow** (zapni po opravě).
- **Obnova posledního OK buildu**: Actions → najdi poslední zelený běh → **Re-run jobs** (přegeneruje web z tehdejšího stavu).
