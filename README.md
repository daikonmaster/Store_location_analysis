# Store_location_analysis
Geospatial analysis for Silpo store location in Kyiv

## To view complete notebook with maps click here: https://nbviewer.org/github/daikonmaster/Store_location_analysis/blob/main/store_analysis.ipynb

## Project Overview
This project analyses the correlation between population metric and store performance metric, builds a location scoring model, and recommends top locations for new stores.

## Methods

### Correlation Analysis
Two methods used as a robustness check:

**BallTree (2km radius)** — finds all population points within a fixed radius and computes mean metric. It is sensitive to radius choice.

**Inverse Distance Weighting (IDW)** — weights population points by inverse distance. Closer points have more influence. Avoids arbitrary radius cutoff and better reflects real-world spatial decay of demand.

Both methods returned consistent results (BallTree: 0.171, IDW: 0.118) — confirming weak but positive correlation.

### Location Scoring Model
`score = (demand × accessibility) / (1 + competition)`

- **demand**: population metric
- **accessibility**: proximity to metro stations (OpenStreetMap)
- **competition**: IDW-weighted presence of existing Silpo stores

## Data Sources
- Provided dataset: 12,752 population points and 92 Silpo stores
- OpenStreetMap: 688 competitor supermarkets and 53 metro stations

## Future Work
- **ML prediction**: with more features (store size, rent, foot traffic) could predict store performance score for any location
- **Geographically Weighted Regression (GWR)**: coefficients vary spatially — the impact of population density may differ between city centre and periphery
- **Huff Model**: probabilistic model of customer store choice based on distance and store size

## Tools
Python · pandas · sklearn · statsmodels · folium · osmnx · Power BI
