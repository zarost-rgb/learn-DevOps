# Procedura Współpracy Zespołowej (Git Flow)

Niniejszy dokument określa standardy pracy z repozytorium zdalnym (GitHub), procedury synchronizacji kodu oraz zasady tworzenia Pull Requestów w zespole DevOps.

---

## 1. Procedura wysłania zmian (Push)

Wszelkie prace programistyczne i konfiguracyjne realizowane są na dedykowanych gałęziach funkcjonalnych (`feature/*`). Zabrania się bezpośredniego pushowania kodu do gałęzi `main` oraz `develop`.

**Kroki do wykonania przed wysłaniem zmian:**
1. Upewnij się, że zmiany zostały lokalnie zatwierdzone (commit):
   ```bash
   git status
   git add <nazwa_pliku>
   git commit -m "Typ: Krótki opis zmian"
