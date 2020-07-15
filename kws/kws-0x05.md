# Arduino i wyświetlacz 1x7

Pojedynczy wyświetlacz siedmiosegmentowy, podłączony do _Arduino_ nie wymaga o wiele więcej gimnastyki niż oprogramowany na _Raspberry Pi_ w Pythonie. Język `C` potrzebuje nieco więcej uwagi i niewielkiego przerobienia poprzedniego algorytmu, ale jest to szybkie i bezbolesne.

Ponieważ cyfrowe piny GPIO są do wykorzystania zupełnie dowolnego, poszczególne segmenty zostały podłączone, jak poniżej:

```C
int s_common = 23;
int s_a = 25;
int s_b = 27;
int s_c = 29;
int s_d = 31;
int s_e = 33;
int s_f = 35;
int s_g = 37;
int s_dp = 39;
```

`s_common` jest tutaj wspólną katodą, a `s_dp` -- kropką dziesiętną.

Program ma za zadanie wyświetlać w pętli kolejne cyfry, dodatkowo zapalając kropkę dziesiętną przy tych nieparzystych.

Trzy kolejne tablice zawierają zbiory:

* wszystkich pinów
* pinów sterujących jakimkolwiek segmentem
* pinów sterujących segmentami składającymi się na cyfry:

```C
int all[] = {s_common, s_a, s_b, s_c, s_d, s_e, s_f, s_g, s_dp};
int visible[] = {s_a, s_b, s_c, s_d, s_e, s_f, s_g, s_dp};
int segments[] = {s_a, s_b, s_c, s_d, s_e, s_f, s_g};
```

... a dziesięć kolejnych to układy segmentów w poszczególnych cyfrach:

```C
int s_0[] =  {1, 1, 1, 1, 1, 1, 0};
int s_1[] =  {0, 1, 1, 0, 0, 0, 0};
int s_2[] =  {1, 1, 0, 1, 1, 0, 1};
int s_3[] =  {1, 1, 1, 1, 0, 0, 1};
int s_4[] =  {0, 1, 1, 0, 0, 1, 1};
int s_5[] =  {1, 0, 1, 1, 0, 1, 1};
int s_6[] =  {1, 0, 1, 1, 1, 1, 1};
int s_7[] =  {1, 1, 1, 0, 0, 0, 0};
int s_8[] =  {1, 1, 1, 1, 1, 1, 1};
int s_9[] =  {1, 1, 1, 1, 0, 1, 1};
```

Wszystkie cyfry zgrupowane zostały o, tak:

```C
int *digits[10] = {s_0, s_1, s_2, s_3, s_4, s_5, s_6, s_7, s_8, s_9};
```

`setup()` zawiera inicjalizację każdego z pinów (będzie od tej pory pinem wyjściowym):

```C
void setup() {
  for (int i = 0; i < 9; i++)
  {
    int pin = all[i];
    pinMode(pin, OUTPUT);
  }
  // Wspólna katoda, więc stan niski na wspólny pin:
  digitalWrite(s_common, LOW);
}
```

... a `loop()` -- iterowanie po wszystkich cyfrach i segmentach, żeby odpowiedni zapalić, lub zgasić w zależności od potrzeb.

Dodatkowo zapalana jest lub gaszona kropka dziesiętna:

```C
void loop() {
  for (int i = 0; i < 10; i++)
  {
    int *dig = digits[i];
    for (int j = 0; j < 7; j++)
    {
      if (dig[j] == 1)
      {
        digitalWrite(segments[j], HIGH);
      }
      else
      {
        digitalWrite(segments[j], LOW);
      }
      int have_dp = i % 2;
      if (have_dp == 1)
      {
        digitalWrite(s_dp, HIGH);
      }
      else
      {
        digitalWrite(s_dp, LOW);
      }
    }
    delay(1000);
  }
}
```

