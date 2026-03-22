# Análise de Cobertura do Dataset — Respira Boa Vista

**Data da análise:** 2026-03-21
**Dataset:** `dataset_qualidade_ar_boavista_2024-2025_complete.csv`
**Período:** 01/01/2024 a 30/12/2025

---

## 1. Resumo da Verificação

Este relatório verifica as afirmações sobre cobertura do dataset feitas no texto do TCC, confrontando cada uma com os dados reais do CSV.

### Texto original analisado

> "O dataset apresentou alta taxa de cobertura: 99,3% dos registros esperados para o período de 730 dias (01/01/2024 a 30/12/2025) estão presentes. Os dados faltantes (0,7%) concentraram-se em três períodos: 12-14 de março de 2024 (interrupção por manutenção), 8-9 de agosto de 2024 (queda de energia) e 3 de dezembro de 2025 (falha de conectividade). Esta cobertura é superior aos 95% recomendados pela literatura para análises de séries temporais de qualidade do ar."

---

## 2. Verificação Ponto a Ponto

### 2.1 "99,3% de cobertura" — ✅ CONFIRMADO

| Métrica | Valor |
|---------|-------|
| Registros esperados (730 dias × 48/dia) | 35.040 |
| Registros presentes | 34.791 |
| Registros faltantes | 249 |
| Cobertura calculada | **99,29%** |

O valor 99,3% é arredondamento aceitável de 99,29%.

### 2.2 "730 dias (01/01/2024 a 30/12/2025)" — ✅ CONFIRMADO

| Métrica | Valor |
|---------|-------|
| Primeiro registro | `2024-01-01 00:00:00 UTC` |
| Último registro | `2025-12-30 23:30:00 UTC` |
| Dias com dados | 730 |
| Dias com cobertura completa (48 registros) | 698 (95,6%) |
| Dias com cobertura incompleta | 32 (4,4%) |

### 2.3 "34.791 registros" — ✅ CONFIRMADO

O CSV contém exatamente 34.791 linhas de dados (excluindo o cabeçalho).

### 2.4 "Três períodos de falha" — ❌ CONTRADITO PELOS DADOS

As três datas citadas no texto **possuem dados completos (48 registros/dia)**:

| Data citada no texto | Causa alegada | Registros reais | Status |
|----------------------|---------------|-----------------|--------|
| 2024-03-12 | Manutenção | **48** | ⚠️ DADOS PRESENTES |
| 2024-03-13 | Manutenção | **48** | ⚠️ DADOS PRESENTES |
| 2024-03-14 | Manutenção | **48** | ⚠️ DADOS PRESENTES |
| 2024-08-08 | Queda de energia | **48** | ⚠️ DADOS PRESENTES |
| 2024-08-09 | Queda de energia | **48** | ⚠️ DADOS PRESENTES |
| 2025-12-03 | Falha conectividade | **48** | ⚠️ DADOS PRESENTES |

**Conclusão:** As datas e causas citadas no texto não correspondem às lacunas reais do dataset.

### 2.5 "Superior aos 95% recomendados pela literatura" — ✅ SUSTENTÁVEL

O valor de 99,29% é de fato superior a 95%. A referência bibliográfica específica deve ser citada no texto.

---

## 3. Lacunas Reais Identificadas

A análise identificou **34 lacunas** (intervalos >30 min entre registros consecutivos), totalizando **249 registros faltantes** distribuídos em **32 dias incompletos**.

### 3.1 Tabela Completa de Lacunas

| # | Início da lacuna | Fim da lacuna | Duração (h) | Registros faltantes |
|---|------------------|---------------|-------------|---------------------|
| 1 | 2024-01-26 15:30 | 2024-01-26 19:00 | 3,5 | 6 |
| 2 | 2024-01-31 02:00 | 2024-02-01 18:30 | 40,5 | 80 |
| 3 | 2024-02-07 18:30 | 2024-02-08 00:00 | 5,5 | 10 |
| 4 | 2024-03-05 21:30 | 2024-03-05 23:00 | 1,5 | 2 |
| 5 | 2024-03-22 18:00 | 2024-03-22 19:00 | 1,0 | 1 |
| 6 | 2024-03-23 12:00 | 2024-03-23 16:30 | 4,5 | 8 |
| 7 | 2024-04-22 15:00 | 2024-04-22 16:00 | 1,0 | 1 |
| 8 | 2024-05-07 22:30 | 2024-05-07 23:30 | 1,0 | 1 |
| 9 | 2024-05-08 03:00 | 2024-05-08 04:30 | 1,5 | 2 |
| 10 | 2024-05-08 04:30 | 2024-05-08 06:00 | 1,5 | 2 |
| 11 | 2024-05-08 06:30 | 2024-05-08 07:30 | 1,0 | 1 |
| 12 | 2024-05-25 12:30 | 2024-05-25 13:30 | 1,0 | 1 |
| 13 | 2024-06-19 03:00 | 2024-06-20 19:30 | 40,5 | 80 |
| 14 | 2024-06-28 06:30 | 2024-06-28 14:00 | 7,5 | 14 |
| 15 | 2024-07-01 00:30 | 2024-07-01 01:30 | 1,0 | 1 |
| 16 | 2024-07-03 20:00 | 2024-07-03 21:00 | 1,0 | 1 |
| 17 | 2024-07-29 00:00 | 2024-07-29 01:00 | 1,0 | 1 |
| 18 | 2024-09-01 14:30 | 2024-09-01 16:00 | 1,5 | 2 |
| 19 | 2024-09-04 02:30 | 2024-09-04 03:30 | 1,0 | 1 |
| 20 | 2024-11-06 18:30 | 2024-11-07 00:00 | 5,5 | 10 |
| 21 | 2024-11-20 19:00 | 2024-11-20 20:30 | 1,5 | 2 |
| 22 | 2024-11-21 12:30 | 2024-11-21 13:30 | 1,0 | 1 |
| 23 | 2024-11-28 14:30 | 2024-11-28 15:30 | 1,0 | 1 |
| 24 | 2025-01-29 14:30 | 2025-01-29 16:30 | 2,0 | 3 |
| 25 | 2025-01-29 19:30 | 2025-01-29 20:30 | 1,0 | 1 |
| 26 | 2025-02-04 17:30 | 2025-02-04 18:30 | 1,0 | 1 |
| 27 | 2025-02-04 19:00 | 2025-02-04 21:00 | 2,0 | 3 |
| 28 | 2025-02-15 19:00 | 2025-02-15 20:00 | 1,0 | 1 |
| 29 | 2025-02-27 17:30 | 2025-02-27 19:00 | 1,5 | 2 |
| 30 | 2025-06-17 11:30 | 2025-06-17 12:30 | 1,0 | 1 |
| 31 | 2025-07-29 17:30 | 2025-07-29 18:30 | 1,0 | 1 |
| 32 | 2025-10-04 14:00 | 2025-10-04 15:30 | 1,5 | 2 |
| 33 | 2025-10-05 15:00 | 2025-10-05 17:30 | 2,5 | 4 |
| 34 | 2025-11-26 15:30 | 2025-11-26 16:30 | 1,0 | 1 |
| | | **TOTAL** | **~142 h** | **249** |

### 3.2 Distribuição por Tamanho

| Categoria | Lacunas | Registros faltantes | % do total |
|-----------|---------|---------------------|------------|
| Grandes (>24 h) | 2 | 160 | 64,3% |
| Médias (4–8 h) | 3 | 32 | 12,8% |
| Pequenas (1–3,5 h) | 29 | 57 | 22,9% |
| **Total** | **34** | **249** | **100%** |

### 3.3 Distribuição Mensal dos Registros Faltantes

| Mês | Registros faltantes |
|-----|---------------------|
| 2024-01 | 49 |
| 2024-02 | 47 |
| 2024-03 | 11 |
| 2024-04 | 1 |
| 2024-05 | 7 |
| 2024-06 | 94 |
| 2024-07 | 3 |
| 2024-08 | 0 |
| 2024-09 | 3 |
| 2024-10 | 0 |
| 2024-11 | 14 |
| 2024-12 | 0 |
| 2025-01 | 4 |
| 2025-02 | 7 |
| 2025-03 a 2025-05 | 0 |
| 2025-06 | 1 |
| 2025-07 | 1 |
| 2025-08 a 2025-09 | 0 |
| 2025-10 | 6 |
| 2025-11 | 1 |
| 2025-12 | 0 |
| **Total** | **249** |

As lacunas estão distribuídas por **15 meses diferentes** — não se concentram em três períodos.

---

## 4. Texto Corrigido Sugerido para o TCC

### Versão anterior (incorreta)

> "O dataset apresentou alta taxa de cobertura: 99,3% dos registros esperados para o período de 730 dias (01/01/2024 a 30/12/2025) estão presentes. Os dados faltantes (0,7%) concentraram-se em três períodos: 12-14 de março de 2024 (interrupção por manutenção), 8-9 de agosto de 2024 (queda de energia) e 3 de dezembro de 2025 (falha de conectividade). Esta cobertura é superior aos 95% recomendados pela literatura para análises de séries temporais de qualidade do ar."

### Versão corrigida

> "O dataset apresentou alta taxa de cobertura: 99,3% dos registros esperados para o período de 730 dias (01/01/2024 a 30/12/2025) estão presentes. Os 249 registros faltantes (0,7%) distribuíram-se em 34 lacunas ao longo do período, sendo as duas maiores: 31/01 a 01/02/2024 (40,5 h sem dados) e 19 a 20/06/2024 (40,5 h sem dados), que juntas representam 64% da perda total. As demais 32 lacunas foram de curta duração (1 a 7,5 h). Dos 730 dias, 698 (95,6%) apresentaram cobertura completa (48 registros). Esta cobertura é superior aos 95% recomendados pela literatura para análises de séries temporais de qualidade do ar."

### Alterações realizadas

1. **Removidas** as três datas fictícias (12-14/mar/2024, 8-9/ago/2024, 3/dez/2025) que possuem dados completos no CSV
2. **Removidas** as causas específicas sem comprovação (manutenção, queda de energia, falha de conectividade)
3. **Adicionadas** as duas maiores lacunas reais com suas datas corretas
4. **Adicionada** a informação sobre dias com cobertura completa (698 de 730)
5. **Mantidos** os dados numéricos confirmados: 99,3%, 730 dias, comparação com 95%

---

## 5. Metodologia

A verificação foi realizada por análise programática do CSV completo:

1. Leitura de todos os 34.791 timestamps do arquivo CSV
2. Ordenação cronológica dos registros
3. Identificação de lacunas: intervalos >30 minutos entre registros consecutivos
4. Contagem de registros por dia para verificar as datas contestadas
5. Cálculo de cobertura: registros presentes / registros esperados (730 × 48)
