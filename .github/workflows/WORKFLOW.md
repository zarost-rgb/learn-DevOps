# Dokumentacja GitHub Actions Workflow

Plik `.github/workflows/advanced-ci.yml` definiuje zaawansowany pipeline CI/CD obsługujący warunkowe etapy budowania, testowania i wdrażania.

## Wyzwalacze (Triggers)
* **push**: Uruchamia pipeline dla gałęzi `main` oraz `develop`.
* **pull_request**: Uruchamia pipeline dla Pull Requestów kierowanych do `main`.

## Zmienne środowiskowe (env)
* `ENVIRONMENT`: Określa docelowe środowisko (`production`).
* `BUILD_DIR`: Katalog wyjściowy dla artefaktów (`dist`).

## Zadania (Jobs)
1. **build**:
   * Kompiluje projekt i tworzy pliki wyjściowe.
   * Artefakty są zapisywane (`upload-artifact`) **tylko** для gałęzi `main`.
2. **test**:
   * Uruchamia się po zakończeniu joba `build`.
3. **deploy**:
   * Uruchamia się **TYLKO** przy push do gałęzi `main`.
4. **status_report**:
   * Выполняется всегда на финише и показывает итоговые статусы.

---

## Uruchamianie lokalne z użyciem Docker (Opcjonalnie)

Абы przetestować workflow lokalnie без отправки изменений на GitHub:
```bash
act push -b main
