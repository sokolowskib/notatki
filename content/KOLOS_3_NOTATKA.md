# Kolos 3 —

Tag = jaka konstrukcja SQL. Skróty: `PK`=primary key, `FK`=foreign key, `WF`=window function.P

---

## Plik 1 — `edu_courses` (DDL + procedura)

### DDL — tworzenie schematu

```sql
create database edu_courses
use edu_courses
```

**create db + use** — przełączenie kontekstu na bazę.

```sql
CourseID int identity(1,1) primary key not null,
base_price money,
planned_groups_amount int default 1,
is_active bit default 1   -- nie ma boolean w T-SQL
```

**create table** — `identity(1,1)` autoinkrement, `PK`, `default`, typy `money`/`bit`/`nvarchar`/`datetime null`.

```sql
constraint fk_course_enrollment_users_user foreign key(user_id) references users_user(user_id)
```

**FK inline w create table** — nazwany constraint, referencja do innej tabeli.

```sql
alter table users_user add phone_number nvarchar(25)
alter table users_user drop column age
alter table course add constraint ck_date check(date_start < date_end)
```

**alter table** — dodanie kolumny / usunięcie kolumny / dodanie `check` constraint po fakcie.

```sql
INSERT INTO users_user (email, first_name, ...) VALUES (...),(...),(...)
```

**INSERT multi-row** — kilka wierszy jednym `values`.

### Indeksy

```sql
create index idx_course_enrollment on course_enrollment(user_id)
create unique index idx_unique_email on users_user(email)
create index idx_course_dates on course(date_start, date_end)
create clustered index idx_clustered_course_enrollment on course_enrollment(user_id, group_id)
create index idx_name_surname on users_user(last_name, first_name)
```

**create index** — nonclustered / `unique` / złożony (2 kolumny) / `clustered` / złożony. Jeden plik = pełen przegląd typów indeksów.

### Procedura `Prepare` — walidacja + transakcja + warunkowy rabat

**procedura + parametry + try/catch + transaction + scope\_identity + warunki rabatowe** — najważniejszy snippet z pliku 1, łączy prawie wszystko.

```sql
create procedure Prepare @email nvarchar(100), @course_id int as
declare @is_active bit
begin
  begin try
  begin transaction
    -- walidacja: kurs istnieje?  (NULL = nie istnieje)
    set @is_active = (select is_active from course where CourseID = @course_id)
    if @is_active is null begin print 'kurs nie istnieje'; rollback transaction; return end
    if @is_active = 0    begin print 'kurs nieaktywny';   rollback transaction; return end

    -- user: jak nie istnieje -> insert + scope_identity()
    declare @user_id int = (select user_id from users_user where email = @email)
    if @user_id is null
      begin insert into users_user(...) values(...); set @user_id = scope_identity() end
    else
      begin -- jak istnieje, sprawdź czy aktywny
        if (select is_active from users_user where email=@email) = 0
          begin print 'user nieaktywny'; rollback transaction; return end
      end

    -- znajdź wolną grupę: capacity > liczba niezrezygnowanych zapisów
    declare @empty_group int = (
      select top 1 g.group_id from [group] g
      where g.max_group_capacity > (select count(*) from course_enrollment ce
                                    where ce.group_id=g.group_id and ce.is_dropped=0)
        and g.course_id = @course_id)
    if @empty_group is null begin print 'brak grupy'; rollback transaction; return end

    -- rabat zależny od liczby kursów usera (if / else if / else)
    declare @course_amount int = (select count(*) from course_enrollment ce
                                  join users_user uu on ce.user_id=uu.user_id
                                  where uu.email=@email and is_dropped=0)
    -- 0 -> -100,  1 -> *0.95,  >1 -> *(1-(0.05+amount*0.01))
  commit transaction
  end try
  begin catch
    rollback transaction
    throw            -- przerzuca błąd dalej
  end catch
end
```

Kluczowe rzeczy: `scope_identity()` po insercie, `top 1` + skorelowany `count(*)` do liczenia obłożenia grupy, `is null` jako test „nie istnieje", `throw` w catch.

```sql
exec Prepare 'jan.kowalski@gmail.com', 2
```

**exec** — wywołanie procedury z parametrami pozycyjnie.

---

## Plik 2 — Northwind (DML + kursory + okna)

### DML — update / insert / delete

```sql
update dbo.Orders set EmployeeID = 4 where EmployeeID = 1
```

**UPDATE prosty** — zmiana po warunku.

```sql
update [Order Details] set Quantity = Round(0.8 * Quantity, 0)
where OrderID in (select OrderID from Orders where OrderDate > '1997-05-15')
  and ProductID = (select distinct ProductID from Products where ProductName='Ikura')
```

**UPDATE + ROUND + podzapytania** — `in (...)` dla wielu, `= (...)` dla jednego; `Round(x,0)` do całkowitej.

```sql
insert into [Order Details](...)
select (select top 1 o.OrderID from Orders o ... order by o.OrderDate desc),
       ProductID, UnitPrice, 1, 0
from Products where ProductName='Chocolade'
```

**INSERT...SELECT + skalarny podzapytanie + TOP 1 + NOT IN** — „ostatni order bez czekolady" przez `order by ... desc` w środku selecta.

```sql
insert into [Order Details](...)
select o.OrderID, p.ProductID, p.UnitPrice, 1, 0
from Orders o, Products p              -- cross join przez przecinek
where o.CustomerID='ALFKI' and p.ProductName='Chocolade'
  and o.OrderID not in (select ... where ProductName='Chocolade')
```

**INSERT...SELECT + CROSS JOIN (FROM a,b) + NOT IN** — dodaj produkt do wszystkich orderów, gdzie go nie ma.

```sql
delete from Customers
where CustomerID not in (select c.CustomerID from Customers c join Orders o on o.CustomerID=c.CustomerID)
```

**DELETE + NOT IN z join** — usuń klientów bez zamówień.

### Transakcje + try/catch (zad 6 i 7)

**try/catch + transaction + commit/rollback** — schemat „sprawdź → zmień → sprawdź → cofnij".

```sql
begin try
  begin transaction
    insert into Products(ProductName) values('Programming in Java')
    update [Order Details] set Quantity = Quantity + 1 where ...
  commit transaction        -- zad6: zatwierdzasz
end try
begin catch
  rollback transaction
end catch
```

Zad 7 = to samo, ale celowo `rollback` zamiast `commit`, żeby pokazać cofnięcie zmian (select sumy przed / w trakcie / po).

### Scenariusze 2 i 3

```sql
alter table Orders add IsCanceled int
update Orders set IsCanceled = case when CustomerID='ALFKI' then 1 else 0 end
```

**ALTER ADD + UPDATE z CASE** — flaga warunkowa.

```sql
update o
set o.TotalValue = agg.Total
from Orders o
join (select od.OrderID, sum(pl.price*od.Quantity) as Total
      from [Order Details] od
      join PriceList pl on pl.ProductID=od.ProductID
      join Orders o2 on o2.OrderID=od.OrderID
      where o2.OrderDate between pl.date_from and pl.date_to
      group by o2.OrderID) agg on agg.OrderID=o.OrderID
```

**UPDATE...FROM JOIN (zagregowane podzapytanie) + BETWEEN po datach** — przepisanie sumy z cennika obowiązującego w dacie zamówienia. Klasyczny update-przez-join.

### Kursory

**KURSOR — szkielet do zapamiętania:**

```sql
declare @x int
declare cur cursor local for (select ... )
open cur
fetch next from cur into @x
while @@fetch_status = 0
begin
    -- robota na @x
    fetch next from cur into @x      -- NIE zapomnij, inaczej pętla w nieskończoność
end
close cur
deallocate cur
```

```sql
create procedure CalculateOrderCount @country varchar(100) as ...
```

**procedura + kursor** — dla każdego klienta z kraju policz `count(*)` zamówień i wpisz do `OrderCount` (skorelowany update w pętli).

```sql
alter procedure ArchiveSomeShit @N int as ...
```

**procedura + kursor + datediff + insert/delete** — archiwizacja: `datediff(year, OrderDate, getdate()) >= @N`, przenosi do Archived\* i kasuje z oryginału. (try/catch+transaction zakomentowane — można dorobić.)

```sql
create procedure SetDiscount @CustomerID nchar(5) as ...
```

**procedura + ZAGNIEŻDŻONE kursory + CASE** — kursor po orderach, w środku kursor po produktach; rabat zależny od liczby wcześniejszych zamówień produktu (`case when ... in (1,2) then 5 ...`).

### Triggery

```sql
create trigger Dupa on Customers after insert as select * from Customers
go
```

**trigger AFTER INSERT** — odpala się po każdym insercie. `instead of` = przechwytuje operację i robi coś zamiast niej.

### Trudniejsze selecty — funkcje okienkowe (WF)

```sql
sum(Quantity*od.UnitPrice) over (partition by od.ProductID)  as TotalForProduct,
sum(Quantity*od.UnitPrice) over (partition by p.CategoryID)  as TotalForCategory
```

**SELECT + WF: SUM OVER PARTITION BY** — suma w obrębie produktu / kategorii bez `group by`. Dodatkowo skalarne podzapytania w `select` na nazwy.

```sql
sum(...) over () as TotalOrdersValue
```

**OVER() puste** — suma globalna po całym zbiorze.

```sql
sum(...) over (order by o.OrderID rows between unbounded preceding and current row) as GrowingSum
```

**WF running total** — suma narastająca (ramka od początku do bieżącego wiersza).

```sql
sum(...) over (order by o.OrderID rows between 2 preceding and current row) as SlidingSum
```

**WF okno przesuwne** — suma z bieżącego + 2 poprzednich wierszy.

```sql
sum(...) over (partition by p.ProductID, year(o.OrderDate) order by o.OrderDate
               rows between unbounded preceding and current row)
count(case when od.Quantity>0 then 1 end) over (partition by p.ProductName, year(o.OrderDate) ...)
```

**WF z partycją po wielu kolumnach + narastająco + count(case)** — sprzedaż produktu rok/miesiąc, narastająco od początku roku, liczba miesięcy z niezerową sprzedażą.

> **ROWS vs RANGE:** `rows` liczy fizyczne wiersze; `range` traktuje wiersze o tej samej wartości `order by` jako jeden blok (wszystkie dostają tę samą sumę).

---

## Szybkie „co wybrać"

- **liczenie w obrębie grupy bez zwijania wierszy** → `OVER (partition by ...)`
- **narastająco / przesuwne okno** → `OVER (order by ... rows between ...)`
- **iteracja wiersz po wierszu, logika proceduralna** → kursor
- **„sprawdź → zmień → ewentualnie cofnij"** → `begin try / begin transaction / commit / catch+rollback`
- **„dodaj X tam gdzie go nie ma"** → `insert...select ... where ... not in (...)`
- **„przepisz zagregowaną wartość do kolumny"** → `update o set ... from o join (select ... group by ...) agg`
