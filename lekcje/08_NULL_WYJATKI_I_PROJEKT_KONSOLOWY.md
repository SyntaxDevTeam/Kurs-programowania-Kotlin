# Lekcja 8 — Null, wyjątki, debugowanie i projekt konsolowy

**Poziom:** I — Fundamenty Kotlina  
**Lekcja:** 8 z 8 w Poziomie I (8 z 36 w całym kursie)  
**Temat:** brak wartości, typy z `?`, bezpieczne wywołania `?.`, operator `?:`, wyjątki, `try/catch`, debugowanie i projekt końcowy  
**Czas:** około 120–150 minut (z ćwiczeniami i projektem zaliczeniowym; możesz podzielić lekcję na dwa spotkania)  
**Potrzebujesz:** IntelliJ IDEA, działający projekt Kotlin/JVM lub edytor kursu; IntelliJ IDEA do ćwiczenia z debuggerem  
**Wymagana wiedza:** lekcje 1–7: zmienne, typy danych, operatory, warunki, pętle, funkcje i kolekcje

---

> To ostatnia lekcja pierwszego poziomu. Nie musisz jeszcze znać klas, obiektów ani API Minecrafta. Z tych zagadnień będziemy korzystać później.
>
> Pełne programy i rozwiązania oznaczone są jako `kotlin-run`. Fragmenty do samodzielnej analizy i celowo błędny kod mają znacznik `kotlin`. Odpowiedzi znajdziesz na końcu. Każdy program uruchamiaj osobno.
>
> Przykłady z `readlnOrNull()` wymagają standardowego wejścia. Jeśli interpreter na stronie nie pozwala wpisywać danych, uruchom je w IntelliJ IDEA. Debugger również poznajemy w IntelliJ IDEA.

## 1. Co zrobić, gdy program nie otrzyma tego, czego oczekuje?

Dotychczas często korzystaliśmy z gotowych liczb i tekstów. Ale użytkownik może wpisać słowo zamiast liczby, wyszukać nieistniejący wpis lub zakończyć wprowadzanie danych.

Program nie powinien zakładać, że wszystko zawsze pójdzie zgodnie z planem. Powinien umieć odróżnić prawidłowy wynik od braku wyniku i odpowiednio zareagować na problem.

Dzisiaj nauczysz się:

- rozumieć, czym jest `null` i czym różni się od zera oraz pustego tekstu;
- oznaczać typy, które dopuszczają brak wartości;
- używać `?.` i `?:` zamiast ryzykownego wymuszania dostępu do wartości;
- rozpoznawać błędy składni, wyjątki podczas działania i błędy logiczne;
- przechwytywać konkretne wyjątki za pomocą `try/catch`;
- sprawdzać działanie programu krok po kroku w debuggerze;
- stworzyć własny rejestr graczy, korzystając z dotychczasowych umiejętności.

Nie chodzi o to, żeby program nigdy nie napotkał problemu. Chodzi o to, aby potrafił reagować na przewidywalne sytuacje, a Ty potrafiła odnaleźć przyczynę pozostałych.

## 2. Null — informacja o braku wartości

Przypomnij sobie mapy z lekcji 7. Jeśli spróbujesz odczytać wartość spod klucza, który nie istnieje, otrzymasz `null`.

```kotlin-run
fun main() {
    val ceny = mapOf("chleb" to 5, "mleko" to 4)

    println(ceny["chleb"])
    println(ceny["jogurt"])
}
```

Wynik:

```text
5
null
```

`null` oznacza tutaj brak wartości. Nie jest zerem, pustym tekstem ani napisem "null".

```kotlin-run
fun main() {
    val brak: String? = null
    val pusty: String = ""
    val napis: String = "null"
    val zero: Int = 0

    println(brak == null)
    println(pusty.isEmpty())
    println(napis == "null")
    println(zero == 0)
}
```

Wynik:

```text
true
true
true
true
```

Każda z tych wartości oznacza coś innego:

- `null` — nie ma wartości.
- `""` — tekst istnieje, ale nie zawiera żadnych znaków.
- `"null"` — zwykły tekst składający się z czterech liter.
- `0` — rzeczywista liczba.

Jeżeli w rejestrze ktoś ma zero punktów, to nie znaczy, że nie istnieje. Pamiętaj o tej różnicy, gdy w dalszej części będziemy wyszukiwać graczy.

## 3. Znak zapytania przy typie — String? oraz Int?

Kotlin rozróżnia typy niedopuszczające braku wartości i typy, które go dopuszczają.

| Zapis | Znaczenie |
|---|---|
| `String` | Musi zawierać tekst. |
| `String?` | Może zawierać tekst albo `null`. |
| `Int` | Musi zawierać liczbę całkowitą. |
| `Int?` | Może zawierać liczbę całkowitą albo `null`. |

```kotlin-run
fun main() {
    var imie: String? = null

    println(imie)

    imie = "Ola"
    println(imie)
}
```

Wynik:

```text
null
Ola
```

Znak zapytania zapisujemy **po nazwie typu**, a nie po nazwie zmiennej. Nie oznacza on, że Kotlin ma zgadywać typ. Mówi, że wartość może być nieobecna.

Poniższy fragment jest celowo błędny:

```kotlin
val imie: String = null
```

Typ `String` nie dopuszcza `null`. Kotlin nie pozwoli skompilować takiego kodu.

Nie dodawaj znaku zapytania do każdego typu na wszelki wypadek. Jeśli informacja jest obowiązkowa i zawsze musi istnieć, używaj zwykłego typu.

## 4. Sprawdzanie, czy wartość istnieje

Jednym z najprostszych sposobów pracy z wartością, której może brakować, jest znany już warunek `if`.

```kotlin-run
fun main() {
    val imie: String? = "Ola"

    if (imie != null) {
        println("Długość imienia: ${imie.length}")
    } else {
        println("Nie podano imienia.")
    }
}
```

Wynik:

```text
Długość imienia: 3
```

Zmień przypisanie na `null`. Otrzymasz komunikat „Nie podano imienia”.

Kotlin zauważa, że wewnątrz pierwszego bloku sprawdziliśmy, czy ta lokalna wartość istnieje. Pozwala nam wtedy odczytać jej długość bez dodatkowego zabezpieczenia. Nazywa się to **smart cast**, czyli inteligentnym rozpoznawaniem typu po sprawdzeniu. Na razie wystarczy wiedzieć, że działa w takich prostych przypadkach, w których kompilator może mieć pewność, że wartość nie zmieni się między sprawdzeniem a użyciem.

Takiego sprawdzenia potrzebujemy zwłaszcza wtedy, gdy dla braku wartości chcemy wykonać zupełnie inne instrukcje.

## 5. Bezpieczne wywołanie — operator ?.

Często wystarczy wykonać jakąś operację tylko wtedy, gdy wartość istnieje. Służy do tego **bezpieczne wywołanie**: znak zapytania i kropka, czyli `?.`.

```kotlin-run
fun main() {
    val imie: String? = "Ola"
    val brak: String? = null

    println(imie?.length)
    println(brak?.length)
}
```

Wynik:

```text
3
null
```

W pierwszej instrukcji Kotlin odczytuje długość imienia. W drugiej pomija odczyt długości i zwraca `null`. Nie próbuje wyliczyć długości nieistniejącego tekstu.

Wartość uzyskana w ten sposób może być typu `Int?`, ponieważ wynikiem może być zarówno liczba, jak i brak wartości.

Bezpieczne wywołanie można stosować również do funkcji:

```kotlin-run
fun main() {
    val tekst: String? = "  kot  "
    val brak: String? = null

    println(tekst?.trim()?.uppercase())
    println(brak?.trim()?.uppercase())
}
```

Wynik:

```text
KOT
null
```

Funkcja `trim()` usuwa białe znaki z początku i końca tekstu, a `uppercase()` tworzy tekst wielkimi literami. Jeżeli na początku nie ma wartości, cały pokazany łańcuch bezpiecznych wywołań daje `null`.

**Zapamiętaj:** `?.` nie zamienia braku wartości na zero ani pusty napis. Nadal możemy otrzymać `null`.

## 6. Operator Elvisa — wartość zastępcza przez ?:

Czasami chcemy pokazać użytkownikowi przyjazny komunikat zamiast napisu null. Kotlin pozwala użyć operatora `?:`, nazywanego operatorem Elvisa.

```kotlin-run
fun main() {
    val imie: String? = null
    val powitanie = imie ?: "Gość"

    println("Witaj, $powitanie!")
}
```

Wynik:

```text
Witaj, Gość!
```

Operator działa następująco: jeżeli wartość po lewej stronie nie jest `null`, wykorzystaj ją. W przeciwnym razie użyj wartości po prawej stronie.

Zmień `null` na `"Ola"` i uruchom ponownie. Program wyświetli „Witaj, Ola!”.

### Łączymy ?. z ?:

```kotlin-run
fun main() {
    val imie: String? = null
    val dlugosc = imie?.length ?: 0

    println("Długość: $dlugosc")
}
```

Wynik:

```text
Długość: 0
```

Najpierw `?.` próbuje odczytać długość tekstu, jeśli tekst istnieje. Potem `?:` zastępuje brak wyniku zerem. Zmienna `dlugosc` ma tutaj typ `Int`, a nie `Int?`.

W tym przykładzie przyjęliśmy umowną zasadę, że brak imienia daje długość zero. W innym programie może to być niewłaściwe.

### Zero i pusty napis nie uruchamiają operatora Elvisa

```kotlin-run
fun main() {
    val punkty: Int? = 0
    val tekst: String? = ""

    println(punkty ?: 10)
    println("Tekst: [${tekst ?: "brak"}]")
}
```

Wynik:

```text
0
Tekst: []
```

Operator Elvisa reaguje tylko na `null`. Zero i pusty tekst są prawidłowymi wartościami, dlatego nie zostaną zastąpione.

### A co oznacza !!?

W kodzie możesz spotkać również dwa wykrzykniki. Zapis `imie!!.length` oznacza: zapewniam, że imię nie jest nullem. Jeżeli zapewnienie jest nieprawdziwe, program zgłosi wyjątek.

Na tym etapie unikaj `!!`. Lepiej sprawdzić wartość przez `if`, użyć `?.` lub świadomie wybrać wartość zastępczą.

## 7. Bezpieczne pobieranie danych — wracamy do konwersji

W lekcji 3 poznaliśmy `toIntOrNull()`. Teraz już wiesz, dlaczego jej wynik może mieć typ `Int?`.

```kotlin-run
fun main() {
    val poprawna = "25".toIntOrNull()
    val bledna = "kot".toIntOrNull()

    println(poprawna)
    println(bledna)
}
```

Wynik:

```text
25
null
```

Pamiętaj, że konwersja może się nie udać także wtedy, gdy wpisana liczba przekracza zakres typu `Int`.

Można też pobierać tekst przez `readlnOrNull()`. W przeciwieństwie do `readln()` ta funkcja zwraca `null`, gdy wejście się zakończyło i nie ma kolejnego wiersza. Samo naciśnięcie Enter bez wpisywania tekstu daje pusty napis, a nie `null`.

```kotlin-run
fun main() {
    print("Podaj liczbę punktów: ")
    val punkty = readlnOrNull()?.trim()?.toIntOrNull()

    if (punkty == null) {
        println("Nie podano poprawnej liczby całkowitej.")
    } else if (punkty < 0) {
        println("Punkty nie mogą być ujemne.")
    } else {
        println("Zapisano $punkty punktów.")
    }
}
```

Przykładowe uruchomienia (każdy przypadek osobno):

| Wpis | Odpowiedź |
|---|---|
| `25` | Zapisano 25 punktów. |
| `0` | Zapisano 0 punktów. |
| `-2` | Punkty nie mogą być ujemne. |
| `kot` | Nie podano poprawnej liczby całkowitej. |
| Pusty wiersz | Nie podano poprawnej liczby całkowitej. |

Prześledźmy kod. Najpierw pobieramy tekst. Jeżeli wejście się zakończyło, bezpieczne wywołania pomijają pozostałe operacje. W innym przypadku usuwamy zbędne spacje na końcach i próbujemy zamienić tekst na liczbę.

Na końcu sprawdzamy nie tylko to, czy konwersja się udała, ale też czy liczba ma sens w naszym programie. Minus dwa jest prawidłową liczbą całkowitą, lecz nie chcemy dopuszczać ujemnych punktów.

## 8. Trzy rodzaje problemów w programie

Nie każdy błąd oznacza to samo. Najpierw rozpoznaj, z jaką sytuacją masz do czynienia.

| Rodzaj | Co się dzieje? | Przykład |
|---|---|---|
| Błąd kompilacji | Kod nie może zostać poprawnie skompilowany. | Przypisanie `null` do typu `String`. |
| Błąd podczas działania | Program się uruchamia, ale napotyka problem w czasie wykonywania. | Próba zamiany napisu „kot” na liczbę przez `toInt()`. |
| Błąd logiczny | Program działa, lecz daje niewłaściwy wynik. | Odejmowanie zamiast dodawania. |

**Wyjątek** (*exception*) to informacja o nieprawidłowej sytuacji podczas wykonywania programu. Jeżeli nie zostanie obsłużony, może przerwać działanie programu.

Nie wszystkie problemy są wyjątkami. Kompilator może zatrzymać niepoprawny kod jeszcze przed uruchomieniem, a program z błędem logicznym może zakończyć się całkowicie normalnie.

## 9. Pierwszy try/catch

W poprzednim przykładzie używaliśmy funkcji `toIntOrNull()`. Zwykła funkcja `toInt()` przy niepoprawnym tekście zgłasza `NumberFormatException`.

Poniższy program **celowo zgłosi wyjątek**. To materiał do analizy, nie gotowy przykład do automatycznego uruchomienia:

```kotlin
fun main() {
    val liczba = "kot".toInt()
    println(liczba)
}
```

Kotlin nie potrafi zamienić słowa „kot” na liczbę. Instrukcja `println()` nie zostanie już wykonana, ponieważ wcześniej wystąpił nieobsłużony wyjątek.

Możemy przechwycić taki wyjątek:

```kotlin-run
fun main() {
    try {
        val liczba = "kot".toInt()
        println("Liczba: $liczba")
    } catch (e: NumberFormatException) {
        println("Nie udało się odczytać liczby.")
    }

    println("Program działa dalej.")
}
```

Wynik:

```text
Nie udało się odczytać liczby.
Program działa dalej.
```

- `try` oznacza fragment, który chcemy wykonać.
- `catch` wskazuje, co zrobić w razie wystąpienia określonego wyjątku.
- `e` jest nazwą przechwyconego wyjątku. Możemy odczytać np. `e.message`, czyli jego komunikat.
- `NumberFormatException` określa rodzaj problemu, który obsługujemy.

Jeżeli zamienisz tekst „kot” na „25”, instrukcje w bloku `try` wykonają się poprawnie, a `catch` zostanie pominięty. W obu przypadkach program wyświetli ostatni komunikat.

**Praktyczna zasada:** jeżeli zwykła niepoprawna wartość jest spodziewaną sytuacją, często wygodniej skorzystać z `toIntOrNull()`. `try/catch` jest potrzebne między innymi wtedy, gdy używana funkcja sygnalizuje problem właśnie przez wyjątek.

## 10. Różne wyjątki, różne reakcje

W lekcji 3 wspominaliśmy o dzieleniu całkowitym przez zero. Na platformie JVM powoduje ono wyjątek `ArithmeticException`. Nie dotyczy to w ten sam sposób dzielenia liczb zmiennoprzecinkowych.

```kotlin-run
fun podziel(a: Int, b: Int): Int {
    return a / b
}

fun main() {
    try {
        println(podziel(12, 0))
    } catch (e: ArithmeticException) {
        println("Nie można dzielić liczby całkowitej przez zero.")
    }

    println(podziel(12, 3))
}
```

Wynik:

```text
Nie można dzielić liczby całkowitej przez zero.
4
```

Jeśli można łatwo sprawdzić dzielnik przed wykonaniem działania, dobrym rozwiązaniem jest zwykły warunek. Wyjątek pokazujemy tutaj, żeby nauczyć się rozpoznawać takie sytuacje.

### Więcej niż jeden catch

```kotlin-run
fun main() {
    val tekst = "12"
    val dzielnik = 0

    try {
        val liczba = tekst.toInt()
        println(liczba / dzielnik)
    } catch (e: NumberFormatException) {
        println("Niepoprawny zapis liczby.")
    } catch (e: ArithmeticException) {
        println("Niepoprawne dzielenie.")
    }
}
```

Wynik:

```text
Niepoprawne dzielenie.
```

Najpierw zamieniamy tekst na liczbę. Udało się, lecz potem wystąpił problem z dzieleniem. Wykonał się drugi `catch`.

Spróbuj zmienić `tekst` na `"kot"`. Wtedy wykona się pierwszy `catch`, a do dzielenia w ogóle nie dojdzie.

Nie przechwytuj wszystkich wyjątków tylko po to, by je ignorować. Obsługuj te, dla których znasz sensowną reakcję. Pozostałe problemy powinny być możliwe do zdiagnozowania.

## 11. Try może zwracać wartość, a finally pomaga zakończyć pracę

Podobnie jak poznane wcześniej `if`, konstrukcja `try/catch` może dać wynik.

```kotlin-run
fun main() {
    val tekst = "kot"

    val wynik: Int? = try {
        tekst.toInt()
    } catch (e: NumberFormatException) {
        null
    }

    println(wynik ?: "Brak poprawnej liczby")
}
```

Wynik:

```text
Brak poprawnej liczby
```

Wartością jest ostatnie wyrażenie w bloku, który zakończył się poprawnie. Przy prawidłowym tekście otrzymamy liczbę, a przy błędnym — `null`.

Kotlin ma też opcjonalny blok `finally`. W normalnym przebiegu wykonywania konstrukcji `try` zostaje wykonany niezależnie od tego, czy próba się udała, czy pojawił się wyjątek.

```kotlin-run
fun main() {
    try {
        println("Rozpoczynam sprawdzanie.")
        println("kot".toInt())
    } catch (e: NumberFormatException) {
        println("Błąd danych.")
    } finally {
        println("Zakończono sprawdzanie.")
    }
}
```

Wynik:

```text
Rozpoczynam sprawdzanie.
Błąd danych.
Zakończono sprawdzanie.
```

Gdy zamienisz „kot” na „25”, zamiast komunikatu o błędzie zobaczysz liczbę. Ostatni komunikat nadal się pojawi. W większych programach `finally` przydaje się np. do porządkowania zasobów. Dzisiaj nie musisz używać go w każdym programie.

## 12. Debugowanie — jak znaleźć przyczynę błędu?

**Debugowanie** to szukanie przyczyn niewłaściwego działania programu. Najprostszy sposób to sprawdzenie, jakie wartości pojawiają się w ważnych miejscach.

Mamy program, który powinien obliczyć cenę trzech zeszytów po osiem złotych. Poniższy kod jest celowo niepoprawny logicznie. Nie uruchamiaj go od razu — najpierw spróbuj odnaleźć problem.

```kotlin
fun main() {
    val cena = 8
    val sztuki = 3
    val razem = cena + sztuki

    println("Do zapłaty: $razem zł")
}
```

Program się kompiluje, ale wyświetli 11 zł zamiast 24 zł. Nie powinniśmy dodawać ceny i liczby sztuk, tylko je mnożyć.

Poprawmy kod i dodajmy pomocnicze komunikaty:

```kotlin-run
fun main() {
    val cena = 8
    val sztuki = 3
    println("DEBUG: cena = $cena, sztuki = $sztuki")

    val razem = cena * sztuki
    println("Do zapłaty: $razem zł")
}
```

Wynik:

```text
DEBUG: cena = 8, sztuki = 3
Do zapłaty: 24 zł
```

Tymczasowy komunikat pozwala sprawdzić dane przed obliczeniem. Gdy już go nie potrzebujesz, usuń go.

### Debugger w IntelliJ IDEA — zatrzymujemy program

Debugger jest narzędziem pozwalającym zatrzymać program na wybranej instrukcji, sprawdzić zmienne i wykonać kolejne linie.

Użyj tego samego poprawionego programu z ceną i liczbą sztuk.

1. Otwórz go w `Main.kt`.
2. Kliknij na lewym marginesie edytora, obok linii `val razem = cena * sztuki`. Pojawi się czerwona kropka — **breakpoint**, czyli punkt zatrzymania.
3. Uruchom program opcją **Debug** zamiast **Run** (możesz użyć menu obok funkcji `main()`).
4. Program zatrzyma się przed wykonaniem zaznaczonej instrukcji. W panelu zmiennych sprawdź `cena` i `sztuki`. Powinny wynosić osiem i trzy.
5. Wybierz **Step Over**, aby wykonać bieżącą instrukcję. Sprawdź, czy `razem` otrzymało wartość 24.
6. Wybierz **Resume Program**, aby kontynuować działanie.

Wygląd przycisków może zależeć od wersji IntelliJ IDEA. W edytorze przeglądarkowym wystarczy ćwiczenie z komunikatem pomocniczym, jeśli nie ma debuggera.

### Jak czytać komunikat wyjątku?

Jeżeli program ulegnie awarii, w konsoli może pojawić się np. nazwa `java.lang.NumberFormatException` wraz z opisem oraz listą miejsc wywołań funkcji. Tę listę nazywamy **stack trace**, czyli śladem stosu.

Poszukaj w nim pierwszego miejsca odnoszącego się do Twojego kodu, np. wpisu zawierającego `Main.kt` i numer linii. Sprawdź, jakie dane trafiły do wskazanej funkcji. Nie musisz od razu rozumieć wszystkich wpisów dotyczących bibliotek.

Dobra kolejność pracy:

1. Odtwórz problem na tych samych danych.
2. Przeczytaj pierwszy istotny komunikat.
3. Sprawdź wartość zmiennych w podejrzanym miejscu.
4. Popraw jedną rzecz i uruchom program ponownie.
5. Sprawdź również dane, które wcześniej działały poprawnie.

## 13. Projekt podsumowujący — konsolowy rejestr graczy

W pierwszym poziomie nauczyliśmy się już wystarczająco dużo, żeby zbudować mały, użyteczny program. Stworzymy rejestr graczy, w którym przechowujemy nick i liczbę punktów.

**To zwykły program konsolowy, nie plugin.** Nie łączy się z serwerem Minecraft i nie pobiera rzeczywistych danych. Na początku rejestr jest pusty. Dane przechowujemy w pamięci i po zakończeniu programu zostaną utracone. Zapis do pliku pojawi się w kolejnych lekcjach.

### Co program ma robić?

| Opcja | Działanie |
|---|---|
| 1 | Dodać nowego gracza. |
| 2 | Pokazać wszystkich graczy. |
| 3 | Wyszukać gracza po nicku. |
| 4 | Ustawić nową liczbę punktów istniejącego gracza. |
| 5 | Usunąć gracza. |
| 0 | Zakończyć program. |

Zasady:

- Nick po usunięciu spacji z początku i końca nie może być pusty.
- W tym uproszczonym rejestrze nicki rozróżniają wielkość liter.
- Nie dodajemy drugi raz tego samego nicku.
- Punkty muszą być poprawną liczbą całkowitą od zera wzwyż, mieszczącą się w typie `Int`.
- Nie można aktualizować punktów ani usuwać gracza, którego nie ma.
- Niepoprawne dane powodują komunikat i powrót do menu, a nie awarię.
- Zakończenie wejścia również pozwala wyjść z programu.

### Krok 1. Wybieramy strukturę danych

Potrzebujemy modyfikowalnej mapy o kluczach tekstowych i wartościach całkowitych:

```kotlin
val gracze = mutableMapOf<String, Int>()
```

Klucz to nick, wartość to liczba punktów. Mapa nie przechowuje dwóch osobnych wpisów o tym samym kluczu. Przy dodawaniu dodatkowo sprawdzimy, czy nick już istnieje, aby nie zastąpić przez przypadek punktów istniejącego gracza.

### Krok 2. Dzielimy program na funkcje

Funkcja `main()` będzie odpowiadała za menu, mapę i pętlę. Oddzielne funkcje obsłużą dodawanie, wyświetlanie, wyszukiwanie, aktualizację i usuwanie. Dwie niewielkie funkcje pomocnicze pobiorą poprawny nick i punkty.

Zastosujemy również wczesny powrót z funkcji: `?: return`. Oznacza on, że jeśli po lewej stronie nie ma wartości, kończymy bieżącą funkcję. Wcześniej funkcja pomocnicza wyjaśni użytkownikowi, co było nie tak. Funkcja `main()` może następnie pokazać menu ponownie.

### Krok 3. Kompletny program

```kotlin-run
fun pobierzNick(): String? {
    print("Podaj nick: ")
    val nick = readlnOrNull()?.trim()

    if (nick == null || nick.isEmpty()) {
        println("Nick nie może być pusty.")
        return null
    }

    return nick
}

fun pobierzPunkty(): Int? {
    print("Podaj liczbę punktów: ")
    val punkty = readlnOrNull()?.trim()?.toIntOrNull()

    if (punkty == null || punkty < 0) {
        println("Podaj poprawną liczbę całkowitą od zera wzwyż.")
        return null
    }

    return punkty
}

fun dodajGracza(gracze: MutableMap<String, Int>) {
    val nick = pobierzNick() ?: return

    if (gracze.containsKey(nick)) {
        println("Ten gracz jest już zapisany.")
        return
    }

    val punkty = pobierzPunkty() ?: return
    gracze[nick] = punkty
    println("Dodano: $nick — $punkty pkt.")
}

fun pokazGraczy(gracze: Map<String, Int>) {
    if (gracze.isEmpty()) {
        println("Rejestr jest pusty.")
        return
    }

    for (wpis in gracze) {
        println("${wpis.key} — ${wpis.value} pkt.")
    }

    println("Liczba graczy: ${gracze.size}")
}

fun znajdzGracza(gracze: Map<String, Int>) {
    val nick = pobierzNick() ?: return
    val punkty = gracze[nick]

    if (punkty == null) {
        println("Nie znaleziono gracza.")
    } else {
        println("Znaleziono: $nick — $punkty pkt.")
    }
}

fun aktualizujPunkty(gracze: MutableMap<String, Int>) {
    val nick = pobierzNick() ?: return

    if (!gracze.containsKey(nick)) {
        println("Nie znaleziono gracza.")
        return
    }

    val punkty = pobierzPunkty() ?: return
    gracze[nick] = punkty
    println("Zaktualizowano: $nick — $punkty pkt.")
}

fun usunGracza(gracze: MutableMap<String, Int>) {
    val nick = pobierzNick() ?: return

    if (gracze.remove(nick) == null) {
        println("Nie znaleziono gracza.")
    } else {
        println("Usunięto: $nick")
    }
}

fun pokazMenu() {
    println()
    println("===== REJESTR GRACZY =====")
    println("1. Dodaj gracza")
    println("2. Pokaż graczy")
    println("3. Wyszukaj gracza")
    println("4. Zmień punkty")
    println("5. Usuń gracza")
    println("0. Zakończ")
    print("Wybierz opcję: ")
}

fun main() {
    val gracze = mutableMapOf<String, Int>()

    while (true) {
        pokazMenu()
        val opcja = readlnOrNull()?.trim() ?: break

        when (opcja) {
            "1" -> dodajGracza(gracze)
            "2" -> pokazGraczy(gracze)
            "3" -> znajdzGracza(gracze)
            "4" -> aktualizujPunkty(gracze)
            "5" -> usunGracza(gracze)
            "0" -> break
            else -> println("Nieznana opcja.")
        }
    }

    println("Do zobaczenia!")
}
```

Nie musisz rozumieć całego programu w pierwszej sekundzie. Przejdź po kolei przez funkcje, a następnie uruchom go i poeksperymentuj z menu.

### Krok 4. Prześledźmy prawidłowe działanie

Wpisz opcję 1, nick Alex i liczbę 0. Następnie dodaj Maję z 15 punktami (w programie wpisz nick `Maja`). Wybierz opcję 2, aby pokazać rejestr.

Pomiędzy kolejnymi operacjami pojawia się menu. Najważniejsze komunikaty z tego uruchomienia to:

```text
Dodano: Alex — 0 pkt.
Dodano: Maja — 15 pkt.
Alex — 0 pkt.
Maja — 15 pkt.
Liczba graczy: 2
```

Wybierz opcję 3 i wpisz Alex. Program powinien pokazać:

```text
Znaleziono: Alex — 0 pkt.
```

Zero punktów nie oznacza braku gracza. Mapa zwraca `null` tylko przy braku klucza, ponieważ nasz typ wartości to `Int`, bez znaku zapytania.

### Krok 5. Sprawdź trudniejsze sytuacje

Przeprowadź poniższe próby w tym samym uruchomieniu po dodaniu Alexa i Mai.

| Próba | Oczekiwany rezultat |
|---|---|
| 1, Alex | „Ten gracz jest już zapisany.” |
| 1, pusty nick | „Nick nie może być pusty.” |
| 1, Lena, kot | Komunikat o niepoprawnej liczbie; Lena nie zostaje dodana. |
| 1, Lena, -5 | Komunikat o niepoprawnej liczbie; Lena nie zostaje dodana. |
| 3, Lena | „Nie znaleziono gracza.” |
| 4, Alex, 20 | Punkty Alexa zmienią się na 20. |
| 5, Maja | Maja zostanie usunięta. |
| 5, Maja | „Nie znaleziono gracza.” |
| 2 | Zostanie Alex z 20 punktami; liczba graczy wyniesie 1. |
| 9 | „Nieznana opcja.” |
| 0 | „Do zobaczenia!” i koniec programu. |

Możesz też sprawdzić bardzo długi zapis liczbowy, np. `999999999999999`. Konwersja do `Int` się nie uda, więc program nie zmieni rejestru.

### Co wykorzystaliśmy z poprzednich lekcji?

| Lekcja | Wykorzystanie w projekcie |
|---|---|
| 1 | `main()`, `print()` i `println()`. |
| 2 | `val`, `String`, `Int` i szablony tekstu. |
| 3 | Porównania, konwersje i dane użytkownika. |
| 4 | `if`, `when` i operatory logiczne. |
| 5 | Pętla menu `while`, `break` i `for` do wypisywania graczy. |
| 6 | Parametry, typy zwracane, `return` i podział na funkcje. |
| 7 | `Map`, `MutableMap`, klucze, wartości, wyszukiwanie i modyfikacja. |
| 8 | `null`, bezpieczne wywołania, operator Elvisa i rozpoznawanie błędów. |

W głównym projekcie celowo nie dodajemy `try/catch` na siłę. Błędny wpis liczby obsługujemy bezpieczną konwersją `toIntOrNull()`. Wyjątki ćwiczyliśmy we wcześniejszych programach. Dobry kod wykorzystuje mechanizm właściwy dla danego problemu, a nie wszystkie poznane naraz.

## 14. Typowe błędy początkujących

| Objaw | Co sprawdzić? |
|---|---|
| Kompilator nie pozwala odczytać `length` | Czy typ może być `null`? Użyj sprawdzenia lub `?.`. |
| Wartość zastępcza nie pojawia się przy zerze | `?:` reaguje wyłącznie na `null`. |
| `NumberFormatException` | Czy tekst rzeczywiście jest liczbą w dopuszczalnym zakresie? |
| `ArithmeticException` przy dzieleniu całkowitym | Czy dzielnik nie jest zerem? |
| `catch` nie reaguje na wyjątek | Czy przechwytujesz właściwy rodzaj wyjątku? |
| Program wyświetla złą kwotę, ale nie zgłasza błędu | Sprawdź działania i wartości pośrednie. |
| Rejestr pozwala dodać ujemne punkty | Sama poprawna konwersja nie sprawdza zasad programu. |
| Punkty równe zero oznaczają brak gracza | Sprawdzaj `null`, a nie porównanie z zerem. |

## 15. Ćwiczenia do samodzielnego wykonania

Najpierw spróbuj samodzielnie. Wszystkie zadania są osobnymi programami. Rozwiązania są dopiero na końcu.

### Zadanie 1 — Brak imienia (łatwe)

Utwórz wartość `imie: String?` zawierającą `null`. Za pomocą `if` wypisz „Nie podano imienia” lub zapisane imię. Sprawdź również wariant z imieniem Ola.

### Zadanie 2 — Bezpieczna długość (łatwe)

Utwórz wartość `tekst: String?` z napisem „Kotlin”. Odczytaj długość przez `?.`, używając `?:` z wartością zastępczą minus jeden. Sprawdź również `null` i pusty napis.

### Zadanie 3 — Przewidź wynik (łatwe)

Zapisz wyniki bez uruchamiania programu.

```kotlin
fun main() {
    val a: Int? = 0
    val b: Int? = null
    val c: String? = ""

    println(a ?: 7)
    println(b ?: 7)
    println(c?.length ?: -1)
}
```

### Zadanie 4 — Poprawne dane (średnie)

Poproś o wiek użytkownika. Użyj `readlnOrNull()`, `trim()` i `toIntOrNull()`. Wyświetl wiek tylko wtedy, gdy konwersja się uda i wartość nie jest ujemna. Przetestuj 18, zero, minus jeden oraz napis „kot”.

### Zadanie 5 — Przechwyć wyjątek (średnie)

Spróbuj zamienić napis „12x” na `Int` przez `toInt()`. Przechwyć odpowiedni wyjątek i wyświetl „Niepoprawna liczba”. Po `try/catch` wypisz „Koniec próby”. Sprawdź również napis „12”.

### Zadanie 6 — Napraw program (średnie)

Poniższy program powinien pokazać długość tekstu lub zero, jeśli go nie ma. Popraw kod.

```kotlin
fun main() {
    val tekst: String? = null
    println(tekst.length)
}
```

### Zadanie 7 — Znajdź błąd logiczny (średnie)

Program ma obliczyć średnią dwóch liczb. Oczekujemy 7.5. Znajdź przyczynę niewłaściwego wyniku, popraw ją i sprawdź zmienne za pomocą komunikatów pomocniczych lub debuggera.

```kotlin
fun main() {
    val a = 7
    val b = 8
    val srednia = (a + b) / 2

    println(srednia)
}
```

### Zadanie 8 — Rozbudowa rejestru graczy (zaliczeniowe)

Po samodzielnym przeanalizowaniu projektu z sekcji 13 dodaj opcję 6 — wyświetlenie łącznej liczby punktów wszystkich graczy.

Wymagania:

1. Dodaj osobną funkcję przyjmującą `Map<String, Int>`.
2. Przejdź przez wszystkie wartości mapy i oblicz ich sumę. Użyj typu `Long`, aby nie przekroczyć zakresu `Int` przy sumowaniu dużych wyników.
3. Wyświetl komunikat z sumą również wtedy, gdy rejestr jest pusty.
4. Dodaj opcję do menu i obsłuż ją w `when`.
5. Sprawdź, czy suma zmienia się po dodaniu, aktualizacji i usunięciu gracza.

Jeśli dodasz Alexa z 20 punktami i Maję z 15, suma ma wynosić 35. Zmiana punktów Alexa na 5 powinna dać 20. Usunięcie Mai pozostawia 5 punktów.

## 16. Mini-test — czy rozumiesz materiał?

1. Czy `null` oznacza zero lub pusty napis?
2. Jaka jest różnica między `String` a `String?`?
3. Co zwraca `?.length`, jeśli tekst jest nullem?
4. Kiedy wykonywana jest prawa strona `?:`?
5. Co zwraca `"abc".toIntOrNull()`?
6. Czy naciśnięcie Enter bez tekstu zawsze oznacza `null`?
7. Do czego służy `catch`?
8. Co odróżnia błąd logiczny od błędu kompilacji?
9. Do czego służy breakpoint?
10. Dlaczego w rejestrze wyszukujemy brak gracza przez porównanie z `null`, a nie z zerem?

## 17. Zaliczenie pierwszego poziomu

Przed przejściem do programowania obiektowego sprawdź, czy potrafisz samodzielnie:

- utworzyć projekt i uruchomić własny program;
- wybrać `val` lub `var` i odpowiedni typ danych;
- pobrać tekst, bezpiecznie zamienić go na liczbę i sprawdzić wynik;
- napisać warunek oraz pętlę, która kończy się wtedy, kiedy powinna;
- wydzielić funkcję przyjmującą dane i zwracającą wynik;
- wykorzystać listę, tablicę, zbiór lub mapę odpowiednio do zadania;
- wyjaśnić, czym jest `null` i jak uniknąć błędnego dostępu do wartości;
- odnaleźć prosty błąd w działającym programie;
- uruchomić rejestr graczy, wyjaśnić jego działanie oraz samodzielnie dodać opcję sumowania punktów.

Nie musisz znać na pamięć nazw wszystkich metod. Korzystanie z notatek i dokumentacji jest normalne. Najważniejsze, żeby rozumieć, dlaczego wybrana konstrukcja działa i móc ją zastosować do nowego problemu.

Jeżeli projekt jest jeszcze za trudny, wróć do odpowiedniego fragmentu kursu. Funkcje są w lekcji 6, mapy w lekcji 7, a pętle w lekcji 5.

---

## 18. Odpowiedzi i przykładowe rozwiązania

### Zadanie 1 — brak imienia

```kotlin-run
fun main() {
    val imie: String? = null

    if (imie == null) {
        println("Nie podano imienia")
    } else {
        println(imie)
    }
}
```

Wynik: Nie podano imienia. Dla `"Ola"` otrzymamy Ola.

### Zadanie 2 — bezpieczna długość

```kotlin-run
fun main() {
    val tekst: String? = "Kotlin"
    println(tekst?.length ?: -1)
}
```

Wynik: 6. Dla `null` wynik to minus jeden, a dla pustego napisu zero.

### Zadanie 3 — przewidywanie

```text
0
7
0
```

Zero i pusty napis nie są nullem. Tylko druga instrukcja używa wartości zastępczej.

### Zadanie 4 — poprawny wiek

```kotlin-run
fun main() {
    print("Podaj wiek: ")
    val wiek = readlnOrNull()?.trim()?.toIntOrNull()

    if (wiek == null || wiek < 0) {
        println("Niepoprawny wiek.")
    } else {
        println("Wiek: $wiek")
    }
}
```

Wpis 18 daje „Wiek: 18”. Zero jest tutaj poprawne, natomiast minus jeden, pusty tekst i „kot” nie przejdą sprawdzenia.

### Zadanie 5 — wyjątek

```kotlin-run
fun main() {
    try {
        val liczba = "12x".toInt()
        println(liczba)
    } catch (e: NumberFormatException) {
        println("Niepoprawna liczba")
    }

    println("Koniec próby")
}
```

Wynik:

```text
Niepoprawna liczba
Koniec próby
```

Po zmianie napisu na `"12"` pierwszym wynikiem będzie liczba 12, a drugi komunikat nadal się pojawi.

### Zadanie 6 — poprawiony dostęp do wartości

```kotlin-run
fun main() {
    val tekst: String? = null
    println(tekst?.length ?: 0)
}
```

Wynik: 0. Bezpieczne wywołanie daje null, a operator Elvisa wybiera wartość zastępczą.

### Zadanie 7 — średnia

```kotlin-run
fun main() {
    val a = 7
    val b = 8
    val suma = a + b

    println("DEBUG: suma = $suma")

    val srednia = suma / 2.0
    println(srednia)
}
```

Wynik:

```text
DEBUG: suma = 15
7.5
```

Dzieliliśmy wcześniej dwie liczby całkowite, więc część ułamkowa została odrzucona. Zapis `2.0` pozwala otrzymać wynik typu `Double`.

### Zadanie 8 — sumowanie punktów

Aby nie powielać całego projektu, poniższy samodzielny program zawiera tę samą funkcję sumującą i mały test jej działania.

```kotlin-run
fun pokazSumePunktow(gracze: Map<String, Int>) {
    var suma = 0L

    for (punkty in gracze.values) {
        suma += punkty
    }

    println("Łączna liczba punktów: $suma")
}

fun main() {
    val gracze = mutableMapOf<String, Int>()
    pokazSumePunktow(gracze)

    gracze["Alex"] = 20
    gracze["Maja"] = 15
    pokazSumePunktow(gracze)

    gracze["Alex"] = 5
    pokazSumePunktow(gracze)

    gracze.remove("Maja")
    pokazSumePunktow(gracze)
}
```

Wynik:

```text
Łączna liczba punktów: 0
Łączna liczba punktów: 35
Łączna liczba punktów: 20
Łączna liczba punktów: 5
```

Aby włączyć tę funkcję do projektu z sekcji 13:

- skopiuj tylko definicję `pokazSumePunktow()`, nie drugą funkcję `main()`;
- do funkcji `pokazMenu()` dopisz `println("6. Pokaż sumę punktów")` przed opcją 0;
- do `when (opcja)` dodaj `"6" -> pokazSumePunktow(gracze)` przed `else`.

`gracze.values` udostępnia wszystkie wartości mapy. Litera `L` w `0L` oznacza, że suma ma typ `Long`. Funkcja nie modyfikuje rejestru, więc wystarczy jej parametr `Map<String, Int>`.

### Mini-test — odpowiedzi

1. Nie, oznacza brak wartości.
2. `String` nie dopuszcza null, a `String?` może zawierać tekst lub null.
3. `null`.
4. Tylko wtedy, gdy lewa strona daje null.
5. `null`.
6. Nie. Pusty wiersz jest pustym tekstem; zakończenie wejścia powoduje, że `readlnOrNull()` zwraca null.
7. Do obsługi wyjątku określonego rodzaju.
8. Kod z błędem kompilacji nie zostanie poprawnie skompilowany; błąd logiczny może dawać niewłaściwy wynik bez awarii.
9. Do zatrzymania programu w wybranym miejscu, aby sprawdzić stan i wykonywać kolejne instrukcje.
10. Ponieważ istniejący gracz może mieć zero punktów. Null oznacza brak wpisu w naszej mapie.

---

## Dodatkowe źródła (dla chętnych)

- [Kotlin — Null safety](https://kotlinlang.org/docs/null-safety.html)
- [Kotlin — Exceptions](https://kotlinlang.org/docs/exceptions.html)
- [Kotlin — Map operations](https://kotlinlang.org/docs/map-operations.html)
- [IntelliJ IDEA — Debugging](https://www.jetbrains.com/help/idea/debugging-code.html)

**Gratulacje — to koniec Poziomu I: Fundamenty Kotlina (lekcje 1–8).**

Wiesz już, jak pisać samodzielne programy konsolowe. Kolejny poziom poświęcimy programowaniu obiektowemu, które pozwoli nam tworzyć własne rodzaje danych i porządkować większe projekty.

**Następna: lekcja 9/36 — Klasy, obiekty i modele danych.**
