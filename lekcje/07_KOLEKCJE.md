# Lekcja 7 — Kolekcje: listy, tablice, mapy i zbiory w Kotlinie

**Poziom:** I — Fundamenty Kotlina  
**Lekcja:** 7 z 8 w Poziomie I (7 z 36 w całym kursie)  
**Temat:** listy, tablice, mapy, zbiory, odczytywanie, dodawanie, usuwanie, wyszukiwanie i przechodzenie przez elementy  
**Czas:** około 90–120 minut (z ćwiczeniami i mini-projektem)  
**Potrzebujesz:** IntelliJ IDEA i działający projekt Kotlin/JVM lub edytor kursu  
**Wymagana wiedza:** lekcje 1–6: zmienne, typy, operatory, warunki, pętle i funkcje

---

> Pełne programy oznaczone jako `kotlin-run` możesz uruchomić w edytorze kursu albo wkleić do `Main.kt`. Bloki `kotlin` są przeznaczone do analizy lub samodzielnej poprawy. Rozwiązania ćwiczeń znajdują się na końcu lekcji.
>
> Nadal uczymy się języka Kotlin, bez API Minecrafta. Pierwszy plugin Spigot powstanie w lekcji 16. Jeżeli edytor kursu nie obsługuje danych wejściowych, przykład z `readln()` i jego odpowiednik w rozwiązaniach uruchom w IntelliJ IDEA.

## 1. Po co przechowywać wiele wartości razem?

Wyobraź sobie, że chcesz zapisać trzy produkty na liście zakupów: chleb, mleko i jabłka.

Można utworzyć trzy osobne wartości. A co, jeśli produktów będzie trzydzieści? Trzeba byłoby wymyślić trzydzieści nazw, a później osobno wyświetlić każdą z nich.

Kotlin pozwala przechowywać wiele informacji w jednej strukturze. Dzięki temu możemy np. przejść przez wszystkie produkty za pomocą pętli, dodać następny produkt albo sprawdzić, czy coś już znajduje się na liście.

W tej lekcji poznasz cztery sposoby przechowywania danych:

| Struktura | Kiedy się przydaje? |
|---|---|
| Lista (`List`) | Elementy mają kolejność, mogą się powtarzać. |
| Tablica (`Array`) | Elementy mają kolejność, a liczba miejsc jest ustalona przy tworzeniu. |
| Zbiór (`Set`) | Chcemy przechowywać unikalne elementy bez powtórzeń. |
| Mapa (`Map`) | Każdej unikalnej nazwie lub innemu kluczowi przypisujemy wartość. |

Tablica nie należy formalnie do interfejsów kolekcji Kotlina, ale poznajemy ją razem z nimi, ponieważ również służy do przechowywania wielu danych.

Po tej lekcji będziesz potrafić wybrać właściwą strukturę, odczytywać elementy, zmieniać zawartość struktur, które na to pozwalają, wyszukiwać informacje oraz łączyć kolekcje z funkcjami, warunkami i pętlami.

## 2. Pierwsza lista — listOf

```kotlin-run
fun main() {
    val produkty = listOf("chleb", "mleko", "jabłka")

    println(produkty)
    println("Liczba produktów: ${produkty.size}")
}
```

Wynik:

```text
[chleb, mleko, jabłka]
Liczba produktów: 3
```

Co się tutaj wydarzyło?

1. Funkcja `listOf()` tworzy listę z podanych elementów. Przecinkami oddzielamy kolejne wartości.
2. `produkty` to nazwa, pod którą przechowujemy listę.
3. `println(produkty)` wyświetla całą listę w nawiasach kwadratowych.
4. `produkty.size` oznacza liczbę elementów. Nie wpisujemy nawiasów okrągłych po `size`, ponieważ jest to właściwość, a nie wywołanie funkcji.

Kotlin rozpoznał, że wszystkie elementy są tekstami. Można również określić typ jawnie:

```kotlin
val produkty: List<String> = listOf("chleb", "mleko", "jabłka")
```

Zapis `List<String>` czytaj jako „lista tekstów”. Zawartość nawiasów ostrych informuje o typie elementów. Na razie nie musisz wiedzieć, jak samodzielnie tworzy się takie typy.

## 3. Indeksy — dlaczego pierwszy element ma numer zero?

Każdy element listy ma pozycję nazywaną **indeksem**. W Kotlinie liczymy indeksy od zera.

| Indeks | Element |
|---:|---|
| 0 | chleb |
| 1 | mleko |
| 2 | jabłka |

```kotlin-run
fun main() {
    val produkty = listOf("chleb", "mleko", "jabłka")

    println(produkty[0])
    println(produkty[1])
    println(produkty[2])
    println("Ostatni indeks: ${produkty.lastIndex}")
}
```

Wynik:

```text
chleb
mleko
jabłka
Ostatni indeks: 2
```

Nawiasy kwadratowe pozwalają odczytać element o wybranym indeksie. Lista zawiera trzy produkty, ale ostatni indeks to dwa, ponieważ liczymy od zera.

Dla niepustej listy ostatni indeks to zawsze `size - 1`. Gdy lista jest pusta, `size` wynosi zero, a `lastIndex` wynosi minus jeden. Nie istnieje wtedy żaden poprawny indeks.

### Jak uniknąć wyjścia poza listę?

Odczyt `produkty[3]` z naszej trzy-elementowej listy spowoduje błąd podczas działania programu. Najpierw sprawdź, czy indeks istnieje:

```kotlin-run
fun main() {
    val produkty = listOf("chleb", "mleko", "jabłka")
    val indeks = 3

    if (indeks in produkty.indices) {
        println(produkty[indeks])
    } else {
        println("Nie ma produktu o tym indeksie.")
    }
}
```

Wynik:

```text
Nie ma produktu o tym indeksie.
```

`indices` to zakres poprawnych indeksów. Zapis `in` pyta, czy dana wartość należy do tego zakresu.

Zmień indeks na 1 i sprawdź, co się wyświetli. Spróbuj też wpisać minus jeden.

## 4. Przechodzenie przez listę za pomocą pętli

Pamiętasz pętlę `for` z lekcji 5? Nie musi korzystać wyłącznie z zakresu liczb. Może przechodzić bezpośrednio przez elementy listy.

```kotlin-run
fun main() {
    val produkty = listOf("chleb", "mleko", "jabłka")

    for (produkt in produkty) {
        println("Kup: $produkt")
    }
}
```

Wynik:

```text
Kup: chleb
Kup: mleko
Kup: jabłka
```

Przy każdym obrocie pętli zmienna `produkt` oznacza kolejny element listy. Nie trzeba samodzielnie obliczać indeksu.

Jeśli potrzebujesz również numeru pozycji, możesz użyć `indices`:

```kotlin-run
fun main() {
    val produkty = listOf("chleb", "mleko", "jabłka")

    for (indeks in produkty.indices) {
        println("${indeks + 1}. ${produkty[indeks]}")
    }
}
```

Wynik:

```text
1. chleb
2. mleko
3. jabłka
```

Indeks nadal zaczyna się od zera. Dodajemy do niego jeden wyłącznie w komunikacie, aby lista wyświetlana użytkownikowi zaczynała się od jedynki.

## 5. Lista tylko do odczytu i lista modyfikowalna

Dotychczas tworzyliśmy listy za pomocą `listOf()`. Taka lista ma typ `List`, czyli udostępnia operacje odczytu, ale nie udostępnia metod dodawania i usuwania elementów.

Jeśli chcesz zmieniać zawartość listy, użyj `mutableListOf()`. Otrzymasz `MutableList`.

```kotlin-run
fun main() {
    val produkty = mutableListOf("chleb", "mleko")

    produkty.add("jabłka")
    println(produkty)

    produkty[1] = "jogurt"
    println(produkty)

    produkty.remove("chleb")
    println(produkty)

    produkty.removeAt(0)
    println(produkty)
}
```

Wynik:

```text
[chleb, mleko, jabłka]
[chleb, jogurt, jabłka]
[jogurt, jabłka]
[jabłka]
```

Po kolei:

- `add("jabłka")` dopisuje element na końcu.
- Zapis `produkty[1] = "jogurt"` zastępuje mleko jogurtem.
- `remove("chleb")` usuwa pierwsze wystąpienie chleba, jeśli istnieje.
- `removeAt(0)` usuwa element o indeksie zero, czyli w tym momencie jogurt.

Po usunięciu elementu pozostałe elementy mogą zmienić swoje indeksy. Przy `removeAt()` zawsze trzeba podać istniejący indeks.

### Dlaczego val, skoro lista się zmienia?

W lekcji 2 poznaliśmy różnicę między `val` i `var`. `val` nie pozwala ponownie przypisać innej wartości do tej samej nazwy. Nie oznacza jednak, że zawartości każdego obiektu nie wolno zmienić.

Tutaj `produkty` nadal wskazuje tę samą listę. Zmieniamy jej zawartość przez `add()`, ale nie przypisujemy nowej listy do nazwy `produkty`.

Dlatego `val` i `mutableListOf()` doskonale ze sobą współpracują.

Odwrotna sytuacja też jest możliwa: `var` z listą utworzoną przez `listOf()` pozwala przypisać inną listę, ale nie daje metod modyfikowania zawartości starej.

> Lista tylko do odczytu nie oznacza automatycznie, że nikt inny nie może zmienić jej zawartości. Na dziś wystarczy wiedzieć, które operacje udostępnia typ listy.

### Pusta lista

```kotlin-run
fun main() {
    val zadania = mutableListOf<String>()

    println(zadania.isEmpty())

    zadania.add("Przeczytać rozdział")
    zadania.add("Zrobić ćwiczenia")

    println(zadania.size)
    println(zadania.isEmpty())
}
```

Wynik:

```text
true
2
false
```

Przy tworzeniu pustej listy podajemy typ, aby Kotlin wiedział, jakie elementy będą do niej trafiać. Funkcja `isEmpty()` odpowiada na pytanie: „czy lista jest pusta?”.

## 6. Wyszukiwanie na liście

Nie zawsze interesuje nas konkretna pozycja. Często chcemy tylko wiedzieć, czy dana informacja istnieje.

```kotlin-run
fun main() {
    val imiona = listOf("Ola", "Bartek", "Maja", "Ola")

    println("Ola" in imiona)
    println(imiona.contains("Kasia"))
    println(imiona.indexOf("Ola"))
    println(imiona.indexOf("Kasia"))
}
```

Wynik:

```text
true
false
0
-1
```

`in` i `contains()` sprawdzają obecność elementu. `indexOf()` zwraca indeks pierwszego pasującego elementu lub minus jeden, gdy go nie ma. Lista może zawierać powtórzenia — dlatego Ola występuje tutaj dwa razy.

Wielkość liter ma znaczenie: napis „Ola” nie jest tym samym co „ola”.

### Wyszukiwanie informacji spełniających warunek

```kotlin-run
fun main() {
    val punkty = listOf(4, 9, 2, 8, 6)
    var liczbaWynikow = 0

    for (wynik in punkty) {
        if (wynik >= 6) {
            println(wynik)
            liczbaWynikow++
        }
    }

    println("Wyników co najmniej 6: $liczbaWynikow")
}
```

Wynik:

```text
9
8
6
Wyników co najmniej 6: 3
```

Wykorzystujemy wyłącznie poznane wcześniej elementy: pętlę, warunek i licznik. Nie musisz znać żadnych nowych, skomplikowanych funkcji do wyszukiwania. Do metod takich jak `filter()` wrócimy w późniejszych lekcjach.

### Liczby przechowywane na liście

```kotlin-run
fun main() {
    val wydatki = listOf(12, 8, 5)
    var suma = 0

    for (wydatek in wydatki) {
        suma += wydatek
    }

    println("Suma wydatków: $suma zł")
    println("Suma obliczona przez sum(): ${wydatki.sum()} zł")
}
```

Wynik:

```text
Suma wydatków: 25 zł
Suma obliczona przez sum(): 25 zł
```

Pierwszy sposób pokazuje, jak powstaje suma krok po kroku. Drugi korzysta z gotowej funkcji `sum()` przeznaczonej między innymi do sumowania list liczb.

## 7. Tablica — arrayOf

Tablica przypomina listę: elementy mają kolejność i indeksy. Istotna różnica polega na tym, że **liczba elementów tablicy nie zmienia się po jej utworzeniu**, choć możemy zastępować poszczególne wartości.

```kotlin-run
fun main() {
    val oceny = arrayOf(3, 4, 5)

    println("Pierwsza ocena: ${oceny[0]}")

    oceny[0] = 4

    println(oceny.contentToString())
    println("Liczba ocen: ${oceny.size}")
}
```

Wynik:

```text
Pierwsza ocena: 3
[4, 4, 5]
Liczba ocen: 3
```

Utworzyliśmy tablicę trzech ocen. Możemy zastąpić pierwszą ocenę, lecz nie dopisać czwartej do tej samej tablicy.

Do czytelnego wyświetlania całej tablicy używamy `contentToString()`. Samo `println(oceny)` na JVM zwykle pokaże techniczny opis obiektu, a nie wszystkie elementy.

Kotlin oferuje także tablice przeznaczone dla konkretnych typów liczbowych, np. `intArrayOf()`. Dzisiaj wystarczy poznać ich podstawowe działanie:

```kotlin-run
fun main() {
    val pomiary = intArrayOf(12, 15, 18)

    for (pomiar in pomiary) {
        println(pomiar)
    }
}
```

Wynik:

```text
12
15
18
```

Jeśli liczba miejsc ma się zmieniać, zazwyczaj wygodniej użyć `MutableList` niż tablicy.

## 8. Zbiór — tylko unikalne elementy

Wyobraź sobie listę obecności. Każda osoba powinna pojawić się na niej tylko raz. Nie chcemy przypadkowo policzyć tej samej osoby dwukrotnie.

Zbiór, czyli `Set`, przechowuje unikalne elementy.

```kotlin-run
fun main() {
    val uczestnicy = setOf("Ola", "Bartek", "Ola", "Maja")

    println(uczestnicy)
    println("Liczba osób: ${uczestnicy.size}")
    println("Ola" in uczestnicy)
}
```

Wynik:

```text
[Ola, Bartek, Maja]
Liczba osób: 3
true
```

Ola została podana dwa razy, ale w zbiorze istnieje tylko jedno takie wystąpienie. Zbiór nie udostępnia dostępu przez indeks tak jak lista. Nie wybieraj go, jeśli znaczenie ma pozycja elementu. Przykładowe `setOf()` zachowuje kolejność podania elementów, ale nie każdy rodzaj zbioru gwarantuje tę samą kolejność.

### Zbiór modyfikowalny

```kotlin-run
fun main() {
    val obecni = mutableSetOf("Ola", "Bartek")

    println(obecni.add("Ola"))
    println(obecni.add("Maja"))

    obecni.remove("Bartek")

    for (osoba in obecni) {
        println(osoba)
    }
}
```

Wynik:

```text
false
true
Ola
Maja
```

Pierwsza operacja `add()` zwraca `false`, ponieważ Ola już istnieje. Druga zwraca `true`, ponieważ Maja jest nowym elementem. Usuwamy Bartka, a potem przechodzimy przez pozostałe osoby.

## 9. Mapa — klucz i przypisana do niego wartość

Lista pozwala odczytać element po indeksie. Ale często wygodniej wyszukać informację po nazwie, np. sprawdzić cenę konkretnego produktu.

Mapa, czyli `Map`, przechowuje pary: **klucz i wartość**. Kluczem może być nazwa produktu, a wartością jego cena. Każdy klucz jest unikalny. Wartości mogą się powtarzać.

```kotlin-run
fun main() {
    val ceny = mapOf(
        "chleb" to 5,
        "mleko" to 4,
        "jabłka" to 6
    )

    println("Cena mleka: ${ceny["mleko"]} zł")
    println("Liczba produktów: ${ceny.size}")
}
```

Wynik:

```text
Cena mleka: 4 zł
Liczba produktów: 3
```

Zapis `"chleb" to 5` tworzy parę, w której kluczem jest tekst „chleb”, a wartością liczba pięć. Kotlin rozpoznaje typ mapy jako `Map<String, Int>`, czyli mapę z kluczami tekstowymi i wartościami całkowitymi.

W przykładzie `ceny["mleko"]` odczytuje wartość przypisaną do klucza. To nie jest indeks.

### Co, jeśli klucza nie ma?

Odczyt nieistniejącego klucza przez nawiasy kwadratowe zwraca `null`, czyli informację o braku wartości. Poznaliśmy ją już w podstawowym zakresie przy bezpiecznych konwersjach liczb. Dokładniej omówimy `null` w lekcji 8.

```kotlin-run
fun main() {
    val ceny = mapOf("chleb" to 5, "mleko" to 4)

    println(ceny["jogurt"])
    println(ceny.containsKey("mleko"))
    println(ceny.getOrDefault("jogurt", 0))
}
```

Wynik:

```text
null
true
0
```

`containsKey()` sprawdza, czy klucz istnieje. `getOrDefault()` pozwala podać wartość zastępczą, gdy klucza brakuje.

W tym przykładzie wybraliśmy zero tylko jako przykład wartości domyślnej. Nie oznacza to, że nieznany produkt jest za darmo. W prawdziwym sklepie należałoby odróżnić brak produktu od ceny wynoszącej zero.

### Przechodzenie przez mapę

```kotlin-run
fun main() {
    val ceny = mapOf("chleb" to 5, "mleko" to 4, "jabłka" to 6)

    for (wpis in ceny) {
        println("${wpis.key}: ${wpis.value} zł")
    }
}
```

Wynik:

```text
chleb: 5 zł
mleko: 4 zł
jabłka: 6 zł
```

Każdy `wpis` ma dwie części: `key`, czyli klucz, i `value`, czyli wartość. Kotlin pozwala także odczytać same klucze przez `ceny.keys` oraz same wartości przez `ceny.values`.

## 10. Zmieniamy zawartość mapy

Jeśli potrzebujemy dopisywać, usuwać lub zmieniać wpisy, wybieramy `mutableMapOf()`.

```kotlin-run
fun main() {
    val ceny = mutableMapOf("chleb" to 5, "mleko" to 4)

    ceny["jabłka"] = 6
    ceny["mleko"] = 5

    println(ceny)

    ceny.remove("chleb")
    println(ceny)
}
```

Wynik:

```text
{chleb=5, mleko=5, jabłka=6}
{mleko=5, jabłka=6}
```

Gdy klucz nie istnieje, przypisanie tworzy nowy wpis. Gdy już istnieje, aktualizuje wartość pod tym kluczem. Mleko nie pojawi się dwa razy, lecz otrzyma nową cenę.

`remove()` w mapie usuwa wpis na podstawie klucza. Nie szuka wartości.

## 11. Kolekcje i funkcje — wykorzystujemy lekcję 6

Nie musimy wykonywać wszystkiego wewnątrz `main()`. Funkcja może otrzymać listę tak samo jak tekst lub liczbę.

```kotlin-run
fun policzWyniki(punkty: List<Int>, minimum: Int): Int {
    var licznik = 0

    for (wynik in punkty) {
        if (wynik >= minimum) {
            licznik++
        }
    }

    return licznik
}

fun main() {
    val punkty = listOf(4, 9, 2, 8, 6)

    println(policzWyniki(punkty, 6))
    println(policzWyniki(punkty, 9))
}
```

Wynik:

```text
3
1
```

Funkcja dostaje listę liczb oraz minimalny wynik. Przechodzi przez listę, liczy pasujące elementy i zwraca ich liczbę. Możemy wywoływać ją wielokrotnie z różnymi danymi, bez przepisywania pętli.

Ponieważ funkcja jedynie odczytuje listę, wystarcza jej parametr typu `List<Int>`.

## 12. Mini-projekt — domowa lista zakupów

Napiszemy program, który pokazuje, co trzeba kupić, w jakiej ilości i co już zostało kupione. Nie pobiera jeszcze rzeczywistych danych ze sklepu — sami ustalamy przykładowe wartości.

Użyjemy mapy dla ilości produktów i zbioru dla kupionych rzeczy. Zamiast dodawać wiele zmiennych i osobne warunki dla każdego produktu, obsłużymy wszystkie wpisy jedną pętlą.

```kotlin-run
fun pokazZakupy(plan: Map<String, Int>, kupione: Set<String>) {
    var pozostaloSztuk = 0

    println("===== LISTA ZAKUPÓW =====")

    for (wpis in plan) {
        val produkt = wpis.key
        val ilosc = wpis.value

        if (produkt in kupione) {
            println("$produkt — $ilosc szt. — kupione")
        } else {
            println("$produkt — $ilosc szt. — do kupienia")
            pozostaloSztuk += ilosc
        }
    }

    println("Pozostało sztuk: $pozostaloSztuk")
}

fun main() {
    val plan = mutableMapOf(
        "chleb" to 2,
        "mleko" to 1,
        "jabłka" to 4
    )
    val kupione = mutableSetOf<String>()

    kupione.add("chleb")
    kupione.add("chleb")
    plan["mleko"] = 2

    pokazZakupy(plan, kupione)
}
```

Wynik:

```text
===== LISTA ZAKUPÓW =====
chleb — 2 szt. — kupione
mleko — 2 szt. — do kupienia
jabłka — 4 szt. — do kupienia
Pozostało sztuk: 6
```

Prześledźmy działanie programu:

1. Mapa zawiera trzy produkty oraz planowane ilości: dwa chleby, jedno mleko i cztery jabłka.
2. Zbiór kupionych produktów jest początkowo pusty.
3. Dwa wywołania `add("chleb")` nie tworzą dwóch wpisów, bo zbiór nie przechowuje powtórzeń.
4. W mapie zmieniamy ilość mleka z jednego na dwa.
5. Funkcja `pokazZakupy()` otrzymuje mapę i zbiór, przechodzi przez wszystkie produkty i sprawdza ich status.
6. Zlicza tylko sztuki jeszcze niekupione: dwa opakowania mleka i cztery jabłka. Wynik to sześć.

W tym uproszczonym projekcie oznaczamy cały produkt jako kupiony, czyli zakładamy zakup wszystkich zaplanowanych sztuk danego produktu. Nie obsługujemy częściowego zakupu ani zapisu do pliku.

**Mały eksperyment:** dopisz w `main()` oznaczenie mleka jako kupionego przed wywołaniem funkcji. Pozostałe instrukcje nie wymagają zmiany.

## 13. Jak wybrać odpowiednią strukturę?

| Potrzeba | Wybór |
|---|---|
| Przechowywanie kolejności utworów na playliście, także powtórzeń | Lista |
| Dopisywanie i usuwanie zadań | `MutableList` |
| Zapis trzech pomiarów o ustalonej liczbie miejsc | Tablica |
| Rejestr unikalnych uczestników | Zbiór |
| Dopisywanie i usuwanie unikalnych uczestników | `MutableSet` |
| Odczyt ceny po nazwie produktu | Mapa |
| Aktualizowanie ilości produktów według nazw | `MutableMap` |

Nie musisz używać wszystkich struktur w każdym programie. Zastanów się, czego naprawdę potrzebujesz: kolejności, ustalonej liczby miejsc, unikalności czy wyszukiwania po kluczu.

## 14. Typowe błędy początkujących

| Objaw | Przyczyna i rozwiązanie |
|---|---|
| Pierwszy element wygląda na pominięty | Indeksy zaczynają się od zera. |
| `IndexOutOfBoundsException` | Indeks listy jest poza poprawnym zakresem. Sprawdź `indices`. |
| Nie można wywołać `add()` na liście | Użyto `listOf()` zamiast `mutableListOf()`. |
| Element nie został dodany drugi raz | Zbiór przechowuje wyłącznie unikalne elementy. |
| Mapa zwraca `null` | Klucz nie istnieje; sprawdź `containsKey()` i pisownię. |
| Po ponownym przypisaniu wartości pod kluczem liczba wpisów się nie zwiększa | Aktualizujesz istniejący klucz, a nie dodajesz nowy. |
| Po wypisaniu tablicy widać niezrozumiały tekst | Skorzystaj z `contentToString()`. |

**Ważne:** nie usuwaj i nie dodawaj elementów do tej samej listy przez `add()` lub `remove()` w trakcie zwykłej pętli `for` po niej. Może to wywołać błąd lub dać nieoczekiwany wynik. Najprościej na tym etapie wykonywać takie zmiany przed pętlą lub po niej.

Gdy coś nie działa, sprawdź kolejno: typ struktury, liczbę elementów, indeks lub klucz, a następnie operację, którą próbujesz wykonać.

## 15. Ćwiczenia do samodzielnego wykonania

Wykonaj zadania samodzielnie przed zajrzeniem do odpowiedzi. Każde rozwiązanie powinno być osobnym programem z `fun main()`.

### Zadanie 1 — Lista ulubionych owoców (łatwe)

Utwórz listę zawierającą gruszkę, banana i śliwkę. Wyświetl drugi element i liczbę wszystkich elementów.

### Zadanie 2 — Lista zadań (łatwe)

Utwórz modyfikowalną listę zawierającą „nauka” i „spacer”. Dodaj „zakupy”, usuń „spacer”, a następnie wypisz każdy pozostały element w osobnej linii.

### Zadanie 3 — Przewidź wynik (łatwe)

Zapisz odpowiedź bez uruchamiania programu.

```kotlin
fun main() {
    val liczby = listOf(7, 2, 7, 4)
    println(liczby[1])
    println(liczby.indexOf(7))
    println(liczby.indexOf(9))
    println(liczby.size)
}
```

### Zadanie 4 — Tablica pomiarów (średnie)

Utwórz tablicę liczb 10, 20, 30. Zmień ostatnią wartość na 35, wypisz całą tablicę i oblicz sumę elementów za pomocą pętli.

### Zadanie 5 — Unikalne imiona (średnie)

Utwórz modyfikowalny zbiór zawierający „Ania” i „Tomek”. Dodaj „Ania” oraz „Lena”, a następnie usuń „Tomek”. Wyświetl liczbę unikalnych osób i sprawdź, czy Lena znajduje się w zbiorze.

### Zadanie 6 — Słownik cen (średnie)

Utwórz modyfikowalną mapę, w której długopis kosztuje 3 zł, a zeszyt 7 zł. Zmień cenę zeszytu na 8 zł, dodaj ołówek za 2 zł i wyświetl wszystkie ceny. Sprawdź, czy w mapie istnieje klucz „gumka”.

### Zadanie 7 — Napraw program (średnie)

Kod ma dopisać herbatę i wypisać pierwszy produkt, ale zawiera dwa błędy. Popraw je.

```kotlin
fun main() {
    val produkty = listOf("kawa", "cukier")
    produkty.add("herbata")
    println(produkty[3])
}
```

### Zadanie 8 — Projekt: rejestr gości (trudniejsze)

Napisz program przechowujący unikalne imiona gości. Początkowo zbiór zawiera Olę i Bartka. Program powinien:

1. Poprosić o imię nowej osoby przez `readln()` i usunąć zbędne spacje z początku i końca.
2. Odrzucić pusty napis.
3. Dodać nową osobę tylko wtedy, gdy jeszcze jej nie ma, i wyświetlić właściwy komunikat.
4. Za pomocą osobnej funkcji wypisać wszystkie zapisane osoby oraz ich liczbę.

Sprawdź trzy osobne uruchomienia: „Maja”, „Ola” i pusty wpis. Rozróżniamy wielkość liter. Nie trzeba zapisywać danych pomiędzy uruchomieniami.

## 16. Mini-test — sprawdź, czy rozumiesz

1. Który indeks ma pierwszy element listy?
2. Jaki jest ostatni indeks listy pięcioelementowej?
3. Jaka jest różnica między `listOf()` a `mutableListOf()`?
4. Dlaczego można dodawać elementy do modyfikowalnej listy zadeklarowanej przez `val`?
5. Co zwróci `indexOf()`, jeżeli szukanego elementu nie ma?
6. Czy można zmienić wartość elementu tablicy? Czy można zwiększyć rozmiar tej samej tablicy?
7. Co stanie się, gdy dwukrotnie dodasz ten sam napis do zbioru?
8. Co oznaczają klucz i wartość w mapie?
9. Co zwróci odczyt nieistniejącego klucza przez nawiasy kwadratowe?
10. Którą strukturę wybierzesz, gdy chcesz sprawdzać numer telefonu po nazwisku osoby?

**Kiedy przejść dalej?** Gdy potrafisz samodzielnie utworzyć listę, zmodyfikować ją, przejść przez elementy pętlą oraz wyjaśnić, czym różni się zbiór od mapy. Jeśli mylisz indeksy, wróć do ćwiczeń 1–4.

## 17. Podsumowanie

Lista pozwala przechowywać elementy w kolejności, również powtarzające się. Tablica ma stały rozmiar. Zbiór przechowuje unikalne elementy, a mapa przypisuje wartości do unikalnych kluczy. Dzięki modyfikowalnym kolekcjom możemy aktualizować dane bez tworzenia wielu osobnych zmiennych.

Znasz już fundamenty potrzebne do pisania większych programów konsolowych. W lekcji 8 uporządkujemy obsługę braku wartości, wyjątków i błędów oraz zbudujemy projekt podsumowujący cały pierwszy poziom.

---

## 18. Odpowiedzi i przykładowe rozwiązania

### Zadanie 1 — owoce

```kotlin-run
fun main() {
    val owoce = listOf("gruszka", "banan", "śliwka")
    println(owoce[1])
    println(owoce.size)
}
```

Wynik: banan, następnie 3.

### Zadanie 2 — lista zadań

```kotlin-run
fun main() {
    val zadania = mutableListOf("nauka", "spacer")
    zadania.add("zakupy")
    zadania.remove("spacer")

    for (zadanie in zadania) {
        println(zadanie)
    }
}
```

Wynik: nauka, zakupy — w osobnych liniach.

### Zadanie 3 — przewidywanie wyniku

```text
2
0
-1
4
```

Drugi element to liczba 2. Pierwsza siódemka ma indeks zero, a dziewiątki nie ma. Powtórzenia na liście wliczają się do jej rozmiaru.

### Zadanie 4 — tablica pomiarów

```kotlin-run
fun main() {
    val pomiary = arrayOf(10, 20, 30)
    pomiary[2] = 35

    var suma = 0
    for (pomiar in pomiary) {
        suma += pomiar
    }

    println(pomiary.contentToString())
    println("Suma: $suma")
}
```

Wynik:

```text
[10, 20, 35]
Suma: 65
```

### Zadanie 5 — unikalne imiona

```kotlin-run
fun main() {
    val osoby = mutableSetOf("Ania", "Tomek")
    osoby.add("Ania")
    osoby.add("Lena")
    osoby.remove("Tomek")

    println(osoby.size)
    println("Lena" in osoby)
}
```

Wynik: 2, następnie true.

### Zadanie 6 — słownik cen

```kotlin-run
fun main() {
    val ceny = mutableMapOf("długopis" to 3, "zeszyt" to 7)
    ceny["zeszyt"] = 8
    ceny["ołówek"] = 2

    for (wpis in ceny) {
        println("${wpis.key}: ${wpis.value} zł")
    }

    println(ceny.containsKey("gumka"))
}
```

Wynik:

```text
długopis: 3 zł
zeszyt: 8 zł
ołówek: 2 zł
false
```

### Zadanie 7 — poprawiony program

```kotlin-run
fun main() {
    val produkty = mutableListOf("kawa", "cukier")
    produkty.add("herbata")
    println(produkty[0])
}
```

Wynik: kawa. Potrzebujemy modyfikowalnej listy. Pierwszy produkt ma indeks zero; indeks trzy nie istnieje nawet po dodaniu herbaty.

### Zadanie 8 — projekt: rejestr gości

```kotlin-run
fun pokazGosci(goscie: Set<String>) {
    println("===== GOŚCIE =====")
    for (gosc in goscie) {
        println(gosc)
    }
    println("Liczba gości: ${goscie.size}")
}

fun main() {
    val goscie = mutableSetOf("Ola", "Bartek")

    print("Podaj imię: ")
    val imie = readln().trim()

    if (imie.isEmpty()) {
        println("Imię nie może być puste.")
    } else if (goscie.add(imie)) {
        println("Dodano: $imie")
    } else {
        println("Ta osoba jest już zapisana.")
    }

    pokazGosci(goscie)
}
```

Przykładowa rozmowa:

```text
Podaj imię: Maja
Dodano: Maja
===== GOŚCIE =====
Ola
Bartek
Maja
Liczba gości: 3
```

Wpisanie Oli nie zwiększa liczby gości. Pusty wpis również nie zmienia zbioru. Zwróć uwagę, że wykorzystujemy wynik `add()` w warunku: ta metoda zarówno próbuje dodać element, jak i zwraca informację, czy udało się dodać nowy wpis.

### Mini-test — odpowiedzi

1. Zero.
2. Cztery.
3. `listOf()` udostępnia odczyt, natomiast `mutableListOf()` również modyfikowanie elementów.
4. Zmieniamy zawartość tej samej listy, a nie wartość przypisaną do jej nazwy.
5. Minus jeden.
6. Tak, element można zastąpić; liczba miejsc w tej samej tablicy nie może się zmienić.
7. Zbiór przechowa tylko jedno wystąpienie.
8. Klucz identyfikuje wpis, a wartość to informacja do niego przypisana.
9. `null`.
10. Mapę, zakładając że nazwiska w tym uproszczonym przykładzie są unikalne.

---

## Dodatkowe źródła (dla chętnych)

- [Kotlin — przegląd kolekcji](https://kotlinlang.org/docs/collections-overview.html)
- [Kotlin — tworzenie kolekcji](https://kotlinlang.org/docs/constructing-collections.html)
- [Kotlin — tablice](https://kotlinlang.org/docs/arrays.html)
- [Kotlin — operacje na mapach](https://kotlinlang.org/docs/map-operations.html)

**Koniec lekcji 7/36. Następna: null, wyjątki i projekt konsolowy.**
