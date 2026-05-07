# Store_location_analysis
Geospatial analysis for Silpo store location in Kyiv

## To view complete notebook with maps I recommedn domloading it, as it will not probably render maps on Github

## Project Overview
This project analyses the correlation between population metric and store performance metric, builds a location scoring model, and recommends top locations for new stores.

## Methods

### Correlation Analysis
I research on topic what are the common techniques to work with geospatial data. And for this case I decided to use first:

**Inverse Distance Weighting (IDW)** — weights population points by inverse distance, where closer points have more influence. Avoids arbitrary radius cutoff and better reflects real-world spatial decay of demand.

And for a robustness check I also tested with:
**BallTree (2km radius)** — finds all population points within a fixed radius and computes mean metric. It is sensitive to radius choice.
Radius of 2 km was chosen as average one for big cities, that people can walk to or drive.

Both methods returned consistent results (BallTree: 0.171, IDW: 0.118) — confirming weak but positive correlation.

### Location Scoring Model
Scoring formula: `score = (demand x accessibility) / (1 + silpo competition + major competitor competitiion)` - simplified version ov Huff Model but without competitor store weights.

- **demand**: population metric at a given point
- **accessibility**: proximity to metro — `1 / (distance_km + 0.3)` - 0.3km minimum friction distance (approx. walking distance to metro entrance)
- **siplo competition**: IDW-weighted presence of existing Silpo stores within 1km (to avoid cannibalization)
- **major competitor competition**: IDW-weighted presence of major competitors within 1km (Фора, АТБ, Новус, Varus, Ашан, Велика Кишеня, ЛотОК, Велмарт, Еко маркет)

Higher score = higher demand, better metro access, lower competition.

## Data Sources
- Provided dataset: 12,752 population points and 92 Silpo stores
- OpenStreetMap:  688 competitor supermarkets (where 545 major competitors) and 53 metro stations

## Future Work Extension
- **ML prediction**: with more features (store size, rent, foot traffic) could predict store performance score for any location
- **Geographically Weighted Regression (GWR)**: coefficients vary spatially — the impact of population density may differ between city centre and periphery
- **Huff Model**: probabilistic model of customer store choice based on distance and store size

## Files for Power Bi
- 1 pbix file
- if needed 4 datasets: major_competitors, new_population, new_stores, THEtop_locations

## Tools
Python · pandas · sklearn · statsmodels · folium · osmnx · Power BI
