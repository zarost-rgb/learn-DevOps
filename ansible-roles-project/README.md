# Projekt Ansible: Refaktoryzacja Stacku LAMP

Projekt automatyzacji wdrażania stosu LAMP (Linux, Apache, MySQL, PHP) z wykorzystaniem ról Ansible, uporządkowaną hierarchią zmiennych, obsługą wielu środowisk oraz najlepszymi praktykami inżynierskimi.

## Struktura projektu

```text
.
├── ansible.cfg              # Główny plik konfiguracyjny Ansible
├── inventories/             # Katalog środowisk wdrożeniowych
│   ├── dev/                 # Środowisko deweloperskie (development)
│   │   └── hosts.ini
│   └── prod/                # Środowisko produkcyjne (production)
│       └── hosts.ini
├── group_vars/              # Zmienne globalne dla wszystkich hostów
│   └── all.yml
├── roles/                   # Role Ansible (odpowiedzialne za konkretne komponenty)
│   ├── apache/              # Konfiguracja serwera WWW Apache
│   ├── mysql/               # Instalacja i konfiguracja MySQL (z obsługą błędów block/rescue)
│   └── php/                 # Instalacja PHP wraz z wymaganymi modułami
├── site.yml                 # Główny playbook spinający projekt z systemem tagów
└── README.md                # Dokumentacja projektu

## Wymagania wstępne
* Zainstalowany Ansible (wersja >= 2.10)
* Docelowy system operacyjny: Ubuntu / Debian

## Instrukcja uruchamiania

1. **Uruchomienie pełnego playbooka dla środowiska deweloperskiego (dev):**
   ```bash
   ansible-playbook site.yml -i inventories/dev/hosts.ini

2. **Wykonanie tylko konfiguracji bazy danych MySQL:**
    '''bash
   ansible-playbook site.yml -i inventories/dev/hosts.ini --tags mysql

3. **Wykonanie zadań związanych z warstwą webową (Apache i PHP):**
   '''bash
   ansible-playbook site.yml -i inventories/dev/hosts.ini --tags web

4. **Uruchomienie całości z pominięciem bazy danych:**
   '''bash
   ansible-playbook site.yml -i inventories/dev/hosts.ini --skip-tags mysql

## Wprowadzone usprawnienia i dobre praktyki
Podział na środowiska: Izolacja konfiguracji dla środowiska testowego (dev) oraz produkcyjnego (prod).

Hierarchia zmiennych: Parametry konfiguracyjne i dane dostępowe zostały wyniesione do folderu group_vars/.

Bezpieczeństwo i obsługa błędów: Krytyczne zadania w role MySQL zostały zabezpieczone konstrukcjami block / rescue / always, co pozwala kontrolować sytuacje awaryjne.

Granularność dzięki tagom: Możliwość uruchamiania fragmentów kodu bez konieczności czekania na ponowne przejście całego procesu (idempotentność i oszczędność czasu).
