# Obywatel BIELIK DB

Repozytorium zawierające skrypty, migracje i definicje bazy danych dla projektu Obywatel BIELIK.

## Opis projektu

Obywatel BIELIK to aplikacja służąca do gromadzenia opisów zdjęć (anotacji) przez społeczność, w tym seniorów i lokalnych aktywistów. To repozytorium przechowuje wszystkie skrypty związane z bazą danych PostgreSQL, która obsługuje aplikację.

## Struktura bazy danych

Baza danych składa się z następujących głównych tabel:

1. **USERS** - Informacje o użytkownikach aplikacji
2. **TEAMS** - Zespoły, do których mogą dołączać użytkownicy 
3. **WEEKLY_GOALS** - Cele tygodniowe dostępne dla użytkowników
4. **USER_WEEKLY_GOALS** - Przypisanie celów tygodniowych do użytkowników
5. **IMAGES** - Dane zdjęć dostępnych w aplikacji
6. **ANNOTATIONS** - Opisy narracyjne i faktograficzne zdjęć
7. **ACHIEVEMENTS** - Osiągnięcia dostępne w aplikacji
8. **USER_ACHIEVEMENTS** - Przypisanie osiągnięć do użytkowników
9. **USER_STATS** - Statystyki użytkowników

## Struktura projektu

```
obywatel-bielik-db/
├── migrations/                 # Migracje bazy danych (Alembic)
│   ├── versions/               # Wersje migracji
│   └── env.py                  # Konfiguracja środowiska migracji
├── schemas/                    # Definicje schematu bazy danych
│   ├── initial_schema.sql      # Pełny schemat bazy danych
│   └── tables/                 # Pojedyncze definicje tabel SQL
├── seeds/                      # Dane początkowe
│   ├── development/            # Dane dla środowiska deweloperskiego
│   └── production/             # Podstawowe dane dla środowiska produkcyjnego
├── scripts/                    # Skrypty pomocnicze
│   ├── backup/                 # Skrypty do tworzenia i przywracania kopii zapasowych
│   └── maintenance/            # Skrypty do utrzymania bazy danych
├── models/                     # Modele ORM (SQLAlchemy)
├── alembic.ini                 # Konfiguracja Alembic
└── docker-compose.yml          # Konfiguracja Docker Compose dla lokalnej bazy danych
```

## Technologie

- **Baza danych:** PostgreSQL 13+
- **Migracje:** Alembic
- **ORM:** SQLAlchemy (definicje w kodzie Pythona)
- **Konteneryzacja:** Docker i Docker Compose

## Schemat bazy danych

Diagram ERD przedstawiający relacje między tabelami znajduje się w pliku `docs/erd_diagram.png`.

## Ustawienie lokalnej bazy danych

### Wymagania

- Docker i Docker Compose
- Python 3.10+ (dla Alembic)

### Instalacja

1. Uruchom lokalną instancję PostgreSQL:

```bash
docker-compose up -d
```

2. Zainstaluj zależności Pythona:

```bash
pip install -r requirements.txt
```

3. Wykonaj migracje:

```bash
alembic upgrade head
```

4. (Opcjonalnie) Załaduj dane przykładowe:

```bash
python scripts/seed_development_data.py
```

## Migracje

### Tworzenie nowej migracji

```bash
alembic revision --autogenerate -m "opis zmian"
```

### Aktualizacja bazy danych

```bash
alembic upgrade head
```

### Cofnięcie ostatniej migracji

```bash
alembic downgrade -1
```

## Backupy

### Tworzenie kopii zapasowej

```bash
./scripts/backup/create_backup.sh
```

### Przywracanie kopii zapasowej

```bash
./scripts/backup/restore_backup.sh [nazwa_pliku_backupu]
```

## Integracja z API

Modele SQLAlchemy w katalogu `models/` są współdzielone z projektem API przez zależność pakietu. Dzięki temu definicje modeli są utrzymywane w jednym miejscu.

## Licencja

Projekt jest dostępny na licencji [licencja]. Szczegółowe informacje w pliku LICENSE.
