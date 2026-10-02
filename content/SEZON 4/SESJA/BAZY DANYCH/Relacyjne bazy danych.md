Wspomniane bazy danych na podstawie teorii matematycznych (podobno) w najbardziej wydajny sposób rozwiązują problem jednoczesnego dzielenia oraz edycji zbioru danych przez wiele osób.

## Jakie rozwiązania oferuje relacyjna baza danych:

1. zwiększenie produktywności
2. uniknięcie niejednoznaczności poprzez udostępnienie sprecyzowanych specyfikacji
3. zwiększenie niezależności programu oraz jego danych

## Definicja relacyjnej bazy danych

Zbiór struktur danych, które mają przechowywać i uporządkować dane.

**RELACJA** - inaczej tabela, ma następujące cechy:
1\. każda ma unikalną nazwę
2\. każda kolumna w tabeli ma unikalną nazwę
3\. Każda wartość w danej kolumnie jest tego samego typu.
4\. Kolejność kolumn w tabeli nie ma znaczenia
5\. Każdy rekord w tabeli jest unikalny (nie ma duplikatów)
6\. Kolejność rekordów nie jest istotna
7\. Rekordy nie są kategoryzowane przez numer rzędu
8\. Tylko atomowe wartości mogą być przechowywane w danej kolumnie (zakaz zbiorów lub podtabel)

Jak rozwiązywalna jest unikalność rekordów => [[Klucz główny]]

## Wartość NULL

W relacyjnych bazach danych wartość null jest nie równa wartości zero lub pustemu miejscu. Zazwyczaj jest wykorzystywana w przypadkach, gdy dany fakt na temat jakiegoś rekordu w tabeli jest nieznany lub niedostępny. **Użycie wartości null zmienia logikę wykonywania query szukającego/dodającego dane z tej kolumny, trzeba uważnie rozpatrzyć jej use case.**
np.: każde porównanie z null zwraca false
