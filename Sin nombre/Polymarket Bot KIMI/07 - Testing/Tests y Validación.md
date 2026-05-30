# Tests y Validación

## Suite de tests

183 tests totales (1 fallo conocido por entorno).

```bash
python -m pytest tests/ -q
```

## Tests nuevos

### `test_ai_exit_advisor.py`

6 tests para `advise_position_exit()`:
- `test_advise_sell` — IA recomienda SELL
- `test_advise_hold` — IA recomienda HOLD
- `test_invalid_action_normalized_to_hold` — Acción inválida → HOLD
- `test_api_error_returns_none` — Error de API → None
- `test_malformed_json_returns_none` — JSON malformado → None

### `test_exit_report.py`

5 tests para `ExitReport`:
- `test_log_buy` — Registro de compra
- `test_log_sell` — Registro de venta con metadata IA
- `test_multiple_transactions` — Varias transacciones
- `test_persistence` — Persistencia en archivo
- `test_clear` — Limpieza de reporte

## Tests existentes

| Archivo | Cobertura |
|---------|-----------|
| `test_bot_pipeline.py` | Ciclo completo del bot |
| `test_copy_trading.py` | Copy trading legacy |
| `test_crypto_strategy.py` | Estrategias crypto |
| `test_trading_engine_pnl.py` | P&L y exits |
| `test_whale_watcher.py` | Whale watcher |
| `test_fast_btc_feed.py` | Feed BTC |
| `test_market_data.py` | Datos de mercado |
| `test_btc_updown_copytrading.py` | Copy trading BTC |
| `test_shadow_mode.py` | Shadow mode |

## Compilación

```bash
python -m py_compile control.pyw bot/runner.py trading_engine.py ai_analyzer.py exit_report.py
```

## Fallo conocido

`test_ai_bridge::test_is_bridge_active_false_without_url` falla porque el entorno tiene `LOCAL_AI_URL` configurado. No afecta al funcionamiento del bot.
