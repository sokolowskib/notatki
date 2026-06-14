_Normalizacja_ -> proces projektowania relacji, celujący w:

1. uniknięcie redundancji w tabelach
2. uniknięcie anomalii w trakcie modyfikacji tabeli
3. zdefiniowanie encji oraz jej atrybutów
   Proces normalizacji wpływa na atrybuty oraz ilość encji

## 1NF

Encja jest w pierwszej formie normalnej gdy:

1. jest już relacją
2. klucz główn![[obrazki/Pasted image 20260613154522.png]]y istnieje w tabeli
3. wszystkie wartości w tabeli są atomowe

Uniknięcie nieatomowych wartości polega na podzielenie listy atrybutów na wiele rekordów, kolumn lub wielu encji.

### Spójność referencyjna

Żeby osiągnąć spójność referencyjną trzeba zapewnić, że każda wartość klucza obcego ([[Klucz obcy]]) widoczna w danej tabeli istnieje w formie klucza głównego w innej, powiązanej z nią tabeli.

Spójność referencyjna może być narzucona przez [[DBMS]], co nadaje restrykcje na wsadzanie/modyfikację danych w tabeli, np.:

- wsadzenie wartości do tabeli z kluczem obcym, jak nie istnieje taka wartość klucza głównego w powiązanej tabeli nie przejdzie
- usunięcie rekordu z kluczem głównym istniejącym jako klucz obcy w innej tabeli się nie powiedzie
- Modyfikowanie klucza głównego / obcego powodując że stara wartość odnosi się do nieistniejącego rekordu nie przejdzie

**JAKAKOLWIEK ZMIANA NARUSZAJĄCA SPÓJNOŚĆ REFERENCYJNĄ NIE JEST DOZWOLONA Z POZIOMU DBMS**.

## 2NF

Encja jest w drugiej formie normalnej gdy:

1. Jest w pierwszej formie normalnej
2. Gdy każda kolumna nie będąca kluczem głównym zależy od całego klucza głównego
   ![[obrazki/Pasted image 20260613153039.png]]

### Na co mi to?

Jeśli tylko część kolumn zależy od całego klucza głównego, ta druga część nie zależy od samej encji. Wiele rekordów zawiera wtedy niepotrzebne dane (redundancja) co prowadzi do niespójności. Co jeśli kilka rekordów ma tego samego klienta, a chcemy tylko zmienić adres domowy tego klienta? => wiele rekordów musi być modyfikowanych.

## 3NF

Encja jest w trzeciej formie normalnej, gdy:

1. jest w drugiej formie normalnej
2. kolumny nie będące w kluczu są od siebie niezależne
   ![[obrazki/Pasted image 20260613153444.png]]

W tym przypadku employee e\_mail jest redundantne, tak samo jak w przykładzie z 2NF. Ten sam problem z modyfikacją emaila pracownika.

## 4NF

Encja jest w czwartej formie normalnej, gdy:

1. Jest w 3 formie normalnej
2. Nie ma nakładających się [[Klucze kandydackie]] (jeśli klucze kandydackie to {A;,B} i {A; C})
3. nie ma MVD (multivalued dependency) -> wartość atrybutu A jednoznacznie wyznacza zbiór wartości B i C, ale B i C są niezależne, np.:
   ![[obrazki/Pasted image 20260613154407.png]]
   Imię jednoznacznie wyznacza Języki oraz Hobby, ale nie są ze sobą powiązane

4nf nie jest zazwyczaj używana przemysłowo, industry standard to 3nf.

![[obrazki/Pasted image 20260613154523.png]]
