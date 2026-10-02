Wektor losowy to grupa zmiennych losowych określonych niekoniecznie na tej samej przestrzenie probabilistycznej. $X = (X_1, ..., X_d).$

## Funkcja masy prawdopodobieństwa

Dla d-wymiarowych dyskretnych wektorów losowych:
$p(x_1,x_2,...,x_d) = P(X_1 = x_1 \land X_2 = x_2 \land ...)$

- Oczywiście p >= 0 dla każdego możliwego $(x_1, ..., x_d)$ .
- $\sum_{x_1,...,x_d} p(x_1, .., x_d) = 1$

Jak dla danej funkcji spełnione są powyższe warunki, to wiadomo że to funkcja masy prawdopodobieństwa jakiegoś wektora losowego.

Rozkład brzegowy otrzymuje się:
$P(X_i = x_i) = \sum_{x1,....,x_{i-1}, x_{i+1}, x_d} p(x_1,...,x_d)$ Dla d-wymiarowych absolutnie ciągłych wektorów losowych:
$P(X\in A) = \int_A f_X(x_1,...,x_d)dx_1dx_2...dx_d$
Całka po $\mathbb{R}$ daje 1.

gęstość brzegowa analogicznie, tylko że nie całkujemy po x\_i.

## Łączna dystrybuanta

$F_X(x_1,x_2,...,x_d) = P(X_1 \le x_1, X_2 \le x_2,...)$
Przez dystrybuantę brzegową oznacza się:

## Niezależność zmiennych losowych

Musi być spełniony warunek:
$F_X(x_1,...,x_d) = F_{X_1}(x_1) F_{X_2}(x_2)....$

#### Twierdzenie o niezależności

Jeśli X to dyskretny wektor losowy, to zmienne losowe $X_1, ..., X_d$ są niezależne wtw. gdy:
$P(X_1 = x_1, ..., X_d = x_d) = P(X_1 = x_1)...P(X_d = x_d)$
Natomiast jeśli X to absolutnie ciągły wektor losowy, to $X_1, ..., X_d$ są niezależne wtw. gdy:
$f_X(x_1,...,x_d) = f_{X_1}(x_1)....f_{X_d}(x_d)$

## Kowariancja i współczynnik korelacji

Zakładamy że Var'y oraz EX istnieją

#### Kowariancja

Nazywamy nią liczbę:
$Cov(X,Y) = E(XY) - E(X)E(Y)$

gdzie:
$E(XY) = \sum_{k,l}x_ky_lP(X = x_k, Y = y_l)$ dla dyskretnych wektorów losowych oraz:
$E(XY) = \int_{\mathbb{R^2}}xyf_{(X,Y)}(x,y)dxdy$

#### Współczynnik korelacji

$\rho_{XY} = \dfrac{Cov(X,Y)}{\sigma_X \sigma_Y}$
Jeśli współczynnik korelacji = 0, to X i Y nieskorelowane.

Własności współczynnika korelacji:

1. $\rho \le 1 \land \rho \ge -1$
2. $\rho$ to siła miary zależności X i Y. $\rho_{XY} = 1$ wtedy i tylko wtedy gdy: P(Y = aX + b) = 1
   (a > 0 to $\rho$ = 1 i a < 0 to $\rho = -1$)

### Twierdzenie o operacjach na wartościach oczekiwanych

$E(aX + bY + c) = aEX + bEY +c$
$Var(aX + bY +c) = a^2Var(X) + b^2Var(Y) +2abCov(X,Y)$

## Twierdzenie o niezależności zmiennych losowych

Jeśli zmienne losowe są niezależne, to $E(XY) = EXEY$ , co pociąga za sobą $Cov(X,Y) = 0$
i dalej $Var(X + Y) = Var(X) + Var(Y)$. Jeśli wiemy że wariancje X i Y są niezerowe, to $\rho_{XY} = 0$

## Macierz kowariancji

![[SEZON 4/obrazki/Pasted image 20260624124826.png]]

## Rozkład sumy zmiennych losowych

Niech wektor losowy $(X,Y)$ ma gęstość $f(x,y)$. Wtedy zmienna losowa $Z = X + Y$ ma gęstość daną wzorem $f_Z(z) = \int_{\mathbb{R}}f(x,z-x)dx$

## Przykłady rozkładów sum niezależnych zmiennych losowych

Jeśli $X_1, ..., X_d$ to niezależne zmienne losowe i

1. $X_i$ ma rozkład dwupunktowy dla każdego $i \in {1,..,d}$ to $Z = X_1 + ... + X_d$ ma rozkład dwumianowy binom(n,p)
2. $X_i$ ma rozkład Poissona o $\lambda_i$ , to Z zdefiniowane jak wyżej też ma rozkład Poissona o $\lambda = \lambda_1 + ... + \lambda_d$
3. $X_i$ ma rozkład normalny o $\mathcal{N}(\mu_i,\sigma_i^2)$ , to Z tak jak wyżej ma rozkład
   $\mathcal{N}(\mu_1 + ... + \mu_n, \sigma_1^2 + ... + \sigma_n^2)$
4. $X_i$ ma rozkład Gamma($a_i$, s) ,to Z ma rozkład $Gamma(a_i + ... + a_n, s)$
