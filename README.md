# SZWR – System Zarządzania Wypożyczalnią Rowerową

Aplikacja internetowa dla małych i średnich wypożyczalni rowerów. Obsługuje rowery i akcesoria (kaski, foteliki, sakwy itp.), rezerwacje, wydania, zwroty, rozliczenia i raporty.

Projekt zaliczeniowy z przedmiotu „Projektowanie i programowanie aplikacji i systemów biznesowych”, Uniwersytet DSW Ideis Wrocław.

## Architektura

Aplikacja trójwarstwowa:

```
Next.js (przeglądarka)  --HTTPS, REST/JSON-->  FastAPI  --SQLAlchemy-->  PostgreSQL
```

- **Frontend** – Next.js (React, TypeScript): część dla klienta oraz panele pracownika, administratora i właściciela.
- **Backend** – Python, FastAPI: REST API, logika biznesowa, walidacja danych (Pydantic).
- **Baza danych** – PostgreSQL, dostęp przez SQLAlchemy, migracje w Alembic.
- **Uwierzytelnianie** – tokeny JWT, hasła haszowane (bcrypt), uprawnienia według ról sprawdzane po stronie API.

## Moduły backendu

| Moduł | Zakres |
|---|---|
| Konta i role | rejestracja, logowanie, role: klient, pracownik, administrator, właściciel |
| Katalog | rowery, akcesoria, kategorie, cennik |
| Rezerwacje | sprawdzanie dostępności w terminie, blokada podwójnej rezerwacji |
| Wypożyczenia i zwroty | rejestracja wydania i zwrotu, historia klienta |
| Rozliczenia | naliczanie opłat i kar, rejestracja płatności |
| Serwis | status roweru, zgłoszenia usterek |
| Raporty | przychody, popularność rowerów |

## Model danych

Główne tabele: `uzytkownicy`, `klienci`, `rowery`, `akcesoria`, `rezerwacje`, `wypozyczenia`, `pozycje` (sprzęt w rezerwacji lub wypożyczeniu), `platnosci`, `zgloszenia_serwisowe`.

Rowery są ewidencjonowane pojedynczo (każdy egzemplarz ma własny rekord i status), akcesoria ilościowo.

## Reguły biznesowe

- Dostępność wynika z rezerwacji i wypożyczeń nakładających się na wybrany termin, a nie z samego statusu roweru.
- Rozliczenie dobowe: liczba dni × cena za dzień, każdy rozpoczęty dzień liczony jak pełny.
- Zwrot po terminie: stawka dzienna + 50% za każdy dzień opóźnienia (wartość konfigurowalna).
- Płatność w punkcie (gotówka lub karta), system tylko ją rejestruje.
- Rezerwacja nieodebrana w dniu rozpoczęcia jest anulowana.

## Planowana struktura repozytorium

```
backend/      # FastAPI: routery, modele SQLAlchemy, schematy Pydantic, migracje Alembic
frontend/     # Next.js
docs/         # dokumentacja i diagramy UML
docker-compose.yml
```

## Narzędzia

Git, Docker Compose, pytest, Swagger UI (dokumentacja API generowana przez FastAPI).

## Status

Etap analizy wymagań. Gotowe są rozdziały 1 i 2 dokumentacji (wymagania, diagram komponentów, diagram aktywności, diagram przypadków użycia). Kod i instrukcja uruchomienia pojawią się w kolejnych etapach.

Dokumentacja: [SZWR_dokumentacja.docx](SZWR_dokumentacja.docx)

## Autorzy

- David Alsheuski (55967)
- Kamil Biesiada (43089)
- Patryk Hoffmann-Guzik (40346)

Prowadzący: dr inż. Konrad Kluwak
