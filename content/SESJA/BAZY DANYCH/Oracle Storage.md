Baza Oracle może być w wielu stanach:

- shutdown - wszystkie pliki zamknięte, nie ma otwartej instancji
- nomount - SGA istnieje, procesy w tle działają, instancja nie jest podłączona do bazy danych
- mount - nomount + wszystkie pliki kontrolne istnieją i są identyczne
- open - w każdej grupie istnieje przynajmniej jeden log file, wszystkie dane on - line są obecne i poprawne, wszystkie dane na dysku są zsynchronizowane z plikami kontrolnymi

## Ustawienia SHUTDOWN

- normal - żadne nowe połączenia nie są dozwolone. Kończy się jak użytkownicy się odłączą.
- transactional - żadne nowe połączenie nie jest dozwolone. Kończy wtedy gdy skończą się wszystkie transakcje
- immediate - wszystkie transakcje są rollbackowane, instancja się kończy
- abort - porównywalny do wyłączenia prądu, logi są niesynchronizowane.

## Normalny shutdown

Mówi się tak na shutdown w trybie normal, transactional lub immediate. Transakcje się kończą, wszystkie dane z cache są wpisywane do pamięci, nagłówki są aktualizowane. Baza danych jest gotowa do użytku następnym razem.

## Storage

Rozróżniamy:

- fizyczny, jak system operacyjny widzi pliki Oracle
- logiczny, jak Oracle zarządza danymi w plikach

## Table Space

Grupa plików danych. Każdy tablespace ma unikalną strategię backupu. Wykorzystywane głównie po to by nałożyć wiele polityk backupowania plików, by force'ować tabele do zapisania się w jakimś zbiorze plików(każda tabela ma własny tablespace) oraz żeby oddzielić dane systemowe od danych użytkownika. Każdy tablespace może przechowywać tylko jeden rodzaj segmentów:

- pernamentne - trwałe obiekty (tabele)
- tymczasowe - obiekty istniejące tylko na chwilę, np. wynik sortowania
- undo - trzyma dane, które są obecnie modyfikowane, po to, by inni użytkownicy otrzymywali odpowiedzi na swoje query.

## Pliki parametryczne

- stare rozwiązanie - _initialisation file_, init\<oracle\_sid>.ora
- nowe rozwiązanie - _server parameter file_, spfile\<oracle\_sid>.ora
  Zawierają tuple (parameter\_name, parameter\_value), służą do ustawień przy startowaniu instancji, takich jak np.: lokalizacja innych plików.

![[obrazki/Pasted image 20260618111911.png]]
