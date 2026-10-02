$\Omega$  - dowolny niepusty zbiór, który oznacza przestrzeń zdarzeń elementarnych.

Np.: Przy jednorazowym rzucie monetą $\Omega = \{O, R\}$

$\mathcal{F}$ - przestrzeń zdarzeń losowych, jest to $\sigma$ -ciało podzbiorów $\Omega$.

1. Jeśli $\Omega$ przeliczalna, to za $\mathcal{F}$  można przyjąć zbiór wszystkich podzbiorów $\Omega$ .
2. Jeśli $\Omega$ nieprzeliczalna, to bierze się wtedy $B(\Omega)$

## Definicja $\sigma$ -ciała

$\sigma$ -ciało podzbiorów $\Omega$ to rodzina $\mathcal{F}$ podzbiorów $\Omega$ spełniająca warunki:

1. $\Omega \in \mathcal{F}$
2. jeśli $A \in \mathcal{F} \implies A' = \Omega - A \in \mathcal{F}$
3. jeśli $A_1, A_2, ...... \in \mathcal{F} \implies A_1 \cup A_2 \cup .... \in \mathcal{F}$

## Definicja prawdopodobieństwa

Prawdopodobieństwo to funkcja P : $P : \mathcal{F} -> R$  spełniającą warunki:

1. P(A) >= 0 dla każdego $A \in \mathcal{F}$
2. $P(\Omega) = 1$
3. jeśli $A_1, A_2, .... \in \mathcal{F}$ są zdarzeniami parami rozłącznymi, to $P(A_1 \cup A_2 \cup ...) = \sum_{i=1}^{\infty}P(A_i)$

## Przestrzeń probabilistyczna

Nazywamy trójkę ($\Omega, \mathcal{F}, P$ ).

## Własności prawdopodobieństwa

1. P($\emptyset$ ) = 0
2. jeśli $A_1, A_2, ...., A_n \in \mathcal{F}$ są parami rozłączne, to: $P(A_1 \cup ...\cup A_n) = P(A_1) + ... + P(A_n)$
3. $P(A') = 1 - P(A)$
4. jeśli $A,B \in \mathcal{F}$ i $A \subset B$, to $P(A) \le P(B)$
5. jeśli $A_1, A_2, ... \in \mathcal{F}$ to $P(A_1 \cup A_2 ....) \le \sum_{i=1}^{\infty}P(A_i)$
6. jeśli $A,B \in \mathcal{F}$ , to $P(A \cup B) = P(A) + P(B) - P(A \cap B)$
7. ![[SEZON 4/obrazki/Pasted image 20260623211236.png]]

## Prawdopodobieństwo geometryczne

Przestrzeń zdarzeń elementarnych jest wtedy nieprzeliczalna.

$B(\Omega)$ - najmniejsze $\sigma$ -ciało do którego należą wszystkie otwarte podzbiory $\Omega$ .

P(A) = $\frac{miara(A)}{miara(\Omega)}$  dla każdego $A \in \Omega$

## Zdarzenia niezależne

A i B niezależne, gdy

$P(A \cap B) = P(A)P(B)$

## Prawdopodobieństwo warunkowe

Prawdopodobieństwo zdarzenia A pod warunkiem zajścia zdarzenia B, takiego że P(B) > 0 jest:
$P(A|B) = \dfrac{P(A \cap B)}{P(B)}$

## Prawdopodobieństwo całkowite

($\Omega, \mathcal{F}, P$) to przestrzeń probabilistyczna. Jak:

1. $B_1, B_2, ...., B_n \in \mathcal{F}$
2. $B_1 \cup B_2 \cup ... \cup B_n = \Omega$
3. $B_1, B_2, ..., B_n$ są parami rozłączne
4. prawdopodobieństwo wszystkich jest większe od 0
   to :
   $P(A) = \sum_{i=1}^n P(A|B_i)P(B_i)$

## Wzór Bayesa:

Jeśli $B_1, ..., B_n$ stanowią zupełny układ zdarzeń, zaś A jest dowolnym zdarzeniem o P(A) > 0, to
$P(B_k|A) = \dfrac{P(A|B_k)P(B_k)}{\sum_{i=1}^n P(A|B_i) P(B_i)}$
czyli prawdopodobieństwo na odwrócony ciąg przyczynowo skutkowy przez prawdopodobieństwo całkowite.
