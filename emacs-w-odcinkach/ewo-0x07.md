# Kodowanie

Pomimo wszechogarniającej popularności `UTF-8` od czasu do czasu zachodzi potrzeba otwarcia pliku w innym kodowaniu, niż aktualnie używane, podobnie z zapisywaniem takiego pliku. Sytuacje takie występują pewnie częściej przy pracy grupowej. `Emacs` radzi sobie z takimi przypadkami znakomicie, udostępniając po prostu kilka wygodnych funkcji:

* `M-x set-buffer-file-coding-system` (`C-x RET f`) ustawia kodowanie znaków, w jakim będzie zapisany (lub odczytany ponownie) dany plik
* `M-x revert-buffer-with-coding-system` (`C-x RET r`) otwiera bieżący plik ponownie, z użyciem podanego kodowania

Skąd wiadomo, jakiego dokładnie kodowania użyć? `M-x list-coding-systems` wyświetla wszystkie dostępne kodowania wraz z opisami i aliasami. Ponadto, dobrze działa dopełnianie klawiszem tabulacji.

Dodatkowo dostępne są następujące funkcje, szalenie ułatwiające czasem życie:

* `M-x set-selection-coding-system` (`C-x RET x`) — ustawienie systemu kodowania znaków dostępnego dla selekcji (obszarów zaznaczanych) — jeśli tylko fragment tekstu zostanie skopiowany, wycięty lub wklejony (z użyciem schowka X lub Windows), zostanie przekonwertowany z użyciem podanego systemu kodowania znaków. Bardzo przydatne przy pracy interaktywnej z innymi programami.
* `M-x set terminal-coding-system` (`C-x RET t`) — ustawienie systemu kodowania znaków dla bieżącego terminala; nie ma to nic wspólnego z otworzonym plikiem, polepsza tylko jego czytelność na terminalu z innym kodowaniem.
* `M-x set-keyboard-coding-system` (`C-x RET k`) — zmienia obłożenie klawiatury. Funkcja ta bardzo przydaje się zwłaszcza w Windows — Emacs dla X grzecznie korzysta z ustawień locales.
* `M-x set-file-name-coding-system` (`C-x RET F`) — zmienia kodowanie używane przy zapisywaniu nazwy pliku.
