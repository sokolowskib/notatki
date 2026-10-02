Z populacji pobieramy próbę, a na podstawie tej próby wyciągamy wnioski na temat populacji.

## Próba losowa

Jeśli $(X_n)$ są niezależne, oraz ma ten sam rozkład co cecha populacji X, to $X_1, ..., X_n$ nazywamy próbą losową z X.

W wyniku zebrania danych otrzymujemy realizację próby losowej, czyli n ustalonych wartości w postaci $x_1, ..., x_n$ .

Na podstawie próby losowej $X_1, ..., X_n$ chcemy opisać rozkład X. Są podejścia:

1. parametryczne -> zakładamy że X ma rozkład o dystrybuancie o znanej postaci a nie znamy jedynie parametrów
2. nieparametryczne -> X ma rozkład o dystrybuancie należącej do rodziny dystrybuant, indeksowane skończenie wymiarowym parametrem.

W podejściu parametrycznym zakładamy, że $X \sim F_\theta$  , gdzie $\theta \in \Theta \subseteq \mathbb{R^k}$  oraz $X_1, ..., X_n$ to próba losowa z X i szukamy parametru $\theta$ .

Estymacja punktowa polega na oszacowaniu $\theta$ za pomocą funkcji mierzalnej, której argumentami są elementy próby losowej $X_1, ..., X_n$ . Oszacowanie takie będziemy nazywać estymatorem $\theta$ i oznaczać $\hat{\theta}$  . Pytanie, jak znajdować taką funkcję, która szacuje ten parametr?

## Metody wyznaczania estymatorów

## Metoda momentów

1. Liczysz tyle momentów ($EX^k$) ile masz niewiadomych parametrów k.
2. Rozwiązujesz względem tych momentów równania dla niewiadomych parametrów
3. Pod momenty teoretyczne podstawiasz empiryczne, dostając estymatory parametrów.

## Metoda kwantyli

Różni się tym, że zamiast wyznaczać k momentów teoretycznych, wyznaczamy k kwantyli różnych rzędów, a potem zastępujemy je kwantylami empirycznymi.

Kwantyl empiryczny rzędu p $\in (0,1)$ to wartość taka, że miej więcej p \* 100% obserwacji jest mniejsza równa od tej wartości.
![[SEZON 4/obrazki/Pasted image 20260624143509.png]]

## Metoda największej wiarygodności

Niech $X_1, ..., X_n$ to prosta próba losowa z rozkładu o gęstości $f_\theta(x)$. Wtedy funkcją wiarygodności nazywamy:
$L(\theta,x_1,x_2,...,x_n) = f_\theta(x_1)f_\theta(x_2)...f_\theta(x_n)$

Analogicznie, jeśli $X_1, X_2, ..., X_n$ to próba losowa z rozkładu o masie prawdopodobieństwa $p_\theta(x)$ , to jej funkcją wiarygodności oznaczamy:
$L(\theta;x_1,...,x_n) = p_\theta(x_1)...p_\theta(x_n)$

Estymatorem największej wiarygodności nazywamy wartość parametru $\theta$ , która przy ustalonej realizacji próby losowej maksymalizuje funkcję wiarygodności:
$\hat{\theta_{NW}} = argmax_{\theta \in \Theta} L(\theta;x_1,...,x_n)$
Dla uproszczenia można zastosować

$\hat{\theta_{NW}} = argmax_{\theta \in \Theta} ln(L(\theta;x_1,...,x_n))$
Ponieważ obie funkcje są ściśle rosnące.

## Twierdzenie

Jeśli $\hat{\theta}$ jest estymatorem największej wiarygodności parametru $\theta$ i g jest funkcją mierzalną, to g($\hat{\theta}$) jest estymatorem największej wiarygodności parametru g($\theta$).

## Wielość estymatorów

$X_1, ..., X_n$ to próba losowa z populacji X o rozkładzie jednostajnym na przedziale $[0,\theta]$ , gdzie $\theta$ > 0. Szukamy estymatora parametru $\theta$

1. metoda momentów
2. metodą NW
   Wynik jest inny :O

## Własności estymatorów punktowych

Jako że sam estymator jest zmienną losową (zależy od realizacji, a realizacja zależy od zdarzenia losowego), to może wydarzyć się sytuacja ,gdzie
$|\theta - \hat{\theta_1}(\omega)| < |\theta - \hat{\theta_2(\omega)}|$
dla pewnych $\omega \in \Omega$ a

$|\theta - \hat{\theta_1}(\omega)| > |\theta - \hat{\theta_2(\omega)}|$
dla innych $\omega \in \Omega$ .

Można to rozwiązać rozważając wartość oczekiwaną tych estymatorów (jako że są to zmienne losowe).

## Estymatory nieobciążone

Mówimy, że estymator parametru $\theta$ jest nieobciążony , gdy
$E(\hat{\theta}) = \theta$ dla każdego $\theta \in \Theta$ .

Funkcję $B(\theta) = E_\theta(\hat{\theta}) - \theta$ nazywamy obciążeniem estymatora $\hat{\theta}$ .

## Przykłady estymatorów nieobciążonych

$X_1, ... , X_n$ będzie próbą losową z populacji X o rozkładzie z wartością oczekiwaną EX = $\mu$ . Wówczas $\hat{\mu} = S_n / n$ jest nieobciążonym estymatorem parametru $\mu$ .

$X_1, ..., X_n$ to próba losowa z populacji X o rozkładzie z wartością oczekiwaną EX = $\mu$ i dodatnią wariancją Var(X) = $\sigma^2 > 0$. Wtedy:
$\hat{\sigma^2} = \frac{1}{n}\sum_{i=1}^n(X_i - \hat{X})^2$jest estymatorem nieobciążonym parametru $\sigma^2$, ponieważ:![[SEZON 4/obrazki/Pasted image 20260624151913.png]]

## Asymptotyczne nieobciążenie

$\hat{\theta_n}$  to ciąg estymatorów parametru $\theta$. Mówimy że jest asymptotycznie nieobciążony gdy:
$\lim_{n->\infty}E_{\theta}(\hat{\theta_n}) = \theta$ dla każdego $\theta \in \Theta$.

## Błąd średniokwadratowy estymatora

$MSE_{\hat{\theta}}(\theta) = E_{\theta}((\hat{\theta} - \theta)^2)$

co sprowadza się do $Var_\theta(\hat{\theta}) + B(\theta)^2$

Więc dla estymatorów nieobciążonych jest to po prostu wariancja.

## Zgodność estymatorów

Mówimy, że ciąg estymatorów $\hat{\theta_n}$ jest zgodny w sensie zbieżności średniokwadratowej, gdy
$\lim_{n->\infty} E_{\theta}((\hat{\theta}_n - \theta)) = 0$
dla wszystkich $\theta \in \Theta$.
Mówimy, że ciąg estymatorów jest mocno zgodny z prawdopodobieństwem 1, gdy
$P_\theta(\lim_{n->\infty} \hat{\theta}_n = \theta) = 1$

Mówimy, że jest słabo zgodny jeśli
$\lim_{n->\infty} P_\theta(|\hat{\theta}_n - \theta| <\epsilon) = 1$
dla dostatecznie małego $\epsilon$.

Każdy estymator zgodny w sensie zbieżności średniokwadratowej i mocno zgodny jest zgodny.

## Twierdzenie

Jeśli ciąg estymatorów parametru $\theta$ jest asymptotycznie nieobciążony oraz
$\lim_{n->\infty}Var_\theta(\hat{\theta}_n) = 0$
to ten ciąg jest zgodny w sensie zbieżności średniokwadratowej.
