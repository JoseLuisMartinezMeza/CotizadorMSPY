# Majority Follower — Nueva Estrategia Única

**Archivo:** `crypto_strategies/majority_follower.py`

## Regla

> Siempre apostar al outcome con el **mayor porcentaje** del mercado.

## Lógica

```
1. Leer precios de UP y DOWN del mercado BTC UP/DOWN 15m
2. Normalizar a porcentajes: UP% = UP_price / (UP_price + DOWN_price)
3. Si UP% > DOWN% por más de 2% → COMPRAR UP
4. Si DOWN% > UP% por más de 2% → COMPRAR DOWN
5. Si ya hay posición y cambia el favorito → SWITCH (vender + comprar nuevo)
```

## Por qué funciona

- En mercados binarios de corta duración (15 min), la **mayoría suele tener razón**.
- El mercado incorpora información nueva (movimientos de BTC) más rápido que cualquier modelo individual.
- Al seguir la mayoría, nos alineamos con el momentum del sentimiento colectivo.

## Parámetros

| Parámetro | Valor | Descripción |
|-----------|-------|-------------|
| `MIN_PCT_ADVANTAGE` | `0.02` (2%) | Ventaja mínima para generar señal. Evita ruido cuando el split es 49.9/50.1. |

## Ventajas sobre estrategias anteriores

| Aspecto | Viejas estrategias | Majority Follower |
|---------|-------------------|-------------------|
| Complejidad | 6 modelos + ensemble ponderado | 1 regla simple |
| Latencia | 3-10 segundos (IA + APIs) | Milisegundos (solo precios locales) |
| Dependencias | Kraken, CoinGecko, DeepSeek, OpenRouter | Solo Polymarket API |
| Costo | API calls ($) | Gratis |
| Mantenibilidad | 6 archivos, weights tuning | 1 archivo, 50 líneas |

## Implementación

```python
def evaluate(market, ctx):
    up_pct = up_price / (up_price + down_price)
    down_pct = down_price / (up_price + down_price)
    diff = abs(up_pct - down_pct)
    
    if diff < MIN_PCT_ADVANTAGE:
        return None  # Too close to call
    
    if up_pct > down_pct:
        edge = up_pct - 0.5
        return StrategyResult(name="majority_follower", edge=edge, ...)
    else:
        edge = -(down_pct - 0.5)
        return StrategyResult(name="majority_follower", edge=edge, ...)
```

## Limitaciones

- Si la mayoría está equivocada, la estrategia pierde.
- En mercados muy volátiles con switches frecuentes, los costos de spread pueden acumularse.
- Requiere `MIN_EDGE` bajo (recomendado `0.02`) para no filtrar señales válidas.
