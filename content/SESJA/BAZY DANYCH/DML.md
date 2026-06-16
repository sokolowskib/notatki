SQL nie jest tylko do zapytań, trzeba też jakoś wsadzić rekordy do DB.

**DML** - Data Manipulation Language

tutaj lista bardziej niszowych poleceń, popularniejsze potem:

- MERGE - służy do insertowania i/lub aktualizowania rekordów
- CALL (Oracle) - do wywołania metody
- BULK INSERT - do wsadzenia pliku z danymi do MSSQL servera

## INSERT

Wsadzenie rekordu/ów do bazy danych.

```SQL
--1. metoda
INSERT INTO Table [(...)] VALUES (......),(...)
-- insertowanie kilka wartości wspierane tylko w niektórych wersjach na przykład MSSQL 2008+ yay ale Oracle 10g nay


--2. metoda
INSERT INTO Table [(...)] select * from ....
```

Kolumny mogą być pomijane przy insercie, ale wtedy musi być podana dokładna lista kolumn insertowanych. W tym przypadku wsadzane są wartości podstawowe.

Przy insercie drugą metodą długość listy wynikowej kolumn oraz ich typ musi się zgadzać z zadeklarowaną listą kolumn. Można tak wsadzać wiele rekordów.

## UPDATE

Modyfikacja jednego/wielu istniejących rekordów.
[[Etapy Normalizacji#Spójność referencyjna|Spójność referencyjna]] może spowodować, że niektóre update nie przejdą.

```SQL
update customers set city = 'Cracow', Discount = 10 where CustomerID = 1045

update customers set ... where customerid in (select * from Orders where ....)
```

## DELETE

Pozwala na usuwanie istniejących rekordów. Może celować w wiele rekordów. [[Etapy Normalizacji#Spójność referencyjna|Spójność referencyjna]] może spowodować niepowodzenie polecenia.

```SQL
delete from Customers c where not exists (select * from order o where o.customerid = c.customerid) -- usuwa wszystkich klientow ktorzy nie maja orderu
```

Wg. slajdów nie wolno korzystać z aliasów w poleceniach update/delete na MSSQL, ale w ORACLE już tak. Jest to nieprawda ponieważ można z nich korzystać używając składni `From`

```SQL
-- SQL Server — DZIAŁA
UPDATE o
SET o.Freight = 0
FROM Orders o
WHERE o.CustomerID = 'ALFKI'

-- SQL Server — NIE DZIAŁA (alias bezpośrednio po nazwie tabeli)
UPDATE Orders o
SET o.Freight = 0
WHERE o.CustomerID = 'ALFKI'
```
