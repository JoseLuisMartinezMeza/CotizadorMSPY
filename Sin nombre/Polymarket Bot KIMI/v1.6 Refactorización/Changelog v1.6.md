# Changelog v1.6 — Majority Follower Refactor

## 🎯 Nueva Estrategia

- **`crypto_strategies/majority_follower.py`** — Estrategia única que apuesta siempre al outcome con mayor porcentaje del mercado.
- **Switch automático** — Si cambia el favorito, el bot vende la posición actual y compra la nueva automáticamente.
- **Sin IA para BTC UP/DOWN** — Bypass completo de DeepSeek/OpenRouter en mercados BTC UP/DOWN, reduciendo latencia de segundos a milisegundos.

## ♻️ Refactorización

### Eliminadas del ensemble
Las siguientes estrategias fueron movidas a `archive/crypto_strategies/`:

- `gbm.py` — Geometric Brownian Motion
- `meanrev.py` — Mean Reversion
- `momentum.py` — Momentum Trading
- `orderbook.py` — Order Book Analysis
- `whale.py` — Whale Signals
- `btc_updown.py` — Brownian Motion para BTC UP/DOWN

### Simplificado
- **`crypto_strategies/ensemble.py`** — Ahora solo orquesta `majority_follower`.
- **`bot/runner.py`** — Agregada lógica de switch + bypass de IA para BTC UP/DOWN.
- **`bot.py`** — Bugfix: `--mock-ai` ahora se procesa antes de validar API keys.

## 🚀 Nuevos archivos

- **`launch_bot.py`** — Lanzador multiplataforma (reemplaza `.bat` y `.vbs`).
- **`docs/dependencies.md`** — Documentación del grafo de dependencias.

## 🎨 Frontend actualizado

- **`control.pyw`**
  - Título actualizado a "v1.6 | Majority Follower Strategy"
  - Panel en tiempo real de UP% vs DOWN%
  - Indicador de favorito y posición abierta
  - Estrategia "Crypto Quant" reemplazada por "Majority Follower"
  - **Nuevos parámetros en panel:**
    - `Ventaja Min MF (%)` — Umbral mínimo de ventaja UP vs DOWN (0.5%-20%)
    - `Rate Limit (s)` — Tiempo entre análisis de mercados (0.0s-2.0s)
    - `Intervalo Scan (s)` — Ahora mínimo 1s (antes 5s) para alta frecuencia

## ✅ Tests

- **`tests/crypto_strategies/test_ensemble.py`** — Reescrito para probar Majority Follower.
- Tests viejos movidos a `archive/tests/crypto_strategies/`.

## 📁 Estructura de archivos

```
Polymarket-bot/
├── bot.py                      # Entry point CLI
├── launch_bot.py               # NUEVO — Lanzador multiplataforma
├── control.pyw                 # ACTUALIZADO — GUI v1.6
├── config.py                   # Configuración central
├── trading_engine.py           # Motor de ejecución
├── strategies.py               # Validación + portfolio
├── market_data.py              # Cliente Polymarket API
├── crypto_strategies/
│   ├── __init__.py             # Solo exporta ensemble
│   ├── base.py                 # StrategyResult, EvalContext
│   ├── majority_follower.py    # NUEVO — Estrategia única
│   └── ensemble.py             # SIMPLIFICADO — Solo majority_follower
├── archive/
│   ├── crypto_strategies/      # Viejas estrategias eliminadas
│   │   ├── gbm.py
│   │   ├── meanrev.py
│   │   ├── momentum.py
│   │   ├── orderbook.py
│   │   ├── whale.py
│   │   └── btc_updown.py
│   └── tests/crypto_strategies/# Tests viejos
└── docs/
    └── dependencies.md         # Grafo de dependencias
```

## ⚡ Performance

| Métrica | v1.5 | v1.6 |
|---------|------|------|
| Estrategias activas | 6 | 1 |
| Latencia decisión BTC UP/DOWN | 3-10s | <100ms |
| Llamadas a IA por ciclo | 1+ | 0 |
| Llamadas a APIs externas | Kraken + CoinGecko + DeepSeek | Solo Polymarket |

## 🔧 Configuración recomendada

```env
YOLO_MODE=true
ENABLE_BTC_UPDOWN_TRADING=true
MIN_EDGE=0.02
SCAN_INTERVAL=5
ANALYSIS_RATE_LIMIT=0.0
MIN_MAJORITY_ADVANTAGE=0.01
TARGET_MARKETS=
FAVORITE_CATEGORIES=
```
