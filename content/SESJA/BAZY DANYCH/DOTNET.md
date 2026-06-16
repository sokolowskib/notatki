W C# zamiast JDBC istnieje ADO.NET, czyli framework do łączenia się do bazy danych.

### Entity framework Core

Alternatywa do ado.net. Oferuje rozwiązanie object-relational mapper (O/RM), czyli mapowanie rekordów bazy relacyjnej do  obiektów istniejących w C#. Istnieją też takie rozwiązania w Javie.

## Data providers w Dotnet

Działają jak sterowniki w JDBC.

![[obrazki/Pasted image 20260615195705.png]]

ODBC to generyczny standard, który może być implementowany przez wiele baz lub źródeł danych, np.: Excel lub wszelakie inne bazy.

## using ()

Odpowiednik [[JDBC#try-with-resources|try with resources]] w Javie. Jak działa wiadomo.

## Ustawienia możliwe do zmiany w ConnString w MSSQL

- data source - do jakiej instancji serwera się połączyć
- Connection timeout - po ilu sekundach nieudanego połączenia aplikacja ma się rozłączyć
- Encrypt - jak na tak to połączenie ma enkrypcje SSL
- Failover Partner  - jak baza jebnie to tu dajesz backup
- Database - nazwa bazy
- Integrated Security - jak dasz na true to wykorzystuje konto Windows, z którego wykonywana jest ta aplikacja jako źródło loginu oraz hasła w auth.
- MultipleActiveResultSet - jak tak to jeden conn może trzymać kilka ResultSetów, nie polecane
- Network Library - jaki rodzaj połączenia sieciowego z bazą
- Pwd - chujowe, używaj IntegratedSecurity
- UserId - sql server login, chujowe

## Connection pool

Gdy connection string jest otwarty i nie ma żadnego otwartego connection pool, nowy jest otwarty. Dlatego każdy connection string jest powiązany z jakimś connection pool. Można skonfigurować minimalną ilość połączeń w poolu.

## IDbConnection i IDbCommand

Wykorzystywane są interfejsy, a nie SQLCommand, żeby kod był przenośny na wiele DBMS.

![[obrazki/Pasted image 20260615212004.png]]

Kod chroniący przed SQL injection.

## Oracle Bazadanych a Dotnet

Zmienia się tylko SQLConnection na OracleConnection. Jako że piszemy generyczny kod pod interfejsy, pisanie kwerend wygląda praktycznie identycznie. Tak samo jak w MSSQL, dostępny jest pooling.
