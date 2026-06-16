Większa izolacja => gorsza wydajność operacji współbieżnych
Wykorzystywane są kompromisy między tymi dwoma wartościami, co może spowodować niepożądane konsekwencje.

## Lista występujących problemów w transakcjach

Bardzo dużo pokrywa się z [[Transakcje#Poziomy izolacji w transakcjach|notatką o transakcji]], więc pobieżnie:

![[obrazki/Pasted image 20260615221639.png]]

## Poziomy izolacji a błędy współbieżności

![[obrazki/Pasted image 20260615222009.png]]

## Blokady

Żeby osiągnąć dany poziom izolacji, DBMS nakłada odpowiednie blokady na rekordy / tabele.

Rodzaje blokad:

- na rekordy:
  1. shared - nakładane w trakcie wielu selectów, pozwala na współbieżne pisanie, nikt nie może wtedy modyfikować rekordu
  2. exclusive - nakładany w trakcie poleceń DML, tylko dla piszącego, nikt nie może czytać
- na scheme:
  1. stability - nakładane w trakcie kompilacji kwerendy na strukturę przeszukiwaną, żeby struktura się nie zmieniła
  2. modification - nakładany w trakcie wykonywania operacji DDL na tabelę, współbieżny dostęp niemożliwy

## Deadlock

Co to deadlock każdy wie, trzeba rollback zrobić żeby cofnąć transakcję.

## Save point

Służy jako checkpoint, żeby nie rollbackować całej transakcji.

```sql
begin transaction a
...
save tran b
...
rollback tran b
...
commit
```

## Długotrwałe transakcje

Potrzebne w sytuacji wymagającej długotrwałego locka. Tworzy kopie danych specjalnie dla transakcji, żeby nie blokować transakcji krótkotrwałych.
