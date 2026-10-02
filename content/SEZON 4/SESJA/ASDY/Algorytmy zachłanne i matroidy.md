Matroid - para (S, _A_) taka, że:

- S jest niepustym zbiorem skończonym, _A_ jest niepustą rodziną podzbiorów S.
- Dla każdych $A \in \mathcal{A} \land B \subset A$ zachodzi $B \in \mathcal{A}$ .
- Dla każdych $A,B \in \mathcal{A} : |B| > |A|\quad \exists x \in B-A  \implies A \cup \{x\} \in \mathcal{A}$

Elementy $\mathcal{A}$ nazywamy zbiorami niezależnymi, a zbiory niezależne o maksymalnej liczbie elementów nazywamy bazą.

```python

function Greedy(([n], A) : matroid, w:[n] => R){
	posortuj elementy tak, aby (w(1) >= w(2) >= w(3) .....);
	
	R <-{ } ;
	for(int i = 1; i <=n ; i++){
		if(R + i < A){
			R += i;
		}
	}
}
```

### Dowód

$R_i$ - stan zbioru po i iteracjach głównej pętli

Dla każdego i istnieje optymalna baza $B_i$ taka, że $R_i \subseteq B$ , oraz $B - R_i$ jest podzbiorem zbioru {$i+1, i+2, ..., n$};

- baza: R puste, więc $R_0 \subset  B$ , a B - $R_0$ zawiera wszystkie wierzchołki.
- Przypuśćmy, że prawda dla i - 1 i rozpatrzmy przypadki:
  - i nie zostało wzięte do R.  Wtedy $R_i - \cup \{i\}$ nie jest niezależne. $B_i = B_i-1$.
  - i zostało wzięte do R. Wtedy znowu są przypadki:
    - $i \in B_i-1$ , wtedy $B_i = B_i-1$.
    - $i \notin B_i-1$ . Wtedy tworzymy bazę B' wielokrotnie wykonując:
      ```python
      	C = R_i;
      	while (|C| < |B_i|){
      		x = B_i - C; z aksjomatu wymiany
      		C = C + x;
      	}
      ```
      Pod koniec tego procesu C staje się bazą, ponieważ osiąga maksymalny rozmiar.
      Przyjmujemy ją jako B'.
      $B' \subset R_i \cup B_i-1  = R_i-1 \cup \{i\} \cup B_{i-1} = B_{i-1} \cup \{i\}$\
      Jako że B' równe rozmiarami do B\_i-1, B' zawiera i oraz B\_i-1 nie zawiera {i}, to musi istnieć jakieś j, którego nie ma B' a jest w B\_i-1. j>i, ponieważ jeśli j zawierało się w B\_i-1, ale nie w B', a R\_i zawiera się w całości w B' oraz R\_i = R\_i-1 + i, to oznacza że j nie jest w R\_i-1, czyli jest w B\_i-1 - R\_i-1. Jako że j nie jest i, to musi należeć do zbioru {i+1, ..., n}, czyli jest większe.
  - Jako że w(B') = w(B\_i-1) + w(i) - w(j), a jako że i < j, więc w(i) > w(j), to baza B' jest optymalniejsza, czyli można wziąć B\_i = B'.

### Problem szeregowania zadań

Można go rozwiązać sposobem zachłannym.
`d : [n] => N` gdzie d(i) to deadline na ite zadanie, a `w: [n] => R+` to kara za wykonanie itego zadania po czasie. Szukana jest takie uszeregowanie zadań, które minimalizuje sumaryczną karę.

1. Istnieje optymalne rozwiązanie, gdzie każde zadanie po terminie jest oddane po wszystkich zadaniach przed terminem. Jeśli jest przeciwnie, można je bezkarnie zamienić
2. Wśród wszystkich zadań terminowych, da się je posortować niemalejąco względem deadline. Poraz kolejny, można je pozamieniać trywialnie.
   Zbiór zadań Z nazwiemy niezależnym, jeśli da się uszeregować je tak, aby wszystkie zadania były terminowe. $\mathcal{B}$ to rodzina wszystkich zbiorów Z. (Z, $\mathcal{B}$ ) to matroid, bo:

- Jeśli A niezależny , to $B \subseteq A$ też
- Aksjomat zachowany, indukcja po rozmiarze zbioru A. Jeśli $|A| < |B|$ , to wtedy:
  - rozpatrzmy ostanie zadanie w B jako x.
  - jeśli $x \in A$ to wtedy dla $A' = A - {x}$ oraz $B' = B - x$ , jako że A' i B' są o jeden element mniejsze, to z założenia indukcyjnego warunek spełniony dla A' i B', więc $\exists y \in B' - A' \land y!=x$ , więc $A' + y$ niezależne. Teraz przypisując x czas zakończenia k+1, wiemy że będzie ono wykonane terminowo, ponieważ jeśli k < m to k+1 <= m, więc A' + x jest niezależne. Na bazie indukcji prawdziwe
    Problem można rozwiązać na podstawie algorytmu zachłannego. maksymalizujemy ilość kar niezapłaconych, rozwiązując tutaj problem bazy matroidu.

Sortowanie kubełkowe w O(n), można przechowywać R w zrównoważonym drzewie binarnym, gdzie w każdym węźle dodatkowo trzymamy ilość potomków, więc O(nlog(n)) całość.
