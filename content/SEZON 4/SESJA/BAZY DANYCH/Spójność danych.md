Spójność danych ma zapewnić wysoką jakość danych przechowywanych w bazie.

## Spójność encji

W.w. spójność ma zapewnić unikalność klucza głównego w tabeli.
W rozwiązaniu tego problemu są wykorzystywane unikalne indeksy, które mają na celu szybko sprawdzić jego istnienie. Następnie [[DBMS]] monitoruje wszystkie wyrażenia update/create , żeby utrzymywać unikalność PK.

## Spójność referencyjna

Dokładniej wytłumaczone w [[Etapy Normalizacji#Spójność referencyjna]].
Rozwiązanie polega na zdefiniowaniu primary key i foreign key w tablicy powiązanej, tworząc relację.

[[Zastępczy klucz główny]] to praktyka wykorzystywana we współczesnych bazach danych.

## Dodatkowe wymogi DBMS

Możliwe jest zdefiniowanie dodatkowych wymogów, jakie kolumny tabeli muszą spełnić.
Te wymogi są sprawdzane przy każdej operacji CRUD.
Są one jednak zależne od lokalnej konfiguracji, przez co wykorzystywane są raczej po stronie aplikacji klienckich (więcej w [[DBMS]])
