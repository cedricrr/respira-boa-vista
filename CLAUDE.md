# CLAUDE.md

## Project Overview

Respira Boa Vista is a TCC (undergraduate thesis) project for air quality monitoring in Boa Vista/RR, Brazil, using IoT PurpleAir sensor data. It's a static frontend application that visualizes real environmental data collected from a PurpleAir PA-II-SD sensor (ID: 56843).

## Project Structure

```
respira-boa-vista/
├── web/                   # Frontend source (3 HTML pages)
│   ├── index.html         # Home/Landing — hero AQI card + overview stats
│   ├── dashboard.html     # Dashboard — 3 D3.js charts + filters (year/month/day)
│   └── mapa.html          # Map — Leaflet interactive map + sidebar stats
├── docs/                  # GitHub Pages deployment (copy of web/ + dataset)
│   ├── index.html         # Deployed home page
│   ├── dashboard.html     # Deployed dashboard
│   ├── mapa.html          # Deployed map
│   ├── tecnologias.md     # Technology stack listing
│   └── dataset/processed/ # CSV copy for GitHub Pages access
├── dataset/
│   ├── raw/               # Raw CSV from PurpleAir API
│   └── processed/         # Processed CSVs (complete, daily, monthly) + metadata JSON
├── .github/workflows/     # CI/CD
│   └── deploy-pages.yml   # GitHub Pages deploy via Actions
├── ESPECIFICACAO.md       # Technical specifications
├── README.md              # Project README
├── CITATION.cff           # Citation metadata
├── .nojekyll              # Skip Jekyll processing on GitHub Pages
└── LICENSE                # MIT (code) + CC BY 4.0 (data)
```

## Tech Stack

- **Frontend**: HTML5, CSS3, vanilla JavaScript (ES6+) — no build tools or frameworks
- **Visualization**: D3.js v7 (charts), Leaflet v1.9.4 (map), OpenStreetMap (tiles)
- **UI**: Google Fonts (Inter), Phosphor Icons, CSS Variables
- **Data**: CSV files loaded client-side via `d3.csv()`, aggregated to daily averages with `d3.group()`
- **Data source**: PurpleAir API v1 (sensor 56843, 30-min intervals, 34,791 records)

## Development

### Running locally

```bash
python3 -m http.server 8000
# Open http://localhost:8000/web/index.html
```

A local HTTP server is required due to CORS restrictions on `d3.csv()` file loading.

### Data path

- **`web/` pages** load data using dynamic `basePath`:
  ```js
  const basePath = location.hostname.includes('github.io') ? '/respira-boa-vista' : '';
  d3.csv(basePath + '/dataset/processed/dataset_qualidade_ar_boavista_2024-2025_complete.csv')
  ```
- **`docs/` pages** (GitHub Pages) use relative path:
  ```js
  d3.csv('./dataset/processed/dataset_qualidade_ar_boavista_2024-2025_complete.csv')
  ```

### Deployment

- GitHub Pages serves from root (`path: '.'`) via GitHub Actions (`.github/workflows/deploy-pages.yml`)
- The `docs/` folder contains a self-contained copy of the app with its own `dataset/processed/` folder
- URL: `https://cedricrr.github.io/respira-boa-vista/docs/index.html`

### CSV columns

`timestamp_utc, pm25_ugm3, pm10_ugm3, temperature_celsius, humidity_percent, aqi, aqi_category, date, month, year`

### Daily aggregation format

The JS code aggregates raw 30-min data to daily using `d3.group()` into objects with keys:
- `d` — date string (YYYY-MM-DD)
- `p` — PM2.5 mean
- `px` — PM2.5 max
- `t` — PM10 mean
- `c` — temperature mean (Celsius)
- `h` — humidity mean (%)

## Conventions

- **Language**: Portuguese (pt-BR) for UI text and documentation
- **Commits**: [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/)
- **Design system**: Inter font, Phosphor Icons, CSS variables for AQI colors, sticky navbar with backdrop-blur
- **No build step**: Pure static HTML/CSS/JS — edit and reload

## Key Constants

- **Sensor coordinates**: 2.828993, -60.662812 (Boa Vista, Roraima)
- **WHO PM2.5 limit**: 15 µg/m³
- **EPA PM2.5 limit**: 35.4 µg/m³
- **AQI "Good" threshold**: 50
