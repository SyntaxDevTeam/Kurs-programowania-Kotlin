# Lekcja 3 — Operatory i wprowadzanie danych przez użytkownika

**Poziom:** I — Fundamenty Kotlina  
**Lekcja:** 3 z 8 w Poziomie I (3 z 36 w całym kursie)  
**Temat:** obliczenia, porównywanie wartości, kolejność działań, `readln()`, konwersje i bezpieczne sprawdzanie danych  
**Czas:** około 90–120 minut (z ćwiczeniami i mini-projektem)  
**Potrzebujesz:** IntelliJ IDEA, JDK 25 i działający projekt Kotlin/JVM z lekcji 1  
**Wymagana wiedza:** lekcje 1–2: `fun main()`, `println()`, `val`, `var`, `Int`, `Double`, `String`, szablony tekstu i podstawowe konwersje

---

> **Jak korzystać z kodu?** Pełne programy i rozwiązania mają oznaczenie `kotlin-run`. Krótsze fragmenty, zadania do przewidzenia i kod z celowymi błędami mają oznaczenie `kotlin`. Każdy gotowy program możesz też wkleić do `Main.kt` w IntelliJ IDEA.
>
> **Uwaga o wpisywaniu danych:** przykłady z `readln()` **czekają na tekst od użytkownika**. Nie każdy interpreter uruchamiany w przeglądarce ma możliwość przekazywania danych na standardowe wejście. Jeśli edytor na stronie nie udostępnia pola wejścia programu, uruchom te przykłady w **IntelliJ IDEA** i wpisuj odpowiedzi w oknie **Run**. To ograniczenie środowiska uruchomieniowego, a nie błąd kodu.
>
> **Ścieżka kursu:** dziś piszemy programy konsolowe. Pierwszy plugin będzie używał **Spigot API** (lekcja 16), a później poznamy **Paper API** (od lekcji 25).

## 1. Od wyświetlania informacji do podejmowania obliczeń

W poprzedniej lekcji zapisywaliśmy informacje, na przykład liczbę graczy czy nazwę serwera. Teraz nauczysz się **obliczać nowe wartości** i **porównywać dane**. Co ważne, program przestanie korzystać wyłącznie z liczb i napisów wpisanych przez autora: poprosi użytkownika o informacje.

Po lekcji potrafisz:

- wykonać dodawanie, odejmowanie, mnożenie, dzielenie i obliczyć resztę z dzielenia;
- rozpoznać różnicę między **dzieleniem liczb całkowitych** a dzieleniem dającym wynik ułamkowy;
- przewidzieć kolejność działań i użyć nawiasów do jej zmiany;
- zastosować `>`, `<`, `>=`, `<=`, `==` i `!=`;
- odróżnić `=` (przypisanie) od `==` (porównanie);
- zmienić wartość `var` za pomocą `+=`, `-=`, `++` i `--`;
- odczytać dane od użytkownika przez `readln()`;
- wyjaśnić, czemu tekst wpisany w konsoli **nie jest od razu liczbą**;
- użyć `toIntOrNull()` i `toDoubleOrNull()`, by uniknąć awarii przy niepoprawnym wpisie;
- stworzyć prosty **kalkulator sklepu serwerowego**.

## 2. Pięć podstawowych operatorów matematycznych

**Operator** to znak wykonujący określoną operację na wartościach. Kotlin obsługuje standardowe działania matematyczne:

| Operator | Działanie | Przykład | Wynik |
|---|---|---|---|
| `+` | dodawanie | `8 + 3` | `11` |
| `-` | odejmowanie | `8 - 3` | `5` |
| `*` | mnożenie | `8 * 3` | `24` |
| `/` | dzielenie | `8 / 2` | `4` |
| `%` | reszta z dzielenia | `8 % 3` | `2` |

Uruchom kompletny program:

```kotlin-run
fun main() {
    val a = 8
    val b = 3

    println("Dodawanie: ${a + b}")
    println("Odejmowanie: ${a - b}")
    println("Mnożenie: ${a * b}")
    println("Dzielenie całkowite: ${a / b}")
    println("Reszta z dzielenia: ${a % b}")
}
```

Wynik:

```text
Dodawanie: 11
Odejmowanie: 5
Mnożenie: 24
Dzielenie całkowite: 2
Reszta z dzielenia: 2
```

**Krok po kroku:**

1. `a` przechowuje liczbę `8`, a `b` liczbę `3`.
2. `${a + b}` oznacza: oblicz sumę i wstaw ją do tekstu.
3. `${a * b}` daje `24`. Do mnożenia używamy `*`, a nie znaku `x`.
4. `8 / 3` daje **`2`**, ponieważ obie liczby mają typ całkowity.
5. `8 % 3` zwraca **`2`**, czyli to, co pozostanie po odjęciu dwóch pełnych trójek od ósemki.

### Reszta z dzielenia `%` — do czego służy?

Przykład z Minecrafta: 25 sztuk przedmiotów można rozłożyć na pełne stosy po 8 sztuk.

```kotlin-run
fun main() {
    val items = 25
    val stackSize = 8

    val fullStacks = items / stackSize
    val remainingItems = items % stackSize

    println("Pełne stosy: $fullStacks")
    println("Pozostałe przedmioty: $remainingItems")
}
```

Wynik:

```text
Pełne stosy: 3
Pozostałe przedmioty: 1
```

To nie jest osobny mechanizm Minecrafta, tylko zwykła matematyka przydatna w przyszłych pluginach.

## 3. Bardzo ważne: `Int / Int` nie daje części ułamkowej

Zobacz, dlaczego rodzaj liczb ma znaczenie:

```kotlin-run
fun main() {
    println(7 / 2)
    println(7 / 2.0)
    println(7.0 / 2)
}
```

Wynik:

```text
3
3.5
3.5
```

Kotlin wykonuje **dzielenie całkowite**, jeśli oba argumenty są liczbami całkowitymi: `Int / Int` daje `Int`. Część ułamkowa zostaje odrzucona — **bez zaokrąglania**.

Chcesz ułamek? Zadbaj, aby co najmniej jeden argument był `Double`:

```kotlin-run
fun main() {
    val onlinePlayers = 7
    val groups = 2

    val incorrectAverage = onlinePlayers / groups
    val correctAverage = onlinePlayers.toDouble() / groups

    println("Wynik całkowity: $incorrectAverage")
    println("Średnia: $correctAverage")
}
```

Wynik:

```text
Wynik całkowity: 3
Średnia: 3.5
```

**Zapamiętaj:** przypisanie wyniku całkowitego dzielenia do zmiennej typu `Double` **nie odzyska** utraconej części ułamkowej. Konwersja musi nastąpić **przed dzieleniem**.

> Nie próbuj dzielić liczb całkowitych przez zero. Wyrażenie `10 / 0` spowoduje błąd podczas działania programu. Kontrolę takich sytuacji przećwiczymy w dalszej części tej lekcji.

## 4. Kolejność działań i nawiasy

Program działa zgodnie z zasadami kolejności działań matematycznych: mnożenie, dzielenie i reszta z dzielenia mają wyższy priorytet niż dodawanie i odejmowanie. Nawiasy pozwalają zmienić tę kolejność.

```kotlin-run
fun main() {
    println(2 + 3 * 4)
    println((2 + 3) * 4)
    println(10 - 3 - 2)
}
```

Wynik:

```text
14
20
5
```

Wyjaśnienie:

- `2 + 3 * 4` → najpierw `3 * 4 = 12`, następnie `2 + 12 = 14`.
- `(2 + 3) * 4` → najpierw `5`, następnie `5 * 4 = 20`.
- `10 - 3 - 2` → działania o tym samym priorytecie wykonujemy tutaj od lewej: `7 - 2 = 5`.

### Obliczanie ceny zakupów

```kotlin-run
fun main() {
    val diamonds = 4
    val pricePerDiamond = 15
    val deliveryFee = 5

    val total = diamonds * pricePerDiamond + deliveryFee
    println("Koszt zakupów: $total monet")
}
```

Wynik:

```text
Koszt zakupów: 65 monet
```

**Eksperyment:** zmień `diamonds` z `4` na `6`. Jaki będzie nowy wynik? Nie poprawiaj ręcznie wartości `total` — powinna obliczyć się automatycznie.

## 5. Skrócony zapis zmiany zmiennej `var`

W lekcji 2 poznałaś `var`, czyli wartość, którą wolno ponownie przypisać. Gdy do liczby graczy dołącza jedna osoba, możemy zapisać:

```kotlin-run
fun main() {
    var onlinePlayers = 10

    onlinePlayers = onlinePlayers + 1
    println("Po dołączeniu: $onlinePlayers")

    onlinePlayers += 2
    println("Po dołączeniu dwóch osób: $onlinePlayers")

    onlinePlayers -= 1
    println("Po wyjściu jednej osoby: $onlinePlayers")
}
```

Wynik:

```text
Po dołączeniu: 11
Po dołączeniu dwóch osób: 13
Po wyjściu jednej osoby: 12
```

Równoważne zapisy dla zmiennych liczbowych:

| Dłuższy zapis | Krótszy zapis | Znaczenie |
|---|---|---|
| `points = points + 5` | `points += 5` | dodaj 5 |
| `points = points - 3` | `points -= 3` | odejmij 3 |
| `points = points * 2` | `points *= 2` | pomnóż przez 2 |
| `points = points / 2` | `points /= 2` | podziel przez 2 |

Jest także zapis `++` (zwiększ o 1) i `--` (zmniejsz o 1):

```kotlin-run
fun main() {
    var lives = 3
    lives--
    println("Życia po porażce: $lives")

    lives++
    println("Życia po nagrodzie: $lives")
}
```

Wynik:

```text
Życia po porażce: 2
Życia po nagrodzie: 3
```

Na razie zapisuj `++` i `--` jako **osobne instrukcje**. Umieszczanie ich wewnątrz innych wyrażeń może być mniej czytelne; nie jest potrzebne w tym kursie na początek.

## 6. Operatory porównania — wynik `true` albo `false`

Operator porównania **nie zmienia** danych. Zwraca wartość typu `Boolean`, czyli `true` lub `false`.

| Operator | Oznacza | Przykład z wynikiem |
|---|---|---|
| `>` | większe niż | `10 > 5` → `true` |
| `<` | mniejsze niż | `10 < 5` → `false` |
| `>=` | większe lub równe | `10 >= 10` → `true` |
| `<=` | mniejsze lub równe | `9 <= 10` → `true` |
| `==` | równe | `10 == 10` → `true` |
| `!=` | różne | `10 != 10` → `false` |

```kotlin-run
fun main() {
    val onlinePlayers = 15
    val maxPlayers = 20

    println("Czy są wolne miejsca? ${onlinePlayers < maxPlayers}")
    println("Czy limit osiągnięty? ${onlinePlayers >= maxPlayers}")
    println("Czy dokładnie 15 graczy? ${onlinePlayers == 15}")
}
```

Wynik:

```text
Czy są wolne miejsca? true
Czy limit osiągnięty? false
Czy dokładnie 15 graczy? true
```

Wynik wygląda jak zwykła informacja w konsoli, ale w lekcji 4 nauczymy się wykorzystać go do **wybierania instrukcji**, które program ma wykonać.

### `=` to nie to samo co `==`

- `onlinePlayers = 16` **przypisuje** nową wartość, jeżeli `onlinePlayers` zostało zadeklarowane jako `var`.
- `onlinePlayers == 16` **sprawdza**, czy obecna wartość wynosi `16`, i daje `Boolean`.

```kotlin-run
fun main() {
    var points = 10
    println(points == 10)

    points = 15
    println(points == 10)
    println(points != 10)
}
```

Wynik:

```text
true
false
true
```

### Porównywanie tekstów

```kotlin-run
fun main() {
    val nickname = "Alex"

    println(nickname == "Alex")
    println(nickname == "alex")
    println(nickname != "Steve")
}
```

Wynik:

```text
true
false
true
```

W Kotlinie `==` porównuje **zawartość** napisów `String`. Wielkość liter ma znaczenie: `"Alex"` i `"alex"` to różne napisy.

## 7. Pierwsza rozmowa z programem — `readln()`

Do tej pory program znał dane, ponieważ wpisywaliśmy je w kodzie. Teraz poprosi o nie użytkownika.

```kotlin-run
fun main() {
    print("Podaj nick gracza: ")
    val nickname = readln()

    println("Witaj na serwerze, $nickname!")
}
```

Przykładowy przebieg (wpisujesz `Mila` i naciskasz Enter):

```text
Podaj nick gracza: Mila
Witaj na serwerze, Mila!
```

**Krok po kroku:**

1. `print()` pokazuje pytanie **bez przejścia do nowej linii**.
2. `readln()` czeka, aż użytkownik wpisze tekst i zatwierdzi go Enterem.
3. Wpisany tekst trafia do `nickname`.
4. `println()` wyświetla powitanie z podanym nickiem.

Dopóki użytkownik nie zatwierdzi odpowiedzi, dalsze instrukcje nie zostaną wykonane. Jeżeli nie ma dostępnego standardowego wejścia, `readln()` może zakończyć się błędem — wtedy użyj IntelliJ IDEA.

### `readln()` zawsze zwraca tekst

Nawet jeśli wpiszesz cyfry `25`, wynik `readln()` jest typu `String`, a nie `Int`:

```kotlin-run
fun main() {
    print("Ile masz diamentów? ")
    val diamondsText = readln()

    println("Wpisałaś: $diamondsText")
    println("Typ danych: tekst")
}
```

Przykładowe wejście: `25`.

Nie da się bezpośrednio dodać liczby do napisu za pomocą zwykłego dodawania liczbowego. Najpierw trzeba zamienić wpisany tekst na liczbę.

## 8. Tekst z konsoli jako liczba — `toInt()`

Z lekcji 2 znasz już konwersję. Teraz wykorzystamy ją na **danych wprowadzonych przez człowieka**:

```kotlin-run
fun main() {
    print("Podaj liczbę diamentów: ")
    val diamonds = readln().toInt()

    val afterReward = diamonds + 5
    println("Po nagrodzie masz: $afterReward diamentów")
}
```

Przykładowy przebieg dla wpisanego `12`:

```text
Podaj liczbę diamentów: 12
Po nagrodzie masz: 17 diamentów
```

**Co oznacza `readln().toInt()`?** Najpierw pobieramy tekst (`readln()`), następnie próbujemy zamienić go na liczbę całkowitą (`toInt()`). Dopiero potem możemy wykonać dodawanie.

**Problem:** jeśli ktoś wpisze `dwanaście`, `12.5` lub pustą odpowiedź, `.toInt()` zgłosi błąd (`NumberFormatException`). Nie wolno zakładać, że użytkownik zawsze poda poprawną liczbę.

## 9. Bezpieczne przetwarzanie danych — `toIntOrNull()`

Kotlin ma wygodną metodę `toIntOrNull()`. Jeśli tekst zawiera poprawną liczbę całkowitą mieszczącą się w `Int`, dostaniemy tę liczbę. Jeśli nie — otrzymamy `null`.

`null` oznacza tutaj: **nie udało się uzyskać prawidłowej liczby**. Dokładne zasady typów dopuszczających `null` poznamy w lekcji 8.

```kotlin-run
fun main() {
    print("Podaj liczbę diamentów: ")
    val diamonds = readln().toIntOrNull()

    println("Odczytana wartość: $diamonds")
}
```

Przykład A — wpisz `12`:

```text
Podaj liczbę diamentów: 12
Odczytana wartość: 12
```

Przykład B — wpisz `nie wiem`:

```text
Podaj liczbę diamentów: nie wiem
Odczytana wartość: null
```

Program nadal działa, ale samo wyświetlenie `null` nie jest wygodne dla użytkownika. Zobaczmy, jak poinformować go o błędzie.

### Pierwszy prosty warunek — tylko przedsmak lekcji 4

W tym przykładzie używamy `if`, aby sprawdzić, czy zamiana tekstu na liczbę się udała. Pełne omówienie instrukcji warunkowych nastąpi w **następnej lekcji**.

```kotlin-run
fun main() {
    print("Podaj liczbę diamentów: ")
    val diamonds = readln().toIntOrNull()

    if (diamonds == null) {
        println("Błąd: wpisz poprawną liczbę całkowitą.")
    } else {
        println("Po nagrodzie: ${diamonds + 5} diamentów")
    }
}
```

**Jak to czytać?**

- `if (diamonds == null)` — *jeżeli* nie udało się uzyskać liczby...
- `{ println("Błąd: ...") }` — ...pokaż komunikat o błędzie;
- `else` — *w przeciwnym razie* wykonaj drugą część;
- `${diamonds + 5}` — w poprawnej gałęzi Kotlin wie już, że `diamonds` jest liczbą, a nie `null`.

To ważny krok: **program nie powinien kończyć działania awarią tylko dlatego, że ktoś wpisał literę zamiast cyfry**.

### Liczby ułamkowe — `toDoubleOrNull()`

Do odczytu wartości typu `Double` używamy `toDoubleOrNull()`:

```kotlin-run
fun main() {
    print("Podaj długość skoku (np. 3.5): ")
    val jumpLength = readln().toDoubleOrNull()

    if (jumpLength == null) {
        println("Błąd: podaj liczbę, np. 3.5")
    } else {
        println("Długość skoku: $jumpLength bloku")
    }
}
```

W konsoli Kotlin/JVM standardowy zapis ułamka to **kropka**, np. `3.5`. Wpis `3,5` nie zostanie bezpośrednio sparsowany przez `toDoubleOrNull()`.

> **Ważne:** metoda `toDoubleOrNull()` rozpoznaje także specjalne wartości zmiennoprzecinkowe, np. `NaN` i `Infinity`. Dlatego przy prawdziwych kwotach, punktach czy odległościach w dalszych projektach potrzebna będzie również **walidacja zakresu i skończoności** danych. Na dziś ćwiczymy przede wszystkim bezpieczną konwersję.

## 10. Dodatkowe sprawdzenie: czy liczba ma sens?

Sama poprawna konwersja nie gwarantuje, że wartość jest sensowna. Liczba `-5` może zostać poprawnie odczytana jako `Int`, ale nie powinna oznaczać liczby kupowanych przedmiotów.

```kotlin-run
fun main() {
    print("Ile chcesz kupić sztuk? ")
    val amount = readln().toIntOrNull()

    if (amount == null) {
        println("Wpisz prawidłową liczbę całkowitą.")
    } else if (amount <= 0) {
        println("Liczba sztuk musi być większa od zera.")
    } else {
        println("Wybrano $amount sztuk.")
    }
}
```

Nie trzeba jeszcze uczyć się na pamięć `else if` — jest to kolejna możliwość sprawdzenia warunku. Zasady działania i łączenia warunków szczegółowo wyjaśnimy w lekcji 4.

**Trzy rodzaje niepoprawnego wejścia:**

1. **Nie jest liczbą:** `abc` → nie przejdzie konwersji.
2. **Jest liczbą, ale nie pasuje do zadania:** `-5` → przejdzie konwersję, lecz zostanie odrzucone przez warunek.
3. **Jest zbyt duża dla `Int`:** ogromna liczba → `toIntOrNull()` da `null`, ponieważ wynik nie mieści się w dopuszczalnym zakresie tego typu.

## 11. Mini-projekt — kalkulator sklepu serwerowego

Wyobraź sobie prosty sklep w Minecraft. Każdy diament kosztuje 12 monet. Gracz wpisuje, ile diamentów chce kupić, a program oblicza rachunek.

```kotlin-run
fun main() {
    val pricePerDiamond = 12

    println("===== SKLEP SERWEROWY =====")
    println("Cena diamentu: $pricePerDiamond monet")
    print("Ile diamentów chcesz kupić? ")

    val amount = readln().toIntOrNull()

    if (amount == null) {
        println("Błąd: wpisz liczbę całkowitą.")
    } else if (amount <= 0) {
        println("Błąd: liczba sztuk musi być dodatnia.")
    } else {
        val totalCost = amount * pricePerDiamond
        println("Wybrane diamenty: $amount")
        println("Kwota do zapłaty: $totalCost monet")
        println("Dziękujemy za skorzystanie ze sklepu!")
    }
}
```

Przykładowy przebieg dla wpisanego `4`:

```text
===== SKLEP SERWEROWY =====
Cena diamentu: 12 monet
Ile diamentów chcesz kupić? 4
Wybrane diamenty: 4
Kwota do zapłaty: 48 monet
Dziękujemy za skorzystanie ze sklepu!
```

Przeanalizuj działanie:

1. Cena pojedynczej sztuki jest zapisana tylko raz, w `pricePerDiamond`.
2. Użytkownik wpisuje liczbę sztuk.
3. `toIntOrNull()` próbuje zamienić tekst na liczbę.
4. Dwa sprawdzenia odrzucają błędne dane oraz liczby niedodatnie.
5. Dopiero przy prawidłowej liczbie wykonywane jest mnożenie `amount * pricePerDiamond`.
6. Rachunek powstaje automatycznie — nie wpisujemy go ręcznie.

**Małe doświadczenie:** zmień cenę z `12` na `20`, uruchom program ponownie i kup `3` sztuki. Wynik powinien wynosić **60 monet**. Potem wpisz `zero` oraz `-3` i sprawdź komunikaty błędów.

> **Uproszczenie edukacyjne:** prawdziwy sklep wymagałby jeszcze sprawdzania salda gracza, limitów, ewentualnego przepełnienia liczb i bezpiecznego zapisu transakcji. Dziś interesuje nas obliczenie rachunku, a nie pełny system ekonomii Minecraft.

## 12. Najczęstsze błędy i diagnostyka

| Problem | Dlaczego powstaje? | Jak go rozwiązać? |
|---|---|---|
| `7 / 2` daje `3`, nie `3.5` | Obie liczby są całkowite | Użyj `7 / 2.0` lub konwersji `toDouble()` **przed** dzieleniem. |
| `NumberFormatException` | `toInt()` dostało tekst, którego nie da się zamienić na `Int` | Przy danych użytkownika wybierz `toIntOrNull()` i obsłuż `null`. |
| `Unresolved reference` | Literówka w nazwie zmiennej lub funkcji | Porównaj pisownię z deklaracją, pamiętaj o wielkości liter. |
| Program czeka i nic nie wyświetla | Działa `readln()` i oczekuje wejścia | Wpisz tekst w konsoli **Run** i naciśnij Enter. |
| Interpreter w przeglądarce przerywa program z `readln()` | Edytor może nie wspierać wejścia konsolowego | Uruchom przykład lokalnie w IntelliJ IDEA. |
| `Val cannot be reassigned` | Próba ponownego przypisania wartości `val` | Użyj `var`, jeśli wartość ma się zmieniać. |
| `==` daje `false` dla dwóch nicków | Różnią się znaki lub wielkość liter | Sprawdź dokładną zawartość obu napisów. |
| Błąd dzielenia przez zero | Dzielnik typu całkowitego wynosi `0` | Zweryfikuj dzielnik **przed** działaniem. |
| Liczba `0` przechodzi konwersję, ale nie nadaje się jako liczba sztuk | Poprawny typ nie oznacza poprawnej wartości | Dodaj kontrolę zakresu, np. `amount <= 0`. |
| Wynik rachunku jest nieoczekiwany | Zła kolejność działań albo błędnie użyte nawiasy | Rozpisz działanie ręcznie i sprawdź operatory. |

### Jak samodzielnie znaleźć błąd?

1. Czy program uruchamia się w ogóle, czy zatrzymuje go **błąd kompilacji**?
2. Jeżeli uruchamia się, czy nie czeka teraz na `readln()`?
3. Jakie dane zostały rzeczywiście wpisane? Czy są liczbą, tekstem, liczbą ujemną?
4. Jeśli rachunek jest zły, policz go na kartce i sprawdź nawiasy oraz dzielenie całkowite.
5. Zmień **jedną rzecz naraz** i przetestuj ponownie.

## 13. Zadania do samodzielnego wykonania

**Zasada:** na razie nie zaglądaj do rozwiązań na końcu dokumentu. Każde zadanie opracuj jako **pełny program Kotlin** z własnym `fun main()`. Przykładowy kod do analizy w zadaniach ma znacznik `kotlin`, aby nie ujawniać gotowego rozwiązania przez uruchomienie go w edytorze.

### Zadanie 1 — Cztery działania (łatwe)

Utwórz `val a = 18` i `val b = 6`. Wyświetl sumę, różnicę, iloczyn i iloraz w czytelnych komunikatach. **Wynik każdej operacji ma być obliczany z `a` i `b`**, a nie wpisany ręcznie do `println()`.

### Zadanie 2 — Pełne stosy i reszta (łatwe)

Masz `53` sztuki przedmiotów i pojemność jednego stosu `16`. Oblicz pełne stosy oraz liczbę pozostałych przedmiotów za pomocą `/` i `%`. Zmień dane na `64` sztuki i porównaj wyniki.

### Zadanie 3 — Przewidź wyniki, zanim uruchomisz (łatwe)

Zapisz wynik **każdej** z poniższych sześciu instrukcji na kartce. Dopiero po zakończeniu sprawdź je w IntelliJ:

```kotlin
fun main() {
    println(15 / 4)
    println(15 % 4)
    println(15 / 4.0)
    println(2 + 3 * 5)
    println((2 + 3) * 5)
    println(8 == 8)
}
```

### Zadanie 4 — Punkty gracza (średnie)

Utwórz `var points = 20`. Następnie wykonaj **kolejno**: dodaj `7`, odejmij `4`, pomnóż wynik przez `2`. Użyj operatorów skróconych (`+=`, `-=`, `*=`). Wyświetl wartość początkową i końcową.

### Zadanie 5 — Sprawdź limit (średnie)

Zadeklaruj `onlinePlayers = 19` i `maxPlayers = 20`. Wyświetl wyniki trzech porównań:

- czy jest jeszcze wolne miejsce;
- czy obecna liczba graczy jest **równa** limitowi;
- czy online jest **co najmniej 10** osób.

Wyniki powinny mieć typ `Boolean` i zostać obliczone przez operatory porównania, a nie zapisane ręcznie jako `true` lub `false`.

### Zadanie 6 — Powitanie użytkownika (średnie)

Poproś o nick przez `readln()` oraz liczbę wygranych meczów. Użyj `toIntOrNull()`. Jeśli druga wartość jest niepoprawna, pokaż komunikat. Jeśli jest poprawna i nieujemna, wypisz nick i liczbę wygranych.

> Przykłady z `readln()` wykonuj w IntelliJ, jeżeli edytor internetowy nie umożliwia wpisania odpowiedzi.

### Zadanie 7 — Napraw uszkodzony program (średnie)

Kod poniżej **celowo ma błędy**. Popraw go tak, aby poprawnie odczytywał liczbę graczy, obliczał liczbę po dołączeniu kolejnych dwóch i wyświetlał wynik bez ryzyka awarii przy wpisaniu liter:

```kotlin
fun main() {
    print("Gracze online: ")
    val players = readln()
    val updated = players + 2
    println("Po dołączeniu: $updated")
}
```

### Zadanie 8 — Projekt zaliczeniowy: zakup przedmiotów (trudniejsze)

Napisz **interaktywny kalkulator zakupu chlebów** w sklepie Minecraft. Program ma:

1. Ustawić stałą cenę chleba na `7` monet oraz budżet gracza na `50` monet.
2. Zapytać przez `readln()`, ile chlebów chce kupić użytkownik.
3. Użyć `toIntOrNull()` i wyświetlić sensowną informację, jeśli wpisano błędny tekst.
4. Odrzucić `0` i liczby ujemne.
5. Obliczyć cenę wszystkich chlebów przez mnożenie.
6. Sprawdzić porównaniem, czy budżet **wystarcza**.
7. Jeśli starcza pieniędzy, wyświetlić koszt zakupu i pozostałe monety. Jeśli nie, pokazać brakującą liczbę monet.

**Wskazówka:** użyj `if` i `else` w uproszczonej formie znanej z mini-projektu. Pełne wyjaśnienie warunków będzie w lekcji 4. Nie musisz stosować `&&` ani `||`.

## 14. Mini-test — sprawdź, co rozumiesz

Zapisz odpowiedzi przed zajrzeniem do końca dokumentu.

1. Jaka jest różnica między `/` i `%` dla dwóch liczb całkowitych?
2. Co zwróci `9 / 2`, a co `9 / 2.0`?
3. Jaki jest wynik `3 + 2 * 5`?
4. Jak zmienić powyższe wyrażenie nawiasami, aby dało `25`?
5. Co robi instrukcja `points += 3`, jeśli `points` ma wartość `10`?
6. Czym różni się `=` od `==`?
7. Jakiego typu wynik zwraca `15 >= 20` i jaka jest jego wartość?
8. Jaki typ danych zwraca `readln()`?
9. Dlaczego `readln().toInt()` może spowodować błąd, a `toIntOrNull()` pozwala go uniknąć?
10. Czy wpisana liczba `-3` zawsze powinna zostać uznana za poprawną tylko dlatego, że `toIntOrNull()` ją rozpoznaje?

### Kiedy przejść do lekcji 4?

Możesz przejść dalej, jeśli:

- rozumiesz, dlaczego `7 / 2` daje `3`, a `7 / 2.0` daje `3.5`;
- samodzielnie piszesz działania i porównania na zmiennych;
- potrafisz pobrać nick przez `readln()`;
- potrafisz pobrać liczbę przez `toIntOrNull()` i wyświetlić komunikat przy błędnym wpisie;
- ukończysz projekt z zadania 8 bez bezmyślnego przepisywania całej odpowiedzi.

Jeżeli któryś punkt nadal sprawia trudność, wróć do odpowiedniego przykładu i zmień w nim dane. Nie musisz uczyć się wszystkich operatorów na pamięć — ważne, żebyś potrafiła z nich korzystać.

## 15. Podsumowanie

Dziś poznałaś narzędzia potrzebne do obliczania nowych wartości, porównywania danych i prowadzenia prostej rozmowy z użytkownikiem. Zauważyłaś również, że **poprawna składnia nie gwarantuje poprawności danych wejściowych**. To rozróżnienie przyda się później przy obsłudze komend graczy na serwerze Minecraft.

W **lekcji 4** dokładnie omówimy instrukcje `if`, `else`, `when` i operatory logiczne `&&`, `||`, `!`, aby tworzyć programy reagujące na różne sytuacje.

---

## 16. Odpowiedzi i przykładowe rozwiązania

> **Nie zaglądaj tutaj podczas pierwszej próby.** Rozwiązania nie są jedynymi możliwymi. Liczy się spełnienie warunków zadania i zrozumienie wyniku. Każdy poniższy pełny program możesz uruchomić niezależnie.

### Zadanie 1 — cztery działania

```kotlin-run
fun main() {
    val a = 18
    val b = 6

    println("Suma: ${a + b}")
    println("Różnica: ${a - b}")
    println("Iloczyn: ${a * b}")
    println("Iloraz: ${a / b}")
}
```

Wynik:

```text
Suma: 24
Różnica: 12
Iloczyn: 108
Iloraz: 3
```

### Zadanie 2 — pełne stosy

```kotlin-run
fun main() {
    val items = 53
    val stackSize = 16

    println("Pełne stosy: ${items / stackSize}")
    println("Pozostałe przedmioty: ${items % stackSize}")
}
```

Wynik: **3 pełne stosy i 5 przedmiotów**. Dla `64` sztuk wynik wynosi **4 pełne stosy i 0 pozostałych przedmiotów**.

### Zadanie 3 — przewidywanie wyniku

W kolejności: `3`, `3`, `3.75`, `17`, `25`, `true`.

- `15 / 4` to dzielenie całkowite.
- `15 % 4` to reszta (`3`).
- `15 / 4.0` daje wynik `Double`.
- `3 * 5 = 15`, więc `2 + 15 = 17`.
- `(2 + 3) * 5 = 25`.
- `8 == 8` jest prawdą.

### Zadanie 4 — punkty gracza

```kotlin-run
fun main() {
    var points = 20
    println("Przed zmianą: $points")

    points += 7
    points -= 4
    points *= 2

    println("Po zmianach: $points")
}
```

Wynik końcowy: `(20 + 7 - 4) * 2 = 46`.

### Zadanie 5 — sprawdzenie limitu

```kotlin-run
fun main() {
    val onlinePlayers = 19
    val maxPlayers = 20

    println("Wolne miejsce: ${onlinePlayers < maxPlayers}")
    println("Osiągnięty limit: ${onlinePlayers == maxPlayers}")
    println("Co najmniej 10 graczy: ${onlinePlayers >= 10}")
}
```

Wyniki to kolejno: `true`, `false`, `true`.

### Zadanie 6 — nick i liczba wygranych

```kotlin-run
fun main() {
    print("Podaj nick: ")
    val nickname = readln()

    print("Podaj liczbę wygranych meczów: ")
    val wins = readln().toIntOrNull()

    if (wins == null) {
        println("Błąd: wpisz liczbę całkowitą.")
    } else if (wins < 0) {
        println("Błąd: liczba wygranych nie może być ujemna.")
    } else {
        println("Gracz: $nickname; liczba wygranych meczów: $wins.")
    }
}
```

Przykładowy wynik dla nicku `Mila` i liczby `3`: `Gracz: Mila; liczba wygranych meczów: 3.`

### Zadanie 7 — poprawiony program

```kotlin-run
fun main() {
    print("Gracze online: ")
    val players = readln().toIntOrNull()

    if (players == null) {
        println("Błąd: podaj poprawną liczbę graczy.")
    } else if (players < 0) {
        println("Błąd: liczba graczy nie może być ujemna.")
    } else {
        val updated = players + 2
        println("Po dołączeniu: $updated")
    }
}
```

**Dlaczego oryginał nie działał poprawnie?** `readln()` zwraca `String`, a `players + 2` w takiej postaci nie wykonuje **dodawania liczbowego**. W poprawionym kodzie najpierw próbujemy odczytać `Int`, a potem sprawdzamy wynik konwersji. Wprowadzono też kontrolę liczb ujemnych.

### Zadanie 8 — mini-projekt: zakup chlebów

```kotlin-run
fun main() {
    val pricePerBread = 7
    val balance = 50

    println("===== SKLEP: CHLEB =====")
    println("Cena sztuki: $pricePerBread monet")
    println("Twój budżet: $balance monet")
    print("Ile chlebów chcesz kupić? ")

    val amount = readln().toIntOrNull()

    if (amount == null) {
        println("Błąd: wpisz liczbę całkowitą.")
    } else if (amount <= 0) {
        println("Błąd: liczba sztuk musi być większa od zera.")
    } else {
        val totalCost = amount.toLong() * pricePerBread

        if (totalCost <= balance) {
            val moneyLeft = balance - totalCost
            println("Kupujesz $amount chlebów za $totalCost monet.")
            println("Pozostało: $moneyLeft monet.")
        } else {
            val missingMoney = totalCost - balance
            println("Masz za mało monet.")
            println("Brakuje Ci $missingMoney monet.")
        }
    }
}
```

**Dlaczego `amount.toLong()`?** Mnożenie dwóch `Int` może przekroczyć ich dopuszczalny zakres, zanim wynik zostanie zapisany. Konwersja pierwszego argumentu na `Long` powoduje, że wynik tego mnożenia ma szerszy zakres. To dodatkowe zabezpieczenie przykładu, nie zagadnienie wymagane do zapamiętania na tym etapie.

Przykładowe wyniki:

- `5` sztuk → koszt `35`, pozostaje `15` monet;
- `8` sztuk → koszt `56`, brakuje `6` monet;
- `0` → komunikat o liczbie sztuk;
- `abc` → komunikat o błędnej liczbie.

### Mini-test — odpowiedzi

1. `/` wylicza iloraz (dla dwóch `Int` bez części ułamkowej), `%` resztę z dzielenia.
2. `9 / 2` daje `4`, a `9 / 2.0` daje `4.5`.
3. `13`.
4. `(3 + 2) * 5` daje `25`.
5. `points` zmieni się z `10` na `13`.
6. `=` przypisuje wartość, `==` porównuje i zwraca `Boolean`.
7. Typ `Boolean`, wartość `false`.
8. `String`.
9. `toInt()` może zgłosić `NumberFormatException`; `toIntOrNull()` zwraca `null` przy niepoprawnym tekście, który można sprawdzić.
10. Nie. Należy również zweryfikować, czy liczba ma sens dla danego zastosowania, np. liczba sztuk w sklepie musi być dodatnia.

---

## Dodatkowe źródła (dla chętnych)

- [Kotlin — podstawowa składnia](https://kotlinlang.org/docs/basic-syntax.html)
- [Kotlin — liczby i operatory](https://kotlinlang.org/docs/numbers.html)
- [Kotlin — działania i przeciążanie operatorów](https://kotlinlang.org/docs/operator-overloading.html)
- [Kotlin — sprawdzanie danych przez `toIntOrNull()`](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.text/to-int-or-null.html)
- [Kotlin — `readln()`](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.io/readln.html)

**Koniec lekcji 3/36. Następna: warunki `if`, `else`, `when` i operatory logiczne.**