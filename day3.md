## Praktyka dzień 3 2.09.2020
* [Nauka podstaw programowania w Cryptol](#Nauka-podstaw-programowania-w-Cryptol)

## Nauka podstaw programowania w Cryptol
* Funkcje polimorifczne
Napisać funkcje o nazwie zeroPrepend która poprzedza  n False bity na początku m-bitowego wektora bitowego input. Należy użyć operatora #.
Brakujące pola wypełnić
```
zeroPrepend : 
  {prezero, numb} (fin prezero, fin numb) =>   [numb] -> [prezero +numb] 
zeroPrepend  input = repeat`{prezero} False # input
```
Wynik działania programu 
```
Main> :set base =2
Main> zeroPrepend`{5} 4
0b00000100
```
* Operatory logiczne
"~" - Negacja
```
Cryptol> ~0b00001111
0b11110000
```
"&&" - AND
```
Cryptol> 0b10110011 && 0b11001100
0b10000000
```
"||" - OR
```
Cryptol> 0b100000000000 || 0b000011011001
0b100011011001
```
"^" - Exclusive OR
```
Cryptol> 0b00001010 ^ 0b01011101
0b01010111
```

* Rotacje i przesunięcia 
">>" - Przesunięcie bitowe w prawo
```
Cryptol> [1..5] >> 3
[0, 0, 0, 1, 2]
```

"<<" - Przesunięcie bitowe w lewo
```
Cryptol> [1..5] << 3
[4, 5, 0, 0, 0]
```
">>>" - Rotacja w prawo
```
Cryptol> [1..5] >>> 2
[4, 5, 1, 2, 3]
```
"<<<" - Rotacja w lewo 
```
Cryptol> [1..5] <<< 2
[3, 4, 5, 1, 2]
```
* if... then ... else
Klasyczna instrukcja warunkowa
```
Cryptol> 1 + (if 10>12 then 12 else 998) + 1
1000
```

* Manipulacje listą 
"take" - Pozwala na zabranie(Skopiowanie) dowolnej ilości elementów z listy i tworzy nową listę składającej się z niej
```
Cryptol> let x= [1..5]
Cryptol> x
[1, 2, 3, 4, 5]
Cryptol> take`{3} x
[1, 2, 3]
```
"drop" - pozwala na opuszczenie dowolnej ilości elementów w liście i tworzy nową tablicę z tych wartości które pozostały
```
Cryptol> let x = [1..10]
Cryptol> x
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
Cryptol> drop`{5} x
[6, 7, 8, 9, 10]
```
"head" - Wyznacza pierwszy element tablicy, podobnie jak w liście gdzie pierwszy element listy jest głową
```
Cryptol> x
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
Cryptol> head x
1
```
"tail" - Wyznacza wszystkie elementy po zerowym ineksie, podobnie jak w liście gdzie pozostałe elementy po głowie listy
tworzą ogon listy
```
Cryptol> x
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
Cryptol> tail x
[2, 3, 4, 5, 6, 7, 8, 9, 10]
``` 
"last" - Ostatni element w tablicy 
```
Cryptol> x
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
Cryptol> last x
10
```
"reverse" - Odwrócenie elementów w tablicy
```
Cryptol> x
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
Cryptol> reverse x
[10, 9, 8, 7, 6, 5, 4, 3, 2, 1]
```
"groupBy" - Pozwala na pogrupowanie tablicy tworząc tablice dwuwymiarową. Należy pamiętać że nie każdą tablicę da się pogrupować
w zależności od zadanej wartości. Np dla 12 elemenowej tablicy  nie możemy jej pogrupować dla wartości 7.
```
Cryptol> groupBy`{2} x
[[1, 2], [3, 4], [5, 6], [7, 8], [9, 10]]

```
"split" - Pozwala na rodzielenie wartości/ napisu i zamieszczenie go w tablicy
```
Cryptol> let y = split`{10} "abcdefghij"
Cryptol> y
["a", "b", "c", "d", "e", "f", "g", "h", "i", "j"]
```
"join"- pozwala na złączenie wszystkich elementów w tablicy tworząc z tego wartość/string
```
Cryptol> let y = split`{10} "abcdefghij"
Cryptol> y
["a", "b", "c", "d", "e", "f", "g", "h", "i", "j"]
Cryptol> join y
"abcdefghij"
```
transpose - Operacja polegająca na transpozycji elementów w tablicy 
```
Cryptol> transpose [[1, 2], [3, 4]]
[[1, 3], [2, 4]]
```

* Operatory programowania funkcyjnego
Cryptol obsługuje kilkia idiomów w programowaniu funkcyjnym

"sum" - Operator wykonuje sekwencję elementów i dodaje je do siebie stosując podstawową arytmetykę 
```
Cryptol> sum [1..10]
55
Cryptol> sum( groupBy`{2}[1..20])
[100, 110]
Cryptol> sum [ [1,2,3],[4,5,6]]
[5, 7, 9]
Cryptol> sum [ sum [1,2,3],sum[4,5,6]]
21
```
"map" - Działa podobnie do "sum" ale tyczy się on każdego elementu w tablicy
```
labs::Language::Basics> map increment [1, 2, 3, 4, 5]
[0b00000000000000000000000000000010,
 0b00000000000000000000000000000011,
 0b00000000000000000000000000000100,
 0b00000000000000000000000000000101,
 0b00000000000000000000000000000110]
 
 labs::Language::Basics> let suma (a,b) = a*b
labs::Language::Basics> map suma [(5,5),(10,10),(100,100)]
[25, 100, 10000]
```
"iterate" - Ten operator odwzorowuje funcję iteracyjnie podan wartość początkową i tworzy nieskończoną listę do zastosowań funkcyjnych
```
labs::Language::Basics> iterate increment 0
[0, 1, 2, 3, 4, ...]
labs::Language::Basics> iterate increment 1
[1, 2, 3, 4, 5, ...]

labs::Language::Basics> let fun  a x = a + x
labs::Language::Basics> iterate (fun 3) 0
[0, 3, 6, 9, 12, ...]
```
* Implementacja prostego szyfru z kursu
```
keyExpand : [32] -> [10][32]
keyExpand key = take roundkeys
    where
        roundkeys:[inf][32]
        roundkeys =[key]#[roundkey<<<1|roundkey <-roundkeys]


encrypt : [32] -> [32] -> [32]
encrypt key plainText = cipherText
  where
    roundKeys = keyExpand key
    roundResults = [plainText] # [ roundResult ^ roundKey
                                 | roundResult <- roundResults
                                 | roundKey <- roundKeys
                                 ]
    cipherText = last roundResults
```
wynik
```
Main> encrypt 0xc001c0de 0x9972831a
3683445328
```







