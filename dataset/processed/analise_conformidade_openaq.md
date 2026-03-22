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
| `value` | Implementado | Nao | Ausente — ha `summary: { min, median, avg, max }` | **Ausente** — valores individuais estao no CSV, nao no metadata |
| `unit` | Implementado | Sim | `sensors[].parameter.units` (ex: "ug/m3") | **Confirmado** |
| `date` | Implementado | Parcial | `datetimeFirst`, `datetimeLast` (UTC + local) | Presente como range temporal, nao como campo por registro |
| `sourceName` | Implementado | Nome diferente | `provider.name: "PurpleAir"` | Presente como `provider`, nao `sourceName` — **`sourceName` e OpenAQ v2, nao v3** |
| `sourceType` | Implementado | Ausente | Nao existe no JSON | **Nao implementado** — `isMonitor: false` e o mais proximo, mas nao equivale |
| `mobile` | Implementado | Sim | `isMobile: false` | **Confirmado** |
| `averagingPeriod` | Implementado | Ausente | Nao existe no JSON | **Nao implementado** — nao faz parte do endpoint locations do OpenAQ v3 |

### Resumo

| Veredicto | Qtd | Campos |
|-----------|-----|--------|
| Confirmado | 4 | `coordinates`, `parameter`, `unit`, `mobile` |
| Parcial/nome diferente | 3 | `location`, `date`, `sourceName` |
| Ausente ou nao implementado | 3 | `value`, `sourceType`, `averagingPeriod` |

---

## 2. Problema identificado: mistura de versoes OpenAQ v2 e v3

Os nomes de campo listados na tabela do TCC **nao correspondem ao padrao OpenAQ v3**. Varios sao da **v2**:

| Campo no texto (v2) | Equivalente real no OpenAQ v3 | Status no JSON |
|---------------------|-------------------------------|----------------|
| `sourceName` | `provider.name` | Presente como `provider` |
| `sourceType` | Sem equivalente direto (`isMonitor` e parcial) | Ausente |
| `averagingPeriod` | Sem equivalente no nivel de location | Ausente |
| `location` | Objeto raiz com `id`, `name`, `locality` | Estrutura diferente |
| `value` | Pertence ao endpoint measurements, nao locations | Ausente do metadata |

O JSON real (`dataset_metadata_openaq_v3.json`) segue a **estrutura** do OpenAQ v3 (objetos aninhados: `provider`, `sensors`, `coordinates`, `datetimeFirst/Last`), mas a **tabela do texto descreve campos da v2** com nomes planos (flat).

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
| `provider` | Fonte dos dados | `{ id: 1, name: "PurpleAir" }` |
| `owner` | Proprietario do sensor | `{ id: 1, name: "UFRR" }` |
| `isMobile` | Sensor fixo ou movel | `false` |
| `isMonitor` | Se e monitor de referencia | `false` |
| `instruments` | Modelo do instrumento | `[{ name: "PurpleAir PA-II-SD (Plantower PMS5003)" }]` |
| `sensors` | Lista de sensores com parametros | 4 sensores: PM2.5, PM10, temperature, humidity |
| `sensors[].parameter` | Poluente medido (id, nome, unidade) | Ex: `{ name: "pm25", units: "ug/m3" }` |
| `sensors[].summary` | Estatisticas resumidas | `{ min, median, avg, max }` |
| `datetimeFirst` / `datetimeLast` | Range temporal (UTC + local) | `2024-01-01` a `2025-12-30` |
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
| `isMobile` | Sensor fixo ou movel | Implementado |
| `isMonitor` | Se e monitor de referencia | Implementado |
| `instruments` | Modelo do instrumento (PA-II-SD) | Implementado |
| `licenses` | Licenciamento dos dados (CC BY 4.0) | Implementado |

> **Nota:** Os campos `value` (concentracao individual) e `averagingPeriod` pertencem ao nivel de medicoes (measurements endpoint) do OpenAQ, nao ao nivel de localizacao (locations endpoint). O JSON de metadados e uma descricao de localizacao, portanto esses campos nao se aplicam a este arquivo. Os valores individuais das medicoes estao nos arquivos CSV do dataset.

---

## 5. Conclusao

O arquivo `dataset_metadata_openaq_v3.json` esta **bem estruturado** e segue a arquitetura do OpenAQ v3 (locations endpoint) com 17 campos implementados corretamente. O problema esta na **tabela do TCC**, que lista nomes de campo do OpenAQ **v2** (formato flat/plano) ao inves dos nomes reais do v3 (formato aninhado/nested). A correcao necessaria e na documentacao textual, nao no arquivo JSON.
