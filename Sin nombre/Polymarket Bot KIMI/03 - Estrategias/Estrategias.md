# Estrategias — v1.6 Majority Follower (LIMPIO)

Panel de estrategias en `control.pyw`:

| Estrategia | ID | Descripción |
|------------|-----|-------------|
| **YOLO Mode** | `yolo_mode` | Trades reales con pUSD |
| **Majority Follower** | `majority_follower` | Apostar al mayor % del mercado (siempre activa) |

> **Todas las demás estrategias han sido eliminadas.** El bot opera ahora con una única regla simple.

## Majority Follower (Estrategia única)

**Regla simple:** Siempre apostar al outcome con el mayor porcentaje del mercado.

- UP > DOWN (con ventaja ≥ 2%) → COMPRAR UP
- DOWN > UP (con ventaja ≥ 2%) → COMPRAR DOWN
- Si cambia el favorito → SWITCH automático (vender actual, comprar nuevo)

**Ventajas:**
- Sin IA (máxima velocidad)
- Sin llamadas a APIs externas
- Decisiones en milisegundos
- Sigue el sentimiento del mercado en tiempo real

## Estrategias eliminadas

Las siguientes estrategias y componentes fueron **completamente eliminados** del proyecto:

| Componente | Archivo | Razón |
|-----------|---------|-------|
| Whale Watcher | `whale_watcher.py` | Eliminado — no necesario con Majority Follower |
| Copy Trading (legacy) | `copy_trading.py` | Eliminado — reemplazado por Majority Follower |
| BTC UP/DOWN Copy | `btc_updown_copytrading.py` | Eliminado — reemplazado por Majority Follower |
| AI Analysis | `ai_analyzer.py` (uso) | Eliminado del flujo — no se llama más |
| Crypto Feed | `crypto_feed.py` | Eliminado — no necesario |
| AI Exit Advisor | `ai_exit_advisor` logic | Eliminado — no se consulta más a IA |
| GBM / Black-Scholes | `crypto_strategies/gbm.py` | Movido a `archive/` |
| Mean Reversion | `crypto_strategies/meanrev.py` | Movido a `archive/` |
| Momentum | `crypto_strategies/momentum.py` | Movido a `archive/` |
| Order Book | `crypto_strategies/orderbook.py` | Movido a `archive/` |
| Whale Signals | `crypto_strategies/whale.py` | Movido a `archive/` |
| BTC Brownian | `crypto_strategies/btc_updown.py` | Movido a `archive/` |

## Modo exclusivo BTC UP/DOWN

Cuando `ENABLE_BTC_UPDOWN_TRADING=true` y no hay `TARGET_MARKETS` ni `FAVORITE_CATEGORIES`, el bot opera **exclusivamente** en BTC UP/DOWN usando Majority Follower.

Para BTC UP/DOWN, la IA está **completamente bypassada** — el bot nunca llama a DeepSeek/OpenRouter, minimizando latencia.
