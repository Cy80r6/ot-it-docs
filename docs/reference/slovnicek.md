# Slovníček pojmů

## ADR (Architecture Decision Record)

**ADR** je zkratka pro *Architecture Decision Record* — stručný zápis, kde si uchováváš důležitá architektonická rozhodnutí v projektu.

### K čemu slouží ADR?
- Umožňuje zpětně dohledat, proč a jaká rozhodnutí byla v projektu učiněna.
- Pomáhá týmu i budoucím členům pochopit kontext, důvody a alternativy.
- Zvyšuje transparentnost a snižuje riziko opakování stejných diskusí.

### Co by měl ADR obsahovat?
- **Název rozhodnutí** (stručný, výstižný)
- **Datum** a případně verzi projektu
- **Kontext**: co jsme potřebovali řešit, jaké byly okolnosti
- **Rozhodnutí**: co bylo vybráno a proč
- **Alternativy**: jaké další možnosti byly zvažovány a proč nebyly vybrány
- **Důsledky**: pozitiva, negativa, dopady na provoz, údržbu, bezpečnost apod.

### Příklad (Sprint 1 – „ADR: Volba stacku“)
- Vybral jsem stack: MkDocs Material + GitHub Pages + mike
- Kontext: Potřeba rychlé tvorby dokumentace, auto-deploy, verzování
- Rozhodnutí: Použít MkDocs Material, hostovat na GitHub Pages, verzovat s mike
- Alternativy: Docusaurus, Hugo, Cloudflare Pages
- Důsledky: Rychlé nasazení, nízká režie, omezené UI možnosti
- Datum: [datum rozhodnutí]

### Proč je ADR důležitý?
- Umožní ti i po roce pochopit, proč jsi něco udělal právě takto.
- Usnadňuje onboarding nových členů týmu.
- Pomáhá při změnách architektury — víš, na co si dát pozor.

> **Tip:** Piš ADR stručně, ale jasně. Každé větší rozhodnutí si zaslouží vlastní zápis.

---

*Pokud potřebuješ další pojmy do slovníčku, stačí říct!*
