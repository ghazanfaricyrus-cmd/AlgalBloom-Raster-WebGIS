# South Australia ΔMCI WebGIS — September 2026

A client-side interactive raster viewer built with **Leaflet** and a **GeoServer WMS** for displaying September 2026 Sentinel-2 Maximum Chlorophyll Index anomaly (ΔMCI) over parts of coastal South Australia.

This repository is a **portfolio presentation of the WebGIS viewer**, not the full satellite-processing pipeline or a complete server deployment.

## What the application demonstrates

- Browser-based rendering of a continuous ΔMCI raster through OGC WMS.
- Satellite imagery background from Esri World Imagery.
- Responsive map controls and a collapsible mobile legend.
- Browser-side request queue capped at three concurrent WMS tile requests.
- Clear tile-error status and manual retry; no automatic retry loop.
- Scientific caveat embedded in the map interface.

## Architecture

```text
Browser (Leaflet; index.html)
    ├── Esri World Imagery tiles (third-party basemap)
    └── HTTPS → Public GeoServer WMS → Published ΔMCI raster and style
```

The viewer uses a public WMS endpoint and an existing GeoServer layer and style. **The data raster, server configuration, credentials and scientific processing code are not included in this repository.**

## Viewing the application

Open `index.html` in a modern browser. Internet access is required for Leaflet, the basemap and the remote WMS service. Availability and rendering speed depend on those services. A blank raster view or tile errors may indicate a service/network issue, not necessarily a JavaScript problem.

The application currently points to a live public WMS. Sharing or previewing the page can trigger requests to that service. Do not run load tests against it.

## Scientific interpretation and limitations

ΔMCI here refers to the difference between September 2026 MCI and a September baseline associated with 2019–2023. This is a **spectral research indicator**, not direct confirmation of harmful algal bloom presence, algal species, toxicity or water safety. The displayed colour scale distinguishes lower/near-baseline/higher values; it is not a health-risk classification.

This repository does not establish field validation, uncertainty bounds or operational early-warning performance. Consult [South Australia's official algal bloom updates](https://www.algalbloom.sa.gov.au/) for public-health and environmental updates.

## Technology

- HTML, CSS and JavaScript
- [Leaflet 1.9.4](https://leafletjs.com/)
- [GeoServer WMS](https://docs.geoserver.org/latest/en/user/services/wms/reference/)
- Esri World Imagery basemap (third-party service)

## Publication boundaries

Only the front-end source is presented. No Sentinel-2 imagery, proprietary third-party datasets, private audit files, CDSE experiments, local project paths, credential files or server backups are distributed here. The publicly visible WMS URL is a runtime endpoint, **not** a secret or administrative API.

Third-party tiles, imagery, and map data remain subject to their providers' terms and attribution requirements. No reuse licence for this project's source code is granted by this repository unless a separate licence is added.

## Repository status

This is a standalone front-end source snapshot prepared for an independent raster-WebGIS portfolio repository. It is separate from the earlier polygon-based WebGIS repository. Static source review does not substitute for browser, accessibility, performance or live-service testing.

**Preparing or downloading this source does not publish it to GitHub or deploy it to production.**
