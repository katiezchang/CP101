# CP101: Introduction to Urban Data Analytics
## Final Project: Bay Area Seismic Hazards & Affordable Housing

**Team Members**: Aalini Jain, Katie Chang, Erin Pan, Anastasiia Shkatulova

This repository contains the notebook `Midterm_Report.ipynb`, which cleans and combines Bay Area earthquake, seismic hazard-zone, income, and low-income housing data. The analysis focuses on the nine-county San Francisco Bay Area and produces maps, summary tables, and visualizations comparing affordable/low-income housing with earthquake epicenters and mapped seismic hazard zones.

## How to run

### Run code
From the project directory:

```bash
python -m venv .venv
# macOS/Linux:
source .venv/bin/activate
# Windows:
# .venv\Scripts\activate

pip install -r requirements.txt
jupyter notebook Midterm_Report.ipynb
```

Then run all notebook cells from top to bottom.

## Data

The notebook currently expects these files in `data/`:

- `Seismic_Hazard_Zone_(Alquist-Priolo)_(HESS)_20261026.csv`
- `bayarea_earthquakes (1).csv`
- `2018thru2022-050-csv (1).zip`
- `List of Projects_1.xlsx`
- `GeocodeResults.csv`

The following files are **generated/intermediate files** by the notebook and do not need to be downloaded separately if the notebook is run from the beginning:

- `ap_fault_zones_clean.gpkg`
- `sfba_earthquakes_clean.gpkg`
- `ctcac_geocoded.csv`

### Original data sources

**Seismic hazard zones / Alquist-Priolo zones**
- Bay Area Metro data portal:
  https://data.bayareametro.gov/Land-People/Seismic-Hazard-Zone-Alquist-Priolo-HESS-/7fdj-fhcc/about_data
- The notebook uses the downloaded CSV listed above.

**Historic earthquakes**
- USGS, *Earthquakes and Faults in the San Francisco Bay Area, 1970–2003*:
  https://www.usgs.gov/maps/earthquakes-and-faults-san-francisco-bay-area-1970-2003
- The downloaded earthquake CSV should be placed in `data/`.

**HUD/HCD household and income data**
- HUD User Community Planning & Development datasets:
  https://www.huduser.gov/portal/datasets/cp.html#data_2006-2022
- The notebook uses the `2018thru2022-050-csv (1).zip` file, specifically tables inside `050/Table1.csv` and `050/Table8.csv`.

**California Tax Credit Allocation Committee (CTCAC) low-income housing projects**
- California State Treasurer / CTCAC project data:
  https://www.treasurer.ca.gov/ctcac/projects
- Download the project workbook and save it as `data/List of Projects_1.xlsx`.

**Census geographic boundaries**
The notebook downloads Census TIGER/Line geographic files directly over the internet, including:
- California place boundaries
- California county boundaries
- California census tract boundaries

The notebook also uses the U.S. Census Geocoder API for project addresses. If `data/ctcac_geocoded.csv` is not available, the notebook can make batch geocoding requests to:

https://geocoding.geo.census.gov/geocoder/locations/addressbatch

Because the notebook currently reads `GeocodeResults.csv` during the housing-geocoding workflow, keep the provided `data/GeocodeResults.csv` available when reproducing the current notebook exactly.

## Geographic scope and processing

The analysis uses the nine Bay Area counties:

- Alameda
- Contra Costa
- Marin
- Napa
- San Francisco
- San Mateo
- Santa Clara
- Solano
- Sonoma

Most spatial calculations use California Albers (`EPSG:3310`) so that distances and areas are measured in meters.

The notebook:
1. Cleans the Alquist-Priolo seismic hazard-zone data and converts its WKT geometry into a GeoDataFrame.
2. Cleans historic earthquake records and converts earthquake coordinates to spatial points.
3. Extracts low-income household measures from HUD data.
4. Filters CTCAC projects to the nine Bay Area counties.
5. Geocodes housing projects using the Census Geocoder, with a census-tract centroid fallback for unmatched projects.
6. Joins housing projects to historic earthquake epicenters using nearest-distance and buffer-based methods.
7. Calculates exposure within 5 km, 10 km, and 25 km of earthquake epicenters.
8. Calculates distance from housing projects to mapped Alquist-Priolo zones.
9. Produces maps and graphs describing the spatial relationship between housing and seismic hazards.

## Important reproduction notes

- Internet access is required because the notebook downloads Census boundary data and may contact the Census Geocoder.
- The notebook currently uses filenames containing spaces and `(1)`. Keep the filenames exactly as listed above, or update the paths in the notebook.
- Run cells in order because later cells depend on cleaned GeoPackages and geocoding results created earlier.
- The geocoding step uses a cached `ctcac_geocoded.csv` when available to avoid repeating Census API requests.
- Some CTCAC rows have missing low-income-unit counts. The notebook intentionally keeps these as missing rather than filling them in, in order to avoid double-counting multi-site projects.
- The notebook's analysis settings use 5, 10, and 25 km earthquake buffers, with 10 km as the main buffer.

## Main software requirements

See `requirements.txt` for the Python packages used by the notebook.
