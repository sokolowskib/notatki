## Architektura Oracle RDBMS

Sama baza danych to tylko pliki na dysku. W Oracle instancja jest tym, co wykonuje rzeczywiste operacje. Wygląda ona tak:

![[obrazki/Pasted image 20260616185000.png]]

Instancja składa się z :

- _SGA_ - system global area, współdzielona pamięć RAM. Siedzą w nim bufory bloków danych.
- _PGA_ - program global area, pamięć prywatna. Każdy server process ma swoją własną PGA.
- Server process - proces przyznany jednemu klientowi. To on obsługuje jego zapytania.
- Background process - służy do utrzymywania bazy danych w spójnym stanie, pisze zmiany z SGA na dysk, pilnuje logów, nawraca stan po awarii.

**Klient tak naprawdę nigdy nie dotyka plików. Obsługa jego zapytań jest przydzielona server procesowi.**

## Oracle Net Services

Warstwa łącząca procesy klienta z procesami serwera przez sieć. Oracle Net (lub np. JDBC czyli moduł symulujący) zawarty jest na każdej maszynie klienckiej. Zawiera:

- komponent w tle, który pozwala na połączenie sieciowe między instancją bazy Oracle a klientem, chowający overhead komunikacji sieciowej
- proces słuchający, który umożliwia komunikację między klientem a bazą.
  Podstawą jest _TNS_ czyli Transparent Network Substrate.  Klient nie zna protokołu sieciowego wybranego pod spodem, dlatego Transparent.
  ![[obrazki/Pasted image 20260616190749.png]]

Tak wygląda flow połączenia między klientem a bazą danych. Za pomocą `tnsnames.ora` sprawdza, pod jakim adresem IP siedzi baza danych, następnie nawiązuję z nią połączenie. Listener nasłuchuje na porcie. Po odebraniu nowego połączenia startuje server process dla klienta oraz przekierowuje sesje danych pomiędzy klientem do server procesu.

## Nawiązanie połączenia

- easy connection - dosłowny connection string, łatwy w użyciu, ale nieprzyjemny jak robi się długi
- net service name - wykorzystanie aliasu, który jest tłumaczony przez `tnsnames.ora`

![[obrazki/Pasted image 20260616192105.png]]

opisane dokładnie to co napisałem wcześniej. plik `tnsnames.ora` . Plik ma zapisane ustawienia połączenia.

### Metody nazewnictwa

- local naming - pliki tnsnames.ora mapują alias do ip/portu
- directory naming - pliki z nazewnictwem siedzą w jakimś LDAPIE, ale nie mam pojęcia co to jest i jak działa, robiony przez administratora chuj wie czego
- non oracle naming - jakieś inne serwisy rozwiązują nazewnictwo

### Pliki konfiguracyjne

- sqlnet.ora - jak rozwiązywać aliasy nazewnictwa
- tnsnames.ora - definiuje lokalne nazewnictwo
- listener.ora - lista wszystkich procesów słuchających na serwerze oraz listę instancji obsługiwanych przez każdego listenera.

### Dynamiczna vs statyczna rejestracja

`SID_LIST` to lista instancji Oracle, jakimi dany listener się opiekuje. Instancje muszą działać, żeby listener do nich przekierował. Skąd ta lista?

1. rejestracja statyczna - admin wpisał listę do pliku konfiguracyjnego
2. rejestracja dynamiczna - instancja sama rejestruje się w liście listenera. Jak instancja przestaje działać, to wypisuje się z tej listy.

## Zaawansowane opcje w `tnsnames.ora`

- connect time failover - alias ma kilku listenerów spisanych, jeśli jeden sie nie uda, próbuje drugiego
- load balancing - alias ma kilku listenerów, wybiera losowego
- timeout
- transparent application failover - jak wyjebie sie instancja to automatyczny reconnect do następnej

#### Dodatek - narzędzia do listenerów

- isnrctl - do zarządzania listenerami
- tnsping - ping ale do listenera
