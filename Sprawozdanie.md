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

### Wykonanie
```
cat dane.txt
paste - - - < dane.txt
echo "x y z" >> dane.txt
```

### Wnioski

Użycie narzędzi takich jak `cat`, `paste` oraz `echo` pozwala w prosty sposób manipulować danymi tekstowymi i przygotować je do dalszego przetwarzania.

---

## Zadanie 4 – Dodawanie poprawek

### Cel zadania
Celem zadania było porównanie dwóch wersji pliku tekstowego – oryginalnego lista.txt oraz zaktualizowanej lista-pop.txt. Następnie należało wygenerować plik z różnicami w formacie łatki, który pozwoliłby na aktualizację pierwotnego pliku do nowszej wersji. Ostatnim krokiem było zweryfikowanie czy wszystko się zgadza.

### Sposób realizacji
- Wykonano `diff`, aby znaleźć różnice.
- Użyto komendy diff, aby wygenerować plik różnic (lista.patch).
- Wyniki zapisano do pliku `lisat-pop.txt`.

### Wykonanie
```
diff -u lista.txt lista-pop.txt > lista.patch
patch lista.txt lista.patch
```

### Wnioski
Komenda diff skutecznie wygenerowała różnice w plikach, co umożliwiło łatwe użycie narzędzia patch do aktualizacji pliku.

---

## Zadanie 5 – CSV -> SQL

#### Cel zadania
Zadanie miało na celu przetworzenie pliku steps-2sql.csv, zawierającego dane z trzech kolumn (time, intensity, steps) oddzielonych średnikami, na format SQL gotowy do importu do bazy danych.

### Wykonanie
```
unzip csv.zip
dos2unix steps-2sql.csv
cat steps-2sql.csv
tail -n +2 steps-2sql.csv | awk -F';' '{print "INSERT INTO stepsData (time, intensity, steps) VALUES (" $1 ", " $2 ", " $3 ");"}' > stepsData.sql
cat stepsData.sql
dos2unix steps-2csv.sql
cat steps-2csv.sql
awk -F'[()]' '{split($2, a, ","); print substr(a[1], 1, length(a[1])-3) ";" a[2] ";" a[3]}' steps-2csv.sql | tail -n +2 > output.csv
cat output.csv
```

### Wnioski
Narzędzia takie jak awk i tail umożliwiły efektywne przekształcenie danych między formatami CSV a SQL.

---

## Zadanie 6 – Marudny tłumacz

### Cel zadania
Zadanie polegało na wsparciu procesu tłumaczenia oprogramowania z języka angielskiego na polski. Plik en-7.2.json5, zawiera angielskie frazy w formacie JSON.

### Sposób realizacji
-tlumacz.zip zostało rozpakowane, a zawartość plików en-7.2.json5 i en-7.4.json5 sprawdzono za pomocą cat.
-Użyto awk do podwojenia linii z dodaniem komentarzy // i zapisano wynik do pl-7.2.json5.
-Następnie wyodrębniono frazy z obu plików za pomocą grep, porównano je, aby znaleźć nowe wpisy, i zapisano je z komentarzami do pl-7.4.json5.
-Poprawność formatu zweryfikowano komendą cat.

### Wykonanie
```
unzip tlumacz.zip
dos2unix en-7.2.json5
dos2unix en-7.4.json5
cat en-7.2.json5
cat en-7.4.json5
awk '{print "// " $0; print $0}' en-7.2.json5 > pl-7.2.json5
cat pl-7.2.json5
grep -E '^\s*".*":' en-7.2.json5 > old.txt
grep -E '^\s*".*":' en-7.4.json5 > new.txt
grep -Fvx -f old.txt new.txt > only_new.txt
awk '{print "// " $0; print $0}' only_new.txt > pl-7.4.json5
cat pl-7.4.json5
```

### Wnioski
Komendy awk i grep pozwoliły na efektywne zdublowanie linii z komentarzami oraz wyodrębnienie nowych fraz. Użycie dos2unix zapewniło zgodność formatu plików.

---

## Zadanie 7 – Fotograf gamon

#### Cel zadania
Celem było przekształcenie pliku CSV w skrypt SQL z komendami INSERT.

### Sposób realizacji
- Archiwa kopie-1.zip i kopie-2.zip zostały rozpakowane do tymczasowych folderów.
- Użyto narzędzia magick, aby zmienić format na JPG, dostosować wysokość do 720 pikseli i ustawić rozdzielczość 96x96 DPI dla każdego pliku.
- Wszystkie przetworzone obrazy zebrano i spakowano do jednego pliku ZIP.
- Poprawność działania zweryfikowano komendami ls i unzip -l.

### Wykonanie
```
unzip kopie-1.zip -d temp1
unzip kopie-2.zip -d temp2
mkdir -p przetworzone
for file in temp1/* temp2/*; do if [ -f "$file" ]; then name=$(basename "$file"); magick "$file" -resize x720 -density 96x96 "${name%.*}.jpg" -quality 90 przetworzone/; fi; done
zip -r gotowe_zdjecia.zip przetworzone
ls przetworzone
unzip -l gotowe_zdjecia.zip
```

### Wnioski
Zadanie pozwoliło na praktyczne wykorzystanie poleceń MSYS2 do automatyzacji przetwarzania plików graficznych. Użycie pętli for i narzędzia magick umożliwiło szybką konwersję wielu zdjęć. Sprawdzanie wyników komendami ls i unzip potwierdziło poprawność działania.

---

## Zadanie 8 – PDF

### Cel zadania
Celem zadania było stworzenie portfolio w formacie PDF na kartkach A4, zawierającego wcześniej przetworzone zdjęcia, po osiem na stronie, z dodanymi nazwami plików pod zdjęciami, gotowego do łatwego wydruku.

### Sposób realizacji
- Utworzono plik portfolio.tex, który robi dokument A4 z układem zdjęć: dwa w wierszu, cztery wiersze na stronę, z nazwami plików pod każdym zdjęciem.
- Użyto pakietu graphicx do wstawiania zdjęć i geometry do ustawienia marginesów.
- Wygenerowano PDF za pomocą pdflatex.
- Sprawdzono poprawność wyniku, otwierając plik PDF.

### Wykonanie
```
pacman -Syu
pacman -S texlive-full
pacman -S texlive-core texlive-latexextra
pdflatex --version
cat > portfolio.tex << EOL
\documentclass[a4paper]{article}
\usepackage{graphicx}
\usepackage[margin=1cm]{geometry}
\begin{document}
\pagestyle{empty}
\begin{center}
EOL
for file in przetworzone/*.jpg; do echo "\includegraphics[width=0.45\textwidth]{$file} \hspace{0.5cm} \parbox{0.45\textwidth}{\centering \tiny $(basename "$file")}\\\\[0.5cm]" >> portfolio.tex; done
echo "\end{center}" >> portfolio.tex
for file in przetworzone/*.jpg; do echo "\includegraphics[width=0.45\textwidth]{$file} \hspace{0.5cm} \parbox{0.45\textwidth}{\centering \tiny $(basename "$file")}\\\\[0.5cm]" >> portfolio.tex; done
echo "\end{document}" >> portfolio.tex
pdflatex portfolio.tex
start portfolio.pdf
```

### Wnioski
Zadanie pozwoliło na praktyczne wykorzystanie LaTeX`a w MSYS2 do stworzenia portfolio w formacie PDF. Automatyzacja wstawiania zdjęć za pomocą pętli w Bashu znacznie przyspieszyła proces. Wyzwaniem było odpowiednie rozmieszczenie zdjęć i nazw plików, aby zmieściły się na stronie A4 i były czytelne.

---

## Zadanie 9 – Porządki w kopiach

### Cel zadania
Celem zadania było uporządkowanie plików kopii zapasowych tak, aby każdy rok miał swój katalog, a w każdym roku kopie z jednego miesiąca znajdowały się w osobnych podkatalogach

### Sposób realizacji
- Rozpakowano archiwa kopie-1.zip i kopie-2.zip.
- Użyto skryptu w Bashu do analizy nazw plików ZIP, wyodrębniając rok i miesiąc z ich nazw.
- Dla każdego pliku ZIP utworzono katalogi w strukturze kopie2/YYYY/MM i przeniesiono pliki do odpowiednich podkatalogów.
- Po zakończeniu usunięto tymczasowy folder temp.
!!! Działa dobrze jedynie gdy archiwa mają nazwy "YYYY-MM-DD.zip"

### Wykonanie
```
mkdir -p temp
unzip -q kopie-1.zip -d temp
unzip -q kopie-2.zip -d temp
cd temp
for f in *.zip; do 
  rok=${f:0:4}; miesiac=${f:5:2}; 
  mkdir -p "../kopie2/$rok/$miesiac"; 
  mv "$f" "../kopie2/$rok/$miesiac/"; 
done
cd .. && rm -r temp
```

### Wnioski
Zadanie pozwoliło na praktyczne wykorzystanie prostych komend do automatyzacji organizacji plików. Pętla for i wyodrębnianie roku oraz miesiąca z nazw plików ułatwiły tworzenie struktury katalogów.
