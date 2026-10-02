## Podstawowe zasady:

- przywileje, które są ci niepotrzebne powinny zostać wyłączone
- nie używane konta użytkownika powinny być blokowane
- powinno się stosować wbudowanych rozwiązań DBMS, nie od nowa wymyślać koło w kodzie aplikacji klienckiej

## Techniki specyficzne dla Oracle Database

- każdy użytkownik ma swoją listę uprawnień
- istnieje funkcja _Roles_, czyli lista uprawnień. Istnieją:
  - System defined roles, takie jak _DBA_ albo _Connect_
  - user defined roles, czyli takie jakie sobie zdefiniujesz
- uprawnienia, kategoryzujemy:
  - uprawnienia systemowe, czyli np. `create table` albo `grant any priviliges`
  - uprawnienia obiektowe, czyli np. czy użytkownik ma dostęp do danej tabeli
- profile, powiązane z kontem użytkownika, wykorzystywane do tworzenia jakiś polityk haseł

## Zarządzanie użytkownikami

```sql
--odblokowywanie/blokowanie usera
alter user [name] account [lock | unlock]

--listujesz użytkowników wraz z ich statusem
select username,account_status from dba_users

revoke execute on utl_file from Public
```

Cokolwiek ustawisz na pseudo-userze Public, zmienia się u wszystkich użytkowników
Utl\_file pozwala na odczytywanie plików tekstowych systemu operacyjnego.

## Profile

Zbiór reguł/zasad nakładanych na danego użytkownika. Nakłada je się głównie po to by:

- ograniczyć ilość CPU, którą user może użyć, żeby jego kwerenda nie blokowała bazy
- ustawić limit prób zalogowania się, żeby nie móc brute-force'ować hasła

## Audit

Monitorowanie aktywności użytkowników, głównie:

- dotykanie niektórych obiektów w DB
- dostęp do poufnych plików
- próby logowania się do konta
- akcje administratora
  **Audyty negatywnie wpływają na wydajność.**

#### Konfiguracja

```sql
alter system set audit_trail = [none | false | os | db | true| db_extended]
```

none lub false - audyt wyłączony
os - ślad audytu zapisywany do _trail file_ systemu operacyjnego
db lub true - do zapisu jest wykorzystywana tabela Sys.Aud\$
db\_extended - zapisywane jest całe zapytanie SQL wraz z zmiennymi wiążącymi

Zmienne wiążące to ta zaślepka z prepared statement, np.:

```java
ps = 'select ...... where id = :id' - > to jest zaślepka
ps.setInt(1,100)
```

#### Przykłady

```java
audit create any trigger
audit session
audit insert on hr.devs
audit session whenever not successful
```

Audyt jest zapisywany w widoku `dba_audit_trail` w tabeli `sys.aud$`. Istnieją jeszcze inne, takie jak:

- `dba_audit_object` - na jakich obiektach
- `dba_audit_statement` - jakie statementy
- `dba_audit_session` - próby zalogowania
  Można jeszcze dodawać triggery, wywoływane np. na edycji danej kolumny na jakąś wartość.
  Można bardzo dużo tutaj poustawiać wg. swoich potrzeb, ale nie pomoże na `Selecty`.

## Fine grained auditing

Służy do monitorowania SQL Select i wyrażen DML. Można monitorować:

- tylko dane tabele
- tylko dane kolumny
- tylko dane rekordy
- tylko niektóre wyrażenia z listy
  Osiągane przez modyfikację polityki FGA w paczce DBA\_FGA

## Unified auditing

Jako że jest mega dużo tych różnych sposobów audytowania, _unified auditing_ zbiera te wszystkie logi w jedno miejsce, do schemy `audsys` do przestrzeni tabel `sysaux`

## Niebezpieczne praktyki

- tworzenie kopii danych wyjętych z miejsc pracy użytkowników
- przechowywanie danych na przenośnych urządzeniach
- tworzenie aplikacji zależnych od jednego użytkownika z długą listą uprawnień
- nie zapobieganie SQL injection
- robienie kodu, który może wywołać polecenie SQL jeśli proszony
