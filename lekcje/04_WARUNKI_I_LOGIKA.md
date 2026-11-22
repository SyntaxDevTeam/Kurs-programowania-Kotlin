# Lekcja 4 — Warunki i logika: `if`, `else`, `when`, `&&`, `||`, `!`

**Poziom:** I — Fundamenty Kotlina  
**Lekcja:** 4 z 8 w Poziomie I (4 z 36 w całym kursie)  
**Temat:** podejmowanie decyzji przez program, warunki, wiele możliwości i łączenie warunków  
**Czas:** około 90–120 minut (łącznie z ćwiczeniami i mini-projektem)  
**Potrzebujesz:** IntelliJ IDEA, działający projekt Kotlin/JVM, JDK i odrobina cierpliwości  
**Wymagana wiedza:** lekcje 1–3: `fun main()`, `println()`, `val`, `var`, typ `Boolean`, porównania (`>`, `<`, `==`), `readln()` i `toIntOrNull()`

---

> **Jak czytać tę lekcję?** Bloki `kotlin-run` są **kompletnymi programami** — możesz je uruchomić w edytorze kursu albo skopiować do `Main.kt`. Bloki `kotlin` zawierają urywki kodu do analizy, przewidywania działania lub naprawy. Rozwiązania zadań znajdują się **dopiero na końcu lekcji**.
>
> **Uwaga o `readln()`:** interaktywne programy wymagają wprowadzania tekstu. Jeśli interpreter na stronie tego nie obsługuje, uruchom te przykłady w IntelliJ IDEA i wpisuj odpowiedzi w dolnym panelu **Run**.
>
> Dziś uczymy się **podstaw Kotlina**. Pętle będą w lekcji 5, funkcje w lekcji 6, a pluginy Spigot dopiero od lekcji 16. Nie musisz znać żadnego API serwera Minecraft.

## 1. Dlaczego program potrzebuje warunków?

Dotychczas nasze programy wykonywały polecenia po kolei. Niezależnie od danych wyświetlały te same instrukcje, o ile sami nie zmieniliśmy kodu.

W prawdziwym programie często trzeba odpowiedzieć na pytanie: **„co zrobić, jeżeli coś się wydarzy?”**. Na przykład:

- Jeżeli użytkownik ma co najmniej 18 lat — pokaż informację o możliwości zapisu.
- Jeżeli temperatura spadła poniżej zera — ostrzeż przed mrozem.
- Jeżeli hasło jest nieprawidłowe — odmów dostępu.

W Kotlinie takie decyzje podejmuje się między innymi za pomocą `if` (czytamy: *if*, czyli „jeżeli”).

**Po zakończeniu lekcji potrafisz:**

1. napisać poprawny warunek z `if`;
2. dodać drugi wariant przy użyciu `else`;
3. sprawdzić kilka możliwości za pomocą `else if` i `when`;
4. połączyć pytania operatorami `&&` („i”), `||` („lub”) i `!` („nie”);
5. wyjaśnić, dlaczego kolejność warunków jest ważna;
6. samodzielnie zbudować prosty program decyzyjny z kontrolą błędnych danych.

## 2. Zanim zaczniemy: odpowiedzi `true` i `false`

Pamiętasz typ `Boolean` z lekcji 2? Może mieć tylko dwie wartości:

- `true` — prawda, warunek jest spełniony;
- `false` — fałsz, warunek nie jest spełniony.

Z lekcji 3 znasz porównania. Zobacz, jaki mają wynik:

```kotlin-run
fun main() {
    val age = 17

    println(age >= 18)
    println(age < 18)
    println(age == 17)
}
```

Wynik:

```text
false
true
true
```

**Dlaczego?** Liczba `17` nie jest większa lub równa `18`, ale jest mniejsza od `18` i jest równa `17`.

Ważna uwaga: `=` służy do **przypisania** wartości, natomiast `==` służy do **porównania**. Nie są zamienne.

## 3. Pierwsza instrukcja `if` — „jeżeli...”

Zacznijmy od bardzo prostego przykładu:

```kotlin-run
fun main() {
    val temperature = -3

    if (temperature < 0) {
        println("Uwaga! Na dworze jest mróz.")
    }

    println("Koniec sprawdzania pogody.")
}
```

Wynik:

```text
Uwaga! Na dworze jest mróz.
Koniec sprawdzania pogody.
```

**Prześledź kod linia po linii:**

1. `val temperature = -3` — zapisujemy temperaturę.
2. `if` rozpoczyna sprawdzanie warunku.
3. `(temperature < 0)` pyta, czy temperatura jest mniejsza od zera.
4. Ponieważ odpowiedź brzmi `true`, wykonuje się kod pomiędzy `{` i `}`.
5. `println("Koniec sprawdzania pogody.")` jest **poza** blokiem `if`, więc wykona się niezależnie od wyniku sprawdzenia.

Zmień temperaturę na `5` i uruchom program ponownie. Zobaczysz tylko:

```text
Koniec sprawdzania pogody.
```

**Zapamiętaj schemat:**

```kotlin
if (warunek) {
    // Instrukcje wykonywane tylko wtedy, gdy warunek jest prawdziwy.
}
```

Klamry `{ }` pomagają wyznaczyć, które instrukcje należą do warunku. Stosuj je konsekwentnie — kod będzie czytelniejszy.

### Warunek może być gotową wartością `Boolean`

Nie musisz za każdym razem pisać porównania. Czasami masz już gotową odpowiedź `true` lub `false`:

```kotlin-run
fun main() {
    val notificationsEnabled = true

    if (notificationsEnabled) {
        println("Powiadomienia są włączone.")
    }
}
```

`if (notificationsEnabled)` jest prawidłowy. Nie trzeba pisać `notificationsEnabled == true`.

## 4. `else` — „w przeciwnym razie...”

Co zrobić, jeśli użytkownik **nie** spełni warunku? Do tego służy `else`.

```kotlin-run
fun main() {
    val age = 16

    if (age >= 18) {
        println("Możesz zapisać się samodzielnie.")
    } else {
        println("Potrzebujesz zgody opiekuna.")
    }
}
```

Wynik:

```text
Potrzebujesz zgody opiekuna.
```

Program ma teraz **dwie drogi**:

- gdy `age >= 18` daje `true`, wykonuje pierwszy blok;
- gdy warunek daje `false`, wykonuje blok po `else`.

W jednym wykonaniu tego `if` nie zobaczysz obu komunikatów naraz.

**Mały eksperyment:** ustaw `age = 19`, później `age = 18`, a na końcu `age = 17`. Sprawdź, czy potrafisz przewidzieć wszystkie trzy wyniki.

### Ważna różnica: dwa `if` a `if` z `else`

Spójrz na program:

```kotlin-run
fun main() {
    val points = 10

    if (points >= 10) {
        println("Masz co najmniej 10 punktów.")
    }

    if (points >= 5) {
        println("Masz co najmniej 5 punktów.")
    }
}
```

Wynik:

```text
Masz co najmniej 10 punktów.
Masz co najmniej 5 punktów.
```

**Dlaczego są dwa komunikaty?** To **dwa osobne sprawdzenia**. Każdy `if` działa niezależnie. W dalszej części zobaczysz, jak wybrać **tylko jeden** z kilku wariantów.

## 5. `else if` — więcej niż dwie możliwości

Załóżmy, że chcemy wystawić prostą ocenę na podstawie wyniku testu:

```kotlin-run
fun main() {
    val score = 73

    if (score >= 90) {
        println("Wynik: bardzo dobry")
    } else if (score >= 70) {
        println("Wynik: dobry")
    } else if (score >= 50) {
        println("Wynik: wystarczający")
    } else {
        println("Wynik: do poprawy")
    }
}
```

Wynik:

```text
Wynik: dobry
```

Program sprawdza warunki **od góry**:

1. Czy `73 >= 90`? Nie.
2. Czy `73 >= 70`? Tak. Wypisuje „Wynik: dobry”.
3. **Kończy tę grupę warunków**. Nie sprawdza już niższych `else if`.

To ważne: w takim łańcuchu `if` / `else if` / `else` wykonuje się **co najwyżej jedna gałąź**.

### Kolejność ma znaczenie

Poniższy program jest poprawny składniowo, ale ma **błąd w logice**:

```kotlin
fun main() {
    val score = 95

    if (score >= 50) {
        println("Wynik wystarczający")
    } else if (score >= 90) {
        println("Wynik bardzo dobry")
    }
}
```

Pierwszy warunek `score >= 50` jest już prawdziwy. Program nigdy nie dojdzie do sprawdzenia `score >= 90` dla wyniku `95`.

**Dobra zasada:** przy sprawdzaniu progów od najwyższego wyniku zaczynaj od **najwyższego progu**. W innych problemach kolejność może być inna — zawsze ustal, który wariant powinien mieć pierwszeństwo.

## 6. `if` może również zwrócić wartość

Znasz już `val`. Zamiast wypisywać komunikat osobno w obu gałęziach, możesz pozwolić `if` **wybrać tekst**, który zapiszesz do `val`.

```kotlin-run
fun main() {
    val points = 12

    val message = if (points >= 10) {
        "Zaliczone"
    } else {
        "Jeszcze trochę brakuje"
    }

    println(message)
}
```

Wynik:

```text
Zaliczone
```

W tym przykładzie `if` wybiera **jedną wartość**. Ostatni zapis w wybranym bloku (`"Zaliczone"` albo `"Jeszcze trochę brakuje"`) jest wynikiem całego wyrażenia.

**Uwaga:** gdy przypisujesz wynik `if` do `val`, potrzebujesz też gałęzi `else`, żeby program miał wartość do przypisania w każdej sytuacji.

Możliwy jest nawet krótszy zapis:

```kotlin-run
fun main() {
    val isSunny = true
    val message = if (isSunny) "Weź okulary" else "Weź parasol"
    println(message)
}
```

Wynik:

```text
Weź okulary
```

Na początek możesz używać bardziej rozbudowanej formy z klamrami, jeżeli łatwiej Ci ją czytać.

## 7. Operatory logiczne — kilka pytań naraz

Czasem jedno pytanie nie wystarcza. Na przykład dostęp do sali może wymagać spełnienia **dwóch warunków jednocześnie**: posiadania biletu **i** przybycia w odpowiednim czasie.

Do łączenia warunków służą trzy podstawowe operatory:

| Operator | Czytamy | Kiedy wynik jest `true`? |
|---|---|---|
| `&&` | **i** | Gdy oba warunki są prawdziwe. |
| `||` | **lub** | Gdy przynajmniej jeden warunek jest prawdziwy. |
| `!` | **nie** | Gdy odwrócony warunek jest fałszywy. |

### 7.1. `&&` — oba warunki muszą być prawdziwe

```kotlin-run
fun main() {
    val hasTicket = true
    val arrivedOnTime = false

    if (hasTicket && arrivedOnTime) {
        println("Możesz wejść na wydarzenie.")
    } else {
        println("Nie spełniasz wszystkich warunków wejścia.")
    }
}
```

Wynik:

```text
Nie spełniasz wszystkich warunków wejścia.
```

Masz bilet, ale nie dotarłaś na czas, więc **nie oba** warunki są prawdziwe.

| Pierwszy warunek | Drugi warunek | `&&` |
|---|---|---|
| `false` | `false` | `false` |
| `false` | `true` | `false` |
| `true` | `false` | `false` |
| `true` | `true` | `true` |

### 7.2. `||` — wystarczy jeden warunek

Załóżmy, że wystarczy być organizatorem **lub** mieć zaproszenie:

```kotlin-run
fun main() {
    val isOrganizer = false
    val hasInvitation = true

    if (isOrganizer || hasInvitation) {
        println("Wejście dozwolone.")
    } else {
        println("Brak uprawnień do wejścia.")
    }
}
```

Wynik:

```text
Wejście dozwolone.
```

| Pierwszy warunek | Drugi warunek | `||` |
|---|---|---|
| `false` | `false` | `false` |
| `false` | `true` | `true` |
| `true` | `false` | `true` |
| `true` | `true` | `true` |

**Ważne:** `||` w Kotlinie znaczy „przynajmniej jeden”, a więc **dwa prawdziwe warunki również dają `true`**.

### 7.3. `!` — odwrócenie odpowiedzi

```kotlin-run
fun main() {
    val isClosed = false

    if (!isClosed) {
        println("Sklep jest otwarty.")
    } else {
        println("Sklep jest zamknięty.")
    }
}
```

Wynik:

```text
Sklep jest otwarty.
```

`isClosed` ma wartość `false`. Zapis `!isClosed` oznacza **„nie jest zamknięty”** i daje `true`.

### Kilka warunków w jednym programie

```kotlin-run
fun main() {
    val age = 19
    val hasDocument = true
    val isSuspended = false

    if (age >= 18 && hasDocument && !isSuspended) {
        println("Weryfikacja zakończona powodzeniem.")
    } else {
        println("Weryfikacja nie powiodła się.")
    }
}
```

Wynik:

```text
Weryfikacja zakończona powodzeniem.
```

Kolejno:

- `age >= 18` daje `true`;
- `hasDocument` to `true`;
- `!isSuspended` oznacza `!false`, czyli `true`.

Wszystkie trzy pytania są spełnione, więc cały warunek jest prawdziwy.

### Nawiasy w logice ułatwiają czytanie

Jeżeli łączysz `&&` i `||`, możesz użyć nawiasów, aby jasno pokazać, jakie zasady mają obowiązywać:

```kotlin-run
fun main() {
    val isOwner = true
    val isModerator = false
    val hasAccessCard = false

    val canEnter = (isOwner || isModerator) && hasAccessCard
    println(canEnter)
}
```

Wynik:

```text
false
```

Nawet właściciel musi w tym przypadku mieć kartę dostępu. Gdybyśmy zapisali warunek inaczej, na przykład `isOwner || (isModerator && hasAccessCard)`, właściciel mógłby wejść bez karty. **To inna zasada**, a nie tylko inny sposób pisania.

> **Dla ciekawych:** w wyrażeniu `A && B` Kotlin nie sprawdza `B`, jeśli `A` jest już `false`. W `A || B` nie sprawdza `B`, jeśli `A` jest `true`. Dzięki temu nie wykonuje zbędnej pracy. Nie musisz jeszcze wykorzystywać tego mechanizmu; wrócimy do niego przy bardziej zaawansowanym kodzie.

## 8. `when` — czytelny wybór spośród wielu wartości

Jeżeli masz kilka konkretnych możliwości, zamiast długiego łańcucha `else if` często wygodniej użyć `when` (czytamy: *wen*).

Wyobraź sobie wybór dnia tygodnia:

```kotlin-run
fun main() {
    val day = 3

    when (day) {
        1 -> println("Poniedziałek")
        2 -> println("Wtorek")
        3 -> println("Środa")
        4 -> println("Czwartek")
        5 -> println("Piątek")
        6 -> println("Sobota")
        7 -> println("Niedziela")
        else -> println("Nie ma takiego dnia tygodnia.")
    }
}
```

Wynik:

```text
Środa
```

**Jak to czytać?**

- `when (day)` — sprawdź wartość zapisaną pod nazwą `day`;
- `1 ->` — gdy wartością jest `1`, wykonaj kod po strzałce;
- `3 ->` — gdy jest `3`, wypisz „Środa”;
- `else ->` — gdy nie pasuje żadna wcześniejsza możliwość.

To jest **jedno sprawdzenie wielu wariantów**, a nie siedem niezależnych `if`.

### `when` działa także z tekstem

```kotlin-run
fun main() {
    val command = "start"

    when (command) {
        "start" -> println("Rozpoczynam działanie.")
        "stop" -> println("Kończę działanie.")
        "help" -> println("Dostępne polecenia: start, stop, help")
        else -> println("Nieznane polecenie.")
    }
}
```

Wynik:

```text
Rozpoczynam działanie.
```

Uwaga: `"start"` i `"Start"` to **dwa różne teksty**. Jeśli program powinien akceptować różną wielkość liter, możesz przekształcić je metodą `lowercase()`:

```kotlin-run
fun main() {
    val command = "StArT".lowercase()

    when (command) {
        "start" -> println("Uruchomiono.")
        "stop" -> println("Zatrzymano.")
        else -> println("Nieznane polecenie.")
    }
}
```

Wynik:

```text
Uruchomiono.
```

### Kilka wartości może prowadzić do tej samej odpowiedzi

```kotlin-run
fun main() {
    val fruit = "gruszka"

    when (fruit) {
        "jabłko", "gruszka" -> println("Owoc z sadu")
        "banan", "ananas" -> println("Owoc tropikalny")
        else -> println("Inny owoc")
    }
}
```

Wynik:

```text
Owoc z sadu
```

Przecinek pozwala połączyć kilka konkretnych wartości w jednej gałęzi.

## 9. `when` bez wartości w nawiasie — warunki zamiast konkretnych odpowiedzi

W lekcji 5 nauczymy się pętli. Teraz ważniejsze jest inne zastosowanie `when`: sprawdzanie kilku **różnych warunków** po kolei.

```kotlin-run
fun main() {
    val temperature = 22

    when {
        temperature < 0 -> println("Mróz")
        temperature < 15 -> println("Chłodno")
        temperature < 25 -> println("Przyjemnie")
        else -> println("Gorąco")
    }
}
```

Wynik:

```text
Przyjemnie
```

**Dlaczego?**

1. `22 < 0` jest fałszywe.
2. `22 < 15` jest fałszywe.
3. `22 < 25` jest prawdziwe, więc program wypisuje „Przyjemnie”.
4. Nie przechodzi już do gałęzi `else`.

**Różnica do zapamiętania:**

- `when (value)` — wygodny, gdy porównujesz **jedną wartość** z kilkoma możliwościami;
- `when { ... }` — przydatny, gdy sprawdzasz **kilka różnych pytań logicznych**.

### `when` też może wybrać wartość do `val`

```kotlin-run
fun main() {
    val score = 84

    val grade = when {
        score >= 90 -> "bardzo dobry"
        score >= 70 -> "dobry"
        score >= 50 -> "wystarczający"
        else -> "do poprawy"
    }

    println("Twój wynik: $grade")
}
```

Wynik:

```text
Twój wynik: dobry
```

Tutaj `when` **nie wypisuje** niczego bezpośrednio — wybiera tekst. Dopiero `println()` pokazuje wynik.

## 10. Warunki i tekst wpisywany przez użytkownika

Teraz połączymy lekcję 3 z dzisiejszym materiałem. Program poprosi o podanie wieku i nie wyświetli złej informacji, jeśli ktoś wpisze przypadkowe litery.

```kotlin-run
fun main() {
    print("Podaj swój wiek: ")
    val age = readln().toIntOrNull()

    if (age == null) {
        println("Błąd: wpisz liczbę całkowitą.")
    } else if (age < 0) {
        println("Wiek nie może być ujemny.")
    } else if (age >= 18) {
        println("Jesteś osobą pełnoletnią.")
    } else {
        println("Jesteś osobą niepełnoletnią.")
    }
}
```

Przykładowe wyniki:

| Wpisana wartość | Komunikat |
|---|---|
| `19` | `Jesteś osobą pełnoletnią.` |
| `15` | `Jesteś osobą niepełnoletnią.` |
| `-2` | `Wiek nie może być ujemny.` |
| `abc` | `Błąd: wpisz liczbę całkowitą.` |

`toIntOrNull()` zamienia poprawny tekst na liczbę albo daje `null`, jeśli nie udało się go przekonwertować. Dlatego **najpierw** sprawdzamy, czy wynik jest `null`. Dzięki temu Kotlin pozwala nam potem bezpiecznie porównywać `age` z liczbami.

Nie musisz rozumieć teraz wszystkich możliwości typu z `null`. Przyjrzymy się temu dokładniej w lekcji 8.

## 11. Mini-projekt — prosty system rezerwacji miejsc

W mini-projekcie wykorzystamy warunki do rozwiązania zwykłego, praktycznego zadania. **Nie tworzymy jeszcze pluginu** — to program uruchamiany w konsoli.

Założenia:

- Organizator ma `5` wolnych miejsc.
- Użytkownik podaje, ile miejsc chce zarezerwować.
- Liczba musi być poprawna i większa od zera.
- Jeżeli miejsc nie wystarczy, program wyświetli wyjaśnienie.
- Jeżeli rezerwacja jest możliwa, program poda liczbę pozostałych miejsc.

```kotlin-run
fun main() {
    val availableSeats = 5

    println("=== REZERWACJA MIEJSC ===")
    println("Wolne miejsca: $availableSeats")
    print("Ile miejsc chcesz zarezerwować? ")

    val requestedSeats = readln().toIntOrNull()

    if (requestedSeats == null) {
        println("Błąd: wpisz liczbę całkowitą.")
    } else if (requestedSeats <= 0) {
        println("Liczba miejsc musi być większa od zera.")
    } else if (requestedSeats > availableSeats) {
        println("Brakuje wolnych miejsc.")
    } else {
        val remaining = availableSeats - requestedSeats
        println("Rezerwacja przyjęta!")
        println("Pozostałe wolne miejsca: $remaining")
    }
}
```

**Jak go przetestować?** Uruchom go kilka razy i wpisz po kolei `2`, `5`, `6`, `0`, `-1` oraz `abc`.

| Wpisana wartość | Co powinno się wydarzyć? |
|---|---|
| `2` | Rezerwacja przyjęta, zostają `3` miejsca. |
| `5` | Rezerwacja przyjęta, zostaje `0` miejsc. |
| `6` | Brak wystarczającej liczby miejsc. |
| `0` | Komunikat o nieprawidłowej liczbie. |
| `-1` | Komunikat o nieprawidłowej liczbie. |
| `abc` | Komunikat o nieprawidłowym formacie liczby. |

**Zastanów się:** dlaczego w tym przykładzie używamy `else if`, a nie czterech niezależnych `if`? Dlatego, że chcemy wykonać **jedną odpowiednią drogę**, a nie kilka niepasujących do siebie komunikatów.

### Krótki dodatek: status rezerwacji przez `when`

Ten program nie pyta użytkownika o dane — ma pokazać inną wygodną konstrukcję:

```kotlin-run
fun main() {
    val status = "pending"

    val message = when (status) {
        "pending" -> "Rezerwacja oczekuje"
        "confirmed" -> "Rezerwacja potwierdzona"
        "cancelled" -> "Rezerwacja anulowana"
        else -> "Nieznany status"
    }

    println(message)
}
```

Wynik:

```text
Rezerwacja oczekuje
```

## 12. Typowe błędy początkujących

| Problem | Co się stało? | Jak poprawić? |
|---|---|---|
| W warunku zapisano `age = 18` | `=` oznacza przypisanie, nie porównanie | Zapisz `age == 18`. |
| Kotlin pokazuje `Type mismatch` przy `if (age)` | `if` wymaga wartości `Boolean`, a `age` jest liczbą | Zapisz np. `if (age >= 18)`. |
| Program wypisuje dwa sprzeczne komunikaty | Użyto dwóch niezależnych `if` | Jeśli wybór ma być jeden, użyj `else` albo `else if`. |
| Gałąź `else if` nigdy się nie wykonuje | Wcześniejszy warunek obejmuje już jej przypadki | Uporządkuj progi i kolejność sprawdzania. |
| Warunek używa `&` albo `|` | To nie jest zapis operatorów `&&` i `||` omawianych w tej lekcji | Użyj `&&` lub `||` dla zwykłych warunków logicznych. |
| Użyto `!` przed liczbą | `!` odwraca wartość logiczną, nie liczbę | Zastosuj je przed wartością typu `Boolean`. |
| Program pokazuje „Nieznane polecenie” dla `Start` | Tekst `Start` nie jest identyczny z `start` | Rozważ `.lowercase()` przed `when`. |
| Błąd przy przetwarzaniu wejścia | Tekst użytkownika nie jest poprawną liczbą | Użyj `toIntOrNull()` i sprawdź `null`. |

**Sprawdzony sposób diagnozowania:**

1. Wymyśl **jeden przykład danych**, np. `age = 17`.
2. Przejdź przez program od góry, pytając przy każdym warunku: „prawda czy fałsz?”.
3. Zapisz, który blok powinien się wykonać.
4. Dopiero potem uruchom program i porównaj wynik z przewidywaniem.

Takie ćwiczenie uczy rozumieć kod, a nie tylko go przepisywać.

## 13. Ćwiczenia do samodzielnego wykonania

**Zasada:** zadania wykonaj bez zaglądania do rozwiązań. W każdym napisz własne `fun main()` w `Main.kt`. Nie musisz wykorzystywać mechanizmów, których jeszcze nie poznaliśmy, takich jak pętle i kolekcje.

### Zadanie 1 — Liczba dodatnia czy ujemna? (łatwe)

Zapisz liczbę w `val number`. Jeżeli jest większa od zera, wypisz `Liczba dodatnia`. W przeciwnym razie, jeżeli jest mniejsza od zera, wypisz `Liczba ujemna`. W przypadku zera wypisz `To jest zero`.

Przetestuj liczby `8`, `-4` i `0`.

### Zadanie 2 — Parzysta czy nieparzysta? (łatwe)

Użyj poznanego wcześniej operatora `%`, aby sprawdzić, czy liczba `7` jest parzysta. Wypisz jeden z komunikatów: `Liczba parzysta` albo `Liczba nieparzysta`. Następnie zmień liczbę na `10`.

*Wskazówka:* liczba parzysta daje resztę `0` przy dzieleniu przez `2`.

### Zadanie 3 — Co wypisze program? (łatwe)

**Nie uruchamiaj kodu przed zapisaniem odpowiedzi.**

```kotlin
fun main() {
    val points = 80

    if (points >= 50) {
        println("A")
    }

    if (points >= 80) {
        println("B")
    }
}
```

Czy wyświetli się tylko `A`, tylko `B`, czy oba komunikaty? Wyjaśnij dlaczego.

### Zadanie 4 — Dostęp do wydarzenia (średnie)

Utwórz wartości `hasTicket` i `isOnList` typu `Boolean`. Uczestnik może wejść, jeżeli **ma bilet lub znajduje się na liście**. Użyj `||`, aby wypisać `Wejście dozwolone` albo `Wejście zabronione`.

Przetestuj wszystkie cztery kombinacje `true` i `false`.

### Zadanie 5 — Wybór dnia tygodnia (średnie)

Użyj `when`, aby dla `day = 1` wypisać `Poniedziałek`, dla `2` — `Wtorek`, dla `3` — `Środa`, a dla każdej innej liczby — `Inny dzień`.

**Nie trzeba** w tym zadaniu obsługiwać wszystkich siedmiu dni.

### Zadanie 6 — Napraw program (średnie)

Poniższy kod ma **celowe błędy**. Popraw go tak, aby dla `age = 20` wypisał `Pełnoletnia osoba`, a dla `age = 15` — `Osoba niepełnoletnia`.

```kotlin
fun main() {
    val age = 20

    if (age = 18) {
        println("Pełnoletnia osoba")
    } else {
        println("Osoba niepełnoletnia")
    }
}
```

*Wskazówka:* zwróć uwagę zarówno na **znak operatora**, jak i na to, jaki warunek w ogóle chcemy sprawdzać.

### Zadanie 7 — Dwie rzeczy naraz (średnie)

Zapisz trzy wartości: `hasPassword = true`, `isAccountActive = true`, `isBlocked = false`. Dostęp ma być dozwolony tylko wtedy, gdy użytkownik **ma hasło, jego konto jest aktywne i nie jest zablokowany**.

W podstawowej wersji zastosuj `&&` i `!`. **Dodatkowo**, jeśli chcesz poćwiczyć trzeci operator, użyj `||` w wariancie, w którym osoba może wejść także jako administrator.

### Zadanie 8 — Projekt zaliczeniowy: prosty kalkulator rabatów (trudniejsze)

Napisz program konsolowy, który:

1. Pyta użytkownika o cenę produktu i zamienia wpisany tekst na liczbę przez `toDoubleOrNull()`.
2. Sprawdza, czy cena jest poprawna i większa od zera.
3. Dla ceny **co najmniej 100 zł** przyznaje rabat **20 zł**.
4. Dla ceny od **50 zł do mniej niż 100 zł** przyznaje rabat **10 zł**.
5. Dla ceny mniejszej niż **50 zł** nie przyznaje rabatu.
6. Wypisuje wysokość rabatu oraz kwotę do zapłaty.

**Przetestuj:** `120`, `70`, `25`, `0`, `-5`, `xyz`.

*Uwaga:* to ćwiczenie dotyczy prostego rabatu kwotowego, a nie procentowego. Nie musisz używać pętli. Wystarczy `if` / `else if` / `else`.

## 14. Mini-test — sprawdź, czy rozumiesz

Nie sprawdzaj odpowiedzi od razu. Spróbuj odpowiedzieć własnymi słowami.

1. Jaki typ wartości musi mieć warunek zapisany w `if (...)`?
2. Co robi `else`, gdy warunek `if` jest fałszywy?
3. Czy dwa osobne `if` mogą wypisać dwa komunikaty w jednym uruchomieniu?
4. Dlaczego w sprawdzaniu progów `90`, `70` i `50` zwykle zaczynamy od `90`?
5. Czym różnią się operatory `=` i `==`?
6. Jaki będzie wynik `true && false`?
7. Jaki będzie wynik `false || true`?
8. Jaki będzie wynik `!true`?
9. Do czego służy gałąź `else` w `when`?
10. Jak bezpiecznie sprawdzić, czy tekst wpisany przez użytkownika jest poprawną liczbą całkowitą?

### Kiedy przejść do lekcji 5?

Przejdź dalej, kiedy potrafisz **bez podglądania rozwiązania** napisać prosty `if / else`, samodzielnie wybrać między `&&` a `||`, oraz użyć `when` do rozpoznawania kilku wartości.

## 15. Podsumowanie

- `if` odpowiada na pytanie: „czy warunek jest prawdziwy?”.
- `else` określa, co wykonać, kiedy odpowiedź brzmi „nie”.
- `else if` pozwala rozpatrzyć kolejne możliwości, ale wykonywana jest tylko pierwsza pasująca gałąź.
- `&&` znaczy „i”, `||` znaczy „lub”, a `!` odwraca odpowiedź logiczną.
- `when` ułatwia wybór spośród kilku wartości lub warunków.
- Kolejność warunków potrafi zmienić zachowanie programu.
- Dane wpisane przez użytkownika wymagają sprawdzenia — nawet przy bardzo prostych programach.

W **lekcji 5** nauczymy się **powtarzać** instrukcje bez kopiowania tych samych linii, używając pętli `for`, `while` i `do-while`.

---

## 16. Odpowiedzi i przykładowe rozwiązania

**To przykłady poprawnych rozwiązań, nie jedyne możliwe wersje.** Możesz otrzymać ten sam wynik przy nieco inaczej napisanym kodzie.

### Zadanie 1 — Liczba dodatnia, ujemna albo zero

```kotlin-run
fun main() {
    val number = -4

    if (number > 0) {
        println("Liczba dodatnia")
    } else if (number < 0) {
        println("Liczba ujemna")
    } else {
        println("To jest zero")
    }
}
```

Zmień `number` na `8` i `0`, aby sprawdzić pozostałe odpowiedzi.

### Zadanie 2 — Liczba parzysta albo nieparzysta

```kotlin-run
fun main() {
    val number = 7

    if (number % 2 == 0) {
        println("Liczba parzysta")
    } else {
        println("Liczba nieparzysta")
    }
}
```

Liczba `7` daje resztę `1` z dzielenia przez `2`, więc jest nieparzysta.

### Zadanie 3 — Przewidywanie wyniku

Program wypisze:

```text
A
B
```

Są w nim dwa **oddzielne** `if`. Oba warunki są prawdziwe dla `points = 80`.

### Zadanie 4 — Wejście na wydarzenie

```kotlin-run
fun main() {
    val hasTicket = false
    val isOnList = true

    if (hasTicket || isOnList) {
        println("Wejście dozwolone")
    } else {
        println("Wejście zabronione")
    }
}
```

Wystarczy spełnić co najmniej jeden z dwóch warunków.

### Zadanie 5 — Dzień tygodnia

```kotlin-run
fun main() {
    val day = 2

    when (day) {
        1 -> println("Poniedziałek")
        2 -> println("Wtorek")
        3 -> println("Środa")
        else -> println("Inny dzień")
    }
}
```

### Zadanie 6 — Poprawiony program

```kotlin-run
fun main() {
    val age = 20

    if (age >= 18) {
        println("Pełnoletnia osoba")
    } else {
        println("Osoba niepełnoletnia")
    }
}
```

`>=` pyta „większy lub równy”, a więc uwzględnia także dokładnie 18 lat. Samo `== 18` sprawdzałoby **tylko** osoby mające dokładnie 18 lat.

### Zadanie 7 — Dostęp do konta

```kotlin-run
fun main() {
    val hasPassword = true
    val isAccountActive = true
    val isBlocked = false

    if (hasPassword && isAccountActive && !isBlocked) {
        println("Dostęp przyznany")
    } else {
        println("Dostęp zabroniony")
    }
}
```

**Dodatkowy wariant z administratorem i `||`:**

```kotlin-run
fun main() {
    val hasPassword = false
    val isAccountActive = true
    val isBlocked = false
    val isAdmin = true

    if (isAccountActive && !isBlocked && (hasPassword || isAdmin)) {
        println("Dostęp przyznany")
    } else {
        println("Dostęp zabroniony")
    }
}
```

Wybrane zasady dostępu są tylko **ćwiczeniem logicznym**, a nie gotowym systemem bezpieczeństwa. W realnym programie weryfikacja użytkownika wymaga dodatkowych zabezpieczeń.

### Zadanie 8 — Kalkulator rabatów

```kotlin-run
fun main() {
    print("Podaj cenę produktu w zł: ")
    val price = readln().toDoubleOrNull()

    if (price == null) {
        println("Błąd: wpisz poprawną liczbę.")
    } else if (!price.isFinite()) {
        println("Błąd: cena musi być zwykłą, skończoną liczbą.")
    } else if (price <= 0) {
        println("Błąd: cena musi być większa od zera.")
    } else {
        val discount = when {
            price >= 100 -> 20.0
            price >= 50 -> 10.0
            else -> 0.0
        }

        val finalPrice = price - discount
        println("Rabat: $discount zł")
        println("Do zapłaty: $finalPrice zł")
    }
}
```

W tym rozwiązaniu `when` wybiera wartość rabatu. Ponieważ progi `100` i `50` są sprawdzane od najwyższego, klient otrzymuje **tylko jeden**, właściwy rabat. Dodatkowe sprawdzenie `isFinite()` chroni przed nietypowymi zapisami liczb, które nie są zwykłymi cenami. Zaokrąglanie i dokładne formatowanie kwot omówimy przy dalszych projektach; w profesjonalnych systemach finansowych zwykle stosuje się inne reprezentacje niż `Double`.

Przykładowe wyniki:

| Wpis | Rabat | Do zapłaty |
|---|---:|---:|
| `120` | 20 zł | 100 zł |
| `70` | 10 zł | 60 zł |
| `25` | 0 zł | 25 zł |
| `0` | Błąd | — |
| `-5` | Błąd | — |
| `xyz` | Błąd | — |

### Mini-test — odpowiedzi

1. Wartość typu `Boolean`, czyli `true` lub `false`.
2. Wykonuje swój blok instrukcji, gdy warunek `if` jest fałszywy.
3. Tak, ponieważ każde `if` sprawdzane jest oddzielnie.
4. Inaczej wysoki wynik może zostać przechwycony przez wcześniejszy, niższy próg.
5. `=` przypisuje wartość; `==` porównuje dwie wartości.
6. `false` — oba warunki muszą być prawdziwe.
7. `true` — wystarczy jeden prawdziwy warunek.
8. `false` — operator `!` odwraca wartość logiczną.
9. Obsługuje wszystkie przypadki, których nie obejmują wcześniejsze gałęzie.
10. Użyć `toIntOrNull()` i sprawdzić, czy wynik jest `null`.

---

## Dodatkowe materiały (dla chętnych)

- [Kotlin — Conditions and loops](https://kotlinlang.org/docs/control-flow.html) — oficjalne przykłady `if`, `else if` i `when`.
- [Kotlin — Booleans](https://kotlinlang.org/docs/booleans.html) — wartości logiczne oraz `&&`, `||`, `!`.
- [Kotlin — Basic syntax](https://kotlinlang.org/docs/basic-syntax.html) — przypomnienie najważniejszych elementów składni.

**Koniec lekcji 4/36. Następna: pętle `for`, `while`, `do-while`, zakresy i powtarzanie instrukcji.**