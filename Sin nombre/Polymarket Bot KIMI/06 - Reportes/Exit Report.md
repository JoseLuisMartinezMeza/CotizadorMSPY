# Exit Report

Reporte acumulativo de transacciones BTC UP/DOWN.

## Ubicación

```
reports/exit_report.json
```

## Estructura

```json
{
  "generated_at": "2026-05-25T04:00:00Z",
  "transactions": [
    {
      "type": "BUY",
      "timestamp": "2026-05-25T03:30:00Z",
      "market_slug": "btc-up-or-down-15m-1779681600",
      "outcome": "Up",
      "entry_price": 0.48,
      "size_usd": 2.00,
      "source": "AI_COPY"
    },
    {
      "type": "SELL",
      "timestamp": "2026-05-25T03:42:00Z",
      "market_slug": "btc-up-or-down-15m-1779681600",
      "outcome": "Up",
      "exit_price": 0.62,
      "pnl_pct": 29.2,
      "exit_reason": "AI_EXIT",
      "ai_confidence": 0.72,
      "ai_reasoning": "Momentum invertido",
      "ai_urgency": "high"
    }
  ]
}
```

## Campos de SELL con IA

| Campo | Descripción |
|-------|-------------|
| `exit_reason` | `AI_EXIT`, `TP_PNL_60%`, `SL_PNL_-33%`, etc. |
| `ai_confidence` | Confianza de la IA (0.0 - 1.0) |
| `ai_reasoning` | Razón de la decisión |
| `ai_urgency` | `low`, `medium`, `high` |

## API

```python
from exit_report import exit_report

# Log compra
exit_report.log_buy(market_slug, outcome, entry_price, size_usd, source)

# Log venta
exit_report.log_sell(
    market_slug, outcome, exit_price, pnl_pct,
    exit_reason, ai_confidence, ai_reasoning, ai_urgency
)

# Resumen
summary = exit_report.get_summary()
# {total_buys, total_sells, ai_exits, total_pnl_pct, last_updated}
```

## Automatización

- Compras se loguean automáticamente en `trading_engine.place_market_order()`
- Ventas se loguean automáticamente en `trading_engine.close_position()`
- Thread-safe con `threading.Lock`
