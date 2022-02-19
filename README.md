# Projekt z WdSI
## Glowny szkielet programu
- wczytanie danych treningowych
- balansowanie danych treningowych
- wczytanie danych testowych
- balansowanie danych testowych
- generowanie slownika
- wyciagniecie cech zbioru treningowego
- trenowanie
- wyciągnięcie cech zbioru testowego
- predykcja
- porównanie i wyniki

## Wczytywanie danych
### "load_data"
Polega na wczytaniu plików graficznych i parametrów z adnotacji
plików graficznych w formie .xml bazy obrazków, obrobieniu ich i
odpowiednim ich  zapisaniu. Parametry są zapisywane w zmiennej
data osobno dla każdego obiektu o ile obiekt spełnia warunek,
że ma co najmniej 10% szerokości i wysokości zdjęcia <br/>

Wczytane parametry dla każdego zdjęcia:
- nazwa obrazka
- szerokość obrazka
- wysokość obrazka
- parametry obiektów
  - nazwa obiektu
  - koordynaty x i y dwóch przeciwległych wierzchołków prostokąta
w których znajduje się obiekt na oryginalnym zdjęciu

Parametry zapsiane w zmiennej data dla każdego obiektu:
- wycięte zdjęcie w którym znajduje się obiekt
- nazwa obiektu
- nazwa i ścieżka do pliku
- koordynaty x i y dwóch przeciwległych wierzchołków prostokąta
w których znajduje się obiekt na oryginalnym zdjęciu

Wykorzystuje charakterystyczne komendy:
- `os.getcwd()` - zwraca ścieżkę do aktualnego folderu
- `os.listdir({path})` - zwraca listę plików i folderów w ścieżce
- `xml.etree.ElementTree.parse{PathAndFileName}` - wczytujuje plik .xml
- `os.path.join({path}, {filename})` - łączy ścieżkę z nazwą pliku
- `{xml.etree.ElementTree.parse}.getroot()` - pobiera zawartość pliku .xml
- `{XMLdata}.findall({name})` - szuka wszystkich korzeni o konkretnej nazwie
- `{root}.find(label)` - szuka korzenia o konkretnej nazwie
- `{foundRoot}.text` - zwraca tekst znalezionego korzenia
- `cv2.imread({PathAndFileName})` - wczytuje obrazek
- `{img}[{y_min}:{y_max}, {x_min}:{x_max}]` - wyciana zdjęcie
- `class_id_to_new_class_id` - zamienia slowo na numer

## Generowanie slownika
### "learn_bovw"
Generuje i zapisuje slownik przy pomocy opisu stworzonego na podstawie punktów
charakterystycznych uzyskanych ze zdjęcia

Wykorzystuje charakterystyczne komendy:
- `cv2.BOWKMeansTrainer({DictionarySize})` - tworzy zbiór treningowy "Bag of Visual Words"
- `cv2.SIFT_create()` - tworzy sito
- `sift.detect({image}, {colorType})` - funkcja sita szukająca punktów
znaczących (keypoint'ów)
- `sift.compute({image}, {keypoints})` - funkcja sita tworząca opisy z
punktów znaczących (keypoint'ów)
- `{BagOfWords}.add({description})` - dodaje opis do zbioru treningowego
- `{BagOfWords}.cluster()` - tworzy slownik ze zbioru treningowego
- `np.save({PathAndFileName}, {vocabulary})` - zapisuje slownik do pliku
- `np.load({PathAndFileName})` - wczytywanie pliku

## Wyciagniecie cech
### "extract_features"
Generuje opisy na podstawie wczytanego slownika i punktów charaktersystycznych
uzyskanych ze zdjęć z wczytanego zbioru

Wykorzystuje charakterystyczne komendy:
- `cv2.FlannBasedMatcher_create()` - tworzenie modułu dopasowującego
- `cv2.BOWImgDescriptorExtractor({sift}, {matcher})` - tworzenie modułu do tworzenia opisów
- `{ImageDescriptorExtractor}.setVocabulary({vocabulary})` - Zwraca slownik wizualny

## Trenowanie
### "train"
Polega na wytrenowaniu klasyfikatora przy pomocy zbioru treningowego

Wykorzystuje charakterystyczne komendy:
- `.squeeze({axes})` - usuwa osie
- `RandomForestClassifier()` - klasyfikator RandomForest
- `{RandomForestClassifier}.fit({descriptions}, {labels})` - tworzy las drzew ze zbioru treningowego

## Predykcja
### "predict"
Predykuje nazwę obiektu na podstawie opisu przy pomocy klasyfikatora
Random Forest

Wykorzystuje charakterystyczne komendy:
- `{RandomForestClassifier}.predict('description')` - predykuje klasę z opisu

## Porównanie i wyniki
### "evaluate"
Zwraca rezultaty klasyfikacji:
- wypisuje w konsoli nazwy zdjęć ze zbioru testowego,
gdzie znajdują się znaki przejść dla pieszych oraz podaje
koordynaty x i y dwóch przeciwległych wierzchołków prostokąta
w których znajduje się obiekt na oryginalnym zdjęciu
- zwraca macierz pomyłek
- zwraca macierz pomylek
- podaje zgodność predykcji z wartościami adnotacji

## Balans danych
### "balance_dataset"
Wczytuje losowy zakres danych od 0.0 - 1.0,
gdzie 0 oznacza 0% zbioru a 1.0 oznacza 100% zbioru

Wykorzystuje charakterystyczne komendy:
- `random.sample({data}, {amount})` - wybór losowych próbek z daty i ich ilości