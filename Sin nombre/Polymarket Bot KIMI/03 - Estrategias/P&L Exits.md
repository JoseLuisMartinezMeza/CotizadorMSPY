# P&L Exits

Sistema de cierre automático de posiciones basado en P&L porcentual.

## Umbrales

| Tipo | Umbral | Descripción |
|------|--------|-------------|
| **Take Profit** | `+60%` | Cierra cuando P&L >= +60% |
| **Stop Loss** | `-33%` | Cierra cuando P&L <= -33% |

## Fórmula de P&L

```python
if position.side == "BUY":
    pnl_usd = (current_price - entry_price) * size_shares
else:
    pnl_usd = (entry_price - current_price) * size_shares

pnl_pct = pnl_usd / size_usd
```

## Flujo

```
_check_exits()
├── Para cada posición abierta:
│   ├── Obtener precio actual del mercado
│   ├── update_position_pnl(position, current_price)
│   └── check_position_exit(position, current_price)  # TP/SL por precio
└── check_pnl_exits(market_prices)
    ├── pnl_pct >= +60% → exit_reason = "TP_PNL_60%"
    └── pnl_pct <= -33% → exit_reason = "SL_PNL_-33%"
```

## Prioridad con AI Exit Advisor

El AI Exit Advisor se ejecuta **después** de `_check_exits()` pero **antes** de `scan_and_analyze()`.

```
run_once()
├── _check_exits()          ← TP/SL fijos primero
├── _ai_advise_exits()      ← IA puede cerrar antes de TP/SL
└── scan_and_analyze()      ← Nuevas entradas
```

La IA puede cerrar una posición antes de que alcance el TP/SL fijo si detecta que el momentum se está invirtiendo.

## Configuración

| Variable | Default |
|----------|---------|
| `STOP_LOSS_PCT` | `0.15` (15% por posición) |
| `TAKE_PROFIT_PCT` | `0.30` (30% por posición) |

Nota: Los umbrales de P&L (+60% / -33%) son fijos en código, no configurables por env.
