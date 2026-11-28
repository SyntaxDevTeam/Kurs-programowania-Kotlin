# Kurs programowania Kotlin — pluginy Minecraft

**Od pierwszej linijki kodu do własnych pluginów Spigot i Paper.**

Kurs w języku polskim dla osoby, która **nigdy wcześniej nie programowała**. Nauka odbywa się w Kotlinie, krok po kroku, z ćwiczeniami i projektami związanymi z Minecraft Java Edition. Nie jest wymagana znajomość Javy, Pythona ani Paper API.

## Program kursu

**36 lekcji w 6 poziomach:**

| Poziom | Lekcje | Tematy |
|---|---:|---|
| I | 1–8 | Fundamenty języka Kotlin |
| II | 9–13 | Programowanie obiektowe |
| III | 14–16 | Gradle, rodzaje silników, **pierwszy plugin Spigot** |
| IV | 17–24 | Komendy, eventy, konfiguracja i projekt **Spigot API** |
| V | 25–31 | **Migracja na Paper**, Adventure, SQL, Folia i integracje |
| VI | 32–36 | Architektura, testy, CI/CD i projekt końcowy |

**Start:** lekcja 1. **Pierwszy plugin:** lekcja 16. **Porównanie silników i API:** lekcja 15. **Migracja Spigot → Paper:** lekcja 25.

## Materiały

- [PLAN_KURSU.md](PLAN_KURSU.md) — pełny plan 36 lekcji i zasady zgodności API.
- [Lekcja 01 — środowisko, pierwszy program i pierwsze dane](lekcje/01_PIERWSZY_PROGRAM_KOTLIN.md) — kompletna lekcja z **4 zrzutami ekranu IntelliJ IDEA**, ćwiczeniami, mini-testem i rozwiązaniami.
- [Lekcja 02 — zmienne, typy danych i tekst](lekcje/02_ZMIENNE_TYPY_DANYCH_I_TEKST.md) — `val`, `var`, typy danych, konwersje, szablony tekstu, **8 ćwiczeń**, mini-test i projekt konsolowy.
- [Lekcja 03 — operatory i wejście użytkownika](lekcje/03_OPERATORY_I_WEJSCIE_UZYTKOWNIKA.md) — działania matematyczne, porównania, `readln()`, `toIntOrNull()`, **8 ćwiczeń**, mini-test i kalkulator sklepu serwerowego.
- [Lekcja 04 — warunki i logika](lekcje/04_WARUNKI_I_LOGIKA.md) — `if`, `else if`, `else`, `when`, `&&`, `||`, `!`, **8 ćwiczeń**, mini-test i projekt systemu rezerwacji.
- [Lekcja 05 — pętle i zakresy](lekcje/05_PETLE_I_ZAKRESY.md) — `for`, `while`, `do-while`, `until`, `downTo`, `step`, `break`, `continue`, **8 ćwiczeń**, mini-test i projekt dziennika wydatków.
- [Lekcja 06 — funkcje i podział programu](lekcje/06_FUNKCJE_I_PODZIAL_PROGRAMU.md) — `fun`, parametry, `return`, `Unit`, argumenty domyślne i nazwane, zasięg zmiennych, **8 ćwiczeń**, mini-test oraz projekt kasy biletowej.
- [Lekcja 07 — kolekcje](lekcje/07_KOLEKCJE.md) — listy, tablice, mapy, zbiory, wyszukiwanie, modyfikowanie, **8 ćwiczeń**, mini-test i domowa lista zakupów. [Wersja lektorska (TXT)](lektor/Lekcja_07_Tekst_dla_lektora.txt).
- [Lekcja 08 — null, wyjątki, debugowanie i projekt konsolowy](lekcje/08_NULL_WYJATKI_I_PROJEKT_KONSOLOWY.md) — `?`, `?.`, `?:`, `try/catch`, debugger, **8 ćwiczeń**, mini-test i rejestr graczy. [Wersja lektorska (TXT)](lektor/Lekcja_08_Tekst_dla_lektora.txt).

**Poziom I — Fundamenty Kotlina: ukończony (8/8 lekcji).**

Pozostałe lekcje będą tworzone **po jednej**, a nie masowo. Każda zostanie udostępniona w osobnym pliku `.md`. Od lekcji 7 repozytorium zawiera również scenariusze dla lektora w katalogu `lektor/`, przygotowane na podstawie odpowiadającej im wersji pisemnej.

## Jak pracować z lekcją?

Przeczytaj teorię, **uruchom każdy przykład**, spróbuj rozwiązać zadania bez patrzenia do odpowiedzi i porównaj swoje rozwiązanie z końcową sekcją pliku. Nie przechodź do następnej lekcji, jeśli kluczowe ćwiczenia nadal wymagają przepisywania gotowego kodu.

Orientacyjny czas: 60–120 minut na lekcję. Ukończenie 36 spotkań nie oznacza automatycznie osiągnięcia poziomu eksperta — projektów nie da się zastąpić samą teorią.

## Dlaczego najpierw Spigot?

Spigot API daje dostęp do wspólnych, podstawowych mechanizmów Bukkit: eventów, komend, uprawnień, konfiguracji i świata gry. Paper rozszerza ten ekosystem, więc kurs **najpierw uczy wspólnego fundamentu, a później świadomego korzystania z dodatków Paper**. Kod korzystający z Paper-only API może nie działać na Spigot; samo `plugin.yml` nie gwarantuje zgodności. Osobna lekcja wyjaśnia też różnice między Spigot, Paper, Purpur, Folia, Fabric/Forge oraz proxy Velocity.

## Informacje dla prowadzącego

Kompletne programy do uruchomienia oznaczamy jako `kotlin-run`. Fragmenty niepełne, błędny kod i zadania do samodzielnego wykonania pozostają oznaczone jako `kotlin`. Lekcje zawierają metrykę (poziom, numer, temat, czas, wymagania), teorię od podstaw, kompletne przykłady, analizę, typowe błędy, ćwiczenia, mini-test oraz rozwiązania na końcu. Dokładne wersje narzędzi i bibliotek w lekcjach pluginowych sprawdzamy na bieżąco.

## Dokumentacja

[Kotlin](https://kotlinlang.org/docs/home.html) · [Spigot API](https://hub.spigotmc.org/javadocs/spigot/) · [SpigotMC Wiki](https://www.spigotmc.org/wiki/) · [Paper API](https://docs.papermc.io/paper/dev/)

---

Projekt edukacyjny **SyntaxDevTeam**. Warunki wykorzystania zawiera plik [LICENSE](LICENSE).
