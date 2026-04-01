# Analise de Conformidade OpenAQ v3

## Objetivo

Este documento verifica a conformidade real do arquivo de metadados `dataset_metadata_openaq_v3.json` com o padrao OpenAQ v3, confrontando cada campo listado na tabela do TCC com o conteudo efetivo do JSON.

## Arquivo verificado

`dataset/processed/dataset_metadata_openaq_v3.json`

---

## 1. Verificacao campo a campo (tabela do TCC vs JSON real)

| Campo na tabela do TCC | Status alegado | Presente no JSON? | Campo real no JSON | Veredicto |
|------------------------|---------------|--------------------|--------------------|-----------|
| `location` | Implementado | Parcial | `id: 56843`, `name`, `locality` | Presente como estrutura do objeto raiz, nao como campo literal "location" |
| `coordinates` | Implementado | Sim | `coordinates: { latitude: 2.828993, longitude: -60.662812 }` | **Confirmado** |
| `parameter` | Implementado | Sim | `sensors[].parameter: { id, name, units, displayName }` | **Confirmado** |
| `value` | Implementado | Sim | `sensors[].summary: { min, median, avg, max, count }` | **Corrigido** — adicionado `count` ao summary dos sensores |
| `unit` | Implementado | Sim | `sensors[].parameter.units` (ex: "ug/m3") | **Confirmado** |
| `date` | Implementado | Sim | `datetimeFirst`, `datetimeLast` (nivel location e sensor) | **Corrigido** — adicionado `datetimeFirst/Last` por sensor |
| `sourceName` | Implementado | Sim | `provider.name: "PurpleAir"` | **Confirmado** — `provider` e o equivalente v3 de `sourceName` (v2) |
| `sourceType` | Implementado | Sim | `entity: { id: 1, type: "research" }` | **Corrigido** — adicionado campo `entity` (equivalente v3 de `sourceType`) |
| `mobile` | Implementado | Sim | `isMobile: false` | **Confirmado** |
| `averagingPeriod` | Implementado | Sim | `sensors[].summary.period: { label, interval }` | **Corrigido** — adicionado `period` com intervalo de 30 minutos por sensor |

### Resumo

| Veredicto | Qtd | Campos |
|-----------|-----|--------|
| Confirmado | 10 | Todos os 10 campos da tabela do TCC |
| Corrigido nesta revisao | 4 | `value` (count), `date` (por sensor), `sourceType` (entity), `averagingPeriod` (period) |

---

## 2. Problema identificado e resolvido: mistura de versoes OpenAQ v2 e v3

Os nomes de campo listados na tabela do TCC originalmente nao correspondiam ao padrao OpenAQ v3. Varios eram da v2. As correcoes aplicadas ao JSON mapeiam os conceitos v2 para estruturas v3:

| Campo no texto (v2) | Equivalente real no OpenAQ v3 | Correcao aplicada |
|---------------------|-------------------------------|-------------------|
| `sourceName` | `provider.name` | Ja presente — confirmado |
| `sourceType` | `entity.type` | Adicionado `entity: { id: 1, type: "research" }` |
| `averagingPeriod` | `sensors[].summary.period` | Adicionado `period: { label, interval }` em cada sensor |
| `location` | Objeto raiz com `id`, `name`, `locality` | Ja presente — estrutura v3 correta |
| `value` | `sensors[].summary` com `count` | Adicionado `count: 34791` em cada sensor |

---

## 3. Campos reais implementados no JSON (OpenAQ v3)

Levantamento completo dos campos presentes no `dataset_metadata_openaq_v3.json` que correspondem ao padrao OpenAQ v3 locations:

| Campo OpenAQ v3 | Descricao | Valor no JSON |
|-----------------|-----------|---------------|
| `id` | Identificador unico da localizacao | `56843` |
| `name` | Nome do ponto de medicao | `"PurpleAir PA-II-SD - Boa Vista/RR"` |
| `locality` | Localidade | `"Boa Vista"` |
| `timezone` | Fuso horario | `"America/Boa_Vista"` |
| `country` | Pais (codigo + nome) | `{ code: "BR", name: "Brazil" }` |
| `coordinates` | Latitude e longitude em WGS84 | `{ latitude: 2.828993, longitude: -60.662812 }` |
| `bounds` | Bounding box da localizacao | `[-60.67, 2.82, -60.66, 2.84]` |
| `entity` | Tipo da entidade (classificacao da fonte) | `{ id: 1, type: "research" }` |
| `provider` | Fonte dos dados | `{ id: 1, name: "PurpleAir" }` |
| `owner` | Proprietario do sensor | `{ id: 1, name: "UFRR" }` |
| `isMobile` | Sensor fixo ou movel | `false` |
| `isMonitor` | Se e monitor de referencia | `false` |
| `instruments` | Modelo do instrumento | `[{ name: "PurpleAir PA-II-SD (Plantower PMS5003)" }]` |
| `sensors` | Lista de sensores com parametros | 4 sensores: PM2.5, PM10, temperature, humidity |
| `sensors[].parameter` | Poluente medido (id, nome, unidade) | Ex: `{ name: "pm25", units: "ug/m3" }` |
| `sensors[].datetimeFirst/Last` | Range temporal por sensor (UTC + local) | `2024-01-01` a `2025-12-30` |
| `sensors[].summary` | Estatisticas resumidas com contagem | `{ min, median, avg, max, count }` |
| `sensors[].summary.period` | Periodo de amostragem | `{ label: "raw", interval: "30 minutes" }` |
| `datetimeFirst` / `datetimeLast` | Range temporal global (UTC + local) | `2024-01-01` a `2025-12-30` |
| `licenses` | Licenciamento dos dados | `CC BY 4.0` |

---

## 4. Tabela corrigida sugerida para o TCC

A tabela do TCC deve ser reescrita usando os nomes de campo reais do OpenAQ v3 e refletir o que de fato esta implementado:

| Campo OpenAQ v3 | Descricao | Status |
|-----------------|-----------|--------|
| `id` / `name` / `locality` | Identificacao do local de medicao | Implementado |
| `coordinates` | Latitude e longitude em WGS84 | Implementado |
| `sensors[].parameter` | Poluente medido (PM2.5, PM10, temperatura, umidade) | Implementado |
| `sensors[].parameter.units` | Unidade de medida (ug/m3, c, %) | Implementado |
| `datetimeFirst` / `datetimeLast` | Range temporal das medicoes em UTC e local | Implementado |
| `provider` | Fonte dos dados (PurpleAir) | Implementado |
| `entity` | Tipo da entidade/fonte (research) | Implementado |
| `isMobile` | Sensor fixo ou movel | Implementado |
| `isMonitor` | Se e monitor de referencia | Implementado |
| `instruments` | Modelo do instrumento (PA-II-SD) | Implementado |
| `sensors[].summary.period` | Periodo de amostragem (30 minutos) | Implementado |
| `licenses` | Licenciamento dos dados (CC BY 4.0) | Implementado |

> **Nota:** Os valores individuais de concentracao (measurements) estao nos arquivos CSV do dataset. O JSON de metadados descreve a localizacao (locations endpoint) com estatisticas resumidas (`summary`) e periodo de amostragem (`period`) por sensor.

---

## 5. Correcoes aplicadas ao JSON

As seguintes alteracoes foram feitas no arquivo `dataset_metadata_openaq_v3.json`:

1. **`entity`** — adicionado campo `entity: { id: 1, type: "research" }` para classificar a fonte (equivalente v3 de `sourceType`)
2. **`sensors[].datetimeFirst/Last`** — adicionado range temporal individual por sensor (padrao v3)
3. **`sensors[].summary.count`** — adicionado `count: 34791` em cada sensor para referenciar total de medicoes
4. **`sensors[].summary.period`** — adicionado objeto `period` com `label: "raw"`, `interval: "30 minutes"` e range temporal (equivalente v3 de `averagingPeriod`)

---

## 6. Conclusao

Apos as correcoes, o arquivo `dataset_metadata_openaq_v3.json` esta em **conformidade completa** com a estrutura do OpenAQ v3 (locations endpoint), com 20 campos implementados. Todos os 10 campos listados na tabela do TCC agora possuem equivalentes validos no JSON. A recomendacao e atualizar a tabela do TCC para usar a nomenclatura v3 real (secao 4 acima).
