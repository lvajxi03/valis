# Nie taki diabeł straszny

Wyniki działania `Customize` zapisywane są do pliku konfiguracyjnego jako kolejne instrukcje lispowe. Można wyręczyć `Customize` i zrobić te wpisy na własną rękę, czasami to w ogóle jedyny możliwy sposób.

## Pliki i katalogi startowe

`Emacs` domyślnie szuka pliku, który będzie interpretowany przy starcie w katalogu domowym użytkownika. Plik powinien nazywać się `.emacs` (kropka na początku obowiązkowa i celowa), natomiast katalog domowy… hmm. Użytkownicy wszelkiej maści systemów uniksopodobnych nad zagadnieniem katalogu domowego przechodzą do porządku dziennego: ot, jest i tyle, różne programy szukają tam różnych plików konfiguracyjnych. Użytkownicy Windows mają gorzej.

W środowisku Windows katalogiem domowym (z punktu widzenia Emacsa, oczywiście) jest katalog wskazywany przez zmienną `%HOME%`, warto sobie ustawić takową zgodnie z modłą systemu, w którym pracujemy (czasami katalogi użytkowników znajdują się w `C:\Documents and Settings`, innym razem w `C:\Users`)– jeśli takowa nie istnieje lub wskazuje na nieistniejący katalog, to wtedy czytane są ustawienia rejestru (`HKEY_CURRENT_USER\Software\GNUEmacs`, pole `HOME`,`HKEY_LOCAL_MACHINE\Software\GNUEmacs\`, pole `HOME`), finalnie — przypisywany jest na sztywno `C:`. Dodatkowo w Windows akceptowany jest plik o nazwie `.emacs` jak i `_emacs` — aby usatysfakcjonować użytkowników FAT-podobnych systemów plików (są jeszcze tacy? na partycji systemowej? serio?)

## Lisp

`Lisp` to charakterystyczne nawiasy, często zagnieżdżone do granic przesady. Nie ma potrzeby jednak zgłębiać dokładnie zawiłości tego języka dla celów związanych li tylko z konfiguracją `Emacsa`, wystarczy opanować kilka podstawowych „idiomów”; często autorzy jakiegoś przydatnego kawałka kodu w lispie piszą dokładnie co należy umieścić w `.emacs`

### Ładowanie kodu w Lispie

Zewnętrzny kod w Lispie dobrze jest wrzucić do jakiegoś sensownego katalogu (najlepiej zrobić wspólny katalog na wszystkie ściągane z Sieci pliki lispowe), a później dopisać do `.emacs` ładowanie tego pliku zgodnie z poniższą składnią:

```elisp
(load-file "ścieżka do pliku")
```

Uniksowe wersje honorują tylko slash jako separator katalogów, wersja dla Windows uznaje zarówno slash jak i backslash. Każdy emacs uznaje początkową tyldę za alias katalogu domowego.

Czasami jakiś plik odwołuje się do innych modułów poprzez nazwę względną. Należy więc powiadomić Emacsa o tym, żeby szukał plików w katalogach z podanej listy.

Można to zrobić chociażby tak:

```elisp
add-to-list 'load-path "/katalog")
```

Zmienna `load-path` jest listą katalogów, w której interpreter szuka potrzebnych mu plików.

### Przypisywanie wartości zmiennym

Czasami aby zmienić jakąś funkcjonalność należy ustawić zmienną o określonej nazwie.

Składnia jest następująca:

```elisp
 (setq nazwa-zmiennej 'wartość)
 (setq nazwa-zmiennej "wartość")
 (setq nazwa-zmiennej wartość)
```

W drugim i trzecim przypadku przypisywany jest odpowiednio łańcuch znakowy i wartość liczbowa. Apostrof w przypadku pierwszym nakazuje lispowi zrobić przypisanie bez obliczania (sprawdzania) wartości wyrażenia — będzie to zrobione „w swoim czasie”.

### Zmienne logiczne

Zmienne mogą przyjmować również wartości logiczne: prawdę oraz fałsz.

Prawda:

```elisp
(setq zmienna-logiczna t)

```

Fałsz:
```elisp
(setq zmienna-logiczna nil)
```

### Moduły

Moduły ładowane są funkcją require o składni:

```elisp
(require 'nazwa-modułu)
```

Dokumentacja mówi jeszcze o różnych parametrach opcjonalnych, które na tym etapie nie są jeszcze potrzebne.

### Komentarze

Dwa średniki oznaczają początek komentarza. Końcem jest po prostu koniec linii.

### Przypisania klawiszy

Globalne (obowiązujące zawsze i wszędzie) przypisania kombinacji klawiszy do funkcji można zrobić tak:

```elisp
(global-set-key kombinacja 'funkcja)
```

Przykłady:

```elisp
(global-set-key [f12] 'flyspell-goto-next-error)
(global-set-key [(meta g)] 'goto-line)
```

`Emacs` umożliwia tworzenie różnych map klawiszy w zależności od zastosowań. Najczęściej różne mapy klawiszy występują w różnych trybach.

Przypisanie kombinacji klawiszy do funkcji dla określonej mapy osiągalne jest poprzez:

```elisp
(define-key mapa klawisze 'funkcja)
```

Podczas mapowania klawiszy problemem może okazać się znalezienie ich nazwy. Popularne skróty typu `C-x`, `M-x`, `C-h f` mogą średnio działać — Emacs woli swoje wewnętrzne identyfikatory, których przeciętny człowiek nie potrzebuje pamiętać. Przydatna funkcja kbd dokonuje konwersji z notacji powszechnie stosowanej przez użytkowników na notację wewnętrzną:

```elisp
(kbd "sekwencja klawiszy")
```

Funkcja zwraca wartość, którą należy wstawić jako argument dla `global-set-key` albo `define-key`:

```elisp
(global-set-key (kbd "M-g") 'goto-line)
```

### Lepsze (?) operacje na blokach tekstowych

Tryby: `pc-selection-mode` i `pc-bindings-mode` pozwalają na zaznaczanie przy pomocy `control`/`shift` oraz strzałek kursora oraz zgodne z Wordstarem kopiowanie, wklejanie, wycinanie i usuwanie (odpowiednio: `C-insert`, `S-insert`, `S-delete`, `C-delete`). Aby skorzystać z tych trybów należy załadować odpowiednie pakiety, a później wywołać funkcje odpowiedzialne za inicjalizacje trybów:

```elisp
(require 'pc-select)
(pc-selection-mode)
(require 'pc-bindings)
(pc-bindings-mode)
```

### Haki

`Hook` to kolejne słowo, które nie ma dobrego polskiego odpowiednika w kontekście programowania. W dużym uproszczeniu można powiedzieć, że `hook` to lista rzeczy (funkcji) do wykonania w odpowiedzi na jakieś zdarzenie. Funkcjonalność ta jest związana mocno z określonymi trybami.

Dodawanie kolejnej funkcji do hooka odbywa się za pomocą `add-hook`:

```elisp
(add-hook 'nazwa-hooka 'funkcja)
```

Przykład: przy otwieraniu pliku tekstowego (`*.txt`) włączany jest (zwykle) tryb `text-mode`. Aby dodać automatyczne sprawdzanie pisowni w locie, należy włączać flyspell-mode:

```elisp
(add-hook 'text-mode-hook 'flyspell-mode)
```

Wszystkie pliki, dla których aktywowany jest `text-mode`, będą miały również włączany `flyspell-mode`.

Ilość funkcji, które można dodać, jest ograniczona w zasadzie tylko ułańską fantazją użytkownika.

### Miscellanea,

czyli przydatne rzeczy, które ciężko pogrupować.

```elisp
(global-font-lock-mode)
```

Życie stanie się piękne, a składnia kolorowa.

```elisp
(setq-default transient-mark-mode t)
```
Zaznaczony blok będzie wyróżniony innym kolorem (i pomyśleć, że nie ma tego włączanego domyślnie…)

```elisp
(setq require-final-newline t)
```

Wymusza dodawanie znaku końca linii na końcu pliku. Przydaje się, owszem.
