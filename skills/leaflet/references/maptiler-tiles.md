# MapTiler Tile URLs for Leaflet

Complete reference for all MapTiler raster tile styles usable with `L.tileLayer()`.

> [MapTiler Maps API](https://docs.maptiler.com/cloud/api/maps/) · [Style gallery](https://www.maptiler.com/maps/)

---

## URL Pattern

```
https://api.maptiler.com/maps/{style}/{tileSize}/{z}/{x}/{y}.{format}?key=YOUR_MAPTILER_KEY
```

- `{style}` — style name (see table below)
- `{tileSize}` — `256` (standard) or omit for 512px (default)
- `{format}` — `png` (most styles) or `jpg` (satellite/hybrid)

---

## 256px Tiles (standard)

Best for simple setups. Use with default Leaflet `tileSize: 256`.

```js
L.tileLayer('https://api.maptiler.com/maps/streets-v4/256/{z}/{x}/{y}.png?key=YOUR_MAPTILER_KEY', {
  tileSize: 256,
  attribution: '\u00a9 <a href="https://www.maptiler.com/">MapTiler</a> \u00a9 <a href="https://www.openstreetmap.org/copyright">OpenStreetMap contributors</a>',
  crossOrigin: true
}).addTo(map);
```

## 512px HiDPI Tiles (retina)

Sharper on retina/HiDPI displays. Requires `tileSize: 512` and `zoomOffset: -1`.

```js
L.tileLayer('https://api.maptiler.com/maps/streets-v4/{z}/{x}/{y}.png?key=YOUR_MAPTILER_KEY', {
  tileSize: 512,
  zoomOffset: -1,
  attribution: '\u00a9 <a href="https://www.maptiler.com/">MapTiler</a> \u00a9 <a href="https://www.openstreetmap.org/copyright">OpenStreetMap contributors</a>',
  crossOrigin: true
}).addTo(map);
```

> **Note:** When using 512px tiles, omit the `/256/` from the URL path. The default tile size from MapTiler is 512px.

---

## All Available Styles

| Style Name | URL path | Format | Description |
|------------|----------|--------|-------------|
| Streets v4 | `maps/streets-v4` | png | Default street map with roads, labels, POIs |
| Streets v4 Dark | `maps/streets-v4-dark` | png | Dark theme streets |
| Streets v4 Light | `maps/streets-v4-light` | png | Light theme streets |
| Satellite | `maps/satellite` | jpg | Satellite/aerial imagery without labels |
| Hybrid | `maps/hybrid` | jpg | Satellite imagery with labels overlay |
| Outdoor v4 | `maps/outdoor-v4` | png | Hiking, cycling, trails, elevation |
| Outdoor v4 Dark | `maps/outdoor-v4-dark` | png | Dark theme outdoor |
| Topo v4 | `maps/topo-v4` | png | Topographic with contour lines |
| Dataviz | `maps/dataviz` | png | Clean background for data overlays |
| Dataviz Dark | `maps/dataviz-dark` | png | Dark theme dataviz |
| Dataviz Light | `maps/dataviz-light` | png | Light theme dataviz |
| Base v4 | `maps/base-v4` | png | Simplified, minimal |
| Base v4 Dark | `maps/base-v4-dark` | png | Dark simplified |
| Base v4 Light | `maps/base-v4-light` | png | Light simplified |
| Bright v4 | `maps/bright-v4` | png | Vibrant, colorful |
| Bright v4 Dark | `maps/bright-v4-dark` | png | Dark vibrant |
| Winter v4 | `maps/winter-v4` | png | Ski slopes, winter terrain |
| Ocean | `maps/ocean` | png | Maritime/nautical |
| Landscape | `maps/landscape` | png | Natural landscape emphasis |
| OpenStreetMap | `maps/openstreetmap` | jpg | Classic OSM look |
| Backdrop | `maps/backdrop` | png | High contrast with hillshading |
| Backdrop Dark | `maps/backdrop-dark` | png | Dark backdrop |

---

## L.tileLayer Options Reference

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `tileSize` | number | 256 | Tile size in pixels |
| `zoomOffset` | number | 0 | Offset for 512px tiles (use `-1`) |
| `minZoom` | number | 0 | Minimum zoom level |
| `maxZoom` | number | 18 | Maximum zoom level |
| `attribution` | string | — | Attribution text (required) |
| `crossOrigin` | boolean/string | false | Enable CORS for canvas export |
| `opacity` | number | 1 | Tile opacity (0-1) |
| `detectRetina` | boolean | false | Auto-detect retina displays |
| `subdomains` | string/array | 'abc' | Subdomains for load balancing |

---

## Creating a Tile Layer Helper

```js
function maptilerLayer(style, options = {}) {
  const format = ['satellite', 'hybrid', 'openstreetmap'].includes(style) ? 'jpg' : 'png';
  const useHiDPI = options.hiDPI !== false;

  const baseUrl = useHiDPI
    ? `https://api.maptiler.com/maps/${style}/{z}/{x}/{y}.${format}?key=YOUR_MAPTILER_KEY`
    : `https://api.maptiler.com/maps/${style}/256/{z}/{x}/{y}.${format}?key=YOUR_MAPTILER_KEY`;

  return L.tileLayer(baseUrl, {
    tileSize: useHiDPI ? 512 : 256,
    zoomOffset: useHiDPI ? -1 : 0,
    attribution: '\u00a9 <a href="https://www.maptiler.com/">MapTiler</a> \u00a9 <a href="https://www.openstreetmap.org/copyright">OpenStreetMap contributors</a>',
    crossOrigin: true,
    ...options
  });
}

// Usage
maptilerLayer('streets-v4').addTo(map);
maptilerLayer('satellite', { hiDPI: false }).addTo(map);
```

---

## Static Map Images

For non-interactive map images (emails, thumbnails, social cards):

```
https://api.maptiler.com/maps/{style}/static/{lng},{lat},{zoom}/{width}x{height}.png?key=YOUR_MAPTILER_KEY
```

Example:
```
https://api.maptiler.com/maps/streets-v4/static/14.4178,50.1167,12/800x600.png?key=YOUR_MAPTILER_KEY
```

Optional parameters: `@2x` for HiDPI, `markers` for pin overlays, `path` for route lines.
