Często praktykowanym rozwiązaniem jest tworzenie osobnej kolumny zwanej _surrogate key_, która ma za zadanie zastąpić naturalne klucze główne. Zazwyczaj definiowany przez autoinkrementowalną liczbę (int IDENTITY(1,1)). Nie mają nic wspólnego ze światem rzeczywistym. Wykorzystywane  np. gdy:

1. nie istnieje naturalny klucz główny
2. klucz główny to klucz złożony

**W przypadku użycia surrogate key wymagana jest spójność referencyjna.**
