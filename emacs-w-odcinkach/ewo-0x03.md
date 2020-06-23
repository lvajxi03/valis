# Seek and destroy

Każdy szanujący się edytor tekstu umożliwia wyszukiwanie i zastępowanie tekstu, interaktywne bądź automatyczne. Edytory z górnej półki potrafią dodatkowo operować wyrażeniami regularnymi. Edytor z najwyższej półki — `Emacs` — radzi sobie z tymi funkcjami znakomicie i na wiele sposobów.

## Wyszukiwanie

Zwykłe wyszukiwanie tekstu jest realizowane przez funkcje:

* `M-x search-forward` umożliwia wyszukanie pojedynczego wystąpienia łańcucha znaków. Jeśli łańcuch został odnaleziony, kursor zostaje umieszczony na jego końcu. W przeciwnym wypadku `Emacs` wyświetli w minibuforze komunikat.
* `M-x search-backward` umożliwia wyszukanie pojedynczego wystąpienia łańcucha znaków począwszy od bieżącego miejsca w tył. Jeśli łańcuch został odnaleziony, kursor jest umieszczany na jego początku. W przeciwnym wypadku `Emacs` wyświetla w minibuforze komunikat.

Funkcje powyższe nie są przypisane do żadnych skrótów klawiszowych, można je wywołać interaktywnie, choć przeznaczone są w zasadzie dla programistów skryptów lispowych (w stylu choćby obsługi błędów kompilacji -- edytor sam skoczy na przykład do pierwszego napotkanego błędu)

## Wyszukiwanie wyrafinowane

(wywoływane zwykle zamiast powyższych funkcji):

`M-x isearch-forward` umożliwia tak zwane szukanie inkrementalne łańcucha znaków: z każdym nowym znakiem wprowadzanym przez użytkownika do minibufora `Emacs` stara się znaleźć wystąpienie całego łańcucha i odpowiednio przesunąć kursor do tego miejsca w tekście. Domyślnie znaleziony łańcuch jest podświetlony. Jeśli w buforze/regionie znajdują się następne wystąpienia tego łańcucha — także zostaną podświetlone (innymi kolorami tła i znaków)

Funkcji tej przypisana jest kombinacja `C-s`. Szukany łańcuch jest pamiętany pomiędzy kolejnymi wywołaniami funkcji — każde następne użycie `C-s` powoduje przeniesienie kursora do miejsca, w którym znajduje się następne wystąpienie szukanego łańcucha.

Jeśli osiągnięto koniec bufora, `Emacs` wyświetli komunikat w minibuforze. Każde następne użycie `C-s` spowoduje szukanie łańcucha od początku bufora.

`M-x isearch-backward` umożliwia identyczne szukanie, ale w tył. Funkcji domyślnie odpowiada kombinacja `C-r`. Po osiągnięciu początku bufora następne wywołanie `C-r` powoduje wyświetlenie komunikatu w minibuforze. Każde następne użycie powoduje szukanie od końca bufora.

## Szukanie z użyciem wyrażeń regularnych

* `M-x search-forward-regexp` umożliwia wyszukanie pojedynczego wystąpienia ciągu znaków pasujących do podanego wyrażenia regularnego. Jeśli ciąg taki został odnaleziony, kursor przesuwany jest na jego koniec. Gdy brak dopasowań, `Emacs` wyświetli komunikat w minibuforze.
* `M-x search-backward-regexp` umożliwia wyszukanie pojedynczego wystąpienia ciągu znaków, pasującego do podanego wyrażenia regularnego ale w tył.
* `M-x isearch-forward-regexp` — działa podobnie do isearch-forward, ale operuje na wyrażeniach regularnych. Domyślną kombinacją klawiszy jest `C-M-s`.
* `M-x isearch-backward-regexp` — działa podobnie do isearch-backward, ale operuje na wyrażeniach regularnych. Domyślną kombinacją klawiszy jest `C-M-r`.

## Zastępowanie

Zwykłe zastępowanie tekstu realizowane jest przez funkcje:

* `M-x replace-string` zastępuje wszystkie wystąpienia jednego łańcucha znaków innym łańcuchem od miejsca, w którym wywołano tą funkcję.
* `M-x replace-regex` zastępuje wszystkie wystąpienia ciągów znaków pasujących do podanego wyrażenia regularnego podanym łańcuchem od miejsca, w którym wywołano tą funkcję.

Zastępowanie wykonywane na co dzień realizowane jest przez funkcje:

* `M-x query-replace` — interaktywnie zastępuje wystąpienia jednego łańcucha znaków innym łańcuchem od miejsca, w którym wywołano tą funkcję. Funkcji przypisana jest domyślnie kombinacja `M-%`

Użytkownik przy każdym wystąpieniu jest pytany o to, co należy zrobić: `y` wykonuje pojedyncze zastąpienie i przechodzi do następnego znalezionego łańcucha, `n` pomija wystąpienie i przechodzi do następnego, `q` kończy zastępowanie, `!` zastępuje to i wszystkie następne wystąpienia łańcucha bez zadawania zbędnych pytań (inne możliwe akcje opisane są choćby w `M-x describe-function RET query-replace RET`)

* `M-x query-replace-regexp` — działa podobnie do query-replace, ale operuje na wyrażeniach regularnych. Funkcji przypisana jest domyślnie kombinacja `C-M-%`
