# Cloud-Optimized GeoTIFF (COG) with GeoRaster

> Official Reference: [Cloud-Optimized GeoTIFF (COG) with GeoRaster](https://github.com/GeoTIFF/georaster-layer-for-leaflet)
> Category: **Enterprise Services & Vector Tiles**

## Overview
Renders Cloud-Optimized GeoTIFF rasters directly on the client with GPU color ramps via `georaster-layer-for-leaflet`.

## Complete Standalone Example

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Leaflet - GeoTIFF Raster Layer</title>
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
  <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
  <script src="https://unpkg.com/georaster"></script>
  <script src="https://unpkg.com/georaster-layer-for-leaflet"></script>
  <style>body { margin: 0; padding: 0; } #map { width: 100vw; height: 100vh; }</style>
</head>
<body>
  <div id="map"></div>
  <script>
    const MAPTILER_KEY = 'YOUR_MAPTILER_API_KEY';

    const map = L.map('map').setView([0, 0], 2);

    L.tileLayer(`https://api.maptiler.com/maps/dataviz-v4-dark/{z}/{x}/{y}.png?key=${MAPTILER_KEY}`, {
      tileSize: 512, zoomOffset: -1, attribution: '&copy; MapTiler'
    }).addTo(map);

    const cogUrl = 'https://sentinel-cogs.s3.us-west-2.amazonaws.com/sentinel-s2-l2a-cogs/10/T/EG/2020/7/S2B_10TEG_20200723_0_L2A/TCI.tif';

    parseGeoraster(cogUrl).then(georaster => {
      const layer = new GeoRasterLayer({
        georaster: georaster,
        opacity: 0.8,
        resolution: 256
      });
      layer.addTo(map);
      map.fitBounds(layer.getBounds());
    });
  </script>
</body>
</html>
```

## Key API Features
- Native Leaflet API implementation.
- Modern MapTiler Planet v4 raster tiles.
- Clean and lightweight dependencies.
