## Praktyka dzień 2 1.09.2020
* [Nauka podstaw programowania w Cryptol](#Nauka-podstaw-programowania-w-Cryptol)

## Nauka podstaw programowania w Cryptol
*Ładowanie modułów 
 Podstawową czynnością do ukończenia kursu jest ładowanie modułów z labolatoriów. Aby się sprawnie poruszać 
na kursie należy załadować ścieżkę prostą instrukcją np...
```
Cryptol> :m labs::Language::Basics
Loading module Cryptol
Loading module labs::Overview::Overview
Loading module labs::Language::Basics
```
*Rodzaje zmiennych 
Ćwiczenie polegające na sprawdzaniu typów zmiennych zdefiniowanych poniżej za pomocą polecenia :t. Wynik zmiennej zostanie zamieszczony w komentarzu 
pod zmienną.
```
varType0 = False
/varType0 :Bit

varType1 = [False]
/varType1 :[1] 

varType2 = [False, False, True]
/varType2 :[3]

varType3 = 0b001
/varType3 :[3]

varType4 = [0x1, 2, 3]
/varType4 :[3][4]  

varType5 = [ [1, 2, 3 : [8]] , [4, 5, 6], [7, 8, 9] ]
/varType5 : [3][3][8]

varType6 = [ [1, 2, 3] : [3][8], [4, 5, 6], [7, 8, 9] ]
/varType6 : [3][3][8]

varType7 = [ [1, 2, 3], [4, 5, 6], [7, 8, 9] ] : [_][_][8]
/varType7 : [3][3][8]

varType8 = (0b1010, 0xff)
/varType8 : ([4], [8])

varType9 = [ (10 : [12], [1, 2 : [4], 3], ([0b101, 7], 0xab)),
             (18 : [12], [0, 1 : [4], 2], ([0b100, 3], 0xcd)) ]
varType9 : [2]([12], [3][4], ([2][3], [8]))

```
Jeśli do zmiennej zamieścimy nieprawidłowy typ np, to Cryptol wyrzuci błąd. Ponieważ Flase nie jest typem Bitowym, więc 
nie może mieć statycznie ustawionego typu 10 bitowej sekwencji
```
varType0 = False : [10]
```
Wszystkie typy powyżej były monomorficzne czyli do każdej zmiennej istniał jeden prawidłowy typ
*Rodzaje funkcji
1.Funkcja zapisana w stylu Curry 
```
add: [32]->[32]->[32]
add x y = x + y
```
2.Funkcja zapisana w stylu Uncurried
```
addUncurried : ([32], [32]) -> [32]
addUncurried (x, y) = x + y
```
Wynik wywołania funkcji dla tych samych wartości
```
Main> add 20 20
40
Main> addUncurried (20, 28)
48
```
Widać że te dwa style są na równowaznym poziomie, chodź styl Curried jest bardziej intuicyjny w pierwszej linijce tego 
stylu zdefiniowaliśmy że pierwsze dwa argumenty ze strzałkami reprezentują są sekwencją 32 bitową, zaś ostatnia jest 
otrzymywanym wynikiem o sekwencji 32 bitowej.

Następujące ćwiczenie polega na użyciu polecenia :type aby odkryć typy funkcji. Wynik typu funkcji zostanie zamieszczony 
jako komentarz pod funkcja
```
funType0 a = a + 7 : [5]
//funType0 : [5] -> [5]

funType1 a b = a + b + 0b0011100
//funType1 : [7] -> [7] -> [7]

funType2 a b = (a + 0x12, b + 0x1234)
//funType2 : [8] -> [16] -> ([8], [16])

funType3 (a, b) = (a + 0x12, b + 0x1234)
//funType3 : ([8], [16]) -> ([8], [16])

funType4 ((a, b), c) = c + 10 : [32]
//funType4 : {a, b} ((a, b), [32]) -> [32]

funType5 [a, b, c : [10]] = [a, b, c]
//funType5 : [3][10] -> [3][10]

funType6 (a : [3][10]) = [a@0, a@1, a@2]
//funType6 : [3][10] -> [3][10]


funType7 x = (x, x, [ [[False, True], x], [x, x], [x, x] ])
//funType7 : [2] -> ([2], [2], [3][2][2])
funType8 = funType2 10
//funType8 : [16] -> ([8], [16])
funType9 = False 
//funType9 : Bit
```
*Dopasowywanie wzorców
Cryptol umożliwia wykonywanie przypisań przez pisanie wzorców na podstawie typu wartości po jej prawej stronie przykład poniżej
```
Cryptol> let ab =(10,20)
Cryptol> ab
(10, 20)
Cryptol> let ( a ,b ) = ab
Cryptol> a
10
Cryptol> b
20
```
Funkcja sprawdzające 3 pierwsze bity dowolnej liczby

```
trzybity : {n} [3+n] ->[3] 
trzybity [a,b,c]=([a,b,c] # x)

```
Wywołanie funkcji trzybity
```
Main> :set base = 2
Main> trzybity 0b11111111
0b111
```
*Funkcje polimorficzne
Cryptol zapewnia polimorficzny system typów, sam polimorfizm można określić za rodzinę typów która pozwala na przykładzie listy zapewnić
jej wielopostaciowość. Np [n] jest rodziną typów zawierająca listy dowolnych typów
Sztywne określanie typów może być ograniczeniem środowiska cryptol.
Poniżej przedstawiam szkielet konstrukcji typy polimorficznego
```
functionName :
    {typeVariable0, typeVariable1}
	(typeConstraint0, typeConstraint1) =>
    inputType0 -> inputType1 -> outputType
```
Po przeanalizowaniu kursu do tego momentu mam możliwość zrozumienia początkowej funkcji sayHello zastosowanej
w tym kursie. 
```
sayHello:                         // funkcja sayHello ma typ
    {n}                           // ze zmienna typu n
    (n <= 12) =>                  // i ograniczenie n <=12 zastosowana do typu definicji
    [n][8] -> [7+n][8]            // która przyjmuje listę 8 wektorów-bitowych -> i zwraca listę 7+n wektorów 8 bitowych 
sayHello name = "Hello, " # name  // "napis" który jest tablicą zostanie połączony z tekstem podanym przez użytkownika.
```
Należy pamiętać że n nie może być większe niż 12 znaków


Funkcja polimorficzna która wyznacza 12 bit dowolnej liczby i jego analiza
```
dwunastybit : // funkcja dwunastybit ma typ
    {n}                   // ze zmienna typu n 
    (fin n , n >= 13) =>  // i ma dwa ograniczenia, n które jest skonczone i musi byc wieksze niz 12
    [n] -> Bit            // przyjmuje ona sekwencje [n] i  zwraca Bit
dwunastybit x =x@12       // @ to operator  który wyznaczy element na 12 element na liscie 
```
Wywołanie funkcji dwunastybit
```
Main> dwunastybit 0b10110010100111101010101010
True
```

Następujące ćwiczenie polegało na przekazaniu zmiennych typu i ich wartości do funkcji repeat(Kod został zamieszczony poniżej).
Dla podanych przykładów wpisać je w interpreter( wyniki zostaną podane w komentarzu pod danym przykładem.
Najpierw funkcja
```
repeat :
    {n, a}
    () =>
    a -> [n]a
repeat value = [ value | _ <- zero : [n] ]
```
Rozwiązania poniżej
```
polyType0 = repeat`{a=[64], n=2} 7
//[0x0000000000000007, 0x0000000000000007]

polyType1 = repeat`{n=4, a=[64]} 7
//[0x0000000000000007, 0x0000000000000007, 0x0000000000000007,
//0x0000000000000007]
 
polyType2 = repeat`{a=Bit, n=20} True
//0xfffff

polyType3 = repeat`{n=20, a=Bit} True
//0xfffff, 0b11111111111111111111 

polyType4 = repeat`{n=20} True
//0xfffff , 0b11111111111111111111 

polyType5 = repeat`{n=4, a=[2][3]} zero
//[[0b000, 0b000], [0b000, 0b000], [0b000, 0b000], [0b000, 0b000]]

polyType6 = repeat`{n=4, a=[3][7]} [1, 2, 3]
//[[0b0000001, 0b0000010, 0b0000011],
// [0b0000001, 0b0000010, 0b0000011],
// [0b0000001, 0b0000010, 0b0000011],
// [0b0000001, 0b0000010, 0b0000011]]

polyType7 = repeat`{n=4} ([1, 2, 3] : [3][7])
//[[0b0000001, 0b0000010, 0b0000011],
// [0b0000001, 0b0000010, 0b0000011],
// [0b0000001, 0b0000010, 0b0000011],
// [0b0000001, 0b0000010, 0b0000011]]
polyType8 = repeat 7 : [5][16]
//[0b0000000000000111, 0b0000000000000111, 0b0000000000000111,
// 0b0000000000000111, 0b0000000000000111]

polyType9 = repeat`{a=[16]} 7 : [5][_]
//[0b0000000000000111, 0b0000000000000111, 0b0000000000000111,
// 0b0000000000000111, 0b0000000000000111]

polyType10 = repeat`{n=5} 7 : [_][16]
//[0b0000000000000111, 0b0000000000000111, 0b0000000000000111,
// 0b0000000000000111, 0b0000000000000111]

polyType11 = repeat`{a=[16], n=5} 7
//[0b0000000000000111, 0b0000000000000111, 0b0000000000000111,
// 0b0000000000000111, 0b0000000000000111]

polyType12 = repeat`{5, [16]} 7
//[0b0000000000000111, 0b0000000000000111, 0b0000000000000111,
// 0b0000000000000111, 0b0000000000000111]

polyType13 = repeat`{5} (7 : [16])
//[0b0000000000000111, 0b0000000000000111, 0b0000000000000111,
// 0b0000000000000111, 0b0000000000000111]
```
Ciekawe jest to że każde wywołanie polyType od 9-13 jest takie same. Pokazuje siłę polimorfizmu i jego dowolnośc
w zastosowaniu. Z funkcją monomoficzna nie było by to tak możliwe.
*Todo 
Napisac funkcje o nazwie zeroPrepend kóra poprzedza n False bity na początku m bitowego wektora bitowego input.
Muszę skorzystać z operatora # 
Te zadanie sprawiło mi problem. 