#  GIT_WORKFLOW.md

## 1. Diagram Git Flow 

Poniższy schemat przedstawia pełny cykl życia gałęzi oraz ich wzajemne powiązania zrealizowane w projekcie:

```text
main           -----------------------------------------------> [v1.0.0] ---------> [Merge Hotfix] ->
                                                                  /                 ^
hotfix/        --------------------------------------------------/---> [Fix] ------/
                                                                /
release/       ------------------------------------> [v1.0.0] -/
                                                       ^
develop        [Initial] -> [Merge/mon] -> [Merge/log] --------> [Fast-forward po hotfixie] --->
                             /               /
feature/mon    ------------/                /
                                           /
feature/log    ---------------------------/


# 2. Procedury dla każdego typu gałęzi 
 main – Główna gałąź produkcyjna. Zawiera wyłącznie stabilny, przetestowany kod gotowy dla użytkowników końcowych. Każde wdrożenie jest oznaczane tagiem wersji (np. v1.0.0).

 develop – Główna gałąź rozwojowa (konwojer). To tutaj integrowane są wszystkie nowe funkcjonalności i stąd przygotowuje się wersje testowe.

 feature/* – Gałęzie lokalne do tworzenia nowych funkcji (np. feature/monitoring). Tworzone od develop, a po zakończeniu prac scalane z powrotem do develop.

 release/* – Gałęzie przygotowujące wydanie nowej wersji systemu. Służą do zamrożenia kodu i zmiany numeracji (np. w pliku VERSION).

 hotfix/* – Awaryjne gałęzie do naprawiania krytycznych błędów bezpośrednio na produkcji. Tworzone od main, a po naprawie scalane do main oraz develop.


# 3. Wykorzystane polecenia (Krok po kroku) 
Krok 1: Inicjalizacja i konfiguracja
Bash
mkdir ~/devops_project && cd ~/devops_project
git init
git config user.name "Dev Student"
git config user.email "dev@learning.com"
git checkout -b develop
echo "# DevOps Project" > README.md
git add README.md
git commit -m "Initial commit on develop"


Krok 2: Praca nad funkcjonalnościami (Gałęzie feature)
Bash
# Feature 1 - Monitoring
git checkout -b feature/monitoring develop
echo "monitoring_config = {}" > monitoring.py
git add monitoring.py
git commit -m "Add monitoring module"

# Feature 2 - Logging
git checkout develop
git checkout -b feature/logging develop
echo "## Logging" >> README.md
git add README.md
git commit -m "Add logging documentation"


Krok 3: Integracja gałęzi feature
Bash
git checkout develop
git merge feature/monitoring
git merge feature/logging  # Tutaj zatwierdzono komunikat scalenia

# Krok 4: Proces wydawniczy (Release) i poprawka błędów struktury
Bash
git checkout -b realease/1.0.0 develop  # Utworzenie gałęzi (z literówką)
echo "1.0.0" > VERSION
git add VERSION
git commit -m "Release 1.0.0"

# Naprawa braku gałęzi main i scalenie
git checkout -b main
git merge --no-ff realease/1.0.0
git tag -a v1.0.0 -m "Version 1.0.0"


# Krok 5: Hotfix (Pilna naprawa produkcji)
Bash
git checkout -b hotfix/critical-bug main
echo "# Bug fix" > bugfix.md
git add bugfix.md
git commit -m "Critical security patch"

# Scalenie do produkcji (main) oraz do rozwoju (develop)
git checkout main
git merge --no-ff hotfix/critical-bug
git checkout develop
git merge hotfix/critical-bug


# 4. Wyjaśnienie konfliktów i napotkanych problemów ️
Podczas realizacji zadania napotkano trzy kluczowe problemy techniczne:

 Brak gałęzi main: Przy próbie przełączenia się na produkcję (git checkout main), Git zgłosił błąd, ponieważ repozytorium zaczynało pracę wyłącznie od develop.
 Rozwiązanie: Użyto git checkout -b main, aby dynamicznie utworzyć gałąź produkcyjną na bazie aktualnego stabilnego kodu.

✏️ Literówka w nazwie gałęzi: Przypadkowo utworzono gałąź realease/1.0.0 zamiast release/1.0.0. Rozwiązanie: Ponieważ Git dopasowuje nazwy dokładnie, konsekwentnie użyto nazwy z literówką
 w poleceniach scalania, co pozwoliło bez przeszkód dokończyć procedurę Git Flow.

 Obsługa edytora tekstowego nano: Podczas używania flagi --no-ff przy operacjach merge, Git automatycznie uruchamiał wewnątrz terminala edytor nano w celu zatwierdzenia wiadomości commitu.
 Rozwiązanie: Edytor zamykano sekwencją klawiszy Ctrl + O (zapis), Enter (potwierdzenie) oraz Ctrl + X (wyjście).


# 5. Oczekiwany rezultat
Kompletne, w pełni funkcjonalne repozytorium Git, które odwzorowuje zaawansowany cykl pracy DevOps (Git Flow). Wszystkie zmiany zostały poprawnie zabezpieczone,
 wersja produkcyjna oznaczona tagiem v1.0.0, a poprawka bezpieczeństwa (hotfix) pomyślnie zsynchronizowana między środowiskiem produkcyjnym a deweloperskim.
