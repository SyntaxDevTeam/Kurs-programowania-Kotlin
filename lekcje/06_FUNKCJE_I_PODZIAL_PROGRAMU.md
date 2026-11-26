# Lekcja 6 — Funkcje: jak dzielić program na mniejsze części

**Poziom:** I — Fundamenty Kotlina  
**Lekcja:** 6 z 8 w Poziomie I (6 z 36 w całym kursie)  
**Temat:** `fun`, wywoływanie funkcji, parametry, argumenty, `return`, typ zwracany, `Unit`, wartości domyślne, nazwane argumenty i zasięg zmiennych  
**Czas:** około 90–120 minut (wraz z ćwiczeniami i mini-projektem)  
**Potrzebujesz:** IntelliJ IDEA i działający projekt Kotlin/JVM z poprzednich lekcji  
**Wymagana wiedza:** lekcje 1–5: `fun main()`, `val`, `var`, typy danych, `if`, operatory, pętle, `readln()` i `toIntOrNull()`

---

> **Jak pracować z kodem?** Bloki `kotlin-run` to **kompletne programy**, które możesz uruchomić w edytorze kursu lub wkleić do `Main.kt`. Bloki `kotlin` pokazują fragmenty, zadania do samodzielnego przemyślenia albo celowe błędy. Rozwiązania ćwiczeń znajdują się **na samym końcu lekcji**.
>
> **Uwaga:** część przykładów pyta o dane przez `readln()`. Jeżeli interpreter na stronie nie obsługuje wpisywania tekstu, uruchom te przykłady w IntelliJ IDEA, w dolnym oknie **Run**.
>
> Dzisiaj uczymy się **samego Kotlina**, bez API Minecrafta. Pierwszy plugin Spigot zaplanowany jest dopiero w lekcji 16.

## 1. Po co nam funkcje?

Wyobraź sobie program, który kilka razy wyświetla takie samo powitanie. Na początku można po prostu przepisać trzy instrukcje. Ale gdy program urośnie, powtarzanie kodu zacznie przeszkadzać: każdy tekst trzeba będzie poprawiać w wielu miejscach.

**Funkcja** to **nazwany fragment programu, który wykonuje określone zadanie**. Możesz go napisać raz, a potem uruchamiać tyle razy, ile potrzebujesz.

Co ważne, znasz już jedną funkcję: `main()`. To od niej zaczyna się zwykły program konsolowy. Teraz nauczysz się tworzyć **własne** funkcje.

Po ukończeniu lekcji będziesz potrafić:

- stworzyć funkcję za pomocą `fun` i wywołać ją z `main()`;
- przekazać funkcji dane za pomocą **parametrów**;
- rozumieć różnicę między **parametrem** a **argumentem**;
- zwrócić wynik przez `return`, zamiast tylko wyświetlić go na ekranie;
- rozpoznać funkcje zwracające `Int`, `String`, `Boolean` lub `Unit`;
- używać argumentów domyślnych i nazwanych;
- zrozumieć, dlaczego zmienna utworzona wewnątrz funkcji jest zwykle niewidoczna poza nią;
- uporządkować większy program w kilka małych funkcji.

## 2. Pierwsza własna funkcja

Zacznijmy od najprostszego przykładu:

```kotlin-run
fun przywitaj() {
    println("Dzień dobry!")
}

fun main() {
    przywitaj()
}
```

Wynik:

```text
Dzień dobry!
```

Przeczytajmy kod spokojnie:

1. `fun` rozpoczyna **definicję funkcji**.
2. `przywitaj` to jej **nazwa**. Wybieraj nazwy mówiące, co funkcja robi.
3. `()` oznacza, że na razie nie przekazujemy jej żadnych danych.
4. `{ ... }` zawiera instrukcje wykonywane przez funkcję.
5. W `main()` zapis `przywitaj()` **wywołuje** funkcję, czyli uruchamia jej instrukcje.

**Ważna różnica:** napisanie definicji funkcji nie wystarcza, by jej kod się wykonał. Trzeba ją jeszcze **wywołać**.

### A co, jeśli nie wywołamy funkcji?

```kotlin-run
fun pokazWiadomosc() {
    println("Tego komunikatu nie zobaczysz.")
}

fun main() {
    println("Działa tylko funkcja main.")
}
```

Wynik:

```text
Działa tylko funkcja main.
```

Funkcja `pokazWiadomosc()` istnieje, ale nikt jej nie uruchomił. To zupełnie normalne zachowanie.

## 3. Wywołuj funkcję wiele razy

```kotlin-run
fun narysujLinie() {
    println("--------------------")
}

fun main() {
    narysujLinie()
    println("LISTA ZADAŃ")
    narysujLinie()
    println("1. Posprzątaj biurko")
    println("2. Przeczytaj książkę")
    narysujLinie()
}
```

Wynik:

```text
--------------------
LISTA ZADAŃ
--------------------
1. Posprzątaj biurko
2. Przeczytaj książkę
--------------------
```

Jedną funkcję `narysujLinie()` wykorzystaliśmy aż **trzy razy**. Jeśli zmienisz długość linii w jej definicji, zmienią się wszystkie trzy linie w wyniku programu.

### Funkcje mogą wywoływać inne funkcje

```kotlin-run
fun przywitanie() {
    println("Witaj!")
}

fun pozegnanie() {
    println("Do zobaczenia!")
}

fun pokazSpotkanie() {
    przywitanie()
    println("Dziś uczymy się funkcji.")
    pozegnanie()
}

fun main() {
    pokazSpotkanie()
}
```

Wynik:

```text
Witaj!
Dziś uczymy się funkcji.
Do zobaczenia!
```

Dzięki temu program można czytać jak listę czynności. Nie musimy umieszczać każdej instrukcji wewnątrz `main()`.

## 4. Parametr — informacja, której funkcja potrzebuje

Dotychczas `przywitaj()` za każdym razem wypisywało to samo. A co, jeśli chcemy witać **różne osoby**?

```kotlin-run
fun przywitaj(imie: String) {
    println("Cześć, $imie!")
}

fun main() {
    przywitaj("Ola")
    przywitaj("Bartek")
    przywitaj("Maja")
}
```

Wynik:

```text
Cześć, Ola!
Cześć, Bartek!
Cześć, Maja!
```

Spójrz na zapis `fun przywitaj(imie: String)`:

- `imie` jest **parametrem** — nazwą informacji, której funkcja potrzebuje do pracy;
- `String` to typ tego parametru; Kotlin wie, że oczekujemy tekstu;
- `przywitaj("Ola")` to **wywołanie**, w którym przekazujemy konkretną wartość, czyli **argument** `"Ola"`.

**Zapamiętaj:** parametr zapisujesz w **definicji** funkcji, a argument podajesz przy jej **wywołaniu**.

### Parametr nie musi być tekstem

```kotlin-run
fun pokazWiek(wiek: Int) {
    println("Masz $wiek lat.")
}

fun main() {
    pokazWiek(18)
    pokazWiek(25)
}
```

Wynik:

```text
Masz 18 lat.
Masz 25 lat.
```

Jeżeli funkcja oczekuje `Int`, nie można przekazać jej napisu `"18"` bez wcześniejszej konwersji. Typ parametru działa jak informacja: „takiego rodzaju danych oczekuję”.

## 5. Więcej niż jeden parametr

Funkcja może potrzebować kilku informacji. Oddzielamy je **przecinkami**:

```kotlin-run
fun przedstawOsobe(imie: String, wiek: Int) {
    println("$imie ma $wiek lat.")
}

fun main() {
    przedstawOsobe("Ania", 20)
    przedstawOsobe("Tomek", 17)
}
```

Wynik:

```text
Ania ma 20 lat.
Tomek ma 17 lat.
```

W zwykłym wywołaniu **kolejność argumentów ma znaczenie**. Funkcja oczekuje najpierw `String`, potem `Int`.

### Mały eksperyment z trzema parametrami

```kotlin-run
fun pokazProdukt(nazwa: String, cena: Int, sztuki: Int) {
    println("Produkt: $nazwa")
    println("Cena za sztukę: $cena zł")
    println("Liczba sztuk: $sztuki")
}

fun main() {
    pokazProdukt("Zeszyt", 8, 3)
}
```

Wynik:

```text
Produkt: Zeszyt
Cena za sztukę: 8 zł
Liczba sztuk: 3
```

**Ćwiczenie w trakcie czytania:** zmień nazwę produktu i liczbę sztuk, a następnie uruchom program ponownie. W definicji funkcji nie musisz niczego zmieniać.

## 6. Wyświetlanie wyniku to nie to samo co zwracanie wyniku

To jeden z najważniejszych fragmentów tej lekcji.

Dotychczas funkcje używały `println()` — czyli **pokazywały** coś użytkownikowi. Ale czasem potrzebujemy, aby funkcja **obliczyła wynik**, z którego skorzystamy później.

Najpierw przykład, który **tylko wypisuje** sumę:

```kotlin-run
fun pokazSume(a: Int, b: Int) {
    println(a + b)
}

fun main() {
    pokazSume(4, 6)
}
```

Wynik: `10`.

Teraz wersja, która **zwraca** sumę:

```kotlin-run
fun dodaj(a: Int, b: Int): Int {
    return a + b
}

fun main() {
    val wynik = dodaj(4, 6)
    println("Wynik: $wynik")
}
```

Wynik:

```text
Wynik: 10
```

Różnica jest bardzo ważna:

| Zapis | Co robi? |
|---|---|
| `println(a + b)` | **Wyświetla** sumę, ale nie oddaje jej jako wyniku funkcji. |
| `return a + b` | **Oddaje** sumę do miejsca, z którego wywołano funkcję. |
| `: Int` | Informuje, że funkcja ma oddać liczbę całkowitą. |
| `val wynik = dodaj(4, 6)` | Przechowuje oddany wynik, dzięki czemu możemy go dalej wykorzystać. |

### Ten sam wynik wykorzystany dwa razy

```kotlin-run
fun obliczKoszt(cena: Int, ilosc: Int): Int {
    return cena * ilosc
}

fun main() {
    val koszt = obliczKoszt(7, 3)
    println("Do zapłaty: $koszt zł")
    println("Po doliczeniu 5 zł za dostawę: ${koszt + 5} zł")
}
```

Wynik:

```text
Do zapłaty: 21 zł
Po doliczeniu 5 zł za dostawę: 26 zł
```

Gdyby `obliczKoszt()` tylko wypisało liczbę, trudniej byłoby wykorzystać ją do następnego obliczenia. **Funkcje zwracające wynik są bardzo przydatne w większych programach.**

## 7. Typ zwracany: `String` i `Boolean`

Nie każda funkcja oddaje liczbę.

### Funkcja zwracająca tekst

```kotlin-run
fun przygotujPowitanie(imie: String): String {
    return "Witaj, $imie!"
}

fun main() {
    val tekst = przygotujPowitanie("Marta")
    println(tekst)
}
```

Wynik: `Witaj, Marta!`.

Funkcja `przygotujPowitanie()` **nie wyświetla** wiadomości. Tworzy ją i oddaje jako `String`. To `main()` decyduje, co zrobić z otrzymanym tekstem.

### Funkcja zwracająca odpowiedź „tak” lub „nie”

```kotlin-run
fun jestPelnoletni(wiek: Int): Boolean {
    return wiek >= 18
}

fun main() {
    println(jestPelnoletni(16))
    println(jestPelnoletni(21))
}
```

Wynik:

```text
false
true
```

Zapis `wiek >= 18` sam daje wartość `Boolean`: `true` albo `false`. Tę odpowiedź oddajemy przez `return`.

### Wykorzystaj wynik funkcji w `if`

```kotlin-run
fun moznaWejsc(wiek: Int): Boolean {
    return wiek >= 16
}

fun main() {
    val wiek = 15

    if (moznaWejsc(wiek)) {
        println("Możesz wejść na wydarzenie.")
    } else {
        println("Wstęp od 16 lat.")
    }
}
```

Wynik: `Wstęp od 16 lat.`.

Ta funkcja odpowiada tylko na **jedno pytanie**: czy osoba ma odpowiedni wiek. W `main()` decydujemy, jaki komunikat wyświetlić. Dzięki temu nie mieszamy obliczeń z wyświetlaniem wiadomości.

## 8. `Unit` — funkcja, która nie oddaje potrzebnego wyniku

Spójrz na funkcję `pokazKomunikat()`. Nie ma po jej nawiasach dopisku `: Int` ani `: String`:

```kotlin-run
fun pokazKomunikat() {
    println("Zapisano ustawienia.")
}

fun main() {
    pokazKomunikat()
}
```

Takie funkcje mają w Kotlinie typ wyniku **`Unit`**. Możemy zapisać go wprost:

```kotlin-run
fun pokazKomunikat(): Unit {
    println("Zapisano ustawienia.")
}

fun main() {
    pokazKomunikat()
}
```

Oba programy działają tak samo. Najczęściej nie zapisujemy `: Unit`, ponieważ Kotlin sam to rozumie.

**Dla początkujących najważniejsze jest to:** funkcja może **wykonać czynność** (np. coś wyświetlić) albo **obliczyć i oddać wynik**. Nie każda funkcja musi zwracać liczbę lub tekst.

## 9. `return` kończy wykonywanie bieżącej funkcji

Po wykonaniu `return` funkcja przekazuje wynik i **nie wykonuje już następnych instrukcji w tej funkcji**.

Przyjrzyj się temu programowi:

```kotlin-run
fun sprawdzWiek(wiek: Int): String {
    if (wiek < 0) {
        return "Błąd: wiek nie może być ujemny."
    }

    return "Podany wiek: $wiek"
}

fun main() {
    println(sprawdzWiek(-2))
    println(sprawdzWiek(18))
}
```

Wynik:

```text
Błąd: wiek nie może być ujemny.
Podany wiek: 18
```

Dla `-2` wykonuje się pierwsze `return`. Druga instrukcja `return` nie zostaje wykonana w tym wywołaniu.

> Zwróć uwagę: `return` kończy **funkcję, w której występuje**, a nie cały program. `main()` może dalej wywoływać inne funkcje.

## 10. Krótszy zapis dla prostych funkcji

Czasami funkcja robi tylko jedno proste obliczenie. Kotlin pozwala zapisać ją z użyciem `=`:

```kotlin-run
fun podwoj(liczba: Int): Int = liczba * 2

fun main() {
    println(podwoj(5))
}
```

Wynik: `10`.

Ten zapis działa tak samo jak:

```kotlin-run
fun podwoj(liczba: Int): Int {
    return liczba * 2
}

fun main() {
    println(podwoj(5))
}
```

Nie musisz używać krótszej wersji. W nauce często **czytelniejsza jest wersja z klamrami i `return`**, zwłaszcza gdy w funkcji dzieje się kilka rzeczy.

## 11. Argumenty domyślne — nie zawsze trzeba podawać wszystko

Wyobraź sobie funkcję wyświetlającą powitanie. Najczęściej ma to być zwykłe „Cześć”, ale czasami chcemy użyć innego słowa.

```kotlin-run
fun przywitaj(imie: String, poczatek: String = "Cześć") {
    println("$poczatek, $imie!")
}

fun main() {
    przywitaj("Ola")
    przywitaj("Bartek", "Dzień dobry")
}
```

Wynik:

```text
Cześć, Ola!
Dzień dobry, Bartek!
```

Co oznacza `poczatek: String = "Cześć"`?

- Parametr `poczatek` jest typu `String`.
- Jeśli przy wywołaniu **nie podamy** drugiego argumentu, funkcja użyje tekstu `"Cześć"`.
- Jeśli go podamy, funkcja skorzysta z przekazanej wartości.

To nazywamy **argumentem domyślnym** — dokładniej, *wartością domyślną parametru*.

### Możesz mieć kilka wartości domyślnych

```kotlin-run
fun pokazUstawienia(jezyk: String = "polski", powiadomienia: Boolean = true) {
    println("Język: $jezyk")
    println("Powiadomienia: $powiadomienia")
}

fun main() {
    pokazUstawienia()
    pokazUstawienia("angielski", false)
}
```

Wynik:

```text
Język: polski
Powiadomienia: true
Język: angielski
Powiadomienia: false
```

Wartość domyślna nie jest „na zawsze” — obowiązuje tylko przy wywołaniach, w których dany argument został pominięty.

## 12. Argumenty nazwane — czytelniejsze wywołanie

Przy wielu parametrach łatwo pomylić ich kolejność. Kotlin pozwala przy wywołaniu napisać nazwę parametru i znak `=`:

```kotlin-run
fun pokazRezerwacje(imie: String, liczbaMiejsc: Int, vip: Boolean) {
    println("Osoba: $imie")
    println("Miejsca: $liczbaMiejsc")
    println("VIP: $vip")
}

fun main() {
    pokazRezerwacje(liczbaMiejsc = 2, vip = false, imie = "Kasia")
}
```

Wynik:

```text
Osoba: Kasia
Miejsca: 2
VIP: false
```

Tutaj argumenty podano w innej kolejności niż w definicji, ale dzięki **nazwom** Kotlin wie, gdzie każdą wartość przypisać.

Porównaj:

```kotlin-run
fun obliczRabat(cena: Int, procent: Int = 10): Int {
    return cena * procent / 100
}

fun main() {
    println(obliczRabat(cena = 200))
    println(obliczRabat(procent = 25, cena = 200))
}
```

Wynik:

```text
20
50
```

Dzięki temu wywołanie samo wyjaśnia, które liczby oznaczają cenę, a które procent rabatu.

## 13. Zasięg zmiennych — gdzie dana nazwa jest widoczna?

**Zasięg** to po prostu miejsce w programie, w którym można korzystać z danej nazwy.

Jeżeli tworzysz zmienną **wewnątrz funkcji**, zwykle istnieje ona tylko w tej funkcji. To ważne, bo chroni przed przypadkowym używaniem informacji z niewłaściwego miejsca.

```kotlin-run
fun pokazCene() {
    val cena = 15
    println("Cena w funkcji: $cena zł")
}

fun main() {
    pokazCene()
    println("Program skończył działanie.")
}
```

Wynik:

```text
Cena w funkcji: 15 zł
Program skończył działanie.
```

A tego kodu **nie da się poprawnie skompilować**:

```kotlin
fun pokazCene() {
    val cena = 15
}

fun main() {
    println(cena) // Błąd: tutaj nie znamy nazwy „cena”.
}
```

Funkcja `main()` **nie widzi** `cena`, bo ta zmienna należy do `pokazCene()`.

### Jak przekazać wynik między funkcjami?

Zamiast próbować dostać się do cudzej zmiennej, **zwróć wynik**:

```kotlin-run
fun obliczCene(): Int {
    val cena = 15
    return cena
}

fun main() {
    val doZaplaty = obliczCene()
    println("Do zapłaty: $doZaplaty zł")
}
```

Wynik: `Do zapłaty: 15 zł`.

### Parametr też ma swój zasięg

```kotlin-run
fun pokazImie(imie: String) {
    println("Witaj, $imie!")
}

fun main() {
    val mojeImie = "Lena"
    pokazImie(mojeImie)
}
```

Wynik: `Witaj, Lena!`.

W `main()` używamy nazwy `mojeImie`. W funkcji `pokazImie()` używamy parametru `imie`. To **dwie różne nazwy**, które odnoszą się do przekazanej wartości tekstowej.

### Nazwy w różnych funkcjach mogą się powtarzać

```kotlin-run
fun pokazPierwszaCene() {
    val cena = 10
    println("Pierwsza cena: $cena")
}

fun pokazDrugaCene() {
    val cena = 25
    println("Druga cena: $cena")
}

fun main() {
    pokazPierwszaCene()
    pokazDrugaCene()
}
```

Wynik:

```text
Pierwsza cena: 10
Druga cena: 25
```

To nie jest konflikt, bo każda zmienna `cena` znajduje się w **innej funkcji**.

## 14. Łączymy funkcje z wcześniej poznanymi warunkami i pętlami

Funkcja może zawierać `if`, pętlę i inne instrukcje, które już znasz. Nie musisz uczyć się nowych zasad, aby z nich korzystać.

```kotlin-run
fun pokazOdliczanie(od: Int) {
    for (liczba in od downTo 1) {
        println(liczba)
    }
    println("Start!")
}

fun main() {
    pokazOdliczanie(3)
}
```

Wynik:

```text
3
2
1
Start!
```

### Dwie funkcje współpracują ze sobą

```kotlin-run
fun czyParzysta(liczba: Int): Boolean {
    return liczba % 2 == 0
}

fun opiszLiczbe(liczba: Int) {
    if (czyParzysta(liczba)) {
        println("$liczba jest parzysta.")
    } else {
        println("$liczba jest nieparzysta.")
    }
}

fun main() {
    for (liczba in 1..4) {
        opiszLiczbe(liczba)
    }
}
```

Wynik:

```text
1 jest nieparzysta.
2 jest parzysta.
3 jest nieparzysta.
4 jest parzysta.
```

Zauważ, jak naturalnie czyta się kod w `main()`: „dla każdej liczby od 1 do 4 — opisz tę liczbę”. Szczegóły, **jak** ją opisać, znajdują się w osobnej funkcji.

## 15. Mini-projekt — kasa biletowa

Stworzymy małą kasę biletową. Program zapyta o wiek kupującej osoby i liczbę biletów. Zgodnie z naszymi **wymyślonymi zasadami** cena biletu to:

- **16 zł** dla osoby poniżej 18 lat;
- **24 zł** dla osoby od 18 lat.

Dzięki kilku funkcjom `main()` będzie krótka i łatwa do zrozumienia.

**Plan programu:**

1. `pokazNaglowek()` — wyświetla tytuł.
2. `wybierzCene(wiek)` — oddaje cenę jednego biletu.
3. `obliczRazem(cena, liczbaBiletow)` — oblicza koszt całego zakupu.
4. `pokazRachunek(...)` — wyświetla podsumowanie.
5. `main()` — pobiera dane, sprawdza je i wywołuje właściwe funkcje.

```kotlin-run
fun pokazNaglowek() {
    println("===== KASA BILETOWA =====")
}

fun wybierzCene(wiek: Int): Int {
    if (wiek < 18) {
        return 16
    }
    return 24
}

fun obliczRazem(cena: Int, liczbaBiletow: Int): Long {
    return cena.toLong() * liczbaBiletow
}

fun pokazRachunek(cena: Int, liczbaBiletow: Int, razem: Long, waluta: String = "zł") {
    println("Cena jednego biletu: $cena $waluta")
    println("Liczba biletów: $liczbaBiletow")
    println("Do zapłaty: $razem $waluta")
}

fun main() {
    pokazNaglowek()

    print("Podaj swój wiek: ")
    val wiek = readln().toIntOrNull()

    print("Podaj liczbę biletów: ")
    val liczbaBiletow = readln().toIntOrNull()

    if (wiek == null || wiek < 0 || liczbaBiletow == null || liczbaBiletow <= 0) {
        println("Błąd: podaj poprawny wiek i dodatnią liczbę biletów.")
    } else {
        val cena = wybierzCene(wiek)
        val razem = obliczRazem(cena, liczbaBiletow)
        pokazRachunek(cena, liczbaBiletow, razem)
    }
}
```

Przykładowa rozmowa (użytkownik wpisał `17`, a potem `3`):

```text
===== KASA BILETOWA =====
Podaj swój wiek: 17
Podaj liczbę biletów: 3
Cena jednego biletu: 16 zł
Liczba biletów: 3
Do zapłaty: 48 zł
```

**Co warto tutaj zauważyć?**

- Wywołanie `pokazNaglowek()` nie wymaga żadnych danych.
- `wybierzCene(wiek)` **zwraca** cenę. Nie wyświetla jej samodzielnie.
- `obliczRazem()` oblicza kwotę. Używamy `Long`, żeby bezpieczniej pomnożyć dwie liczby całkowite; nie musisz jeszcze zapamiętywać zakresów liczbowych.
- `pokazRachunek()` **wyświetla** wynik. Parametr `waluta` ma wartość domyślną `"zł"`.
- Sprawdzenie wieku i liczby biletów odbywa się przed obliczeniami.

### Samodzielna mała modyfikacja

Zmień cenę dla niepełnoletnich na 12 zł i dla dorosłych na 27 zł. Upewnij się, że modyfikujesz **tylko funkcję `wybierzCene()`**, a reszta programu działa tak jak wcześniej. Następnie spróbuj przy wywołaniu `pokazRachunek()` podać nazwany argument `waluta = "PLN"`.

## 16. Typowe błędy początkujących

| Objaw | Dlaczego się pojawia? | Jak to poprawić? |
|---|---|---|
| Program nie wypisuje wiadomości z nowej funkcji | Zdefiniowano funkcję, ale jej nie wywołano | Wywołaj ją, np. `powitaj()` w `main()`. |
| `No value passed for parameter` | Funkcja oczekuje argumentu, ale go pominięto | Podaj wartość albo zdefiniuj sensowną wartość domyślną. |
| `Type mismatch` | Przekazany argument ma inny typ | Sprawdź typ parametru, np. `Int` zamiast `String`. |
| `Unresolved reference` | Użyto nazwy poza miejscem jej działania | Przekaż wartość parametrem lub przez `return`. |
| Funkcja `: Int` nie zwraca liczby | Zamiast `return` użyto wyłącznie `println()` | Oddaj obliczony wynik przez `return`. |
| Niewłaściwe wartości trafiają do parametrów | Pomylenie kolejności argumentów o tym samym typie | Użyj argumentów nazwanych. |
| Program kończy funkcję zbyt wcześnie | `return` występuje przed kolejnymi instrukcjami | Sprawdź, czy wcześniejszy `return` jest potrzebny. |
| Dwie funkcje o tej samej nazwie i tych samych parametrach | Zduplikowano definicję | Zostaw jedną definicję; osobne programy uruchamiaj pojedynczo. |

### Jak szukać przyczyny błędu?

1. Sprawdź, **w której funkcji** błąd wystąpił.
2. Przeczytaj **nazwę funkcji i listę parametrów** — ile danych potrzebuje i jakiego typu?
3. Sprawdź, czy przy wywołaniu przekazujesz wszystkie potrzebne wartości.
4. Jeśli chcesz użyć wyniku, sprawdź, czy funkcja go **zwraca**, czy tylko **wyświetla**.
5. Popraw jedną rzecz i uruchom program ponownie.

## 17. Zadania do samodzielnego wykonania

**Zasada:** wykonaj każde zadanie w osobnym uruchomieniu `Main.kt`. Nie zaglądaj jeszcze do odpowiedzi. Najpierw spróbuj, nawet jeśli popełnisz kilka błędów — to część nauki.

### Zadanie 1 — Powitanie w funkcji (łatwe)

Napisz funkcję `przywitaj()`, która wypisze `Witaj na kursie Kotlina!`. W `main()` wywołaj ją **dwa razy**. Nie kopiuj dwóch takich samych `println()` do `main()`.

### Zadanie 2 — Powitanie z parametrem (łatwe)

Napisz funkcję `powitajOsobe(imie: String)`, która wypisuje `Miło Cię widzieć, <imię>!`. Wywołaj ją dla dwóch różnych imion.

### Zadanie 3 — Obliczanie pola (łatwe)

Napisz funkcję `poleProstokata(szerokosc: Int, wysokosc: Int): Int`, która **zwraca** pole prostokąta. W `main()` oblicz pole dla szerokości 4 i wysokości 6, zapisz wynik w `val` i wyświetl. Oczekiwany wynik: `24`.

### Zadanie 4 — Wartość domyślna (średnie)

Zdefiniuj funkcję `pokazPowitanie(imie: String, slowo: String = "Cześć")`. W `main()` użyj jej raz z jednym argumentem, a drugi raz z dwoma argumentami. Wyświetl różne pozdrowienia.

### Zadanie 5 — Co pokaże program? (średnie)

Najpierw zapisz wynik **bez uruchamiania kodu**:

```kotlin
fun podwoj(liczba: Int): Int {
    return liczba * 2
}

fun main() {
    val wynik = podwoj(3)
    println(wynik)
    println(podwoj(wynik))
}
```

Dopiero później możesz sprawdzić przewidywanie w edytorze.

### Zadanie 6 — Znajdź i popraw błędy (średnie)

Kod poniżej celowo nie działa poprawnie. Popraw go tak, aby wypisał `Wynik: 9`:

```kotlin
fun dodaj(a: Int, b: Int): Int {
    println(a + b)
}

fun main() {
    val wynik = dodaj(4)
    println("Wynik: $wynik")
}
```

Wskazówka: sprawdź **liczbę argumentów** i to, czy funkcja **oddaje** wynik.

### Zadanie 7 — Wiek i warunek (średnie)

Napisz funkcję `czySenior(wiek: Int): Boolean`, która zwraca `true` dla wieku **co najmniej 65 lat**. W `main()` użyj `if` z wywołaniem tej funkcji i wyświetl jeden z dwóch komunikatów. Sprawdź działanie dla wartości `64` oraz `65`.

### Zadanie 8 — Projekt zaliczeniowy: koszyk zakupowy (trudniejsze)

Napisz program, który pobiera od użytkownika **cenę jednego produktu** oraz **liczbę sztuk**, a następnie wyświetla rachunek.

Wymagania:

1. Funkcja `pokazNaglowek()` wyświetla nazwę programu.
2. Funkcja `obliczKoszt(cena: Int, ilosc: Int): Long` **zwraca** koszt wszystkich produktów.
3. Funkcja `pokazPodsumowanie(ilosc: Int, koszt: Long, waluta: String = "zł")` wyświetla wynik. Użyj argumentu domyślnego.
4. W `main()` pobierz dwie liczby za pomocą `readln().toIntOrNull()`.
5. Przy złym wpisie, ujemnej cenie lub liczbie sztuk mniejszej lub równej zero wyświetl prosty komunikat o błędzie.
6. Gdy dane są poprawne, wywołaj obie funkcje i wyświetl rachunek.
7. Użyj funkcji ponownie z nazwanym argumentem `waluta = "PLN"`, aby pokazać drugą wersję komunikatu.

**Przykład:** cena `8`, liczba sztuk `3` → koszt `24 zł`. Twoja wersja może mieć własne komunikaty. Najważniejszy jest **podział na funkcje**, nie wygląd ramki.

## 18. Mini-test — sprawdź zrozumienie

Odpowiedz własnymi słowami, nie patrząc do odpowiedzi:

1. Co to jest **funkcja** i po co jej używamy?
2. Czy samo napisanie `fun przywitaj() { ... }` spowoduje wyświetlenie wiadomości?
3. Czym różni się **parametr** od **argumentu**?
4. Co oznacza `: Int` po nawiasie funkcji?
5. Czym różnią się `println(a + b)` oraz `return a + b`?
6. Co stanie się z dalszymi instrukcjami wewnątrz funkcji po wykonaniu `return`?
7. Co oznacza `Unit` jako typ wyniku?
8. Do czego służy zapis `imie: String = "Gość"`?
9. Dlaczego wywołanie `wyswietl(imie = "Lena", wiek = 20)` może być czytelniejsze niż `wyswietl("Lena", 20)`?
10. Czy `main()` może użyć wprost zmiennej `cena` zadeklarowanej wewnątrz innej funkcji? Jak zwykle przekazujemy taki wynik?

### Kiedy przejść do lekcji 7?

Możesz przejść dalej, jeśli potrafisz bez przepisywania gotowego kodu:

- napisać własną funkcję z **dwoma parametrami**;
- wywołać ją z `main()`;
- obliczyć i **zwrócić** wynik przez `return`;
- zastosować **wartość domyślną parametru**;
- wyjaśnić, skąd bierze się błąd, gdy próbujesz użyć zmiennej z innej funkcji.

Jeśli któryś punkt sprawia trudność, wróć do przykładów z sekcji 5–13 i zmień w nich własnoręcznie dane.

## 19. Podsumowanie

- `fun` pozwala **zdefiniować funkcję**.
- Kod funkcji wykona się, gdy funkcja zostanie **wywołana**.
- Parametr określa **jakie dane** funkcja przyjmuje, a argument to **konkretna wartość** przekazana przy wywołaniu.
- `return` **zwraca wynik** i kończy wykonywanie bieżącej funkcji.
- Funkcja z `: Int`, `: String` albo `: Boolean` powinna zwrócić wynik odpowiedniego typu.
- Funkcja wykonująca czynność bez użytecznego wyniku zwykle zwraca `Unit`.
- Wartości domyślne i argumenty nazwane ułatwiają korzystanie z funkcji.
- Zmienna utworzona wewnątrz funkcji jest zazwyczaj widoczna **tylko w niej**.

**Najważniejsza myśl:** dobra funkcja robi jedną zrozumiałą rzecz. Gdy program jest podzielony na małe funkcje, łatwiej go czytać, poprawiać i rozwijać.

---

## 20. Odpowiedzi i przykładowe rozwiązania

> Ta część służy do **sprawdzenia swojej pracy**. Możesz mieć inne, równie poprawne rozwiązanie. Nie ucz się kodu na pamięć — sprawdź, czy rozumiesz rolę funkcji i przepływ danych.

### Zadanie 1 — powitanie

```kotlin-run
fun przywitaj() {
    println("Witaj na kursie Kotlina!")
}

fun main() {
    przywitaj()
    przywitaj()
}
```

### Zadanie 2 — parametr tekstowy

```kotlin-run
fun powitajOsobe(imie: String) {
    println("Miło Cię widzieć, $imie!")
}

fun main() {
    powitajOsobe("Ola")
    powitajOsobe("Marek")
}
```

### Zadanie 3 — pole prostokąta

```kotlin-run
fun poleProstokata(szerokosc: Int, wysokosc: Int): Int {
    return szerokosc * wysokosc
}

fun main() {
    val pole = poleProstokata(4, 6)
    println(pole)
}
```

Wynik: `24`. Zwracany wynik można zapisać w `val` i wykorzystać w kolejnych obliczeniach.

### Zadanie 4 — parametr domyślny

```kotlin-run
fun pokazPowitanie(imie: String, slowo: String = "Cześć") {
    println("$slowo, $imie!")
}

fun main() {
    pokazPowitanie("Ola")
    pokazPowitanie("Marek", "Witaj")
}
```

### Zadanie 5 — przewidź wynik

Poprawne przewidywanie:

```text
6
12
```

Wywołanie `podwoj(3)` zwraca `6`. Następnie `podwoj(wynik)` to `podwoj(6)`, więc wynik wynosi `12`.

### Zadanie 6 — poprawiony program

```kotlin-run
fun dodaj(a: Int, b: Int): Int {
    return a + b
}

fun main() {
    val wynik = dodaj(4, 5)
    println("Wynik: $wynik")
}
```

**Poprawiliśmy dwa błędy:** funkcja powinna zwrócić `Int`, a wywołanie `dodaj()` wymaga dwóch argumentów.

### Zadanie 7 — rozpoznawanie wieku

```kotlin-run
fun czySenior(wiek: Int): Boolean {
    return wiek >= 65
}

fun main() {
    val wiek = 65

    if (czySenior(wiek)) {
        println("Osoba jest seniorem według przyjętej granicy.")
    } else {
        println("Osoba nie osiągnęła jeszcze przyjętej granicy wieku.")
    }
}
```

Zmień `wiek` na `64`, aby zobaczyć drugą gałąź warunku. Granica 65 lat jest tutaj **założeniem zadania**, a nie ogólną definicją.

### Zadanie 8 — projekt: koszyk zakupowy

```kotlin-run
fun pokazNaglowek() {
    println("===== KOSZYK ZAKUPOWY =====")
}

fun obliczKoszt(cena: Int, ilosc: Int): Long {
    return cena.toLong() * ilosc
}

fun pokazPodsumowanie(ilosc: Int, koszt: Long, waluta: String = "zł") {
    println("Liczba produktów: $ilosc")
    println("Do zapłaty: $koszt $waluta")
}

fun main() {
    pokazNaglowek()

    print("Podaj cenę jednego produktu: ")
    val cena = readln().toIntOrNull()

    print("Podaj liczbę sztuk: ")
    val ilosc = readln().toIntOrNull()

    if (cena == null || cena < 0 || ilosc == null || ilosc <= 0) {
        println("Błąd: cena musi być nieujemna, a liczba sztuk dodatnia.")
    } else {
        val koszt = obliczKoszt(cena, ilosc)
        pokazPodsumowanie(ilosc, koszt)
        pokazPodsumowanie(koszt = koszt, ilosc = ilosc, waluta = "PLN")
    }
}
```

Przykładowy wynik dla ceny `8` i liczby `3`:

```text
===== KOSZYK ZAKUPOWY =====
Podaj cenę jednego produktu: 8
Podaj liczbę sztuk: 3
Liczba produktów: 3
Do zapłaty: 24 zł
Liczba produktów: 3
Do zapłaty: 24 PLN
```

W drugim wywołaniu `pokazPodsumowanie()` celowo użyliśmy **nazwanych argumentów** i podaliśmy walutę `PLN`. Dzięki temu jedna funkcja potrafi przygotować dwa warianty tego samego podsumowania.

### Mini-test — odpowiedzi

1. Funkcja to nazwany fragment kodu do wykonania konkretnego zadania; pozwala korzystać z tego samego kodu wiele razy.
2. Nie. Trzeba wywołać funkcję, np. `przywitaj()` z `main()`.
3. Parametr jest nazwą i typem danych w definicji funkcji; argument to wartość podana w wywołaniu.
4. Funkcja ma zwrócić wartość typu `Int` (liczbę całkowitą).
5. `println()` wypisuje wartość na ekran; `return` oddaje wynik wywołującemu kodowi.
6. Nie są wykonywane, ponieważ `return` kończy bieżącą funkcję.
7. Oznacza, że funkcja nie zwraca użytecznego wyniku do dalszych obliczeń; na przykład może tylko coś wyświetlać.
8. Jest to wartość domyślna parametru, używana wtedy, gdy przy wywołaniu pominiemy argument `imie`.
9. Dzięki nazwom widać, która wartość trafia do którego parametru; można też zmienić kolejność nazwanych argumentów.
10. Nie. Zmienna lokalna należy do funkcji, w której ją zadeklarowano. Zwykle przekazuje się dane przez **parametry i `return`**.

---

## Dodatkowe źródła (dla chętnych)

- [Kotlin — funkcje](https://kotlinlang.org/docs/functions.html)
- [Kotlin — podstawowa składnia](https://kotlinlang.org/docs/basic-syntax.html)
- [Kotlin — zakresy widoczności i deklaracje](https://kotlinlang.org/docs/visibility-modifiers.html) — na później, gdy pojawią się klasy.

**Koniec lekcji 6/36. Następna: kolekcje — listy, tablice, mapy i zbiory.**