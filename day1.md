## Praktyka dzień 1 31.08.2020
* [Wstępne przygotowanie](#Wstepne-przygotowanie)
* [Nauka podstaw programowania w Cryptol](#Nauka-podstaw-programowania-w-Cryptol)


## Wstępne przygotowanie
* Przygotowanie środowiska
Podstawą rozpoczęcia kursu było zainstalowanie Dockera oraz Visual Studio Code gdzie następnie pobrano kontener
z Coryptolem  i zgodnie z instukcjami zawartymi na stronie przygotowałem środowisko do pracy z czym zeszło się najdłużej.
https://github.com/weaversa/cryptol-course/blob/master/INSTALL.md
*Pierwsze kroki 
Przechodząc do drugiego i trzeciego  rozdziału zapoznałem się z podstawowym napisaniem "Hello World" oraz wywoływaniu gotowych funkcji.
Zapoznałem się z podstawową mechaniką używając komendy :help .


## Nauka podstaw programowania w Cryptol
* Podstawowe typy danych 
1.Deklaracja liczby całkowitej 
```
Cryptol> let x=3 :Integer
Cryptol> x
3
Cryptol> -x
-3
```

2.Deklaracja liczby całkowitej z możliwośią ustawienia n-bitowej długości zmiennej
Dodatkowo instrukcja :s base=2,8,10,16 pozwala nam zmienić tryb widoku widzianych dla nas przez interpreter systemów liczbowych

```
Cryptol> let x=3:[16]
Cryptol> :s base=2
Cryptol> x
0b0000000000000011

```


3.Typ Rational - reprezentuje podzbiór liczb wymiernych które przedstawia się jako stosunek liczb całkowitych
```
Cryptol> let x=ratio 1 2
Cryptol> let y=ratio 2 4
Cryptol> x
(ratio 1 2)
Cryptol> y
(ratio 1 2)
Cryptol> let z =(1/.2) :Rational 
Cryptol> z
(ratio 1 2)
```

4.Typ Tuples - Krotka czyli prosty uporządkowany zbióre z dowolnymi typami. Zapisujemy je w nawiasach i mogą być róznych typów 
Dodatkowo aby wyświetlić elementy w krotce w której zawierają się napisy( Zamiast znaków ascii) to należy użyć
instrukcji :s ascii = on 
```
Cryptol> let tuple= (1,2:[3],True,("A","B","C"))
Cryptol> tuple
(1, 2, True, ("A", "B", "C"))
```
5.Typ Sequences - Tablice jedno i wielo-wymiarowe
```
Cryptol> let tab = [1,2,3]
Cryptol> let tab2 = [[1,2,3],[1,2,3]]
Cryptol> tab
[1, 2, 3]
Cryptol> tab2
[[1, 2, 3], [1, 2, 3]]
```
*Generowanie tablic i operacje
1. Enumerations - Pozwala nam na stworzenie tablicy która reprezentuje ciąg arytmetyczny.
Pierwszy element reprezentuje początek tablicy, ostatni zaś końcowy. 
```
Cryptol> let x = [1 .. 10]
Cryptol> x

[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```
Natomiast środkowy element przy jej tworzeniu pozwala nam stworzyć odstęp
```
Cryptol> let y =[1,4 .. 10]
Cryptol> y
[1, 4, 7, 10] //  różnica = 4-1 =3  każdy następny element będzie większy o 3
```

2. Generowanie tablicy
2.1 Generowanie tablicy za pomocą operatora "," który pozwoli aby każdy kolejny element tablicy był wynikiem danego działania
z całego zakresu. Możliwe jest tworzenie iloczynów kartezjańskich.
```
Cryptol> [ x + y | x<-[1..2] ,y <-[1..3]]
[2, 3, 4, 3, 4, 5]
Cryptol> [ (x,y) | x<-[1..2] ,y <-[1..3]]
[(1, 1), (1, 2), (1, 3), (2, 1), (2, 2), (2, 3)]
```
Dla operatora "," wynikiem będą wszystkie operacje elementów  zawarte w przedzialach 

2.2 Generowanie tablicy za pomocą operatora "|" który pozwoli aby każdy kolejny element tablicy  był wynikiem danego działania
w tym przypadku operacja będzie się wykonywać do końca zakresu liczności drugiego zbioru. 
```
Cryptol> [ x + y | x<-[1..5]|y <-[1..3]] // Jak widać w tablicy reprezentujacy zbiór y  jest mniej liczny 
[2, 4, 6]                                //Zatem operacja generowania nowej tablicy zakonczy sie na 3 elementach
Cryptol> [ (x,y) | x<-[1..2] | y <-[1..3]]
[(1, 1), (2, 2)]

```
2.3 Generowanie nieskończonych tablic
```
Cryptol> let x = [1:[32] ...] 
Cryptol> x
[1, 2, 3, 4, 5, ...]
```
3. Indeksacja i permutacja
3.1 Operator "#" - pozwala nam na dodanie do tablicy elemeny z drugiej tablicy na kolejnych jej indeksach
```
Cryptol> let x =[1,2,3] # [3,4,5]
Cryptol> x
[1, 2, 3, 3, 4, 5]
```

3.2 Operator "@" służy do wyznaczenia elementu tablicy pod względem wpisanego indeksu
```
Cryptol> let x =[1,2,3,3,4,5]
Cryptol> x@1
2
```

3.3 Operator "!" służy do wyznaczania elementu tablicy pod wzdlędem wpisanego indeksu licząc od ostatniego elementu

```
Cryptol> let x =[1,2,3,3,4,5]
Cryptol> x!1
4
```

3.4 Operatory permutacji "@@" i "!!"  oba wygenerują dowolną permutacją nastomiast drugi wykona ją z indeksacją od tyłu
```
invalid sequence index: 5
Cryptol> let x= [1..20]
Cryptol> x @@ [1,5,2]
Cryptol> let y=[ 1,4,7]
[2, 5, 8]
Cryptol> x !! y
[19, 16, 13]
```





	


