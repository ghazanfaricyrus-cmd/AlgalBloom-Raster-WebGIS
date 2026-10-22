# South Australia Algal Bloom Monitoring | Raster & Vector WebGIS

**Sentinel-2-derived ΔMCI raster and stable-water vector layer · September 2026 · South Australia**

[**Explore the live WebGIS**](https://map.gistrategix.com.au/projects/algalbloom/)

## Project overview

This independent geospatial project explores how satellite remote sensing, reproducible spatial processing and web mapping can be combined to investigate changes in coastal-water spectral conditions in South Australia.

The application displays a **September 2026 Maximum Chlorophyll Index anomaly (ΔMCI) raster** derived from Sentinel-2 observations and compared with a **2019–2023 September reference baseline**. It also offers a separate **Sentinel-2 Stable Water Mask (2019–2023)** layer to show the valid-water analysis domain. The study covers selected coastal areas of South Australia, including Spencer Gulf, Gulf St Vincent and the Coorong/Murray Mouth region.

The **valid-water analysis domain** was derived from a **20-season (20-quarter) assessment spanning 2019–2023**, using Sentinel-2 observations to establish the stable-water mask at **20 m resolution**. This mask defines where water-based analysis is considered valid; it does not identify algal blooms.

**ΔMCI is a research-oriented spectral indicator, not a direct measurement or confirmation of harmful algal blooms, algal species, toxins or water safety.** The stable-water mask defines an analysis domain; it is not a bloom-detection layer.

## What I built

The wider project combines several GIS and remote-sensing tasks:

1. **Satellite data and processing:** Used Sentinel-2 imagery, **Google Earth Engine (GEE)** and **Python** to support multi-year processing and anomaly analysis.
2. **Reference comparison:** Compared the September 2026 MCI product with a **2019–2023 September baseline**.
3. **Spatial review and preparation:** Used **ArcGIS Pro** and **QGIS** to review raster outputs, coordinate systems, spatial alignment and web-publication suitability.
4. **Raster publication:** Published the georeferenced ΔMCI raster through **GeoServer WMS**, with a diverging `vik` colour style.
5. **Vector publication:** Derived the valid-water domain from **20 seasonal quarters across 2019–2023**, then prepared polygons from the 20 m analysis domain and made them available through **PostGIS and GeoServer WFS**. The published detailed layer contains **11,014 polygons**.
6. **WebGIS delivery:** Built a responsive **Leaflet** viewer combining raster WMS, vector WFS, a generalised GeoJSON overview and selectable basemaps.

These describe the **broader project context**. This repository contains the browser-facing viewer and its lightweight GeoJSON overview, **not** the full GEE/Python workflows, scientific raster, detailed PostGIS dataset or server configuration.

## Live application

**WebGIS:** https://map.gistrategix.com.au/projects/algalbloom/

The ΔMCI raster uses a diverging colour scale: **blue** for lower MCI relative to the reference, **neutral** for near-reference values and **red** for higher MCI. These colours are **not** hazard categories.

### Front-end features

- Two selectable basemaps: **Esri Satellite** and **Esri Streets**.
- Independently switchable **September 2026 ΔMCI WMS raster** and **Sentinel-2 Stable Water Mask (2019–2023)** layers.
- A generalised water-mask **GeoJSON overview** at wider scales, switching to viewport-based **WFS polygon requests** at closer scales (zoom 9 and above, subject to a feature limit).
- Water-polygon popups distinguishing the analysis domain from bloom or water-safety assessments.
- An in-map ΔMCI legend that appears **only when the raster layer is enabled**, with a collapsible layout on mobile/tablet screens.
- Responsive map controls, scale bar and an informational raster-loading notice.
- A browser-side queue limiting the raster layer to **three concurrent WMS tile requests**. This is a client-side performance safeguard, not a server-wide rate limit.
- Scientific interpretation notes and a link to official South Australian information.

When a WFS request reaches the viewer's feature limit, the application retains the generalised overview rather than presenting an incomplete detailed subset as complete coverage.

## System overview

```text
Broader analytical workflow (not included in this repository)
Sentinel-2 observations
        |
Google Earth Engine + Python
        |
2019–2023 baseline comparison + September 2026 ΔMCI
        |
Desktop GIS review and data preparation
        |
        +--> Scientific ΔMCI raster --> GeoServer WMS ----+
        |                                                |
        +--> Valid-water polygons --> PostGIS --> WFS ----+--> Leaflet WebGIS
        |                                                |
        +--> Generalised water GeoJSON overview ----------+
                                                         |
                                             Esri basemap services
```

The analytical working grid uses **20 m resolution** in **GDA2020 / Australian Albers (EPSG:9473)**. The browser uses Leaflet's web-map projection and requests rendered raster tiles and geographic vector features from the published services. It does not reproduce the scientific analysis.

## Technologies

| Area | Technologies |
| --- | --- |
| Remote sensing and analysis (wider project) | Sentinel-2, Google Earth Engine, Python |
| Desktop GIS and data preparation (wider project) | ArcGIS Pro, QGIS |
| Spatial database and vector publication | PostgreSQL/PostGIS, GeoServer, OGC WFS |
| Raster publication | GeoServer, OGC WMS |
| Browser-based WebGIS | HTML, CSS, JavaScript, Leaflet 1.9.4, GeoJSON |
| Basemaps | Esri World Imagery, Esri World Street Map (third-party services) |

## Repository contents

```text
AlgalBloom-Raster-WebGIS/
├── index.html                    # Leaflet raster + vector viewer
├── stable_water_overview.geojson # Generalised water-mask overview
├── README.md                     # Project documentation
└── .gitignore                    # Local-file and data-output exclusions
```

The repository name reflects the original raster-focused phase; the current viewer includes both raster and vector layers.

## Running and hosting

For the **fully working production application**, use the [live WebGIS](https://map.gistrategix.com.au/projects/algalbloom/).

The source can be inspected locally, but it is **not a self-contained deployable copy**: the current `index.html` uses production-root-relative paths (`/algalbloom/wfs` and `/projects/algalbloom/stable_water_overview.geojson`) for the vector layer. Therefore, merely placing the GeoJSON beside `index.html`, opening the file via `file://`, or publishing the unchanged HTML on GitHub Pages **does not reproduce the complete vector functionality**. A different host would require path configuration and appropriate service access/CORS settings. The raster and basemaps also depend on live third-party/public services.

The viewer generates normal map-service requests. Please do not use the public endpoints for load testing.

## Scientific scope and limitations

- **ΔMCI** describes differences in a satellite-derived spectral index relative to a reference period. It does **not** directly establish algal species, harmful algal bloom presence, toxin concentrations or public-health risk.
- **Stable water** represents the valid-water analysis domain; it is **not** an algal bloom classification, a current shoreline survey or a water-safety assessment.
- Cloud conditions, atmospheric and water-surface effects, observation coverage and nearshore/intertidal complexity can affect satellite-based interpretation.
- This public repository does **not** provide evidence of field validation, quantified uncertainty, forecast skill or a validated operational early-warning service.
- For health, beach-use or environmental advice, consult [official South Australian algal bloom information](https://www.algalbloom.sa.gov.au/).

## Publication and access boundaries

This repository publishes only the browser-facing source and a **generalised GeoJSON overview**. It does **not** include credentials, infrastructure settings, private audit material, raw Sentinel-2 imagery, full-resolution scientific raster exports, the complete detailed polygon database or server backups.

The WMS and WFS addresses referenced by `index.html` are **public read-only map-service endpoints**, not administrative credentials. GeoServer access control, request limits and server security are managed separately from this repository.

Third-party libraries, satellite products and basemap services remain subject to their respective terms, licences and attribution requirements.

## Project and authorship

**Mehdi Ghazanfari**  
Senior GIS Analyst | Spatial Data • Analysis • GIS Systems | Infrastructure • Environment • Decision Support

This independent technical portfolio project illustrates a geospatial delivery workflow from satellite-data analysis through raster and vector publication to a browser-accessible WebGIS. This GitHub repository is a **snapshot of the presentation layer**, separate from the production deployment and underlying analysis.

**Licence:** No open-source licence has been granted for the repository's original code at this time. Public visibility on GitHub does not itself grant permission to reuse, modify or redistribute it.
