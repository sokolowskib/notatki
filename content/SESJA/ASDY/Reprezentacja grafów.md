## Macierz sąsiedztwa

Plusy:

- dodanie / usuwanie / sprawdzenie istnienia krawędzi w O(1)
  Wady:
- złożoność pamięciowa O(n^2)
- przetworzenie wszystkich krawędzi incydentnych z wierzchołkiem w O(n)

## Lista Sąsiedztwa

Plusy:

- złożoność pamięciowa O(m) a nie O(n^2)
- Przetwarzanie krawędzi incydentnych z wierzchołkiem w O(d)
  Wady:
- Dostęp do krawędzi (sprawdzenie) pesymistycznie w O(d)

## DFS

```python
function DFS(G, v){
	visited[v] = true;
	foreach(var u in N(v)){
		if(!visited[u]){
			DFS(G, u)
		}
	}
}
```

## BFS

```python
function BFS(v){
	q = new Queue<int>();
	q.add(v);
	visited[v] = true;
	while(q.count > 0){
		int w = q.pop();
		foreach(var u in N(w)){
			if(!visited[u]){
				q.add(u);
				visited[u] = true;
			}
		}
	}
}
```

jest w kolejce <=> jest oznaczony visited

![[obrazki/Pasted image 20260619140638.png]]

## ZAD 1

a) Lista sąsiedztwa zapewni zarówno przejrzenie krawędzi wychodzących z `v` w czasie O(deg(v)) jak i złożoność pamięciową O(m). Natomiast operacje na pojedynczej krawędzi w O(log(n)) musi przyspieszyć dodatkowa struktura, np. lista sąsiedztwa w postaci drzewa zrównoważonego, np. AVL. Można trzymać drzewo zamiast listy;
b) Lista sąsiedztwa trzymana w postaci listy łączonej, insert na początku w O(1), przejście przez linked listę w O(deg(v)), złożoność pamięciowa O(m).
c) Jako że nie ma wymogów pamięciowych poza macierzą sąsiedztwa można obok trzymać listę sąsiedztwa, oba warunki spełnione
d) Potrzebne są dwie struktury:

- lista sąsiedztwa -> rozwiązuje przejrzenie wszystkich sąsiadów w O(d), pamięciowo O(m)
- tablica hashująca -> trzyma zapisane tuple (u,v),  które reprezentują istnienie krawędzi w grafie, poza tym ma funkcję hashującą. Optymistycznie insert + checkw O(1), jeśli nie ma kolizji hashy. W węźle, w którym przetrzymywana jest ta tupla znajduje się wskaźnik na odpowiednik w liście sąsiedztwa, co pozwala na delete w O(1).

## ZAD 2

```python
function nonRecDFS(v){
	var s = new Stack<int>();
	s.add(v);
	
	while(s.count > 0){
		var w = s.pop();
		if(visited[w]) continue;
		visited[w] = true;
		foreach(var u in N(w)){
			if(!visited[u]){
				s.add(u);
			}
		}
	}
}
```

tutaj markowanie na visited musi być poza pętla dla sąsiadów, wierzchołek ma prawo być wielokrotnie w stacku ale musi zostać sprawdzony

## ZAD 3

```python
function BFS(v){
	visited[v] = true;
	var q = new queue<int>();
	q.push(v);
	while (q.count > 0){
		var w = q.pop();
		foreach(var u in N(w)){
			if(!visited[u]){
				q.add(u);
				visited[u] = true;
			}
		}
	}
}
```

## ZAD 4

Rozwiązanie to odpalenie BFS z każdego wierzchołka. Odpalając z każdego nowego wierzchołka dodajemy jedną spójną składową (jeśli BFS się zakończył a wierzchołek nie był odwiedzony, to oznacza że niemożliwe było dotarcie do niego z poprzedniego wierzchołka startowego => nowy wierzchołek znajduje się w innej spójnej składowej).

```python
function ZnajdzIloscSpojnychSkladowych(G){
	visited = new bool[n];
	int count = 0;
	for(int i = 0; i < n; i++){
		if(!visited[i]){
			count++;
			BFS(i);
		}
	}
	return count;
}

function BFS(v){
	var q = new queue<int>();
	q.push(v);
	visited[v] = true;
	while(q.count > 0){
		var w = q.pop();
		foreach(var u in N(w)){
			if(!visited[u]){
				visited[u] = true;
				q.add(u);
			}
		}
	}
}
```

Każdy wierzchołek jest dokładany do kolejki dokładnie raz. Dlatego że BFSY dzielą tabelę visited, pokrywają każdą składową dokładnie raz. O(m);

## ZAD 5

Graf jest dwudzielny < = > Graf jest dwukolorowalny

```python
function ZnajdzCzyDwudzielny(G){
	c = new int[n];
	foreach(int i = 0; i < n; i++){
		if(c[i] == 0){
			if(DFSColor(i, 1) == false)
				return false;
		}
	}
	return true;
}


function DFSColor(v, color){
	int other = color == 1 ? 2 : 1;
	
	c[v] = color;
	foreach(var u in N(v)){
		if(c[u] == 0){
			if(DFSColor(u,other) == false) 
				return false;
		}
		else if(c[u] == color)
			return false;
	}
	return true;
}

```

Każdy wierzchołek koloruję dokładnie raz, ilość krawędzi odwiedzonych pomnożona przez stałą. O(n + m);

## ZAD 6

Na moje to jest po prostu DFS z post-order insertem

```python
function TopologicalSort(G){
	order = new list<int>
	visited = new int[n];
	for(int i = 0; i < n; i++){
		if(!visited[i]){
			DFS(i);
		}
	}
	return order.reverse();
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

```python

function findEulerCycle(G){
	var order = new list<int>();
	start = -1;
	for(int i = 0; i < n; i++){
		if(g.degree(i) > 0)
			start = i;
	}
	int used = 0;
	int m = g.Edgecount;
	
	if(start == -1)
		return null;

	DFS(start);
	
	if(used == m){
		return order;
	}
	else 
		return null;
}

function DFS(v){
	visited[v] = true;
	foreach(var e in N(v)){
		var u = e.To;
		G.remove(e);
		used++;
		DFS(u);
	}
	order.add(v);
}
```
