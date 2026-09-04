# WMS Layer with GetFeatureInfo Click Query

> **Documentation Reference:** [WMS Layer with GetFeatureInfo Click Query](https://leafletjs.com/examples/wms/wms.html)
> Category: **Enterprise Services & Vector Tiles**

## Overview
Executes OGC `GetFeatureInfo` HTTP queries on map click to retrieve and display attribute information from a WMS service in Leaflet popups.

## Complete Standalone Example

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Leaflet - WMS GetFeatureInfo</title>
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
  <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
  <style>body { margin: 0; padding: 0; } #map { width: 100vw; height: 100vh; }</style>
</head>
<body>
  <div id="map"></div>
  <script>
    const MAPTILER_KEY = 'YOUR_MAPTILER_API_KEY';

    const map = L.map('map').setView([45.5, -122.6], 8);

    L.tileLayer(`https://api.maptiler.com/maps/streets-v4/{z}/{x}/{y}.png?key=${MAPTILER_KEY}`, {
      tileSize: 512, zoomOffset: -1, attribution: '&copy; MapTiler &copy; OpenStreetMap'
    }).addTo(map);

    const wmsUrl = 'https://mesonet.agron.iastate.edu/cgi-bin/wms/nexrad/n0r.cgi';
    const wmsLayer = L.tileLayer.wms(wmsUrl, {
      layers: 'nexrad-n0r',
      format: 'image/png',
      transparent: true,
      attribution: 'Weather data &copy; IEM'
    }).addTo(map);

    map.on('click', function (e) {
      const point = map.latLngToContainerPoint(e.latlng, map.getZoom());
      const size = map.getSize();
      const bounds = map.getBounds();
      const sw = bounds.getSouthWest();
      const ne = bounds.getNorthEast();

      const params = {
        request: 'GetFeatureInfo',
        service: 'WMS',
        srs: 'EPSG:4326',
        version: '1.1.1',
        bbox: `${sw.lng},${sw.lat},${ne.lng},${ne.lat}`,
        height: size.y,
        width: size.x,
        layers: 'nexrad-n0r',
        query_layers: 'nexrad-n0r',
        info_format: 'text/html',
        x: Math.round(point.x),
        y: Math.round(point.y)
      };

      const url = wmsUrl + L.Util.getParamString(params, wmsUrl, true);
      L.popup()
        .setLatLng(e.latlng)
        .setContent(`<iframe src="${url}" width="200" height="100" frameborder="0"></iframe>`)
        .openOn(map);
    });
  </script>
</body>
</html>
```

## Key API Features
- Native Leaflet API implementation.
- Modern MapTiler Planet v4 raster tiles.
- Clean and lightweight dependencies.
