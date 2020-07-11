
# Katalog dla każdego


W _Najlepszym z Edytorów_ oprócz plików można odwiedzać także katalogi. W tym drugim przypadku katalog otwierany jest przez narzędzie zwane `Dired`, które jest znakomitym menedżerem plików.

`Dired` może zostać wywołany zarówno poprzez odwiedzenie katalogu, jak i użycie funkcji `M-x dired` (`Emacs` następnie spyta o nazwę katalogu do otwarcia)

## Pierwsze wrażenie

Domyślnie bufor direda wygląda zupełnie jak wyjście programu `ls` uruchomionego z parametrem `-l`. Widać szczegółowy listing plików, włącznie z nazwą właściciela, grupą, uprawnieniami, rozmiarem i datą modyfikacji. Po buforze tym można nawigować kursorem, jak w zwykłym buforze edycyjnym (wyszukiwać, kopiować itd)

## Operacje na plikach

Bieżący plik (lub katalog) to ten znajdujący się w linijce wskazywanej przez kursor (u góry spisu znajduje się zwykle nagłówek, pomijany przez `Emacsa` przy wywoływaniu wszystkich dostępnych funkcji)

Przykładowe operacje:

* Kopiowanie (`C`)
* Zmiana nazwy (`R`)
* Usuwanie (`D`)
* Wykonanie polecenia powłoki (`!`)
* Utworzenie dowiązania symbolicznego (`S`)
* Utworzenie dowiązania twardego (`H`)
* Wydrukowanie pliku (`P`)
* Kompresja pliku gzipem (`Z`)
* Zmiana uprawnień (`M`)
* Zmiana grupy (`G`)
* Zmiana właściciela (`O`)
* Wyszukanie plików (`A`)
* Wyszukanie z interaktywnym „szukaj i zastąp” w plikach (`Q`)

## Zaznaczanie plików

Operacje można przeprowadzać zarówno na pojedynczych plikach, jak i na grupach plików. Pliki można grupować albo poprzez ręczne flagowanie, albo poprzez wybór konkretnej maski (lub rodzaju)

Przykładowe zaznaczenia:

* Zaznaczenie pojedynczego pliku (`m`)
* Odznaczenie pojedynczego pliku (`u`)
* Odwrócenie zaznaczenia (`t`)
* Zaznaczenie plików wykonywalnych (`* *`)
* Zaznaczenie katalogów (`* /`)
* Zaznaczenie linków symbolicznych (`* @`)
* Odznaczenie wszystkich zaznaczonych plików (`U`)
* Zaznaczenie pliku do usunięcia (`d`)

Do poruszania się po liście zaznaczonych plików służą `M-{` (w tył) i `M-}` (w przód)

Operacje na plikach pasujących do podanego wyrażenia regularnego:

* Zaznaczenie (`% m`)
* Zaznaczenie do usunięcia (`% d`)
* Kopiowanie (`% C`)
* Zmiana nazwy (`% R`)
* Utworzenie dowiązań symbolicznych (`% S`)
* Zamiana małych liter w nazwach plików na duże (`% u`)
* Zamiana dużych liter w nazwach plików na małe (`% l`)

Inne popularne operacje

* Tworzenie katalogu (`+`)
* Wyświetlanie różnic w plikach (`=`)
* Porównywanie pliku z jego kopią zapasową (`M-=`)
* Otwieranie pliku w trybie tylko do odczytu (`v`)

## Ciekawostki

Od wersji _22.x_ `Emacs` otwiera także obrazki/zdjęcia, a dzięki `tumme` generuje także podglądy dla plików graficznych znajdujących się w bieżącym katalogu (`C-t d`)

## WDired Mode

Tryb ten służy do edycji nazw lików w bieżącym buforze `direda`, dowiązań symbolicznych oraz praw dostępu do poszczególnych plików. Po skończeniu edycji odpowiednie akcje zostaną przedsięwzięte na zmienionych plikach.

## Na koniec

Zarówno `Dired`, jak i `WDired` są narzędziami dość rozbudowanymi. Dostarczają mnóstwo interaktywnych (jak i nieinteraktywnych) funkcji; w menu umieszczony jest jedynie ich niewielki podzbiór. Dobrze jest zapoznać się z odpowiednimi rozdziałami podręcznika oraz przejrzeć grupy opcji do ustawienia.
