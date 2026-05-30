# Gráfico BTC en Tiempo Real

Chart embebido en `control.pyw` usando matplotlib.

## Elementos visuales

| Elemento | Color | Descripción |
|----------|-------|-------------|
| Línea principal | Azul/Rojo | Precio BTC spot |
| Línea horizontal punteada | Verde/Rojo | Target (precio de apertura de ventana) |
| Línea vertical punteada | Naranja | Momento de inicio de la ventana |
| Label inferior | Dinámico | Target + Spot + Diff % |

## Cálculo del target

```python
mkt = data_client.get_active_btc_updown_market(CONFIG.btc_updown_series_slug)
window_start_time = mkt.start_date.astimezone().replace(tzinfo=None)

# Buscar en buffer
best_price = min_delta(t in _btc_buffer, window_start_time)

# Fallback si no está en buffer (< 60s cercano)
_fetch_btc_open_at_timestamp(window_start_time)  # Kraken OHLC
```

## Timezone fix

Bug anterior: `start_date` viene en UTC pero el buffer usa hora local.
Fix: `mkt.start_date.astimezone().replace(tzinfo=None)` convierte UTC → local.

## Actualización

- Poller thread: fetch cada 2s → `_btc_buffer.append((datetime.now(), price))`
- Main thread: redraw cada 2s → `_schedule_chart_redraw()`

## Constantes

```python
CHART_POLL_SECONDS = 2      # Polling interval
CHART_BUFFER_MAX = 1800     # Max points (1h at 2s)
CHART_15M_POINTS = 450      # Points for 15m view
KRAKEN_BTC_PAIR = "XBTUSD"  # BTC/USD
```
