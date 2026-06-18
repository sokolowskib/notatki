Geobazy służą do przechowywania rekordów, które zawierają koordynaty geograficzne wraz z przetwarzaniem danych przestrzennych. Zazwyczaj dane są przechowywane w postaci map numerycznych, czyli zbioru danych na temat różnych obiektów. Są przechowywane w formie warstw, czyli zbioru obiektów o tej samej kategorii.

## Rodzaje warstw

- rastrowe - skan map papierowych, zdjęcia satelitarne
- wektorowe - zbiór obiektów, posiadających własną geometrię, topologię i koordynaty.
  W odróżnieniu do warstwy rastrowej, warstwa wektorowa może być wyświetlona w każdej wielkości bez utraty jakości obrazu.

## GIS - definicja

_Geographical Information System_ to zbiór sprzętu, programów oraz danych służący do przyjmowania, przechowywania, przetwarzania oraz wyświetlania danych geograficznych. Zarządza geobazami i służy do wyświetlania map z warstw rastrowych i wektorowych. Przykłady:

- ESRI
- Autodesk
- Bentley Systems

## Jak działa

![[obrazki/Pasted image 20260617114654.png]]

Analogicznie do aplikacji klienckich i DBMS.

## Warstwy wektorowe

Zawierają różne rodzaje kształtów, linie łamane, wielokąty i punkty. Problematyczne natomiast są różne rodzaje współrzędnych.

## Format przechowywanych danych

Prostym rozwiązaniem jest przechowywanie plików (np. rozszerzenia CAD). Problemy pojawiają się przy edycji, ponieważ tylko jeden użytkownik może edytować jeden plik. W dodatku jedna instalacja może składać się z tysięcy takich plików. Rozwiązaniem branżowym jest zwykła baza relacyjna wraz z rozszerzeniem do przetwarzania danych przestrzennych.

**QGIS** - darmowa platforma GIS.

## Funkcje GIS

Można wyszukiwać nie tylko po danych geograficznych (odległość), ale też po opisach obiektów (np. z czego zrobiony jest dany obiekt).

## Przykład architektury systemowej

![[obrazki/Pasted image 20260617121518.png]]

Desktop - modyfikacja oraz analiza
Web client - przeglądanie oraz wyszukiwanie po mapach
GIS - zarządzanie oraz utrzymywanie danych
