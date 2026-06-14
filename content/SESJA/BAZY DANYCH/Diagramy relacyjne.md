Istnieją w celu komunikacji między ludźmi od biznesu a informatykami na temat poprawnego zrozumienia modelu biznesowego. Jest to mniej techniczne przedstawienie struktury zaprojektowanego przez nas systemu bazodanowego, istniejące żeby sprawdzić / poprawić nasze zrozumienie na temat encji, procesów biznesowych, zasad działania itp.

## Terminologia:

_encja_ - obiekt ze świata rzeczywistego, np.: pacjent, klient
_klasa encji_ - zbiór encji o tej samej cesze, który pozwala na ich wspólną kategoryzację
_atrybut_ - dana cecha / fakt o danym przykładzie encji.

## Diagramy ER (encja - relacja)

Opisują encje, ich atrybuty oraz relacje. Pozwalają na dokładny opis modelu danych oraz struktury bazy. Wykorzystywane praktycznie przez cały life-span bazy, do prawie wszystkich decyzji.
Logiczny model danych może być stworzony za pomocą diagramu ER.

### Podejścia do projektowania baz danych

![[obrazki/Pasted image 20260613165902.png]]

Top down -> tworzysz coś nowego, zazwyczaj z opisu wymagań jakiegoś biznesu, z ogólnego opisu przechodzisz do detali
Bottom up -> zazwyczaj w celu zastąpienia czegoś istniejącego, od specyficznych use-case'ów bazy przechodzisz do ogólniejszego zrozumienia potrzebnych encji i ich atrybutów.

## Kardynalność relacji

![[obrazki/Pasted image 20260613170716.png]]

Opisy przy relacjach oraz nazwy encji powinny być dokładne. ![[obrazki/Pasted image 20260613170852.png]]

Relacja rekurencyjna może być udokumentowana przez zdefiniowanie klucza obcego nawiązującego do klucza głównego tej samej tabeli.

Każdy atrybut powinien mieć zdefiniowaną własną domenę, czyli np.: numery telefonu z czy bez kodu kraju.

## Podtypy encji

Jeśli w encji znajdują się podtypy, trzeba je przedstawić na diagramie (wszystkie!). Często wchodzą one w relację z innymi encjami, co można przedstawić na diagramie by uzyskać dokładny model danych, np.:
![[obrazki/Pasted image 20260613171425.png]]
Tutaj finished order wchodzi bezpośrednio w relację z fakturą.

## Relacje mtm (many to many)

Dozwolone w przypadku koncepcyjnego modelu danych, rozwiązywane ekstra encją typu x\_x w DBMS. Zawiera ona przynajmniej dwa klucze obce, każdy do innej tabeli z relacji mtm.

## Relacje wykluczające się

Nie mogą zachodzić jednocześnie. Jeśli zakończyliśmy zamówienie, klient zapłacił albo kartą albo gotówką. ![[obrazki/Pasted image 20260613171734.png]]
