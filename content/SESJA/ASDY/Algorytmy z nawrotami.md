## Notacja O\*

Notacja O, tylko że z dokładnością do wielomianu.

### Cykl Hamiltona w grafach subkubicznych

Założenie: $\Delta <= 3$  .  Czy P da się rozszerzyć do cyklu Hamiltona?

```python
function IsHamilton(G, P = {v_1, ...., v_k}){
	if v_k = v(G)
		return v_k -> v_i in E(G); // jesli wszystkie wierzcholki w srodku, zobacz czy istnieje bezposrednie polaczenie zamykajace cykl
		
	else 
		foreach var u in N(v_k) not in P
			if(IsHamilton(G,P + u))
				return true
			
		return false;
}
```

Jako że $p_k$ ma co najwyżej dwóch sąsiadów nie na ścieżce P, oraz po wykonaniu rekurencji ilość niewykorzystanych wierzchołków maleje o jeden (właśnie go użyliśmy), to z jednego problemu dla $p_k$ mogą powstać co najwyżej dwa podproblemy + sprawdzanie warunków czy wierzchołek jest sąsiadem czy nie w czasie wielomianowym. Niech f to czas wykonania funkcji dla danej ilości niewykorzystanych wierzchołków, czyli n'.

$f(n') <= 2f(n-1) + \mathcal{O}(n)$

### Twierdzenie o rozwiązywaniu rekurencji

Niech f będzie funkcją z N\_+ -> R\_+ taką że:
$f(n) <= c_1f(n-1) + c_2f(n-2) + ... + c_df(n-d) + g(n)$
gdzie f(1), f(2),...,f(d) <= g(d) dla pewnych stałych d,c1,...,cd oraz niemalejącej funkcji g.

Wówczas f(n) = O\*(g(n)$\theta^n$)
gdzie $\theta$ jest największym pierwiastkiem równania
$x^d = c_1x^{d-1} + c_2x^{d-2} +..... c_d$
Wiemy, że $\theta$ spełnia warunek:
$\theta^n <= c_1\theta^{d-1} + c_2\theta^{d-2} +.... +c_d $

### Znajdowanie największego zbioru niezależnego

```python
function MaxIs(g){
	if(Delta(G) <= 2){
		zwroc sume ceil(v(C)/2) - ilosc nieparzystych cykli
		
		else 
		v <- wierzcholek o stopniu >= 3
		return max(Maxis(G-N[v]), maxis(G - v));
	}
}
```

f(n) <= f(n-3) + f(n-1) + O(m) =>
f(n) <= f(n-1) + f(n-3)
$x^3 = x^2 + 1$ co daje $\theta = 1.4568$

![[obrazki/Pasted image 20260622163059.png]]

## ZAD 1

założenia, G, $\Delta \le 3$ .

D\_min to najmniejszy zbiór dominujący, który udało nam się znaleźć.

D to obecny stan najmniejszego zbioru dominującego, a X to zbiór w którym trzymamy wierzchołki na pewno nie znajdujące się w D. Warunkiem zatrzymania będzie |D| + |X| = n

Możemy również skorzystać z faktu, że jeśli |D| > |D\_min|, to możemy uciąć drzewo rekurencji.

```python
function DominatingSet(G, D , X){
	if |D| > |D_min| and |D_min| > 0
		return
	
	Y <- zbiór wierzchołków, które nie są w X ani w D.
	
	if Y = { }
		if każdy wierzchołek z X jest zdominowany
			D_min = D;
		return;
	Niech u to dowolny wierzchołek z Y
	
	DominatingSet(G, D + u, X)
	
	if|N(u) >= 1|{
		v_1 in N(u)
		if v_1 not in D and v_1 not in X
			DominatingSet(G, D + v_1, X + u);
	}
	
	if |N(u) >= 2|
		v_2 in N(u) - v_1
		if v2 not in D and v_2 not in X
			DominatingSet(G, D + v_2, X + u + v_1)
			
	if |N(u)| = 3
		v_3 in N(U) - v_1 - v_2
		if v3 not in D and v3 not in X
			DominatingSet(G, D + v3, X + u + v1 +v2);
}
```

$f(n) \le f(n-1) + f(n-2) + f(n-3) +f(n-4)$ Daje to wynik O\*(1.9276^n).

## ZAD 2

Podejście brute force, można zoptymalizować, przez rozpatrzanie tylko opcji:
x = 1,
x = 0, y = 1
x = 0, y = 0, z = 1

```python
function Solve3SAT(R: zbior nie rozwiazanych klauzul){

	if R = { }
		return true
	
	T <- nierozstrzygnięta klauzula z R
	if(T nie spełnia klauzuli przy obecnym wartościowaniu)
		return false
	x, y , z <- literaly kasi, ktore nie maja jeszcze wartościowania
	
	x = true
	R` = takie klauzuly, ktore nie maja jeszcze rozwiazan, R - te rozwiazane x = true
	if Solve3SAT(R`)
		return true
	
	x = false, y = true
	R` = klauzule z R bez T oraz tych spelnionych przez y oraz ~x
	
	if Solve3SAT(R`)
		return true
	
	x = false, y = false, z = true;;
	
	R` = klauzula z R bez T oraz tych spelnionych przez z oraz ~y oraz ~x.
	
	
	if Solve3SAT(R`)
		return true


}
```

f(n) <= f(n-1) + f(n-2) + f(n-3) + O(n) <- obliczanie R'.

## ZAD 3

Wierzchołki z V(G) rozbijamy na 2 (ten o jednym i ten o drugim kolorze). Łączymy rozbite wierzchołki oraz te, które mają ten sam kolor (wierzchołki z których były rozbite muszą sąsiadować ze sobą). G da się pokolorować listowo, gdy G' jest dwudzielny, oraz  każdy wierzchołek z G ma swojego odpowiednika w tej samej klasie dwudzielności.

Dzieje się tak, ponieważ dla każdego wierzchołka $v_i \in V(G)$ klasa X zawiera wierzchołek $v_{ik}$ reprezentujący pokolorowanie v\_i na k. Zawiera ona dokładnie jeden wierzchołek v\_ik odpowiadajacy v\_i ze wzgledu na obecnosc krawedzi laczacych v\_ik z vil dla l in L\_v\_i.

Jednocześnie z dwudzielności wynika, ze dla kazdego koloru k w klasie X nie sąsiadują zadne inne wierzcholki v\_ik v\_jk takie, że v\_iv\_j należy do E(G). Kolorowanie jest więc poprawne. Analogicznie jeżeli istnieje kolorowanie grafu, do G' jest dwudzielny i odpowiednie wierzcholki G' odpowiadajace temu kolorowaniu tworza klase dwudzielnosci.

## ZAD 4

```python

function find3Coloring(G: ){
	color = new int[n];
	for(int i = 0; i < n; i++){
		color[i] = -1;
	}
	
	for(int i =0 ; i < n; i++){
		if(color[i] == -1)
			order = BFS(i) <- zwraca kolejnosc od zrodla do kolorowania
			color[i] = 0;
			Color(order, idx)
			
	}
}

function Color(order, idx){
	if idx = len(order)
		return true;
	
	v = order[i]
	forbidden = {color[u] : color[u] != -1 and uv in E(G)}
	for c in forbidden
		color[v] = c
		if Color(order, idx+1)
			return true;
		color[v] = none
	
	return false;
}
```

## Zad 5

```python
function podzialnatrojkaty (G){
	if n mod 3 != 0
		return false
		
	
	byMin = new List<int>[n] <- tablica, gdzie byMin[v] to trojkat o najmniejszym indeksie v
	
	foreach {a, b, c} in V, a <= b <= c
		if e(ab) and e(b,c) and e(ac):
			byMin[a].add({a,b,c});
	
	memo = pusta mapa bitowa
	fullset = {0, 1, ..., n-1}
	
	return solve(Fullset)
	
	
	function solve(S){
		if S == { }
			return true
		
		if S in memo:
			return memo[S]
			
			
		v = min(S)
		result = false
		foreach t in byMin[v]:
			if t in S
				if(Solve(S-t))
					result = true;
					break;
		memo[S] = result
		return result;
	}
}
```

Dowód na podstawie optymalnej podstruktury, jeśli S da się podzielić na na rozłączne trójkąty.

indukcja po |S|
Baza: dla zera zwraca true

Krok Indukcyjny: zakładamy że dla |S| - j działa.
v = min(S). Jeśli S da się rozłożyć na trójkąty, to v gdzieś w nich leży. Wszystkie wierzchołki z t należą do do S, bo if t in S. Jako że min(S) wzięło minimalny dozwolony, do v jest też najmniejsze w t. Jako że wszystkie te wierzchołki należą do S i to trójka, to algorytm wykrył je poprawnie. S-t zwróci poprawnie.

## ZAD 6

```python

function SteinerTree(G, w, S){
	k = |S|
	
	d = FloydWarshall(G,w)
	
	DP = nowa tablica rozmiaru 2^k z |V|
	DP = tablica[podzbiory S][wierzcholki V]
	for u in S 
		for v in V 
			dp[{u}][v] = d[u,v]
			
	foreach A in S with A.size >= 2 roznaco po wielkosci:
		foreach v in V
			foreach B in A 3^k
				D[A][v] = min(D[A][v] , D[B][v] + D[A-B][v])
				
		foreach v in V;
			foreach u in V:
				D[A][v] = min(D[A][v] , D[A][u] + dist[u][v])
				
	return min(D[S]);
}
```
