# Struktura projektu
# Projekt składa się z następujących ról:

ansible-lamp/
├── inventory.ini
├── site.yml
├── README.md
└── roles/
    ├── base/        # Podstawowa konfiguracja systemu i narzędzia
    ├── database/    # Instalacja i konfiguracja MySQL / MariaDB
    ├── php/         # Instalacja PHP i wymaganych rozszerzeń
    ├── web/         # Instalacja i konfiguracja serwera Apache
    └── app/         # Wdrożenie aplikacji PHP i konfiguracja połączenia z DB

# Opis ról i ich działania
# 1) base:  Co robi: Przygotowuje system operacyjny. Aktualizuje cache menedżera pakietów oraz instaluje podstawowe narzędzia administracyjne (curl, git), niezbędne do dalszej pracy.
# 2) database: Co robi, Instaluje serwer baz danych MySQL (lub MariaDB) oraz biblioteki Pythona wymagane przez moduły Ansible. Następnie uruchamia usługę, tworzy dedykowaną bazę danych dla aplikacji oraz użytkownika z odpowiednimi uprawnieniami.
# 3) php: Co robi, Instaluje interpreter PHP oraz kluczowe pakiety i rozszerzenia integrujące PHP z serwerem Apache (libapache2-mod-php) oraz z bazą danych MySQL (php-mysql).
# 4) web: Co robi, Instaluje serwer WWW Apache, zapewnia jego ciągłe działanie i włącza autostart przy uruchamianiu systemu. Wykorzystuje również handlery do automatycznego restartu Apache w przypadku zmian w konfiguracji.
# 5) app: Co robi, Wdraża przykładową aplikację PHP (plik index.php) w katalogu domyślnym serwera. Skrypt testuje połączenie z bazą danych i wyświetla status operacji, pobierając dane konfiguracyjne ze zmiennych roli.


Dostosowanie działania:

Każda rola posiada domyślne wartości w plikach defaults/main.yml, które możesz łatwo
dostosować do własnych potrzeb bez modyfikowania zadań.

Baza danych (roles/database/defaults/main.yml):

YAML
db_name: lamp_app_db              # Nazwa bazy danych
db_user: lamp_user                # Użytkownik bazy danych
db_password: secure_password_123  # Hasło użytkownika

Uruchomienie playbooka:
Bash
ansible-playbook site.yml --ask-become-pass
(Flaga --ask-become-pass jest wymagana do uzyskania uprawnień administratora sudo
na maszynie docelowej).

Po zakończeniu wdrożenia otwórz przeglądarkę internetową lub wpisz w terminalu: curl http://localhost
