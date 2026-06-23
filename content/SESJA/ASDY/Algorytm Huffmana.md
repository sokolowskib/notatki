![[obrazki/Pasted image 20260622124438.png]]

Kod stałej długości => każdemu znakowi przypisujesz kod
Kod zmiennej długości => Znakom występującym częściej dajemy krótszy kod.

Kod prefiksowy => żaden kod nie jest początkiem innego kodu.  Czytasz bit po bicie i jak dopasujesz do jakiegoś znaku => wtedy wiesz że to on.

Kody te trzyma się w drzewie binarnym, żeby przyspieszyć lookup znaku odpowiadającemu danemu kodowi.

## Problem optymalnego kodu prefiksowego

Szukamy drzewa binarnego, reprezentującego cały alfabet, które minimalizuje
$B(T) = \sum_{c\in C}f(c)d_T(c)$ gdzie d\_T(c) to długość kodu znaku c. -> liczba krawędzi od korzenia do liścia zawierającego znak c.

## Algorytm

```python

function Huffman(C: zbior wezlow, f:C => N){

	Q = new priority_queue<int>(C,f(c)) // kolejka priorytetowa z elementami c zainicjalizowana priorytetami nadanymi przez f;
	
	while Q.Count >1 {
		z <- new node();
		z.left = q.extractMin();
		z.right = q.Extractmin();
		f(z) = f(x) + f(y);
	}
	return q.extractmin();
}

```

Idea algorytmu polega na tym, że w optymalnym drzewie nie ma wewnętrznych węzłów, które mają tylko jedno dziecko. Jeśli taki węzęł ma tylko jedno dziecko, to można je przepiąć do węzła wewnętrznego wyżej, co daje nam mniejszy $b_T(c)$ .

Niech x i y to dwa najrzadziej używane znaki. Wtedy Istnieje optymalne drzewo kodowe, gdzie x i y mają wspólnego rodzica.

Jeśli drzewo T' jest optymalnym drzewem kodowym dla alfabetu C' = C - {x,y} $\cup$ {z}, gdzie f(z) = f(x) + f(y), wtedy T otrzymane z T' przez dodanie x i y jako dzieci węzła z jest optymalne, bo

$B(T) = B(T') + f(x) + f(y)$
Gdyby istniało B(T\*) < B(T) dla C, to zbudowalibyśmy drzewo T'' dla C' o koszcie
B(T\*) - f(x) - f(y) < B(T') , co jest sprzeczne z optymalnością T'.

Indukcja po rozmiarze alfabetu kończy dowód.

Pierwsza to **własność zachłannego wyboru** (slajd 12): istnieje _jakieś_ optymalne drzewo, w którym `x` i `y` (dwa najrzadsze) są rodzeństwem na najgłębszym poziomie. Dowód przez wymianę: weź dowolne optymalne `T`. Z lematu o pełnym drzewie najgłębszy liść ma rodzeństwo — nazwij tę parę `a`, `b`. Zamień `x` z `a` oraz `y` z `b`. Policz zmianę kosztu przy zamianie `x` i `a`:

$Δ=(d(a)−d(x)) (f(x)−f(a))\Delta = (d(a) - d(x))\,(f(x) - f(a))Δ=(d(a)−d(x))(f(x)−f(a))$

`a` jest na najgłębszym poziomie, więc d(a)≥d(x)d(a) \ge d(x) d(a)≥d(x); `x` jest najrzadszy, więc f(x)≤f(a)f(x) \le f(a) f(x)≤f(a). Iloczyn nieujemnego i niedodatniego jest ≤0\le 0 ≤0, czyli koszt **nie rośnie**. Skoro `T` było optymalne, koszt nie może też spaść — więc się nie zmienił, a nowe drzewo jest równie optymalne. To pokazuje, że zachłanny ruch (sklejenie dwóch najrzadszych) niczego nie psuje: jest optymalne drzewo, które się z nim zgadza.
