---
date: '2025-05-27'
project: Polymarket Bot KIMI
tags:
  - trading
  - polymarket
  - sl-tp
  - bot
  - session-log
---
# Sesión 2025-05-27 - Advanced SL/TP Engine v2

## Resumen Ejecutivo

En esta sesión se implementaron **tres mejoras críticas** al sistema de salidas del bot, se ejecutó una sesión real de 15 minutos con captura de datos, y se reorganizó la estructura de tests del proyecto.

---

## 📊 Sesión Real de 15 Minutos (11:48 - 12:03 UTC)

| Métrica | Valor |
|---|---|
| **Duración** | 14 min 54 seg |
| **Iteraciones** | 15 |
| **Señales MF** | 13 |
| **BUYs ejecutados** | 4 ($1.00 c/u) |
| **SELLs ejecutados** | 3 |
| **Duplicate skips** | 4 ✅ |
| **Errores** | 0 |
| **P&L Total Realizado** | **+$0.30 (+1.2%)** |
| **Win Rate** | **66.7%** (2 TP / 1 SL) |
| **Mejor cierre** | **+34.8%** (Take Profit) |
| **Peor cierre** | **-6.8%** (Stop Loss) |

### Secuencia de trades

| # | Acción | Outcome | Precio | Resultado |
|---|--------|---------|--------|-----------|
| 1 | BUY | Up | $0.575 | — |
| 2 | SELL | Up | $0.575 | **+34.8% TP** ✅ |
| 3 | BUY | Up | $0.735 | — |
| 4 | SELL | Up | $0.735 | **-6.8% SL** 🔴 |
| 5 | BUY | Down | $0.605 | — |
| 6 | SELL | Down | $0.605 | **+1.7% TP** ✅ |
| 7 | BUY | Up | $0.565 | *abierto al final* |

### Hallazgos clave

- ✅ Protección anti-duplicado funciona: 4 oportunidades skippeadas
- ✅ SL/TP Watcher operativo: 3 cierres automáticos en 15 min
- ✅ Sin errores: 0 excepciones, 0 insufficient balance
- ✅ P&L positivo: +$0.30 en 15 min sobre bankroll de $25
- 1 posición abierta al final: `btc-updown-15m-1779904800 / Up` @ $0.565

---

## 🚀 Nuevas Features Implementadas

### 1. 🔒 Breakeven Lock

**Qué hace**: Cuando P&L ≥ 20%, mueve automáticamente el SL a `entry + 2%` (BUY) o `entry - 2%` (SELL). Nunca pierdes capital si la ganancia alcanza ese nivel.

**Configuración**:
```bash
BREAKEVEN_LOCK_ENABLED=true
BREAKEVEN_LOCK_TRIGGER_PCT=0.20
BREAKEVEN_LOCK_BUFFER_PCT=0.02
```

**Método**: `TradingEngine.apply_breakeven_lock()` — llamado desde `check_pnl_exits()` en cada ciclo del watcher.

---

### 2. 📈 Momentum Filter

**Qué hace**: Al tocar el TP rígido, evalúa la velocidad del precio (%/min). Si el momentum es menor al umbral, cierra normalmente (TAKE_PROFIT). Si es mayor, activa trailing.

**Por qué**: Evita activar trailing en mercados planos donde el TP se tocó por suerte, no por tendencia.

**Configuración**:
```bash
TRAILING_TP_MIN_MOMENTUM_PCT_PER_MIN=2.0
```

**Implementación**: `SLTPWatcher._watch()` computa `velocity_pct_per_min` y lo pasa a `check_position_exit()`.

---

### 3. 🪜 Stepped Trailing TP

**Qué hace**: El trailing distance se ajusta según el nivel de ganancia:

| P&L alcanzado | Trail distance | Ejemplo (BUY @ $0.50) |
|---|---|---|
| +30% – +50% | 10% | Pico $0.65 → SL $0.585 |
| +50%+ | 5% | Pico $0.75 → SL $0.712 |

**Por qué**: Cuanto más sube, más apretado el SL. Protege ganancias grandes sin cerrar prematuramente.

**Configuración**:
```bash
TRAILING_TP_STEPPED_ENABLED=true
TRAILING_TP_TIERS=[[0.30, 0.10], [0.50, 0.05]]
```

---

## 🔄 Flujo Completo de una Posición

```
Open → SL=entry*0.85, TP=entry*1.35
  │
  ├─ Precio cae a SL ───────────────────────────────→ Close STOP_LOSS
  │
  ├─ P&L > 20% ─────────────────────────────────────→ 🔒 Breakeven Lock: SL → entry*1.02
  │
  ├─ Precio toca TP rígido ─────────────────────────→ Evaluar Momentum
  │     ├─ Velocidad < 2%/min ──────────────────────→ Close TAKE_PROFIT (normal)
  │     └─ Velocidad ≥ 2%/min ──────────────────────→ 🪜 Activate Stepped Trailing
  │             ├─ P&L 30-50% ──────────────────────→ Trail 10%
  │             └─ P&L > 50% ───────────────────────→ Trail 5%
  │
  └─ Precio retrasa al SL dinámico ─────────────────→ Close TRAILING_STOP
```

---

## 🛠️ Archivos Modificados

| Archivo | Cambios |
|---|---|
| `config.py` | +6 campos: stepped tiers, momentum threshold, breakeven params |
| `trading_engine.py` | `Position` (+4 campos), `apply_breakeven_lock()`, `check_position_exit()` (stepped+momentum), `check_pnl_exits()` |
| `bot/runner.py` | `SLTPWatcher._watch()` (velocity tracking) |
| `.env.example` | Documentación completa de nuevas variables |
| `tests/test_trading_engine_pnl.py` | +8 tests (breakeven, momentum, stepped trailing) |

---

## 🧪 Reorganización de Tests

Estructura final:

```
tests/
├── unit/                    ← 71 tests
│   ├── test_ai_bridge.py
│   ├── test_ai_exit_advisor.py
│   ├── test_exit_report.py
│   ├── test_market_data.py
│   ├── test_shadow_mode.py
│   ├── test_strategies.py
│   └── test_trading_engine_pnl.py
├── integration/             ← 4 tests
│   └── test_bot_pipeline.py
├── crypto_strategies/       ← 4 tests
│   ├── test_base.py
│   └── test_ensemble.py
└── legacy/                  ← Tests de módulos eliminados
    ├── test_btc_updown_copytrading.py
    ├── test_btc_updown_discovery.py
    ├── test_copy_trading.py
    ├── test_crypto_feed.py
    ├── test_crypto_strategy.py
    ├── test_fast_btc_feed.py
    ├── test_price_history.py
    └── test_whale_watcher.py
```

**Resultados**: 79/79 tests core pasando ✅

---

## ⚙️ Configuración Recomendada para BTC UP/DOWN 15m

```bash
# Activar todo
TRAILING_TP_ENABLED=true
TRAILING_TP_STEPPED_ENABLED=true
TRAILING_TP_TIERS=[[0.30, 0.10], [0.50, 0.05]]
TRAILING_TP_MIN_MOMENTUM_PCT_PER_MIN=3.0
TRAILING_TP_DISTANCE_PCT=0.10

BREAKEVEN_LOCK_ENABLED=true
BREAKEVEN_LOCK_TRIGGER_PCT=0.20
BREAKEVEN_LOCK_BUFFER_PCT=0.02
```

**Razonamiento**: En mercados 15min, momentum de 3%/min filtra ruido. Breakeven a 20% protege rápido. Stepped trailing permite capturar los raros +50%+ cuando BTC se mueve fuerte.

---

## 📁 Archivos Generados en la Sesión

- `real_15min_analysis_2026-05-27_1148.xlsx` — Reporte completo de la sesión de 15 min
- `_parsed_15min.json` — Datos crudos parseados
- `simulate_advanced_sltp.py` — Script de simulación de las nuevas features
- `parse_15min_report.py` — Parser del log a Excel

---

## 🎯 Próximos Pasos Sugeridos

1. Activar las nuevas configuraciones en `.env`
2. Lanzar sesión real de 15 minutos con Advanced SL/TP Engine v2 activado
3. Comparar resultados vs sesión anterior (sin trailing escalonado)
4. Ajustar tiers según comportamiento observado

---

*Guardado el: 2025-05-27*
*Bot version: v1.0 + Advanced SL/TP Engine v2*
