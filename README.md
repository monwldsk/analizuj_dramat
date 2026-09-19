# Cyfrowa analiza dramatu

Narzędzie do ilościowej i eksploracyjnej analizy **dramatu wierszowanego napisanego w języku polskim**.

Projekt pozwala badać zróżnicowanie wersyfikacyjne dramatu, strukturę dialogu, udział poszczególnych postaci, didaskalia oraz wybrane cechy składniowe i leksykalne tekstu. Analiza wykonywana jest bezpośrednio w przeglądarce.

Narzędzie zostało pomyślane przede wszystkim jako pomoc w **eksploracji tekstu i formułowaniu hipotez interpretacyjnych**. Wyniki wskaźników należy traktować jako operacyjne przybliżenia, a nie jako automatyczną analizę literaturoznawczą lub pełny parser języka polskiego.

## Demo

Wersja działająca online dostępna jest przez GitHub Pages:

[monwldsk.github.io/analizuj_dramat/](https://monwldsk.github.io/analizuj_dramat/)

---

## Najważniejsze możliwości

Narzędzie umożliwia:

- analizę tekstu wklejonego bezpośrednio do aplikacji;
- wczytywanie własnych plików `.txt` z dysku lub metodą drag & drop;
- wybór tekstu z dołączonej biblioteki dramatów oraz wyszukiwanie po autorze lub tytule;
- automatyczne przypisywanie wersów do mówiących postaci;
- rozpoznawanie didaskaliów;
- przybliżone liczenie sylab;
- wykrywanie wersów współdzielonych przez dwie postaci;
- analizę dominujących formatów sylabicznych;
- obliczanie wskaźników wersyfikacyjnych, dialogowych, składniowych, interpunkcyjnych i leksykalnych;
- wizualizację wyników na wykresach;
- analizę udziału poszczególnych bohaterów w tekście dialogowym;
- porównywanie struktury wersyfikacyjnej wypowiedzi różnych bohaterów;
- tabelaryczne porównanie do 8 bohaterów według dodatkowych wskaźników stylistycznych i składniowych;
- eksport tabeli analitycznej do XLSX;
- porównywanie dowolnej liczby dramatów według wybranego wskaźnika;
- śledzenie zmian wybranych wskaźników w kolejnych 20-procentowych odcinkach dramatu;
- eksperymentalne porównanie wyników parsera TXT z analizą strukturalną TEI/XML (**BETA**).

---

# Jak działa analiza?

## 1. Parsowanie dramatu

Tekst dzielony jest na linie. Parser próbuje następnie określić funkcję każdej z nich.

W uproszczeniu:

```text

PUSTELNIK

Kto jestem?… jeszcze rano… powiedzieć nie mogę.

Idę z daleka, nie wiem, z piekła czyli z raju,

A dążę do tegoż kraju.

Tymczasem małą dam tobie przestrogę.

KSIĄDZ

/ na stronie /

Trzeba z nim, widzę, innego sposobu.

```

### Nazwy postaci

Linia zapisana wielkimi literami jest interpretowana jako oznaczenie mówiącej postaci.

Przykład:

```text

KONRAD

GUSTAW

PUSTELNIK

```

Od tego miejsca kolejne wersy zostają przypisane wykrytej postaci aż do pojawienia się kolejnej nazwy.

### Didaskalia

Linia zawierająca znak `/` traktowana jest jako didaskalium.

Przykład:

```text

/ na stronie /

```

Tak rozpoznana linia otrzymuje kategorię:

```text

DIDASKALIA

```

### Pomijane linie

Parser ignoruje:

- puste linie;

- linie rozpoczynające się od `++`.

---

# Wersy współdzielone

Narzędzie obsługuje sytuację, w której jeden wers metryczny jest podzielony pomiędzy dwie postaci.

Jeżeli druga część wersu posiada wyraźne wcięcie, parser może potraktować obie linie jako części tego samego wersu:

```text

DZIECI

/ czytają /

„Onego czasu…”

KSIĄDZ

                        Kto tam? kto tam stuka?

```

W implementacji druga część jest rozpoznawana jako współdzielona przy wcięciu odpowiadającym co najmniej ok. 20 spacjom. Tabulator liczony jest jako większe wcięcie.

Fragmenty oznaczane są jako:

```text

P_1

P_2

```

i otrzymują ten sam numer wersu.

Didaskalium umieszczone pomiędzy `P_1` i `P_2` nie przerywa potencjalnego wersu współdzielonego.

Na potrzeby analizy metrycznej `P_1` i `P_2` są następnie scalane w jeden rzeczywisty wers, a liczby sylab obu fragmentów są sumowane.

Na potrzeby analizy dialogu zachowywane są jednak jako osobne fragmenty przypisane właściwym bohaterom.

---

# Liczenie sylab

Liczba sylab jest szacowana na podstawie samogłosek.

Algorytm uwzględnia wybrane charakterystyczne dla polszczyzny połączenia literowe, m.in.:

```text

ie

ia

iu

ię

ią

io

ii

iy

ió

```

które są na potrzeby obliczenia traktowane jako jeden ośrodek sylabiczny (głoska "i" nie pełni w większości takich przypadków funkcji sylabotwórczej).

Następnie liczona jest liczba wystąpień samogłosek:

```text

a e i o u ą ę ó y

```

## Ważne ograniczenie

Nie jest to pełny analizator fonologiczny ani słownikowy sylabifikator języka polskiego.

Algorytm może pomylić się m.in. przy:

- nietypowych połączeniach samogłosek;

- granicach morfemów (wtedy ciąg np. "ia" będzie tworzył 2 sylaby);

- niektórych zapożyczeniach;

- dyftongach (np. au czy oi - jak w wyrazach "pauza" czy "sinusoida");

- nazwach własnych;

- wyjątkach fonetycznych.

Z tego względu wyniki należy traktować jako **przybliżenie odpowiednie przede wszystkim dla analiz statystycznych większych tekstów**.

---

# Dostępne wskaźniki

## Zróżnicowanie metryczne

Narzędzie oblicza rozkład wersów według liczby sylab.

Pozwala to określić m.in.:

- dominujące formaty sylabiczne całego dramatu;

- dominujące formaty w wypowiedziach poszczególnych bohaterów;

- różnice wersyfikacyjne pomiędzy postaciami.

Formaty występujące w < 1% wersów są grupowane w zbiorczą kategorię.

---

## Średnia długość odcinka izosylabicznego

Odcinek izosylabiczny jest rozumiany jako ciąg kolejnych rzeczywistych wersów metrycznych posiadających tę samą liczbę sylab.

Obliczana jest:

```text

średnia długość =

suma długości odcinków izosylabicznych / liczba odcinków

```

Dodatkowo wyznaczany jest wskaźnik częstości zmiany liczby sylab pomiędzy kolejnymi wersami.

Może on służyć jako przybliżony wskaźnik stabilności lub zmienności organizacji rytmicznej tekstu.

---

## Udział wypowiedzi bohaterów

Narzędzie oblicza procent wszystkich wersów przypadających na poszczególne postaci.

W przypadku zwykłego wersu całość przypisywana jest jednemu bohaterowi.

Jeżeli wers jest współdzielony przez kilka postaci, jego udział jest rozdzielany między nie **proporcjonalnie do liczby sylab wypowiedzianych przez każdą postać**.

Bohaterowie posiadający mniej niż 1% udziału mogą zostać zgrupowani w kategorię zbiorczą.

---

## Średnia długość wypowiedzi jednej postaci

Wypowiedź jest definiowana jako ciąg kolejnych wersów przypisanych temu samemu bohaterowi.

Wskaźnik:

```text

liczba wersów wypowiedzi /

liczba bloków wypowiedzi

```

może służyć jako proste przybliżenie dynamiki interakcji dialogowej.

Krótsze bloki mogą wskazywać na częstsze zmiany mówiącego, dłuższe — na bardziej rozbudowane wypowiedzi lub partie monologowe.

---

## Średnia długość zdania

Narzędzie próbuje automatycznie podzielić wypowiedzi bohaterów na zdania.

Uwzględniane są m.in.:

- `.`

- `...`

- `…`

- `?`

- `!`

Serie znaków, np.:

```text

?!

!!!

...

```

traktowane są jako jedno zakończenie zdania.

Parser posiada również prostą obsługę:

- typowych skrótów;

- inicjałów;

- liczb dziesiętnych;

- kropki występującej wewnątrz zdania.

Dla wykrytych zdań obliczana jest:

- średnia liczba słów;

- średnia przybliżona liczba sylab;

- liczba wykrytych zdań.

Didaskalia są wyłączane z tej analizy.

---

## Zdania hipotaktyczne

Narzędzie wykorzystuje algorytm oparty na obecności językowych sygnałów podporządkowania składniowego.

Do dość niezawodnych markerów należą m.in.:

```text

że

iż

żeby

aby

ażeby

ponieważ

gdyż

bo

jeśli

jeżeli

gdyby

choć

chociaż

skoro

zanim

dopóki

odkąd

gdy

```

Uwzględniane są również bardziej kontekstowe formy, np.:

```text

czy

kiedy

gdzie

dokąd

skąd

dlaczego

kto

co

który

która

które

```

Dla markerów wieloznacznych analizowane jest dodatkowo ich położenie względem granic składniowych.

Wynikiem jest:

- liczba zdań zakwalifikowanych jako hipotaktyczne;

- ich procentowy udział wśród wszystkich wykrytych zdań.

**Nie jest to zatem pełny parser składniowy.**

---

## Jawne relacje kauzalne i wynikowe

Narzędzie mierzy odsetek zdań zawierających jawne leksykalne wykładniki relacji przyczynowo-skutkowej, uzasadnienia lub wnioskowania.

Uwzględniane są m.in.:

```text

bo

bowiem

albowiem

ponieważ

gdyż

jako że

dlatego że

więc

zatem

toteż

przeto

dlatego

tedy

w rezultacie

w konsekwencji

z tego powodu

w takim razie

tak że

```

Jedno zdanie liczone jest najwyżej jeden raz niezależnie od liczby markerów.

Ze względu na wieloznaczność części wyrażeń wynik powinien być traktowany jako wskaźnik eksploracyjny.

---

## Niezgodność granicy wersowej i syntagmatycznej

Dla każdego rzeczywistego wersu sprawdzane jest jego zakończenie.

Za sygnały zamknięcia uznawane są m.in.:

```text

.

,

!

?

;

…

:

-

—

–

```

Wskaźnik określa procent wersów, które **nie kończą się takim znakiem interpunkcyjnym**.

W przybliżeniu może wskazywać stopień rozbieżności pomiędzy segmentacją wersową i składniową.

Nie należy utożsamiać go automatycznie z liczbą przerzutni. Rozpoznanie przerzutni wymaga analizy składniowej i semantycznej relacji pomiędzy sąsiadującymi wersami.

---

## Wektory ekspresji interpunkcyjnej

Obliczane są:

```text

gęstość wykrzyknień =

liczba znaków ! / liczba wersów metrycznych × 100

```

oraz:

```text

gęstość pytań =

liczba znaków ? / liczba wersów metrycznych × 100

```

Didaskalia nie są wliczane do długości dramatu używanej w mianowniku ułamka.

---

## Referencja pierwszoosobowa

Program oblicza frekwencję wybranych form zaimkowych pierwszej osoby.

### Liczba pojedyncza

Uwzględniane są m.in.:

```text

ja, mnie, mi, mną,

mój, mojego, mojemu, moim,

moja, mojej, moją,

moje, moi, moich, moimi

```

Wynik podawany jest jako liczba wystąpień na 1000 słów.

### Liczba mnoga

Analogicznie analizowane są m.in.:

```text

my, nas, nam, nami,

nasz, naszego, naszemu, naszym,

nasza, naszej, naszą,

nasze, nasi, naszych, naszymi

```

Wskaźniki rejestrują zatem jedynie określone jawne formy leksykalne.

---

# Didaskalia i wypowiedzi „na stronie”

Program oblicza trzy dodatkowe wskaźniki.

### Gęstość metakomentarza

```text

liczba didaskaliów /

liczba wersów metrycznych × 100

```

### Nasycenie didaskaliów wypowiedziami „na stronie”

```text

liczba didaskaliów zawierających „na stronie” /

liczba wszystkich didaskaliów × 100

```

### Gęstość apartu względem długości dramatu

```text

liczba didaskaliów zawierających „na stronie” /

liczba wersów metrycznych × 100

```

---

# Analiza postaci

Po przeanalizowaniu dramatu można dodatkowo badać rozkład i właściwości wypowiedzi poszczególnych bohaterów.

Dostępne są:

- wykres procentowego udziału wypowiedzi wszystkich bohaterów w całym tekście dialogowym;
- wykresy kołowe rozkładu formatów sylabicznych dla wybranych bohaterów;
- możliwość połączenia kilku bohaterów w jedną grupę na potrzeby analizy metrycznej;
- wspólny wykres słupkowy porównujący formaty sylabiczne wielu postaci;
- tabelaryczne porównanie **maksymalnie 8 bohaterów** według dodatkowych wskaźników.

W tabeli porównawczej bohaterów uwzględniane są:

- średnia długość wypowiedzi;
- gęstość pytań i wykrzyknień na 100 wersów;
- niezgodność granicy wersowej i syntagmatycznej;
- referencja pierwszoosobowa w liczbie pojedynczej i mnogiej na 1000 słów;
- udział zdań hipotaktycznych;
- udział zdań z jawnym wykładnikiem relacji kauzalnej lub wynikowej;
- średnia liczba słów w zdaniu;
- średnia liczba sylab w zdaniu.

Dla składni, długości zdań i referencji pierwszoosobowej analizowane są wyłącznie wypowiedzi wybranego bohatera. W przypadku wersów współdzielonych `P_1 + P_2` wskaźnik granicy wersowej odnosi się do całego zrekonstruowanego wersu.

---

# Analiza porównawcza dramatów

Osobny moduł pozwala porównać większą liczbę tekstów na wspólnym wykresie.

Można:

1. wybrać dramaty z biblioteki;
2. dodać własne pliki `.txt`;
3. skorzystać ze skrótu zaznaczającego przygotowany zestaw dramatów Juliusza Słowackiego;
4. zmienić nazwy pozycji;
5. ustawić ich kolejność na wykresie;
6. wybrać jeden z dostępnych wskaźników;
7. wygenerować wspólny wykres porównawczy.

Porównywać można:

- średnią długość odcinka izosylabicznego;
- średnią długość wypowiedzi;
- średnią długość zdania w słowach;
- średnią długość zdania w sylabach;
- odsetek zdań hipotaktycznych;
- udział zdań z jawnym wykładnikiem relacji kauzalnej lub wynikowej;
- niezgodność granicy wersowej i syntagmatycznej;
- gęstość wykrzyknień;
- gęstość pytań;
- referencję pierwszoosobową w liczbie pojedynczej;
- referencję pierwszoosobową w liczbie mnogiej;
- gęstość metakomentarza (didaskaliów);
- **liczbę wystąpień apartu** (`„na stronie”`);
- nasycenie didaskaliów wypowiedziami „na stronie”;
- gęstość apartu względem długości dramatu.

Biblioteczne teksty są buforowane w pamięci przeglądarki podczas porównania, aby nie pobierać ich ponownie dla każdego obliczenia.

## Zmiany wskaźnika w toku dramatu

Dla wybranych wskaźników moduł porównawczy generuje również dwa wykresy przebiegu wartości w toku dramatu. Tekst każdego dramatu jest dzielony na pięć kolejnych odcinków:

```text
0–20%
20–40%
40–60%
60–80%
80–100%
```

Wartość wskaźnika jest obliczana osobno dla każdego odcinka. Wers współdzielony `P_1 + P_2` pozostaje zawsze w jednym segmencie i nie jest rozcinany między dwa przedziały.

Położenie w dramacie może być wyznaczane na dwa sposoby:

- **według liczby sylab** — na podstawie skumulowanej liczby sylab w rzeczywistych wersach wypowiedzi;
- **według liczby wersów** — na podstawie kolejności rzeczywistych wersów, tak aby każdy dramat dochodził do 100% niezależnie od długości.

Analiza przebiegu jest obecnie dostępna dla: liczby wystąpień apartu, średniej długości odcinka izosylabicznego, średniej długości wypowiedzi, obu miar długości zdania, hipotaksy, relacji kauzalnych/wynikowych, gęstości pytań i wykrzyknień, obu wskaźników referencji pierwszoosobowej oraz gęstości metakomentarza. Nie jest generowana dla wskaźnika granicy wersowej ani dwóch względnych wskaźników apartu.

---

# Porównanie TXT z TEI/XML — BETA

Aplikacja zawiera eksperymentalny moduł kontrolny pozwalający porównać analizę tego samego dramatu wykonaną dwiema metodami:

- przez dotychczasowy parser pliku `.txt`, oparty na konwencjach typograficznych;
- przez osobny parser **TEI/XML**, wykorzystujący strukturę dokumentu.

Parser TEI/XML korzysta m.in. z:

```text
<sp>
<speaker>
<l>
<stage>
@who
<particDesc>
```

Sekcja BETA pozwala użyć domyślnej pary plików porównawczych lub wskazać własny plik TXT i XML na czas bieżącej sesji. Przed uruchomieniem analizy pokazuje podgląd obu źródeł.

Wynik porównania obejmuje:

- tabelę kontrolną podstawowych cech parsowania;
- porównanie udziału wersów przypadających na postaci;
- porównanie głównych wskaźników analitycznych i różnic `XML − TXT`.

W tabeli udziałów parser próbuje mapować etykiety wykryte w TXT do encji wskazanych w TEI przez `@who`, dzięki czemu różne etykiety odnoszące się do tej samej postaci mogą zostać zestawione jako jedna encja. Moduł TEI/XML nie zastępuje jeszcze głównego przepływu analizy TXT i ma status **BETA**.

---

# Format własnego pliku

Najlepiej używać zwykłych plików UTF-8 `.txt`.

Minimalny przykład:

```text

DZIECI

Jezus, Maryja!

KSIĄDZ

                        Któż to jest na progu?

/ zmieszany /

Ktoś ty taki?… po co?… na co?

DZIECI

Ach, trup, trup! upiór, ladaco!

W imię Ojca!… zgiń, przepadaj!

KSIĄDZ

Ktoś ty, bracie? odpowiadaj.

```

Najważniejsze zasady:

1. nazwa bohatera powinna znajdować się w osobnej linii i być zapisana WIELKIMI LITERAMI;

2. każda linia wypowiedzi powinna odpowiadać jednemu wersowi lub fragmentowi wersu;

3. didaskalia powinny zawierać `/`;

4. należy usunąć elementy pozatekstowe, takie jak metadane wydania, ISBN, informacje techniczne czy spis treści;

5. nagłówki zapisane wielkimi literami mogą zostać błędnie rozpoznane jako nazwy postaci;

6. sposób przygotowania tekstu ma wpływ na wyniki.

Parser jest szczególnie dobrze dopasowany do konwencji spotykanej w tekstowych eksportach utworów z serwisu Wolne Lektury.

---

# Wyniki i eksport

Podstawowym wynikiem analizy jest tabela:

\| Pole | Znaczenie |

\|---|---|

\| Nr wersu | numer rzeczywistego wersu |

\| Treść wersu | tekst analizowanej linii |

\| Liczba sylab | wynik liczenia sylab na podstawie wykrycia samogłosek w pozycji zgłoskotwórczej |

\| Bohater | rozpoznana postać lub `DIDASKALIA` |

\| Wers współdzielony | oznaczenie `P_1` / `P_2` |

Tabelę można wyeksportować do pliku:

```text

wyniki.xlsx

```

Eksport generowany jest bezpośrednio w przeglądarce.

---

# Biblioteka

Biblioteka ma przede wszystkim charakter demonstracyjny i zawiera teksty przygotowane pod wymagania parsera. Są to głównie pliki `.txt` oparte na zasobach Wolnych Lektur, uzupełnione również o utwory spoza tego zbioru.

Pozycje można filtrować na bieżąco przez wyszukiwarkę działającą po **autorze lub tytule**. Wybrany tekst jest ładowany do aplikacji i analizowany tym samym mechanizmem co tekst wklejony lub własny plik `.txt`.

---

# Uruchomienie lokalne

Projekt nie wymaga budowania aplikacji, instalowania frameworków JavaScript ani uruchamiania backendu.

### 1. Pobierz repozytorium

Za pomocą GitHub CLI:

```bash

gh repo clone monwldsk/analizuj_dramat

cd analizuj_dramat

```

### 2. Uruchom prosty serwer HTTP

Przykładowo:

```bash

python3 -m http.server 8000

```

Następnie otwórz w przeglądarce serwer lokalny na porcie `8000`.

Uruchomienie przez serwer HTTP jest zalecane, ponieważ biblioteczne pliki tekstowe ładowane są przez `fetch()`. Bezpośrednie otwarcie `index.html` z systemu plików może spowodować problemy z dostępem do lokalnych zasobów.

---

# Architektura

Projekt jest celowo prostą aplikacją typu static site.

```text
analizuj_dramat/
├── index.html
├── index_dev.html
├── README.md
└── assets/
    ├── biblioteka/
    ├── comparison_tei_vs_txt/
    ├── card_icons/
    ├── charts/
    └── ...
```

Większość logiki aplikacji znajduje się obecnie bezpośrednio w `index.html`:

- HTML interfejsu;
- CSS;
- parser dramatu TXT;
- eksperymentalny parser i moduł porównawczy TEI/XML;
- algorytm liczenia sylab;
- obliczanie wskaźników;
- analiza i porównywanie bohaterów;
- obsługa wykresów;
- analiza porównawcza wielu dramatów;
- analiza zmian wskaźników w toku dramatu;
- eksport danych.

---

# Technologie

Projekt wykorzystuje:

- HTML5;
- CSS3;
- JavaScript (bez frameworków);
- Chart.js — generowanie wykresów;
- SheetJS / XLSX — eksport wyników do arkusza;
- natywny `DOMParser` przeglądarki — odczyt dokumentów TEI/XML w module BETA.

Obliczenia dotyczące wczytanego dramatu wykonywane są po stronie klienta (w przeglądarce użytkownika).

Nie jest wymagany backend ani baza danych.

---

# Ograniczenia metodologiczne

Narzędzie wykorzystuje szereg uproszczeń.

Rezultat zależy m.in. od sposobu przygotowania źródła.

Szczególnie istotne są następujące ograniczenia:

### Parser struktury dramatu

Struktura rekonstruowana jest z formatowania zwykłego pliku tekstowego.

Nie istnieje semantyczne oznaczenie typu:

```xml

<speaker>

<stage>

<l>

<sp>

```

jak w TEI/XML.

Wielkie litery, ukośniki i wcięcia pełnią więc funkcję zastępczych znaczników strukturalnych.

### Liczenie sylab

Algorytm nie uwzględnia wszystkich zjawisk fonetycznych polszczyzny.

### Analiza składniowa

Klasyfikacja zdań hipotaktycznych wykorzystuje markery leksykalne i interpunkcyjne. Nie wykonuje pełnego parsowania zależności składniowych.

### Relacje kauzalne

Obecność określonego spójnika lub wyrażenia nie musi w każdym kontekście oznaczać relacji przyczynowej.

### Granica wersu

Brak interpunkcji na końcu wersu nie jest równoznaczny z wystąpieniem przerzutni.

---

### TEI/XML — moduł BETA

Obsługa TEI/XML ma obecnie charakter kontrolny i eksperymentalny. Parser oczekuje dokumentu w przestrzeni nazw TEI i wykorzystuje określony podzbiór struktury dramatu (`<sp>`, `<speaker>`, `<l>`, `<stage>`, `@who`, opisy osób i grup). Nie należy traktować go jeszcze jako pełnej obsługi wszystkich wariantów kodowania TEI ani jako zamiennika głównego parsera TXT.

# Jak interpretować wyniki?

Najbardziej użyteczny model pracy z narzędziem to:

```text

tekst

  ↓

pomiar

  ↓

różnica / anomalia

  ↓

hipoteza

  ↓

close reading

```

Wynik ilościowy powinien być punktem wyjścia do ponownej analizy tekstu.

Przykładowo wysoka zmienność metryczna może prowadzić do pytania:

> W których scenach dochodzi do zmiany formatu wiersza i czy zmiana ta koreluje ze zmianą postaci, sytuacji komunikacyjnej albo funkcji wypowiedzi?

Takie pytanie wymaga następnie powrotu do konkretnego fragmentu dramatu.

---

# TXT jako format podstawowy i TEI/XML jako kierunek rozwoju

TXT pozostaje podstawowym formatem wejściowym aplikacji, ponieważ zapewnia niski próg wejścia: użytkownik może wkleić tekst albo otworzyć prosty plik `.txt` bez wcześniejszego znakowania strukturalnego.

W konsekwencji główny parser musi rekonstruować strukturę dramatu na podstawie konwencji typograficznych, co nie daje 100% skuteczności.

Równolegle projekt zawiera już **eksperymentalny parser TEI/XML (BETA)** wykorzystywany w sekcji porównawczej TXT ↔ TEI/XML. W takim formacie mówca, wypowiedź, wers, didaskalium oraz inne elementy mogą być zapisane strukturalnie zamiast odgadywane z formatowania.

Obecny moduł TEI/XML służy przede wszystkim do kontroli różnic pomiędzy podejściem heurystycznym i strukturalnym. Pełne włączenie TEI/XML do głównego przepływu analizy pozostaje dalszym etapem rozwoju.

---

# Możliwe kierunki rozwoju

Najważniejsze potencjalne rozszerzenia:

- pełna integracja TEI/XML z głównym przepływem analizy, wykraczająca poza moduł porównawczy BETA;
- raport jakości parsowania tekstu;
- walidacja skuteczności algorytmu liczenia formatu wierszowego na ręcznie opisanym korpusie;
- eksport wszystkich wskaźników do CSV/XLSX/JSON;
- eksport wyników analizy porównawczej i przebiegów wskaźników;
- podział kodu na moduły;
- automatyczne testy jednostkowe parserów i obliczeń wskaźników;
- większy i bardziej zróżnicowany korpus dramatów;
- pełniejsza analiza składniowa;
- rozszerzenie analizy zmian w toku dramatu na wszystkie wskaźniki i — przy danych strukturalnych — na akty oraz sceny;
- analiza sieci interakcji postaci;
- generowanie raportu reprodukowalnego dla konkretnego pliku i wersji algorytmu.

---

# Reprodukowalność

Przy powoływaniu się na wyniki narzędzia warto zapisać:

- wersję lub commit repozytorium;

- dokładny plik źródłowy;
- w przypadku porównania TXT ↔ TEI/XML — oba użyte pliki źródłowe;

- źródło i wydanie analizowanego tekstu;

- ewentualne zmiany dokonane przed analizą;

- datę wykonania obliczeń.

Pozwala to ograniczyć problem różnic pomiędzy wydaniami i sposobami formatowania tego samego dramatu.

---

# Status projektu

Projekt ma charakter badawczo-eksploracyjny. Główny przepływ opiera się na plikach TXT, a obsługa TEI/XML pozostaje eksperymentalnym modułem BETA.

Nie jest pełnym systemem NLP ani automatycznym interpretatorem tekstu literackiego. Jego zadaniem jest dostarczenie mierzalnych cech, które mogą wspomagać:

- distant reading;

- close reading;

- badania wersologiczne;

- stylistykę ilościową;

- dydaktykę;

- eksplorację większych zbiorów dramatów.
