# Lekcja 5 — Pętle i zakresy: jak powtarzać instrukcje w Kotlinie

**Poziom:** I — Fundamenty Kotlina  
**Lekcja:** 5 z 8 w Poziomie I (5 z 36 w całym kursie)  
**Temat:** `for`, zakresy `..` i `until`, `downTo`, `step`, `while`, `do-while`, `break`, `continue`  
**Czas:** około 90–120 minut (razem z ćwiczeniami i projektem)  
**Potrzebujesz:** IntelliJ IDEA i działający projekt Kotlin/JVM z wcześniejszych lekcji  
**Wymagana wiedza:** lekcje 1–4: `fun main()`, `println()`, `val`, `var`, liczby, porównania, `if`, `else`, `readln()`, `toIntOrNull()`

---

> **Jak pracować z przykładami?** Wszystkie pełne, poprawne programy mają znacznik `kotlin-run` — możesz je uruchomić w edytorze kursu lub w IntelliJ IDEA. Fragmenty do analizy oraz programy z celowymi błędami pozostają oznaczone jako `kotlin`. **Rozwiązania zadań są dopiero na końcu.**
>
> **Uwaga o `readln()`:** tylko przykłady pobierające dane wymagają możliwości wpisania tekstu do programu. Jeżeli interpreter na stronie jej nie obsługuje, uruchom je w IntelliJ IDEA (okno **Run**). Pozostałe przykłady nie pytają o żadne dane.
>
> Nadal uczymy się **samego języka Kotlin**. Do API Spigot przejdziemy w lekcji 16, a do Paper w lekcji 25. Nie potrzebujesz teraz żadnego serwera Minecraft.

## 1. Po co są pętle?

Wiesz już, jak kazać programowi wykonać polecenie i jak wybrać jedną z możliwości za pomocą `if`. A co, jeżeli chcesz wykonać **to samo polecenie pięć razy**?

Możesz napisać:

```kotlin-run
fun main() {
    println("Dzień dobry!")
    println("Dzień dobry!")
    println("Dzień dobry!")
    println("Dzień dobry!")
    println("Dzień dobry!")
}
```

Program działa, ale powtarzanie tych samych linijek jest niewygodne. Jeśli trzeba wypisać komunikat sto razy, kod staje się nieczytelny.

**Pętla** to konstrukcja, dzięki której program **wielokrotnie wykonuje fragment kodu**. Zamiast pisać pięć instrukcji, możesz napisać jedną wewnątrz pętli.

Po tej lekcji będziesz umieć:

- wyświetlić ten sam komunikat wiele razy bez kopiowania kodu;
- wykonać polecenie dla kolejnych liczb, np. od 1 do 5;
- rozróżnić zakres z włączonym i wyłączonym końcem;
- odliczać w dół i zmieniać krok odliczania;
- korzystać z `while` oraz `do-while`;
- zakończyć pętlę wcześniej przez `break` albo pominąć jeden obrót przez `continue`;
- rozpoznać pętlę, która może nigdy się nie skończyć;
- samodzielnie napisać prosty program zbierający dane od użytkownika.

## 2. Pętla `for` — powtórz instrukcję określoną liczbę razy

Zacznijmy od najprostszego przykładu:

```kotlin-run
fun main() {
    for (number in 1..5) {
        println("Dzień dobry! Numer: $number")
    }
}
```

Wynik:

```text
Dzień dobry! Numer: 1
Dzień dobry! Numer: 2
Dzień dobry! Numer: 3
Dzień dobry! Numer: 4
Dzień dobry! Numer: 5
```

**Co tutaj oznacza każdy fragment?**

| Fragment | Znaczenie |
|---|---|
| `for` | Rozpoczyna pętlę, która przechodzi przez kolejne wartości. |
| `number` | Nazwa bieżącej wartości. Możesz nadać jej inną sensowną nazwę, np. `day`. |
| `in` | Oznacza tutaj „dla kolejnych wartości z…”. |
| `1..5` | Zakres od 1 **do 5 włącznie**. |
| `{ ... }` | Instrukcje wykonywane przy każdej wartości. |

Przebieg jest prosty:

1. `number` ma wartość `1` — wykonuje się `println()`.
2. `number` ma wartość `2` — `println()` wykonuje się ponownie.
3. Tak samo dzieje się dla `3`, `4` i `5`.
4. Gdy skończą się wartości z zakresu, program przechodzi do następnej instrukcji **za pętlą**.

**Ważne:** `number` otrzymuje następną wartość automatycznie. Nie wpisujemy w środku `number++`.

Zobacz, co dzieje się po pętli:

```kotlin-run
fun main() {
    println("Początek")

    for (day in 1..3) {
        println("Dzień $day")
    }

    println("Koniec")
}
```

Wynik:

```text
Początek
Dzień 1
Dzień 2
Dzień 3
Koniec
```

**Ćwiczenie na minutę:** zmień `1..3` na `1..6`. Ile razy zostanie wyświetlony napis `Dzień`?

## 3. Zakresy: `..` i `until` — mała różnica, duże znaczenie

Dwa zapisy wyglądają podobnie, ale inaczej traktują ostatnią liczbę:

```kotlin-run
fun main() {
    print("Od 1 do 4 włącznie: ")
    for (i in 1..4) {
        print("$i ")
    }

    println()

    print("Od 1 do liczby mniejszej niż 4: ")
    for (i in 1 until 4) {
        print("$i ")
    }
}
```

Wynik:

```text
Od 1 do 4 włącznie: 1 2 3 4 
Od 1 do liczby mniejszej niż 4: 1 2 3 
```

`println()` bez argumentów po prostu przechodzi do nowej linii.

| Zapis | Jakie liczby obejmuje? | Czy ostatnia liczba jest uwzględniona? |
|---|---|---|
| `1..4` | `1`, `2`, `3`, `4` | Tak |
| `1 until 4` | `1`, `2`, `3` | Nie |
| `0 until 3` | `0`, `1`, `2` | Nie |

Kiedy przydaje się `until`? Na przykład wtedy, kiedy chcesz powtórzyć czynność **trzy razy**, a liczysz od zera:

```kotlin-run
fun main() {
    for (attempt in 0 until 3) {
        println("Próba nr ${attempt + 1}")
    }
}
```

Wynik:

```text
Próba nr 1
Próba nr 2
Próba nr 3
```

Zaczynamy od `0`, ale w komunikacie dodajemy `1`, żeby użytkownik widział bardziej naturalną numerację.

> Możesz też spotkać zapis `1..<4`. Ma on podobne znaczenie jak `1 until 4`. W naszym kursie na razie stosujemy `until`, ponieważ jest czytelny dla początkujących.

## 4. Odliczanie w dół: `downTo`

Chcesz odliczyć od 5 do 1? Użyj `downTo`:

```kotlin-run
fun main() {
    for (seconds in 5 downTo 1) {
        println("Pozostało: $seconds")
    }

    println("Start!")
}
```

Wynik:

```text
Pozostało: 5
Pozostało: 4
Pozostało: 3
Pozostało: 2
Pozostało: 1
Start!
```

`downTo` oznacza „od tej liczby w dół, do wskazanej liczby **włącznie**”.

Nie pisz `5..1`, oczekując automatycznego odliczania w dół: taki rosnący zakres jest pusty i pętla niczego nie wypisze.

```kotlin-run
fun main() {
    println("Przed pętlą")

    for (number in 5..1) {
        println(number)
    }

    println("Po pętli")
}
```

Wynik:

```text
Przed pętlą
Po pętli
```

## 5. Co drugi, co trzeci… czyli `step`

Czasami nie chcesz wypisywać wszystkich liczb. Możesz wybrać, o ile zmienia się liczba w każdym kroku:

```kotlin-run
fun main() {
    for (number in 0..10 step 2) {
        print("$number ")
    }
}
```

Wynik:

```text
0 2 4 6 8 10 
```

`step 2` oznacza: **przechodź do następnej wartości, zmieniając liczbę o 2**.

Można to połączyć z odliczaniem w dół:

```kotlin-run
fun main() {
    for (number in 10 downTo 1 step 3) {
        println(number)
    }
}
```

Wynik:

```text
10
7
4
1
```

W pętli malejącej `step 3` nie znaczy „licz w górę”. Kierunek nadal wyznacza `downTo`, a `step` podaje **wielkość kroku**.

**Nie używaj `step 0`** — krok musi być dodatni. W przeciwnym razie program zgłosi błąd podczas wykonywania.

## 6. Pętla może również obliczać wynik

Dotąd głównie wyświetlaliśmy tekst. Teraz wykorzystamy pętlę do dodawania kolejnych liczb.

```kotlin-run
fun main() {
    var sum = 0

    for (number in 1..4) {
        sum += number
        println("Po dodaniu $number mamy: $sum")
    }

    println("Suma końcowa: $sum")
}
```

Wynik:

```text
Po dodaniu 1 mamy: 1
Po dodaniu 2 mamy: 3
Po dodaniu 3 mamy: 6
Po dodaniu 4 mamy: 10
Suma końcowa: 10
```

Tutaj `sum` musi być `var`, ponieważ jej wartość się zmienia. Zapis `sum += number` to skrót od `sum = sum + number`.

Sprawdź przebieg w tabeli:

| Obrót pętli | `number` | `sum` przed dodaniem | `sum` po dodaniu |
|---|---:|---:|---:|
| 1 | 1 | 0 | 1 |
| 2 | 2 | 1 | 3 |
| 3 | 3 | 3 | 6 |
| 4 | 4 | 6 | 10 |

**Nie pomyl tego z sumą tekstów**: tutaj obie wartości mają typ liczbowy, więc `+` wykonuje dodawanie.

Jeszcze jeden przykład: krótka tabliczka mnożenia.

```kotlin-run
fun main() {
    val number = 4

    for (multiplier in 1..5) {
        println("$number × $multiplier = ${number * multiplier}")
    }
}
```

Wynik:

```text
4 × 1 = 4
4 × 2 = 8
4 × 3 = 12
4 × 4 = 16
4 × 5 = 20
```

Zwróć uwagę na `${number * multiplier}` — wewnątrz nawiasów klamrowych obliczamy wynik mnożenia.

## 7. Pętla `while` — powtarzaj, dopóki warunek jest prawdziwy

Pętla `for` jest wygodna, gdy przechodzisz przez kolejne wartości. A co, jeśli chcesz powtarzać instrukcję, **dopóki spełniony jest określony warunek**?

Do tego służy `while`:

```kotlin-run
fun main() {
    var counter = 1

    while (counter <= 4) {
        println("Licznik: $counter")
        counter++
    }

    println("Pętla zakończona")
}
```

Wynik:

```text
Licznik: 1
Licznik: 2
Licznik: 3
Licznik: 4
Pętla zakończona
```

Przeczytaj zapis `while (counter <= 4)` jako: **„dopóki licznik jest mniejszy lub równy 4”**.

W każdym obrocie:

1. Kotlin sprawdza `counter <= 4`.
2. Jeśli warunek ma wartość `true`, wykonuje kod pomiędzy `{` i `}`.
3. `counter++` zwiększa licznik o 1.
4. Program wraca do warunku.
5. Kiedy licznik osiąga `5`, warunek staje się `false` i pętla się kończy.

### `while` może wykonać się zero razy

To ważna różnica: pętla najpierw sprawdza warunek, **dopiero potem** wykonuje instrukcje.

```kotlin-run
fun main() {
    var counter = 8

    while (counter < 5) {
        println("Nie zobaczysz tego tekstu")
        counter++
    }

    println("Koniec programu")
}
```

Wynik:

```text
Koniec programu
```

Ponieważ `8 < 5` od razu jest fałszem, zawartość pętli nie wykona się ani razu.

## 8. `do-while` — wykonaj raz, potem sprawdź warunek

Czasem chcesz wykonać jakąś czynność **przynajmniej raz**, nawet jeśli warunek nie jest spełniony. Wtedy możesz użyć `do-while`:

```kotlin-run
fun main() {
    var counter = 8

    do {
        println("Ten komunikat pojawi się raz")
        counter++
    } while (counter < 5)

    println("Koniec programu")
}
```

Wynik:

```text
Ten komunikat pojawi się raz
Koniec programu
```

W `do-while` kolejność jest odwrotna niż w `while`:

1. Najpierw wykonuje się kod po `do`.
2. Dopiero potem Kotlin sprawdza warunek po `while`.
3. Jeśli warunek jest prawdziwy, program wykonuje następny obrót.

Zobacz drugi przykład z warunkiem, który początkowo jest prawdziwy:

```kotlin-run
fun main() {
    var remaining = 3

    do {
        println("Pozostało $remaining")
        remaining--
    } while (remaining > 0)

    println("Gotowe")
}
```

Wynik:

```text
Pozostało 3
Pozostało 2
Pozostało 1
Gotowe
```

### Jak wybrać odpowiednią pętlę?

| Pętla | Kiedy najczęściej jej używać? |
|---|---|
| `for` | Gdy chcesz przejść po określonym zakresie, np. od 1 do 10. |
| `while` | Gdy kod ma się powtarzać, dopóki warunek jest spełniony; możliwe jest **zero** wykonań. |
| `do-while` | Gdy zawartość pętli musi wykonać się **co najmniej raz**. |

Nie istnieje jedna „najlepsza” pętla na wszystkie sytuacje. Wybieraj zapis, który najłatwiej wyjaśnić drugiej osobie.

## 9. `break` — zakończ pętlę wcześniej

Czasami pętla ma zaplanowanych kilka obrotów, ale już w trakcie pracy wiesz, że dalsze powtarzanie nie jest potrzebne.

```kotlin-run
fun main() {
    for (number in 1..10) {
        if (number == 4) {
            println("Zatrzymuję się przy 4")
            break
        }

        println("Sprawdzam: $number")
    }

    println("Jestem już poza pętlą")
}
```

Wynik:

```text
Sprawdzam: 1
Sprawdzam: 2
Sprawdzam: 3
Zatrzymuję się przy 4
Jestem już poza pętlą
```

`break` znaczy: **wyjdź natychmiast z tej pętli**. Program nie wykona już kolejnych obrotów, lecz przejdzie do instrukcji za pętlą.

> Ważne: `break` nie oznacza „zamknij cały program”. Kończy tylko najbliższą pętlę. Zagnieżdżone pętle poznamy później, gdy będzie to potrzebne.

## 10. `continue` — pomiń jeden obrót

`continue` nie kończy całej pętli. Mówi: **„pomiń pozostałe instrukcje w tym obrocie i przejdź do następnego”**.

```kotlin-run
fun main() {
    for (number in 1..6) {
        if (number == 3) {
            continue
        }

        println("Liczba: $number")
    }
}
```

Wynik:

```text
Liczba: 1
Liczba: 2
Liczba: 4
Liczba: 5
Liczba: 6
```

Nie ma liczby `3`, ponieważ program pominął `println()` w tym obrocie. Ale pętla działała dalej i pokazała `4`, `5` i `6`.

Porównanie:

| Słowo | Co robi? |
|---|---|
| `break` | Całkowicie kończy pętlę. |
| `continue` | Pomija resztę **bieżącego** obrotu i przechodzi do kolejnego. |

Można je zastosować razem z warunkami z lekcji 4:

```kotlin-run
fun main() {
    for (number in 1..8) {
        if (number % 2 != 0) {
            continue
        }

        println("Parzysta liczba: $number")
    }
}
```

Wynik:

```text
Parzysta liczba: 2
Parzysta liczba: 4
Parzysta liczba: 6
Parzysta liczba: 8
```

Wyrażenie `number % 2 != 0` sprawdza, czy liczba jest **nieparzysta**. Jeśli tak, `continue` pomija jej wypisanie.

## 11. Pętla i dane użytkownika: ogranicz liczbę prób

Z wcześniejszych lekcji znasz `readln()` i `toIntOrNull()`. Połączymy teraz pętlę z pobieraniem danych. Zamiast pytać tylko raz, program da użytkownikowi **maksymalnie trzy próby**.

```kotlin-run
fun main() {
    var correct = false

    for (attempt in 1..3) {
        print("Próba $attempt/3. Wpisz liczbę 7: ")
        val number = readln().toIntOrNull()

        if (number == 7) {
            println("Zgadza się!")
            correct = true
            break
        } else {
            println("To nie jest 7.")
        }
    }

    if (!correct) {
        println("Wykorzystano wszystkie próby.")
    }
}
```

Przykładowy przebieg dla wpisów `5`, `abc`, `7`:

```text
Próba 1/3. Wpisz liczbę 7: 5
To nie jest 7.
Próba 2/3. Wpisz liczbę 7: abc
To nie jest 7.
Próba 3/3. Wpisz liczbę 7: 7
Zgadza się!
```

Program pokazuje trzy różne umiejętności:

- `for (attempt in 1..3)` ustala **górną granicę liczby prób**;
- `toIntOrNull()` nie powoduje awarii, kiedy ktoś wpisze `abc` zamiast liczby;
- `break` zatrzymuje dalsze pytania po uzyskaniu prawidłowej odpowiedzi.

**Uwaga:** nie musisz jeszcze pisać „systemu logowania”. W tym przykładzie ćwiczymy tylko działanie pętli i prostych warunków.

## 12. Mini-projekt — dziennik wydatków z pięciu dni

Napiszemy niewielki program konsolowy. Użytkownik poda, ile wydał w każdym z pięciu dni, a program **zsumuje poprawne dane**. Zastosujemy również `continue` do pomijania niepoprawnych kwot oraz `break` do wcześniejszego zakończenia.

**Zasady działania:**

- Program pyta o kwotę w dniach od 1 do 5.
- Dodatnia liczba lub `0` to poprawna kwota.
- Wpisanie niepoprawnego tekstu lub liczby ujemnej nie kończy programu — takie dane są pomijane.
- Wpisanie `koniec` kończy zbieranie danych wcześniej.
- Na końcu widzisz sumę i liczbę poprawnie zapisanych dni.

```kotlin-run
fun main() {
    var total = 0
    var validDays = 0

    println("=== DZIENNIK WYDATKÓW ===")

    for (day in 1..5) {
        print("Dzień $day — kwota (albo 'koniec'): ")
        val answer = readln()

        if (answer == "koniec") {
            println("Kończymy wprowadzanie danych.")
            break
        }

        val amount = answer.toIntOrNull()

        if (amount == null || amount < 0) {
            println("Niepoprawna kwota — pomijam ten dzień.")
            continue
        }

        total += amount
        validDays++
        println("Zapisano $amount zł.")
    }

    println("-------------------------")
    println("Poprawnie zapisane dni: $validDays")
    println("Suma wydatków: $total zł")
}
```

Przykładowy przebieg dla wpisów `10`, `abc`, `5`, `koniec`:

```text
=== DZIENNIK WYDATKÓW ===
Dzień 1 — kwota (albo 'koniec'): 10
Zapisano 10 zł.
Dzień 2 — kwota (albo 'koniec'): abc
Niepoprawna kwota — pomijam ten dzień.
Dzień 3 — kwota (albo 'koniec'): 5
Zapisano 5 zł.
Dzień 4 — kwota (albo 'koniec'): koniec
Kończymy wprowadzanie danych.
-------------------------
Poprawnie zapisane dni: 2
Suma wydatków: 15 zł
```

**Przeczytaj kod krok po kroku:**

1. `total` zaczyna się od `0`: będziemy w nim przechowywać sumę wpisanych kwot.
2. `validDays` zaczyna się od `0`: zliczy **tylko poprawne** wpisy.
3. `for (day in 1..5)` pozwala pobrać maksymalnie pięć odpowiedzi.
4. Jeśli odpowiedź brzmi dokładnie `koniec`, `break` kończy pętlę.
5. `toIntOrNull()` sprawdza, czy odpowiedź jest poprawną liczbą całkowitą.
6. Niepoprawna liczba uruchamia `continue`. Nie dodajemy nic do sumy ani licznika.
7. Poprawną kwotę dodajemy do sumy, a licznik dni zwiększamy o `1`.
8. Na końcu program wypisuje wyniki.

**Uproszczenie na potrzeby nauki:** kwoty są tu całkowite (bez groszy), a suma ma typ `Int`. W prawdziwym programie finansowym należałoby dokładniej zaprojektować sposób przechowywania kwot i sprawdzić ograniczenia ich zakresu. Na dziś koncentrujemy się na pętlach.

### Samodzielna modyfikacja mini-projektu

Zmień maksymalną liczbę dni z 5 na 3. Następnie sprawdź zachowanie programu, gdy **wszystkie trzy odpowiedzi są niepoprawne**. Co powinno pojawić się w podsumowaniu?

## 13. Typowe błędy początkujących

### Błąd 1 — pomylenie `..` z `until`

`1..5` wypisze również `5`, a `1 until 5` zatrzyma się na `4`. To częsty powód, dla którego pętla wykonuje się **o jeden raz za dużo lub za mało**.

### Błąd 2 — licznik w `while` nigdy się nie zmienia

**Poniższy przykład jest celowo błędny. Nie uruchamiaj go.**

```kotlin
fun main() {
    var counter = 1
    while (counter <= 3) {
        println(counter)
        // BŁĄD: zapomnieliśmy zwiększyć counter
    }
}
```

Warunek `counter <= 3` cały czas pozostaje prawdziwy. Program może wypisywać `1` bez końca.

**Jak to naprawić?** Upewnij się, że wartość używana w warunku pętli zmienia się w taki sposób, aby warunek w końcu mógł stać się fałszywy. W tym przykładzie trzeba zwiększać `counter`.

### Błąd 3 — mylenie `break` i `continue`

- `break`: po nim pętla **nie wraca** do kolejnych wartości.
- `continue`: bieżący obrót się kończy, ale pętla **próbuje następnego**.

### Błąd 4 — użycie `val` tam, gdzie zmieniamy licznik

Jeśli sami chcemy zwiększać licznik (np. w `while`), zazwyczaj potrzebujemy `var`:

```kotlin
val counter = 1
counter++ // BŁĄD — nie możesz ponownie przypisać wartości do val
```

W pętli `for` zmienna sterująca, taka jak `number`, jest ustawiana automatycznie — **nie trzeba** jej zwiększać.

### Błąd 5 — brak nawiasów klamrowych

W tym kursie konsekwentnie używamy `{ ... }` przy pętlach. Tak zapisany kod jest czytelniejszy i trudniej przypadkowo pomylić, które instrukcje są powtarzane.

### Co zrobić, kiedy program się nie zatrzymuje?

Jeśli samodzielnie tworzysz pętlę i program wypisuje ogromną ilość tekstu, przerwij go przyciskiem **Stop** w IntelliJ IDEA. Następnie sprawdź warunek zakończenia pętli oraz to, czy zmienne w nim użyte rzeczywiście się zmieniają.

## 14. Zadania do samodzielnego wykonania

**Ważne:** najpierw wykonaj ćwiczenia bez zaglądania do końcowych odpowiedzi. Każde zadanie programistyczne rozwiąż jako samodzielny program z `fun main()`. W zadaniach do przewidzenia wyniku **najpierw zapisz wynik na kartce**.

### Zadanie 1 — Liczymy od 2 do 7 (łatwe)

Za pomocą `for` wypisz kolejne liczby od `2` do `7`, każdą w osobnej linii. Nie używaj sześciu oddzielnych `println()`.

### Zadanie 2 — Co trzecią liczbę (łatwe)

Wypisz liczby `0`, `3`, `6`, `9`, `12`, `15` za pomocą pętli `for` i `step`. Nie wypisuj każdej liczby ręcznie.

### Zadanie 3 — Odliczanie (łatwe)

Za pomocą `downTo` wypisz liczby od `6` do `1`, a potem komunikat `Start!`.

### Zadanie 4 — Suma od 1 do 5 (średnie)

Za pomocą pętli oblicz `1 + 2 + 3 + 4 + 5`. Wynik powinien wynosić `15`. Nie wpisuj gotowego wyniku do `println()` i nie dodawaj tych pięciu liczb ręcznie.

### Zadanie 5 — Przewidź wynik (średnie)

**Nie uruchamiaj programu przed zapisaniem odpowiedzi.** Jakie linie wypisze ten kod?

```kotlin
fun main() {
    for (number in 2 until 7 step 2) {
        println(number)
    }

    println("Gotowe")
}
```

### Zadanie 6 — Napraw pętlę (średnie)

Program ma wypisać `1`, `2` i `3`, a następnie `Koniec`. Obecnie może pracować bez końca. Znajdź i popraw błąd:

```kotlin
fun main() {
    var number = 1

    while (number <= 3) {
        println(number)
    }

    println("Koniec")
}
```

### Zadanie 7 — `break` czy `continue`? (średnie)

Napisz program, który przechodzi przez liczby od `1` do `10`, ale:

- **pomija** liczbę `4`;
- **zatrzymuje się** po dotarciu do `8`, nie wypisując `8`.

Oczekiwane liczby: `1`, `2`, `3`, `5`, `6`, `7`. Użyj zarówno `continue`, jak i `break`.

### Zadanie 8 — Projekt zaliczeniowy: licznik punktów (trudniejsze)

Stwórz program konsolowy, który pyta o liczbę punktów zdobytych w **czterech rundach** gry. Program ma:

1. Użyć `for`, aby zapytać o rundy od 1 do 4.
2. Odczytać wynik za pomocą `readln().toIntOrNull()`.
3. Pominąć niepoprawne odpowiedzi i liczby ujemne (zastosuj `continue`).
4. Dodać poprawne wyniki do wspólnej sumy.
5. Policzyć, ile rund zostało poprawnie zapisanych.
6. Wyświetlić podsumowanie: sumę punktów i liczbę poprawnych rund.

Dla wpisów `10`, `abc`, `5`, `0` program powinien policzyć **15 punktów z trzech poprawnych rund**.

**Dodatkowe wyzwanie (nieobowiązkowe):** pozwól wpisać `koniec`, aby zakończyć zbieranie wyników przez `break`.

## 15. Mini-test — sprawdź, czy rozumiesz

Nie zaglądaj jeszcze do odpowiedzi.

1. Do czego służy pętla w programie?
2. Ile razy wykona się `for (i in 1..5)`?
3. Jakie liczby obejmuje `1 until 4`?
4. Co oznacza `downTo`?
5. Co wypisze `for (i in 2..8 step 3) { print("$i ") }`?
6. Jaka jest najważniejsza różnica między `while` i `do-while`?
7. Dlaczego w pętli `while (counter < 5)` trzeba zadbać o zmianę `counter`?
8. Czy `break` pomija tylko jeden obrót pętli?
9. Co robi `continue`?
10. Dlaczego w projekcie z licznikiem sumy używamy `var`, a nie `val`?

### Kiedy przejść do lekcji 6?

Przejdź dalej, jeżeli potrafisz **bez podglądania rozwiązań** napisać pętlę `for` z zakresem, wskazać warunek zakończenia `while` oraz poprawnie rozróżnić `break` i `continue`. Nie musisz znać wszystkiego na pamięć — ważne, by umieć wyjaśnić działanie kodu własnymi słowami.

## 16. Podsumowanie

- **`for`** przechodzi przez kolejne wartości, na przykład z zakresu liczb.
- **`1..5`** obejmuje końcową liczbę, a **`1 until 5`** jej nie obejmuje.
- **`downTo`** pozwala odliczać w dół; **`step`** zmienia wielkość kroku.
- **`while`** najpierw sprawdza warunek, a potem wykonuje kod.
- **`do-while`** najpierw wykonuje kod, a dopiero później sprawdza warunek.
- **`break`** kończy pętlę; **`continue`** pomija resztę jednego obrotu.
- Przy pętlach `while` zawsze pomyśl, **co sprawi, że pętla się zatrzyma**.

---

## 17. Odpowiedzi i przykładowe rozwiązania

> Rozwiązania są przykładowe. Możesz napisać inny kod, który spełnia wszystkie wymagania zadania.

### Zadanie 1 — liczby od 2 do 7

```kotlin-run
fun main() {
    for (number in 2..7) {
        println(number)
    }
}
```

### Zadanie 2 — co trzecia liczba

```kotlin-run
fun main() {
    for (number in 0..15 step 3) {
        println(number)
    }
}
```

### Zadanie 3 — odliczanie

```kotlin-run
fun main() {
    for (number in 6 downTo 1) {
        println(number)
    }

    println("Start!")
}
```

### Zadanie 4 — suma

```kotlin-run
fun main() {
    var sum = 0

    for (number in 1..5) {
        sum += number
    }

    println("Suma: $sum")
}
```

Wynik to `Suma: 15`. Zmienna `sum` przechowuje coraz większy wynik.

### Zadanie 5 — przewidź wynik

```text
2
4
6
Gotowe
```

`until 7` nie zawiera liczby `7`, a `step 2` przechodzi kolejno przez `2`, `4` i `6`.

### Zadanie 6 — poprawiona pętla

```kotlin-run
fun main() {
    var number = 1

    while (number <= 3) {
        println(number)
        number++
    }

    println("Koniec")
}
```

Dopisaliśmy `number++`, dzięki czemu po wypisaniu `3` licznik wzrośnie do `4` i warunek przestanie być prawdziwy.

### Zadanie 7 — pomijanie i zatrzymywanie

```kotlin-run
fun main() {
    for (number in 1..10) {
        if (number == 4) {
            continue
        }

        if (number == 8) {
            break
        }

        println(number)
    }
}
```

Liczby `4` nie wypisujemy, bo pomijamy jej obrót. Przy `8` kończymy całą pętlę.

### Zadanie 8 — projekt: licznik punktów

```kotlin-run
fun main() {
    var totalPoints = 0
    var validRounds = 0

    for (round in 1..4) {
        print("Runda $round — zdobyte punkty: ")
        val points = readln().toIntOrNull()

        if (points == null || points < 0) {
            println("Niepoprawna liczba punktów — pomijam rundę.")
            continue
        }

        totalPoints += points
        validRounds++
        println("Zapisano: $points")
    }

    println("Suma punktów: $totalPoints")
    println("Poprawnie zapisane rundy: $validRounds")
}
```

Dla wpisów `10`, `abc`, `5`, `0` wynik końcowy to:

```text
Suma punktów: 15
Poprawnie zapisane rundy: 3
```

**Dlaczego `0` jest poprawne?** Można zdobyć zero punktów w rundzie. Odrzucamy tylko liczbę ujemną albo wpis, którego nie da się zamienić na liczbę całkowitą.

### Mini-test — odpowiedzi

1. Do wielokrotnego wykonywania pewnego fragmentu kodu.
2. Pięć razy.
3. `1`, `2`, `3`.
4. Odliczanie w dół do wskazanej wartości (włącznie).
5. `2 5 8 ` (z końcową spacją).
6. `while` sprawdza warunek **przed** wykonaniem, a `do-while` **po** wykonaniu, więc `do-while` wykona się przynajmniej raz.
7. Żeby warunek mógł w końcu stać się fałszywy, a program nie wykonywał tej samej instrukcji bez końca.
8. Nie. `break` kończy całą pętlę, a jeden obrót pomija `continue`.
9. Pomija pozostałe instrukcje bieżącego obrotu i przechodzi do następnego.
10. Ponieważ w kolejnych obrotach aktualizujemy sumę, więc potrzebujemy zmiennej, której wartość można ponownie przypisywać.

---

## Dodatkowe źródła (dla chętnych)

- [Kotlin — Conditions and loops](https://kotlinlang.org/docs/control-flow.html)
- [Kotlin — Ranges and progressions](https://kotlinlang.org/docs/ranges.html)
- [Kotlin — Returns and jumps (`break`, `continue`)](https://kotlinlang.org/docs/returns.html)

**Koniec lekcji 5/36. Następna: funkcje, parametry i dzielenie programu na mniejsze części.**