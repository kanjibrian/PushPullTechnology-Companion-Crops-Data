# Push–Pull Technology — Companion Crops Occurrence Data

This repository contains georeferenced occurrence records for species used as companion crops in Push–Pull technology research. The data are provided as CSV files suitable for mapping, analysis, and integration into agroecological workflows.

## Contents

- `Bracharia brizantha.csv` — occurrence records (columns: `species`, `lat`, `long`, `country`, `Occurrence`, `year`, `optional`)
- `Desmodium intortum (Mill.).csv` — occurrence records (same schema as above)
- `Cotesia icipe_Occurrence data.csv` — occurrence records (columns: `ID`, `Longitude`, `Latitude`)

## Schema (summary)

- For `Bracharia brizantha.csv` and `Desmodium intortum (Mill.).csv`:
  - `species`: scientific name (string)
  - `lat`: latitude in decimal degrees (numeric)
  - `long`: longitude in decimal degrees (numeric)
  - `country`: country name (string)
  - `Occurrence`: occurrence flag (e.g., `PRESENT`)
  - `year`: year of record (numeric)
  - `optional`: additional flag/boolean

- For `Cotesia icipe_Occurrence data.csv`:
  - `ID`: species or record identifier
  - `Longitude`: longitude (decimal degrees)
  - `Latitude`: latitude (decimal degrees)

Note: Column names and order vary across files (e.g., `long` vs `Longitude`). Verify and normalize column names prior to combining datasets.

## Quick start (Python)

Install required packages (recommended):

```
pip install pandas geopandas
```

Example: load and clean `Bracharia brizantha.csv` with `pandas`:

```python
import pandas as pd

df = pd.read_csv("Bracharia brizantha.csv")
df = df.rename(columns={"long": "lon", "lat": "lat"})
df['lon'] = pd.to_numeric(df['lon'], errors='coerce')
df['lat'] = pd.to_numeric(df['lat'], errors='coerce')
df = df.dropna(subset=['lat', 'lon'])
df = df.drop_duplicates()
```

Create a GeoDataFrame and export to GeoJSON:

```python
import geopandas as gpd

gdf = gpd.GeoDataFrame(df, geometry=gpd.points_from_xy(df['lon'], df['lat']), crs='EPSG:4326')
gdf.to_file('brachiaria.geojson', driver='GeoJSON')
```

For `Cotesia icipe_Occurrence data.csv` the coordinate columns are named `Longitude` and `Latitude`:

```python
c = pd.read_csv('Cotesia icipe_Occurrence data.csv')
c = c.rename(columns={'Longitude': 'lon', 'Latitude': 'lat'})
c['lon'] = pd.to_numeric(c['lon'], errors='coerce')
c['lat'] = pd.to_numeric(c['lat'], errors='coerce')
```

## Data quality notes

- Some records use `0,0` for coordinates—treat these as missing or check original metadata.
- Check for duplicates, obvious coordinate errors, and country/coordinate mismatches before analysis.
- Ensure latitude is within [-90, 90] and longitude within [-180, 180].

## Provenance & citation

These CSVs were assembled for the Push–Pull project. If you reuse these data, please cite this repository and, where applicable, the original sources. Add a DOI or formal citation entry once available.

Suggested citation (placeholder):

> Push–Pull Technology — Companion Crops Occurrence Data. (Year). Repository. URL or DOI.

## License

Add a `LICENSE` file to indicate reuse terms. A permissive choice is Creative Commons Attribution 4.0 (`CC BY 4.0`) if you intend wide reuse; change as appropriate for your project.

## Contributing

- Report issues or request changes via GitHub Issues.
- Submit pull requests to add cleaned/standardized versions, metadata files (`DATA_DICTIONARY.md`), or analysis notebooks.

## Next steps (recommended)

- Add a `DATA_DICTIONARY.md` describing all fields in detail.
- Add a `LICENSE` file and a `CITATION.cff` or `CITATION` text file.
- Provide a cleaning notebook (`notebooks/cleaning.ipynb`) that documents processing steps and assumptions.

----

If you want, I can commit this `README.md` to the repository and push it (or adjust the text, license, or the suggested citation). Tell me how you'd like to proceed.