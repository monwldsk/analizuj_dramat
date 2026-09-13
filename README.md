# Cyfrowa analiza dramatu

Narzędzie do ilościowej i eksploracyjnej analizy **dramatu wierszowanego napisanego w języku polskim**.

Projekt pozwala badać zróżnicowanie wersyfikacyjne dramatu, strukturę dialogu, udział poszczególnych postaci, didaskalia oraz wybrane cechy składniowe i leksykalne tekstu. Analiza wykonywana jest bezpośrednio w przeglądarce.

Narzędzie zostało pomyślane przede wszystkim jako pomoc w **eksploracji tekstu i formułowaniu hipotez interpretacyjnych**. Wyniki wskaźników należy traktować jako operacyjne przybliżenia, a nie jako automatyczną analizę literaturoznawczą lub pełny parser języka polskiego.

## Demo

Wersja działająca online dostępna jest przez GitHub Pages:

`monwldsk.github.io/analizuj_dramat/`

---

## Najważniejsze możliwości

Narzędzie umożliwia:

- analizę tekstu wklejonego bezpośrednio do aplikacji;
- wczytywanie własnych plików `.txt`;
- wybór tekstu z dołączonej biblioteki dramatów;
- automatyczne przypisywanie wersów do mówiących postaci;
- rozpoznawanie didaskaliów;
- przybliżone liczenie sylab;
- wykrywanie wersów współdzielonych przez dwie postaci;
- analizę dominujących formatów sylabicznych;
- porównywanie struktury wersyfikacyjnej wypowiedzi różnych bohaterów;
- obliczanie dodatkowych wskaźników stylistycznych i składniowych;
- wizualizację wyników na wykresach;
- eksport tabeli analitycznej do XLSX;
- porównywanie dowolnej liczby dramatów według wybranego wskaźnika.

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

Wypowiedź jest definiowana jako ciąg kolejnych fragmentów przypisanych temu samemu bohaterowi.

Wskaźnik:

```text
liczba fragmentów wypowiedzi /
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

Po przeanalizowaniu dramatu można wygenerować dodatkowe wizualizacje dla wybranych bohaterów.

Dostępne są:

- wykresy kołowe rozkładu formatów sylabicznych;
- możliwość połączenia kilku bohaterów w jedną grupę;
- wykres słupkowy porównujący formaty sylabiczne wielu postaci;
- wykres procentowego udziału wypowiedzi wszystkich bohaterów w całym tekście dialogowym.

---

# Analiza porównawcza dramatów

Osobny moduł pozwala porównać większą liczbę tekstów.

Można:

1. wybrać dramaty z biblioteki;
2. dodać własne pliki `.txt`;
3. zmienić nazwy pozycji;
4. ustawić ich kolejność;
5. wybrać jeden z dostępnych wskaźników;
6. wygenerować wspólny wykres.

Porównywać można m.in.:

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
- gęstość didaskaliów;
- udział wypowiedzi „na stronie”.

Biblioteczne teksty są buforowane w pamięci przeglądarki podczas porównania, aby nie pobierać ich ponownie dla każdego obliczenia.

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

| Pole | Znaczenie |
|---|---|
| Nr wersu | numer rzeczywistego wersu |
| Treść wersu | tekst analizowanej linii |
| Liczba sylab | wynik liczenia sylab na podstawie wykrycia samogłosek w pozycji zgłoskotwórczej |
| Bohater | rozpoznana postać lub `DIDASKALIA` |
| Wers współdzielony | oznaczenie `P_1` / `P_2` |

Tabelę można wyeksportować do pliku:

```text
wyniki.xlsx
```

Eksport generowany jest bezpośrednio w przeglądarce.

---

# Biblioteka

Biblioteka ma przede wszystkim charakter demonstracyjny.

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
    ├── card_icons/
    ├── charts/
    └── ...
```

Większość logiki aplikacji znajduje się obecnie bezpośrednio w `index.html`:

- HTML interfejsu;
- CSS;
- parser dramatu;
- algorytm liczenia sylab;
- obliczanie wskaźników;
- obsługa wykresów;
- analiza porównawcza;
- eksport danych.

---

# Technologie

Projekt wykorzystuje:

- HTML5;
- CSS3;
- JavaScript (bez frameworków);
- Chart.js — generowanie wykresów;
- SheetJS / XLSX — eksport wyników do arkusza.

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

# Dlaczego TXT, a nie TEI/XML?

Obecny format zapewnia niski próg wejścia: użytkownik może wkleić tekst lub otworzyć prosty plik `.txt`.

W konsekwencji program rekonstruuje strukturę dramatu na podstawie przyjętych reguł rozpoznawania poszczególnych elementów, co nie daje jednak 100% skuteczności.

Docelowym kierunkiem rozwoju jest obsługa TEI/XML, gdzie takie elementy jak:

- mówca
- wypowiedź
- wers
- didaskalium
- akt
- scena

byłyby oznaczone strukturalnie, a nie odgadywane z formatowania.

---

# Możliwe kierunki rozwoju

Najważniejsze potencjalne rozszerzenia:

- obsługa TEI/XML;
- raport jakości parsowania tekstu;
- walidacja skuteczności algorytmu liczenia formatu wierszowego na ręcznie opisanym korpusie;
- eksport wszystkich wskaźników do CSV/XLSX/JSON;
- eksport wyników analizy porównawczej;
- podział kodu na moduły;
- automatyczne testy jednostkowe parsera;
- większy i bardziej zróżnicowany korpus dramatów;
- pełniejsza analiza składniowa;
- rozbudowa narzędzia o analizę dynamiki zmian poszczególnych wskaźników w toku dramatu;
- analiza sieci interakcji postaci;
- generowanie raportu reprodukowalnego dla konkretnego pliku i wersji algorytmu.

---

# Reprodukowalność

Przy powoływaniu się na wyniki narzędzia warto zapisać:

- wersję lub commit repozytorium;
- dokładny plik źródłowy;
- źródło i wydanie analizowanego tekstu;
- ewentualne zmiany dokonane przed analizą;
- datę wykonania obliczeń.

Pozwala to ograniczyć problem różnic pomiędzy wydaniami i sposobami formatowania tego samego dramatu.

---

# Status projektu

Projekt ma charakter badawczo-eksploracyjny.

Nie jest pełnym systemem NLP ani automatycznym interpretatorem tekstu literackiego. Jego zadaniem jest dostarczenie mierzalnych cech, które mogą wspomagać:

- close reading;
- badania wersologiczne;
- stylistykę ilościową;
- dydaktykę;
- eksplorację większych zbiorów dramatów.