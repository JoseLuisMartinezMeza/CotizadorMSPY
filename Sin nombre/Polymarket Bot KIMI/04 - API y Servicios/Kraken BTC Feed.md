# Kraken BTC Feed

Fuente de precios BTC para el chart en tiempo real y el contexto de trading.

## Endpoints

### Ticker (spot price)
```
GET https://api.kraken.com/0/public/Ticker?pair=XBTUSD
```

### OHLC (velas históricas)
```
GET https://api.kraken.com/0/public/OHLC?pair=XBTUSD&interval=1&since={timestamp}
```

## Polling

- **Intervalo**: 2 segundos
- **Buffer máximo**: 1800 puntos (1 hora a 2s)
- **Vela**: 1 minuto para OHLC

## Componentes

### `fast_btc_feed.py`
- Thread con lock para thread-safety
- `get_price()` — Último precio cacheado
- `get_price_at(ts)` — Precio histórico via OHLC

### `btc_chart_api.py`
- Buffer circular con timestamps ISO
- `get_current_btc_price()` — Precio actual
- `get_price_history(minutes)` — Historial reciente

### `control.pyw`
- `_fetch_btc_once()` — Fetch directo cada 2s
- `_btc_buffer` — deque de (datetime, price)
- `_schedule_chart_redraw()` — Redibuja cada 2s

## Uso en AI Exit Advisor

El contexto BTC se pasa a DeepSeek:
- `btc_spot` — Precio spot actual
- `btc_change_5m` — Cambio % en 5 minutos
- `btc_change_10m` — Cambio % en 10 minutos
- `target_price` — Precio BTC al inicio de la ventana
