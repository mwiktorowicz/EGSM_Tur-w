# EGMS – deformacje terenu w rejonie KWB Turów

Identyfikacja wzorców i anomalii w deformacjach terenu na podstawie danych
[EGMS](https://egms.land.copernicus.eu/) dla obszaru
**Zittau (DE) – Hrádek nad Nisou (CZ) – Bogatynia (PL)** wokół odkrywki Turów.

## Uruchomienie

```
python -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
```

Notatniki są w folderze `EGMS/` i uruchamia się je w kolejności:

1. `EGMS_data_preparation.ipynb` – tylko gdy trzeba odtworzyć dane wejściowe
   (wymaga surowych danych w `EGMS/dane_EGMS/`, których nie ma w repozytorium),
2. `EGMS_analysis_pipeline.ipynb` – ok. 2 min, zapisuje `EGMS_Turow_U_results.csv`,
3. `EGMS_buildings_analysis.ipynb` – zapisuje `budynki_EGMS_results.gpkg`.

Wyniki są już w repozytorium, więc do własnych analiz nie trzeba niczego uruchamiać.

## Pliki

| Plik | Opis |
|---|---|
| `EGMS_Turow_U.csv` | EGMS L3, składowa pionowa – 6268 PS, 239 dat (2020–2024, bez zim) |
| `EGMS_Turow_E.csv` | EGMS L3, składowa wschód–zachód – te same PS i daty |
| `budynki_Turow.gpkg` | 39 156 budynków z EUBUCCO (CZ, PL, DE), EPSG:3035 |
| `daty_bez_zimy.txt` | lista dat zachowanych w szeregu czasowym |
| `EGMS_Turow_U_results.csv` | **wynik** – cechy, `anomaly_score`, `anomaly` i `cluster` dla każdego PS |
| `budynki_EGMS_results.gpkg` | **wynik** – budynki z najbliższym PS (do 30 m) i atrybutami EUBUCCO (`kraj`, `type`, `height`, `floors`) |
| `mask/maska_kopalnia_zwalowisko.shp` | maska kopalni z QGIS – poligony odkrywki i zwałowiska zewnętrznego (1125 PS); notatnik wczytuje wszystkie `.shp` z `mask/` |
| `EGMS_Turow_bez_kopalni_U.csv`, `..._E.csv` | wcześniejszy wybór punktów z QGIS (PS odkrywki) – **nieużywany**, zastąpiony przez `mask/` |
| `EGMS_Turow_bez_kopalni_U_results.csv` | **wynik** wariantu bez kopalni (`EGMS_bez_kopalni_analysis.ipynb`) – 5143 PS, kolumny jak `EGMS_Turow_U_results.csv` |
| `budynki_EGMS_bez_kopalni_results.gpkg` | **wynik** – budynki z najbliższym PS bez kopalni (`ryzyko_budynkow_bez_kopalni.ipynb`) |
| `budynki_ryzyko_bez_kopalni.gpkg` | **wynik** – wskaźnik ryzyka budynków bez kopalni + porównanie z wariantem pełnym |

Układ współrzędnych wyników: **EPSG:3035** (można je otworzyć bezpośrednio w QGIS).

## Ustalenia analizy bazowej

- `k = 4` (uzasadnienie w notatniku: metoda łokcia + silhouette),
- `MAX_DISTANCE = 30 m` przy przypisaniu PS do budynków,
- za anomalie uznano 5% PS o najwyższym `anomaly_score` (314 PS),
- ok. 89% anomalii należy do dwóch klastrów szybkiego osiadania (ok. −31 mm/rok),
  położonych prawie wyłącznie w obrębie odkrywki i zwałowiska.

## Dodatkowe analizy 

1. Wariant bez kopalni – wymaskowanie odkrywki i zwałowiska, porównanie anomalii i klastrów z wariantem pełnym
   (**zrobione:** `EGMS_bez_kopalni_analysis.ipynb`, budynki: `ryzyko_budynkow_bez_kopalni.ipynb`).
2. Strefa wpływu kopalni – prędkość osiadania w funkcji odległości od odkrywki.
3. Porównanie PL / DE / CZ – prędkości i odsetek anomalii w poszczególnych krajach.
4. Składowa E – ta sama analiza dla `EGMS_Turow_E.csv`, połączenie składowych U i E.
5. Wskaźnik ryzyka budynków – prędkość, `anomaly_score`, wysokość i typ budynku.
6. Wrażliwość na `MAX_DISTANCE` – wyniki dla 30 / 50 / 70 m.


## Źródła

- EGMS: https://egms.land.copernicus.eu/
- Dokumentacja EGMS: https://land.copernicus.eu/en/products/european-ground-motion-service?tab=documentation
- Budynki EUBUCCO: https://eubucco.com/data/download/v0.2
