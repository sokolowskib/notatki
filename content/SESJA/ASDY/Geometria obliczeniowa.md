### Otoczka wypukła

#### Graham

`P[0]` to najniższy punkt ze wszystkich, jak jest kilka to ten najbardziej po lewej.
Potem następne wierzchołki są ustawione posortowane wzgl. kolejności przeciwnej do ruchu wskazówek zegara. Czyli jeśli prowadzimy prostą z `P[0] do P[i] oraz P[0] do P[j]`, jeśli iloczyn wektorowy p1xp2 >0, to oznacza że p2 będzie potem w kolejności.

```python
function Graham{
	spermutacja P jak wyżej
	S.Push(P[0]);
	S.Push(P[1]);
	for(int k = 2; k <n; k++){
		var top = S.Pop();
		var below = S.Peek();
		while(S.Count >= 2 &&vector(below, top, P[k]) < 0){
			top = S.Pop();
			below = S.Peek();
		}
		S.Push(top);
		S.Push(P[k])
	}
	return S
}
```

### Dowód

`P[0]` - zawiera się w otoczce wypukłej zbioru P

Niezmiennik: punkty zawarte w S to otoczka wypukła zbioru od `P[0] do P[k]`.

. `P[0]` zawsze jest elementem otoczki wypuklej, ale załóżmy odwrotnie. Jeśli nie, to otoczka wypukła ma krawędź, która leży pod punktem `P[0]`, co jest niezgodne z definicją, sprzeczność.

Indukcja wobec niezmiennika:
Baza: S zawiera tylko `P[0]` oraz `P[1]`. Jest to naturalna otoczka punktów 0 i 1 (względem permutacji z wykonania Grahama).
Krok indukcyjny: Załóżmy, że niezmiennik prawdziwy dla zbioru {0,...,k-1}.

1. Po zakończeniu pętli while, S.BelowTop, S.Top oraz `P[k]` tworzą skręt w lewo. Oznacza to również że każda następna trójka obecna na stacku jest skrętem w lewo.
2. `P[k]` znajduje się na lewo względem prostej między `P[0]` a S.Top(), więc nie dojdzie do samoprzecięcia
3. Niech S' to wszystkie wierzchołki zdjęte w trakcie iteracji k-tej. Niech $Q \in S'$  to dowolny wyjęty wierzchołek. Z prawoskrętności S.Top, Q, `P[k]` wiemy,  że trójka ta tworzy kąt wklęsły. Jako że z sortowania P wynika, że kąt prostej pomiędzy  `P[0]` a S.Top jest mniejszy niż kąt między `P[0]` a Q, oraz że kąt prostej między `P[0]` a `P[k]` jest większy, to oznacza że Q leży w środku trójkąta `P[0], P[k] oraz S.Top`. Oznacza to, że zbiór S' należy do wielokąta wypukłego S\_k -> stanu stosu z k-tej iteracji. Dodanie k do S\_k-1 nie psuje wypukłości wielokąta wyznaczonego przez stos, ponieważ są obecne na nim tylko lewoskręty.  Z założenia indukcyjnego wszystkie wierzchołki {`P[0] , ... ,P[k-1]`} leżą wewnątrz wielokąta wypukłego wyznaczonego przez S\_k-1. Skoro wszystkie punkty z S\_k-1 albo należą do S\_k albo leżą wewnątrz wielokąta wypukłego wyznaczonego przez S\_k, to S\_k musi stanowić otoczkę wypukłą zbioru {`P[0] , .. , P[k]`}
4. Na mocy indukcji, po n-1 iteracjach pętli for S wyznaczy otoczkę wypukłą wszystkich punktów z P

Ograniczenie czasowe wynika z sortowania wierzchołków, co da się zrobić w O(nlog(n)

## Jarvis

Owijasz nitkę wokół wierzchołków

`P[0]` jak wyżej, reszta dowolnie

```python

function Jarvis()( P: tablica punktów){
	Wykonaj permutację P, czyli chodzi o samo P[0];
	
	s = new stack<int>();
	S.Push(P[0]);
	
	l <- oś X;
	
	while true
		znajdź k, takie że l minimalizuje kąt między prostą l od S.Top[] i P[k]
		
		if k==0
			break;
		l = prosta zawierająca S.Top i P[k]
		S.Push(P[k]);
	return S;

}
```

O(nh)

Punkt $P[k_i]$ wybrany w i-tej iteracji jest i+1 punktem otoczki wypukłej w kolejności ruchu wskazówek zegara.

### Quick Hull Rec

```python
L <- lista wynikowa

function QuickHullRec(A,B, S){
	if S pusty 
		return
	C <- punkt z S najdalej od prostej AB;
	S_1 <- zbiór punktów z S po prawej stronie AC;
	S_1 <- zbior punktów po prawej stronie CB ;
	QuickHullRec(A,C ,S1);
	Dodaj C do L;
	QuickHullRec(C,B,S_2);

}
```

A i B zawierają się w otoczce wypukłej. C też. Otoczka wypukła zbioru {A, B} $\cup S$ jest sumą otoczek {A,C} $\cup S_1$ oraz {C,B} $\cup S_2$ . Algorytm jest więc poprawny.

Pesymistyczna złożoność O(n^2) , oczekiwana O(nlog(n)). Punkty w trójkącie ABC są pomijane, ponieważ nie mogą znaleźć się w otoczce wypukłej.

## Zamiatanie

Y struktura <- wszystkie odcinki przecinające się z miotłą
X struktura <- posortowane zdarzenia (wobec osii X). Zdarzenia to koniec lub początek odcinka bądź przecięcie odcinków.

Żeby zbadać przecięcie dwóch odcinków, wystarczy obserwować zmianę kolejności odcinków w y-strukturze. Taka zmiana może wydarzyć się tylko wtedy gdy wsadzamy coś do miotły, czyli napotykamy z X struktury początek nowego odcinka, usuwanie z miotły, czyli koniec odcinka lub zmiana wynikająca z przecięcia dwóch innych odcinków. Wystarczy badać te zmiany.

Y jest posortowane względem tego, jaką wartość odcinek przyjmuje dla danego x\_0. Po insercie badamy przecięcie y.above i y.below z wkładanego s.

```python
function SegmentIntersection(S: zbior odcinkow){
	Y <- null
	X <- lista końców odcinków z S rosnąco względem x;
	
	for p in X
		if p jest lewym końcem s 
			Y += s;
			if s przecina się z Y.above(s) lub Y.Below(s)
				return true;
		else
			if Y.above(s) i Y.below(s) przecinają się
				return true;
			usuń s z Y
	return false;
}
```

W wariancie drugim po wykryciu przecięcia zamiast zwrócić true dodajemy punkt przecięcia do X. W przypadku napotkania w X punktu przecięcia zamieniamy kolejność odcinków w Y i badamy przecięcia z nowymi sąsiadami. W związku z tym że w tym wariancie punkty przecięcia są tworzone dynamicznie, X- struktura musi być dynamiczna.

Wykonanie operacji insert wraz ze sprawdzeniem above i below wykonuje się w czasie logarytmicznym. O(nlog(n));

Algorytm może zwrócić prawdę wtw. gdy rzeczywiście jakieś dwa odcinki s\_i i s\_j przecinają się w przestrzeni.

Jeżeli algorytm nie wykryje jakiegoś przecięcia, to oznacza że wartości przecinających się odcinków nigdy nie były sąsiadami w Y strukturze => istniał odcinek między nimi. Natomiast jeśli wiemy że odcinki s\_1 i s\_2 przecinają się w punkcie P, to pośrednie s\_3 albo zostanie usunięte z Y-struktury przed zderzeniem, albo przetnie się z którymś z s\_1 lub s\_2, dając nam wcześniejsze przecięcie, albo zjedzie się do punktu P, co zaprzecza założeniu o braku 3 przecinających się odcinków. Taki odcinek więc nie istnieje, co oznacza że s\_1 i s\_2 muszą sąsiadować w Y strukturze. Jeśli sąsiadują, to poprawność wykrycia ich przecięcia wynika z poprawności testu przecięć dwóch odcinków.

Jako że udowodniliśmy że test jest poprawny dla dwóch dowolnych odcinków sąsiadujących ze sobą w y strukturze, to na mocy poprawności działania sortowania w y strukturze oraz indukcji po ilosci elementow wyjetych z X struktury algorytm dziala.

![[obrazki/Pasted image 20260622185326.png]]

## Zad 1

Stosujemy metodę zamiatania. Jako zdarzenie bierzemy typ danych, który przechowuje wierzchołek p, dwa odcinki s1 oraz s2 ktorych koncem jest p oraz do ktorego wielokata nalezy ten wierzcholek.

Na wejściu oba wielokąty to listy zdarzeń posortowane względem współrzędnej x wierzchołka p.

Stosujemy merge  na dwóch listach zdarzeń. Lista wynikowa to X struktura.

Jako Y strukturę wykorzystujemy dowolna struktura danych, jako że w Y strukturze jest max 4 elementy to operacje na nich w sumie w O(1).

Jako że jeden wielokąt w drugim to też przecinające się wielokąty, to trzeba sprawdzić czy odcinek z 1 wielokąta nie jest jednocześnie pod jednym i nad drugim z dwoch odcinkow drugiego wielokąta.

```python

function HasIntersectingSegments(S: zbior odcinkow spelniajacych zalozenia);

X <- Merge(L_1, L_2)
Y = { }

while X not empty{
	ev = x.pop();
	s1 = ev.s1, s2 = ev.s2;
	
	p = ev.p
	
	if p jest poczatkiem odcinka s {
		doday s do Y-struktury
		if s nalezy do jednego wielokata oraz y.above(s) lub z y.below(s) naleza do drugiego
			return true
		if s przecina sie z y.above lub y.below(s) then
			return true
	
	}
	else
		if y.above(s) przecina sie z y.below(s)
			return true
		usun s z Y
return false
}
```

## Zad 2

```python

function arepolygonsintersecting(S: zbior wielokatow){
	X = {zbior zainicjalizowany zbiorem wielokatow}
	
	X.Sort();
	
	while X not empty{
	
		ev = X.pop();
		

		s1 = ev.s1, s2 = ev.s2;
		p = ev.p
		if p jest poczatkiem odcinka
			s doday do Y-struktury
			if s nalezy do jednego wielokata oraz y.above i y.below(s) nalezaz do drugiego
				return true
			if s przecina sie z y.above lub y.below(s) 
				return true
		else
			if s przecina sie z y.above lub y.below (s)
				return true
			usun s z Y
	} 
	return false
}

```

## Zad3

Należenie w O(log(n))

Sortujesz po x-owej wspolrzednej, a nastepnie dzielisz je na paski.

Kroki bo mnie zaraz popierdoli:

1. dzielisz na paski. bierzesz posortowane x-owo wierzcholki. dla kazdego wierzcholka, trzymasz jego dwa sasiednie wierzcholki. jesli wierzcholek jest przed, wsadzasz do poprzedniego pasa, jest w przod wsadzasz do nastepnego. tablica n paskow tak sie tworzy
2. binary search po paskach. paski maja w sobie posortowane y-owo krawedzie
3. sprawdzasz drugim binary searchem czy ilosc krawedzi nad q jest parzysta czy nieparzysta. jesli parzysta, poza, jesli nieparzysta nie

## Zad 4

mega proste. jesli p jest w srodku, to wszystkie skrety vi vi+1 vi+2 mod n sa w lewo. jesli nie, to jest poza.

1. sprawdz wszystkie skrety. jesli wszystkie na lewo , to zwroc Z
2. jesli na prawo, to zapisz indek v\_i na ktorym skreca w prawo. Wyjeb go, wstaw na jego miejsce p. teraz dla kazdej trojki v\_i, v\_i+1, v\_i+2

```python

findConvexHull(Z, p){
	for(int i = 1; i < n-1; i++){
		if(Z[i-1], Z[i] , p nie są skrętem w lewo){
			return foo(Z,p, i)
		}
		return Z
	}
}
foo(Z, p, i)[
	Z[i] = p;
	
	for(int j = i+1; i<n; i++){
		if Z[j-1], Z[j], Z[j+1] nie skret w lewo{
			j--;
			Usuń Z[j];
		}
		else
			break;
	}
	return Z;
]
```

## Zad 5

Otoczka górna i dolna

```python
function findconvexhull(Z1, Z2){
	P1 = najmniejszy wierzcholek wzgl. x w Z1
	P2 = najwiekszy wierzcholek wzgl x w Z2
	
	A_1 <- wszystkie na lewo od prostej p1 p2 w Z1
	A_2 <- wszystkie na lewo od prostej p1 p2 w Z2
	B_1 <- wszystkie na prawo od prostej p1 p2 w Z1
	B_2 <- wszystkie na prawo od prostej p1 p2 w Z2
	
	A = {p2 a2 a1 p1}
	B = {p1 b1 b2 p2}
	
	OA = nosortgraham(A)
	OB = nosortgraham(B)
	
	return OA + O B - p1 - p2
}

function no sort graham(P){
	s = new stack<int>();
	s.push(P[0])
	s.push(P[1])
	for(int i = 2; i < n; i++){
		while(s.belowtop(), s.top, P[i] nie sa skretem w lewo)
			s.pop();
		s.push(P[i])
	}
}
```

## Zad 6

Psuje się w górnym pół okręgu

## Zad 7

## Zad 8

Funkcja MinKołoObejmujące(Z):
// Z: Zbiór wejściowy wszystkich punktów

```
// Uzasadnienie: Losowa permutacja zbioru wejściowego jest konieczna, 
// aby zagwarantować oczekiwaną złożoność czasową O(n). Zapobiega to
// wystąpieniu najgorszego przypadku (O(n!)) dla specyficznie 
// ułożonych danych wejściowych.
P = LosowaPermutacja(Z)
R = ∅ // Zbiór pusty punktów brzegowych

Zwróć Welzl(P, R)
```

Funkcja Welzl(P, R):
// P: Zbiór punktów pozostałych do przetworzenia
// R: Zbiór punktów (maksymalnie 3), które na pewno leżą na brzegu koła

```
// Krok 1: Warunek brzegowy rekurencji
Jeśli P jest zbiorem pustym LUB rozmiar(R) == 3:
    Zwróć TrywialneKoło(R)
    
// Krok 2: Pobranie punktu z przetworzonej wcześniej losowej permutacji
Niech p będzie ostatnim punktem w zbiorze P
Zbiór P_bez_p = P \ {p}

// Krok 3: Wywołanie rekurencyjne dla podzbioru
K = Welzl(P_bez_p, R)

// Krok 4: Weryfikacja przynależności punktu p
Jeśli Zawiera(K, p) == Prawda:
    // Koło wyznaczone dla podzbioru obejmuje również punkt p
    Zwróć K
    
W przeciwnym wypadku:
    // Zgodnie z wcześniejszym dowodem, p nie należy do K, 
    // więc musi leżeć na brzegu nowego koła obejmującego.
    Zbiór R_nowy = R ∪ {p}
    Zwróć Welzl(P_bez_p, R_nowy)
```

Funkcja TrywialneKoło(R):
// Funkcja analitycznie wyznacza koło dla n <= 3 punktów brzegowych

```
Jeśli rozmiar(R) == 0:
    Zwróć PusteKoło (promień = 0)
    
Jeśli rozmiar(R) == 1:
    Zwróć Koło(środek = R[0], promień = 0)
    
Jeśli rozmiar(R) == 2:
    Niech S będzie środkiem odcinka wyznaczonego przez R[0] i R[1]
    Niech r będzie połową odległości między R[0] i R[1]
    Zwróć Koło(środek = S, promień = r)
    
Jeśli rozmiar(R) == 3:
    // Koło to okrąg opisany na trójkącie wyznaczonym przez R[0], R[1], R[2]
    Wyznacz środek S jako punkt przecięcia symetralnych boków trójkąta
    Niech r będzie odległością od S do R[0]
    Zwróć Koło(środek = S, promień = r)
```
