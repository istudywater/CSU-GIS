

# Greene County (Ohio) GIS Open Data via ArcGIS REST (MapServer)

This tutorial shows how to:

1. Understand what an **ArcGIS REST Map Service** is.
2. Connect to Greene County’s **Open Data** service in **QGIS** and **Esri ArcGIS**.
3. Save (cache) layers **locally** for offline use and faster performance.

> Audience: undergraduate environmental engineering students learning GIS fundamentals.

---

## Lesson 0 — What you are looking at (ArcGIS REST in plain English)

### What is a “REST API” in GIS?

A **REST API** is a web-based interface where software can request data by calling a **URL endpoint**.

In the ArcGIS world, many datasets are published as web services. The service exposes:

- **A directory page** (human-readable)
- **JSON responses** (machine-readable)
- Operations like **query**, **identify**, and **export**

### The Greene County endpoint

Service endpoint (directory):

- `https://gis.greenecountyohio.gov/webgis2/rest/services/OpenData/OpenData/MapServer`

This is an **ArcGIS Server Map Service** (a `MapServer`). A MapServer can contain many layers (parcels, zoning, floodplains, etc.). Depending on how the service is published, layers may behave like:

- **Renderable map layers** (good for viewing)
- **Queryable feature layers** (good for downloading features / attributes)

### Key concepts you’ll see in most ArcGIS REST services

- **Service**: a collection of layers, identified by a URL ending in `/MapServer`
- **Layer ID**: each layer has an integer ID (e.g., Parcels is `/MapServer/1`)
- **JSON view**: add `?f=json` or click **JSON** to see machine-readable metadata
- **MaxRecordCount**: servers often limit how many features you can request at once (this service reports 2000)
- **Pagination**: if a layer has more than the max record count, you request it in chunks (e.g., using `resultOffset` / `resultRecordCount`)

---

## Lesson 1 — Explore the layers available

Open the service directory:

- `https://gis.greenecountyohio.gov/webgis2/rest/services/OpenData/OpenData/MapServer`

You’ll see a list of layers. Each layer has:

- A **name** (e.g., *Parcels*)
- An **ID** in parentheses (e.g., *(1)*)

### Layer list (as published in the service)

Common layers students often use for environmental work:

- Parcels (1)
- Addresses (5)
- Road Centerlines (6)
- Streams (17)
- Hydrology Features (19)
- FEMA Floodplain 2017 (8)
- FEMA Floodplain 2022 (48)
- Wetlands (43)
- Soils (29)
- Land Cover (27)
- Zoning (15)

There are additional public safety, voting, parks, and other reference layers.

---

## Lesson 2 — Connect to the service in QGIS

### Option A (recommended): Add an ArcGIS REST Server connection

1. Open **QGIS**.
2. Turn on the Browser Panel: **View → Panels → Browser Panel**.
3. In the Browser, expand **ArcGIS REST Servers**.
4. Right-click **ArcGIS REST Servers → New Connection**.
5. Enter:
   - **Name**: `Greene County OpenData`
   - **URL**: `https://gis.greenecountyohio.gov/webgis2/rest/services/OpenData/OpenData/MapServer`
6. Click **OK**.
7. Expand the new connection, then drag layers into the map.

### Option B: Add a single layer by its Layer URL

If you want a specific layer, you can try adding it directly:

- Example (Parcels): `https://gis.greenecountyohio.gov/webgis2/rest/services/OpenData/OpenData/MapServer/1`

In QGIS:

1. **Layer → Add Layer → Add ArcGIS REST Server Layer** (menu name may vary slightly by version).
2. Use the layer URL above.

> Tip: If your QGIS build doesn’t show an ArcGIS REST option, your version may be missing the provider or it may be hidden behind the Browser panel workflow.

### Troubleshooting (QGIS)

- If layers draw slowly: you are streaming data from the web. Use the **Save locally** lesson below.
- If attributes don’t load: the layer may not support feature queries (some MapServer layers are view-only). Try a different layer.
- If CRS looks wrong: set the project CRS and enable “on-the-fly” reprojection.

---

## Lesson 3 — Connect to the service in Esri ArcGIS (Pro or ArcMap)

### ArcGIS Pro (recommended)

**Method 1 — Add from Portal/URL (quickest)**

1. Open **ArcGIS Pro**.
2. Go to **Map** tab → **Add Data**.
3. Choose **Data From Path** (or similar) and paste:
   - `https://gis.greenecountyohio.gov/webgis2/rest/services/OpenData/OpenData/MapServer`
4. Pro will add the service; expand it to choose layers.

**Method 2 — Add an ArcGIS Server connection (cleaner for repeated use)**

1. In the **Catalog** pane: **Project → Servers → Add ArcGIS Server**.
2. Choose **Use GIS services**.
3. Paste the MapServer URL.
4. After it connects, browse to layers and add them to your map.

### ArcMap (legacy)

1. Use **File → Add Data → Add Data From ArcGIS Server**.
2. Connect to a GIS service and paste the MapServer URL.
3. Add the layers.

> Note: Many employers still have ArcMap installed, but ArcGIS Pro is the actively developed platform.

---

## Lesson 4 — Save layers locally (QGIS)

When you add a web layer, you’re streaming data live from the server. For analysis, it is often better to save a local copy.

### Best practice: Save to a GeoPackage (one file, many layers)

1. In QGIS, right-click the layer → **Export → Save Features As…**
2. Set:
   - **Format**: `GeoPackage`
   - **File name**: `greene_county_opendata.gpkg`
   - **Layer name**: something like `parcels` or `fema_floodplain_2022`
3. Choose:
   - **CRS**: keep the layer CRS (recommended), or reproject to your project CRS
4. Click **OK**.

Repeat for multiple layers; they can all live inside the same `.gpkg`.

### Alternative formats

- **Shapefile** (`.shp`): common but has limitations (field name length, multiple files per layer)
- **GeoJSON** (`.geojson`): good for web and quick sharing
- **CSV**: only for tabular exports (geometry may be lost unless you include WKT/X/Y)

### Verify your local layer

- Turn off (uncheck) the web service layer.
- Turn on the local GeoPackage layer.
- Confirm:
  - geometry draws correctly
  - attributes are present

---

## Lesson 5 — Save layers locally (ArcGIS Pro)

### Export to a File Geodatabase (recommended)

1. In ArcGIS Pro, right-click the layer in **Contents**.
2. Choose **Data → Export Features**.
3. Set an output location such as:
   - a **File Geodatabase** (e.g., `GreeneCounty.gdb`)
4. Run the export.

### Export to Shapefile or GeoJSON

- Same process: **Export Features**, then pick the output type.

### Why export?

- Faster rendering and analysis
- Works offline
- Avoids server downtime or throttling

---

## Lesson 6 — How ArcGIS REST “query” works (light technical)

Most GIS APIs work like this:

1. You choose a layer endpoint (example: Parcels layer):
   - `.../MapServer/1`
2. You use the **query** operation to request features:
   - `.../MapServer/1/query`
3. You send parameters such as:

- `where=1=1` (means “return everything”)
- `outFields=*` (return all attributes)
- `returnGeometry=true` (include geometry)
- `f=geojson` (return in GeoJSON format)

### Example (do not paste into a browser unless you understand it)

```
.../MapServer/1/query?where=1%3D1&outFields=*&returnGeometry=true&f=geojson
```

### Why you can’t always pull “everything” at once

Servers usually limit results (for performance). This service reports a max record count of **2000**, which means downloads often require **pagination**.

In practice, QGIS and ArcGIS handle this for you when you use “Export” or “Save Features As…”. If you ever write Python code to pull data, you’ll need to request features in batches.

---

## Lesson 7 — Mini-lab for students (30–60 minutes)

### Goal

Create a simple environmental screening map showing:

- Streams
- FEMA Floodplain (2022)
- Wetlands
- Parcels (optional, can be heavy)

### Steps

1. Connect to the MapServer in QGIS.
2. Add Streams (17), FEMA Floodplain 2022 (48), Wetlands (43).
3. Change symbology:
   - Floodplain: transparent fill with outline
   - Wetlands: light fill
   - Streams: line symbol
4. Save each layer locally to a GeoPackage.
5. Create a map layout:
   - title
   - legend
   - north arrow
   - scale bar
6. Export a PDF.

### Discussion questions

- Why does a local GeoPackage feel faster than streaming web layers?
- What limitations do you notice in attributes (fields)?
- How might these layers support a Phase I ESA or a screening-level NEPA constraints map?

---

## Notes and ethics

- **Use open data responsibly.** This is public-facing county GIS data—still verify suitability for engineering decisions.
- **Don’t treat web layers as authoritative survey data.** For design, obtain stamped surveys / engineered datasets.

---

## References

- Greene County GIS OpenData MapServer (ArcGIS REST):
  - https://gis.greenecountyohio.gov/webgis2/rest/services/OpenData/OpenData/MapServer
- Greene County GIS Data Hub
  - https://gishub-gimsoh29.opendata.arcgis.com