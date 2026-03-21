# CLAUDE.md

## Project Overview

Respira Boa Vista is a TCC (undergraduate thesis) project for air quality monitoring in Boa Vista/RR, Brazil, using IoT PurpleAir sensor data. It's a static frontend application that visualizes real environmental data collected from a PurpleAir PA-II-SD sensor (ID: 56843).

## Project Structure

```
respira-boa-vista/
├── web/                   # Frontend source (3 HTML pages)
│   ├── index.html         # Home/Landing — hero SVG illustration + AQI card (with last reading date) + overview stats
│   ├── dashboard.html     # Dashboard — 3 D3.js charts (IQA, PM2.5, Temp/Humidity) + filters (year/month/day)
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
- **Data formats**: CSV (time series), JSON (metadata), GeoJSON (geospatial sensor data — RFC 7946)
- **Data loading**: CSV files loaded client-side via `d3.csv()`, aggregated to daily averages with `d3.group()`
- **Data source**: PurpleAir API v1 (sensor 56843, 30-min intervals, 34,791 records)
- **Data processing (offline)**: Python 3.8+ with `requests` (API download), `pandas` (DataFrame manipulation, F→C conversion, AQI calculation, temporal aggregation), `csv` (stdlib). Scripts not included in the repo — only the resulting CSVs are committed.

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
- URL: `https://cedricrr.github.io/respira-boa-vista/index.html`

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

---

# CLAUDE.md (Portugues)

## Visao Geral do Projeto

Respira Boa Vista e um projeto de TCC (Trabalho de Conclusao de Curso) para monitoramento da qualidade do ar em Boa Vista/RR, Brasil, utilizando dados do sensor IoT PurpleAir. E uma aplicacao frontend estatica que visualiza dados ambientais reais coletados de um sensor PurpleAir PA-II-SD (ID: 56843).

## Estrutura do Projeto

```
respira-boa-vista/
├── web/                   # Codigo-fonte do frontend (3 paginas HTML)
│   ├── index.html         # Inicio — ilustracao SVG hero + card AQI (com data da ultima leitura) + resumo estatistico
│   ├── dashboard.html     # Dashboard — 3 graficos D3.js (IQA, PM2.5, Temp/Umidade) + filtros (ano/mes/dia)
│   └── mapa.html          # Mapa — mapa interativo Leaflet + painel lateral de estatisticas
├── docs/                  # Deploy no GitHub Pages (copia de web/ + dataset)
│   ├── index.html         # Pagina inicial (deploy)
│   ├── dashboard.html     # Dashboard (deploy)
│   ├── mapa.html          # Mapa (deploy)
│   ├── tecnologias.md     # Lista de tecnologias utilizadas
│   └── dataset/processed/ # Copia do CSV para acesso no Pages
├── dataset/
│   ├── raw/               # CSV bruto da API PurpleAir
│   └── processed/         # CSVs processados (completo, diario, mensal) + metadados JSON
├── .github/workflows/     # CI/CD
│   └── deploy-pages.yml   # Deploy no GitHub Pages via Actions
├── ESPECIFICACAO.md       # Especificacao tecnica
├── README.md              # README do projeto
├── CITATION.cff           # Metadados de citacao
├── .nojekyll              # Desabilita processamento Jekyll no GitHub Pages
└── LICENSE                # MIT (codigo) + CC BY 4.0 (dados)
```

## Stack Tecnologica

- **Frontend**: HTML5, CSS3, JavaScript vanilla (ES6+) — sem build tools ou frameworks
- **Visualizacao**: D3.js v7 (graficos), Leaflet v1.9.4 (mapa), OpenStreetMap (tiles)
- **UI**: Google Fonts (Inter), Phosphor Icons, CSS Variables
- **Formatos de dados**: CSV (series temporais), JSON (metadados), GeoJSON (dados geoespaciais do sensor — RFC 7946)
- **Carregamento de dados**: Arquivos CSV carregados no cliente via `d3.csv()`, agregados em medias diarias com `d3.group()`
- **Fonte de dados**: API PurpleAir v1 (sensor 56843, intervalos de 30 min, 34.791 registros)
- **Processamento de dados (offline)**: Python 3.8+ com `requests` (download da API), `pandas` (manipulacao de DataFrames, conversao F para C, calculo AQI, agregacao temporal), `csv` (stdlib). Scripts nao incluidos no repositorio — apenas os CSVs resultantes sao commitados.

## Desenvolvimento

### Executar localmente

```bash
python3 -m http.server 8000
# Abra http://localhost:8000/web/index.html
```

Um servidor HTTP local e necessario devido a restricoes de CORS no carregamento de arquivos via `d3.csv()`.

### Caminho dos dados

- **Paginas `web/`** carregam dados usando `basePath` dinamico:
  ```js
  const basePath = location.hostname.includes('github.io') ? '/respira-boa-vista' : '';
  d3.csv(basePath + '/dataset/processed/dataset_qualidade_ar_boavista_2024-2025_complete.csv')
  ```
- **Paginas `docs/`** (GitHub Pages) usam caminho relativo:
  ```js
  d3.csv('./dataset/processed/dataset_qualidade_ar_boavista_2024-2025_complete.csv')
  ```

### Deploy

- GitHub Pages serve a partir da raiz (`path: '.'`) via GitHub Actions (`.github/workflows/deploy-pages.yml`)
- A pasta `docs/` contem uma copia autocontida da aplicacao com sua propria pasta `dataset/processed/`
- URL: `https://cedricrr.github.io/respira-boa-vista/index.html`

### Colunas do CSV

`timestamp_utc, pm25_ugm3, pm10_ugm3, temperature_celsius, humidity_percent, aqi, aqi_category, date, month, year`

### Formato de agregacao diaria

O codigo JS agrega os dados brutos de 30 min para diario usando `d3.group()` em objetos com as chaves:
- `d` — string de data (YYYY-MM-DD)
- `p` — media PM2.5
- `px` — maximo PM2.5
- `t` — media PM10
- `c` — media de temperatura (Celsius)
- `h` — media de umidade (%)

## Convencoes

- **Idioma**: Portugues (pt-BR) para textos da interface e documentacao
- **Commits**: [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/)
- **Design system**: Fonte Inter, Phosphor Icons, CSS variables para cores AQI, navbar sticky com backdrop-blur
- **Sem etapa de build**: HTML/CSS/JS puro — editar e recarregar

## Constantes Importantes

- **Coordenadas do sensor**: 2.828993, -60.662812 (Boa Vista, Roraima)
- **Limite OMS PM2.5**: 15 ug/m3
- **Limite EPA PM2.5**: 35.4 ug/m3
- **Limiar AQI "Boa"**: 50
