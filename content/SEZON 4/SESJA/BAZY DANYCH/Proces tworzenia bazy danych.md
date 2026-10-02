Koncept: system bazodanowy to inwestycja, ma być zaprojektowany jak najlepiej by zwiększać zysk i obniżyć koszty. Jego wartość można ocenić za pomocą metryk:

- ROI - return on investment
- NPV - net present value (ile w danym momencie zysku przynosi system programistyczny)
  Ważne żeby design był jak najlepszy.

## Koncepcyjny design bazy danych - kroki

1. Znajdź zakres i granice systemu -> dokument zawierający informacje na temat co system ma osiągnąć, co ma być / nie być w systemie, z jakimi innymi systemami ma współpracować
2. zidentyfikuj najważniejsze _procesy biznesowe_, które system ma wspierać (mają być jak najbardziej zoptymalizowane) -> zautomatyzowane przygotowanie, zmniejszone ryzyko błędu w procesie, informacje  na temat procesu są dostępne. **UNIKAĆ PODZIELONEGO OBSŁUGIWANIA PROCESU BIZNESOWEGO MIĘDZY SYSTEMAMI- ALBO CAŁOŚĆ ALBO WCALE**
3. poznaj zasady biznesowe -> jak wygląda faktura, kto może z czym wchodzić w interakcje, czy można usuwać jakieś dokumenty
4. zidentyfikuj obiekty danego biznesu -> wynik to lista encji (rzeczy ze świata rzeczywistego, na temat której dane będą przechowywane zazwyczaj w tabeli)
5. normalizacja encji -> [[Etapy Normalizacji]]
