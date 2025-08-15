# How-to: CI/CD – Mike pro verze dokumentace

!!! info "Účel"
    Nasazovat a spravovat více verzí dokumentace pomocí nástroje mike a GitHub Actions.

## Předpoklady
- Repo na GitHubu
- MkDocs, mkdocs-material a mike v projektu
- Nastavený GitHub Pages (branch `gh-pages`)

---

## Kroky
1. Nainstaluj mike (`pip install mike`).
2. Pro novou verzi spusť:
   ```bash
   mike deploy --push --update-aliases 0.2 latest
   ```
   (nahraď číslo verze dle potřeby)
3. Ověř, že se v menu objevil přepínač verzí.
4. Pro rollback použij starší alias nebo nasazení.

---

## Ověření
- Web nabízí přepínání mezi verzemi (latest, 0.1, 0.2…)
- Nová verze je dostupná na webu.

---

## Rollback
- Nasazení předchozí verze pomocí mike nebo GitHub Actions.
