# Kotlin od zera — od pierwszego programu do pluginów Minecraft

> **Wersja planu:** 3.0 · 20 listopada 2026  
> **Łącznie:** **36 lekcji** · 6 poziomów  
> **Ścieżka:** Kotlin → wspólne podstawy Bukkit/Spigot → **Spigot API** → przejście na **Paper API** → samodzielny projekt  
> **Adresat:** początkująca osoba bez znajomości programowania; nie trzeba znać Javy ani Pythona  
> **Tempo:** orientacyjnie jedna lekcja dziennie (zwykle 60–120 minut), ale ćwiczenia i projekty wymagają praktyki również pomiędzy lekcjami

## Cel kursu

Nauczyć samodzielnego projektowania, tworzenia, debugowania i publikowania pluginów do Minecraft Java Edition w języku Kotlin. Kurs **nie zaczyna się od specyficznego API Paper**. Uczeń najpierw poznaje podstawowy, powszechnie używany model pluginów oparty na **Bukkit/Spigot API**, a dopiero później uczy się świadomie korzystać z funkcji **Paper API**.

**Dlaczego ta kolejność?** Zrozumienie wspólnych mechanizmów (klasa `JavaPlugin`, `plugin.yml`, listenery, komendy, scheduler i konfiguracja) pomaga czytać dokumentację wielu platform. Kiedy pojawi się Paper, uczeń będzie wiedzieć, **które funkcje są przenośne**, a które są specyficzne dla określonego silnika. Pisanie pluginu na bazie `spigot-api` nie oznacza jednak automatycznej zgodności ze wszystkimi serwerami i wersjami Minecrafta.

Celem **nie** jest obiecywanie poziomu profesjonalnego programisty po 36 dniach. To uporządkowany kurs obejmujący fundamenty i ścieżkę do coraz trudniejszych projektów.

## Zasady prowadzenia lekcji

1. **Jeden numer = jeden osobny dokument Markdown** w `lekcje/`. Na razie opracowujemy tylko lekcję 1; pozostałe pozycje stanowią program kursu.
2. Każdy dokument zawiera metrykę: **Poziom / Lekcja / Temat / Czas / Potrzebujesz**, cele i wymagania wstępne.
3. Każde nowe pojęcie wyjaśniamy od zera, bez porównywania do Javy lub Pythona i bez zakładania znajomości Minecraft API.
4. Przykłady muszą być kompletne, możliwe do uruchomienia, oznaczone jako `kotlin`; wyniki programu pokazujemy osobno.
5. Wyjaśniamy kod krok po kroku, typowe błędy i sposoby ich samodzielnego diagnozowania.
6. Ćwiczenia o rosnącej trudności, mini-test i projekt praktyczny poprzedzają **rozwiązania zebrane na końcu** pliku.
7. Powiązane tematy łączymy w jedną lekcję, zamiast sztucznie mnożyć spotkania.
8. W lekcjach pluginowych podajemy **docelowe API** (Spigot lub Paper) oraz sprawdzamy aktualne wymagania JDK, zależności Gradle i wersji Minecrafta w momencie tworzenia materiału.
9. Nie korzystamy na starcie z zaawansowanych zewnętrznych bibliotek jako zamiennika nauki standardowego API.
10. Każda praktyka pluginowa obejmuje także uruchomienie na testowym serwerze i analizę logów, nie tylko kompilację.

## Poziom I — Fundamenty Kotlina (lekcje 1–8)

**Cel:** samodzielne krótkie programy konsolowe.

01. **Środowisko, pierwszy program i pierwsze dane.** IntelliJ IDEA, JDK, JVM, `fun main()`, `print`, `println`, komentarze, `val`, szablony tekstu. *Dokument dostępny.*
02. **Zmienne, typy danych i tekst.** `val`, `var`, `Int`, `Long`, `Double`, `Boolean`, `Char`, `String`, wnioskowanie typów, konwersje, interpolacja.
03. **Operatory i wejście użytkownika.** Arytmetyka, operatory porównania, `readln()`, parsowanie i sprawdzanie danych.
04. **Warunki i logika.** `if`, `else`, `when`, `&&`, `||`, `!`; rozwiązywanie problemów decyzyjnych.
05. **Pętle i zakresy.** `for`, `while`, `do-while`, `until`, `downTo`, `break`, `continue`.
06. **Funkcje i podział programu.** Parametry, typ zwracany, argumenty domyślne, zasięg, wielokrotne użycie funkcji.
07. **Kolekcje.** Listy, tablice, mapy, zbiory, iterowanie, modyfikowanie i wyszukiwanie.
08. **Null, wyjątki i projekt konsolowy.** `?`, `?.`, `?:`, `try/catch`, debugowanie i konsolowy rejestr graczy.

## Poziom II — Programowanie obiektowe (lekcje 9–13)

**Cel:** projektowanie małych programów w kilku współpracujących klasach.

09. **Klasy, obiekty i modele danych.** Właściwości, metody, konstruktory, `data class`.
10. **Enkapsulacja, interfejsy i pojedyncze instancje.** Widoczność, gettery/settery, `interface`, `object`, `companion object`.
11. **Dziedziczenie, kompozycja i modelowanie stanów.** `open`, `override`, `super`, `enum class`, wprowadzenie do `sealed`.
12. **Przetwarzanie danych w Kotlinie.** Lambdy, `map`, `filter`, `find`, proste generyki, funkcje rozszerzające, wybrane scope functions.
13. **Projekt obiektowy.** Konsolowy system graczy z zapisem/odczytem danych, rozdzielonymi klasami i prostą walidacją.

## Poziom III — Narzędzia, silniki serwerowe i pierwszy plugin (lekcje 14–16)

**Cel:** rozumienie, dla jakiej platformy powstaje plugin, oraz uruchomienie pierwszego JAR-a.

14. **Warsztat Kotlin/JVM.** IntelliJ IDEA, Gradle i Kotlin DSL (`build.gradle.kts`), zależności, Gradle Wrapper, budowanie JAR, biblioteki uruchomieniowe Kotlina, debugger, podstawy Git i GitHub.
15. **Rodzaje silników Minecraft i wybór API — osobna lekcja.** Vanilla; Bukkit (historyczny projekt i API); Spigot; Paper; Purpur; Folia; Fabric, Forge/NeoForge (mody); Velocity i BungeeCord (proxy). Różnica między **silnikiem**, **API**, **pluginem**, **modem** i **proxy**; co znaczy `spigot-api` lub `paper-api` w Gradle; `plugin.yml` vs `paper-plugin.yml`; zależność kompatybilności od użytych metod, formatu pluginu, wersji serwera, Javy i wątków. Zadanie: dobrać API dla kilku rzeczywistych scenariuszy.
16. **Pierwszy plugin na Spigot API.** Projekt Kotlin + Gradle z zależnością `org.spigotmc:spigot-api` w zakresie `compileOnly`, klasa `JavaPlugin`, `onEnable()`, `onDisable()`, `plugin.yml`, logi, poprawne spakowanie Kotlin runtime i instalacja na testowym Spigot.

> **Ważne:** Spigot API jest punktem wyjścia ze względu na wspólne podstawy, a nie dlatego, że jest zawsze wydajniejszym lub nowocześniejszym serwerem od Paper.

## Poziom IV — Praktyczne pluginy Spigot (lekcje 17–24)

**Cel:** samodzielny plugin użytkowy oparty na przenośnym rdzeniu Bukkit/Spigot.

17. **Zdarzenia i gracze.** `Listener`, `@EventHandler`, rejestracja listenerów, join/quit, `Player`, UUID i anulowanie eventów.
18. **Komendy, argumenty i uprawnienia.** `CommandExecutor`, `TabCompleter`, `CommandSender`, `plugin.yml`, obsługa błędnych danych i permissions.
19. **Konfiguracja YAML i komunikaty.** `config.yml`, `saveDefaultConfig()`, odczyt wartości, odświeżanie danych i komunikaty graczom.
20. **Przedmioty i ekwipunki.** `ItemStack`, `ItemMeta`, `Inventory`, `PersistentDataContainer` i zdarzenia kliknięcia.
21. **Światy, bloki i teleportacja.** `World`, `Location`, bezpieczne teleporty, spawn, domy i warpy.
22. **Scheduler, wydajność i wątki.** Zadania opóźnione/cykliczne w modelu Spigot, co musi działać na głównym wątku, czego nie robić przy I/O.
23. **Trwałe dane pluginu.** YAML/JSON, ścieżki plików, zapis i odczyt, obsługa błędów i ochrona przed utratą danych.
24. **Projekt etapowy: kompletny plugin Spigot.** Np. SimpleWarpX: komendy, uprawnienia, eventy, konfiguracja, zapis danych, test po restarcie i podstawowy README.

## Poziom V — Przejście na Paper i bardziej zaawansowane pluginy (lekcje 25–31)

**Cel:** rozumieć, kiedy i po co wykorzystywać API Paper, oraz rozbudować projekt bez utraty świadomej kontroli nad zgodnością.

25. **Migracja Spigot → Paper.** Podmiana zależności z `spigot-api` na `paper-api`, uruchomienie tego samego pluginu na Paper, sprawdzenie regresji, rozróżnienie wspólnego API od elementów Paper-only, dokumentowanie kompatybilności.
26. **Komunikaty nowej generacji.** Adventure `Component`, MiniMessage, migracja starszych wiadomości i bezpieczne formatowanie danych użytkownika.
27. **Możliwości specyficzne dla Paper.** Wybrane zdarzenia i API Paper, nowoczesne API komend, różnica `plugin.yml` / `paper-plugin.yml`, inicjalizacja i zależności. Nie zakładamy, że każdy plugin Paper wymaga `paper-plugin.yml`.
28. **Bazy danych SQL.** JDBC, SQL parametryzowany, HikariCP, SQLite oraz MySQL/MariaDB, operacje asynchroniczne, migracje schematu.
29. **Współbieżność, Folia i poprawne schedulery.** Model regionów, region/entity/global scheduler, granice kompatybilności, podstawy coroutines i bezpieczne zarządzanie cyklem życia zadań.
30. **Integracje i rozszerzalność.** Zależności opcjonalne, integracje z cudzymi pluginami, własne zdarzenia, podział na serwisy i interfejsy.
31. **Projekt etapowy: rozbudowany plugin Paper.** Rozszerzenie pluginu Spigot o świadomie wybrane Paper API, SQL, diagnostykę i testy zgodności.

## Poziom VI — Jakość, utrzymanie i publikacja (lekcje 32–36)

**Cel:** przygotować projekt do bezpiecznego rozwijania i publikowania.

32. **Architektura większych pluginów.** Odpowiedzialności klas, warstwy, wstrzykiwanie zależności, separacja domeny od API serwera, moduły Gradle i semantyka wersji.
33. **Testowanie, debugowanie i kontrola regresji.** Testy jednostkowe i integracyjne, mocki, przypadki brzegowe, czytanie stack trace i przegląd kodu.
34. **Budowanie i wydania.** Gradle, testy w GitHub Actions, shading bez przypadkowego dublowania bibliotek, JAR, wersjonowanie, release i changelog.
35. **Bezpieczeństwo, wydajność i zgodność platform.** Uprawnienia, walidacja, ograniczenia API, pamięć, logi, polityka kompatybilności Spigot/Paper/Purpur/Folia, dokumentacja i wsparcie użytkownika.
36. **Projekt końcowy.** Od specyfikacji przez implementację i testy na wybranych serwerach do dokumentacji, wydania i planu rozwoju.

---

## Punkty kontrolne

| Lekcja | Umiejętność po zakończeniu |
|---|---|
| 8 | Samodzielny program konsolowy |
| 13 | Prosta aplikacja obiektowa z zapisem danych |
| 15 | Świadomy wybór typu serwera i docelowego API |
| 16 | Pierwszy uruchomiony plugin **Spigot** |
| 19 | Plugin Spigot z eventem, komendą, uprawnieniem i konfiguracją |
| 24 | Samodzielny plugin użytkowy dla **Spigot API** |
| 25 | Zrozumienie procesu przenoszenia pluginu na **Paper** |
| 31 | Większy plugin Paper z bazą danych i testami |
| 36 | Projekt końcowy z dokumentacją, CI i publikacją |

## Kluczowe zasady kompatybilności

- **Silnik serwera ≠ API**. Silnik wykonuje grę i ładuje rozszerzenia; API określa, z których klas i metod korzysta kod.
- **Spigot API** rozszerza podstawy Bukkit; **Paper API** udostępnia dodatkowe możliwości. Kod skompilowany wyłącznie pod `spigot-api` często działa na Paper, ale nadal wymaga testów.
- **Plugin używający API Paper nie musi działać na Spigot**. Sam `plugin.yml` nie gwarantuje zgodności wstecznej.
- `paper-plugin.yml` to osobny format ładowania pluginów Paper. Nie należy go używać tylko dlatego, że program jest testowany na Paper; zależy to od potrzeb projektu.
- **Folia** ma osobny model wielowątkowego wykonywania regionów. Zwykły plugin Paper nie staje się bezpieczny dla Folia bez odpowiedniego dostosowania i testów.
- **Velocity/BungeeCord to proxy, nie serwery świata gry**. Ich własne pluginy korzystają z innych API niż pluginy Spigot/Paper.
- **Fabric/Forge/NeoForge dotyczą przede wszystkim modów**, z inną ścieżką tworzenia rozszerzeń i innymi API. Nie instalujemy na nich standardowego JAR-a Spigot tak, jak na Paper.
- Sprawdzaj **konkretną wersję Minecrafta, Java/JDK, API i serwera** przed skompilowaniem lub wydaniem. Nie utożsamiaj `api-version` w `plugin.yml` z wersją Java ani z deklaracją „plugin Paper”.
- **API w Gradle to zależność do kompilacji** (zwykle `compileOnly`), natomiast uruchomieniowe biblioteki Kotlina trzeba zapewnić na serwerze lub dołączyć do JAR zgodnie z architekturą projektu.

## Materiały referencyjne

- [Kotlin — Basic syntax](https://kotlinlang.org/docs/basic-syntax.html)
- [Kotlin — Get started with JVM](https://kotlinlang.org/docs/jvm-get-started.html)
- [SpigotMC — Start coding with Spigot](https://www.spigotmc.org/wiki/start-coding-with-spigot/)
- [SpigotMC — plugin.yml](https://www.spigotmc.org/wiki/plugin-yml/)
- [Spigot API — JavaPlugin](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/plugin/java/JavaPlugin.html)
- [Paper — Developer documentation](https://docs.papermc.io/paper/dev/)
- [Paper — Bukkit plugin.yml](https://docs.papermc.io/paper/dev/plugin-yml/)
- [Paper — Paper plugins](https://docs.papermc.io/paper/dev/getting-started/paper-plugins/)
- [Folia — dokumentacja projektu](https://github.com/PaperMC/Folia)

**Stan realizacji:** przygotowane: plan, lekcja 1. **Lekcje 2–36 będą opracowywane pojedynczo**, zgodnie z programem.
