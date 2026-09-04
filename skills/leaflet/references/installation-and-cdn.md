# Leaflet Installation & CDN Reference

Source: https://leafletjs.com/download.html

This guide details all methods for loading Leaflet: Hosted CDN (with Subresource Integrity), NPM/Yarn/Bun package managers, and offline self-hosted archives.

---

## 1. Hosted CDN (Vanilla HTML / No Build Step)

Include Leaflet CSS in the `<head>` and Leaflet JS right before closing `</body>` or in `<head>`:

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8" />
  <title>Leaflet Map</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">

  <!-- 1. Leaflet CSS (Official unpkg with SRI) -->
  <link rel="stylesheet" 
        href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"
        integrity="sha256-p4NxAoJBhIIN+hmNHrzRCf9tD/miZyoHS5obTRR9BMY="
        crossorigin=""/>

  <!-- 2. Leaflet JavaScript (Make sure this is after Leaflet CSS) -->
  <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"
          integrity="sha256-20nQCchB9co0qIjJZRGuk2/Z9VM+kNiyxNV1lvTlZBo="
          crossorigin=""></script>

  <style>
    #map { height: 100vh; width: 100%; margin: 0; padding: 0; }
  </style>
</head>
<body>
  <div id="map"></div>

  <script>
    const map = L.map('map').setView([50.0755, 14.4378], 13);
    
    L.tileLayer('https://api.maptiler.com/maps/streets-v4/{z}/{x}/{y}.png?key=YOUR_API_KEY', {
      tileSize: 512,
      zoomOffset: -1,
      minZoom: 1,
      attribution: '<a href="https://www.maptiler.com/copyright/" target="_blank">&copy; MapTiler</a> <a href="https://www.openstreetmap.org/copyright" target="_blank">&copy; OSM</a>'
    }).addTo(map);

    L.marker([50.0755, 14.4378]).addTo(map).bindPopup('Prague, Czechia').openPopup();
  </script>
</body>
</html>
```

### Alternative CDN Providers:
* **cdnjs:**
  * CSS: `https://cdnjs.cloudflare.com/ajax/libs/leaflet/1.9.4/leaflet.css`
  * JS: `https://cdnjs.cloudflare.com/ajax/libs/leaflet/1.9.4/leaflet.js`
* **jsDelivr:**
  * CSS: `https://cdn.jsdelivr.net/npm/leaflet@1.9.4/dist/leaflet.css`
  * JS: `https://cdn.jsdelivr.net/npm/leaflet@1.9.4/dist/leaflet.js`

---

## 2. Package Managers (NPM / Yarn / PNPM / Bun)

For modern bundlers (Vite, Webpack, Rollup, Next.js, Parcel):

### Install:
```bash
# npm
npm install leaflet
npm install -D @types/leaflet

# yarn
yarn add leaflet
yarn add -D @types/leaflet

# pnpm
pnpm add leaflet
pnpm add -D @types/leaflet

# bun
bun add leaflet
bun add -D @types/leaflet
```

### Ingestion in ES Modules:
```javascript
import L from "leaflet";
import "leaflet/dist/leaflet.css"; // CRITICAL: Always import CSS in root JS/TS file!
```

---

## 3. Vector Tiles CDN Setup (Leaflet + MapLibre GL Leaflet)

To use sharp vector tiles directly from CDN without a build tool:

```html
<!-- Leaflet -->
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" crossorigin=""/>
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js" crossorigin=""></script>

<!-- MapLibre GL JS (Underlying Vector Renderer) -->
<link rel="stylesheet" href="https://unpkg.com/maplibre-gl@4.7.1/dist/maplibre-gl.css" crossorigin=""/>
<script src="https://unpkg.com/maplibre-gl@4.7.1/dist/maplibre-gl.js" crossorigin=""></script>

<!-- MapLibre GL Leaflet Plugin -->
<script src="https://unpkg.com/@maplibre/maplibre-gl-leaflet@0.0.22/leaflet-maplibre-gl.js" crossorigin=""></script>

<div id="map" style="height: 100vh;"></div>

<script>
  const map = L.map('map').setView([50.0755, 14.4378], 13);

  L.maplibreGL({
    style: 'https://api.maptiler.com/maps/streets-v4/style.json?key=YOUR_API_KEY'
  }).addTo(map);
</script>
```

---

## 4. Self-Hosted / Offline Downloads

For air-gapped systems or offline apps:
* Stable Release Archive: `https://github.com/Leaflet/Leaflet/releases/download/v1.9.4/leaflet.zip`
* Unpack `leaflet.js`, `leaflet.css`, and the `images/` directory (`marker-icon.png`, `marker-shadow.png`) into your public assets folder.
