# Lekcja 1 — Kotlin od zera: środowisko, pierwszy program i pierwsze dane

> Kurs 36 lekcji · ścieżka: Kotlin → Spigot → Paper · aktualizacja: 20.11.2026

**Poziom:** I — Fundamenty Kotlina  
**Lekcja:** 1 z 8 w Poziomie I (1 z 36 w całym kursie)  
**Temat:** IntelliJ IDEA, JDK, `main()`, wyświetlanie informacji, `val`, szablony tekstu i komentarze  
**Czas:** około 75–100 minut (razem z ćwiczeniami; instalacja może zająć dodatkowy czas)  
**Potrzebujesz:** komputer, dostęp do internetu przy instalacji, IntelliJ IDEA, JDK 25, chęć eksperymentowania  
**Wymagana wiedza:** żadna — kurs jest przeznaczony także dla osoby, która nigdy nie programowała

---

> **Ścieżka nauki:** najpierw Kotlin i programy konsolowe, później pierwsze pluginy w oparciu o **Spigot API**, a następnie świadome przejście na **Paper API**. Rodzaje silników serwerowych i znaczenie wyboru API wyjaśnimy w osobnej **lekcji 15**. Na tym etapie wystarczy wiedzieć, że plugin jest dodatkiem uruchamianym przez odpowiedni serwer, a nie zwykłym programem konsolowym.

## 1. Po co jest ta lekcja?

W skróconym, **36-lekcyjnym kursie** nie poświęcamy osobnych spotkań na każde pojedyncze polecenie. Już pierwszego dnia uruchomisz program, nauczysz się wyświetlać komunikaty i **przechowywać proste informacje**, aby nie wpisywać ich za każdym razem ręcznie.

Po tej lekcji potrafisz:

- wyjaśnić, czym jest program i jaką rolę pełnią Kotlin, JVM, JDK oraz IntelliJ IDEA;
- założyć projekt Kotlin/JVM i znaleźć plik `Main.kt`;
- uruchomić program rozpoczynający się od `fun main()`;
- użyć `print()` i `println()` oraz przewidzieć wynik w konsoli;
- rozpoznać tekst i liczbę w kodzie;
- utworzyć prostą wartość za pomocą `val` i umieścić ją w komunikacie przez `$nazwa`;
- napisać komentarz oraz poprawić podstawowy błąd składni;
- samodzielnie stworzyć **konsolową kartę serwera Minecraft**.

> **Nie tworzysz jeszcze pluginu.** Najpierw nauczysz się pisać i uruchamiać zwykły program. Do pierwszego pluginu **Spigot** przejdziemy w **lekcji 16**, a od **lekcji 25** będziemy poznawać i stosować dodatkowe możliwości Paper API. Nie musisz rozumieć wszystkich mechanizmów JVM, Gradle ani Spigot API już teraz.

## 2. Czym jest programowanie?

Program to **zbiór instrukcji**, które komputer wykonuje zgodnie z określonymi zasadami. Zapisujesz je w języku programowania, tutaj — w Kotlinie.

Wyobraź sobie trzy polecenia:

1. Pokaż nazwę serwera.
2. Pokaż liczbę graczy.
3. Napisz, że serwer jest gotowy.

Komputer zrobi dokładnie to, co zapiszesz. Nie dopowie brakującej instrukcji i nie odgadnie, że w nazwie jest literówka.

### Cztery pojęcia, które warto odróżnić

| Pojęcie | Znaczenie na dziś |
|---|---|
| **Kotlin** | Język, w którym zapisujesz instrukcje programu. |
| **IntelliJ IDEA** | Środowisko programistyczne (IDE): miejsce pisania, uruchamiania i poprawiania kodu. |
| **JDK** | Zestaw narzędzi potrzebnych do tworzenia i uruchamiania aplikacji na platformie Java. |
| **JVM** | Wirtualna maszyna, na której może działać skompilowany program Kotlin/JVM. |

W uproszczeniu nasza droga wygląda tak:

```text
Kod w Main.kt → kompilacja → program uruchomiony na JVM → wynik w konsoli
```

**Konsola** to okno, w którym widzisz tekst wypisany przez program oraz komunikaty narzędzi programistycznych.

## 3. Przygotowanie środowiska — raz, a dobrze

### Krok 1. Zainstaluj IntelliJ IDEA

Wejdź na <https://www.jetbrains.com/idea/download/> i zainstaluj IntelliJ IDEA. Wersja darmowa wystarcza do tego kursu.

### Krok 2. Utwórz projekt

1. Uruchom IntelliJ IDEA i wybierz **New Project** (lub **File → New → Project**).
2. Wybierz projekt **Kotlin** przeznaczony dla **JVM**.
3. Wpisz nazwę `KotlinNauka`.
4. Jako system budowania wybierz **Gradle**, a język skryptów budowania — **Kotlin DSL**, jeśli kreator udostępnia te opcje.
5. Wybierz **JDK 25**. Jeśli go nie masz, skorzystaj z opcji **Download JDK** dostępnej w kreatorze.
6. Jeśli jest opcja **Add sample code**, możesz ją włączyć.
7. Kliknij **Create** i pozwól projektowi zakończyć pierwszą synchronizację.

> Dlaczego JDK 25? W tym kursie docelowo będziemy tworzyć pluginy dla wybranych współczesnych wersji Spigot i Paper. Wymagana wersja Javy zależy od konkretnej wersji serwera; nie każdy Spigot wymaga Java 25. Nie musisz na razie konfigurować serwera Minecraft. Jeśli już masz działający projekt na zgodnym JDK, nie trzeba instalować kilku wersji tylko do tej lekcji.

Wygląd kreatora zależy od wersji IntelliJ IDEA. Istotne są: **Kotlin + JVM + prawidłowy JDK**.

### Krok 3. Znajdź plik z kodem

Zazwyczaj wygląda to tak:

```text
KotlinNauka/
├── build.gradle.kts
└── src/
    └── main/
        └── kotlin/
            └── Main.kt
```

Jeśli `Main.kt` nie powstał automatycznie, kliknij prawym przyciskiem folder `src/main/kotlin`, wybierz **New → Kotlin Class/File**, utwórz **File** o nazwie `Main` i wpisz kod z następnej sekcji.

**Nie edytuj na razie `build.gradle.kts`.** To skrypt służący do konfiguracji budowania projektu, a nie miejsce, w którym piszemy dzisiejszy program.

## 4. Pierwszy działający program

Otwórz `Main.kt` i wpisz całość:

```kotlin
fun main() {
    println("Cześć, świecie!")
}
```

Kliknij zielony symbol uruchomienia obok `main()` i wybierz **Run**.

Oczekiwany wynik programu:

```text
Cześć, świecie!
```

Jeśli widzisz też komunikaty Gradle, na przykład `BUILD SUCCESSFUL`, nie oznacza to błędu: część tekstu pochodzi z narzędzia budującego projekt.

### Rozbieramy kod na części

| Fragment | Co oznacza? |
|---|---|
| `fun` | Informuje, że definiujesz **funkcję** — nazwany fragment programu. |
| `main` | Nazwa funkcji startowej programu konsolowego. |
| `()` | Nawiasy, w których można przekazywać informacje do funkcji; tutaj są puste. |
| `{` i `}` | Początek i koniec bloku instrukcji tej funkcji. |
| `println(...)` | Wypisanie wartości do konsoli i przejście do nowej linii. |
| `"Cześć, świecie!"` | Tekst. Cudzysłowy wyznaczają jego początek i koniec. |

Kod pomiędzy nawiasami `{` i `}` jest zawartością funkcji `main`. Wcięcie przed `println` ułatwia czytanie — same spacje nie powodują wykonania instrukcji.

### Mały eksperyment

Zamień tekst na własne powitanie, uruchom program ponownie i upewnij się, że wynik się zmienił. **Sama zmiana pliku nie zawsze aktualizuje już otwarte okno wyniku — kod trzeba uruchomić ponownie.**

## 5. Kilka instrukcji i kolejność wykonania

```kotlin
fun main() {
    println("Start programu")
    println("Wczytywanie danych...")
    println("Gotowe!")
}
```

Wynik:

```text
Start programu
Wczytywanie danych...
Gotowe!
```

Na tym etapie instrukcje wykonują się **po kolei, od góry do dołu**. Jeżeli zamienisz miejscami dwie linie, zmienisz też kolejność komunikatów.

### `print()` i `println()`

Obie funkcje wypisują informacje, ale tylko `println()` kończy bieżący wiersz:

```kotlin
fun main() {
    print("Serwer: ")
    println("CraftWorld")
    print("Tryb: ")
    println("Survival")
}
```

Wynik:

```text
Serwer: CraftWorld
Tryb: Survival
```

- `print("Serwer: ")` wyświetla tekst i **nie** przechodzi do nowej linii.
- `println("CraftWorld")` dopisuje nazwę, po czym rozpoczyna następny wiersz.

**Sprawdź od razu:** zmień pierwsze `print` na `println`. Jak zmieni się wynik? Następnie przywróć kod.

## 6. Tekst i liczba — dlaczego cudzysłowy mają znaczenie?

```kotlin
fun main() {
    println("12")
    println(12)
}
```

Wynik:

```text
12
12
```

Wygląd wyniku jest taki sam, ale dla języka są to **dwie różne wartości**:

- `"12"` — tekst;
- `12` — liczba całkowita.

Nazywamy to różnymi **typami danych**. Dzisiaj wystarczy rozpoznać różnicę. Więcej o typach i obliczeniach będzie w lekcjach 2–3.

Jeśli chcesz wypisać słowo, użyj cudzysłowów:

```kotlin
fun main() {
    println("Minecraft")
}
```

Zapis `println(Minecraft)` jest błędny **w naszym przykładzie**, ponieważ nie zdefiniowaliśmy żadnej wartości o nazwie `Minecraft`. Kotlin nie wie, do czego ta nazwa się odnosi.

## 7. Pierwsza wartość `val` — program zaczyna przechowywać informacje

Dotąd wpisywaliśmy tekst bezpośrednio do `println()`. Spróbuj czegoś bardziej praktycznego:

```kotlin
fun main() {
    val serverName = "CraftWorld"
    val onlinePlayers = 12

    println(serverName)
    println(onlinePlayers)
}
```

Wynik:

```text
CraftWorld
12
```

### Co dzieje się krok po kroku?

1. `val serverName = "CraftWorld"` tworzy nazwę `serverName`, której przypisujemy tekst.
2. `val onlinePlayers = 12` tworzy nazwę `onlinePlayers`, której przypisujemy liczbę.
3. `println(serverName)` wypisuje **przechowywany tekst**, nie dosłowny napis `serverName`.
4. `println(onlinePlayers)` wypisuje przechowywaną liczbę.

`val` oznacza, że **nie można później przypisać nowej wartości do tej samej nazwy**. Dzisiaj nie potrzebujemy jeszcze niczego zmieniać w trakcie działania programu. W lekcji 2 poznasz także `var`, rodzaje typów i konsekwencje tego wyboru.

### Szablony tekstu — `$nazwa`

Wypisanie samej nazwy to za mało. Chcemy zbudować pełne komunikaty:

```kotlin
fun main() {
    val serverName = "CraftWorld"
    val onlinePlayers = 12

    println("Serwer: $serverName")
    println("Gracze online: $onlinePlayers")
}
```

Wynik:

```text
Serwer: CraftWorld
Gracze online: 12
```

W tekstowym napisie `$serverName` oznacza: **wstaw w tym miejscu wartość przechowywaną pod nazwą `serverName`**. Kotlin nazywa to **szablonem tekstu** (*string template*).

Uwaga na różnicę:

```kotlin
fun main() {
    val serverName = "CraftWorld"

    println("serverName")
    println(serverName)
    println("$serverName")
}
```

Wynik:

```text
serverName
CraftWorld
CraftWorld
```

Pierwsza linia wypisuje dosłowny tekst. Druga i trzecia korzystają z wartości `serverName`, ale tylko w trzeciej wartość jest **wstawiana do tekstu**.

> **Wskazówka:** jeśli zmienisz `"CraftWorld"` na `"MoonValley"` w miejscu utworzenia `val`, wszystkie komunikaty używające `$serverName` pokażą nową nazwę. Nie musisz poprawiać każdej linii osobno.

## 8. Komentarze — wyjaśnienia, a nie instrukcje

```kotlin
fun main() {
    // Nazwa serwera, którą pokażemy w powitaniu
    val serverName = "CraftWorld"

    println("Witamy na $serverName!")
}
```

Wynik:

```text
Witamy na CraftWorld!
```

Komentarz rozpoczynający się od `//` kończy się wraz z końcem wiersza. Nie pojawia się w wyniku programu. Dłuższy komentarz można zapisać tak:

```kotlin
/*
To przykładowy program konsolowy.
Później wykorzystamy podobne informacje w pluginie.
*/
fun main() {
    println("Program uruchomiony!")
}
```

Nie trzeba komentować każdego `println()`. Komentarz powinien wyjaśniać **po co** istnieje fragment kodu, gdy nie jest to oczywiste.

## 9. Mini-projekt — karta serwera Minecraft

Połączymy teraz: `main`, `println`, `print`, komentarze, `val` oraz szablony tekstu.

Przepisz i uruchom **cały** program:

```kotlin
fun main() {
    // Dane naszej przykładowej społeczności
    val serverName = "CraftWorld"
    val gameMode = "Survival"
    val onlinePlayers = 12
    val maxPlayers = 50

    println("================================")
    println("          KARTA SERWERA         ")
    println("================================")

    print("Nazwa: ")
    println(serverName)
    println("Tryb gry: $gameMode")
    println("Gracze online: $onlinePlayers / $maxPlayers")
    println("--------------------------------")
    println("Witaj na $serverName!")
}
```

Oczekiwany wynik:

```text
================================
          KARTA SERWERA         
================================
Nazwa: CraftWorld
Tryb gry: Survival
Gracze online: 12 / 50
--------------------------------
Witaj na CraftWorld!
```

### Dlaczego to lepsze od sztywnego wpisywania tekstu?

Zwróć uwagę, że `CraftWorld` występuje w kodzie jako wartość `serverName` tylko raz, choć widzisz ją dwa razy w wyniku. Gdybyś zmieniła nazwę serwera, poprawiasz jedno miejsce.

**Ćwiczenie na 3 minuty:** ustaw inną nazwę serwera, inny tryb i liczbę graczy, a następnie uruchom program. Sprawdź, czy oba wystąpienia nazwy zmieniły się automatycznie.

## 10. Najczęstsze błędy i jak je rozpoznać

| Błąd / objaw | Możliwa przyczyna | Co zrobić? |
|---|---|---|
| `Unresolved reference` | Literówka lub użycie nazwy, której nie utworzono | Porównaj nazwę w `val` i `println`; sprawdź cudzysłowy. |
| `Expecting ')'` | Brakuje `)` | Sprawdź otwarcie i zamknięcie nawiasu przy `println(...)`. |
| `Expecting '}'` | Brakuje klamry kończącej funkcję | Sprawdź parę `{` / `}`. |
| Komunikat pokazuje `$serverName` dosłownie | Wstawiono znak dolara w niewłaściwym miejscu lub użyto zapisu, który go ucieka | Użyj zwykłego `"Serwer: $serverName"`. |
| Komunikat pokazuje `serverName` zamiast nazwy | Wpisano `"serverName"` zamiast `serverName` lub `"$serverName"` | Usuń cudzysłowy lub dodaj `$` wewnątrz tekstu. |
| Wiersze są nieprawidłowo łamane | Pomylenie `print()` z `println()` | Zwróć uwagę, która instrukcja kończy wiersz. |
| Program nie startuje | Gradle się synchronizuje albo JDK jest błędnie ustawiony | Sprawdź pasek synchronizacji i ustawienia JDK projektu. |
| Widać dodatkowe komunikaty | Informacje IDE lub Gradle | Oddziel wynik programu od komunikatów narzędzi. |

### Mała metoda debugowania

1. **Przeczytaj cały pierwszy komunikat błędu** — nie tylko czerwone podkreślenie.
2. Sprawdź wskazaną linię oraz linię bezpośrednio powyżej.
3. Znajdź brakujący cudzysłów, nawias lub literówkę.
4. Zmień **jedną rzecz**, ponownie uruchom program i porównaj wynik.

Nie kasuj programu tylko dlatego, że pierwsza próba nie zadziałała. Poprawianie błędów jest normalną częścią programowania.

## 11. Zadania do samodzielnego wykonania

**Zasada:** rozwiązania są na końcu pliku. Najpierw wykonaj zadania samodzielnie. Dla każdego zadania napisz kompletny program z `fun main()`; możesz zmieniać zawartość jednego pliku `Main.kt`.

### Zadanie 1 — Trzy komunikaty (łatwe)

Wyświetl dokładnie:

```text
Dzień dobry!
Rozpoczynam kurs Kotlina.
Mój program działa.
```

### Zadanie 2 — Jedna linia z dwóch poleceń (łatwe)

Użyj **jednego** `print()` i **jednego** `println()`, aby otrzymać:

```text
Nick gracza: Alex
```

### Zadanie 3 — Przewidź wynik (łatwe)

Nie uruchamiaj kodu, dopóki nie zapiszesz przewidywanego wyniku:

```kotlin
fun main() {
    print("Witaj ")
    print("na ")
    println("serwerze!")
    println("Powodzenia!")
}
```

### Zadanie 4 — Pierwsze przechowywane informacje (średnie)

Utwórz trzy wartości `val`:

- `playerName` — wybrany nick;
- `playerLevel` — dowolna liczba poziomów;
- `serverName` — wymyślona nazwa serwera.

Za pomocą szablonów tekstu wypisz **dwa zdania**, które korzystają z tych danych. Nick powinien pojawić się w obu zdaniach, a nazwa serwera w jednym.

### Zadanie 5 — Popraw uszkodzony program (średnie)

Poniższy kod **celowo zawiera błędy**. Znajdź je i popraw, żeby program wypisał `Witaj na CraftWorld!`.

```kotlin
fun main( {
    val serverName = "CraftWorld"
    println("Witaj na $servername!")
```

### Zadanie 6 — Zmień dane tylko raz (średnie)

Napisz kod, w którym nazwa `playerName` jest ustalona **jeden raz** za pomocą `val`, ale pojawia się w **trzech różnych komunikatach**. Następnie zmień wartość `playerName` i potwierdź, że wszystkie trzy komunikaty pokazują nową nazwę.

### Zadanie 7 — Projekt na zaliczenie: konsolowa karta gracza (trudniejsze)

Stwórz program, który wyświetli estetyczny ekran profilu gracza Minecraft.

**Warunki obowiązkowe:**

1. Przechowuj w `val` co najmniej cztery informacje: `playerName`, `serverName`, `level`, `gameMode`.
2. Wyświetl nagłówek, nick, nazwę serwera, poziom, tryb gry i komunikat pożegnalny.
3. Wykorzystaj co najmniej **dwa** szablony tekstu (`$nazwa`).
4. Zastosuj przynajmniej jedno `print()` i jedno `println()`.
5. Dodaj jeden przydatny komentarz.
6. Zmień nick i sprawdź, czy jest aktualny w całym programie bez ręcznej zmiany tekstów w kilku miejscach.

**Dodatkowe wyzwanie (nieobowiązkowe):** wyświetl komunikat `Nick: Alex | Poziom: 10` jako jeden wiersz z użyciem dwóch wartości `val`.

## 12. Mini-test — sprawdzamy zrozumienie

Odpowiedz własnymi słowami, zanim zajrzysz do rozwiązania:

1. Czym różni się kod zapisany w `Main.kt` od tekstu widocznego w konsoli?
2. Po co w naszych programach istnieje funkcja `main()`?
3. Jaką różnicę zauważysz między `print()` i `println()`?
4. Czym różnią się wartości `"15"` oraz `15`?
5. Co tworzy instrukcja `val serverName = "CraftWorld"`?
6. Jaki będzie wynik `println("Serwer: $serverName")`, jeśli wcześniej utworzono `val serverName = "CraftWorld"`?
7. Dlaczego `println("serverName")` i `println(serverName)` nie oznaczają tego samego?
8. Do czego służą komentarze rozpoczynające się od `//`?

### Warunek przejścia do Lekcji 2

Powinnaś potrafić **bez kopiowania gotowego rozwiązania**:

- napisać i uruchomić `fun main()`;
- wypisać komunikat w jednej lub wielu liniach;
- utworzyć co najmniej dwie wartości `val` i wykorzystać je w `println`;
- znaleźć literówkę w nazwie i brakujący nawias.

Jeśli to potrafisz, nie musisz już robić kolejnej godziny teorii o `println`. Przejdź do następnej lekcji i wracaj do tego materiału tylko wtedy, gdy coś sprawia trudność.

## 13. Podsumowanie i następny krok

Masz działające środowisko, pierwszy program oraz praktyczny przykład użycia przechowywanych informacji. Rozumiesz różnicę między **tekstem dosłownym** a **nazwą z przypisaną wartością**.

**Lekcja 2 — Zmienne, typy danych i tekst w Kotlinie** rozwinie poznane `val`: dowiesz się, kiedy używać `var`, czym są `Int`, `Double`, `Boolean`, `Char` i `String`, jak działa wnioskowanie typów i kiedy potrzebna jest konwersja danych. Nie będziemy od nowa powtarzać `main()` ani `println()`.

---

## 14. Odpowiedzi i przykładowe rozwiązania

> **Nie zaglądaj przed wykonaniem zadań.** Rozwiązania są przykładami — w wielu przypadkach poprawny może być też inny kod.

### Zadanie 1

```kotlin
fun main() {
    println("Dzień dobry!")
    println("Rozpoczynam kurs Kotlina.")
    println("Mój program działa.")
}
```

### Zadanie 2

```kotlin
fun main() {
    print("Nick gracza: ")
    println("Alex")
}
```

### Zadanie 3

```text
Witaj na serwerze!
Powodzenia!
```

Pierwsze dwa `print` nie kończą wiersza. Dopiero `println("serwerze!")` dodaje przejście do nowej linii.

### Zadanie 4 — jedna z możliwych wersji

```kotlin
fun main() {
    val playerName = "Alex"
    val playerLevel = 10
    val serverName = "CraftWorld"

    println("Gracz $playerName ma poziom $playerLevel.")
    println("Gracz $playerName dołączył do serwera $serverName.")
}
```

### Zadanie 5

```kotlin
fun main() {
    val serverName = "CraftWorld"
    println("Witaj na $serverName!")
}
```

W oryginale brakowało `)` przy `main`, klamry `}` na końcu i użyto `servername` zamiast `serverName`. Kotlin rozróżnia wielkość liter.

### Zadanie 6 — jedna z możliwych wersji

```kotlin
fun main() {
    val playerName = "Alex"

    println("Witaj, $playerName!")
    println("Profil gracza: $playerName")
    println("Do zobaczenia, $playerName!")
}
```

Po zmianie `"Alex"` na inny nick wszystkie trzy linie wykorzystają nową wartość.

### Zadanie 7 — przykładowy projekt

```kotlin
fun main() {
    // Dane profilu — zmieniamy je w jednym miejscu
    val playerName = "Alex"
    val serverName = "CraftWorld"
    val level = 10
    val gameMode = "Survival"

    println("============================")
    println("        PROFIL GRACZA       ")
    println("============================")
    print("Nick: ")
    println(playerName)
    println("Serwer: $serverName")
    println("Poziom: $level")
    println("Tryb gry: $gameMode")
    println("----------------------------")
    println("Powodzenia, $playerName!")
}
```

Przykładowy wynik:

```text
============================
        PROFIL GRACZA       
============================
Nick: Alex
Serwer: CraftWorld
Poziom: 10
Tryb gry: Survival
----------------------------
Powodzenia, Alex!
```

### Mini-test — odpowiedzi

1. Kod jest zbiorem instrukcji zapisanych w pliku, a konsola pokazuje rezultat ich wykonania (oraz czasem komunikaty narzędzi).
2. `main()` jest punktem startowym naszego programu konsolowego.
3. `print()` nie przechodzi do nowej linii, `println()` przechodzi.
4. `"15"` jest tekstem, a `15` liczbą całkowitą.
5. Nazwę `serverName` przypisaną do tekstu `CraftWorld`; przez `val` nie można później przypisać do tej nazwy innej wartości.
6. `Serwer: CraftWorld`.
7. Cudzysłowy oznaczają tekst dosłowny; bez nich odwołujesz się do wcześniej utworzonej wartości.
8. Komentarze pomagają wyjaśniać kod i nie są wykonywane jako polecenia programu.

---

## Dodatkowe źródła (dla chętnych)

- [Kotlin — pierwszy program konsolowy](https://kotlinlang.org/docs/jvm-get-started.html)
- [Kotlin — podstawowa składnia](https://kotlinlang.org/docs/basic-syntax.html)
- [Kotlin — szablony tekstu](https://kotlinlang.org/docs/strings.html)
- [SpigotMC — podstawy tworzenia pluginów](https://www.spigotmc.org/wiki/start-coding-with-spigot/)
- [PaperMC — dokumentacja dla programistów (na później)](https://docs.papermc.io/paper/dev/)

**Koniec Lekcji 1/36. Następna: zmienne, typy i tekst. Pierwszy plugin Spigot: lekcja 16.**