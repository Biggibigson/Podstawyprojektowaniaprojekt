# SPRAWOZDANIE – MSYS2

## Cel projektu

Celem realizacji zadań było praktyczne wykorzystanie środowiska **MSYS2** wraz z narzędziami systemu Unix, takimi jak `cat`, `paste`, `awk`, `sed`, `diff`, `md5sum`, `magick` i inne. Skupiono się na przetwarzaniu plików tekstowych, graficznych oraz pracy w terminalu `bash`, co miało na celu pogłębienie znajomości pracy w środowiskach uniksowych.

---

## Zadanie 3 – Formatowanie danych (`cat`, `paste`, `echo`)

### Cel zadania

Zadanie polegało na odpowiednim przekształceniu pliku tekstowego z danymi, aby umożliwić jego łatwy import do programu Excel. Dodatkowo należało podzielić zawartość na trzy kolumny oraz dodać nagłówki X Y Z.

### Sposób realizacji

- Za pomocą polecenia `cat` sprawdzono treść pliku `dane.txt`.
- Komenda `paste - - - < dane.txt` została użyta do rozdzielenia danych na trzy kolumny.
- Instrukcja `echo "X Y Z"` posłużyła do dodania nazw kolumn.

### Wnioski

Użycie narzędzi takich jak `cat`, `paste` oraz `echo` pozwala w prosty sposób manipulować danymi tekstowymi i przygotować je do dalszego przetwarzania.

