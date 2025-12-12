NYC Urban Growth Analysis: Built Environment, Population Change, and Flood Vulnerability

A Geospatial Data Science Project (Python + GeoPandas)

Project Overview

New York City maintains a detailed planimetric basemap that includes building footprint polygons derived from aerial photogrammetry. These datasets can be used to analyze changes in the built environment across update cycles. By comparing historic and current building-footprint data and integrating population change at the Neighborhood Tabulation Area (NTA) level, this project examines the relationship between physical urban growth and demographic change.

To extend this analysis, the project incorporates NYC’s Stormwater Flood Maps to assess whether construction and population growth are occurring within areas vulnerable to present-day flooding. This allows identification of neighborhoods where existing flood risk may already be colliding with patterns of growth.

This project addresses three primary objectives:

A. Measure how NYC’s built environment has physically changed between two planimetric basemap snapshots.
B. Compare built-environment changes with population changes from 2000 to 2020.
C. Assess current stormwater flood vulnerability in relation to construction activity and population growth.

Research Questions
Part A — Built Environment Growth

How much total building footprint area exists in each NTA?

How has footprint area changed between historic and current datasets?

Which neighborhoods experienced the largest increases or decreases in footprint area?

How do these changes differ across boroughs?

Part B — Built Environment vs Population Change

How did NTA populations change between 2000, 2010, and 2020?

Do NTAs with significant physical growth also show population growth?

Where do physical expansion and demographic change diverge?

What might these divergences imply for zoning, housing supply, or land-use patterns?

Part C — Flood Vulnerability and Structural Risk

What proportion of each NTA lies within NYC’s current stormwater flood-risk zones?

What proportion of current building footprint area overlaps with these zones?

Are neighborhoods experiencing recent construction growth located in high-risk stormwater areas?

Do NTAs with strong population growth coincide with high flood vulnerability?

Which neighborhoods show high exposure but limited structural expansion, indicating unmet resiliency needs?

Data Sources
Building Footprints (Current Edition — Map)

NYC Open Data

Polygon footprints of qualifying NYC buildings

Includes geometry, footprint area, BIN, and elevation attributes where available

Used as the “current” built-environment snapshot

Derived from the NYC planimetric basemap

Last updated December 1, 2025

Building Footprints Historic (Map)

NYC Open Data

Previous edition of the building-footprint dataset

Same structure as the current version

Represents the prior basemap update cycle

Used as the “historic” baseline snapshot

Last updated November 30, 2025

NYC Population Data (NTA Level)

Neighborhood Tabulation Areas (NTAs) provide a consistent geography for longitudinal demographic analysis. Population counts for 2000, 2010, and 2020 are used.

NYC NTA Boundary Shapefiles

Used for spatial aggregation and joining building-level measures to neighborhood-level metrics.

NYC Stormwater Flood Maps

NYC Open Data

Modeled stormwater flooding under heavy rainfall conditions based on current climate

Used to identify flood-exposed areas and evaluate overlap with construction and population growth

Includes multiple scenarios; this project selects one scenario for consistency (e.g., “Extreme Stormwater Flooding”)

Metadata Reference

All building-footprint definitions, criteria, and field descriptions follow the standards documented in the NYC GeoMetadata repository:
https://github.com/CityOfNewYork/nyc-geo-metadata/tree/main/Metadata

Methodology

Download historic and current building-footprint datasets.

Load footprints using GeoPandas and reproject to a coordinate system suitable for area calculations (e.g., EPSG:2263).

Compute building footprint area where needed.

Spatially join footprints to NTA boundaries and aggregate:

total footprint area

building counts

Compute absolute and percent changes in footprints between historic and current datasets.

Load and process population data for 2000–2020; compute population changes.

Join demographic and built-environment metrics at the NTA level.

Load stormwater flood polygons and compute:

percent of NTA area in flood zone

percent of building footprint area in flood zone

Compare construction growth, population growth, and flood vulnerability to identify at-risk or underserved neighborhoods.

Produce mapping, charts, and statistical summaries.

Data Limitations

Only buildings with footprint area ≥ 400 sq ft and height ≥ 12 ft are included unless assigned a BIN.

Building footprint counts may vary across versions due to splitting or merging of polygons.

Elevation fields in planimetric datasets are incomplete.

Construction-year fields may be missing or approximate.

BINs may change over time, making building-level comparisons unreliable; aggregation is done at the NTA level.

Only two historical building-footprint snapshots are available via NYC Open Data; this is not a continuous time series.

Stormwater flood maps represent modeled flooding under specific storm scenarios, not observed events.

Repository Structure
nyc-urban-growth/
│
├── data/
│   ├── raw/
│   │   ├── building_current/
│   │   ├── building_historic/
│   │   └── stormwater/
│   ├── processed/
│
├── notebooks/
│   ├── 01_load_buildings.ipynb
│   ├── 02_aggregate_built_area.ipynb
│   ├── 03_population_vs_growth.ipynb
│   ├── 04_flood_risk_overlay.ipynb
│   └── 05_visualizations.ipynb
│
├── src/
│   ├── utils.py
│   ├── spatial_processing.py
│   └── population_processing.py
│
├── docs/
│   ├── slides
│   └── other
│
├── README.md
├── requirements.txt
└── .gitignore

Future Work

Integrate additional planimetric layers such as land cover, sidewalks, parks, or hydrography.

Combine construction and flood-risk data with zoning information (PLUTO) to evaluate regulatory constraints.

Model multiple stormwater scenarios and compare exposure.

Incorporate sea-level-rise projections (2050s floodplain) for long-term planning context.

Build interactive geospatial dashboards or animations.