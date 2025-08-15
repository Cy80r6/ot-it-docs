# ADR 0001 — Volba stacku: MkDocs Material + GitHub Pages + mike

!!! info "Status"
    **Přijato** • Datum: <!--  2025‑08‑15 --> • Verze projektu: <!-- 0.1 -->

## Kontext
- Potřebujeme **rychle psát dokumentaci** k OT/IT projektům v čistém Markdownu.
- Cíl: **nízká režie**, **auto‑deploy** z GitHubu, **vyhledávání**, **diagramy (Mermaid)**, a **verzování** dokumentace.
- Skrz roadmapu míříme k profilu *OT/IT Solution Architect*; dokumentace má být **opakovatelné how‑to**, **projekty**, **ADR** a **playbooky**.

## Problém
Jaký **publikační stack** zvolit, aby byl:
- jednoduchý na údržbu (žádný server),
- rychlý na psaní (Markdown, minimum build magie),
- srozumitelný diff v Gitu,
- připravený na **verzování** (uvolňování 0.1, 0.2… bez lámání starých stránek).

## Rozhodnutí
Volím **MkDocs Material** pro generování webu + **GitHub Pages** pro hosting + **mike** pro verzování dokumentace.
- Build a publish běží přes GitHub Actions, větev `gh-pages`.
- Struktura: `projects/`, `how-to/`, `playbooks/`, `reference/`, `adr/`.
- Diagramy kreslím v ```mermaid blocích (Material je umí nativně).

## Alternativy (zvažované)
- **Docusaurus (React):** silné doc‑features, ale vyšší režie (Node/React, buildy, komponenty).
- **Hugo / Astro:** velmi rychlé generátory, ale méně „out‑of‑the‑box“ doc‑komfortu; víc rozhodování o šablonách.
- **Cloudflare Pages:** moderní hosting, ale GitHub Pages je „nejkratší cesta“ (Actions + permissions už vyřešeny).
- **Sphinx:** skvělý pro Python ekosystém, horší UX pro běžný Markdown a méně „plug‑and‑play“ vzhled.

## Důsledky
**Pozitiva**
- Markdown-first, nízká mentální i infrastrukturní režie.
- Rychlý start, hezké výchozí UI, vyhledávání, call‑outy, Mermaid.
- **mike** umožní držet **více verzí** webu (0.1, latest…).

**Negativa / trade‑offs**
- Omezenější custom UI vs. React řešení.
- Pokročilé interakce (custom komponenty) znamenají extra práci.
- Verzování přes `mike` vyžaduje disciplínu v release workflow.

## Realizace (high‑level)
1. **CI/CD:** workflow `.github/workflows/deploy.yml` — build MkDocs, publish do `gh-pages`.
2. **Pages:** Settings → Pages → *Deploy from a branch* → `gh-pages` → `/`.
3. **Struktura obsahu:** složky výše, konvence názvů `kebab-case.md`.
4. **Verzování:** `mike deploy 0.1 latest` (manuálně nebo separátním workflow).
5. **Kvalita obsahu:** každá stránka má *Účel, Kroky, Ověření, Rollback* (Feynman, měřitelnost).

## Ověření úspěchu
- Po pushi do `main` se web během chvíle aktualizuje na GitHub Pages.
- Full‑text vyhledávání funguje; Mermaid diagramy se renderují.
- `mike` umí nasadit verzi **0.1** a držet alias **latest**.
- Minimálně 3 stránky (How‑to, Project, ADR) jsou publikovány.

## Rizika a mitigace
- **„Míšmaš strategií deploye“** (branch vs. artifact) → držet se **`gh-pages`** a `mkdocs gh-deploy`.
- **Cache/CDN klame** → tvrdý refresh, cache‑buster `?_cb=YYYYMMDDHHMM`.
- **Vendor lock‑in na Material theme** → minimalizovat vendor specifika v obsahu, držet čistý Markdown.

## Následné kroky
- Přidat workflow na **release verze** (`mike`) s inputem verze.
- Založit **style‑guide** (nadpisy, tagy, obrázky, diagramy).
- Vytvořit **šablony**: Project, How‑to, ADR (re‑use kousky).

---

**Poznámky**
- Autor: <!-- C023 -->
- Související: Roadmapa OT/IT, Sprint 1, CI/CD stránka (`ci-cd-mkdocs-pages.md`)
