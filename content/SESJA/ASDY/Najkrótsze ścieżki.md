## Bellman-Ford

Potrzebujesz założenia o braku nieujemnych cykli.

```python
function BellmanFord(G, s){
	dist = new int[n];
	foreach(var v in V(G)){
		dist[v] = inf;
	}
	dist[s] = 0;

	for(int i = 1; i < n; i++){
		foreach(var e in E(G)){
			int u = e.From;
			int v = e.To;
			if(dist[v] > dist[u] + e.weight)
				dist[v] = dist[u] + e.weight;
		}
		return dist;
	}
}
```

Bellman Ford wpierw szacuje odległość każdego wierzchołka od s. Następnie co następną iterację `i` poprawia wynik długości najkrótszych co najwyżej i elementowych spacerów od s do i.

### Dowód poprawności

d\_k(v) -> minimalna długość spaceru o <= k krawędziach od s do v;
d(v) -> odległość od s do v;

- w każdym momencie zachodzi `dist[v] >= d(v)`. Indukcja po ilości wykonań `dist[v] > dist[u] + w(uv)`
  - i = 0. `dist[s] = 0`. brak ujemnych cykli, więc poprawne, `dist[u] = inf > d(u)` dla każdego innego u, spełniony warunek
  - przed wywołaniem `dist[v] = dist[u] + w(uv)`spełniony jest warunek `dist[u] >= d(v)`. Po wywołaniu pętli
    `dist[v] = dist[u] + w(uv) >= d(u) + w(uv)`.
  - najkrótsza ścieżka do u + odległość uv jest niekrótsza niż najkrótsza ścieżka do v, więc:
  - `dist[v] = dist[u] + w(uv) >= d(u) + w(uv) >= d(v)`
  - po wykonaniu linii dla każdego wierzchołka v w G spełniony jest ten warunek na mocy indukcji
- po i iteracjach zewnętrznej pętli zachodzi `dist[v] <= d_i(v)`:
  - zachodzi dla i = 0, bo 0 elementowy spacer z s dojdzie tylko do s, czyli `dist[s]` = 0;
  - załóżmy, że prawdziwe dla i = j, gdzie j < (0, n-2); weźmy dowolny wierzchołek v z G.
  - niech `dist[v]` to stan po j-tej iteracji, a `dist'[v]` po j+1;
  - chcę pokazać że `dist'[v] <= d_j+1(v)`. Jeśli `dist[v] <= d_j+1(v)`, to `dist[v] >= dist'[v]`, bo w trakcie algorytmu tablica dystansu tylko zmniejsza wartości.
  - rozpatrzamy więc przypadek `dist[v] > d_j+1(v)` . Wtedy :
    - Niech P to najtańszy spacer o co najwyżej j+1 krawędziach z s do v. Wtedy niech P' to spacer o jedną krawędź krótszy, od s do x. Jako że P to najkrótsza ścieżka to v, to d\_j+1(v) = d\_j(x) + w(xv);
    - Wtedy z założenia indukcyjnego `dist[x] <= d_j(x)` . Więc `dist[x] + w(uv) <= d_j(x) + w(uv) = d_j+1(v)`. Jako że `dist[x] + w(uv) = dist'[v]` to pociąga `dist'[v] <= d_j+1(v)`.
- Dla każdego wierzchołka zachodzi d(v) = d\_n-1(v). Ponieważ jeśli nie, to musimy mieć ujemny cykl, sprzeczne z założeniem.  Po n-1 iteracjach zewnętrznej pętli:
  - `d(v) <= dist[v] <= d_n+1(v) = d(v)`, więc `dist[v] = d(v)`. CNU.

Złożoność O(nm);

## Dijkstra

Zakłada brak nieujemnych krawędzi

```python
function Dijkstra (G, s){
	dist[v] = new int[n];
	for(int i = 0; i < n; i++){
		dist[v] = inf;
	}
	dist[s] = 0;
	
	var q = new queue<int,int>(dist);
	
	q.add(s);
	
	while(q.count > 0){
		var u = q.pop();
		foreach(var v in g.outneighbors(u) in q){
			if(dist[v] > dist[u] + w(uv)){
				dist[v] = dist[u] + w(uv);
				q.decreasekey(v, dist[v]);
			}
		}
	}
	return dist;
}
```

### Dowód poprawności

W dowodzie wykorzystujemy dwa niezmienniki:

1. dla każdego w !< Q zachodzi `dist[w] = d(w)`
2. dla każdego w < Q `dist[w]` to długość najkrótszej ścieżki od s do w prowadzącej tylko przez wierzchołki spoza Q.

Dowód opiera się na indukcji wobec wykonań pętli while.

- przed pierwszym wykonaniem, wszystkie wierzchołki są w kolejce, więc nie da się sformułować ścieżki z wierzchołków poza kolejką
- 1 i 2 spełnione przed iteracją i; q i dist to stany przed iteracja petli, q' i dist' po iteracji.
- wpierw pokażemy że dla każdego w !< Q' spełniony jest warunek `dist[w] = d(w)`. Niech u to wierzchołek wybrany w tej iteracji pętli. `dist'[u] = dist[u]` ponieważ w pętli modyfikujemy tylko odległości sąsiadów u. Trzeba więc pokazać, że `dist[u] = d(u)`.
- Weźmy najkrótszą ścieżkę z s do u i nazwijmy ją P. Następnie zdefiniujmy x, czyli pierwszy wierzchołek na P, który jeszcze leży w Q. Niech ścieżka P' to odcinek między s a x. Z własności optymalnej podstruktury, P' musi być najkrótszą ścieżką do x (jeśli istnieje krótsza do x, to wtedy bierzemy ją i końcówkę z (P - P') i otrzymujemy szybsza ścieżkę do u, co jest sprzeczne z założeniem). Jako że x jest pierwszym wierzchołkiem na P w Q, to oznacza,że każdy wierzchołek przed nim jest już spoza Q. Wykorzystując niezmiennik drugi, wiemy więc że d(x) = w(P') = `dist[x]`. Na podstawie tego:
  \-  `d(u) <= dist[u] <= dist[x] = d(x) <= d(u)`
  \- pierwsza nierówność wynika z tego że `dist[u]` zawsze trzyma odległość jakiejś ścieżki z s do u, może nie najlepszej
  \- druga nierówność wynika z minimalności u w kolejce, jako że u było wybrane do wyjęcia to jest to spełnione
  \- trzecia nierówność wynika z tego, że dla nieujemnych krawędzi dystans najkrótszej ścieżki od v\_1 do v\_n jest zawsze <= dystansowi ścieżki v\_1 do v\_n+j;
  \- zatem `dist[u] = dist'[u] = d(u)` zatem niezmiennik pierwszy zachowany
- drugi niezmiennik można rozpatrzyć przypadkami. Niech u takie samo, a w leży jeszcze w Q. Wtedy:
  - nie ma krawędzi uw -> nie ma opcji na poprawienie odległości, niezmiennik zachowany przez brak zmiany
  - jest krawędź uw ale odległość `dist'[w] = dist[w]` -> wtedy uw to krawędź, ale tworzy gorsze połączenie z s do w niż rozpatrzone wcześniej => zachowanie niezmiennika przez brak zmiany stanu
  - jest krawędź uw i zmieniona jest odległość -> `dist'[w] < dist[w]` , jako że u zostało teraz wyjęte z kolejki i poprawił się stan, to oznacza że przez u istniała lepsza ścieżka do w, jako że u wyjęte to ścieżka prowadzi przez tylko wierzchołki spoza kolejki, niezmiennik zachowany.

## Floyd Warshall

Założenie o braku ujemnych cykli

```python
function FloydWarshall(G){
	dist = new int[n,n] {inf};
	
	for( i < V(G)){
		for(j < V(G)){
			if(ij < E(G)){
				dist[i,j] = w(ij);
			}
		}
	}
	
	for(v < V(G)){
		dist[v,v] = 0;
	}
	
	for(k < V(G)){
		for(i < V(G)){
			for(j < V(G)){
				if(dist[i,j] > dist[i,k] + dist[k,j]){
					dist[i,j] = dist[i,k] + dist[k,j];
				}
			}
		}
	}
}
```

Dowód to zadanie na potem

## Johnson

Założenie: nie ma cykli ujemnych

```python

function Johnson(G){
	tworze graf G'', który ma ekstra hiperwierzchołek, wszystkie jego krawedzie maja wage 0;
	h = BellmanFord(G');
	
	
	niech w'' to waga zdefiniowana jako
		w''(uv) = w(uv) + h[u] - h[v];
		
	for(int i = 0; i < n; i++){
		d_i = Dijkstra (G,w'');
		foreach(var v in V(G)){
			dist[i,v] = d_i[v] - h(i) + h(v);	
		}
	}
}
```

Dowód też będzie potem

![[obrazki/Pasted image 20260619220421.png]]

## ZAD 1

```python
function BellmanFord(G, s ,v){
	dist = new int[n];
	from = new int[n];
	
	for(int i = 0; i < n; i++){
		dist[i] = inf;
	}
	dist[s] = 0;
	
	for(int i = 1; i < n; i++){
		foreach(var e in E(G)){
			int u = e.From;
			int w = e.To;
			if(dist[w] > dist[u] + w(e)){
				dist[w] = dist[u] + w(e);
				from[w] = u;
			}
		}
	}
	if (dist[v] = inf)
		return null;
	
	order = new List<int>();
	
	temp = v;
	while(temp != s){
		order.add(temp);
		temp = from[temp];
	}
	order.add(s);

	return order.reverse();
}
```

## ZAD 2

```python
function FindNegativeCycle(G){
	G.AddVertex(n);
	
	for(int i = 0; i < n; i++){
		G.AddEdge(n,i, 0); // krawedz z n do i o wadze 0;
	}
	
	dist = new int[n+1];
	from = new int[n+1];
	
	for(int i = 0 ; i <=n ;i++){
		dist[i] = inf;
		from[i] = -1;
	}
	dist[n] = 0;
	
	for(int i = 1; i <= n; i++){
		foreach(var e in E(G)){
			var u = e.From;
			var v = e.To;
			if(dist[v] > dist[u] + w(uv)){
				dist[v] = dist[u] + w(uv);
				from[v] = u;
			}
		}
	}
	int x = -1;
	// realny check 
	HasNegativeCycle = false;
	foreach(var e in E(G)){
		var u = e.From;
		var v = e.To;
		if(dist[v] > dist[u] + w(uv)){
			from[v] = u;
			x = v;
			hasNegativeCycle = true;
			break;
		}
	}
	
	if(!HasNegativeCycle)
		return null;
	

	for(int i = 0; i < n; i++){
		x = from[x];
	}
	order = new list<int>();
	int v = x;
	
	do{
		order.add(x);
		x = from[x]
	} while(v != x);
	return order;
}
```

## ZAD 3

`A^k [i,j]`, ponieważ to jest `A^2[i,j] = ∑_x A[i,x] * A[x,j] `, czyli suma wszystkich połączeń z i do x i x do j. Potem schodząc na wyższe potęgi dostajemy odpowiedź dla dłuższych spacerów.

## ZAD 4

```python
function SortTopological(G, u ,v){
	order = new list<int>();
	visited = new int[n];
	
	for(int i = 0; i < n; i++){
		if(!visited(i))
			DFS(i);
	}

	dist = new int[n];
	for(int i = 0; i < n; i++){
		dist[i] = inf;
	}

	dist[u] = 0;

	foreach(var w in order){
		if(dist[w] == inf) continue;
		foreach(var p in N(w)){
			if(dist[p] > dist[w] + w(wp))
				dist[p] = dist[w] + w(wp);
		}
	}

	return dist[v];
	
}

function DFS(v){
	visited[v] = true;
	foreach(var u in N(v)){
		if(!visited[u]){
			DFS(u);
		}
	}
	order.add(v);
}

```

## ZAD 7

Dowód Floyda Warshalla, wpierw algortym

```python
function FloydWarshall(G){
	dist = new int[n,n];
	
	for(int i = 0; i < n; i++){
		for(int j = 0; j < n; j++){
			dist[i,j] = inf;
		}
		dist[i,i] = 0;
	}
	
	foreach(var e in E(G)){
		dist[e.from,e.to] = e.weight;
	}
	
	for(int k = 0; k < n; k++){
		for(int i = 0; i < n; i++){
			for(int j = 0; j < n; j++){
				if(dist[i,j] > dist[i,k] + dist[k,j])
					dist[i,j] = dist[i,k] + dist[k,j];
			}
		}
	}
	return dist;
}
```

Dowód poprawności indukcyjny.
Niezmiennik: `dist[i,j] <= d_l(i,j)`. Pokażę, że jest zachowany przed i po każdej iteracji pętli zewnętrznej.
Baza: `dist[i,j] <= d_0(i,j)` , zbiór pusty, więc nie ma żadnych wewnętrznych wierzchołków w ścieżce z i do j. Jako że tablica jest zainicjalizowana długościami każdej krawędzi, to jeśli istnieje połączenie i-j, znajduje się w tablicy przed pierwszą iteracją pętli z k, spełnione.
Krok indukcyjny (względem ilości wykonań pętli zewnętrznej): Niech `dist` to stan tablicy przed wykonaniem l'tej iteracji, a `dist'` to stan po wykonaniu l'tej iteracji. Możliwe są przypadki:

- `d_l(i,j) = d_l-1(i,j)`. Oznacza to, że dodanie l-1 wierzchołka do puli możliwych wierzchołków wewnętrznych ścieżki między i a j nie poprawiło wyniku. Możemy założyć, że `dist'[i,j] <= dist[i,j]` bo kod nigdy nie zwiększa wyniku. W takim wypadku:
  `dist'[i,j] <= dist[i,j] <= d_l-1(i,j) = d_l(i,j)` czyli warunek spełniony.
- `d_l(i,j) < d_l-1(i,j)`. Dodanie wierzchołka l-1 do puli wierzchołków wewnętrznych poprawi wynik. Jako że `d_l(i,j)` to długość najkrótszej ścieżki od i do j a właśnie założyliśmy że zawiera l-1 wierzchołek wewnątrz, to:
  `d_l(i,j) = d_l-1(i,l-1) + d_l-1(l-1,j)`, dlatego że ścieżki od i do l-1 zawierają tylko pulę wierzchołków {0, ... , l-2} wewnątrz ścieżki. Jako że ścieżka od i do j używa l-1 dokładnie raz (brak ujemnych cykli), to dzieli w taki sposób ścieżkę i do j na dwie części.  Z tego wynika:
  `dist'[i,j] = dist[i,l-1] + dist[l-1,j] <= d_l-1(i,l-1) + d_l-1(l-1, j) = d_l(i,j)`.
  Po n-1 iteracjach pętli otrzymujemy:
  `dist[i,j] <= d_n(i,j)`. d\_n(i,j) to najkrótsza ścieżka korzystająca z możliwie wszystkich wierzchołków wewnętrznych, więc `d_n(i,j) = d(i,j)`, co implikuje `dist[i,j] <= d(i,j)`.
  Jako że `dist[i,j]` w każdej iteracji pętli przetrzymuje odległość jakiejś ścieżki między i a j (
  na początku są to tylko wagi krawędzi lub zera i nieskończoności, a po każdej następnej iteracji każda relaksacja łączy te wyniki w dłuższe spacery/ścieżki, to wynik odpowiada jakiemuś spacerowi bądź ścieżce, jeszcze dokładniej:
- indukcja przed wykonaniem relaksacji:
  - baza, jeszcze nie wykonana żadna relaksacja, jedyne skonczone `dist[i,j]` to 0, lub wagi krawędzi dla wierzchołków połączonych ze sobą, więc `dist[i,j] >= d(i,j)`
  - krok indukcyjny: weźmy dowolne `(i,j)` przed wykonaniem relaksacji z wierzchołkiem `k`. Tylko `(i,j)` relaksowane, więc inne pary spełniają ten warunek.
  - `dist[i,j] = dist[i,k] + dist[k,j] >= d(i,k) + d(k,j)`.
  - Weźmy najkrótszą ścieżkę z i do j. Dla każdego k `d(i,k) + d(k,j) >= d(i,j)`, ponieważ najkrótsza ścieżka może prowadzić przez k (wtedy k dzieli ścieżke na właśnie te dwie długości), albo może być krótsza. Więc:
  - `dist[i,j] = dist[i,k] + dist[k,j] >= d(i,k) + d(k,j) >= d(i,j)`.
  - Z indukcji spełnione dla każdej ilości wykonań relaksacji, więc spełnione na końcu algorytmu dla każdej pary wierzchołków.
    ), to automatycznie `d(i,j) <= dist[i,j] <= d(i,j)` co kończy dowód.
