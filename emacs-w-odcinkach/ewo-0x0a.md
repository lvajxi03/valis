# Harry Potter i komnata czarownic

## Zakładki

Pracując często z dużą ilością tekstu irytującym staje się wyszukiwanie konkretnego wystąpienia jakiegoś wzorca, zwłaszcza w wielu buforach.
Z pomocą przychodzą zakładki (_bookmarks_): można oznaczyć podane miejsce w tekście, a później łatwo do niego skoczyć, niezależnie od tego, który bufor jest aktualnie aktywny. Co ciekawe, zakładki są zachowywane wraz z otaczającym je tekstem (tzw. _kontekst_) tak, aby dana zakładka potrafiła się dynamicznie dostosować do zmian w pliku (dodanie bądź usunięcie linii przed zakładką spowodowałoby — w przypadku zakładek statycznych — eleganckie „rozjechanie się” zakładki i miejsca w tekście, które faktycznie oznaczono)

Operacje na zakładkach:

* `C-x r m RET` (`M-x bookmark-set`) wstawia nienazwaną zakładkę w miejscu bieżącego położenia kursora
* `C-r r m nazwa RET` (`M-x bookmark-set nazwa`) wstawia nazwaną zakładkę w miejscu bieżącego położenia kursora
* `C-x r b nazwa RET` (`M-x bookmark-jump`) skacze do miejsca oznaczonego przez zakładkę o podanej nazwie; jeśli plik, do którego odwołuje się podana zakładka nie jest jeszcze otwarty, Emacs otworzy dla niego nowy bufor i załaduje go
* `M-x bookmark-delete RET nazwa RET` usuwa zakładkę
* `C-x r l` (`M-x bookmark-bmenu-list`) wyświetla listę zakładek; klawiszem `d` można zaznaczyć niechciane zakładki do usunięcia

### Zapisywanie i ładowanie plików z zakładkami

Domyślnie `Emacs` zapisuje pod koniec sesji wszystkie zakładki do pliku `~/.emacs.bmk`, a po starcie automatycznie ładuje ten plik, zwalniając użytkownika z obowiązku wykonywania tej czynności. Od czasu do czasu może zaistnieć jednak potrzeba załadowania dodatkowego pliku z zakładkami bądź zapisania zakładek do własnego pliku. Służą do tego funkcje:

* `M-x bookmark-load RET nazwa_pliku` — ładuje plik z zakładkami
* `M-x bookmark-write RET nazwa_pliku RET` — zapisuje wszystkie zakładki do pliku o podanej nazwie
* `M-x bookmark-save` — zapisanie zakładek do domyślnego pliku (funkcja wywoływana automatycznie podczas kończenia sesji emacsowej)

Różne ciekawe ustawienia zakładek można znaleźć też w grupie „bookmark”.

## Rejestry

W rejestrach można umieszczać wszystko, co może przydać się później. Każdy rejestr ma swoją nazwę (pojedynczy znak); do rejestru można włożyć jednocześnie tylko jedną rzecz, włożenie drugiej spowoduje nadpisanie pierwszej.

### Tekst w rejestrze

(W miejsce `R` należy wstawić jednoznakowy identyfikator rejestru)

Poniższe funkcje pozwalają na kopiowanie, wstawianie i przemieszczanie tekstu do i z bieżącego bufora:

* `C-x r s R` (`M-x copy-to-register`) umieszcza kopię tekstu w rejestrze
* `C-u C-x r s R` przesuwa zaznaczony tekst do rejestru (jeśli bieżący bufor jest tylko do odczytu, w minibuforze znajdzie się komunikat błędu o niemożności usunięcia zaznaczonego tekstu do bufora; tekst zostanie po prostu skopiowany do rejestru)
* `C-x r i R` (`M-x insert-register`) wkleja zawartość rejestru do bufora. Pozycja kursora nie jest przesuwana na koniec wklejonego tekstu
* `M-x append-to-register` — dołącza region tekstu do poprzedniej zawartości danego rejestru
* `M-x prepend-to-register` — dołącza region tekstu na początek poprzedniej zawartości danego rejestru

### Konfiguracja okien w rejestrach

Do zapisywania i odtwarzania konfiguracji okien do i z rejestrów służą funkcje:

* `C-x r w R` (`M-x window-configuration-to-register`) — zapisuje stan okien z bieżącej ramki do podanego rejestru
* `C-x r f R` (`M-x frame-configuration-to-register`) — zapisuje stan wszystkich ramek i okien w nich zawartych do podanego rejestru
* `C-x r j R` (`M-x jump-to-register`) — oryginalnie przesuwa kursor do pozycji wskazywanej przez rejestr, ale jeśli znajduje się w nim konfiguracja okien — odtwarza ją.

Rejestry mogą przechowywać także liczby oraz pozycje w tekście, ale dla tych pierwszych ciężko znaleźć oczywiste zastosowanie (rejestry tekstowe są wykorzystywane znacznie częściej), a w przypadku tych drugich lepiej używać zakładek.

## Poza tematem

Tekst ten pierwotnie został opublikowany 15 marca 2007, pierwszym opublikowanym komentarzem był _„o czym jest ta strona? i co to jest ta komnata czarownic? bo takiego czegos nie bylo jak harry potter i komnata czarownic!!!!!!!!!!!„_
