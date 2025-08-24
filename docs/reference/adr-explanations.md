
# Co je ADR (Architecture Decision Record)

ADR (Architecture Decision Record) je stručný, formální zápis důležitých architektonických rozhodnutí v projektu. Koncept pochází od Michaela Nygarda (*Documenting Architecture Decisions*).

---

## Proč ADR

- Zvyšuje transparentnost a kontinuitu projektu
- Umožňuje rychlý onboarding nových členů
- Snižuje riziko opakovaných/duplicitních diskusí
- Umožňuje zpětně dohledat důvody a kontext rozhodnutí

---

## Jak strukturovat ADR

Doporučené sekce každého ADR:

- **Název** (stručný, výstižný)
- **Datum** (kdy bylo rozhodnutí přijato)
- **Kontext** (proč je rozhodnutí potřeba, okolnosti)
- **Problém** (co je hlavní otázka/problém)
- **Rozhodnutí** (jaké řešení bylo zvoleno a proč)
- **Alternativy** (jaké možnosti byly zvažovány a proč nebyly vybrány)
- **Důsledky** (pozitiva, negativa, rizika)
- **Realizace** (hlavní kroky implementace)
- **Ověření úspěchu** (jak poznáš, že rozhodnutí bylo úspěšně realizováno)
- **Rizika a mitigace** (hlavní rizika a jak je minimalizovat)
- **Následné kroky** (co je potřeba udělat dál)

---

## Jak psát ADR

- Piš stručně, ideálně na jednu stránku
- Každé rozhodnutí = samostatný soubor (adr-0001, adr-0002…)
- Čísluj ADR podle pořadí vzniku
- Piš ADR kdykoliv děláš zásadní rozhodnutí (ne až zpětně)
- Uváděj jen relevantní alternativy a důsledky

---

Podrobnou šablonu najdeš zde: [ADR Template](../adr/adr-template.md)

Externí zdroj: [Michael Nygard – Documenting Architecture Decisions](https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions.html)
