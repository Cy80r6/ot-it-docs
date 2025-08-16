# How-to: Instalace Docker + Compose

Tento návod popisuje instalaci Docker Desktop a Docker Compose na Windows 10/11 s WSL2.

## Předpoklady
- Windows 10 (build 19041+) nebo Windows 11
- Administrátorský přístup
- Stabilní internet

## Postup
1. Otevři PowerShell jako správce a spusť:
   ```powershell
   wsl --install
   ```
2. Po restartu spusť nově nainstalovanou Linux distribuci (např. Ubuntu) a nastav uživatelské jméno a heslo.
3. Stáhni a nainstaluj Docker Desktop z [docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop/).
4. Během instalace zaškrtni "Use the WSL 2 based engine".
5. Po instalaci spusť Docker Desktop a v Settings → Resources → WSL Integration povol svou Linux distribuci.
6. Ověř instalaci:
   ```powershell
   docker version
   docker compose version
   docker run hello-world
   ```

## Rollback / Odinstalace
- Odinstaluj Docker Desktop z Nastavení → Aplikace
- Odinstaluj Linux distribuci přes Microsoft Store → Moje knihovna
- Vypni WSL a Virtual Machine Platform:
   ```powershell
   dism.exe /online /disable-feature /featurename:Microsoft-Windows-Subsystem-Linux
   dism.exe /online /disable-feature /featurename:VirtualMachinePlatform
   ```
