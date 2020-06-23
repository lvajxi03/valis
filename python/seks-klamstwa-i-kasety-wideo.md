# Seks, kłamstwa i kasety wideo

Czasami nie ma wyjścia i trzeba własnoręcznie przygotować pythonowy moduł do dziwacznej biblioteki w C (bo akurat nikt jeszcze nie zrobił), napisać taką bibliotekę własnoręcznie, albo zlinkować swój program z cudzą biblioteką (a mamy akurat `gcc` zamiast `Visual Studio`); cały ten tekst powstał lata temu, aby mi przypomnieć, jak to się robi (zawsze zapominam).

Do dzieła.

## Biblioteka

Przykładowa biblioteka niech będzie bardzo prosta i zawiera dwie funkcje: dodawanie i mnożenie dwóch liczb całkowitych.

Niech kod wygląda, jak poniżej:

```C
int mul2x(int x, int y)
{
  return x * y;
}

int add2x(int x, int y)
{
  return x + y;
}
```

Biblioteka jest prosta, więc niech się nazywa `SimpLib` (powyższy kod w C będzie się znajdował w pliku `simplib.c`)

### Budowanie biblioteki

Oczywiście źródła trzeba skompilować. Aby wygenerować bibliotekę, należy użyć flagi `-shared` dla `gcc`.

Konwencje nazewnicze: biblioteki dla systemu `Linux` zwykle mają przedrostek `lib` i przyrostek `.so` (rozszerzenie, jak zwał, tak zwał); łaczenie z parametrem `-lname` oznacza, że linker szuka pliku `libname.so` gdzieś w `${LD_LIBRARY_PATH}` albo w `ld.so.cache`; wiele bibliotek `GNU` dla `win32` podąża tą konwencją, aczkolwiek używane jest rozszerzenie `.dll`

#### Linux

Komendy do wykonania:

```sh
$ gcc -o simplib.o -c simplib.c
$ gcc -o libsimp.so simplib.o -shared
```

W przypadku wielu plików najlepiej kompilować każdy w osobnej transakcji, a zlinkować już grzecznie na koniec wszystkie pliki wynikowe.

Biblioteka została zbudowana i jest gotowa do użytku.

#### Windows

Pomimo używania tego samego kompilatora wymagany jest dodatkowy trik, aby biblioteki dało się używać. Trzeba wygenerować tak zwaną `interface library` (nie ma dobrego określenia ani tłumaczenia na ten twór) i dokleić do parametrów wołania linkera już w aplikacji:

```sh
$ gcc -o simplib.o -c simplib.c
$ dlltool -l libsimp.lib simplib.o
$ gcc -o libsimp.dll simplib.o -shared
```

Biblioteka została zbudowana zarówno w postaci właściwej dla systemu (`.dll`), jak i dodatkowego tworu, potrzebnego do zlinkowania aplikacji.

### Plik nagłówkowy

Dostarczanie plików nagłówkowych powinno widnieć w kodeksie honorowym każdego programisty gdzieś w okolicach punktu pierwszego. No więc:

```C
#ifndef SIMPLIB_H__
#define __SIMPLIB_H__

int mul2x(int x, int y);
int add2x(int x, int y);

#endif /* __SIMPLIB_H */
```

(Plik nagłówkowy będzie się znajdował w pliku `simplib.h`)

## Pierwszy program

Najlepszym sposobem na przetestowanie biblioteki będzie prosty program:

```C
#include
#include "simplib.h"

int main(int argc, char *argv[])
{
  printf("Adding 5 to 10: %dn", add2x(5, 10));
  printf("Multiplying 5 by 10: %dn", mul2x(5, 10));
  return 0;
}
```

(Kod źródłowy będzie się znajdował w pliku `simprg.c`)

### Budowanie programu

#### Linux

```sh
$ gcc -o simprg.o -c simprg.c
$ gcc -i simprg simprg.o -lsimp
```

#### Windows

Trzeba pamiętać o tej nieszczęsnej `interface library`:

```sh
$ gcc -o simprg.o -c simprg.c
$ gcc -o simprg.o libsimp.lib -lsimp
```

#### Dodatkowe flagi

Jeśli nasza biblioteka znajduje się gdzieś indziej niż standardowe systemowe miejsce na biblioteki, trzeba pamiętać o `-L` i podaniu lokalizacji. Windows honoruje domyślnie katalog bieżący, Linux niekoniecznie -- trzeba pamiętać o `-L.`

## Moduł dla języka Python

Kod modułu powstaje (cóż za niespodzianka!) w języku `C`:

```C
#include <Python.h>
#include "simplib.h"

static PyObject *simp_mul2x(PyObject *self, PyObject *args)
{
  int x, y, result;
  PyObject *pyresult;
  if (!PyArg_ParseTuple(args, "ii", &x, &y))
  {
    return NULL;
  }
  result = mul2x(x, y);
  pyresult = Py_BuildValue("i", result);
  return pyresult;
}

static PyObject *simp_add2x(PyObject *self, PyObject *args)
{
  int x, y, result;
  PyObject *pyresult;
  if (!PyArg_ParseTuple(args, "ii", &x, &y))
  {
    return NULL;
  }
  result = add2x(x, y);
  pyresult = Py_BuildValue("i", result);
  return pyresult;
}

static PyMethodDef SimpMethods[] = {
  {"mul2x", simp_mul2x, METH_VARARGS, "Multiply two integer values"},
  {"add2x", simp_add2x, METH_VARARGS, "Add two integer values"},
  {NULL, NULL, 0, NULL}
};

PyMODINIT_FUNC initsimp(void)
{
  (void) Py_InitModule("simp", SimpMethods);
}
```

(Kod modułu zostanie zapisany jako `simpmodule.c`)

### Krótkie objaśnienie kodu

* kod modułu trzeba jakoś zainicjalizować, więc potrzebna funkcja inicjalizacyjna
    * zgodnie z konwencją nazewniczą powinna zawierać łańcuch `init` i nazwę modułu (więc będzie nosić nazwę `initsimp`)
* funkcja inicjująca tworzy obiekt modułu; wszystkie funkcje wymienione w tablicy metod (tutaj: `SimpMethods`) staną się automatycznie metodami tego obiektu
* tablica metod zawiera identyfikatowy łańcuchowe, rzeczywiste funkcje `C` i ewentualne parametry, o ile takowe będą potrzebne (wartość `METH_VARARGS` oznacza, że do danej metody zostanie przekazana krotka parametrów, w które na razie nie wnikamy)

Właściwe funkcje modułu są bardzo proste: wywołują odpowiednie funkcje `C` i zwracają wynik. Dobrze gdzieś na samym początku sprawdzić poprawność parametrów:

```C
if (!PyArg_ParseTuple(args, "ii", &x, &y))
{
  return NULL;
}
```

* jeśli parametry nam nie pasują, to zwracamy `NULL` i koniec pieśni; w powyższym kodzie oczekiwane są dwie liczby typu integer (stąd `ii`), zapisywane potem do `x` oraz `y`
* po wykonaniu właściwej funkcji w `C` trzeba jeszcze zwrócić wynik w postaci zrozumiałej dla `Pythona`:

```C
pyresult = Py_BuildValue("i", result);
return pyresult;
```

* w powyższym kodzie zwracany jest wynik typu integer (stąd `i`)
* podręcznik języka `Python` zaleca, aby wszystkie funkcje (oprócz inicjującej) i zmienne były statyczne

### Setup

Standardowa dystrybucja `Pythona` dostarcza własny mechanizm budowania i zarządania modułami. Wszystkie podstawowe funkcje są zawarte w pakiecie `distutils`.

Typowy instalator jest zwykłym programem `Pythonowym`, przykładowy może wyglądać tak, jak poniżej:

```python
#!/usr/bin/env python

from distutils.core import setup, Extension

simpmodule = Extension('simp',
  sources = ['simpmodule.c'],
  libraries = ['simp'],
  library_dirs = ['.'],
)

setup (name = 'SimpLib',
  version = '1.0',
  description = 'This is SimpLib bindings package',
  ext_modules = [simpmodule])
```

(Kod programu zostanie zapisany jako `setup.py`)

Tworzony jest nowy obiekt typu `Extension` (`simpmodule`), ze wszystkimi parametrami potrzebnymi do zbudowania: nazwa, pliki źródłowe, biblioteki, ścieżki. Później wołana jest funkcja `setup`, aby to wszystko wykonać.

### Budowanie  modułu

#### Linux

```sh
$ python setup.py build
```

#### Windows

Na nieszczęście, `Python` chce zbudować moduł przy pomocy `VisualC++` (bo oficjalna dystrybucja też jest kompilowana w ten sposób, brrr). Aby użyć innego kompilatora, trzeba zaznaczyć to flagą `-compiler`:

```sh
$ python setup.py build –compiler=mingw32
```

### Instalacja modułu

#### Linux

```sh
$ python setup.py install
```

Zbudowane moduły i biblioteki lądują tam, gdzie powinny i nie ma sensu zawracać sobie dalej tym głowy (tylko używać, używać, używać...)

#### Windows

Powstały plik `simp.pyd` należy skopiować do katalogu `Libsite-packages`.

#### Przykładowy program

Gdy moduł został już zainstalowany, dobrze jest napisać krótki program testowy:

```python
#!/usr/bin/env python

from simp import *

print("Adding 212 to 384:", add2x(212, 384))
print("Multiplying 212 by 384:", mul2x(212, 384))
```

albo nawet sprawdzić, co sie stanie, gdy argumenty są od czapy:

```python
print("Multiplying 212 by string:", mul2x(212, "invalid argument"))
```

albo od czapy jest ich ilość:

```python
print("Multiplying 212 by 384 by 25:", mul2x(212, 384, 25))
```
