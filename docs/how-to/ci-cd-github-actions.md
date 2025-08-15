# How-to: CI/CD – GitHub Actions pro MkDocs

!!! info "Účel"
    Automatizovat build a nasazení dokumentace MkDocs na GitHub Pages pomocí GitHub Actions.

## Předpoklady
- Repo na GitHubu
- MkDocs a mkdocs-material v projektu
- Nastavený GitHub Pages (branch `gh-pages`)

---

## Kroky
1. Vytvoř workflow `.github/workflows/deploy.yml` podle šablony v projektu.
2. Ujisti se, že v `mkdocs.yml` je správně nastavený `site_url` a `repo_url`.
3. Pushni změny do větve `main`.
4. Ověř, že Actions běží a web je nasazen na GitHub Pages.

---

## Ověření
- Po pushi do `main` se web automaticky aktualizuje.
- Nové změny jsou vidět na webu.

---

## Rollback
- Vrať poslední commit nebo workflow v GitHub Actions.
