$(\Omega, \mathcal{F}, P)$ to przestrzeń probabilistyczna. Zmienną losową nazywamy dowolną funkcję X: $\Omega => R$
która spełnia warunek:
$\{\omega : X(\omega) \le a\} \in \mathcal{F}$ dla każdego $a\in R$ . Jak coś spełnia ten warunek, to jest mierzalne.

Rozróżniamy:

1. zmienne losowe dyskretne -> przyjmują one skończoną liczbę wartości.
2. zmienne losowe absolutnie ciągłe -> przyjmują nieprzeliczalną liczbę wartości ich rozkład da się opisać funkcją gęstości
   $P(X\in A) = \int_A f(x) dx$ dla dowolnego $A \in \mathcal{B(\mathbb{R})}$  .

## Własności funkcji gęstości:

1. f(x) >= 0
2. $\int_R f(x)dx = 1$

**Dla zmiennych absolutnie ciągłych mamy $P(X = a) = \int_{\{a\}}f(x)dx = 0$ , co nie musi być prawdą.**

## Wielkości opisujące zmienne losowe

### Dystrybuanta

$F(x) = P(X \le x)$ . Jeśli X to zmienna losowa dyskretna, to jej dystrybuanta jest funkcją schodkową, prawostronnie ciągłą. Jeśli jest absolutnie ciągła, to:
$F(t) = P(X \le t) = \int_{-\infty}^t f(x)dx$
Dystrybuanta jest:

1. niemalejąca
2. co najmniej prawostronnie ciągła
3. dąży do zera po lewej i do 1 po prawej stronie.
   Każda funkcja spełniająca te własności jest dystrybuantą jakiejś zmiennej losowej.

### Ogon dystrybuanty

1- F(x) = P(X > x)

### Kwantyl rzędu $\alpha \in (0,1)$

$q_{\alpha}$ to najmniejsza liczba spełniająca $F(q_{\alpha}) >= \alpha$

### Wartość oczekiwana

1. dla zmiennych dyskretnych
   $EX = \sum_k x_kp_k$
2. dla zmiennych absolutnie ciągłych
   $\int_{\mathbb{R}}xf(x)dx$
   Jak wartość oczekiwana jest równa $+/- \infty$ to mówimy że nie istnieje.
   Własności wartości oczekiwanej:

- E(a) = a
- E(aX) = aE(X)
- E(X + Y) = E(X) + E(Y)
- jeśli X <= Y  to EX <= EY

### Wariancja

$Var(X) =E((X-EX)^2) = E(X^2) - (EX)^2$
gdzie $E(X^2)$ to
$\sum_k x_k^2p_k$
dla zmiennych losowych dyskretnych oraz
$\int_{\mathbb{R}}x^kf(x)dx$
dla zmiennych losowych absolutnie ciągłych.
Szczególnie:
$E(g(X)) = \sum_k g(x)p_k \quad lub \int_{\mathbb{R}}g(x)f(x)dx$
zależnie czy jest zmienna losowa abs. ciągła czy dyskretna.

Własności wariancji:

1. jeśli $a,b \in \mathbb{R}$ i Var(X) istnieje, to $Var(aX + b) = a^2Var(X), Var(b) = 0$
2. dla każdej X, że Var(X) istnieje, Var(X) >= 0

### Odchylenie standardowe

$\sigma = \sqrt{Var(X)}$
Przykłady rozkładów na kartce.
