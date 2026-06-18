## Procedury składowane

Procedura zdefiniowana w dialekcie SQL, służą do przyspieszenia drogich oraz długich operacji na DBMS. Pozwalają również na wykonywanie skomplikowanych operacji po stronie serwera. Wyróżniamy:

- procedury systemowe - pozwalające na modyfikację silnika bazodanowego w sposób niedostępny przez zwykłe polecenia SQL
- procedury rozszerzone - DLL'ki rozszerzające funkcjonalność DBMS o kod w nich skompilowany

### Przykładowa procedura z kursorem

```sql
CREATE PROCEDURE CalculateOrderCount @country varchar(100)
AS
DECLARE @customer varchar(100)
DECLARE custom CURSOR LOCAL FOR SELECT CustomerId FROM 
Customers WHERE country=@country
OPEN custom
FETCH NEXT FROM custom INTO @customer
WHILE @@FETCH_STATUS=0
BEGIN
UPDATE Customers SET OrderCount=(SELECT COUNT(*) FROM 
Orders WHERE CustomerId=@customer) WHERE 
CustomerId=@customer
FETCH NEXT FROM custom INTO @customer
END
CLOSE custom
DEALLOCATE custom
```

## Procedury składowane vs bezpośrednie przetwarzanie kwerend

Jednym z plusów procedur jest to, że eliminuje niepotrzebną komunikację między klientem a serwerem w rozwiązaniach client-server. Jeśli klient chce otrzymać odpowiedzi kilku kwerend, nie musi wysyłać wielu zapytań HTTP przez sieć, tylko wysyła prośbę wykonania jednej procedury, co eliminuje nadmiar komunikacji, przyspieszając proces.

**Niestety kod procedur jest nieprzenośny, istnieją różne standardy w innych dialektach SQL.**

## Trigger

Kod, który możemy wykonywać automatycznie po/zamiast modyfikacji jakiejś tabeli.
W niektórych DBMS triggery mogą być wywoływane na widokach. Trigger to zmiana, więc może wywoływać wykonanie innych triggerów, itd.

```sql
--skladnia
create trigger name on table [after | instead of][delete | update | insert] as 
...


CREATE TRIGGER InsertOrder ON Orders AFTER INSERT 
AS
DECLARE @customer varchar(100)
SET @customer=(SELECT CustomerId FROM inserted)
UPDATE Customers SET OrderCount=OrderCount+1 
WHERE CustomerId=@customer
```

## SQL Server Agent

Odpowiedzialny za automatyczne wykonywanie procedur na podstawie jaka jest godzina.
Cron job w SQL.

![[obrazki/Pasted image 20260616113330.png]]

Można zdefiniować ile się chce wykonać procedur i w jakiej kolejności, na jakiej bazie i o jakiej godzinie.

**Procedury, triggery oraz funkcje pozwalają na spójne procesowanie danych przez wiele aplikacji jednocześnie.**

## Procedury a plan wykonania

Procedury mogą przyspieszyć swoje wykonywanie przez DBMS, analizując [[Indexy i plan wykonania#Plan wykonania|plan wykonania]].
DBMS przechowuje ich plan wykonywania, ulepszając wydajność każdego następnego wywołania procedury.
