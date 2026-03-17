# weather-mcp-ts

A production-ready **Model Context Protocol (MCP) server** for weather intelligence, built with TypeScript, Express, and the [Open-Meteo API](https://open-meteo.com/). Deployed on Render with Auth0 JWT authentication and OAuth 2.0 support.

> **Built on an iPhone 17** using [Claude Code](https://claude.ai/code) (web) and the GitHub iOS app — no laptop required.

## Features

- **14 weather tools** across two layers of abstraction
- JWT authentication via Auth0
- OAuth 2.0 / RFC 9728 Protected Resource Metadata
- Dockerized with Render deployment config
- Vitest test suite
- Structured logging with Pino

## Tool Inventory

### Layer 1 — Primitives

Single-purpose tools returning focused, typed data.

| Tool | Description |
|---|---|
| `get_weather` | Current conditions — temperature, humidity, wind, sky |
| `get_forecast` | 7-day daily forecast with high/low and precipitation chance |
| `get_hourly_forecast` | Hourly temperature, conditions, and precipitation probability |
| `get_air_quality` | AQI (US/EU), PM2.5, PM10, ozone, UV index, pollen |
| `get_marine` | Wave height/direction/period, swell, ocean currents |
| `get_soil_conditions` | Soil moisture and temperature at multiple depths |
| `get_wind` | Speed, direction, gusts, Beaufort scale, 7-day forecast |
| `get_precipitation` | Rain/snow/shower data, hourly probabilities, 7-day totals |
| `get_humidity` | Relative humidity, dew point, comfort levels, fog potential |

### Layer 2 — Compound Tools

Vertical tools that combine multiple data sources into a single contextual assessment.

| Tool | Description |
|---|---|
| `get_fire_weather` | Wildfire risk — wind, humidity, temp, soil moisture, precipitation history |
| `get_growing_conditions` | Agriculture — soil, evapotranspiration, frost risk, planting recommendations |
| `get_outdoor_conditions` | Outdoor activity — weather, AQI, UV, pollen, suitability ratings |
| `get_marine_conditions` | Full marine assessment — surfing, boating, fishing, diving recommendations |
| `get_severe_weather` | Hazard scan — heat, cold, wind, storm, air quality, UV alerts |

## Stack

- **Runtime**: Node.js + TypeScript (ESM)
- **Framework**: Express
- **MCP SDK**: `@modelcontextprotocol/sdk`
- **Auth**: Auth0 JWT (`jose`)
- **Weather data**: Open-Meteo (no API key required)
- **Validation**: Zod
- **Logging**: Pino + pino-http
- **Testing**: Vitest
- **Deployment**: Docker + Render

## Getting Started

```bash
git clone https://github.com/areeves9/weather-mcp-ts.git
cd weather-mcp-ts
cp .env.example .env
npm install
npm run dev
```

### Environment Variables

See `.env.example` for required configuration (Auth0 domain, client ID, server URL, etc.).

## Development

```bash
npm run dev        # Run with tsx (no build step)
npm run build      # Compile TypeScript
npm test           # Run Vitest suite
npm run inspect    # Open MCP Inspector
```

## Deployment

The repo includes a `render.yaml` and `Dockerfile` for one-command deployment to Render.

```bash
docker build -t weather-mcp-ts .
docker-compose up
```

## Architecture

```
src/
├── app.ts          # Express app factory (CORS, auth, routing)
├── index.ts        # Server entrypoint
├── manifest.ts     # Tool manifest for hub discovery
├── auth/           # JWT middleware
├── config/         # Environment config
├── mcp/            # MCP request handler
├── oauth/          # OAuth 2.0 proxy routes
├── shared/         # Logger, error types, utilities
└── tools/
    ├── weather/        # Current conditions
    ├── forecast/       # Daily + hourly forecast
    ├── air-quality/    # AQI + pollutants
    ├── marine/         # Ocean data
    ├── soil/           # Soil moisture + temp
    ├── wind/           # Wind conditions
    ├── precipitation/  # Rain + snow
    ├── humidity/       # Humidity + dew point
    ├── fire-weather/   # Wildfire risk (compound)
    ├── agriculture/    # Growing conditions (compound)
    ├── outdoor/        # Activity assessment (compound)
    ├── marine-conditions/ # Marine safety (compound)
    └── severe-weather/ # Hazard alerts (compound)
```

## License

MIT
