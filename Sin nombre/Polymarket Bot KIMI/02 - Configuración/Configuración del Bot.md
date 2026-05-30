# Configuración del Bot

El bot usa un sistema de configuración centralizado en `config.py`.

## Cómo funciona

1. `config.py` lee variables de entorno al inicio
2. Genera un objeto `CONFIG` inmutable (dataclass)
3. `config_editor.py` permite actualizar en caliente

## Configs críticas

### Para trades reales (YOLO)

```env
YOLO_MODE=true
POLYMARKET_PK=0x...
POLYMARKET_FUNDER=0x...
POLYMARKET_API_KEY=...
POLYMARKET_API_SECRET=...
POLYMARKET_API_PASSPHRASE=...
```

### Para análisis con IA

```env
MOCK_AI_MODE=false
DEEPSEEK_API_KEY=sk-...
# o
OPENROUTER_API_KEY=sk-or-v1-...
AI_MODEL=deepseek-chat
```

### Para BTC UP/DOWN exclusivo

```env
ENABLE_BTC_UPDOWN_TRADING=true
BTC_UPDOWN_SERIES_SLUG=btc-up-or-down-15m
ENABLE_BTC_UPDOWN_COPYTRADING=true
ENABLE_AI_EXIT_ADVISOR=true
AI_EXIT_THRESHOLD=0.60
```

## Validación

`validate_config()` en `config.py` verifica:
- `MAX_BET_SIZE <= MAX_BANKROLL`
- `0 < MIN_EDGE < 1`
- `0 < STOP_LOSS_PCT <= 1`
- `0 < TAKE_PROFIT_PCT <= 1`

## Hot-reload

Algunas configs se pueden cambiar en runtime via `config_editor.py`:
- `scan_interval`
- `max_bet_size`
- `min_edge`
- `stop_loss_pct`
- `take_profit_pct`

## Archivos relacionados

- `config.py` — Definición y carga
- `config_editor.py` — API de edición
- `.env` — Variables locales (no commitear)
- `.env.example` — Template
