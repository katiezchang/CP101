# CP101: Introduction to Urban Data Analytics
## Final Project: Bay Area Seismic Hazards & Affordable Housing

**Team Members**: Aalini Jain, Katie Chang, Erin Pan, Anastasiia Shkatulova

This project analyzes historic Bay Area earthquakes, Alquist-Priolo seismic hazard zones, household income, and low-income housing projects across the nine-county San Francisco Bay Area.

## Repository structure

```text
analysis/
  data_cleaning.ipynb       # Prepares input data and derived datasets
notebooks/
  Midterm_Report.ipynb      # Main analysis, maps, and charts
data/
  List of Projects_1.xlsx   # CTCAC workbook used by the analysis
  raw/                      # Downloaded source files
  processed/                # Cleaned datasets, geocoding results, and cache
visualizations/             # Exported PNG figures and interactive HTML maps/charts
requirements.txt            # Python dependencies
```

## How to run

Run the commands from the repository root to create the environment and install dependencies:

```bash
python -m venv .venv
```

Activate it:

**Windows PowerShell**

```powershell
.\.venv\Scripts\Activate.ps1
```

**macOS/Linux**

```bash
source .venv/bin/activate
```

Then install the packages:

```bash
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

The main notebook uses paths relative to the `notebooks/` directory. Start Jupyter there:

```bash
cd notebooks
jupyter notebook Midterm_Report.ipynb
```

In VS Code, open `notebooks/Midterm_Report.ipynb`, select the `.venv` kernel, and set the notebook working directory to `notebooks/`. Run the cells from top to bottom. The cleaning notebook at `analysis/data_cleaning.ipynb` locates the project root and writes cleaned datasets to `data/processed/`.

## Data locations and sources

The repository includes the following source files:

| File in this repository | Source and notes |
|---|---|
| `data/raw/Seismic_Hazard_Zone_(Alquist-Priolo)_(HESS)_20261026.csv` | [Bay Area Metro Alquist-Priolo seismic hazard zones](https://data.bayareametro.gov/Land-People/Seismic-Hazard-Zone-Alquist-Priolo-HESS-/7fdj-fhcc/about_data). Download/export the dataset as a CSV to replace this input. |
| `data/raw/bayarea_earthquakes (1).csv` | [USGS Earthquakes and Faults in the San Francisco Bay Area, 1970–2003](https://www.usgs.gov/maps/earthquakes-and-faults-san-francisco-bay-area-1970-2003). Place a replacement earthquake table here. |
| `data/raw/2018thru2022-050-csv (1).zip` | [HUD CHAS data](https://www.huduser.gov/portal/datasets/cp.html#data_2006-2022). The notebook reads tables such as `050/Table1.csv` and `050/Table8.csv` directly from the archive; do not extract it. |
| `data/List of Projects_1.xlsx` | [California Tax Credit Allocation Committee (CTCAC) project data](https://www.treasurer.ca.gov/ctcac/projects). The analysis uses the `California Mapped Projects` worksheet. The source archive `List of Projects_1.xlsx.zip`, if needed, is in `data/raw/`; extract the workbook to `data/`. |

The notebook downloads 2023 Census TIGER/Line place, county, and tract boundaries directly from `www2.census.gov`. It also uses the [U.S. Census Geocoder batch API](https://geocoding.geo.census.gov/geocoder/locations/addressbatch) to locate project addresses when the processed geocoding cache is missing or incomplete. These steps require internet access.

## Processed data and outputs

`data/processed/` contains intermediate files used by the main analysis, including cleaned GeoPackages, county income and housing tables, and geocoding results/cache. The cleaning notebook creates or refreshes these files from the raw inputs.

`visualizations/` contains exported PNG charts/maps and interactive HTML visualizations. Open the HTML files in a web browser.

## Analysis notes

- The study area is Alameda, Contra Costa, Marin, Napa, San Francisco, San Mateo, Santa Clara, Solano, and Sonoma counties.
- Spatial distances and areas use California Albers (`EPSG:3310`).
- Housing exposure is evaluated at 5 km, 10 km, and 25 km from historic earthquake epicenters; 10 km is the primary buffer.
- Some CTCAC rows have missing low-income-unit counts. These remain missing to avoid double-counting multi-site projects.
- Run notebook cells in order because later analysis cells depend on processed data created earlier.
