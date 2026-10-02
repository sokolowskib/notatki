## Nierówność Czebyszewa

Jeśli zmienna losowa X ma skończoną wariancję, to dla każdego c >0 prawdziwe jest $P(|X - EX| \ge c) \le \dfrac{Var(X)}{c^2} $

## Zbieżność zmiennych losowych

Jeśli $X_1, ..., X_n$ są określone na $(\Omega, \mathcal{F}, P)$, to jeśli
$\lim_{n->\infty} X_n(\omega) = X(\omega)$ to powiemy, że ciąg ($X_n$) zbiega do zmiennej losowej X.
Jako że ta zbieżność zachodzi bardzo rzadko zastępuje się ją innymi:

## Zbieżność ciągu prawie na pewno

$X_1, ..., X_n$ zbiega do X prawie na pewno, gdy $P(\{\omega: \lim_{n->\infty}X_n(\omega) = X(\omega)\} = 1$

## Zbieżność ciągu według prawdopodobieństwa

Jeśli dla każdego $\epsilon$ > 0 zachodzi
$\lim_{n->\infty} P(\{\omega:|X_n(\omega) - X(\omega)| > \epsilon\}) = 0$

## Zbieżność względem p-tej średniej

$\lim_{n->\infty} E(|X_n - X|^p) = 0$

jak p = 2, to nazywamy to zbieżnością średniokwadratową.

## Zbieżność według rozkładu

$\lim_{n->\infty} F_n(x) = F(x)$
dla każdego $x \in \mathbb{R}$ , który jest punktem ciągłości F.

**Zachodzą następujące zależności**
(z prawdopodobieństwem 1 / wg. p-tej średniej) -> wg. prawdopodobieństwa -> wg. rozkładu

## Twierdzenia graniczne

## Mocne prawo wielkich liczb Kołmogorowa

($X_n$) to ciąg niezależnych zmiennych losowych o tym samym rozkładzie o skończonej wartości oczekiwanej $\mu = E(X_1)$ i oznaczamy $S_n = \sum_{i=1}^n X_i$ . Wtedy:
$\dfrac{S_n}{n}$ zbiega prawie na pewno do $\mu$ .

## Twierdzenie Poissona

Jeśli ($X_n$ ) jest ciągiem zmiennych losowych takim, że $X_n$ ~ binom(n,$p_n$), gdzie $\lim_{n->\infty} np_n = \lambda > 0$ , to
$X_n ->_d X$
gdzie X ~ Pois($\lambda$) ,
co pociąga za sobą:
$\lim_{n->\infty}P(X_n = k) = e^{-\lambda} \dfrac{\lambda^k}{k!}$

## Centralne twierdzenie graniczne

$(X_n)$ to ciąg niezależnych zmiennych losowych o tym samym rozkładzie ze skończoną wariancją. Oznaczmy $\mu$ = $E(X_1)$ , $\sigma^2 = Var(X_1)$ i $S_n = \sum_{i=1}^{n} X_i$ . Wówczas:
$\dfrac{S_n - n\mu}{\sigma \sqrt{n}} ->_d U$które ma rozkład U ~ $\mathcal{N}(0,1)$ , co inaczej można zapisać jako:
$\lim_{n->\infty} P(\dfrac{S_n - \mu n}{\sigma \sqrt{n}} \le x) = \phi(x)$
dla każdego $\phi \in \mathbb{R}$

.
