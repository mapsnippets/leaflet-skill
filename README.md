# Leaflet — Agent Skill 🍃🤖

> Official **Leaflet** skill for AI coding assistants (Cursor, Claude Code, Antigravity, GitHub Copilot, Windsurf, Cline).

Maintained by **[MapSnippets](https://mapsnippets.org/)** — Open-source geospatial snippets, guides, and agent tools.

---

🌐 [Website](https://mapsnippets.org/) &nbsp; 📚 [Leaflet Documentation](https://leafletjs.com/reference.html)

---

<br>

<details>
<summary><b>Table of Contents</b></summary>
<ul>
<li><a href="#what-it-does">What it does</a></li>
<li><a href="#getting-started-cdn--npm">Getting Started (CDN & NPM)</a></li>
<li><a href="#how-skills-plugins-and-agents-fit-together">How skills, plugins, and agents fit together</a></li>
<li><a href="#-installation">Installation</a></li>
<li><a href="#-repository-layout">Repository layout</a></li>
<li><a href="#-quickstart-examples">Quickstart Examples</a></li>
<li><a href="#-basemap-api-keys">Basemap API Keys</a></li>
<li><a href="#links">Links</a></li>
<li><a href="#-contributing">Contributing</a></li>
<li><a href="#-license">License</a></li>
</ul>
</details>

<br>


## 🚀 Getting Started (CDN & NPM)

### Option 1: Modern NPM / Bundler (Vite, Webpack, Next.js)
```bash
npm install leaflet
npm install -D @types/leaflet
```
```javascript
import L from "leaflet";
import "leaflet/dist/leaflet.css";
```

### Option 2: Vanilla HTML (Official CDN with SRI)
```html
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" integrity="sha256-p4NxAoJBhIIN+hmNHrzRCf9tD/miZyoHS5obTRR9BMY=" crossorigin=""/>
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js" integrity="sha256-20nQCchB9co0qIjJZRGuk2/Z9VM+kNiyxNV1lvTlZBo=" crossorigin=""></script>
```

## What it does

A skill is on-demand expertise: the agent loads it only when your request matches the skill's description, then follows its instructions instead of guessing. When you ask for Leaflet maps, vector tiles, marker clustering, custom panes, or GeoJSON layers, this skill makes the agent:

- **Generate pure native Leaflet code** (v1.9+) with proper container lifecycle, touch event handling, and `invalidateSize` resize logic.
- **Render modern vector tiles in Leaflet** using standard plugins like `@maplibre/maplibre-gl-leaflet` (`L.maplibreGL`) with MapTiler `streets-v4` styles for sharp zooming and vector rendering.
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
git clone https://github.com/mapsnippets/leaflet-skill.git; mkdir "$HOME\.gemini\skills" -Force; cp -Recurse leaflet-skill\skills\leaflet "$HOME\.gemini\skills\"; rm -Recurse -Force leaflet-skill
```

#### Linux & macOS (bash)
```bash
git clone https://github.com/mapsnippets/leaflet-skill.git && mkdir -p ~/.gemini/skills && cp -r leaflet-skill/skills/leaflet ~/.gemini/skills/ && rm -rf leaflet-skill
```

### Cursor

Project-scoped. Copy the skill folder into your project's skills directory:

```bash
mkdir -p .cursor/skills && cp -r skills/leaflet .cursor/skills/
```

### VS Code & GitHub Copilot

Project-scoped. Place the skill folder into `.agents/skills/`:

```bash
mkdir -p .agents/skills && cp -r skills/leaflet .agents/skills/
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
    SKILL.md          — Main skill prompt entry point & progressive disclosure router
    evals/            — Standard benchmark evaluation suites (agentskills.io spec)
    examples/         — 28 standalone runnable task examples (HTML/CSS/JS)
    references/       — 19 deep technical reference guides & API specifications
README.md             — Documentation & installation guide
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
  style: "https://api.maptiler.com/maps/streets-v4/style.json?key=YOUR_API_KEY"
}).addTo(map);
```

### High-DPI Raster Tiles:
```javascript
L.tileLayer("https://api.maptiler.com/maps/streets-v4/{z}/{x}/{y}.png?key=YOUR_API_KEY", {
  tileSize: 512,
  zoomOffset: -1,
  minZoom: 1,
  attribution: "\u003ca href=\"https://www.openstreetmap.org/copyright\" target=\"_blank\"\u003e&copy; OpenStreetMap contributors\u003c/a\u003e",
  crossOrigin: true
}).addTo(map);
```

<br>

## 🔑 Basemap API Keys

The examples in this skill utilize MapTiler vector and raster tile endpoints. To run the examples with live map tiles:
- Follow the guide on [how to get a free MapTiler API Key](https://docs.maptiler.com/cloud/api/authentication-key/) (includes a free plan with 100,000 monthly tile requests).
- Replace `YOUR_API_KEY` in the snippet with your key.

---

<br>

## Links

- 🌐 [MapSnippets Community](https://mapsnippets.org/)
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
  Maintained by <a href="https://mapsnippets.org/">MapSnippets</a> — Open web mapping tools & snippets.
</p>
