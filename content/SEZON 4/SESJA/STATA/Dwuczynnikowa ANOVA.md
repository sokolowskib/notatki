Czy zmienna odpowiedzi zależy od dwóch czynników. Czynniki te nazwiemy A i B.

Interakcja to łączne oddziaływanie czynników na zmienną odpowiedzi. Jeśli średnia wartość zmiany odpowiedzi spowodowana zmianą jednego czynnika zależy od wartości drugiego czynnika, to wówczas są one w interakcji.

Dla dwuczynnikowej anovy wyprowadzamy model z interakcjami:
$Y_{ijm} = \mu + \alpha_i + \beta_j + \gamma_{ij} + \epsilon_{ijm}$
gdzie,
Y - wartość zmiennej odpowiedzi dla m-tej obserwacji w grupie, w której czynnik A jest na i-tym poziomie a czynnik B na j-tym poziomie
$\mu + \alpha_i + \beta_j + \gamma_{ij}$ - wartość średnia zmiennej odpowiedzi w grupie, w której czynnik A jest na i-tym poziomie a czynnik B na j-tym poziomie,
alfa - efekt czynnka a
beta - efekt czynnika B
gamma - interakcja miedzy i-tym poziomem czynnika A i j-tym poziomem czynnika B
epsilon - blad losowy

W obu modelach (z interakcjami i bez) zakładamy, że w każdej grupie (tzn. dla każdej z kl możliwych kombinacji poziomów czynników A i B) rozkład zmiennej odpowiedzi jest normalny z taką samą wariancją.
Założenie to implikuje, że epsilon ma rozkład normalny z jakąś wariancją i wartością oczekiwaną równą zero.

Ponadto aby wartości były określone jednoznacznie, to trzeba coś o nich założyć.

1. ![[SEZON 4/obrazki/Pasted image 20260625115529.png]]
2. ![[SEZON 4/obrazki/Pasted image 20260625115539.png]]

## Testowanie hipotez

1. Sprawdzamy czy A i B są w interakcji
   $H_{0AB}: \gamma_{ij} = 0$ dla każdego ij
   $H_{1AB}$  istnieje takie ij, że gamma nie jest równa zero (jest interakcja)
   Jeśli hipoteza zerowa przyjęta, to można potem osobno sprawdzać czy czynniki A i B mają wpływ na zmienną odpowiedzi. Jeśli natomiast zachodzi interakcja, to sens będzie miało badanie wpływów A i B tylko na niektórych poziomach czynników.

## Konstrukcja statystyk testowych

$SST = SSA + SSB + SSAB + SSE$

Potem tworzy się statystykę testową

$F_{AB} = \dfrac{\dfrac{1}{(k-1)(l-1)}SSAB}{\dfrac{1}{kl(n-1)}SSE}$

teraz znowu to będzia miało rozkład F-snedecora o stopniach swobody (k-1)(l-1) i kl(n-1)

Jeśli na poziomie istotności 1-$\alpha$ większe od kwantyla, to wtedy odrzucamy. Mniejsze to przyjmujemy.

## Eksperyment czynnikowy bez replikacji

Jeśli n = 1, czyli mamy tylko po jednej obserwacji w każdej grupie, to nie jesteśmy w stanie przeprowadzić testów w modelu z interakcjami, bo w mianowniku statystyk testowych pojawia się kl(n-1), czyli 0.
Można wtedy:

1. Narysować wykresy średnich. Jeśli łamane średnich okażą się w przybliżeniu równoległe, to będzie można uznać że nie ma interakcji między czynnikami A i B i używać modelu bez interakcji
2. ![[SEZON 4/obrazki/Pasted image 20260625121655.png]]

![[SEZON 4/obrazki/Pasted image 20260625121714.png]]
