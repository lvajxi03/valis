# Umysł jak brzytwa

Powtarzanie w kółko tych samych czynności podczas pracy z tekstem (lub wieloma tekstami) jest bardzo nużące: czy to wpisywanie tego samego tekstu, czy wywoływanie w kółko tego samego zestawu funkcji, potrafi irytować, a i pomylić się można bardzo łatwo. Na szczęście można sobie pomóc makrami, które grupują w jedną całość naciśnięte klawisze (tekst pisany, jak i wywoływane interaktywnie funkcje wraz z potrzebnymi danymi)

Wywoływanie zdefiniowanego makra odtwarza wszystkie wciśnięcia klawiszy, jakie miały miejsce podczas jego nagrywania — co oznacza, że makro może być użyte także do wstawiania dłuższego tekstu.

Przydatne funkcje:

* `M-x kmacro-start-macro` (`C-x (` lub `F3`) rozpoczyna nagrywanie nowego makra. Tak, można już pisać lub wywoływać funkcje.
* `M-x kmacro-end-macro` (`C-x )`) kończy nagrywanie nowego makra.
* `M-x kmacro-end-and-call-macro` (`C-x e`) wywołuje makro; jeśli było akurat nagrywane — kończy nagrywać przed wywołaniem. Do dalszego wywoływania można używać klawisza `e` — minibufor zawiera stosowną podpowiedź

## Przykłady

* `C-x (` dowolny tekst `C-x )`

Powyższe makro będzie wstawiać „dowolny tekst” od bieżącego położenia kursora. Jednak do wstawiania li tylko tekstu lepiej używać skrótów, które będą tematem osobnego odcinka.

* `C-x (` `C-a` `C-SPC` `C-e` `M-w` `RET` `C-a` `C-y` `C-x )`

Powyższe makro będzie kopiować treść bieżącego wiersza, wstawiać po nim nową linię i wklejać skopiowaną treść.
Inne przydatne funkcje

* `M-x kmacro-end-or-call-macro` (`F4`) kończy nagrywanie lub wykonuje ostatnie makro.
* `M-x apply-macro-to-region-lines` (`C-x C-k r`) wywołuje ostatnie makro dla każdej linii w zaznaczonym regionie tekstu. Gdyby zaznaczyć cały tekst w pliku a następnie wywołać drugie makro z podanych przykładów, linie w pliku zduplikowałyby się.
* `C-u C-x` ( wywołuje ostatnie makro, następnie pozwala dołączyć na jego koniec dodatkowe znaki (koniec edycji jest możliwy, jak łatwo się domyślić, poprzez `C-x )`)
* `C-u C-u C-x` ( pozwala dołączyć dodatkowe znaki na koniec makra, ale bez wywoływania go
* `M-x kmacro-edit-macro` (`C-x C-k RET`) otwiera edytor makr, na zakończenie pracy należy wcisnąć `C-c C-c`
* `M-x kmacro-edit-lossage` (`C-x C-k l`) zbiera sto ostatnich naciśnięć klawiszy w makro i włącza edytor makr
* `M-x kmacro-name-last-macro` (`C-x C-k n`) przypisuje ostatniemu makru konkretną nazwę (identyfikator)
* `M-x kmacro-edit-kbd-macro` (`C-x C-k e nazwa RET`) otwiera edytor makr z makrem o podanej nazwie

Nazywanie przydatnych makr jest dobrą praktyką: wywoływać je później można poprzez `M-x nazwamakra`

## Zapisywanie makr

Zapisywanie makr nie jest aż takie oczywiste i wygodne, jak zapisywanie zakładek. Funkcja `M-x insert-kbd-macro` wstawia odpowiadający kod lispowy do bieżącego bufora — tak więc dobrze wpisywać makra bezpośrednio do `~/.emacs` albo do dodatkowego pliku, ładowanego w `~/.emacs`.

## Udogodnienia związane z makrami

* `M-x kmacro-bind-to-key` (`C-x C-k b`) przypisuje skrót klawiszowy ostatniemu makru (Emacs ostrzeże, jeśli kombinacja klawiszy jest w użyciu, ale nic nie stoi na przeszkodzie, aby tą kombinację klawiszy nadpisać)
* Wciśnięcie `C-x q` podczas nagrywania makra nie spowoduje co prawda wstawienia dodatkowego znaku podczas jego wykonywania, ale użytkownik zostanie zapytany, czy makro ma być dalej wykonywane

## Makra nienazwane — nawigacja

Wszystkie zdefiniowane makra są pamiętane w postaci tzw. pierścienia — w dużym uproszczeniu można powiedzieć, że jest to lista bez początku i końca (zawinięta)
Między kolejnymi pozycjami pierścienia można przełączać się przy pomocy funkcji:

* `M-x kmacro-cycle-ring-next` (`C-x C-k n`) przesuwa się na następne makro w pierścieniu
* `M-x kmacro-cycle-ring-previous` (`C-x C-k p`) przesuwa się na następne makro w pierścieniu

Po przełączeniu się można bieżące makro nazwać, wykonać i tak dalej.

Należy pamiętać, że wszystkie makra obowiązują dla wszystkich buforów w danej sesji.
