1. JAK DZIALA EPOLL? WYWOLANIE SYSTEMOWE
2. POROWNAJ METODY OBSLUGI KLIENTOW PRZEZ SERWER:
   -watki
   -procesy
3. ROLA LINKERA? CO NA WYJSCIU I WEJSCIU. JAKIE TRANSFORMACJE WYKONUJE
4. DIAGRAM NAJPROSTSZEGO MMU bez dostepu do pamieci innego procesu
5. ![[obrazki/Pasted image 20260606154751.png]]
6. ![[obrazki/Pasted image 20260606154815.png]]
7. wytlumacz pojecia:
   -fizyczna przestrzen adresowa
   -logiczna przestrzen adresowa
   -wirtualna przestrzen adresowa
8. porownaj segmentacje ze stronicowaniem
9. zadanie 5 z powrotem: ![[obrazki/Pasted image 20260606155007.png]]
10. ![[obrazki/Pasted image 20260606155031.png]]
11. 3 rozne sposoby alokacji miejsca w systemie plikow:
    -ciagly
    -listowy
    -indeksowy
12.

0x0510  - 0000 0101 0001 0000 -> ramka 10, offset: 01 0001 0000
0010100100010000 -> adres fizyczny
0x2910 -> fizyczny

0xBAAD -> 1011 1010 1010 1110 -> nie ma w tabeli, nie zadziala, proba odwolania sie do pamieci ktora nie jest przydzielona do procesu

4kib -> 12 bitach

4 pierwsze bity -> strona
12 bitow ostatnich -> offset
0x2a72 -> strona 0010
offset -> 1010 0111 0010

frame 8 -> 1000
0x8a72
