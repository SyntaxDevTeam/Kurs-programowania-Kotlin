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
- [Lekcja 01 — środowisko, pierwszy program i pierwsze dane](lekcje/01_PIERWSZY_PROGRAM_KOTLIN.md) — kompletna lekcja z ćwiczeniami, mini-testem i rozwiązaniami.

Pozostałe lekcje będą tworzone **po jednej**, a nie masowo. Każda zostanie udostępniona w osobnym pliku `.md`.

## Jak pracować z lekcją?

Przeczytaj teorię, **uruchom każdy przykład**, spróbuj rozwiązać zadania bez patrzenia do odpowiedzi i porównaj swoje rozwiązanie z końcową sekcją pliku. Nie przechodź do następnej lekcji, jeśli kluczowe ćwiczenia nadal wymagają przepisywania gotowego kodu.

Orientacyjny czas: 60–120 minut na lekcję. Ukończenie 36 spotkań nie oznacza automatycznie osiągnięcia poziomu eksperta — projektów nie da się zastąpić samą teorią.

## Dlaczego najpierw Spigot?

Spigot API daje dostęp do wspólnych, podstawowych mechanizmów Bukkit: eventów, komend, uprawnień, konfiguracji i świata gry. Paper rozszerza ten ekosystem, więc kurs **najpierw uczy wspólnego fundamentu, a później świadomego korzystania z dodatków Paper**. Kod korzystający z Paper-only API może nie działać na Spigot; samo `plugin.yml` nie gwarantuje zgodności. Osobna lekcja wyjaśnia też różnice między Spigot, Paper, Purpur, Folia, Fabric/Forge oraz proxy Velocity.

## Informacje dla prowadzącego

W przykładach używamy `kotlin` zamiast `python-run`. Lekcje zawierają metrykę (poziom, numer, temat, czas, wymagania), teorię od podstaw, kompletne przykłady, analizę, typowe błędy, ćwiczenia, mini-test oraz rozwiązania na końcu. Dokładne wersje narzędzi i bibliotek w lekcjach pluginowych sprawdzamy na bieżąco.

## Dokumentacja

[Kotlin](https://kotlinlang.org/docs/home.html) · [Spigot API](https://hub.spigotmc.org/javadocs/spigot/) · [SpigotMC Wiki](https://www.spigotmc.org/wiki/) · [Paper API](https://docs.papermc.io/paper/dev/)

---

Projekt edukacyjny **SyntaxDevTeam**. Warunki wykorzystania zawiera plik [LICENSE](LICENSE).
