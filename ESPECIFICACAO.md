# Especificacao Tecnica Minima -- Respira Boa Vista

**Versao:** 1.0
**Data:** 2026-03-14
**Projeto:** Respira Boa Vista -- Monitoramento de Qualidade do Ar

---

## 1. Visao Geral do Sistema

O sistema Respira Boa Vista e composto por tres camadas:

1. **Camada de Coleta** -- Sensor IoT PurpleAir coletando dados atmosfericos continuamente
2. **Camada de Dados** -- Pipeline de processamento e armazenamento em CSV
3. **Camada de Apresentacao** -- Aplicacao web estatica para visualizacao comunitaria

```
┌─────────────┐    ┌──────────────┐    ┌──────────────┐    ┌──────────────┐
│  PurpleAir  │───>│  Download    │───>│ Processamento│───>│  Frontend    │
│  Sensor     │    │  API CSV     │    │  Python       │    │  HTML/JS     │
│  (PA-II-SD) │    │  (raw/)      │    │  (processed/) │    │  (web/)      │
└─────────────┘    └──────────────┘    └──────────────┘    └──────────────┘
```

---

## 2. Requisitos Funcionais

### RF-01: Coleta de Dados
- O sistema DEVE coletar dados do sensor PurpleAir PA-II-SD (ID 56843) via API REST
- Os dados DEVEM ser coletados em intervalos de 30 minutos
- Os campos coletados DEVEM incluir: PM2.5, PM10, temperatura, umidade
- O download DEVE armazenar os dados brutos em formato CSV no diretorio `dataset/raw/`
- O sistema DEVE registrar logs de progresso da coleta em `Progress_Logs.log`

### RF-02: Processamento de Dados
- O sistema DEVE converter temperatura de Fahrenheit para Celsius
- O sistema DEVE calcular o AQI (Indice de Qualidade do Ar) usando a formula US EPA para PM2.5
- O sistema DEVE classificar cada medicao em categorias de AQI (Boa, Moderada, Prejudicial, etc.)
- O sistema DEVE gerar tres niveis de agregacao temporal:
  - **Completo**: resolucao de 30 minutos (todos os registros)
  - **Diario**: media, minimo e maximo por dia
  - **Mensal**: media, minimo, maximo e desvio padrao por mes
- Os dados processados DEVEM ser salvos em CSV no diretorio `dataset/processed/`

### RF-03: Calculo do AQI
O calculo do AQI DEVE seguir a tabela de breakpoints US EPA para PM2.5:

| PM2.5 (ug/m3) | AQI   | Categoria                       |
|----------------|-------|---------------------------------|
| 0 -- 12,0      | 0-50  | Boa                             |
| 12,1 -- 35,4   | 51-100| Moderada                        |
| 35,5 -- 55,4   | 101-150| Prejudicial Grupos Sensiveis   |
| 55,5 -- 150,4  | 151-200| Prejudicial                    |
| 150,5 -- 250,4 | 201-300| Muito Prejudicial              |
| 250,5+         | 301+  | Perigosa                        |

Formula: `AQI = ((IHI - ILO) / (BHI - BLO)) * (Cp - BLO) + ILO`

Onde:
- `Cp` = concentracao PM2.5
- `BLO/BHI` = breakpoints inferior/superior da concentracao
- `ILO/IHI` = breakpoints inferior/superior do AQI

### RF-04: Portal Principal (index.html)
- DEVE exibir a qualidade do ar atual com valor AQI e cor correspondente
- DEVE exibir valores de PM2.5, PM10, temperatura e umidade
- DEVE apresentar recomendacoes de saude baseadas na categoria AQI atual
- DEVE conter navegacao para as paginas de mapa e dashboard
- DEVE ser responsivo (mobile, tablet, desktop)

### RF-05: Dashboard de Dados (dashboard.html)
- DEVE exibir graficos interativos de series temporais usando D3.js v7
- DEVE conter cards de estatisticas: PM2.5 medio, PM2.5 maximo, AQI medio, total de registros
- DEVE permitir filtragem por: ano, mes e dia
- DEVE atualizar graficos e estatisticas dinamicamente ao alterar filtros
- Os dados DEVEM ser carregados via `d3.csv()` a partir de arquivo CSV

### RF-06: Mapa Interativo (mapa.html)
- DEVE exibir mapa interativo usando Leaflet v1.9.4 com tiles OpenStreetMap
- DEVE exibir marcador na localizacao do sensor (2,828993 N, 60,662812 W)
- DEVE conter painel de filtros com selecao de ano e mes
- DEVE exibir painel lateral com estatisticas do periodo selecionado
- DEVE incluir legenda com escala de cores AQI

---

## 3. Requisitos Nao-Funcionais

### RNF-01: Desempenho
- A aplicacao web DEVE carregar em menos de 5 segundos em conexoes 3G
- Os filtros DEVEM atualizar a visualizacao em menos de 1 segundo

### RNF-02: Compatibilidade
- DEVE funcionar nos navegadores: Chrome 90+, Firefox 90+, Safari 14+, Edge 90+
- DEVE ser responsivo com breakpoints em 768px (mobile) e 1024px (desktop)

### RNF-03: Acessibilidade
- A interface DEVE estar em portugues brasileiro (pt-BR)
- O sistema de cores AQI DEVE seguir o padrao internacional (EPA)
- DEVE funcionar sem autenticacao (acesso publico)

### RNF-04: Dados
- Os dados DEVEM ser disponibilizados em formato CSV (UTF-8)
- Os metadados DEVEM seguir formato JSON estruturado
- Os timestamps DEVEM estar em UTC (ISO 8601)
- A taxa de cobertura DEVE ser superior a 95%

### RNF-05: Portabilidade
- A aplicacao web DEVE ser estatica (HTML/CSS/JS puro), sem dependencia de backend
- NAO DEVE requerer banco de dados, servidor de aplicacao ou build tools
- DEVE ser possivel executar abrindo os arquivos HTML diretamente no navegador

### RNF-06: Licenciamento
- O codigo DEVE ser licenciado sob MIT
- Os dados DEVEM ser licenciados sob CC BY 4.0

---

## 4. Requisitos de Infraestrutura

### Hardware (Sensor)
| Item | Especificacao |
|------|---------------|
| Sensor | PurpleAir PA-II-SD |
| Detector | Plantower PMS5003 (laser scattering, canais A e B) |
| Alimentacao | 5V USB |
| Conectividade | Wi-Fi 802.11 b/g/n |
| Instalacao | Externa, protegida de chuva direta |

### Software (Processamento)
| Item | Especificacao |
|------|---------------|
| Linguagem | Python 3.8+ |
| Bibliotecas | requests, pandas, csv (stdlib) |
| API | PurpleAir API v1 (REST, JSON) |

### Software (Frontend)
| Item | Especificacao |
|------|---------------|
| Bibliotecas externas | D3.js v7, Leaflet v1.9.4, Phosphor Icons |
| Hospedagem | Qualquer servidor HTTP estatico (ou GitHub Pages) |
| Servidor minimo | Nao requer -- funciona como arquivos locais |

---

## 5. Formato dos Dados

### 5.1 Dataset Completo (complete.csv)

```csv
timestamp_utc,pm25_ugm3,pm10_ugm3,temperature_celsius,humidity_percent,aqi,aqi_category,date,month,year
2024-01-01T00:00:00Z,3.2,4.1,28.5,65.0,14,Boa,2024-01-01,1,2024
```

| Campo | Tipo | Obrigatorio | Descricao |
|-------|------|-------------|-----------|
| timestamp_utc | datetime (ISO 8601) | Sim | Data/hora UTC da medicao |
| pm25_ugm3 | float | Sim | PM2.5 em ug/m3 |
| pm10_ugm3 | float | Sim | PM10 em ug/m3 |
| temperature_celsius | float | Sim | Temperatura em Celsius |
| humidity_percent | float | Sim | Umidade relativa (%) |
| aqi | integer | Sim | Indice de Qualidade do Ar |
| aqi_category | string | Sim | Categoria AQI |
| date | date (YYYY-MM-DD) | Sim | Data da medicao |
| month | integer (1-12) | Sim | Mes |
| year | integer | Sim | Ano |

### 5.2 Dataset Diario (daily.csv)

| Campo | Tipo | Descricao |
|-------|------|-----------|
| time_stamp | date | Data do dia |
| pm2.5_atm_mean / _min / _max | float | PM2.5 agregado |
| pm10.0_atm_mean / _min / _max | float | PM10 agregado |
| temp_celsius_mean / _min / _max | float | Temperatura agregada |
| humidity_mean / _min / _max | float | Umidade agregada |

### 5.3 Dataset Mensal (monthly.csv)

| Campo | Tipo | Descricao |
|-------|------|-----------|
| month_year | string (MM/YYYY) | Mes/Ano |
| pm2.5_atm_mean / _min / _max / _std | float | PM2.5 com desvio padrao |
| pm10.0_atm_mean / _min / _max / _std | float | PM10 com desvio padrao |
| temp_celsius_mean / _min / _max | float | Temperatura agregada |
| humidity_mean / _min / _max | float | Umidade agregada |

---

## 6. Arquitetura do Frontend

### 6.1 Diagrama de Componentes

```
┌─────────────────────────────────────────────────────────┐
│                    Aplicacao Web                        │
│                                                         │
│  ┌─────────────┐  ┌─────────────┐  ┌────────────────┐  │
│  │   Portal    │  │  Dashboard  │  │  Mapa          │  │
│  │ index.html  │  │  D3.js      │  │  Leaflet       │  │
│  │             │  │             │  │                │  │
│  │ - AQI atual │  │ - Graficos  │  │ - Mapa OSM    │  │
│  │ - Metricas  │  │ - Cards     │  │ - Marcadores  │  │
│  │ - Saude     │  │ - Filtros   │  │ - Filtros     │  │
│  │ - Navegacao │  │   (A/M/D)   │  │ - Estatistic. │  │
│  └─────────────┘  └─────────────┘  └────────────────┘  │
│                                                         │
│  ┌──────────────────────────────────────────────────┐   │
│  │              Dados (CSV via d3.csv())             │   │
│  │  - Arquivo CSV carregado via fetch/d3.csv()      │   │
│  │  - Agregado a medias diarias na inicializacao    │   │
│  └──────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────┘
```

### 6.2 Navegacao

```
Portal (index.html)
  ├── Dashboard (dashboard.html)
  └── Mapa (mapa.html)
```

### 6.3 Design Responsivo

| Breakpoint | Layout |
|------------|--------|
| < 768px | Coluna unica, menu hamburger, controles empilhados |
| >= 768px | Multi-colunas, navegacao horizontal, paineis laterais |

---

## 7. Validacao e Qualidade dos Dados

### Criterios de Aceitacao do Dataset

| Criterio | Limiar |
|----------|--------|
| Cobertura temporal | >= 95% dos intervalos de 30 min |
| Dados faltantes PM2.5 | <= 5% |
| Dados faltantes PM10 | <= 5% |
| Faixa PM2.5 valida | 0 -- 1000 ug/m3 |
| Faixa PM10 valida | 0 -- 1000 ug/m3 |
| Faixa temperatura | -10 -- 60 Celsius |
| Faixa umidade | 0 -- 100% |
| AQI calculado corretamente | 100% das linhas |

### Verificacao de Integridade

- O numero de registros DEVE corresponder ao informado nos metadados
- As datas DEVEM estar em ordem cronologica crescente
- NAO DEVE haver registros duplicados (mesmo timestamp)
- Todos os campos obrigatorios DEVEM estar preenchidos

---

## 8. Versionamento

### Dataset
| Versao | Periodo | Registros | Data de Lancamento |
|--------|---------|-----------|-------------------|
| v1.0 | Jan/2024 -- Jan/2025 | 17.867 | 2026-02-10 |
| v2.0 | Jan/2024 -- Dez/2025 | 34.791 | 2026-02-04 |

### Aplicacao Web
| Versao | Descricao |
|--------|-----------|
| v1 | Portal basico, dashboard e mapa sem filtros avancados |
| v2 (atual) | Dashboard e mapa com filtros por ano/mes/dia, design responsivo aprimorado |

---

## 9. Glossario

| Termo | Definicao |
|-------|-----------|
| **AQI** | Air Quality Index -- Indice de Qualidade do Ar, padrao US EPA |
| **PM2.5** | Material particulado com diametro < 2,5 micrometros |
| **PM10** | Material particulado com diametro < 10 micrometros |
| **PurpleAir** | Fabricante de sensores de baixo custo para qualidade do ar |
| **PA-II-SD** | Modelo do sensor PurpleAir com cartao SD para armazenamento local |
| **PMS5003** | Detector Plantower baseado em espalhamento laser |
| **OMS** | Organizacao Mundial da Saude (diretriz PM2.5: 5 ug/m3 media anual) |
| **US EPA** | Agencia de Protecao Ambiental dos EUA |
| **Koppen Aw** | Classificacao climatica: savana tropical com estacao seca |
| **D3.js** | Biblioteca JavaScript para visualizacao de dados orientada a documentos |
| **Leaflet** | Biblioteca JavaScript de codigo aberto para mapas interativos |
