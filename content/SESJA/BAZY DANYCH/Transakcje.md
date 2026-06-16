Transakcje rozwiązują potrzebę istnienia opcji wprowadzenia atomowych, 100% wykonanych zmian w bazie danych.

### Przykładowa lista problemów, które rozwiązują transakcje

1. potrzeba wykonania kilku kwerend bezpośrednio po sobie, gdy jakaś może nie przejść
2. możliwość utraty zmiany z powodu zapisania jej w cache'u i np. straty prądu
3. użytkownicy wykonujący bezpośrednio po sobie konfliktujące kwerendy

## ACID

- A - Atomicity, zmiany trzymane są w transaction log, jeśli jakaś nie przejdzie wykonywany jest rollback wszystkiego
- C - Consistency, transakcja przenosi bazę danych ze stanu spójnego do stanu spójnego, nie ma naruszonych warunków `constraint` itd.
- I - Isolation, równoległe transakcje nie widzą jeszcze niezatwierdzonych zmian.
- D - Durability, po commit zmiany są zapisane na stałe

## Przykład

```SQL
begin transaction
update account set balance = balance - 100 where id = 455
update account set balance = balance + 100 where id = 655
commit transaction
```

## Rodzaje transakcji w MSSQL

![[obrazki/Pasted image 20260615174130.png]]

### Słowa kluczowe

- begin transaction
- commit
- rollback - cofa wszystkie zmiany

\*\*Np. w Oracle Database koniec transakcji (rollback lub commit) oznacza początek następnej, więc nie trzeba pisać ponownie `begin transaction`. \*\*

## DDL a transakcje

Wg. prezentacji wyrażenia DDL nie mogą być częścią transakcji.

- W MSSQL `rollback` usunie stworzoną w transakcji tabelę
- W Oracle Database `rollback` nie usunie tabeli stworzonej w transakcji.

## Jak pod spodem działają transakcje

![[obrazki/Pasted image 20260615174822.png]]

- w przypadku rollbacku tylko częściowo wykonanej transakcji, sprawdzany jest transaction log, gdzie zapisywane są wszystkie zmiany, które mają zostać wykonane w trakcie transakcji
- w przypadku zcache'owania zmiany przez bazę danych i nagłego wyłączenia prądu, po restarcie systemu sprawdzany jest transaction log
- jako że ponad 99% transakcji jest commitowanych, dla wydajności często zmiany są wprowadzane nawet przed wykonaniem commit, często w przypadku `rollback` cofa się bazę do poprzedniego stanu
- im więcej izolowanych transakcji, tym wolniej może działać baza

## Poziomy izolacji w transakcjach

- `read uncommited` - najniższa, czyta jeszcze nie zatwierdzone dane innych transakcji
- `read commited` - wyższa, czyta tylko commited dane, ale po update zwalnia lock na rekord, więc może wystąpić phantom read albo non - repeatable read
- `repeatable read` - lock trzymany do końca transakcji, phantom read dalej może się pojawić
- `serialazible` - lock jest trzymany na cały range pasujący do where
- `snapshot `  - robiony jest "screenshot" danych na początku transakcji, tylko na nich pracuje.

### OSS DMBS a ACID

Wiele open source'owych DBMS'ów nie wspiera ACID, przez co wykorzystanie ich w aplikacjach krytycznych jest nierozważne.
