# pawcho_z2
Repozytorium na zadanie 2 z przedmiotu Programowanie Aplikacji w Chmurze Obliczeniowej.

Stworzone zostały zmienne oraz secret.

Do repozytorium skopiowano pliki z zadania 1 (src/, web/). 

**Zmodyfikowano plik Dockerfile, żeby współpracował z Github Actions:**

```
# Kopiujemy kod źródłowy z GitHuba (dostarczony przez akcję checkout) prosto do kontenera
COPY src/ ./src/
```

```
COPY web/ /web/
```

**Stworzono łańcuch Github Actions:**

Nazwa łańcucha - Pipeline. Automatyzuje on proces budowy, skanowania i publikacji do GHCR. Wykorzystano środowisko ubuntu, emulator QEMU oraz Docker Buildx.

Proces budowania obrazu wspiera dwie architektury sprzętowe: linux/amd64 oraz linux/arm64.

Dane cache są przechowywane w zewnętrznym, publicznym repozytorium na DockerHub. Wykorzystano eksport typu registry w trybie mode=max. Tryb ten gwarantuje, że cache obejmuje wszystkie warstwy.

Do wykonania testu bezpieczeństwa wybrano skaner Trivy. Zdecydowano się na to narzędzie ze względu na bezproblemową integrację ze środowiskiem GitHub Actions bez konieczności dodatkowego uwierzytelniania w płatnych usługach.

Tagowanie obrazów: do zarządzania tagami użyto oficjalnego mechanizmu docker/metadata-action. Obrazy są tagowane w oparciu o dwa schematy: 
* Skrócony hash commita - wykorzystano domyślny format, który automatycznie dodaje przedrostek *sha* i skraca hash do 7 znaków. Zapewnia to identyfikowalność kodu (traceability). Pozwala powiązać kontener w chmurze z dokładnym stanem kodu źródłowego w repozytorium (https://docs.docker.com/guides/reactjs/configure-github-actions/).

* semver - mapuje wersje nadawane w Git na oficjalne tagi obrazu. Pozostawiono włączone domyślne zachowanie, generujące tag latest - pobranie obrazu bez wersji pobiera najnowszy obraz. 



