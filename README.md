# South Australia Algal Bloom Monitoring | Raster WebGIS

**Sentinel-2-derived ΔMCI visualisation · September 2026 · South Australia**

[**Explore the live WebGIS**](https://map.gistrategix.com.au/projects/algalbloom/)

## Project overview

This independent geospatial project explores how satellite remote sensing, reproducible spatial processing and web mapping can be combined to investigate changes in coastal-water spectral conditions in South Australia.

The September 2026 application displays a **continuous raster of the Maximum Chlorophyll Index anomaly (ΔMCI)** derived from Sentinel-2 observations and compared with a corresponding **2019–2023 September reference baseline**. The map covers selected coastal areas of South Australia, including the Spencer Gulf, Gulf St Vincent and Coorong/Murray Mouth region.

The objective is to present spatial patterns of an ocean-colour-related spectral indicator in an accessible, interactive format. **ΔMCI is not a direct measurement or confirmation of harmful algal blooms, algal species, toxins or water safety.**

## What I built

The wider project brings together multiple parts of a GIS and remote-sensing workflow:

1. **Satellite data and processing:** Worked with Sentinel-2 imagery through **Google Earth Engine (GEE)** and **Python** to support multi-year processing and anomaly analysis.
2. **Reference comparison:** Used the **2019–2023** period as the historical reference for interpreting the September 2026 MCI product.
3. **Geospatial preparation and quality review:** Used desktop GIS tools, including **ArcGIS Pro** and **QGIS**, to review raster outputs, spatial alignment, coordinate systems and suitability for map publication.
4. **Raster publication:** Prepared a georeferenced raster product for presentation through **GeoServer** using the **OGC Web Map Service (WMS)** standard.
5. **Interactive delivery:** Developed a **Leaflet-based WebGIS** to make the resulting map accessible from desktop and mobile browsers.

These steps describe the **broader project context**. This repository contains the **front-end WebGIS source only**, not the complete GEE/Python workflow, processing notebooks, underlying imagery, GIS project files, raster datasets or server configuration.

## Live application

**WebGIS:** https://map.gistrategix.com.au/projects/algalbloom/

The application overlays a September 2026 ΔMCI raster on Esri World Imagery. A diverging colour scale represents **lower-than-reference (blue)**, **near-reference (neutral)** and **higher-than-reference (red)** MCI values.

### Front-end features

- Interactive satellite basemap with an optional GeoServer WMS raster overlay.
- Continuous-raster display with a matching in-map legend and scale bar.
- Responsive layout and collapsible legend for mobile/tablet screens.
- Browser-side queue limiting this viewer to **three concurrent WMS tile requests**.
- Visible tile-error feedback and **manual retry** of the current view; no automatic retry loop.
- Scientific interpretation notes and a link to official South Australian updates.

The request queue is a **client-side performance safeguard**, not a rate limit or security policy for GeoServer.

## System overview

```text
Broader geospatial project (not included in this repository)
Sentinel-2 observations
        |
Google Earth Engine + Python
        |
Multi-year reference comparison and Delta MCI analysis
        |
Desktop GIS review and raster preparation
        |
Published raster + GeoServer WMS
        |
        v
This repository
Browser -> Leaflet (index.html) -> HTTPS WMS map images
                       + Esri World Imagery basemap
```

The analytical project uses a **20 m working raster grid** in **GDA2020 / Australian Albers (EPSG:9473)**. The web viewer requests map images from GeoServer; it does not distribute the scientific raster or reproduce the analytical processing.

## Technologies

| Area | Technologies |
| --- | --- |
| Satellite remote sensing and processing (wider project) | Sentinel-2, Google Earth Engine, Python |
| GIS review and raster preparation (wider project) | ArcGIS Pro, QGIS |
| Web mapping (this repository) | HTML, CSS, JavaScript, Leaflet 1.9.4 |
| Published map service | GeoServer, OGC WMS |
| Basemap | Esri World Imagery (third-party service) |

## Repository contents

```text
AlgalBloom-Raster-WebGIS/
├── index.html   # Standalone Leaflet front-end
├── README.md    # Project context and documentation
└── .gitignore   # Local-file and data-output exclusions
```

## Run the viewer locally

1. Download or clone this repository.
2. Open `index.html` in a modern web browser.
3. Keep an internet connection available: the viewer loads Leaflet, Esri basemap tiles and raster map images from external services.

This is a **viewer**, not a self-contained GIS server. The live WMS is an external runtime dependency, and availability or performance can change. Opening the viewer generates normal map-service requests; please do not use it for load testing.

## Scientific scope and limitations

- **ΔMCI** describes a difference in a satellite-derived spectral index relative to the reference period; it is **not** a direct observation of algal species, HAB presence, toxin concentrations or public-health risk.
- Map colours are **relative spectral differences**, not hazard categories or advisories.
- Cloud conditions, atmospheric and water-surface effects, observation coverage and nearshore/intertidal complexity can affect satellite-based interpretation.
- This public front-end repository does **not** provide evidence of field validation, quantified uncertainty, forecast skill or a validated operational early-warning service.
- For health, beach-use or environmental advice, consult [official South Australian algal bloom information](https://www.algalbloom.sa.gov.au/).

## Publication and access boundaries

This repository intentionally publishes only the browser-facing source. It does **not** include credentials, infrastructure settings, private audit material, raw Sentinel-2 imagery, raster exports, proprietary datasets or server backups.

The WMS endpoint referenced by `index.html` is a **public map-service address** used by the live application, not an administrative login or secret. GeoServer access control, request limits and server security are managed separately from this repository.

Third-party libraries, satellite products and basemap services remain subject to their respective terms, licences and attribution requirements.

## Project and authorship

**Mehdi Ghazanfari**  
Senior GIS Analyst | Spatial Data • Analysis • GIS Systems | Infrastructure • Environment • Decision Support
This is an independent technical portfolio project illustrating an end-to-end geospatial delivery approach, from remote-sensing analysis to a browser-accessible raster map. The GitHub repository is a **snapshot of the presentation layer**, separate from the live deployment and the earlier polygon-based WebGIS work.

**Licence:** No open-source licence has been granted for the repository's original code at this time. Public visibility on GitHub does not itself grant permission to reuse, modify or redistribute it.
