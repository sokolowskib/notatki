**DDL** - Data Definition Language
Służy do tworzenia baz danych, tabel, restrykcji w nich, kluczy głównych i obcych z fizycznego modelu danych.

## Fizyczny model danych

Żeby poprawnie stworzyć bazę danych należy podążać za fizycznym modelem danych. W trakcie procesu jego tworzenia:

- encje z logicznego modelu zamieniają się w tabele
- atrybuty zamieniają się w kolumny z ich typami

Celem jest stworzenie takiego fizycznego modelu danych żeby zawierać wszystkie dane z logicznego modelu danych oraz jak najbardziej zmaksymalizować wydajność poleceń.

![[obrazki/Pasted image 20260615170509.png]]

Proces zmiany LDM w PDM. Powinny być typy kolumn ale prezentacja jest chujowa.

## Podziały tabeli

1. pionowe - najczęściej wykorzystywane kolumny w jednej, inne w drugiej tabeli
2. poziome - dzielenie rekordów na kilka tabel, aby uniknąć przetwarzania wszystkich.

## Tworzenie tabeli

```SQL
create table Tablename(
Column1 int identity(1,1) primary key,
column2 date not null,
column3 nvarchar(14) default 'dupa',

constraint fk_table_key foreign key (column3) references TableInny(id)
)
```

DDL jest praktycznie nie przenośny między DBMS.

## Alter table

Modyfikuje jak wygląda definicja tabeli.

```SQL
alter table first add birthdate datetime
alter table first alter column psurname varchar(200)
```

**Nie wszystkie kolumny można zmienić, czasami musisz wyrzucić rekordy z tabeli.**

## Drop

Usuwa rekordy z tabeli, indexy lub kolumny tabel.

```SQL
drop table first
alter table firstg drop column birthdate
```

**Nie wolno usuwać np. kolumn uczestniczących w jakiejś relacji bez usunięcia tej relacji.**

## Zależność on systemu

Działanie oraz składnia DDL zależy w dużej części od implementacji SQL w wybranym DBMS.

## ANSI\_NULLS

```SQL
-- ansi nulls on
where phone = null != where phone is null

--ansi nulls off
where phone = null == where phone is null
```
