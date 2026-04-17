# WorkshopManager (ProjektNet)

Aplikacja webowa zbudowana w technologii **ASP.NET Core**, służąca do zarządzania warsztatem samochodowym. Umożliwia koordynację pracy zespołu, zarządzanie zleceniami, częściami oraz relacjami z klientami.

## Wykorzystane technologie
- **Framework:** ASP.NET Core (MVC / Razor Pages)
- **Mapowanie obiektów:** [Mapperly](https://mapperly.github.io/mapperly/)
- **Generowanie dokumentów:** QuestPDF (raporty PDF)
- **Procesy w tle:** Hosted Services / BackgroundService
- **CI/CD:** GitHub Actions

## Funkcjonalności aplikacji

Aplikacja opiera się na architekturze ról, ograniczając dostęp do modułów w zależności od uprawnień zalogowanego użytkownika.

### System Ról i Uprawnień

* **Klient** 
    * Przeglądanie historii własnych napraw.
    * Dodawanie zdjęć do swoich pojazdów.
    * Dodawanie komentarzy do realizowanych zleceń.
* **Recepcjonista**
    * Rejestracja nowych klientów i dodawanie ich pojazdów.
    * Tworzenie nowych zleceń naprawy.
    * Łączenie fizycznych profili klientów z ich internetowymi kontami w aplikacji.
    * Zarządzanie inwentarzem (aktualizacja stanów magazynowych części).
    * Generowanie indywidualnych raportów PDF z historii napraw dla klientów.
* **Mechanik**
    * Przegląd i aktualizacja statusu przypisanych zleceń.
    * Rejestrowanie wykonanych czynności naprawczych.
    * Deklarowanie użytych części w ramach konkretnego zlecenia.
* **Admin**
    * Dostęp do wszystkich funkcji pozostałych ról.
    * Zarządzanie kontami użytkowników i nadawanie im ról.
    * Zarządzanie głównym katalogiem części (dodawanie nowych pozycji).
    * Generowanie globalnych, miesięcznych raportów serwisowych PDF.

## Struktura projektu

Projekt zachowuje strukturę opartą o wzorce architektoniczne ASP.NET:

```text
/WorkshopManager
├── Controllers/         # Kontrolery obsługujące żądania i logikę biznesową
├── DTOs/                # Obiekty transferu danych (Data Transfer Objects)
├── Models/              # Modele domenowe i encje bazodanowe
├── Services/            # Logika biznesowa i usługi (w tym Background Services)
├── Mappers/             # Konfiguracje mapowań obiektów (Mapperly)
├── PdfRaports/          # Klasy odpowiedzialne za generowanie dokumentów za pomocą QuestPDF
├── Pages/               # Widoki Razor Pages dla dedykowanych funkcjonalności
├── Views/               # Widoki Razor dla kontrolerów MVC
├── wwwroot/             # Pliki statyczne
│   └── uploads/         # Katalog przechowujący wgrane zdjęcia pojazdów
├── Data/                # Kontekst bazy danych (DbContext)
└── Program.cs           # Konfiguracja potoku aplikacji i wstrzykiwania zależności (DI)
```

## Usługi działające w tle (Background Services)

Aplikacja posiada wbudowany system powiadomień. Wykorzystując `OpenOrderReportBackgroundService`, system automatycznie generuje i wysyła zbiorczy raport e-mail (np. podsumowanie otwartych zleceń) co określoną jednostkę czasu (co godzinę). 
*(Uwaga: Adres docelowy e-mail można skonfigurować w systemie).*

## CI/CD - GitHub Actions

Repozytorium posiada w pełni zautomatyzowany potok CI/CD. Workflow uruchamia się automatycznie po każdym `push` lub włączeniu `pull request` do gałęzi `main`.

Kroki potoku:
1. `dotnet restore` – pobranie i przywrócenie wymaganych pakietów NuGet.
2. `dotnet build` – kompilacja aplikacji z upewnieniem się, że kod jest wolny od błędów budowania.
3. `dotnet test` – uruchomienie zestawu testów jednostkowych.

---

## Uruchomienie lokalne

Aby uruchomić projekt na swoim komputerze, postępuj zgodnie z poniższymi instrukcjami:

1. **Sklonuj repozytorium:**
   ```bash
   git clone [https://github.com/tostrowski8080/ProjektNet.git](https://github.com/tostrowski8080/ProjektNet.git)
   cd ProjektNet/WorkshopManager
   ```

2. **Skonfiguruj bazę danych:**
   Zmień connection string w pliku `appsettings.json`, wskazując na swój lokalny serwer SQL:
   ```json
   "ConnectionStrings": {
     "DefaultConnection": "Server=TWÓJ_SERWER;Database=WorkshopManagerDb;Trusted_Connection=True;MultipleActiveResultSets=true"
   }
   ```

3. **Zastosuj migracje bazy danych:**
   ```bash
   dotnet ef database update
   ```

4. **Uruchom aplikację:**
   ```bash
   dotnet run
   ```
