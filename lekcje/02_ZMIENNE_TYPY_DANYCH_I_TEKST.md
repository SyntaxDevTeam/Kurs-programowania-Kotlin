# Lekcja 2 — Zmienne, typy danych i tekst w Kotlinie

**Poziom:** I — Fundamenty Kotlina  
**Lekcja:** 2 z 8 w Poziomie I (2 z 36 w całym kursie)  
**Temat:** `val`, `var`, podstawowe typy danych, wnioskowanie typów, konwersje i praca z tekstem  
**Czas:** około 90–120 minut (wraz z ćwiczeniami i mini-projektem)  
**Potrzebujesz:** IntelliJ IDEA, działający projekt Kotlin/JVM z lekcji 1, JDK 25  
**Wymagana wiedza:** podstawy z lekcji 1 — `fun main()`, `println()`, `val` i `$nazwa`

---

> **Jak korzystać z przykładów?** Bloki oznaczone `kotlin-run` zawierają **kompletne programy**: możesz wkleić je do `Main.kt` albo uruchomić w edytorze kursu. Zwykłe bloki `kotlin` to fragmenty do omówienia, kod do poprawienia albo zadania, których rozwiązanie należy wykonać samodzielnie. Nie zakładamy znajomości Javy ani Pythona.
>
> **Ścieżka kursu:** Kotlin → pluginy **Spigot API** (od lekcji 16) → przejście na **Paper API** (od lekcji 25). Dzisiejsze ćwiczenia to nadal zwykłe programy konsolowe.

## 1. Dlaczego warto znać typy danych?

W lekcji 1 wyświetlaliśmy tekst i korzystaliśmy z prostej wartości `val`. Ale nazwa gracza, liczba punktów, informacja o włączonym trybie administracyjnym czy pojedyncza litera **nie oznaczają tego samego**. Program musi wiedzieć, z jakimi danymi pracuje.

Po ukończeniu tej lekcji potrafisz:

- wybrać między `val` i `var` oraz wyjaśnić, **kiedy wartość może się zmienić**;
- rozróżnić typy `String`, `Char`, `Int`, `Long`, `Double` i `Boolean`;
- zapisać typ jawnie oraz pozwolić Kotlinowi odgadnąć go z wartości;
- zauważyć, dlaczego `"15"` nie jest tym samym co `15`;
- przekonwertować podstawowe dane, np. tekstową liczbę na `Int`;
- używać szablonów tekstu `$nazwa` i `${wyrażenie}`;
- stworzyć **konsolowy panel serwera Minecraft**, który pokazuje dane przed zmianą i po niej.

## 2. `val` i `var` — najważniejsza różnica

Wróćmy do wartości z poprzedniej lekcji. `val` pozwala **przypisać wartość tylko raz do konkretnej nazwy**. W tym przykładzie nazwa serwera pozostaje taka sama podczas działania programu:

```kotlin-run
fun main() {
    val serverName = "CraftWorld"
    println("Serwer: $serverName")
}
```

Wynik:

```text
Serwer: CraftWorld
```

A co, jeśli liczba graczy zmienia się w czasie? Wówczas możemy wykorzystać `var`:

```kotlin-run
fun main() {
    var onlinePlayers = 8

    println("Przed dołączeniem: $onlinePlayers")
    onlinePlayers = 9
    println("Po dołączeniu: $onlinePlayers")
}
```

Wynik:

```text
Przed dołączeniem: 8
Po dołączeniu: 9
```

**Analiza krok po kroku:**

1. `var onlinePlayers = 8` — tworzymy zmienną z początkową liczbą graczy.
2. Pierwszy `println()` wypisuje obecną wartość: `8`.
3. `onlinePlayers = 9` **nie tworzy nowej zmiennej**. Zastępuje wartość pod istniejącą nazwą.
4. Drugi `println()` pokazuje aktualną wartość: `9`.

Zwróć uwagę, że przy ponownym przypisaniu **nie piszemy kolejnego `var`**.

| Cecha | `val` | `var` |
|---|---|---|
| Przypisanie nowej wartości do tej samej nazwy | Nie | Tak |
| Typ jest ustalany przy tworzeniu | Tak | Tak |
| Kiedy używać | Gdy nie planujemy ponownego przypisania | Gdy wartość rzeczywiście będzie się zmieniać |
| Typowy przykład | Nazwa serwera, adres, limit miejsc | Aktualna liczba graczy, licznik |

**Praktyczna zasada:** domyślnie wybieraj `val`. Sięgaj po `var` dopiero, kiedy wymaga tego działanie programu. Nie oznacza to, że obiekt przechowywany przez `val` zawsze jest całkowicie niemodyfikowalny — do tego zagadnienia wrócimy przy kolekcjach i klasach.

### Błąd: próba zmiany `val`

Ten fragment jest **celowo błędny**. Nie uruchamiaj go jako gotowego przykładu:

```kotlin
val serverName = "CraftWorld"
serverName = "NewWorld" // BŁĄD: val cannot be reassigned
```

Kotlin odrzuci próbę ponownego przypisania. Gdy naprawdę musisz zmienić wartość, zastosuj `var`; gdy nie musisz — pozostań przy `val`.

## 3. Czym jest typ danych?

**Typ danych** informuje Kotlin, jaki rodzaj wartości znajduje się pod daną nazwą i jakie operacje mają dla niej sens.

Na przykład:

- nick gracza jest **tekstem**;
- liczba poziomów jest **liczbą całkowitą**;
- średni czas wykonania zadania może zawierać **część ułamkową**;
- informacja „serwer jest w trybie konserwacji” ma wartość **prawda albo fałsz**.

Przykład — sześć najważniejszych typów w jednym programie:

```kotlin-run
fun main() {
    val playerName: String = "Alex"
    val rankSymbol: Char = 'A'
    val level: Int = 25
    val totalVisits: Long = 3_500_000_000L
    val serverTps: Double = 19.75
    val maintenanceMode: Boolean = false

    println("Gracz: $playerName")
    println("Symbol rangi: $rankSymbol")
    println("Poziom: $level")
    println("Wizyty łącznie: $totalVisits")
    println("TPS serwera: $serverTps")
    println("Tryb konserwacji: $maintenanceMode")
}
```

Wynik:

```text
Gracz: Alex
Symbol rangi: A
Poziom: 25
Wizyty łącznie: 3500000000
TPS serwera: 19.75
Tryb konserwacji: false
```

### Tabela typów, do której warto wracać

| Typ | Co przechowuje? | Przykładowa wartość | Typowe zastosowanie |
|---|---|---|---|
| `String` | Tekst, także wiele znaków | `"Alex"` | Nick, opis, nazwa serwera |
| `Char` | Pojedynczy znak | `'A'` | Symbol, jedna litera |
| `Int` | Liczba całkowita | `25` | Poziom, licznik, ilość |
| `Long` | Duża liczba całkowita | `3_500_000_000L` | Bardzo duże liczniki |
| `Double` | Liczba z częścią ułamkową | `19.75` | Pomiar, cena, wynik z ułamkiem |
| `Boolean` | Prawda lub fałsz | `true`, `false` | Włączona/wyłączona opcja |

**Kilka ważnych szczegółów:**

- `String` zapisujemy w **podwójnych cudzysłowach**: `"A"` to tekst długości 1.
- `Char` zapisujemy w **pojedynczych apostrofach**: `'A'` to pojedynczy znak.
- W zapisie liczb dziesiętnych używamy **kropki**, a nie przecinka: `19.75`.
- Litera `L` przy liczbie oznacza literał typu `Long`. Podkreślenia w `3_500_000_000L` jedynie ułatwiają czytanie; nie są częścią wartości.
- `Boolean` przyjmuje `true` i `false` bez cudzysłowów. Napis `"true"` jest tekstem, a nie wartością logiczną.
- Istnieją także inne typy, np. `Float`, `Byte` i `Short`. Na razie wystarczy znać powyższą szóstkę.

## 4. Czy trzeba zawsze wpisywać typ po dwukropku?

Nie. Kotlin często rozpoznaje typ **na podstawie wartości po znaku `=`**. Nazywa się to **wnioskowaniem typu** (*type inference*).

```kotlin-run
fun main() {
    val nickname = "Alex"       // Kotlin rozpoznaje String
    val level = 12             // Kotlin rozpoznaje Int
    val ratio = 0.75           // Kotlin rozpoznaje Double
    val isOnline = true        // Kotlin rozpoznaje Boolean

    println("$nickname ma poziom $level")
    println("Współczynnik: $ratio")
    println("Online: $isOnline")
}
```

Wynik:

```text
Alex ma poziom 12
Współczynnik: 0.75
Online: true
```

Możesz również zapisać typ **jawnie**, po dwukropku:

```kotlin-run
fun main() {
    val nickname: String = "Alex"
    val level: Int = 12
    val ratio: Double = 0.75
    val isOnline: Boolean = true

    println("Gracz: $nickname, poziom: $level")
    println("Współczynnik: $ratio, online: $isOnline")
}
```

Obie formy są poprawne. Jawne określenie typu przydaje się, gdy chcesz mieć pełną kontrolę nad sposobem przechowywania danych albo wyjaśnić zamiar swojego kodu. W codziennym kodzie można często pozwolić Kotlinowi rozpoznać typ.

**Istotne:** `var` pozwala zmieniać **wartość**, ale nie zmienia ustalonego **typu** zmiennej.

```kotlin
var level = 12
level = "dwanaście" // BŁĄD: level ma typ Int, a to jest String
```

To właśnie jeden z powodów, dla których typy pomagają wykrywać błędy, zanim program zostanie uruchomiony.

## 5. Tekst, liczba i pojedynczy znak — pozornie podobne, ale różne

Spójrz na program:

```kotlin-run
fun main() {
    val playerLevel = 15
    val levelAsText = "15"
    val grade = 'A'
    val gradeAsText = "A"

    println("Poziom jako liczba: $playerLevel")
    println("Poziom jako tekst: $levelAsText")
    println("Ranga jako znak: $grade")
    println("Ranga jako tekst: $gradeAsText")
}
```

Wynik:

```text
Poziom jako liczba: 15
Poziom jako tekst: 15
Ranga jako znak: A
Ranga jako tekst: A
```

Wypisany wynik nie pokazuje różnicy, ale program **zna typ każdej wartości**. To ważne, ponieważ liczby można wykorzystywać do obliczeń, a tekst — do komunikatów i przetwarzania znaków. W następnej lekcji poznasz operatory, które pozwolą wykonywać takie obliczenia.

### Uwaga na nazwy zmiennych

Dobre nazwy mówią, co jest przechowywane: `onlinePlayers`, `playerLevel`, `serverName`, `maintenanceMode`. Kotlin rozróżnia wielkość liter: `playerName` i `playername` to **dwie różne nazwy**.

Przyjmij na początku prostą zasadę: nazwy zmiennych pisz w języku angielskim i stosuj styl `camelCase` — pierwsze słowo małą literą, kolejne zaczynają się wielką, np. `maxPlayers`.

## 6. `String` w praktyce — czytelne komunikaty

Znasz już `$nazwa`. Dziś poznasz drugi zapis: `${...}`.

```kotlin-run
fun main() {
    val nickname = "Alex"
    val serverName = "CraftWorld"

    println("Witaj, $nickname!")
    println("Twój nick ma ${nickname.length} znaki.")
    println("Nazwa serwera wielkimi literami: ${serverName.uppercase()}")
    println("Twoja odznaka: ${nickname}_VIP")
}
```

Wynik:

```text
Witaj, Alex!
Twój nick ma 4 znaki.
Nazwa serwera wielkimi literami: CRAFTWORLD
Twoja odznaka: Alex_VIP
```

### Dlaczego czasami potrzebne są nawiasy klamrowe?

- `$nickname` — wystarczy, gdy chcesz wstawić jedną znaną wartość i dalszy tekst nie należy do jej nazwy.
- `${nickname.length}` — potrzebne, gdy korzystasz z właściwości (tutaj: liczby znaków w tekście).
- `${serverName.uppercase()}` — potrzebne, gdy chcesz wstawić wynik działania metody na tekście.
- `${nickname}_VIP` — nawiasy klamrowe jasno oddzielają nazwę zmiennej od dalszych liter, które mają znaleźć się w wyniku.

**Nowe pojęcia, bez zagłębiania się jeszcze w ich budowę:**

| Zapis | Co robi? |
|---|---|
| `nickname.length` | Odczytuje liczbę znaków w napisie |
| `serverName.uppercase()` | Zwraca wersję tekstu zapisaną wielkimi literami |
| `serverName.lowercase()` | Zwraca wersję tekstu zapisaną małymi literami |
| `serverName.trim()` | Usuwa białe znaki z początku i końca tekstu, np. zbędne spacje |

Warto zauważyć, że wyniki tych operacji **nie zmieniają automatycznie tekstu przechowywanego w `val`**. Tworzą wynik, który możemy wypisać lub zapisać osobno.

### Mały eksperyment: spacje i wielkość liter

```kotlin-run
fun main() {
    val rawNickname = "   Alex   "
    val cleanNickname = rawNickname.trim()

    println("Oryginał: [$rawNickname]")
    println("Po usunięciu spacji: [$cleanNickname]")
    println("Wersja wielkimi literami: ${cleanNickname.uppercase()}")
}
```

Wynik:

```text
Oryginał: [   Alex   ]
Po usunięciu spacji: [Alex]
Wersja wielkimi literami: ALEX
```

Nawiasy kwadratowe w `println()` nie mają tu żadnego specjalnego znaczenia — po prostu ułatwiają zauważenie spacji.

## 7. Konwersja typów — kiedy dane muszą zmienić postać

**Konwersja** to świadome uzyskanie wartości innego typu. Jest potrzebna np. wtedy, gdy liczba przychodzi jako tekst, a chcesz potraktować ją jako liczbę.

### 7.1. Liczba całkowita → liczba ułamkowa

```kotlin-run
fun main() {
    val level: Int = 25
    val levelAsDouble: Double = level.toDouble()

    println(level)
    println(levelAsDouble)
}
```

Wynik:

```text
25
25.0
```

Użyliśmy `toDouble()`, aby uzyskać wartość typu `Double`. Kotlin nie wykonuje automatycznie każdej konwersji liczbowej podczas **przypisania** — np. nie można po prostu przypisać zmiennej `Int` do zmiennej `Double` bez odpowiedniego przekształcenia.

### 7.2. Liczba ułamkowa → liczba całkowita

```kotlin-run
fun main() {
    val measuredTps = 19.85
    val wholeTps = measuredTps.toInt()

    println("Pomiar: $measuredTps")
    println("Część całkowita: $wholeTps")
}
```

Wynik:

```text
Pomiar: 19.85
Część całkowita: 19
```

**Ważne:** `toInt()` zastosowane do `Double` **odrzuca część ułamkową**, a nie zaokrągla do najbliższej liczby. Na przykład `19.85.toInt()` daje `19`.

### 7.3. Tekst → liczba

```kotlin-run
fun main() {
    val playersAsText = "24"
    val playersAsNumber = playersAsText.toInt()

    println("Tekstowa liczba graczy: $playersAsText")
    println("Liczba graczy: $playersAsNumber")
}
```

Wynik:

```text
Tekstowa liczba graczy: 24
Liczba graczy: 24
```

`toInt()` działa dla napisu zawierającego poprawnie zapisaną liczbę całkowitą. **Nie zadziała dla tekstu `"dwadzieścia cztery"` albo `"24.5"`** — taka próba spowoduje błąd w trakcie działania programu. W lekcji 3 poznamy pobieranie danych od użytkownika i bezpieczne sprawdzanie wpisywanych liczb przez `toIntOrNull()`.

### 7.4. Liczba → tekst

```kotlin-run
fun main() {
    val playerLevel = 24
    val levelAsText: String = playerLevel.toString()

    println("Poziom: $levelAsText")
    println("Tekst ma ${levelAsText.length} znaki.")
}
```

Wynik:

```text
Poziom: 24
Tekst ma 2 znaki.
```

**Wskazówka:** wewnątrz szablonu tekstu `$playerLevel` Kotlin potrafi sam przygotować tekstową reprezentację liczby. Nie musisz wtedy wywoływać `toString()` ręcznie.

### Najważniejsze konwersje do zapamiętania

| Zapis | Przeznaczenie |
|---|---|
| `wartosc.toInt()` | Otrzymanie `Int`, jeśli konwersja jest możliwa |
| `wartosc.toLong()` | Otrzymanie `Long` |
| `wartosc.toDouble()` | Otrzymanie `Double` |
| `wartosc.toString()` | Tekstowa postać wartości |

**Nie myl konwersji z przechowywaniem danych:** po zapisaniu `val converted = original.toInt()` oryginalna zmienna `original` zachowuje swój dotychczasowy typ i wartość.

## 8. `var` w działaniu: raport przed zmianą i po zmianie

Czas na przykład pokazujący, jak kilka poznanych elementów współpracuje:

```kotlin-run
fun main() {
    val serverName: String = "CraftWorld"
    val maxPlayers: Int = 50
    var onlinePlayers: Int = 12
    var messageOfTheDay: String = "Witaj na serwerze!"
    val maintenanceMode: Boolean = false

    println("=== PANEL SERWERA ===")
    println("Serwer: $serverName")
    println("Gracze: $onlinePlayers / $maxPlayers")
    println("Wiadomość: $messageOfTheDay")
    println("Konserwacja: $maintenanceMode")

    // Symulujemy aktualizację stanu serwera.
    val newPlayerCount = "18"
    onlinePlayers = newPlayerCount.toInt()
    messageOfTheDay = "Miłej gry!"

    println("--- PO AKTUALIZACJI ---")
    println("Gracze: $onlinePlayers / $maxPlayers")
    println("Wiadomość: $messageOfTheDay")
}
```

Wynik:

```text
=== PANEL SERWERA ===
Serwer: CraftWorld
Gracze: 12 / 50
Wiadomość: Witaj na serwerze!
Konserwacja: false
--- PO AKTUALIZACJI ---
Gracze: 18 / 50
Wiadomość: Miłej gry!
```

**Co tu się stało?**

1. `serverName` oraz `maxPlayers` są wartościami, których nie zmieniamy — dlatego używamy `val`.
2. `onlinePlayers` i `messageOfTheDay` są deklarowane jako `var`, ponieważ ich zawartość później aktualizujemy.
3. `maintenanceMode` jest wartością logiczną. Na razie ją tylko wyświetlamy; instrukcji warunkowych nauczymy się w lekcji 4.
4. `newPlayerCount` ma typ `String`, bo zapisaliśmy `"18"` w cudzysłowach.
5. `newPlayerCount.toInt()` zamienia prawidłowy tekst liczbowy na `Int`, więc może on zostać przypisany do `onlinePlayers`.
6. Drugie wypisanie panelu korzysta z **nowych wartości** bez tworzenia kolejnych zmiennych o tych samych nazwach.

**Samodzielna próba:** zmień początkową liczbę graczy na `5`, nową liczbę na `"32"`, a wiadomość na własną. Sprawdź, czy oba raporty odpowiadają wpisanym wartościom.

## 9. Najczęstsze błędy początkujących

| Objaw | Co zwykle się stało? | Jak naprawić? |
|---|---|---|
| `Val cannot be reassigned` | Próbujesz zmienić `val` | Zastanów się, czy zmiana jest konieczna; jeśli tak, użyj `var` |
| `Initializer type mismatch` | Wartość nie odpowiada zadeklarowanemu typowi | Sprawdź cudzysłowy, apostrofy i typ po dwukropku |
| `Argument type mismatch` | Funkcja oczekuje innego typu | Przekaż wartość właściwego typu lub wykonaj konwersję |
| `Unresolved reference` | Literówka w nazwie lub brak deklaracji | Sprawdź pisownię i wielkość liter |
| Program kończy się błędem przy `toInt()` | Tekst nie jest poprawną liczbą całkowitą | Na razie użyj poprawnego tekstu, np. `"24"`; walidacja będzie w lekcji 3 |
| Widzisz `24.0` zamiast `24` | Wartość ma typ `Double` | To oczekiwany zapis liczby ułamkowej |
| `"true"` nie zachowuje się jak `true` | Pierwsza wartość jest tekstem | Użyj typu `Boolean`, jeśli przechowujesz prawdę/fałsz |
| Zmienna `level` nie przyjmuje tekstu | Typ `var` nie zmienia się po deklaracji | Zmień projekt danych lub przekonwertuj wartość |
| Napis zawiera dziwny fragment przy `$` | Błędne oddzielenie nazwy zmiennej | Użyj `${nazwa}`, gdy dodajesz litery bezpośrednio za nazwą |

### Jak samodzielnie szukać rozwiązania?

1. Odczytaj **pierwszy** komunikat błędu w IntelliJ IDEA.
2. Sprawdź, czy problem dotyczy `val`, `var`, typu danych albo cudzysłowów.
3. Zlokalizuj linię, na którą wskazuje IDE.
4. Zmieniaj po jednym elemencie i uruchamiaj program ponownie.
5. Nie poprawiaj błędu przez przypadkową zmianę wszystkiego na `String` — typy pełnią ważną funkcję.

## 10. Zadania do samodzielnego wykonania

**Zasada:** najpierw napisz rozwiązanie w `Main.kt` i uruchom je samodzielnie. Dopiero później sprawdź odpowiedzi na końcu lekcji. Kod do analizy i uszkodzone programy mają zwykły znacznik `kotlin`, aby nie pomylić ich z gotowym przykładem.

### Zadanie 1 — Stałe i zmienne (łatwe)

Napisz program, w którym:

- nazwa serwera jest przechowywana w `val`;
- liczba graczy jest przechowywana w `var`;
- wypisujesz obie wartości;
- zmieniasz liczbę graczy;
- ponownie wypisujesz liczbę graczy.

**Oczekiwane zachowanie:** nazwa serwera pozostaje niezmieniona, liczba graczy się zmienia.

### Zadanie 2 — Sześć typów danych (łatwe)

Utwórz i wyświetl sześć wartości z jawnymi typami: `String`, `Char`, `Int`, `Long`, `Double` i `Boolean`. Wybierz własne przykłady związane z Minecraftem. Nie kopiuj wartości z tabeli w sekcji 3.

### Zadanie 3 — Tekst czy liczba? (łatwe)

Zapisz w kodzie dwa poziomy gracza: `levelNumber = 42` oraz `levelText = "42"`. Wyświetl każdą z wartości. W komentarzu wyjaśnij, jaki typ ma każda z nich i dlaczego.

### Zadanie 4 — Zmieniamy format danych (średnie)

Utwórz `val pointsAsText = "150"`, przekonwertuj tę wartość na `Int` i wypisz komunikat `Gracz zdobył 150 punktów.`. Następnie przekonwertuj liczbę punktów na `Double` i wypisz `Punkty jako Double: 150.0`.

### Zadanie 5 — Przewidź wynik (średnie)

**Nie uruchamiaj programu przed zapisaniem odpowiedzi.** Podaj dokładnie pięć wierszy wyniku:

```kotlin
fun main() {
    var online = 5
    val server = "BlockLand"
    println("$server: $online")
    online = 7
    println("$server: $online")
    println("${server.uppercase()}_PL")
    println("${server.length}")
    println("${online.toDouble()}")
}
```

### Zadanie 6 — Napraw program (średnie)

Kod celowo zawiera **trzy błędy**: błąd ponownego przypisania, niezgodny typ i pomylenie `Char` ze `String`. Popraw go bez usuwania informacji o poziomie i symbolu rangi:

```kotlin
fun main() {
    val level = 10
    level = 11
    var nickname: String = 123
    val rankSymbol: Char = "A"

    println("$nickname — poziom $level, ranga $rankSymbol")
}
```

### Zadanie 7 — Własne szablony tekstu (średnie)

Utwórz `val nickname = "Steve"` i `val serverName = "GreenValley"`. Wyświetl:

```text
Witaj, Steve!
Twój nick ma 5 znaków.
Serwer: GREENVALLEY
Odznaka: Steve_VIP
```

Wymagania: skorzystaj z `length`, `uppercase()` oraz `${...}` przynajmniej dwa razy.

### Zadanie 8 — Mini-projekt: profil serwera przed aktualizacją i po niej (trudniejsze)

Napisz samodzielny program wyświetlający dwie wersje panelu Minecraft. **Nie kopiuj całego rozwiązania z sekcji 8** — wybierz własną nazwę serwera, nick i wartości.

Wymagania:

1. Utwórz `val serverName: String` i `val maxPlayers: Int`.
2. Utwórz `var onlinePlayers: Int` i `var playerRank: String`.
3. Dodaj `val pvpEnabled: Boolean` oraz `val rankCode: Char`.
4. W pierwszej części wyświetl co najmniej: nazwę serwera, liczbę graczy, nick, rangę, kod rangi i informację o PvP.
5. Zapisz nową liczbę graczy jako tekst (np. `"20"`) i przekonwertuj ją na `Int` przed aktualizacją `onlinePlayers`.
6. Zmień także rangę przechowywaną w `playerRank`.
7. W drugiej części wyświetl nową liczbę graczy i nową rangę.
8. Wykorzystaj minimum cztery szablony tekstu i jeden komentarz wyjaśniający **po co** wykonujesz aktualizację.

**Warunek zaliczenia:** program uruchamia się bez błędów i pokazuje różne dane przed aktualizacją oraz po niej.

## 11. Mini-test — czy rozumiesz materiał?

Zapisz odpowiedzi, **zanim** przewiniesz do rozwiązań.

1. Co odróżnia `val` od `var`?
2. Czy `var level = 10` pozwala później przypisać `"dziesięć"` do `level`? Dlaczego?
3. Jaki typ ma `"A"`, a jaki `'A'`?
4. Jaki typ Kotlin zwykle wywnioskuje z `val score = 25`?
5. Jaki typ Kotlin zwykle wywnioskuje z `val tps = 19.5`?
6. Jak zapisać wartość logiczną oznaczającą prawdę?
7. Co wypisze `println("${20.9.toInt()}")`?
8. Co robi `"56".toInt()` i kiedy może spowodować błąd?
9. Do czego służą nawiasy klamrowe w zapisie `${nickname.length}`?
10. Czy `val name = "Alex"` pozwala wywołać `name.uppercase()` bez zmiany samej wartości `name`?

### Warunek przejścia do lekcji 3

Przejdź dalej, jeśli umiesz bez podpowiedzi:

- napisać program używający jednocześnie `val` i `var`;
- świadomie dobrać typ dla informacji o graczu;
- zamienić `"25"` na liczbę, a liczbę na `Double`;
- zbudować poprawny komunikat z `$nazwa` i `${...}`;
- znaleźć i naprawić przynajmniej dwa błędy typów.

## 12. Podsumowanie — co zapamiętać?

`val` ogranicza ponowne przypisanie wartości, a `var` pozwala je wykonać. **Oba** rozwiązania mają ustalony typ. Typy określają, jakie dane przechowujemy: `String`, `Char`, `Int`, `Long`, `Double` i `Boolean`. Kotlin potrafi często rozpoznać typ sam, ale czasem warto zapisać go po dwukropku. Szablony tekstu pozwalają budować komunikaty, a konwersje pomagają poprawnie przechodzić między różnymi postaciami danych.

**Następna lekcja (3/36): Operatory i wejście użytkownika.** Nauczysz się wykonywać działania matematyczne, porównywać wartości, pobierać informacje z klawiatury oraz bezpiecznie interpretować wpisane liczby.

---

## 13. Odpowiedzi i przykładowe rozwiązania

> **Stop!** To sekcja do sprawdzania własnej pracy. Wróć do niej dopiero po samodzielnym rozwiązaniu zadań. Poniższe gotowe programy mają znacznik `kotlin-run` — nadają się do uruchomienia i porównania wyników.

### Zadanie 1 — przykładowe rozwiązanie

```kotlin-run
fun main() {
    val serverName = "CraftWorld"
    var onlinePlayers = 6

    println("Serwer: $serverName")
    println("Gracze: $onlinePlayers")

    onlinePlayers = 8
    println("Gracze po aktualizacji: $onlinePlayers")
}
```

### Zadanie 2 — przykładowe rozwiązanie

```kotlin-run
fun main() {
    val nickname: String = "Mila"
    val rank: Char = 'M'
    val level: Int = 14
    val visits: Long = 8_000_000_000L
    val speed: Double = 4.5
    val isOnline: Boolean = true

    println("Nick: $nickname")
    println("Kod rangi: $rank")
    println("Poziom: $level")
    println("Wizyty: $visits")
    println("Prędkość: $speed")
    println("Online: $isOnline")
}
```

### Zadanie 3 — przykładowe rozwiązanie

```kotlin-run
fun main() {
    val levelNumber = 42 // Int: liczba całkowita
    val levelText = "42" // String: cyfry zapisane jako tekst

    println("Poziom jako liczba: $levelNumber")
    println("Poziom jako tekst: $levelText")
}
```

### Zadanie 4 — przykładowe rozwiązanie

```kotlin-run
fun main() {
    val pointsAsText = "150"
    val points = pointsAsText.toInt()
    val pointsAsDouble = points.toDouble()

    println("Gracz zdobył $points punktów.")
    println("Punkty jako Double: $pointsAsDouble")
}
```

Wynik:

```text
Gracz zdobył 150 punktów.
Punkty jako Double: 150.0
```

### Zadanie 5 — poprawne przewidywanie wyniku

```text
BlockLand: 5
BlockLand: 7
BLOCKLAND_PL
9
7.0
```

Dlaczego? Nazwa `BlockLand` ma 9 znaków, `online` po zmianie ma wartość `7`, a `toDouble()` przekształca ją na `7.0`.

### Zadanie 6 — poprawiona wersja

```kotlin-run
fun main() {
    var level = 10
    level = 11
    val nickname: String = "Steve"
    val rankSymbol: Char = 'A'

    println("$nickname — poziom $level, ranga $rankSymbol")
}
```

Wynik:

```text
Steve — poziom 11, ranga A
```

Poprawiliśmy `val` na `var`, liczbę `123` na tekstowy nick i `"A"` na znak `'A'`.

### Zadanie 7 — przykładowe rozwiązanie

```kotlin-run
fun main() {
    val nickname = "Steve"
    val serverName = "GreenValley"

    println("Witaj, $nickname!")
    println("Twój nick ma ${nickname.length} znaków.")
    println("Serwer: ${serverName.uppercase()}")
    println("Odznaka: ${nickname}_VIP")
}
```

### Zadanie 8 — przykładowy projekt

```kotlin-run
fun main() {
    val serverName: String = "OakCraft"
    val playerName: String = "Mila"
    val maxPlayers: Int = 60
    var onlinePlayers: Int = 11
    var playerRank: String = "Gracz"
    val pvpEnabled: Boolean = true
    val rankCode: Char = 'G'

    println("===== PANEL SERWERA =====")
    println("Serwer: $serverName")
    println("Nick: $playerName")
    println("Gracze: $onlinePlayers / $maxPlayers")
    println("Ranga: $playerRank ($rankCode)")
    println("PvP włączone: $pvpEnabled")

    // Aktualizujemy symulowany stan po dołączeniu kolejnych graczy.
    val newOnlineText = "20"
    onlinePlayers = newOnlineText.toInt()
    playerRank = "Budowniczy"

    println("----- PO AKTUALIZACJI -----")
    println("Gracze: $onlinePlayers / $maxPlayers")
    println("Nowa ranga $playerName: $playerRank")
}
```

Przykładowy wynik:

```text
===== PANEL SERWERA =====
Serwer: OakCraft
Nick: Mila
Gracze: 11 / 60
Ranga: Gracz (G)
PvP włączone: true
----- PO AKTUALIZACJI -----
Gracze: 20 / 60
Nowa ranga Mila: Budowniczy
```

Nie musisz otrzymać identycznych nazw i liczb. Liczy się poprawne wykorzystanie typów, zmiana wyłącznie właściwych zmiennych oraz zgodność wyniku z Twoimi danymi.

### Mini-test — odpowiedzi

1. `val` nie pozwala ponownie przypisać wartości do tej samej nazwy; `var` na to pozwala.
2. Nie. `level` ma typ `Int`, więc nie może przechowywać `String`.
3. `"A"` to `String`, a `'A'` to `Char`.
4. `Int`.
5. `Double`.
6. `true` — bez cudzysłowów.
7. `20`, ponieważ `toInt()` odrzuca część ułamkową.
8. Zmienia prawidłowo zapisany tekst liczbowy na `Int`; dla niepoprawnego tekstu może zgłosić wyjątek podczas działania programu.
9. Pozwalają użyć bardziej rozbudowanego wyrażenia w szablonie tekstu — tutaj odczytać długość napisu.
10. Tak. Metoda zwraca nowy tekst wielkimi literami; `name` nadal zawiera `"Alex"`.

---

## Dodatkowe źródła (dla chętnych)

- [Kotlin — przegląd typów](https://kotlinlang.org/docs/types-overview.html)
- [Kotlin — liczby i konwersje](https://kotlinlang.org/docs/numbers.html)
- [Kotlin — tekst i szablony tekstu](https://kotlinlang.org/docs/strings.html)
- [Kotlin — podstawowa składnia](https://kotlinlang.org/docs/basic-syntax.html)

**Koniec lekcji 2/36. Następna: operatory i wprowadzanie danych przez użytkownika.**