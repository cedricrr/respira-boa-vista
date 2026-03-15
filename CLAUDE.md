# CLAUDE.md

## Project Overview

Respira Boa Vista is a TCC (undergraduate thesis) project for air quality monitoring in Boa Vista/RR, Brazil, using IoT PurpleAir sensor data. It's a static frontend application that visualizes real environmental data collected from a PurpleAir PA-II-SD sensor (ID: 56843).

## Project Structure

```
respira-boa-vista/
├── web/                   # Frontend application (3 HTML pages)
│   ├── index.html         # Home/Landing — hero AQI card + overview stats
│   ├── dashboard.html     # Dashboard — 3 D3.js charts + filters (year/month/day)
│   └── mapa.html          # Map — Leaflet interactive map + sidebar stats
├── dataset/
│   ├── raw/               # Raw CSV from PurpleAir API
│   └── processed/         # Processed CSVs (complete, daily, monthly) + metadata JSON
├── docs/                  # Project documentation
│   └── tecnologias.md     # Technology stack listing
├── ESPECIFICACAO.md       # Technical specifications
├── README.md              # Project README
├── CITATION.cff           # Citation metadata
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

All 3 HTML pages load data from:
```
../dataset/processed/dataset_qualidade_ar_boavista_2024-2025_complete.csv
```
This relative path works when served from the project root.

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
