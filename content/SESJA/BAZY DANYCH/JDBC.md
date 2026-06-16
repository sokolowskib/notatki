_Java Database Connectivity_ - API do łączenia się z DBMS ze strony aplikacji klienckiej.
Składa się z `java.sql`, czyli paczki dodającej główne klasy oraz interfejsy, oraz `javax.sql`, która zawiera dodatki po stronie strony serwerowej.

### Dlaczego to jest mocne

- wspiera prawie wszystkie dialekty SQL, ale również inne źródła danych, takie jak pliki z danami tabularnymi
- aplikacja ma dostęp do prawie każdego źródła danych i może być wywołana z każdej maszyny z JVM.

## Nawiązywanie połączenia z bazą danych

### Ładowanie specyficznego sterownika JDBC

Plusy:

- to samo API do wielu implementacji DBMS, bo sterownik sam ogrania połączenie, handshake i specyficzna implementacje dla tego DBMS
  Minusy:
- driver zhardcode'owany w aplikacje, wymaga przepisanie oraz rekompilacji w przypadku zmiany.
- utworzenie oraz zamknięcie połączenia jest kosztowne, wymagany connection pooling.

#### Connection pool

Robienie za każdym razem `DriverManager.getConnection` jest  drogie, więc trzyma się pule otwartych połączeń po stronie serwera.

![[obrazki/Pasted image 20260615191835.png]]

## Wykorzystanie data source

W pliku XAML definiujesz connection stringa, nazwę tego "obiektu" na twojej maszynie a następnie wsadzasz do procesu nadrzędnego. Teraz nie masz tego zhardcode'owanego w kodzie oraz nie musisz samemu nawiązywać połączenia z bazą danych.
Plusy:

- takie samo API na różne bazy danych
- Nie ma hardcode'd danych o DBMS
- łatwy connection pool
  Minusy:
- twoja aplikacja musi być na tej samej maszynie co ten proces nadrzędny

![[obrazki/Pasted image 20260615193054.png]]

## Prepared statement

```java
PreparedStatement ps = con.prepareStatement("update job set min_salary = ? where job_title = ?");
ps.setInt(1,100);
ps.setString(2, "cos");
```

Przyspiesza wielorazowe wykorzystanie tej samej "templatki" polecenia SQL tworząc zprekompilowane wyrażenie, zapobiegając SQL injection attack.

### try-with-resources

Jako że zasoby mają być zwalniane ASAP (żeby uniknąć leaku), to można wykorzystywać strukturę `try()`, żeby jak najszybciej zamykać to połączenie/zasób.
