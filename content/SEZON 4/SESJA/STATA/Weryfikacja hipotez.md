Hipotezą statystyczną nazywamy przypuszczenie, dotyczące nieznanego rozkładu badanej cechy populacji, o prawdziwości lub fałszywości którego wnioskuje się na podstawie pobranej próby.

Hipotezy dotyczące wartości parametru/ów nazywamy hipotezami parametrycznymi.

Hipotezy proste to takie, które jednoznacznie określa rozkład badanej cechy. Hipoteza złożona nie robi tego.

W praktyce rozważamy dwie hipotezy: zerowa oraz alternatywna.

## Statystyka testowa

Nazywamy nią funkcję próby $\delta(X_1, ..., X_n)$, która służy do weryfikacji $H_0$ przeciwko $H_1$.

Jeśli $\delta(X_1, ..., X_n) \in W$, to $H_0$ odrzucamy
Jeśli $\delta(X_1, ..., X_n) \in W'$ , to $H_1$ przyjmujemy.
W nazywamy zbiorem odrzuceń bądź zbiorem krytycznym testu.

## Rodzaje błędów

1. Odrzucamy $H_0$ gdy jest ona prawdziwa
2. Nie odrzucamy $H_0$ gdy jest ona fałszywa

Pierwszy rodzaj -> odrzucamy poprawna hipoteze , drugi to false positive

## Poziom istotności testu

Oznaczamy jako $\alpha \in (0,1)$ , jest to kres górny wszystkich możliwych wartości prawdopodobieństwa błędu pierwszego rodzaju. Dokładniej, jeśli $H_0 : \theta \in \Theta_0$ , to :
$\alpha = sup_{\theta \in \Theta_0} P (A)$ gdzie A oznacza odrzucenie $H_0$ , gdy testowany parametr ma wartość $\theta$.

Jest to wartość narzucana z góry, czyli test ma być tak dobrany że prawdopodobieństwo odrzucenia poprawnej hipotezy $H_0$ jest co najwyżej równa $\alpha$.

Jako że wraz z minimalizacją możliwości popełnienia błędu pierwszego uwrażliwiamy się na możliwość popełnienia testu drugiego, zazwyczaj po prostu ustawiamy $\alpha$ na jakąś wartość a potem szukamy jak najmniejszego prawdopodobieństwa popełnienia błędu 2.

## Funkcja mocy

$\beta(\theta) = P(odrzuci\quad H_0 | \theta)$
Szczególnie dla $\theta_0 \in \Theta_0$ , czyli dla wartości parametru spełniających hipotezę zerową:
$\beta(\theta_0) = P(odrzuci\quad H_0|H_0)$ czyli jest to prawdopodobieństwo błędu 1, oraz dla $\theta_1 \in \Theta_1$:
$\beta(\theta_1) = P(odrzuci \quad H_0|H_1) = 1 - P(odrzuci \quad H_1|H_1)$
czyli 1 - prawdopodobieństwo popełnienia błędu 2 rodzaju.

## Definicja

Najmniejszy poziom istotności, przy którym zaobserwowana wartość statystyki testowej prowadzi do odrzucenia $H_0$ nazywamy p-value.

- jeśli p-value <= $\alpha$ ====> odrzucamy $H_0$
- jeśli p-value > $\alpha$ ====> przyjmujemy $H_0$
