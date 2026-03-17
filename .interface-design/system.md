# Respira Boa Vista — Design System

## Direction

Interface enraizada na paisagem de Boa Vista/RR. Tons terrosos quentes inspirados no lavrado roraimense — ocre do capim seco, cinza-fumaça dos ceus de queimada, verde que retorna com a chuva. A metafora central e o "ritmo de respiracao": o IQA pulsa como um pulmao, dados expandem e contraem com filtros. Sensacao de vigilancia ambiental — seria, quente, atenta.

## Tokens

### Primary
- `--primary: #B87A2E` (ocre/ambar — cor da terra do lavrado)
- `--primary-light: rgba(184, 122, 46, 0.10)`

### Text Hierarchy
- `--ink: #3A3028` (warm dark — texto principal)
- `--ink-secondary: #6E6358` (texto de apoio)
- `--ink-muted: #9E948A` (labels, metadata)
- `--ink-faint: #BFB8AF` (unidades, placeholders)

### Surfaces (warm parchment)
- `--canvas: #F3EEE6` (fundo da pagina)
- `--surface: #FAF8F4` (navbar, sidebar, elevacao 1)
- `--surface-raised: #FDFCFA` (cards, elevacao 2)

### Borders
- `--border: rgba(58, 48, 40, 0.10)` (separacao padrao)
- `--border-emphasis: rgba(58, 48, 40, 0.18)` (inputs, foco, enfase)

### AQI (dessaturadas para harmonia quente)
- `--aqi-boa: #4A9E5C`
- `--aqi-moderada: #D4A843`
- `--aqi-sensivel: #D4793A`
- `--aqi-prejudicial: #C4392F`
- `--aqi-muito-prejudicial: #7B3580`
- `--aqi-perigosa: #6B1D2A`

### Chart Colors
- `--chart-pm25: #B87A2E` (ocre — mesmo do primary)
- `--chart-aqi: #6E8B74` (verde-seco do cerrado)
- `--chart-temp: #C4642A` (terracota da terra exposta)
- `--chart-hum: #5B8A9A` (azul-aco do ceu encoberto)

## Depth Strategy

**Borders-only.** Sem sombras em nenhum componente. Separacao por bordas sutis de tom quente. Navbar e sidebar usam `border-bottom` / `border-right` em vez de box-shadow.

## Spacing

Base unit: 8px. Escala usada:
- Micro: 4px (gap entre icone e texto)
- Component: 8px, 12px (padding interno, gaps)
- Section: 16px, 24px (padding de cards, gaps de grid)
- Major: 32px, 48px (padding de secoes)

## Border Radius

- `--radius-sm: 8px` (inputs, botoes, mini-stats)
- `--radius-md: 12px` (cards, filtros, CTAs)
- `--radius-lg: 16px` (card principal AQI, secoes)

## Typography

Font: Inter. Decisoes:
- Headlines: weight 700-800, `letter-spacing: -0.02em` a `-0.03em`
- Data/numeros: weight 700, `letter-spacing: -0.03em`, `font-variant-numeric: tabular-nums`
- Labels: weight 700, `font-size: 0.65rem`, `text-transform: uppercase`, `letter-spacing: 0.08em`
- Body: weight 400-500, `font-size: 0.85-0.95rem`

## Signature

**Breathing AQI** — O card principal do IQA usa `animation: breathe 4s ease-in-out infinite` com `transform: scale(1.015)`. Sutil, quase imperceptivel — reforça a metafora de respiracao sem distrair.

## Component Patterns

### Navbar
- Background: `var(--surface)`, mesma cor que canvas vizinho
- Separacao: `border-bottom: 1px solid var(--border)`
- Logo: `var(--primary)`, weight 700, tracking negativo
- Links: `var(--ink-secondary)`, weight 500, active em `var(--primary)`

### Stat Card
- Background: `var(--surface-raised)`
- Border: `1px solid var(--border)` + `border-left: 3px solid var(--primary)`
- Hover: `border-left-color` muda para `var(--ink)`
- Sem sombra, sem transform no hover

### Filter Bar
- Background: `var(--surface-raised)`
- Border: `1px solid var(--border)`
- Selects: `background: var(--surface)`, `border: 1.5px solid var(--border-emphasis)`
- Botao aplicar: `background: var(--primary)`, hover com `filter: brightness(0.92)`
- Botao reset: transparente, borda emphasis, hover com primary-light

### Chart Card
- Background: `var(--surface-raised)`
- Border: `1px solid var(--border)`
- Grid lines: `stroke: var(--border)`
- Axis labels: `fill: var(--ink-muted)`, Inter 11px

### Map Sidebar
- Mesma cor que canvas (`var(--surface)`)
- Separacao por `border-right: 1px solid var(--border)`
- Mini-stats: `background: var(--canvas)`, `border: 1px solid var(--border)`

### Tooltip (D3)
- Background: `var(--ink)`, color: `var(--canvas)`
- Border: `1px solid var(--border-emphasis)`
- Radius: `var(--radius-sm)`, font-size: 0.78rem

### CTA Buttons
- Primary: `background: var(--primary)`, white text, radius-md
- Secondary: transparente, `border: 1.5px solid var(--border-emphasis)`, hover com primary-light
