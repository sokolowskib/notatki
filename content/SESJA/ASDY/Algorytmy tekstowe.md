## Wyszukanie wzorca w tekście

x - napis długości m (wzorzec)
y - napis długości n (tekst)

### Naiwne przeszukiwanie

```python
function naive(x,y){
	bool ok = true;
	for(int i = 0; i < n - m; i++){
		for(j = 0; j < m; j++){
			if(y[i +m] != x[m])
				ok = false;
		}
		if(ok)
			result.add((i,i+m-1));
	}
}
```

To jest chujowe bo jest O(n^2) pesymistycznie, ale jeśli alfabet jest duży, a wzorzec i tekst losowy, to średnio wykonywane są tylko dwa porównania. Czas optymistyczny jest więc lepszy, w granicach O(n).

## Algorytm KMP

Działa na zasadzie tablicy prefikso-sufiksów.

![[obrazki/Pasted image 20260621224402.png]]

```python

function KMP(x,y){
	P = new int[m+1];
	result = new list<int>()
	P <- ComputeP(x)
	
	
	int j = 0;
	for(int i= 0; i < n-m; i+= max(j - P[j],1))
		j = P[j];
		while j<m && y[i+j] == x[j] do
			j++;
		if j == m
			result.add(i);

}


function ComputeP(x){
	P[0] = P[1] = t = 0;
	for(int j = 2; j <=m; j++){
		while t > 0 && x[t] != x[j-1]
			t = P[t];
		if x[t] == x[j-1]
			t++;
		P[j] = t;
	}
}

```

Jeżeli $y[i: i + j + 1] = x[0 : j-1]$ oraz $y[i + j] != x[j]$ (czyli psuje się na j'tej pozycji), to dla żadnego k, takiego że i <= k <= i + j - P\[j], zachodzi y\[k : k + m - 1] != x

### Poprawność algorytmu ComputeP W j-tej komórce znajduje się długość NWPS j pierwszych znaków napisu x.

Wykorzystuje lemat: Prefikso-sufiks poprawnego prefikso-sufiksu słowa, jest jego prefikso-sufiksem. Jeśli mamy słowo w, oraz jego dwa prefikso - sufiksy, s<- najdłuższe, u <- krótsze.

Jako że s jest prefiksem w oraz u jest prefiksem w, to u jest też prefiksem s. Jako że s jest sufiksem w oraz u jest sufiksem w, ale s jest dłuższe, to oznacza że u jest sufiksem s => u jest prefikso-sufiksem s.

Dowód: Indukcja po j.

Baza: Długość NWPS dla j = 0 i j = 1 jest równa zero z definicji, spełniona
Krok indukcyjny: Zakładamy, że `P[j-1]` zawiera długość NWPS dla j-1 pierwszych znaków x.
$x' = x[0,..,j-2], T = NWPS(x'),  c=x[j-1]$. W pętli while iterujemy się po prefikso-sufiksach x', z założenia indukcyjnego są wyznaczone poprawnie. Po zakończeniu pętli otrzymujemy T'.

1. Jeśli T' jest słowem pustym, to :
   - $P[j] = 1 \quad jeśli \quad x[0] = c$
   - $P[j] = 0 \quad jeśli \quad x[0] \ne c$
     Zatem zawartość $P[j]$ jest wyznaczona poprawnie.
2. Jeśli T' ma długość t, oraz $T' = x[0, ..., t-1]$ oraz ma długość t, to z warunku pętli $x[t] != x[j-1]$ można wnioskować że $x[t] = c$. Jako że T'c jest poprawnie wyznaczonym prefikso-sufiksem x'c. Ponieważ prefikso-sufiksy rozpatrzaliśmy od najdłuższego, jest to NWPS napisu x'c. Najdłuższy prefikso-sufiks słowa x to najdłuższy prefikso-sufiks x' długości t spełniający zależność
   $x[t] = x[j-1]$.

### Czas działania ComputeP

Koszt zamortyzowany jednej pętli for jest t, wykonujemy tylko t++.
Jedyna operacja zmniejszająca t to t = $P[t]$ , każda taka operacja zmniejsza t o $t - P[t]$.
Ponieważ t nigdy nie jest ujemne, operacja $t = P[t]$ wykona się co najwyżej m-1 razy, więc koszt zamortyzowany jest stały, ponieważ wykona się m iteracji pętli i w puli iteracji jest max m-1 operacji $t - P[t]$, czyli O(m).

### Czas działania KMP

Porównań negatywnych może być co najwyżej n - m + 1 (każda próba jest przerywana po pierwszej niezgodności). Porównań pozytywnych jest tyle, ile wykona się j++. Wyrażenie indeksowe i+j nie maleje, ponieważ:

- pomyślne - rośnie o 1
- niepomyślne - w kolejnej iteracji pętli zewnętrzenej `i' + j' >= i + (j - P[j]) + P[j] = i +j`
- Ponieważ wyrażenie i+j ma wartość początkową 0 i jest ograniczone przez n, to operacji ++j zwiększających wartość jest co najwyżej O(n).

## Naiwny w tył

```python
function NaiveStringSearch(x,y){
	for(int i = 0; i <= n-m; i++){
		j = m-1
		
		while j>= 0 && y[i +j] = x[j]
			--j;
		if(j == -1)
			dodaj i do listy wynikow
	}
}
```

## Algorytm Karpa - Rabina

```python
function KarpRabin(x,y){
	bm = b do potęgi (m-1)
	hw = skrót wzorca
	h = skrót y[0:m-1]
	
	for(int i = 0; i < n-m; i++){
		if h == hw
			if y[i : i + m -1 ] == x
				dodaj i do listy wyników
		
		h = (h - y[i] *bm) * b + y[i+m];
	}

}
```

b<- podstawa numeracji

![[obrazki/Pasted image 20260622212841.png]]

## Zad 1

Konkatenujesz wzorzec z napisem
x # y
Potem robisz ComputeP dla kazdej dlugosci.

x # ........... x, ........
|, tutaj dlugosc tekstu np. l, dla tego momentu compute p wskaze dlugosc wzorca, wiec to bedzie najdluzszy prefiks.

## Zad 2

## Zad 3

procedure MAX\_POWER ( x : napis )

m = | x |

P = COMPUTE\_P ( x ) // Wyznaczenie tablicy LPS w czasie O ( m )

okres = m - P \[ m ] // Wyliczenie najkr ó tszego mo ż liwego okresu

if m mod okres == 0 then

return m / okres // S ł owo dzieli si ę na r ó wne bloki

else

return 1 // S ł owo nie jest pot ę g ą ż adnego kr ó tszego pods ł

owa

## Zad 4

Dzielimy wzorzec po znaku "?" na k-1 podwzorców. Tworzymy tabelę n x (k+1) elementową, gdzie (i,j) oznacza czy na i-tej pozycji tekstu znajduje się jty podwzorzec. Korzystając z KMP, wyszukujemy każdy podwzorzec w tekście i zapisujemy w tablicy odpowiednią informację o jego obecności. Dla pustych podwzorców wypełniamy cały wiersz informacją o tym, że znaleźliśmy podwzorzec. Następnie przeglądamy wygenerowaną tablicę w celu znalezienia wystąpienia całego wzorca. PO znalezieniu wystąpienia pierwszego podwzorca, czyli np na pozycji (i\_0, 0), przesuwamy się o jeden wiersz w dół i długość podwzorca plus jeden w prawo, to znaczy w miejsce gdzie spodziewamy się dopasowania kolejnego podzworca. RObimy tak aż do znalezienia ostatniego podwzorca, gdyż wtedy dopasowaliśmy cały wzorzec. Jeśli którykolwiek podwzorzec po drodze nie został znaleziony, to przerywamy operację i przechodzimy do kolejnego wystąpienia pierwszego podwzorca (i\_i, 0) i powtarzamy szukanie.

KMP k+1 razy, potem przejscie po tabeli też maksymalnie O(nk)
