## Znalezienie dwóch najbliższych punktów na płaszczyźnie oraz zwrócenie odległości między nimi

```python

function findClosestPoints(S in R^2){
	if |S| <= 3
		brute force
		
	
	x <- mediana wspolrzednych x ze zbioru S
	S_l, S_r <- zbior punktow na lewo oraz na prawo od x
	
	delta = min(findClosestPoints(S_l), findClosestPoints(S_r));
	
	Y <- punkty z S o współrzędnej x z zakresu [x - delta, x + delta] posortowane rosnąco względem współrzędnej y
	
	
	for i = 1,2,...,|Y|
		for j = i-7, ..., i-1 
			if(d(Y[i] < Y[j]) < delta)
				delta = d(Y[i], Y[j]);
	zwroc delta
}
```

O chuj tu chodzi:

Dzieląc na podproblemy L i R jesteśmy w najmniejszym przypadku znaleźć najmniejszą odległość między punktami brute force'em. Wracając z rekurencji, musimy jednak rozpatrzeć czy najmniejsza odległość między punktami nie wynika z faktu, że jeden punkt leżał w lewej a drugi punkt w prawej stronie podproblemu. Wykonujemy więc sprawdzenie na środku.

Do tego wykorzystujemy Y, czyli posortowaną wzgl. współrzędnej y tablicy elementów wokół mediany. Ten pas ma szerokość 2delta, bo tylko takie rozwiązanie ma sens, inne są rozpatrzane w podproblemach lub automatycznie mają za dużą odległość.

Dla tego pasu trzymamy dwa iteratory, i oraz j. Dla każdego i, cofamy się co najwyżej 7 elementów do dołu, i sprawdzamy czy odległość między elementem Y\[i] a Y\[j] jest mniejsza niż obecnie zapisana.

Rozpatrzane jest tylko max. 8 elementów przed i, ponieważ jest to maksymalna ilość, która ma sens. Jeśli punkty p = (x\_p, y\_p) oraz q = (x\_q , y\_q) są po dwóch stronach to:
Powiedzmy że prostokąt `[x - delta, x + delta] x [(min(y_p, y_q), max(y_p, y_q))]` Ma więcej niż 8 elementów (czyli iterator będzie musiał mieć więcej niż 7 kroków wstecz). Szerokość tego prostokąta to 2 delta, a wysokość delta. Jeśli w tym prostokącie jest 9 elementów, to z zasady szufladkowania Dirichelta w jednej połowie jest 5 elementów. Jak podzielimy tą połowę na 4 kwadraty, to przynajmniej w jednym z nich (delta x delta) są dwa elementy. W takim wypadku odległość między nimi jest mniejsza niż delta, co jest sprzeczne z minimalnością znalezionej dotychczas delty w podproblemie (no bo oba byłyby z tej samej połówki a max odległość to ok. 0.707 delta). Oznacza to że  na podstawie wyniku podproblemu w tym pasie może być max 8 elementów.

Złożoność O(nlog^2(n)).

Sprytniejsza implementacja wymaga przechowywania x posortowanych względem x i y, przyspieszy sortowanie w pod problemach do O(n).

## Problem mnożenia liczb

Dwie n - cyfrowe liczby x i y, trzeba je pomnożyć.

### Algorytm Caracuby:

Wyznacz x1, x2 ,y1, y2:

`x = x_1 * 10^n/2 + x2`
`y = y_1 * 10^n/2 + y2`

Potem oblicz:
A = x1y1
B = x2y2
C = (x1 + x2)(y1 + y2)

Wówczas
xy = A10^n + C 10 ^n/2 + B

Czas działania
T(n) = 3T(n/2) + O(n)
czyli O(n^log\_2(3))

x\_1 to bardziej znaczące cyfry, x\_2 to mniej znaczące cyfry.

xy = A x 10^n + (C - A - B) x 10 ^n/2 + B;

## Twierdzenie o rekurencji uniwersalnej

$f(n) = af(n/b) + g(n)$

dla powyższego problemu a = 3, b = 2, g(n) = O(n)
Wychodzi:
O(n^log\_2(3))
