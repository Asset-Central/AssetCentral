# AssetCentral

**AssetCentral** es una plataforma de gestión de activos financieros personales para el mercado argentino. Consolida en un solo lugar las posiciones de múltiples brokers y exchanges, mostrando el portfolio unificado en tiempo real.

**Demo en vivo:** [assetcentral.com.ar](https://assetcentral.com.ar)
**Informe breve** Informe_Hackathon2026.pdf 
**Informe extendido** Informe_Extendido_Hackathon_2026.pdf

---

## ¿Qué hace?

- **Dashboard unificado** — valuación total del portfolio en ARS y USD, distribución por tipo de activo (treemap + torta), variación diaria y mensual.
- **Vinculación de cuentas** — conecta con Cocos Capital, Invertir Online (IOL), MercadoPago, Banco Nación y Binance. Las credenciales se encriptan en reposo.
- **Activos consolidados** — lista filtrable y ordenable de todos los instrumentos: CEDEARs, bonos, FCI, acciones, crypto y efectivo.
- **Portfolios personalizados** — el usuario puede armar portfolios temáticos, comparar su rendimiento e historial de precios con rangos configurables (1h · 1d · 1w · 30d · 1y).
- **Heatmap de mercado** — visualización de rendimiento por instrumento.
- **Comparador de favoritos** — comparación lado a lado de activos seleccionados.
- **Precios en tiempo real** — scheduler interno que actualiza cotizaciones vía Yahoo Finance.
- **Inflación** — ajuste por inflación integrado en las métricas de rendimiento.

---

## Stack

| Capa | Tecnología |
|---|---|
| Frontend | Lit 3 + TypeScript, Vite, Vaadin Router |
| Backend | Python, FastAPI, Supabase (PostgreSQL) |
| Auth | Supabase Auth (email/password + Google OAuth) |
| Integraciones | Binance REST API, Prometeo Open Banking, Yahoo Finance |

---

## MCP (Model Context Protocol)

El backend expone un servidor MCP compatible con cualquier agente de IA (Claude, Cursor, etc.).

Usando `fastapi-mcp`, todos los endpoints de la API se convierten automáticamente en tools MCP. Esto permite que un asistente de IA pueda consultar el contexto financiero del usuario directamente:

- `get_financial_summary` — valuación total en ARS/USD desglosada por tipo de activo.
- `get_assets` — lista de posiciones actuales, filtrables por tipo (cedear, bono, fci, crypto…).
- `get_portfolios` — portfolios del usuario con su valuación consolidada.

El header `Authorization` del usuario se propaga a cada tool call, garantizando que el agente solo accede a los datos del usuario autenticado.

**Ejemplo de uso:** conectar Claude Desktop al endpoint MCP de AssetCentral para preguntarle al asistente cosas como "¿cuánto tengo en CEDEARs?" o "¿cómo está distribuido mi portfolio?".

---

## Arquitectura

```
Frontend (Lit/TS)
      ↓ HTTP
FastAPI Backend  ←→  Supabase (auth + datos)
      ↓
  Connectors: IOL  · Nación · Binance
      ↓
  MCP Server (fastapi-mcp)  ←  agentes de IA
```

---

## Equipo

Nicolas Formichelli, Ignacio Romero, Bautista de Suto Nagy, Silvano Picard — [Asset-Central](https://github.com/Asset-Central)
