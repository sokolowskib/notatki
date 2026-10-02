ANOVA - analysis of variance, procedura służąca do porównywania średnich w wielu grupach. Jednoczynnikowa ANOVA pozwala na testowanie hipotezy:
$H_0 : \mu_0 = \mu_1 = \mu_2 =  ...  = \mu_k$
przeciwko hipotezie:
$H_1 : istnieja \quad i\neq j \quad że\quad \mu_i \neq \mu_j$

Pozwala badać czy istnieje zależność pomiędzy pewnymi cechami - dokładniej czy zmienna zwana zmienną objaśnianą (lub zmienną odpowiedzi) zależy od tzw. zmiennej objaśnianej (nazwanej też czynnikiem).

## Przykład

Czy papier wytwarzany przez trzech producentów różni się pod względem jaskrawości (i jeśli tak, to kto®y z tych trzech producentów zapewnia największą jaskrawość); wtedy:

- zmienna objaśniana (odpowiedzi) to jaskrawość
- zmienna objaśniająca (czynnik) to producent czynnik ten występuje w trzech poziomach, producent 1,2,3
  Problem czy papier wytwarzany przez trzech producentów różni się pod względem jaskrawości, możemy rozwiązywać testując hipotezę
  $H_0 = \mu_1 = \mu_2 = \mu_3$
  przeciwko hipotezie przeciwnej (oczywista).

Powiedzmy, że te jaskrawości mają wartości jak w tabelce poniżej:
![[SEZON 4/obrazki/Pasted image 20260624214724.png]]

Żeby sprawdzić hipotezę, tworzymy model matematyczny:

$Y_{i,j} = \mu + \alpha_i + \epsilon_{ij}$
gdzie i = nr. producenta a j to obserwacja w i-tej grupie.

- $Y_{ij}$ to jaskrawość j-tej próbki papieru od i-tego producenta
- $\mu + \alpha_i$ to średnia jaskrawość papieru pochodzącego od i-tego producenta
- $\alpha_i$ to efekt i-tego producenta
- $\epsilon_{ij}$ to błąd losowy dla j-tej próbki papieru pochodzącej od i-tego producenta.

Ogólniej
$Y_{i,j} = \mu + \alpha_i + \epsilon_{ij}$
gdzie $i \in {1,...,k}$ oraz $j \in {1,...,n}$ gdzie k to liczba poziomów czynnika, a n to liczba obserwacji na każdym poziomie czynnika.

- $Y_{ij}$ to wartość zmiennej odpowiedzi dla j-tej obserwacji w i-tej grupie.
- $\mu + \alpha_i$ to wartość średnia zmiennej odpowiedzi w i-tej grupie.
- $\alpha_i$ to efekt w i-tej grupie
- $\epsilon_{ij}$ to błąd losowy dla j-tej obserwacji w i-tej grupie.

Jako że zakładamy że zmienne odpowiedzi mają tą samą wariancję, to ciągnie za sobą że $\epsilon_{ij}$ również mają rozkłady normalne o tej samej wariancji. Ponieważ pomiary wykonujemy niezależnie, to błędy też są niezależne o tym samym rozkładzie $\mathcal{N}(0,\sigma^2)$.

Ponadto założyliśmy plan zrównoważony, czyli że każdy czynnik ma tyle samo wykonanych obserwacji.

## Konwencje założeń przy ANOVIE

1. $\alpha_1 + \alpha_2 + ... + \alpha_k = 0$
   Wtedy $\mu$ interpretujemy jako ogólną wartość średnią zmiennej odpowiedzi a $\alpha_i$ to efekt działania i-tego poziomu czynnika względem średniej ogólnej.
2. $\alpha_1 = 0$
   Wtedy poziom nr. 1 przyjmujemy za poziom odniesienia a $\mu$ jest wartością średnią zmiennej odpowiedzi w grupie, która jest poziomem odniesienia, a $\alpha_i$ to efekt działania i-tego poziomu czynnika względem poziomu czynnika nr. 1

Niezależnie od wybranej konwencji weryfikujemy hipotezę
$H_0 : \mu_1 = ...=\mu_k \leftrightarrow H_0: \alpha_1 = ... = \alpha_k = 0$
przeciwko hipotezie odwrotnej(oczywistej).

## Konstrukcja testu do weryfikacji hipotez

Jeśli hipoteza zerowa jest prawdziwa i dla każdego poziomu czynnika zmienna odpowiedz ma tą samą wariancję, to wewnątrzgrupowe rozproszenie obserwacji (zmienność wewnątrzgrupowa) oraz międzygrupowe rozproszenie obserwacji będą w przybliżeniu równe.

Zmienność wewnątrzgrupowa -> $\dfrac{1}{k(n-1)}SSE$

Zmienność międzygrupowa ->$\dfrac{1}{k-1}SSA$

Ma zachodzić równość:
$SSE + SSA = SST$ gdzie SST to zmienność ogólna.
SSA(z tym ułamkiem) ma tendencję do przyjmowania większych wartości niż SSE(też z tym ułamkiem). Dlatego testem poprawności jest:
$F = \dfrac{\dfrac{1}{k-1}SSE}{\dfrac{1}{k(n-1)}SSE}$
Jeśli F jest duże to odrzucamy $H_0$, jeśli nie to przyjmujemy $H_0$. Jak to ocenić?

F ma podobny rozkład do F-Snedscore'a o k-1 i k(n-1) stopniach swobody.

Jeśli $F \ge f_{1-\alpha; k-1,k(n-1}$ to odrzucamy. Jeśli mniejsze, to nie mamy podstaw do odrzucenia.

Przed wykonaniem czegokolwiek trzeba sprawdzić założenia:

1. Czy dla każdego poziomu czynnika rozkład zmiennej odpowiedz jest normalny?
2. Czy wariancje w grupach są przynajmniej w przybliżeniu równe

## Porównania wielokrotne

Jeśli odrzucamy $H_0$ , to pytanie które $\mu$ są nierówne. Robimy porównania parami wykonując t-test, z nieznajomą, lecz równą wariancją.

![[SEZON 4/obrazki/Pasted image 20260625112735.png]]

Problem jak dobrać poziom istotności każdego testu żeby poziom istotności całej procedury był pożądany.
![[SEZON 4/obrazki/Pasted image 20260625112908.png]]
