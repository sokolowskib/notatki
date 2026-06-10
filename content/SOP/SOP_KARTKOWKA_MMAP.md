# mmap ->

polecenie pozwalające na mapowanie między pamięcią programu a tak zwanym "obiektem pamięci".
USE CASE:

1. mapowanie "pamięć - plik", które umożliwia łatwe edytowanie pliku w sposób identyczny do tablicy
2. tworzenie pamięci współdzielonej z innymi procesami
3. "shared mapping"

# SYGNATURA

```
void *mmap(void * addr, size_t len, int prot, int flags, int fildes, off_t off);
```

addr -> wskaźnik na obszar pamięci, który chcemy mappować, jak podasz
**NULL**  to wypełni za ciebie
len -> wielkość pamięci
prot -> read / write / exe , goto `man 3p mmap`
flags-> masz trzy do wyboru:
\- `MAP_ANONYMOUS` -> nowy anonimowy obszar pamieci, fildes wtedy na -1
\- `MAP_SHARED` -> mapowanie dzielone z innymi procesami
\- `MAP_PRIVATE` -> mapowanie prywatne, tak działa `malloc()`
fildes -> deskryptor pliku, który chcemy zmapować
off -> offset pliku

Każda zmiana wykonana na pliku zmapowanym z `MAP_SHARED` będzie widoczna w innym procesie z tym samym deskryptorem.

Funkcje powiązane use casem z `mmap`:
\- `msync` -> `fflush` ale dla mapowania, inaczej zmiany mogą nie być natychmiastowe z powodu optymalizacji os'a. `man 3p msync`
\- `munmap` -> odmapowanie pamięci. `man 3p munmap`

# shm\_\* - pamięć dzielona

`mmap` działa tylko dla IPC z procesami potomnymi. A co jak procesy są ze sb niezwiązane? `shm_*`.

`shm_open` -> tworzy nazwany obiekt pamięci dzielonej. Na początku tworzy się z zerowym rozmiarem, więc trzeba wywołać `ftruncate`. Potem tworzysz normalne mapowanie `mmap`.

Funkcje powiązane:
\- `shm_open`-> tworzy obiekt, `man 3p shm_open`
\- `shm_unlink` -> zamyka obiekt, `man 3p shm_unlink`
\- `ftruncate` -> do inicjalizacji, `man 3p ftruncate`

# SYNCHRONIZACJA

Pracując na tym samym deskryptorze wymagana jest synchronizacja. Można umieścić semafory i mutexy w pamięci współdzielonej.

### SEMAFOR: `man 3p sem_init`

1. Jako drugi parametr `pshared` ustawiasz wartość większą niż zero, to będzie on działał też dla wielu procesów.
2. Wstawiasz więc gdzieś do pamięci współdzielonej `sizeof(sem_t)` wskaźnik na semafora z `pshared > 0` i działa.

### MUTEX : `man 3p pthread_mutexattr_getpshared`

Gdyby proces wywalił się w sekcji krytycznej, to nie mógłby go odblokować, tworząc deadlock. Wykorzystuje się więc ekstra atrybut `pthread_mutexattr_setrobust`(  `man 3p pthread_mutexattr_getrobust`).
Dodaje on ekstra flagę błędu `EOWNERDEAD`, którą otrzymuje proces czekający na mutexie na zawsze zablokowanym przez wykolejony proces. Wtedy wymagana jest naprawa mutexa funkcją `pthread_mutex_consistent`.

Podobnie wygląda sytuacja z condvarami i barierami (nie mają one tych samych problemów co mutexy). `man 3p pthread_condattr_getpshared` , `man 3p pthread_barrierattr_getpshared`.
