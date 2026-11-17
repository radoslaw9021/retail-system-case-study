# System operacyjny dla sieci 3 sklepów

**Case study.** Nazwa firmy i dane osobowe są pominięte. Kod jest prywatny, bo system działa na produkcji i przetwarza dane pracowników. Tutaj opisuję problem, rozwiązanie, architekturę i efekty. Demo na żywo pokazuję na rozmowie.

> Zbudowany **samodzielnie, od zera, od lutego 2026**. Używany codziennie przez zespół trzech sklepów i administratorów.

---

## Problem

- Grafik pracy układany ręcznie, a luki w obsadzie łatane telefonicznie: ok. **3 godziny pracy miesięcznie**.
- Obecność zapisywana na papierowych listach, godziny do wypłat liczone ręcznie.
- **Brak danych** o tym, kto ile realnie sprzedaje na godzinę pracy.
- Duża rotacja: każdą nową osobę trzeba było wdrażać od zera, dokumenty i umowy leżały w szufladzie.

## Rozwiązanie

Jeden spójny system, ok. **18 modułów**:

| Obszar | Moduły |
|---|---|
| Ludzie i czas pracy | grafik z planowaniem według wydajności, dyspozycje, ewidencja czasu z kioskiem odbić kart (wejście, przerwa, wyjście), liczenie godzin |
| Sprzedaż | wskaźnik **zł na godzinę** liczony automatycznie z danych sprzedaży i ewidencji, panel wydajności |
| Rekrutacja i kadry | moduł rekrutacji z **automatycznym przetwarzaniem CV w Pythonie**, ankiety pracownicze, generator umów |
| Codzienna operacja | zadania i checklisty, reklamacje i zwroty, inwentaryzacja skanerem, rozliczenie kasy, obieg kluczy na kodach QR |
| Wiedza i szkolenia | baza wiedzy „jak sprzedawać i pracować”, quiz szkoleniowy z elementami grywalizacji, onboarding |
| Komunikacja | powiadomienia push na telefony zespołu |

## Liczby

| | |
|---|---|
| **2 259** | zmian ułożonych w grafiku w 10 miesięcy |
| **~1 450** | rejestracji czasu pracy miesięcznie |
| **74** | urządzenia z zainstalowaną aplikacją |
| **~30 / 90+** | osób na co dzień / kont w ciągu roku (duża rotacja) |
| **150+** | kandydatek obsłużonych przez moduł rekrutacji |
| **23–26** | osób, dla których co miesiąc liczony jest wskaźnik zł/h |

**Uczciwie:** nie twierdzę, że sprzedaż wzrosła, bo dane są sezonowe. Twierdzę, że **wydajność stała się mierzalna** tam, gdzie wcześniej nie było żadnej miary.

## Przedtem → teraz

| Przedtem | Teraz |
|---|---|
| grafik ręcznie, luki łatane telefonicznie ~3 h/mies. | dyspozycje i grafik w aplikacji, obsada widoczna od razu |
| obecność na papierowych listach | ok. 1 450 odbić karty miesięcznie, godziny do wypłat liczone z danych |
| brak danych o sprzedaży na osobę | wskaźnik zł/h dla każdej osoby, pierwszy raz w historii firmy |
| ręczne przeglądanie CV | skrypt wyciąga dane z PDF i porządkuje zdjęcia kandydatów |
| dokumenty w szufladzie | ankiety, umowy i terminy w systemie |

## Technika

- **Frontend:** aplikacja SPA w czystym JavaScript, działa na telefonach zespołu w sklepach i na komputerach administratorów
- **Backend:** PHP + MySQL, REST API (JSON), logowanie wspólne dla kilku modułów z centralnym wyłączaniem dostępów
- **Automatyzacje:** Python (PyMuPDF do ekstrakcji danych z CV, OpenCV i usuwanie tła do zdjęć), powiadomienia Web Push
- **Bezpieczeństwo danych:** odbicia kart chronione GPS i NFC; dane krytyczne (ewidencja, wypłaty, logowania) zabezpieczone przed utratą i nadpisaniem: operacje atomowe, blokady, kopie zapasowe przed każdym wdrożeniem
- **Sposób pracy:** AI-first (Cursor, potem Claude Code), z czytaniem i weryfikacją każdego fragmentu kodu przed wdrożeniem

## Czego się nauczyłem

- **Najpierw rozmowa, potem kod.** Najlepsze moduły powstały z pytania „co Was najbardziej wkurza w tej pracy?” zadanego zespołowi.
- **Dane krytyczne wymagają zabezpieczeń od pierwszego dnia.** Po jednym incydencie z utratą danych dodałem blokady i kopie zapasowe przed każdym wdrożeniem.
- **Wdrożenie to też szkolenie ludzi.** Sam uczyłem zespół korzystania z systemu, przy dużej rotacji to proces ciągły.

---

📫 Kontakt: tymon.radek@gmail.com · [Profil GitHub](https://github.com/radoslaw9021)
