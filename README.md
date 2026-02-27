# Greene County (Ohio) GIS Open Data via ArcGIS REST (MapServer)

This tutorial shows how to:

1. Understand what an **ArcGIS REST Map Service** is.
2. Connect to Greene County’s **Open Data** service in **QGIS** and **Esri ArcGIS**.
3. Save (cache) layers **locally** for offline use and faster performance.
4. Understand foundational GIS vocabulary used in environmental engineering.

> Audience: undergraduate environmental engineering students learning GIS fundamentals.

---

# Comprehensive GIS & REST API Vocabulary

This section defines terminology students will encounter when working with web GIS services and environmental datasets.

## Core GIS Concepts

**GIS (Geographic Information System)**  
A system for capturing, storing, analyzing, managing, and visualizing spatial (location-based) data.

**Spatial Data**  
Data that has geographic location information (coordinates).

**Attribute Data**  
Tabular data describing the characteristics of spatial features.

**Feature**  
A spatial object with geometry and attributes (e.g., one parcel, one stream segment).

**Geometry**  
The spatial shape of a feature (point, line, or polygon).

**Point**  
A single coordinate location (e.g., monitoring well).

**Line (Polyline)**  
A connected set of vertices representing linear features (e.g., stream, road).

**Polygon**  
A closed area representing boundaries (e.g., parcel, floodplain).

**Raster**  
A grid of pixels representing continuous data (e.g., elevation, land cover imagery).

**Vector Data**  
Spatial data represented as points, lines, or polygons.

**Layer**  
A dataset displayed in a GIS project.

**CRS (Coordinate Reference System)**  
Defines how geographic data is projected onto a flat map.

**Projection**  
Mathematical transformation from the Earth’s curved surface to a flat coordinate system.

**Datum**  
A reference model of the Earth used for coordinate calculations.

**Reprojection**  
Converting spatial data from one CRS to another.

**GeoPackage (.gpkg)**  
An open-standard spatial database format that stores multiple layers in one file.

**Shapefile (.shp)**  
Legacy ESRI vector format consisting of multiple files.

**File Geodatabase (.gdb)**  
ESRI’s native spatial database format.

---

## ArcGIS REST & Web GIS Concepts

**REST (Representational State Transfer)**  
A web architecture that allows data to be accessed using URLs.

**API (Application Programming Interface)**  
A defined method for software to communicate with another system.

**Endpoint**  
A URL that provides access to a service or dataset.

**ArcGIS Server**  
Software that publishes GIS data as web services.

**MapServer**  
A REST endpoint that serves map layers (rendering and possibly querying).

**FeatureServer**  
A REST endpoint optimized for querying and editing features.

**Service**  
A published collection of GIS layers accessible via web.

**Layer ID**  
The numeric identifier for a dataset inside a service (e.g., `/MapServer/1`).

**Query Operation**  
A REST function used to request features from a layer.

**JSON (JavaScript Object Notation)**  
Machine-readable text format used to transmit data.

**GeoJSON**  
A JSON format specifically for spatial data.

**MaxRecordCount**  
Maximum number of features the server allows per request.

**Pagination**  
Downloading large datasets in multiple smaller requests.

**Streaming Layer**  
A layer drawn live from a remote server rather than stored locally.

---

# Lesson 0 — Understanding the Greene County REST Service

Service endpoint:

- https://gis.greenecountyohio.gov/webgis2/rest/services/OpenData/OpenData/MapServer

This is an **ArcGIS Server Map Service** that contains multiple GIS layers.

Each layer has:

- A name
- A numeric ID
- Attribute fields
- A spatial reference

Common environmental layers:

- Parcels (1)
- Streams (17)
- FEMA Floodplain 2022 (48)
- Wetlands (43)
- Soils (29)
- Zoning (15)

---

# Lesson 2 — Connect to the REST Service

## 2.A — QGIS Workflow

### Add ArcGIS REST Server Connection

1. Open QGIS.
2. Enable Browser Panel.
3. Expand **ArcGIS REST Servers**.
4. Right-click → **New Connection**.
5. URL:
   https://gis.greenecountyohio.gov/webgis2/rest/services/OpenData/OpenData/MapServer
6. Connect and drag layers into the map.

### Notes

- If performance is slow, export locally.
- Ensure project CRS is set properly.

---

## 2.B — Esri ArcGIS Workflow (Pro or ArcMap)

### ArcGIS Pro

1. Open ArcGIS Pro.
2. Map → Add Data.
3. Paste MapServer URL.
4. Expand and select layers.

### ArcGIS Server Connection (Repeat Use)

1. Catalog → Servers → Add ArcGIS Server.
2. Use GIS services.
3. Paste MapServer URL.

### ArcMap (Legacy)

File → Add Data → Add Data From ArcGIS Server.

---

# Lesson 3 — Save Layers Locally

## 3.A — QGIS Export Workflow

1. Right-click layer → Export → Save Features As…
2. Format: GeoPackage
3. File: greene_county_opendata.gpkg
4. Confirm CRS.
5. Save.

Recommended: store multiple layers in one GeoPackage.

---

## 3.B — ArcGIS Pro Export Workflow

1. Right-click layer → Data → Export Features.
2. Output to File Geodatabase.
3. Run export.

---

# Lesson 4 — How REST Query Works (Technical Overview)

Layer endpoint example:

.../MapServer/1

Query endpoint:

.../MapServer/1/query

Parameters:

- where=1=1
- outFields=*
- returnGeometry=true
- f=geojson

Servers limit record count (e.g., 2000 features per request).

Export tools in QGIS and ArcGIS automatically manage pagination.

---

# Lesson 5 — Mini Lab Exercise

### Objective

Create an environmental screening map.

### Required Layers

- Streams
- FEMA Floodplain 2022
- Wetlands

### Deliverables

- Map PDF
- Saved GeoPackage
- Short explanation (1 paragraph):
  - Why local data is faster
  - How web GIS supports environmental screening

---

# Notes for Environmental Engineering Students

GIS proficiency is increasingly expected in:

- Phase I Environmental Site Assessments
- NEPA constraints analysis
- Floodplain review
- Stormwater planning
- Site selection

Understanding REST APIs enables:

- Automated data retrieval
- Integration with Python
- Creation of repeatable workflows
- Development of custom environmental tools

---

# References

Greene County GIS OpenData MapServer:
https://gis.greenecountyohio.gov/webgis2/rest/services/OpenData/OpenData/MapServer

Greene County GIS Data Hub:
https://gishub-gimsoh29.opendata.arcgis.com