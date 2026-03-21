# Tecnologias Usadas no Projeto Respira Boa Vista

## Hardware

- **PurpleAir PA-II-SD** (Sensor ID: 56843) — sensor IoT de qualidade do ar
- **Plantower PMS5003** — detector laser de partículas (dentro do PA-II-SD)

## Coleta de Dados

- **PurpleAir API v1** (REST) — download dos dados brutos do sensor

## Processamento de Dados (Python)

- **Python 3.8+** — usado para processar os dados brutos offline (scripts não incluídos no repositório)
  - **pandas** — manipulação e agregação dos DataFrames (conversão F→C, cálculo AQI, agregação temporal 30min → diário → mensal)
  - **requests** — chamadas HTTP à API REST do PurpleAir para download dos CSVs
  - **csv** (stdlib) — leitura/escrita de arquivos CSV
  - **Evidência**: arquivo `Progress_Logs.log` registra 9 requisições à API (34s total), e os 3 CSVs processados (complete, daily, monthly) foram gerados por esses scripts

## Frontend

- **HTML5** — estrutura das 3 páginas (`index.html`, `dashboard.html`, `mapa.html`)
- **CSS3** — layout responsivo, CSS Variables, animações (`@keyframes`), `backdrop-filter`, media queries (768px, 900px)
- **JavaScript (ES6+ vanilla)** — lógica client-side, async/await, template literals, arrow functions, sem frameworks

## Bibliotecas JavaScript (via CDN)

- **D3.js v7** — carregamento de CSV (`d3.csv()`), agregação (`d3.group()`), gráficos de série temporal (PM2.5, IQA, Temperatura/Umidade)
- **Leaflet v1.9.4** — mapa interativo com marcador pulsante, popup, círculo de raio e legenda AQI
- **Phosphor Icons** — ícones da interface (navbar, cards, filtros)
- **Google Fonts (Inter)** — tipografia (pesos 300, 400, 600, 700)

## Serviços Externos

- **OpenStreetMap** — tiles do mapa base (via Leaflet)
- **Google Fonts API** — servir a fonte Inter

## Armazenamento de Dados

- **CSV** — formato principal de séries temporais (34.791 registros no complete, 730 no daily, 24 no monthly)
- **JSON** — metadados do dataset e configuração do sensor (`dataset_metadata_v2.json`, `sensors.json`)
- **GeoJSON** — dados geoespaciais do sensor com localização, metadados e estatísticas (`sensor_qualidade_ar_boavista.geojson`) — RFC 7946, CRS WGS 84

## Padrões e Referências

- **US EPA AQI** — fórmula de cálculo do Índice de Qualidade do Ar
- **OMS (WHO)** — limites de referência PM2.5 (15 µg/m³)
- **ISO 8601** — formato de timestamps (UTC)
- **UTF-8** — encoding dos arquivos

## Licenciamento

- **MIT** — código-fonte
- **CC BY 4.0** — dataset

## Infraestrutura

- **GitHub** — repositório e versionamento
- **GitHub Pages** — hospedagem da aplicação via GitHub Actions (deploy automático)
- **GitHub Actions** — workflow de deploy (`.github/workflows/deploy-pages.yml`)
- **Servidor HTTP local** (`python3 -m http.server`) — para desenvolvimento local (necessário por CORS)
- **Sem build tools** — sem webpack, Vite, npm dependencies — páginas estáticas puras
