##NYC Urban Growth, Construction Density, and Flood Exposure
#A Geospatial Data Science Project

**Project Overview**

New York City maintains a high-resolution planimetric basemap derived from aerial photogrammetry that includes polygon footprints for qualifying buildings citywide. This data provides a detailed view of all construction built across the city, enabling precise spatial analysis of building footprint area, density, and neighborhood-level development patterns.

This project began as an attempt to quantify structural growth over time by comparing historic and current building-footprint datasets. During exploratory analysis, it became clear that NYC’s historic planimetric datasets do not represent a complete or time-indexed snapshot of the city’s building stock. Instead, they primarily capture retired or modified or demolished features and buildings between edition releases, making them unsuitable for the initial intent, although it seems that one can contact the New York City gov to get access to these older data points, so as a future enhancement that would be possible.

Pivot:

Where is New York City’s existing built density already exposed to present-day stormwater flood risk?

All datasets and results are aggregated to Neighborhood Tabulation Areas (NTAs) to ensure geographic consistency.

Core Analytical Focus

This project addresses three related topics:

1. Construction Density

How much of each neighborhood is already built?

How concentrated is building footprint area relative to land area?

Where are the most structurally dense NTAs in NYC?

2. Flood Exposure of the Built Environment

Which buildings intersect modeled stormwater flood zones?

What proportion of total building footprint area is exposed to flooding?

How does flood exposure vary across neighborhoods?

3. Risk Concentration

Which NTAs combine high construction density and significant flooded footprint area during moderate rains?

These neighborhoods represent elevated risk for infrastructure systems, insurers, real estate industries, and city planners.

Population metrics were explored early in the project, and were going to be used to determine recent NTAs with rapid growth in population, but upon further inspection we realized that NYC changed its NTA codes/logic between 2010 and 2020, making for a very poor match rate between datasets and thus decided to focus solely on making this a present day risk model, which could be expanded in the future with more robust data.

Data Sources
Building Footprints — Current Edition

Source: NYC Open Data

Description: Polygon footprints of qualifying buildings citywide

Key Attributes:

Geometry

Footprint area (computed)

BIN / ObjectID

Partial height and elevation metadata

Purpose: Represents the current built environment

Notes: Only buildings meeting minimum size/height thresholds are included unless assigned a BIN

Neighborhood Tabulation Areas (NTAs)

Source: NYC Department of City Planning

Description: Stable neighborhood geographies designed for demographic and planning analysis

Fields:

NTACode

NTAName

Borough

Geometry

Purpose: Spatial aggregation unit for all metrics

NYC Stormwater Flood Maps

Source: NYC Open Data

Description: Modeled stormwater flooding under heavy rainfall scenarios

Format: File Geodatabase (GDB)

Scenario Used:

Moderate stormwater flooding

~2.13 inches/hour rainfall

Current sea levels

Purpose: Identify present-day flood exposure of buildings and neighborhoods

Methodology
Coordinate Systems and Area Measurement

All spatial datasets are reprojected to:

EPSG:2263 — NAD83 / New York Long Island

This ensures accurate area calculations in square feet, which are critical for density and footprint metrics.

Construction Density Pipeline

Load current building footprints

Reproject to EPSG:2263

Compute geometry-based footprint area (geom_area_sqft)

Spatially join buildings to NTA polygons using the centroid of buildings polygons

Aggregate per NTA:

- Building count

- Total footprint area

- Median building footprint

- Built area ratio (building footprint/footprint area)

- Buildings per square mile

- Built square feet per square mile


Flood Exposure Analysis

- Load stormwater flood polygons (shapes of flood zones)

- Identify buildings intersecting flood zones

- Flag buildings with is_flooded

- Compute flooded footprint area per building

- Aggregate flood exposure metrics at the NTA level

- These metrics are merged with construction density to produce an NTA-level flood risk table


Risk Score (Exploratory)

A simple composite risk score is derived from the below calculated/enriched metrics

- Built area ratio

- Percent of footprint flooded


**Key Findings**

Flood exposure and risk is highly concentrated

Several NTAs with high construction density also show high flooded footprint area

Recent development clusters appear in flood-exposed zones in multiple boroughs



Visualization Outputs

Two parquet files are produced for visualization workflows:

1. NTA-Level Visualization Table
nta_viz.parquet


Contains:

NTA geometry

Built density metrics

Flood exposure metrics

Composite risk score

Used for choropleth maps and comparative charts.

2. Building-Level Visualization Table
buildings_viz.parquet


Contains:

Building geometry (polygons or centroids)

NTA identifiers

Footprint area

Flood status

Flooded footprint area

Construction year indicators

Used for:

Point-based flood maps

Recent construction overlays

Interactive inspection


Data Limitations

Construction year fields are incomplete and not entirely reliable

Stormwater flood maps are prediction model based

Analysis reflects current exposure, not future projections

Future Work

Integrate zoning and land-use constraints

Evaluate additional stormwater scenarios

Add sea-level-rise floodplains (2050s, 2080s)

Incorporate insurance or claims-based exposure

Develop a standalone interactive dashboard

Consider the construction of flood safety features to effect the prediction models