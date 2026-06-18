_OLTP_ -  Online Transaction Processing system, służy do przetwarzania transakcji z dnia na dzień w biznesach, skupia się na wspieraniu wielu transakcji współbieżnych.
_OLAP_ - Online Analytical Processing system, służy do podejmowania decyzji biznesowych na podstawie danych umieszczonych w bazie. Opiera się na innych modelach danych niż w OLTP.

## Hurtownia danych

Pomysł polega na ekstrakcji, transformacji i załadowaniu (ETL) lub ekstrakcji, załadowaniu i transformacji (ELT) z wielu baz do hurtowni. Dane do hurtowni mogą być przesyłane np.: na koniec dnia w batchach. Hurtownie danych zazwyczaj zawierają wiele wysokiej jakości, legacy informacji już nie potrzebnych do prowadzenia transakcji w systemach OLTP. Głównym celem jest analiza trendów w firmie na podstawie dostępnych danych.

## Problemy wydajnościowe

W odróżnieniu od OLTP, hurtownie są optymalizowane pod wyszukiwanie, dlatego dane często są:

- upraszczane
- zdenormalizowane

![[obrazki/Pasted image 20260616124514.png]]

## Business Intelligence

Bierze danych z hurtowni i pokazuje trendy albo wnioski biznesowe. Używane przez ludzi w garniturkach żeby podejmować decyzje.

## Słowa kluczowe

- fakt - element danych mierzący wydajność biznesu (rekordy ze sprzedaży, produkcji itd.)
- wymiar - atrybut lub zbiór atrybutów wykorzystywany do grupowania faktów, np: czas wykonywania, miejsce lub organizacja
- miara - numeryczna wartość przypisywana faktom, aby ocenić wydajność, np. wartość sprzedaży w jakiejś walucie
- tabela faktów - zawiera fakty oraz kilka atrybutów i miar ich dotyczących, np.: data powstania rekordu, kilka miar jakichś atrybutów, klucze obce do tabeli wymiarów
- tabela wymiarów - zawiera opisy danych w tabeli faktów, np. jeśli w tabeli faktów miasta są oznaczone jako numery, tabela wymiarów miast zawiera słownik numer - miasto
  ![[obrazki/Pasted image 20260616125849.png]]

## Kostka danych

Struktura danych wykorzystywana przez systemy OLAP. Każda komórka odpowiada jakiejś wartości miary, a każdy bok odpowiada jakiemuś wymiarowi atrybutów.
![[obrazki/Pasted image 20260616130054.png]]

Tutaj komórka reprezentuje jakąś miarę ceny, natomiast wymiary to Produkt, Czas i Lokalizacja.

Dla kombinacji miary i wymiaru definiuje się funkcję agregującą, najczęściej sumę.

## Cykl życia rozwiązania BI

![[obrazki/Pasted image 20260616135256.png]]
