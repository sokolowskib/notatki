```
  
--1  
-- Wypisz informacje o produktach które zostały zamówione jednorazowo w ilości (quantity) poniżej 105%  
--   średniej ilości tego produktu zamawianej we wszystkich zamówieniach. Wyniki przedstaw w kolejności rosnącej  
--  liczby zamówień na produkt.  
with pre_avg as (  
    select p.ProductID, avg(od.Quantity) * 1.05 as AvgThreshold, count(*) as ProductCount  
    from dbo.Products p  
             join dbo.[Order Details] od on p.ProductID = od.ProductID  
    group by p.ProductID  
)  
select p.ProductName ,od.ProductID  
from dbo.[Order Details] od  
join pre_avg on od.ProductID = pre_avg.ProductID  
join dbo.Products p on od.ProductID = p.ProductID  
where Quantity < AvgThreshold  
group by od.ProductID, p.ProductName, pre_avg.ProductCount  
order by pre_avg.ProductCount  
  
  
--2  
--(3p.) Wypisz w porządku alfabetycznym nazwiska pracowników, którzy nie obsługiwali żadnego zamówienia wy-  
--słanego do Niemiec lub Brazylii i kiedykolwiek w ich nadzorowanym zamówieniu znajdował się produkt o nazwie  
--kończącej się na literę ’k’.  
  
select *  
from dbo.Employees e  
where  not exists (  
select *  
from dbo.Orders o  
where o.ShipCountry in ('Brazil' , 'Germany') and o.EmployeeID = e.EmployeeID  
)  
and exists(  
    select *  
    from Orders o  
    join dbo.[Order Details] od on o.OrderID = od.OrderID  
    join dbo.Products p on od.ProductID = p.ProductID  
    where ProductName LIKE '%k' and o.EmployeeID = e.EmployeeID  
)  
order by e.LastName  
  
--3  
--3p.) Całkowita liczba sztuk sprzedanych produktów w 1996, wyliczona w okresie od poprzedzającego do następ-  
--nego miesiąca. Wynik: Month, TotalQuantityForYear, TotalQuantityForTheMonths.  
with monthly as (  
select month(o.OrderDate) as Month,  
       sum(od.Quantity) as MonthlyTotal  
from dbo.Products p  
join dbo.[Order Details] od on p.ProductName = od.ProductID  
join dbo.Orders o on od.OrderID = o.OrderID  
where year(o.OrderDate) = 1996  
group by month(o.OrderDate)  
) select  
      Month,  
      sum(MonthlyTotal) over () as TotalQuantityForYear,  
      sum(MonthlyTotal) over (order by Month ROWS between 1 preceding  and 1 following) as TotalQuantityForMonths  
from monthly  
order by Month  
  
  
  
--4  
--(3p.) Wypisz 2 najstarszych pracowników, którzy sprzedali więcej różnych produktów (asortyment, nie sztuki) w 2  
--kwartale 1997 niż w 1 kwartale 1997 roku  
with pre_sql as (  
select e.EmployeeID as ID,e.FirstName as Name, e.LastName as Surname, month(o.OrderDate) as Month, count(distinct od.ProductID) as SoldMonthly  
from dbo.Employees e  
join dbo.Orders o on o.EmployeeID = e.EmployeeID  
join dbo.[Order Details] od on o.OrderID = od.OrderID  
where year(o.OrderDate) = 1997  
group by e.EmployeeID, e.FirstName, e.LastName, month(o.OrderDate)  
), pre_sql2 as(  
select  ID, Name, Surname,  
          sum(case when Month between 1 and 3 then SoldMonthly else 0 end) as Q1,  
          sum(case when Month between 4 and 6 then SoldMonthly else 0 end) as Q2  
from pre_sql  
group by ID,Name,Surname  
) select top 2 ID, Name, Surname  
from pre_sql2  
join dbo.Employees e on e.EmployeeID = ID  
where Q2 > Q1  
order by e.BirthDate  
  
  
  
  
-- Lista pracowników, którzy co najmniej raz sprzedali więcej niż przeciętną zamawianą ilość produktu 'Boston Crab Meat'. Wynik: imię, nazwisko  
with pre_sql as (  
    select p.ProductID, avg(od.Quantity) as AvgQuantity  
    from dbo.Products p  
    join dbo.[Order Details] od on p.ProductID = od.ProductID  
    join dbo.Orders o on od.OrderID = o.OrderID  
    where p.ProductName = 'Boston Crab Meat'  
    group by p.ProductID  
) select distinct e.FirstName, e.LastName  
from dbo.Employees e  
join dbo.Orders o on o.EmployeeID = e.EmployeeID  
join dbo.[Order Details] od on od.OrderID = o.OrderID  
join pre_sql on pre_sql.ProductID = od.ProductID  
where Quantity > AvgQuantity  
  
  
  
-- Lista produktów, dla których łączna zamówiona ilość jest wyższa w okresie maj-październik niż listopad-kwiecień. Wynik: nazwa produktu  
-- laczna ilosc zamowien produktow  
with pre_sql as (  
    select p.ProductID as PID, p.ProductName as Name,  month(o.OrderDate) as Month, sum(od.Quantity) as Sold  
    from dbo.Products p  
    join dbo.[Order Details] od on od.ProductID = p.ProductID  
    join dbo.Orders o on o.OrderID = od.OrderID  
    group by p.ProductID,p.ProductName,  month(o.OrderDate)  
) , pre_sql2 as (  
    select PID,Name,sum(case when Month between 5 and 10 then Sold else 0 end) as MayOctSold,  
           sum(case when Month between 1 and 4 or Month between 11 and 12 then Sold else 0 end) as NovAprilSold  
    from pre_sql  
    group by pid, name  
) select p.ProductName from dbo.Products p  
join pre_sql2 on p.ProductID = pre_sql2.PID  
where MayOctSold > NovAprilSold  
  
  
  
-- Lista zamówień zawierających co najmniej 5 różnych produktów, które nie były nigdy dostarczone do Francji. Wynik: pełne dane zamówienia  
with france as (  
    select p.ProductID  
    from dbo.Products p  
    join dbo.[Order Details] od on od.ProductID = p.ProductID  
    join dbo.Orders o on o.OrderID = od.OrderID  
    where o.ShipCountry = 'France'  
),filtered as (  
    select od.OrderID  
    from dbo.[Order Details] od  
    where od.ProductID not in (select * from france)  
    group by od.OrderID  
    having count(distinct od.OrderID) >=5  
)  
select *  
from dbo.Orders o  
join filtered f on o.OrderID = f.OrderID
```
