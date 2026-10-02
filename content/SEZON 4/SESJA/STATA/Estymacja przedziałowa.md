Przedziałem ufności dla parametru $\theta$ na poziomie ufności 1 - $\alpha$ ,gdzie $\alpha \in (0,1)$ nazywamy przedział ($\theta_1 , \theta_2$), gdzie $\theta_1$ i $\theta_2$ to mierzalne funkcje próby (estymatory) takie że $\theta_1 \le \theta_2$ oraz $P(\theta \in (\theta_1,\theta_2)) = 1 - \alpha$ dla każdego $\theta \in \Theta$.

Końce przedziału ufności to zmienne losowe. Dla różnych realizacji próby może być tak, że raz parametr należy do wyznaczonego przedziału ufności(wyznaczony dla jakiejś próby), a raz nie.

## Długość przedziału ufności

$l_n = \theta_2(X_1,...,X_n) - \theta_1(X_1,...,X_n)$
Żeby estymacja była jak najlepsza (pod względem precyzji), to staramy się minimalizować długość przedziału ufności. Ponownie, $l_n$ to zmienna losowa, więc może być tak że dla jednej realizacji próby długość jednego przedziału jest większa niż drugiego, a dla innej realizacji próby wice wersa. Dlatego tak samo jak w estymatorach punktowych, wykorzystuje się wartości oczekiwane długości przedziałów ufności.

## Przedziały ufności dla wartości średniej

Modele te są wszystkie na kartkach do egzaminu, więc skrótowo opiszę tylko kiedy korzystać z którego.

## Model 1

Nieznana jest EX, znana jest wariancja. Wtedy z przekształceń na zmiennych losowych dostajemy U ~ N(0,1).

## Model 2

Nieznana wartość oczekiwana ani wariancja. Wykorzystywany jest wtedy wzorek na zmienna losową o rozkładzie t studenta.

## Model 3

Nieznana wartość oczekiwana oraz nieznana wariancja. dla tego problemu n jest duże (>= 100)
Z centralnego twierdzenia granicznego wynika, że

![[SEZON 4/obrazki/Pasted image 20260624171746.png]]

Jako że $S_n$ jest mocno zgodnym estymatorem parametru $\sigma$ to można go tam zastąpić dostając
![[SEZON 4/obrazki/Pasted image 20260624171904.png]]

## Przedziały ufności dla wariancji

## Model

![[SEZON 4/obrazki/Pasted image 20260624172240.png]]

## Wymagana liczność próby do osiągnięcia danej długości przedziału ufności

Jako że długość przedziału ufności jest zależna od liczności próby, zwiększając liczność próby można z góry ograniczyć długość przedziału na danym poziomie ufności

## Model 1 dla wartości średniej

![[SEZON 4/obrazki/Pasted image 20260624174128.png]]

## Model 3 dla wartości średniej

$n_0 > 30$
![[SEZON 4/obrazki/Pasted image 20260624174347.png]]

Jako że wartość n jest zależna od wartości wariancji, to najpierw liczymy wariancję dla próby losowej o mniejszej liczności, a następnie sprawdzamy co jest większe. Można wtedy dolosowywać te pozostałe n, nie trzeba powtarzać całości.

## Model 2

![[SEZON 4/obrazki/Pasted image 20260624174538.png]]

tutaj nie wymagamy n0 >30, poniewaz ten wzor nie zalezy juz od rozmiaru. Model III wykorzystuje prawo wielkich liczb, ten nie.

![[SEZON 4/obrazki/Pasted image 20260624174619.png]]
