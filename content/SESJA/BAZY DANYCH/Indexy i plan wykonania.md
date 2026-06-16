Struktura on-disk związana z tabelą czy widokiem. Ma służyć do przyspieszenia procesu wyciągania rekordów z tabeli.

Do implementacji indeksów zazwyczaj wykorzystuje się B-drzewa.

**Tworzenie indeksów należy do projektowania fizycznego modelu danych.**

## Indeksy zgrupowane

Po ang. _clustered index_, narzucają na DBMS przechowywanie danych na dysku w sposób posortowany wg. wybranych kolumn/kolumny. Może istnieć maksymalnie jeden indeks zgrupowany na jednej tabeli. Zazwyczaj rekordy są przechowywane w B-drzewie, liście zawierają strony danych.

**Indeksy zgrupowane wymagają większej złożoności operacji update/insert, ponieważ możliwa jest zmiana kolejności zapisu rekordów.**

## Alokacja tabeli niezgrupowanej

Gdy nie ma indeksu zgrupowanego, rekordy są przechowywane na stercie. Nie ma porządku zapisu danych, więc modyfikacja oraz dodanie danych jest szybsze.

## Tworzenie indeksów w MSSQL

```SQL
create [unique] [clustered | nonclustered] index name on object(column1,...)
```

## Indeksy niezgrupowane

Dodatkowa struktura przechowywana obok tabel. Może ich być wiele, każdy wymaga własnej przestrzeni na dysku.

Liście zawierają tylko pointer do miejsca, gdzie rekordy są przechowywane w tabeli, DBMS musi wykonać `key lookup`,  żeby odczytać dane rekordu.

## Kategorie indeksów

1. zgrupowane -> definiujemy jak dane są przechowywane na dysku
2. niezgrupowane stworzone na stercie -> `key lookup` do struktury nieposortowanej
3. niezgrupowane stworzone na indeksie zgrupowanym -> `key lookup` do struktury posortowanej

## Klucz indeksowy

Klucz indeksowy to wartość, po której sortujemy strukturę indeksu. W przypadku indeksów niezgrupowanych mogą być one niewystarczające żeby odpowiedzieć na zapytanie, dlatego musi być wykonany wspomniany wyżej `key lookup`. MSSQL pozwala na dodanie ekstra kolumn do klucza indeksowego żeby móc odpowiedzieć bezpośrednio na więcej kwerend samym indeksem.

## Główny use-case indeksów

Kwerendy z warunkiem `where` albo `join`, lub sortowanie danych `order by`.

## Negatywne cechy indeksów

Po stworzeniu indeksu na tabeli, odpowiedzialnością DBMS staje się utrzymywanie tego indeksu, co może powodować znaczący overhead w trakcie poleceń DML.

## Indeks unikalny

Struktura stworzona przez DBMS zazwyczaj do szybszego sprawdzania unikalności `primary key` jakiejś tabeli.

## Indeksy - yay i nay

Yay:

- żeby utrzymywać unikalność primary key
- żeby zwiększyć wydajność sprawdzania spójności referencyjnej
- gdy oczekiwana jest duża ilość rekordów w tabeli (> 1000)
- kolumny są wykorzystywane w operacjach wyszukiwania i sortowania
- rozmiar kluczu indeksowego jest mały
  Nay:
- bardzo duża ilość poleceń DML na tabeli, która indeksujemy
- słaba selektywność danej kolumny, np: przyjmuje tylko dwie wartości

## Plan wykonania

Żeby odpowiedzieć na skomplikowane zapytanie, DBMS tworzy algorytm jego wykonania, a następnie go implementuje. Śledząc ten algorytm, czyli _execution plan_, możemy przeanalizować jak przetwarzane i wyszukiwane są nasze dane, pozwalając na utworzenie potrzebnych indeksów.

## Automatyczne indeksy w Oracle DBMS

W 19c zostały dodane automatyczne indeksy. DBMS w trakcie poleceń może:

- wymyśleć nowy indeks na podstawie wykorzystanych kolumn
- stworzyć niewidzialny indeks do rozważenia potem
- zamienić go w widoczny gdy kwerenda okaże się szybsza
  Nagła raptowna zmiana w tabeli może zaburzyć wyniki statystyk poleceń SQL wykorzystywanych do tworzenia automatycznych indeksów, co może poskutkować wydłużonym czasem wykonywania.

## Automatyczny tuning w MSSQL

Podobnie jak wyżej, DBMS może zidentyfikować problematyczny plan wykonania i zaproponować nowy plan lub indeksy do dodania/usunięcia.
