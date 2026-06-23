M jest skojarzeniem w G, gdy żadna krawędź z M nie ma wspólnego wierzchołka z żadną inną krawędzią.

### M jest największym skojarzeniem, jeśli nie ma ścieżki powiększającej

Dowód : MD2

## Hopcroft Karp

```python
function HopcroftKarp(G: graf dwudzielny){
	M = {}
	while istnieje ścieżka powiększająca{
		P <- zbiór najkrótszych wierzchołkowo rozłącznych ścieżek powiększających dla M
		
		M = M xor P;
	}
	return M;
}
```

### Lemat

Jeśli P to zbiór najkrótszych wierzchołkowo rozłącznych ścieżek powiększających względem M, to długość najkrótszej ścieżki powiększającej względem M xor P jest większa.

d = długość ścieżek z P
$\pi$ = długość najkrótszej ścieżki z M xor P
Stan krawędzi zmienia się tylko, jeśli należy ona do P, jeśli nie, to zostaje w M nienaruszona. Jeśli pi nie przecina żadnej ze ścieżek z P, to oznacza, że wszystkie krawędzie są nieruszone przez P, co oznacza że pi byłaby ścieżką powiększającą dla M. Musi więc się przecinać z P.
Nie wprost zakładamy, że $\pi$ jest <= d; Udowodniliśmy, że musi się przecinać ze ścieżkami z P.

Niech $\pi$ przecina się po kolei ze ścieżkami $P_1, P_2, ..$. Każde $P_i$ rozcinasz w punkcie przecięcia na dwie połówki. $\pi$ sklejasz tak:

1. $R_1$  - początek $\pi$ od startu do x\_1, + reszta P\_i
2. $R_l$  - początek (lewa część) $P_i-1$ do $x_i-1$ , potem kawałek $\pi$ od $x_i-1$ do $x_i$ + reszta $P_i$
3. $R_l+1$ - początek $P_l$ + reszta $\pi$.

![[obrazki/Pasted image 20260622140005.png]]

Każda ścieżka $R_i$ jest powiększająca względem M, ponieważ:

- dwa końce wolne, co zawsze spełnione z definicji $P_i$ oraz $\pi$
- różnica symetryczna dwóch skojarzeń. $M'' = (M xor P) xor \quad \pi.$.
  Jest to poprawne skojarzenie, bo $\pi$ jest powiększające względem M xor P. Wtedy:
  M xor M'' = M xor (M xor P xor $\pi$) = P xor $\pi$ .
  Zbiór elementów w ścieżkach R to różnica symetryczna dwóch skojarzeń.

Jako że długość $R_i$ musi być >= d z założenia. (l+1) \* d < e($\pi$) + dl

więc $e(\pi)$ > d, co kończy dowód.

### Lemat

Jeśli w grafie G najkrótsza ścieżka powiększająca względem skojarzenia M ma d krawędzi, wówczas najliczniejsze skojarzenie w G ma rozmiar co najwyżej |M| + n/d.

Jeśli M\* to najliczniejsze skojarzenie G, to M xor M\* zawiera nie więcej niż n/d+1 ścieżek powiększających.

M\* - M to liczba ścieżek powiększających M względem M xor M\* (bo M xor M\* to albo cykle, albo sciezki, na cyklach jest tyle samo w M i M\*, na sciezkach parzystych tyle samo co M i M\*, na nieparzystych jest jedna krawedz wiecej, z maksymalnosci M\* sa tylko takie co M\* ma wiecej).
Każda z tych ścieżek ma długość >= d, oraz k to ilość takich ścieżek to
$k(d+1) \le n \implies k \le n / (d+1)$ Po pierwszych $\sqrt(n)$ iteracjach najkrótsza ścieżka powiększająca ma długość przynajmniej $\sqrt(n)$. Kolejnych iteracji jest nie więcej niż $\sqrt(n)$, bo każda iteracja powiększa skojarzenie o conajmniej 1.

Maksymalny zbiór najkrótszych ścieżek powiększających znajdujemy w czasie O(m) (BFS z grafem warstwowym). O($\sqrt(n) m$)

## Najliczniejsze skojarzenie - Edmonds

```python

function Edmonds(G: graf dwudzielny){
	M = { }
	while istnieje ścieżka powiększająca 
		Znajdź P;
		M = M xor P;
	zwróć M
}
```

**Kielich** - cykl C o długości 2k+1 z k krawędziami skojarzenia taki, że istnieje parzysta ścieżka naprzemienna z wierzchołka C do wolnego wierzchołka.

Wykonaj BFS od wolnego wierzchołka, przechodząc po krawędziach szukając ścieżek naprzemiennych. Jeśli stworzyłeś kielich, ściągnij go do pojedynczego wierzchołka i kontynuuj przeszukiwanie. Jeśli dotrzesz do innego wolnego wierzchołka, cofnij ściągnięcia kielichów wzdłuż ścieżki i odtwórz ścieżkę powiększającą (?????).
