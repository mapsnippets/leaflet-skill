# Leaflet AI Skill 🍃🤖

> Official **Leaflet** skill for AI coding assistants (Cursor, Claude Code, Antigravity, GitHub Copilot, Cline).

Maintained by **[MapSnippets](https://mapsnippets.com/)**.

---

## 📌 Overview

This skill provides comprehensive instructions, idiomatic patterns, and guardrails for building lightweight, responsive web maps with **pure native [Leaflet](https://leafletjs.com/)** (v1.9+).

### Core Capabilities Covered:
* **Map Initialization & Mobile Responsiveness:** Container setup, touch controls, invalidateSize handling.
* **Vector Tiles in Leaflet:** Rendering vector tile styles in Leaflet via `@maplibre/maplibre-gl-leaflet` (`L.maplibreGL`) and `Leaflet.VectorGrid`.
* **High-DPI Raster Tiles:** Retina tile layers (`@2x.png`), maxZoom, attribution, error tile handling.
* **GeoJSON Overlays:** Vector styling, pointToLayer custom icons/divIcons, onEachFeature event binding.
* **Marker Clustering:** High-performance clustering using `leaflet.markercluster`.
* **Panes & Z-Indexes:** Custom map panes for strict layer ordering (keeping labels on top of polygons).
* **Coordinate Conventions:** Preventing `[lat, lng]` (Leaflet) vs `[lng, lat]` (GeoJSON) coordinate inversion bugs.

---

## ⚡ Quick Start / Installation

### Cursor
Add to your project rules in `.cursor/rules/leaflet.mdc` or project instructions.

### Claude Code / Anthropic Projects
Add the `SKILL.md` content to your Project Instructions or system prompt.

### Antigravity / Agents
Clone or symlink into `.agents/skills/leaflet/`:
```bash
git clone https://github.com/mapsnippets/leaflet-skill.git .agents/skills/leaflet
```

---

## 🗺️ Recommended Basemap Defaults

### Vector Tiles in Leaflet (Recommended):
```javascript
import L from "leaflet";
import "@maplibre/maplibre-gl-leaflet";
import "leaflet/dist/leaflet.css";

const map = L.map("map").setView([50.0755, 14.4378], 13); // [lat, lng]

L.maplibreGL({
  style: "https://api.maptiler.com/maps/streets-v2/style.json?key=YOUR_MAPTILER_API_KEY"
}).addTo(map);
```

### High-DPI Raster Tiles:
```javascript
L.tileLayer("https://api.maptiler.com/maps/streets-v2/{z}/{x}/{y}.png?key=YOUR_MAPTILER_API_KEY", {
  tileSize: 512,
  zoomOffset: -1,
  minZoom: 1,
  attribution: "\u003ca href=\"https://www.maptiler.com/copyright/\" target=\"_blank\"\u003e&copy; MapTiler\u003c/a\u003e \u003ca href=\"https://www.openstreetmap.org/copyright\" target=\"_blank\"\u003e&copy; OpenStreetMap contributors\u003c/a\u003e",
  crossOrigin: true
}).addTo(map);
```

---

## 📄 License
MIT © [MapSnippets](https://mapsnippets.com/)
