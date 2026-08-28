# Leaflet — Agent Skill 🍃🤖

> Official **Leaflet** skill for AI coding assistants (Cursor, Claude Code, Antigravity, GitHub Copilot, Windsurf, Cline).

Maintained by **[MapSnippets](https://mapsnippets.com/)** — Open-source geospatial snippets, guides, and agent tools.

---

🌐 [Website](https://mapsnippets.com/) &nbsp; 📚 [Documentation](https://leafletjs.com/reference.html)

---

<br>

<details>
<summary><b>Table of Contents</b></summary>
<ul>
<li><a href="#what-it-does">What it does</a></li>
<li><a href="#how-skills-plugins-and-agents-fit-together">How skills, plugins, and agents fit together</a></li>
<li><a href="#-installation">Installation</a></li>
<li><a href="#-repository-layout">Repository layout</a></li>
<li><a href="#-quickstart-examples">Quickstart Examples</a></li>
<li><a href="#links">Links</a></li>
<li><a href="#-contributing">Contributing</a></li>
<li><a href="#-license">License</a></li>
</ul>
</details>

<br>

## What it does

A skill is on-demand expertise: the agent loads it only when your request matches the skill's description, then follows its instructions instead of guessing. When you ask for Leaflet maps, vector tiles, marker clustering, custom panes, or GeoJSON layers, this skill makes the agent:

- **Generate pure native Leaflet code** (v1.9+) with proper container lifecycle, touch event handling, and `invalidateSize` resize logic.
- **Render modern vector tiles in Leaflet** using standard plugins like `@maplibre/maplibre-gl-leaflet` (`L.maplibreGL`) for sharp zooming and vector rendering.
- **Implement high-DPI raster tile layers** with `tileSize: 512`, `zoomOffset: -1`, and `@2x` retina scaling.
- **Cluster high-density markers** cleanly using `leaflet.markercluster` with custom cluster icons and spiderfy behavior.
- **Prevent coordinate order inversion bugs** — strictly enforces Leaflet's `[latitude, longitude]` API convention vs GeoJSON's `[longitude, latitude]`.
- **Manage custom map panes & z-indexes** to keep interactive labels and vector overlays correctly ordered.

<br>

## How skills, plugins, and agents fit together

1. **The skill** is the portable content: a `SKILL.md` plus a `references/` folder. This is what every AI agent reads.
2. **The plugin** is a Claude Code–specific wrapper for distributing the skill through a marketplace.
3. **The agent** (Claude Code, Gemini CLI, Cursor, Antigravity, Windsurf…) loads the skill from its designated skills directory.

<br>

## 📦 Installation

### Universal — via Skills CLI

Works with Claude Code, Cursor, Gemini CLI, Windsurf, and dozens of other agents. The [Skills CLI](https://github.com/vercel-labs/skills) auto-detects which agents you have installed:

```bash
npx skills add mapsnippets/leaflet-skill
```

### Claude Code — as a plugin

Add the marketplace, install the plugin, then reload:

```bash
/plugin marketplace add mapsnippets/leaflet-skill
/plugin install leaflet-skill@leaflet-skill
/reload-plugins
```

### Gemini CLI & Antigravity

Install directly from the repository:

#### Windows (PowerShell)
```powershell
git clone https://github.com/mapsnippets/leaflet-skill.git; mkdir "$HOME\.gemini\skills\leaflet" -Force; cp -Recurse leaflet-skill\skills\* "$HOME\.gemini\skills\leaflet\"; rm -Recurse -Force leaflet-skill
```

#### Linux & macOS (bash)
```bash
git clone https://github.com/mapsnippets/leaflet-skill.git && mkdir -p ~/.gemini/skills/leaflet && cp -r leaflet-skill/skills/* ~/.gemini/skills/leaflet/ && rm -rf leaflet-skill
```

### Cursor

Project-scoped. Copy the skill folder into your project's skills directory:

```bash
mkdir -p .cursor/skills && cp -r skills/leaflet .cursor/skills/
```

### Windsurf

Project-scoped, read by Cascade:

```bash
mkdir -p .windsurf/skills && cp -r skills/leaflet .windsurf/skills/
```

---

<br>

## 📘 Repository layout

```text
.claude-plugin/
  marketplace.json    — Claude Code marketplace manifest
  plugin.json         — Claude Code plugin manifest
skills/
  leaflet/
    SKILL.md          — Main skill prompt entry point
    references/       — Deep technical reference guides (loaded on demand)
README.md             — This guide
LICENSE.md            — MIT License
```

<br>

## 🗺️ Quickstart Examples

### Vector Tiles in Leaflet (Recommended):
```javascript
import L from "leaflet";
import "@maplibre/maplibre-gl-leaflet";
import "leaflet/dist/leaflet.css";

const map = L.map("map").setView([50.0755, 14.4378], 13); // [lat, lng]

L.maplibreGL({
  style: "https://api.maptiler.com/maps/streets-v2/style.json?key=YOUR_API_KEY"
}).addTo(map);
```

### High-DPI Raster Tiles:
```javascript
L.tileLayer("https://api.maptiler.com/maps/streets-v2/{z}/{x}/{y}.png?key=YOUR_API_KEY", {
  tileSize: 512,
  zoomOffset: -1,
  minZoom: 1,
  attribution: "\u003ca href=\"https://www.openstreetmap.org/copyright\" target=\"_blank\"\u003e&copy; OpenStreetMap contributors\u003c/a\u003e",
  crossOrigin: true
}).addTo(map);
```

---

<br>

## Links

- 🌐 [MapSnippets Community](https://mapsnippets.com/)
- 🍃 [Leaflet Documentation](https://leafletjs.com/reference.html)
- 🐙 [GitHub Repository](https://github.com/mapsnippets/leaflet-skill)

---

<br>

## 🤝 Contributing

Contributions are welcome! Feel free to open issues or submit pull requests with improved snippets and documentation.

<br>

## 📄 License

This project is licensed under the MIT License — see the [LICENSE](./LICENSE.md) file for details.

<br>

<p align="center">
  Maintained by <a href="https://mapsnippets.com/">MapSnippets</a> — Open web mapping tools & snippets.
</p>
