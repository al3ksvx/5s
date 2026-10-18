# HouseKeeping

Prosta aplikacja webowa (PWA) do obsługi housekeepingu w hotelu. Działa na iOS i Androidzie i można ją przypiąć do ekranu głównego. Jeden plik HTML, bez budowania i bez zależności (poza czcionkami Google).

## Funkcje

- **Konta:** logowanie imieniem, nazwiskiem i PIN-em (4 cyfry). Pierwsze logowanie to założenie konta. Urządzenie zapamiętuje zalogowaną osobę.
- **FS1, FS2, FS3:** pokoje jako kafelki. Statusy: posprzątany, wyjazd, wyjazd + przyjazd, wolny, rozebrany. Numery pokoi dodaje się pojedynczo lub zakresem (`101-120, 205`).
- **Godziny pracy:** suwaki początku i końca (co 30 minut), liczba posprzątanych pokoi, opcje POMOC, DYŻUR, INNE i opis.
- **Personel:** Administrator nadaje role, resetuje PIN-y i usuwa osoby.
- **Arkusz:** tabela godzin i pokoi (podsumowanie osób lub wszystkie wpisy) z filtrem miesiąca i osoby oraz eksportem do schowka i CSV.

## Role

| Rola | Uprawnienia |
|---|---|
| Administrator | wszystko, w tym Personel i Arkusz |
| Inspektor | wszystkie statusy pokoi, dodawanie i usuwanie pokoi, godziny wszystkich osób |
| Houseman | statusy „Rozebrany” i „Posprzątany” |
| Pokojowa | status „Posprzątany” |
| Oczekuje na rolę | brak dostępu do czasu nadania roli |

Pierwsze założone konto (gdy nie ma żadnego Administratora) zostaje Administratorem. Kolejne osoby czekają na rolę.

## Uruchomienie na GitHub Pages

1. Utwórz repozytorium i wgraj wszystkie pliki z tego folderu do głównego katalogu (razem z `.nojekyll`).
2. W repozytorium wejdź w **Settings → Pages**.
3. W **Build and deployment** wybierz **Deploy from a branch**, gałąź `main` i folder `/ (root)`.
4. Po minucie strona będzie pod adresem `https://TWOJ-LOGIN.github.io/NAZWA-REPOZYTORIUM/`.

Lokalnie wystarczy dowolny serwer statyczny, np. `python3 -m http.server 8000`. Service worker i instalacja działają tylko przez HTTPS lub `localhost`.

## Przypięcie do ekranu głównego

- **iPhone (Safari):** Udostępnij → Dodaj do ekranu początkowego.
- **Android (Chrome):** menu ⋮ → Zainstaluj aplikację (albo Dodaj do ekranu głównego).

## Ważne ograniczenie: gdzie są dane

Na GitHub Pages aplikacja zapisuje dane w pamięci przeglądarki (`localStorage`). Każdy telefon ma więc własną, osobną bazę: pokoje, konta i godziny nie synchronizują się między urządzeniami. W nagłówku widać napis „Tylko to urządzenie”.

Żeby wiele osób pracowało na wspólnych danych, trzeba podłączyć wspólną bazę (np. Firebase Firestore lub Supabase). Warstwa danych jest odizolowana w dwóch funkcjach, `makeLocal` i `makeDb` w `index.html`, więc podmiana magazynu nie wymaga zmian w reszcie kodu.

## Bezpieczeństwo

Role i PIN-y są sprawdzane w przeglądarce. To wygoda, nie zabezpieczenie: osoba techniczna z dostępem do danych może je obejść. PIN-y są przechowywane jako skrót z solą, ale 4 cyfry łatwo odgadnąć. Nie przechowuj w aplikacji danych wrażliwych.

## Pliki

| Plik | Opis |
|---|---|
| `index.html` | cała aplikacja (HTML, CSS, JavaScript) |
| `manifest.webmanifest` | dane instalacji PWA |
| `sw.js` | service worker (działanie bez sieci) |
| `icon-192.png`, `icon-512.png`, `apple-touch-icon.png` | ikony aplikacji |
| `.nojekyll` | wyłącza przetwarzanie Jekyll na GitHub Pages |
