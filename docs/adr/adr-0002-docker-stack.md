# ADR 0002: Docker stack jako výchozí prostředí

## Kontext
Pro laboratorní a výukové účely je potřeba mít opakovatelně nasaditelné, izolované a snadno čistitelné prostředí pro OT/IT integraci.

## Rozhodnutí
Jako základní prostředí bude použit Docker stack (Docker Desktop + WSL2 na Windows 11) s docker-compose pro orchestraci služeb.

## Důvody
- Rychlá instalace a odstranění bez zásahu do hostitelského systému
- Snadná replikace prostředí na jiném počítači
- Možnost verzovat konfiguraci v Gitu
- Podpora pro běh na Windows, Linux i Mac
- Možnost využít GPU (NVIDIA) pro AI/ML kontejnery

## Alternativy
- Lokální instalace všech služeb přímo na OS (vyšší riziko zaneřádění, složitější úklid)
- Virtuální stroje (vyšší nároky na HW, pomalejší start)

## Důsledky
- Všechny návody a projekty budou vycházet z Docker prostředí
- Uživatelé musí mít možnost spustit Docker Desktop a WSL2
- Pro pokročilé scénáře lze rozšířit o další kontejnery/služby
