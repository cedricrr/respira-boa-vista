# Respira Boa Vista

**Dataset aberto e sistema de visualizacao comunitaria de qualidade do ar para Boa Vista/RR**

> Trabalho de Conclusao de Curso -- Especializacao em Computacao Aplicada na Industria 4.0
> *Expansao e Democratizacao do Monitoramento de Qualidade do Ar em Boa Vista/RR -- Dataset Aberto e Sistema de Visualizacao Comunitaria*
> Autor: Cedric Carol Patrician Williams Filho
> Instituicao: UFRR -- Universidade Federal de Roraima

---

## Sobre o Projeto

O **Respira Boa Vista** e um projeto de ciencia aberta que coleta, processa e disponibiliza dados de qualidade do ar da cidade de Boa Vista, Roraima, Brasil. O sistema utiliza sensores IoT de baixo custo (PurpleAir) para monitoramento continuo de material particulado (PM2.5 e PM10), temperatura e umidade, e oferece uma plataforma web interativa para consulta e visualizacao dos dados pela comunidade.

### Objetivos

- Disponibilizar dados abertos de qualidade do ar para pesquisadores, gestores publicos e cidadaos
- Fornecer ferramentas de visualizacao interativa (mapas e dashboards) acessiveis ao publico
- Monitorar padroes sazonais de poluicao atmosferica na regiao amazonica
- Detectar eventos extremos de poluicao e gerar alertas de saude publica
- Demonstrar praticas de ciencia aberta com divulgacao completa de dados e metodologia

---

## Estrutura do Repositorio

```
respira-boa-vista/
├── README.md                          # Documentacao principal do projeto
├── ESPECIFICACAO.md                   # Especificacao tecnica minima
├── CLAUDE.md                          # Contexto do projeto para IA
├── LICENSE                            # Licenca MIT (codigo)
├── CITATION.cff                       # Metadados de citacao (CFF v1.2.0)
├── .gitignore
├── .nojekyll                          # Desabilita Jekyll no GitHub Pages
│
├── .github/workflows/
│   └── deploy-pages.yml               # Deploy automatico no GitHub Pages
│
├── web/                               # Aplicacao web (codigo-fonte)
│   ├── index.html                     # Portal principal
│   ├── dashboard.html                 # Dashboard com graficos D3.js e filtros
│   └── mapa.html                      # Mapa interativo Leaflet com filtros
│
├── docs/                              # Versao deployada no GitHub Pages
│   ├── index.html                     # Portal principal (deploy)
│   ├── dashboard.html                 # Dashboard (deploy)
│   ├── mapa.html                      # Mapa interativo (deploy)
│   ├── tecnologias.md                 # Stack de tecnologias
│   └── dataset/processed/             # Copia do CSV para acesso no Pages
│
└── dataset/                           # Dados de qualidade do ar
    ├── README.md
    ├── raw/                           # Dados brutos do sensor
    │   ├── 56843 2024-01-01 2025-12-31 30-Minute Average.csv
    │   └── Progress_Logs.log
    ├── processed/                     # Dados processados
    │   ├── README_v2.md
    │   ├── dataset_metadata_v2.json
    │   ├── dataset_qualidade_ar_boavista_2024-2025_complete.csv
    │   ├── dataset_qualidade_ar_boavista_2024-2025_daily.csv
    │   └── dataset_qualidade_ar_boavista_2024-2025_monthly.csv
    └── metada/                        # Metadados do sensor
        └── sensors.json
```

---

## Dataset

Dados coletados via sensor [PurpleAir PA-II-SD](https://www.purpleair.com/) (Sensor ID: 56843), instalado em Boa Vista/RR, disponibilizados em formato aberto (CSV).

| Atributo             | Valor                                        |
|----------------------|----------------------------------------------|
| **Periodo**          | Janeiro/2024 a Dezembro/2025 (730 dias)      |
| **Resolucao**        | 30 minutos                                   |
| **Total de registros** | 34.791 medicoes                            |
| **Cobertura**        | 99,29%                                       |
| **Variaveis**        | PM2.5, PM10, temperatura, umidade, AQI       |
| **Licenca**          | CC BY 4.0                                    |

### Arquivos de Dados

| Arquivo | Descricao | Registros |
|---------|-----------|-----------|
| `dataset_qualidade_ar_boavista_2024-2025_complete.csv` | Dados completos, resolucao 30 min | 34.791 |
| `dataset_qualidade_ar_boavista_2024-2025_daily.csv` | Agregados diarios (media/min/max) | 730 |
| `dataset_qualidade_ar_boavista_2024-2025_monthly.csv` | Agregados mensais (media/min/max/desvio) | 24 |

### Colunas do Dataset Completo

| Coluna | Descricao | Unidade |
|--------|-----------|---------|
| `timestamp_utc` | Data/hora da medicao em UTC | ISO 8601 |
| `pm25_ugm3` | Material particulado < 2,5 micrometros | ug/m3 |
| `pm10_ugm3` | Material particulado < 10 micrometros | ug/m3 |
| `temperature_celsius` | Temperatura do ar | Celsius |
| `humidity_percent` | Umidade relativa do ar | % |
| `aqi` | Indice de Qualidade do Ar (padrao US EPA) | adimensional |
| `aqi_category` | Classificacao qualitativa do AQI | texto |
| `date` | Data da medicao | YYYY-MM-DD |
| `month` | Mes da medicao | inteiro |
| `year` | Ano da medicao | inteiro |

### Estatisticas Principais

| Metrica | 2024 | 2025 | Variacao |
|---------|------|------|----------|
| PM2.5 medio | 13,63 ug/m3 | 5,03 ug/m3 | -63,1% |
| PM2.5 mediano | 6,50 ug/m3 | 3,70 ug/m3 | -43,1% |
| PM2.5 maximo | 373,20 ug/m3 | 57,40 ug/m3 | -84,6% |
| Eventos extremos (PM2.5 > 100) | 246 | 0 | -100% |

Acesse a documentacao detalhada dos dados em [`/dataset/processed/README_v2.md`](./dataset/processed/README_v2.md).

---

## Visualizacao Web

Aplicacao HTML/CSS/JS estatica para consulta interativa dos dados, sem necessidade de backend ou banco de dados.

### Paginas

1. **Portal Principal** (`web/index.html`) -- Pagina inicial com qualidade do ar atual, AQI em tempo real, data da ultima leitura, ilustracao SVG tematica (skyline, particulas, sensor) e orientacoes de saude
2. **Dashboard** (`web/dashboard.html`) -- Graficos interativos com D3.js (IQA, PM2.5, Temperatura/Umidade), cards de estatisticas e filtros por ano/mes/dia
3. **Mapa Interativo** (`web/mapa.html`) -- Mapa Leaflet com localizacao do sensor, filtros temporais e painel de estatisticas

### Tecnologias

| Tecnologia | Versao | Uso |
|------------|--------|-----|
| HTML5 / CSS3 | -- | Estrutura e estilizacao |
| JavaScript (vanilla) | ES6+ | Logica de interacao e filtros |
| D3.js | v7 | Graficos e visualizacoes de dados |
| Leaflet | v1.9.4 | Mapas interativos |
| OpenStreetMap | -- | Tiles do mapa |
| Phosphor Icons | web | Iconografia |

### Escala AQI (Cores)

| Categoria | Faixa AQI | Cor |
|-----------|-----------|-----|
| Boa | 0-50 | Verde (#00e400) |
| Moderada | 51-100 | Amarelo (#ffff00) |
| Prejudicial Grupos Sensiveis | 101-150 | Laranja (#ff7e00) |
| Prejudicial | 151-200 | Vermelho (#ff0000) |
| Muito Prejudicial | 201-300 | Roxo (#8f3f97) |
| Perigosa | 300+ | Vermelho escuro (#7e0023) |

### Acesso Online

A aplicacao esta disponivel em: **https://cedricrr.github.io/respira-boa-vista/docs/index.html**

O deploy e feito automaticamente via GitHub Actions a cada push na branch `main`.

### Como Executar Localmente

A aplicacao web e composta apenas de arquivos estaticos. Um servidor HTTP local e necessario devido a restricoes CORS do `d3.csv()`:

```bash
python3 -m http.server 8000
# Acesse http://localhost:8000/web/index.html
```

---

## Sensor

| Atributo | Valor |
|----------|-------|
| **Modelo** | PurpleAir PA-II-SD |
| **Detector** | Plantower PMS5003 (laser scattering) |
| **Sensor ID** | 56843 |
| **Coordenadas** | 2,828993 N, 60,662812 W |
| **Elevacao** | ~85 metros |
| **Cidade** | Boa Vista, Roraima, Brasil |
| **Zona climatica** | Savana tropical (Aw - Koppen) |
| **Fuso horario** | America/Boa_Vista (UTC-4) |
| **Status** | Ativo |

[Ver sensor no mapa PurpleAir](https://map.purpleair.com/air-quality-standards-us-epa-aqi?opt=%2F1%2Flp%2Fa10%2Fp604800%2FcC0#18.19/2.828993/-60.662812)

---

## Pipeline de Dados

```
PurpleAir API  -->  Download CSV bruto  -->  Processamento  -->  Datasets CSV  -->  Frontend HTML
   (sensor)         (raw/)                   (Python)           (processed/)       (web/)
```

1. **Coleta**: API PurpleAir consulta sensor 56843 em intervalos de 3 meses
2. **Download**: 34.791 registros obtidos em 9 requisicoes (34 segundos)
3. **Processamento**: Conversao de temperatura (F para C), calculo do AQI (formula US EPA), agregacao temporal
4. **Publicacao**: Dados carregados via `d3.csv()` no frontend a partir de arquivos CSV

---

## Citacao

Se utilizar este dataset ou codigo, cite:

> WILLIAMS FILHO, Cedric Carol Patrician. **Expansao e Democratizacao do Monitoramento de Qualidade do Ar em Boa Vista/RR: Dataset Aberto e Sistema de Visualizacao Comunitaria.** 2026. Monografia (Especializacao em Computacao Aplicada) -- UFRR, Boa Vista, 2026.

Veja tambem o arquivo [`CITATION.cff`](./CITATION.cff) para citacao estruturada.

---

## Licencas

| Componente | Licenca |
|------------|---------|
| Codigo-fonte (web/) | MIT |
| Dataset (dataset/) | CC BY 4.0 |

---

## Contato

**Cedric Carol Patrician Williams Filho**
Email: cedricstm@gmail.com
Instituicao: UFRR -- Departamento de Ciencia da Computacao
Programa: Especializacao em Computacao Aplicada na Industria 4.0
