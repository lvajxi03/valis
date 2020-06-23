# Dolary, funty, franki? #

Wiemy już, że `Najlepszy z Edytorów` składa się z solidnego silnika lispowego, mnóstwa modułów lispowych oraz całej natywnej otoczki. Moduły lispowe wystawiają użytkownikowi całą masę różnych funkcji -- jest ich tak dużo (a poprzez instalację różnych dodatków jeszcze więcej), że nie sposób ich wszystkich używać podczas codziennej pracy. Nie sposób nawet poznać ich w odpowiednim czasie, sztuką jest wyszukać zestaw na tyle użytecznych, żeby w codziennej pracy bardziej pomagały niż przeszkadzały.

## Przerywanie ##

Kod lispowy jest wykonywany praktycznie cały czas. Bardzo często operacje te trwają bardzo długo (wyszukiwanie lub zastępowanie w plikach o wielu wierszach, ściąganie z sieci ogromnej ilości danych, itp), bądź w porę orientujemy się, że gdzieś popełniliśmy błąd i chcemy przerwać wykonywanie tego, co i tak nam już niepotrzebne -- z pomocą przychodzi usłużna kombinacja klawiszy `C-g` lub funkcja `M-x keyboard-quit` (funkcja może nie będzie aż tak pomocna, gdy `Emacs` zabierze nam na chwilę możliwość wprowadzania tekstu na czas wykonywania swojego przydługawego zadania, ale nazwa może się jeszcze kiedyś przydać)

Tak, czy siak, `C-g` powinno działać zawsze.

## Co to za klawisz? ##

`M-x describe-function` pokazuje, do czego przypięta jest dana kombinacja klawiszy. Poniżej przykład z `C-x C-s` -- jak widać, po krótkiej informacji o kombinacji klawiszy opis przechodzi płynnie we właściwą dokumentację funkcji `save-buffer` (tak, dokumentacja w `Emacsie` potrafi być bardzo obszerna):

```
C-x C-s runs the command save-buffer (found in global-map), which is
an interactive compiled Lisp function in ‘files.el’.

It is bound to C-x C-s, <menu-bar> <file> <save-buffer>.

(save-buffer &optional ARG)

Save current buffer in visited file if modified.
Variations are described below.

By default, makes the previous version into a backup file
 if previously requested or if this is the first save.
Prefixed with one C-u, marks this version
 to become a backup when the next save is done.
Prefixed with two C-u’s,
 makes the previous version into a backup file.
Prefixed with three C-u’s, marks this version
 to become a backup when the next save is done,
 and makes the previous version into a backup file.

With a numeric prefix argument of 0, never make the previous version
into a backup file.

(blah, blah, blah, tu jeszcze spory fragment dokumentacji)
```

`C-h f` to szybsze wywołanie tej funkcji.

## Co to za znak? ##

`M-x describe-char` pokazuje opis znaku, znajdującego się pod kursorem. Opis jest dość szczegółowy, wygląda mniej-więcej jak poniżej (akurat pod kursorem znalazła się litera `ł`)

```
             position: 761 of 982 (77%), column: 33
            character: ł (displayed as ł) (codepoint 322, #o502, #x142)
              charset: iso-8859-2 (ISO/IEC 8859/2)
code point in charset: 0xB3
               script: latin
               syntax: w 	which means: word
             category: .:Base, L:Left-to-right (strong), h:Korean, j:Japanese, l:Latin
             to input: type "C-x 8 RET 142" or "C-x 8 RET LATIN SMALL LETTER L WITH STROKE"
          buffer code: #xC5 #x82
            file code: #xC5 #x82 (encoded by coding system utf-8-unix)
              display: by this font (glyph code)
    uniscribe:-outline-Courier New-normal-normal-normal-mono-17-*-*-*-c-*-iso8859-1 (#xE1)

Character code properties: customize what to show
  name: LATIN SMALL LETTER L WITH STROKE
  old-name: LATIN SMALL LETTER L SLASH
  general-category: Ll (Letter, Lowercase)
  decomposition: (322) ('ł')

There are text properties here:
  fontified            t
```


## Co to za funkcja? ##

W drugą stronę też to oczywiście działa, `M-x describe-function` wyświetli informacje o funkcji i do jakiej kombinacji klawiszy jest przypięta:

```
save-buffer is an interactive compiled Lisp function in ‘files.el’.

It is bound to C-x C-s, <menu-bar> <file> <save-buffer>.

(save-buffer &optional ARG)

Save current buffer in visited file if modified.
Variations are described below.

(i tak dalej, dobrze znany już fragment dokumentacji dla save-buffer)
```

`C-h f` to szybsze wywołanie tej funkcji.

## Bufory, okna i ramki ##

O buforach, oknach i ramkach [już było](https://valis.thera.pl/ewo-0x01/), ale może warto uporządkować wiedzę o dostępnych operacjach:

* `C-x 2` -- poziomy podział ramki (okna)
* `C-x 3` -- pionowy podział ramki (okna)
* `C-x 1` — zlikwidowanie wszystkich podziałów, czyli powrót do ramki z jednym oknem.

> Ważne: bufory otworzone w podzielonych oknach nie będą teraz niszczone -- wszystkie są dalej otwarte, choć widoczny jest tylko jeden.

* `C-x 0` — usunięcie bieżącego okna
* `C-x 5 2` — otwarcie nowej ramki
* `C-x 5 1` — usunięcie bieżącej ramki
* `C-x 5 0` — usunięcie wszystkich ramek oprócz bieżącej
* `C-x o` — przeniesienie kursora (jak i całego sterowania) do następnego okna w bieżącej ramce
* `C-x S-o` — przeniesienie kursora (i sterowania...) do poprzedniego okna w bieżącej ramce

Ponieważ `Emacs` może utworzyć tyle plików, na ile pozwala mu wolna pamięć operacyjna dostępna dla systemu, nie ma sensu otwierać kolejnych instancji programu wraz z kolejnymi plikami. Gdy zachodzi potrzeba skorzystania z kolejnego pliku, należy go po prostu otworzyć -- zostanie stworzony nowy bufor, wypełniony treścią otwieranego pliku.

Jeśli plik nie jest już więcej potrzebny, można zamknąć (*zabić*) bufor za pomocą `C-x k`. Domyślnie `Emacs` ma w zwyczaju pytać o to, co zrobić z niezapisanymi buforami.

Przełączanie na następny lub poprzedni bufor jest możliwe dzięki (odpowiednio) `C-x strzałka-w-lewo` lub `C-x strzałka-w-prawo`. Można przełączyć się też na konkretny bufor przy pomocy `C-x b`, jeśli tylko pamięta się jego nazwę (w przypadku buforów zawierających pliki ich nazwa jest taka sama jak nazwa pliku)

Lista wszystkich buforów dostępna jest poprzez `C-x C-b`; można wtedy wybrać żądany bufor i przykładowo przełączyć się na niego. Teoretycznie wygląda to na sztukę dla sztuki, prawda? Różne metody zarządzania buforami można docenić przy pracy z kilkudziesięcioma lub więcej plikami (zwłaszcza, gdy powtarzają się nazwy plików, rozsianych gdzieś w drzewie katalogów)

