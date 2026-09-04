# Leaflet Working Examples Directory 🍃🧪

This directory contains standalone, production-ready code examples demonstrating the most critical workflows in Leaflet.

---

## 📂 Catalog of Working Examples:

1. **[`01_basic_raster_map.html`](01_basic_raster_map.html)** — High-DPI 512px raster basemap with `tileSize: 512, zoomOffset: -1` and popup.
2. **[`02_vector_tiles_maplibre_gl_leaflet.html`](02_vector_tiles_maplibre_gl_leaflet.html)** — Vector tiles in Leaflet via `@maplibre/maplibre-gl-leaflet` bridge.
3. **[`03_geojson_choropleth_and_legend.html`](03_geojson_choropleth_and_legend.html)** — GeoJSON choropleth, hover highlight interaction, and custom `L.control` legend.
4. **[`04_marker_clustering.html`](04_marker_clustering.html)** — `leaflet.markercluster` with 400 points and spiderfy zooming.

---

## 🚀 How to Run Locally

Open any `.html` file directly in your browser or serve with:
```bash
npx serve .
# or
python -m http.server 8000
```
> **Note:** Replace `YOUR_API_KEY` with your free key from [MapTiler Cloud](https://docs.maptiler.com/cloud/api/authentication-key/).
