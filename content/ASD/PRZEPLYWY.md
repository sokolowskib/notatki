```
1: procedure NaiveFlow((G, c, s, t): sieć przepływowa) 
2: f ← przepływ o wartości 0 na każdej krawędzi grafu G 
3: while istnieje ścieżka od s do t w G do 
4: P ← ścieżka od s do t w G 
5: a ← najmniejsza przepustowość krawędzi na ścieżce P 
6: for e ∈ E(P) do 
7: f(e)+ = a 
8: c(e)− = a 
9: zwróć f jako wynik
```


zad.2
instancja problemu wykonana w czasie T, a kazda krawedz ma k-krotny podpodzial

1  =====> 1-2-3....-k

a) (k+1) * T, ilosc krawedzi faktycznie sie zwieksza wiec przez kazda krawedz przechodzil ford fulkerson.
b) O(f_max * m), ale reszta w sciezce rezydualnej zwieksza sie tez k-krotnie, wiec taka sama zlozonosc. Taki sam czas


![[Pasted image 20260325172743.png]]
Menger ale z wersja forda fulkersona, v to s, w to t, 
max zbior wewnetrznie rozlacznych sciezek v-w to minimalny v-w separator. 
kroki:
	- 
	-