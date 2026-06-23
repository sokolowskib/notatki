Budowanie sieci rezydualnej w przepływach:
Dla każdej krawędzi uv, danego przepłwu f oraz (G,c,s,t) zachodzi w sieci rezydualnej:

- `c_f(uw) = c(uw) - f(uw) jeśli uw < E(G) i wu !< E(G)`
- `c_f(uw) = f(wu) jeśli uw !< E(G) i wu < E(G)`
- `c_f(uw) = (c(uw) - f(uw)) + f(wu) gdy wu i uw <E(G)`

W tak zdefiniowanej sieci rezydualnej ścieżka powiększająca to każda ścieżka od s do t.
Do momentu aż istnieje ścieżka powiększająca w sieci rezydualnej dla danego f, to f nie jest maksymalne (cały czas można powiększyć f wzdłuż tej ścieżki, otrzymując większy przepływ).

## Ford Fulkerson

```python
function FordFulkerson((G,c,s,t)){
	f <- zerowy przepływ dla G
	(R,r) <- sieć rezydualna na G dla f
	while(istnieje ścieżka powiększająca P w R){
		r = min(v_i, v_i+1) w P
		powiększ f o a na wszystkich krawędziach P;
		Uaktualnij R o r na wszystkich krawędziach P;
	}
	return f;
}
```

Poprawność algorytmu wynika bezpośrednio z twierdzenia Forda Fulkersona.

### Twierdzenie Forda Fulkersona

Przepływ f jest maksymalny wtw. gdy nie istnieją dla niego żadne ścieżki powiększające w sieci rezydulanej.

Dowód przez sprzeczność. Powiedzmy że f to przepływ maksymalny, ale ma ścieżkę powiększającą P = {v\_1, ....., v\_n} . To oznacza, że istnieje ścieżka od s do t w sieci rezydualnej dla f. Zdefiniujmy f' jako:
\- f(uw) gdy uw !< P
\- f(uw) + r(P) gdy uw < P
Jako że powiększamy każdą krawędź ścieżki od v\_1 - v\_2 do v\_n-1 -v\_n, to zwiększyliśmy wartość przepływu (v\_1 = s, więc wypływa więcej w f' niż w f), oraz zachowane zostało prawo Kirchoffa (dla każdego v\_i v\_i+1 tyle o ile zwiększyliśmy przepływ, tyle wypłynie v\_i+1 v\_i+2), więc f' jest poprawnym przepływem o większej wartości niż f, sprzeczność.

Wadą algorytmu jest zależność złożoności czasowej od wartości maksymalnego przepływu.

## Edmond Karps

Zmodyfikowany Bellman Ford - zamiast szukać jakiejkolwiek ścieżki powiększającej, szukamy najkrótszej ścieżki powiększającej

```python
function EdmondKarps((G,c,s,t)){
	f <- zerowy przepływ
	(R,r) <- sieć rezydualna dla f
	while istnieje ścieżka P od s do t w R{
		P = {v1,....,vn}; // najkrótsza ścieżka
		a <- min( r(vi-vi+1));
		Powiększ f o a na wszystkich krawędziach P;
		Uaktualnij (R,r) o a na wszystkich krawędziach P;
	}
	return f
}
```

Dowód:

- `d_f(uv)` - odległość między u i v w sieci rezydualnej dla f

- `L_f` - liczba krawędzi, które należą do jakiejkolwiek najkrótszej ścieżki od s do t w sieci rezydualnej dla f

- f i f' - stan przed i po wykonaniu pętli while
  Wybierzmy jakąś najkrótszą ścieżkę od s do t. Wykonanie pętli while może usunąć krawędzie z wierzchołków bliższych do s do wierzchołków dalszych do s oraz dodać krawędzie z wierzchołków dalszych od s do wierzchołków bliższych do s. Nie ma więc możliwości znalezienia krótszej ścieżki po wykonaniu pętli.

- a) Dla każdego v zachodzi `d_f'(s,v) >= d_f(s,v)`. Załóżmy przeciwnie.

- Istnieje takie v, gdzie `d_f'(s,v) < d_f(s,v)`. Wybieramy takie v, które jest najbliższe do s, czyli ma najmniejsze `d_f'(s,v)`. Niech P' to ścieżka w sieci rezydualnej dla f' od s do v.

- w to ostatni wierzchołek przed v w P'. Dla niego `d_f'(s,w) = d_f'(s,v) - 1`

- Jako że w jest bliżej do s niż v (a v jest najbliższym wierzchołkiem zaprzeczającym tezie), to

- `d_f'(s,w) >= d_f(s,w)` Teraz istnieją dwa przypadki:
  - albo w->v istniało w sieci rezydualnej dla f, wtedy
    `d_f(s,v) <= d_f(s,w) + 1 <= d_f'(s,w) + 1 = d_f'(s,v)` bo w poprzedza v w P'
  - albo w->v nie istniało w sieci rezydualnej f. Nowe krawędzie powstają tylko wstecz wobec znalezionego P w while, więc v->w musiało leżeć na najkrótszej ścieżce od s do t, więc:
    `d_f(s,w) = d_f(s,v) + 1`, => `d_f(s,v) = d_f(s,w) - 1 <= d_f'(s,w)j - 1 = d_f'(s,v) - 2 < d_f'(s,v)`

- teraz pokażemy że jeśli  `d_f'(s,t) = d_f(s,t) to L_f' < L_f`.

- Rozważmy najkrótszą ścieżkę P' od s do t w sieci rezydualnej f'.

- P'= {w\_1, ...., w\_k}; Rozważmy ich odległości od s w sieci rezydualnej dla f.

- Może zachodzić:
  - `d_f(w_i+1) <= d_f(w_i) + 1` jeśli krawędź w\_i w\_i+1 istniała w SR dla f
  - `d_f(w_i+1) <= d_f(w_i) - 1`  jeśli krawędź w\_i w\_i+1 powstała w trakcie iteracji
  - Jako że `d_f(s,t) = d_f'(s,t)` to nie może w P' wchodzić żadna krawędź typu drugiego, bo wtedy długość byłaby inna
  - Zatem zbiór L\_f' < L\_f

- Jako że w trakcie iteracji L\_f' straciło jedną krawędź, to L\_f' < L\_f (ilościowo)

- W takim wypadku skoro d(s,t) nie maleje, a może przyjąć co najwyżej n wartości, a jako że dla jednej wartości d(s,t) pętla może wykonać się m razy(ponieważ każda ścieżka może składać się z co najwyżej m krawędzi, a liczba krawędzi maleje z każdą iteracją) to ogranicza się to w O(nm), BFS w każdej iteracji kosztuje O(m). Więc dla każdej wartości d (n) wykonujemy co najwyżej m pętli while, w której wywołujemy BFS, sumarycznie O(nm^2).

## Sieci rezydualne z kosztami

Dla (G,c,s,t) oraz funkcji kosztów k i przepływu f nazywamy graf skierowany R z funkcją wag c\_f oraz funkcją kosztów k\_f:

- c\_f(uw) = c(uw) - f(uw) i k\_f(uw) = k(uw) gdy uw < E(G) i wu \<! E(G)
- c\_f(uw) = f(wu) i k\_f(uw) = -k(wu) gdy uw \<! E(G) oraz wu < E(G)

## Twierdzenie o minimalnym koszcie

Niech f będzie maksymalnym przepływem z funkcją kosztu k. Wówczas f jest maksymalnym przepływem o minimalnym koszcie wtw. gdy odpowiadająca mu sieć rezydualna nie ma cyklu o ujemnym koszcie.
\=> ) Powiększenie przepływu wzdłuż ujemnego cyklu zmniejszy koszt.
Jeżeli powiększymy przepływ wzdłuż ujemnego cyklu to zmniejszymy koszt. Przepływ pozostaje o tej samej wartości, ale koszt się zmniejsza.
<= ) Niech f to przepływ maksymalny nie minimalizujący kosztów. Niech f' minimalizuje koszt o tym samym przepływie co f. f' minimalizuje ilość różniących się z f krawędzi. Niech S to zbiór krawędzi gdzie {uw: f(uw) < f'(uw) albo f(wu) > f'(wu)} (albo mniej przesyłam albo mniej zwracam, czyli albo wysyłam za mało albo cofam za dużo). S'  = {wu : $uw \in S$}
Z prawa kirchoffa $f_+(v) - f_-(v) = f'_+(v) - f'_-(v)$ co daje $f_+(v) - f'_+(v) = f_-(v) - f'_-(v)$ .
Jeśli do v wchodzi jakaś krawędź z S, to pierwsza strona równania jest większa niż 0, więc musi też wychodzić z niego krawędź z S. W S musi być cykl, ponieważ jest niepuste ponieważ f' i f się różnią oraz spełniony jest powyższy warunek. Oznaczmy go jako C. Przez C' oznaczamy odwrócenie tego cyklu, który musi leżeć w S' z definicji.

1. C to cykl dodatni. Jeżeli uv jest dodatnia, to w C' będzie ujemna. Suma kosztów w C' będzie wtedy ujemna. Wtedy znaleźliśmy cykl ujemny w sieci rezydualnej f', co jest sprzeczne
2. C to cykl zerowy. C' też zerowy. Wtedy można wziąć przepływ f'', tak że można zwiększyć wartość przepływu na f' o tyle ile maksymalnie się da. Skoro f'' zmienia się na cyklu, to ma ten sam koszt. Nasyciliśmy w ten sposób jakąś krawędź z C, co oznacza że nie należy ona do sieci rezydualnej f'', co oznacza że ta nasycona krawędź nie znajdzie się w zrekonstruowanym zbiorze S i S''. Na tej krawędzi f i f'' się nie różnią, co jest sprzeczne z minimalnością różniących się krawędzi f i f', sprzeczność.

```python
function cycleCancelling(G,c,s,t){
	f<- maksymalny przeplyw dla (G,c,s,t);
	(R,r) <- siec rezydualna dla f;
	while istnieje ujemny cykl C w R,k:
		a <- min r(v_i v_i+1) dla kazdego v_i nalezacego do C
		zwiekszamy kazda krawedz w C o a;
		aktualizujemy kazda krawedz z C w R o a;
	zwroc f
}
```

## Do dokończenia

![[obrazki/Pasted image 20260620153738.png]]

## ZAD 1

![[obrazki/Pasted image 20260620155110.png]]

## ZAD 2

a) Ford fulkerson ma złożoność O(mF\_max); k-krotny podpodział wydłuży działanie algorytmu k+1 krotnie, ponieważ z każdej krawędzi robi się k+1 krawędzi, F\_max pozostaje takie same, T => (k+1) T
b) Pomimo tego że każda przepustowość jest k razy większa, to jedna iteracja pętli while złapie teraz k razy większą wartość. Powiększenie przepustowości każdej krawędzi nie wpłynie na czas działania algorytmu forda fulkersona.

## ZAD 3

```python

function FindPaths(G, v, w){
	(G,c,s,t) <- tupla uzyskana po grafie G, w kazda waga krawedzi 1, s = v, t = w, każdy wierzchołek rozdzielamy na v_in oraz v_out z c = 1, każda inna krawędź ma cap = inf.
	f <- zerowy przeplyw
	(R,r) <- siec rezydualna dla f
	while istnieje sciezka powiekszajaca P od s do t w R{
		P = {v_1, ..., v_n};
		a = min_{P}r(v_i, v_i+1);
		
		powieksz f o a dla każdej krawędzi w P;
		zaktualizuj R o a dla każdej krawędzi w R;
	}
	zwroc f;
}
```

Dlaczego to zadziała: To jest twierdzenie mengera ale na przepływach. Wg. twierdzenia mengera ilość tych ścieżek <=> ilość wierzchołków potrzebnych do rozłączenia v od w. Ilość tych rozłącznych ścieżek to dla sieci przepływowej będzie tak naprawdę mincap, co jest równe maxflow. Poprawność algorytmu wynika z twierdzenia forda fulkersona.

## ZAD 4

Do znalezienia jest mincut, min\_cut = max\_flow z MD2.

```python
function FF((G,c,s,t)){
	f<- zerowy przeplyw
	(R,r)<- siec rezydualna dla f
	
	while istnieje P powiekszajaca z s do t w R:
		a = min(v_i , v_i+1) z P;
		powieksz f o a na każdej krawędzi z P
		zmień R o a na każdej krawędzi z P

	int visited = new int[n];
	BFS(s);
	
	S = visited == true;
	T = visited == false;
	return {(a,b) < E(G) : a < S, b <- T};
}

function BFS(s){
	q = new queue<int>();
	visited[s] = true;
	q.add(s);
	
	while(q.count> 0){
		var w = q.pop();
		foreach(var u in N_R(w)){ // tutaj po R operuje
			if(!visited[u])
				visited[u] = true;
				q.add(u);
		}
	}
}

```

## ZAD 5

a) znajdź przepływ blokujący

```python

function constructLevel(G, s, t){
	dist = new int[n];
	visited = new bool[n];
	q = new queue<int>();
	dist[s] = 0;
	visited[s] = true;
	
	q.push(s);
	while(q.count > 0){
		var w = q.pop();
		foreach(var u in N(w)){
			if(!visited[u] && !dead[u]){
				visited[u] = true;
				dist[u] = dist[w] + 1;
			}
		}
	}
}

function FindBlockingFlow(G,c,s,t){
	dead = new bool[n];
	constructLevel(G,c,s,t);
	d = dist[t];
	
	


}
```
