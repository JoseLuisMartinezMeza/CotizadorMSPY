# Dependencias y Arquitectura — v1.6

## Grafo de Dependencias

### Core (siempre cargado)

| Módulo | Depende de | Descripción |
|--------|-----------|-------------|
| `config` | — | Configuración central desde `.env` |
| `utils` | — | Helpers compartidos |
| `bot` | `config` | Entry point CLI |
| `bot.runner` | `config`, `market_data`, `strategies`, `trading_engine`, `ai_analyzer`, `crypto_strategies`, `crypto_feed`, `btc_updown_copytrading`, `copy_trading`, `whale_watcher`, `exit_report`, `dashboard`, `utils`, `fast_btc_feed`, `btc_chart_api`, `price_history`, `ai_bridge` | **Motor principal** |

### Estrategias (crypto_strategies)

| Módulo | Depende de | Descripción |
|--------|-----------|-------------|
| `crypto_strategies.base` | — | `StrategyResult`, `EvalContext` |
| `crypto_strategies.majority_follower` | `base` | **Nueva estrategia única** |
| `crypto_strategies.ensemble` | `majority_follower`, `base` | Orchestrator simplificado |

> **Viejas estrategias movidas a `archive/crypto_strategies/`**: gbm, meanrev, momentum, orderbook, whale, btc_updown.

### Diagrama simplificado

```
                    ┌─────────────┐
                    │   bot.py    │
                    └──────┬──────┘
                           │
              ┌────────────┼────────────┐
              ▼            ▼            ▼
       ┌──────────┐ ┌──────────┐ ┌──────────┐
       │ control  │ │ bot.py   │ │ launch   │
       │ .pyw     │ │ --once   │ │ _bot.py  │
       └──────────┘ └────┬─────┘ └──────────┘
                          │
                    ┌─────┴─────┐
                    │ bot.runner│
                    └─────┬─────┘
           ┌──────────────┼──────────────┐
           ▼              ▼              ▼
    ┌──────────┐  ┌──────────────┐  ┌──────────┐
    │strategies│  │crypto_strategies│  │trading   │
    │          │  │  (majority    │  │_engine   │
    │          │  │   follower)   │  │          │
    └────┬─────┘  └───────┬──────┘  └────┬─────┘
         │                │              │
    ┌────┴────┐     ┌────┴────┐    ┌────┴────┐
    │market   │     │fast_btc │    │wallet   │
    │_data    │     │_feed    │    │_data    │
    └─────────┘     └─────────┘    └─────────┘
```

## Cómo leer este grafo

- **Flechas** indican importación (`A -> B` significa que A importa B).
- **Módulos sin dependencias** son los más seguros de modificar.
- **`bot.runner`** es el nodo más acoplado — cualquier cambio aquí afecta a casi todo.
- **Majority Follower** es ahora una hoja del grafo: solo depende de `base`, y `ensemble` es el único que depende de ella.
