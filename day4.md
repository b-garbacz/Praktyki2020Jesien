## Praktyka dzień 2 1.09.2020
* [Nauka podstaw programowania w Cryptol](#Nauka-podstaw-programowania-w-Cryptol)

## Nauka podstaw programowania w Cryptol
* Definiowanie za pomocą let
Interpreter Cryptola obsługuje proste definicje let, które nie są trwałe bo istnieją do momentu ponownego zdefiniowania 
tej samej nazwy
```
Main> let x = {a=0b011,b=0b100}
Main> x.a
3
Main> x.b
4
Main> let incrementt (x:[8]) = x+1
Main> incrementt 255
0
```
* Klazula where
Crytpl daje możliwość tworzenia lokalnego powiązania za pomocą klazuli "where" po to aby nadać nazwy typom i wyrażeniom
podrzędnym.
```
twoPlusXY: ([8],[8]) ->[8]
twoPlusXY (x,y) =2 + xy
    where xy = x*y
	
Main> twoPlusXY (1,2)
4
```

```
minMax4 :{a} (Cmp a) => [4]a ->(a,a)
minMax4 [a,b,c,d] = (x,y)
    where   x= min a(min b (min c d))
            y =max a(max b (max c d))
Main> minMax [1..4]
(1, 4)
```

*Wyrażenie lamda
Funkcje lambda nazywamy funkcjami anonimowymi(bez nazwy). Klazule where wykorzystujemy w przypadku funkcji pomocniczej, 
w przypadku kiedy jest ona bardzo prosta można zastosować wyrażenie lambda i  nie nazywać tej funkcji.
```
Main> f 8 where f x = x+1
9
Main> (\x ->x+1) 8
9
```
* Rekursja i rekuerncje 
Przykład rekurencyjnego sprawdzenia przystości liczby
```
isOdd, isEven : {n} (fin n, n >= 1) => [n] -> Bit
isOdd x = if x == 0 then False else isEven (x - 1)
isEven x = if x == 0 then True else isOdd (x - 1)

Cryptol> isEven`{8}4 
True
Cryptol> isOdd`{8}4 
False
```
Drugi przykład mając świadomość że ostatni bit jest bitem parzystości
Przykład Rekursyjnego sprawdzenia maksimum w tablicy
```
isOdd2, isEven2 : {n} (fin n, n >= 1) => [n] -> Bit
isOdd2 x = x!0
isEven2 x = ~(x!0)

Main> isEven2`{8} 16
True
```
Przykład rekursji w kórym szukamy maksymalnego elementu w tablicy
```
maxSeq : {a,b} (fin a , fin b) => [a][b] -> [b]
maxSeq xs = ys ! 0
    where ys = [0] # [ max x y | x <- xs
                               | y <- ys
                               ]

Main> maxSeq [1,2..12]
12
Main> maxSeq ([10, 9 .. 1] # [1 .. 10])
10
```

```
maxSeq' : {a,b} (fin a , fin b) => [a][b] -> [1+a][b]
maxSeq' xs = ys
    where ys = [0] # [ max x y | x <- xs
                               | y <- ys
                               ]

Main> maxSeq'[10,9..1]
[0, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10]

```

Wzorzec zastosowany w maxSeq jest przykładam jak działa Folds... Wyrażenie takiego warunku w Cryptolu wygląda następująco
```
ys =[i] # [f(x,y]) | x <-xs
                     y<-ys
          ]
```
W zasadzie to wzorzec ten można postrzegać jako generowanie sekwencji, gdzie xs jest sekwencją wejściową transformwaną przez f
które są kumulowane w ys. Zwróćmy uwagę że każdy nowy element ys jest generowany przy użyciu poprzedniego elementu [i] możemy traktować 
jako seed. Taka implementacja pozwoli nam zastąpić pętlę while.
Napiszmy Funkcje sumAll który zsumuje wszystkie elementy w tablicy za pomocą tego wzorca
```
sumAll : {n, a} (fin n, fin a) => [n][a] -> [a]
sumAll  xs = ys ! 0
    where ys = [0] # [x+y |x<-xs
                          |y<-ys
                       ]
					   
Main> sumAll [1.. 10]:[20]
55
```
*Równania strumieniowe
Większośc algorytmów kryptograficznych jest opisana jako operacja strumienia bitów. Sposób przedstawiania tych operacji wykorzystują
równania strumienia. Output ze strumienia tablicy deklarujemy jako "as"

Rozwiązany przykład
```
xs input = as where    
    as =[0x89,0xAB,0xCD,0xEF] # new
    new = [a ^ b ^ c | a<-as
                      | b<-drop`{2} as
                      | c<-input]
					  
Main> xs [3,3,3,3,3,3,3,3,3]
[0x89, 0xab, 0xcd, 0xef, 0x47, 0x47, 0x89, 0xab, 0xcd, 0xef, 0x47,
 0x47, 0x89]

```

