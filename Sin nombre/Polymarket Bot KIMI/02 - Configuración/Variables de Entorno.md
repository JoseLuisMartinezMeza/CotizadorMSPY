# Variables de Entorno

> Archivo `.env` — NUNCA commitear este archivo. Ya está en `.gitignore`.

## API Keys

| Variable | Descripción | Requerido |
|----------|-------------|-----------|
| `OPENROUTER_API_KEY` | API key de OpenRouter (DeepSeek/Kimi) | Sí, si usas OpenRouter |
| `DEEPSEEK_API_KEY` | API key directa de DeepSeek | Sí, si no usas OpenRouter |
| `POLYGONSCAN_API_KEY` | API key de PolygonScan | Opcional |
| `DUNE_API_KEY` | API key de Dune Analytics | Opcional |

## Polymarket Trading

| Variable | Descripción | Ejemplo |
|----------|-------------|---------|
| `POLYMARKET_PK` | Private key de tu wallet (0x...) | `0xabc...` |
| `POLYMARKET_FUNDER` | Dirección de tu wallet | `0x123...` |
| `POLYMARKET_CHAIN_ID` | Chain ID de Polygon | `137` |
| `POLYMARKET_API_KEY` | API key de CLOB | `abc...` |
| `POLYMARKET_API_SECRET` | API secret de CLOB | `xyz...` |
| `POLYMARKET_API_PASSPHRASE` | Passphrase de CLOB | `pass...` |

## Configuración del Bot

| Variable | Descripción | Default |
|----------|-------------|---------|
| `YOLO_MODE` | `true` = trades reales, `false` = simulación | `false` |
| `MOCK_AI_MODE` | `true` = IA mock sin APIs | `false` |
| `SCAN_INTERVAL` | Segundos entre iteraciones | `300` |
| `MAX_BET_SIZE` | Tamaño máximo por trade ($) | `25.0` |
| `MAX_BANKROLL` | Bankroll total máximo ($) | `500.0` |
| `MIN_EDGE` | Edge mínimo requerido | `0.05` |
| `STOP_LOSS_PCT` | Stop loss por posición | `0.15` |
| `TAKE_PROFIT_PCT` | Take profit por posición | `0.30` |
| `MAX_DAILY_LOSS` | Pérdida máxima diaria ($) | `10.0` |

## BTC UP/DOWN

| Variable | Descripción | Default |
|----------|-------------|---------|
| `ENABLE_BTC_UPDOWN_TRADING` | Activar trading BTC UP/DOWN | `false` |
| `BTC_UPDOWN_SERIES_SLUG` | Slug de la serie | `btc-up-or-down-15m` |

## Majority Follower (BTC UP/DOWN)

| Variable | Descripción | Default |
|----------|-------------|---------|
| `MIN_MAJORITY_ADVANTAGE` | Ventaja mínima UP vs DOWN para señal | `0.02` |
| `ANALYSIS_RATE_LIMIT` | Segundos entre análisis de mercados | `0.1` |

## .env.example

El proyecto incluye `.env.example` con placeholders. Copia a `.env` y rellena con tus valores reales.

## Configuración recomendada para Majority Follower

Para operar exclusivamente en BTC UP/DOWN con alta frecuencia:

```env
YOLO_MODE=true
ENABLE_BTC_UPDOWN_TRADING=true
MIN_EDGE=0.02
MAX_BET_SIZE=25
MAX_BANKROLL=500
SCAN_INTERVAL=5
ANALYSIS_RATE_LIMIT=0.0
MIN_MAJORITY_ADVANTAGE=0.01

# Dejar vacíos para modo exclusivo:
TARGET_MARKETS=
FAVORITE_CATEGORIES=
```

> **Nota:** `MIN_EDGE=0.02` es recomendado para Majority Follower. El default `0.05` puede filtrar señales válidas cuando el mercado está 55/45. `ANALYSIS_RATE_LIMIT=0.0` da máxima velocidad entre análisis de mercados individuales.
