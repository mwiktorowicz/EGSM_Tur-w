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

Notatniki są w folderze `EGMS/`.

## Pliki

| Plik | Opis |
|---|---|
| `EGMS_data_preparation.ipynb` | porządkuje dane przycięte w QGIS i zapisuje pliki poniżej |
| `EGMS_analysis_pipeline.ipynb` | cechy szeregów czasowych, Isolation Forest, K-means |
| `EGMS_buildings_analysis.ipynb` | przypisanie punktów PS do budynków |
| `EGMS_Turow_U.csv` | EGMS L3, składowa pionowa – 6268 PS, 239 dat (2020–2024, bez zim) |
| `EGMS_Turow_E.csv` | EGMS L3, składowa wschód–zachód – te same PS i daty |
| `budynki_Turow.gpkg` | 39 156 budynków z EUBUCCO (CZ, PL, DE), EPSG:3035 |
| `daty_bez_zimy.txt` | lista dat zachowanych w szeregu czasowym |

Surowe dane (`EGMS/dane_EGMS/`) nie są w repozytorium, bo są za duże – potrzebne
tylko do ponownego uruchomienia `EGMS_data_preparation.ipynb`.

## Do zrobienia

W notatnikach analizy podmienić nazwy plików `EGMS_Bytom.csv` →
`EGMS_Turow_U.csv` i `budynki_polaczone_all.gpkg` → `budynki_Turow.gpkg`,
a w notatniku budynków wpisać wartość `MAX_DISTANCE`.

## Źródła

- EGMS: https://egms.land.copernicus.eu/
- Budynki EUBUCCO: https://eubucco.com/data/download/v0.2
