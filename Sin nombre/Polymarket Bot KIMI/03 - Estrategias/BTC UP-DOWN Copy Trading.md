# BTC UP-DOWN Copy Trading

Sistema de copy trading especializado para mercados BTC UP/DOWN de 15 minutos.

## Cómo funciona

1. **Descubrimiento automático** de top traders
2. **Monitoreo** de sus posiciones cada iteración
3. **Detección** de cambios (ENTER, EXIT, SWITCH, INCREASE, DECREASE)
4. **Validación por IA** antes de copiar (DeepSeek)
5. **Ejecución** con tamaño ajustado

## Descubrimiento de wallets

```python
discover_top_traders(market, limit=20)
├── /holders?token={token_id}&limit=20
└── /trades?slug={slug}&limit=200
```

Endpoints públicos de `data-api.polymarket.com` — no requieren auth.

## Acciones detectadas

| Acción | Descripción | Validación IA |
|--------|-------------|---------------|
| `ENTER_UP` | Nueva posición UP | Sí |
| `ENTER_DOWN` | Nueva posición DOWN | Sí |
| `SWITCH` | Cambió de dirección | Sí |
| `INCREASE` | Aumentó >50% | Sí |
| `DECREASE` | Disminuyó >50% | Sí |
| `EXIT` | Cerró posición | No (cierra directo) |

## Validación por IA

El agente valida antes de copiar:
- Dirección del momentum de BTC
- Minutos restantes en ventana (>3)
- Precio favorable (<0.70)
- Confianza mínima: `0.55`

## Tamaño de posición

```python
size = min(
    bankroll * BTC_UPDOWN_COPY_SIZE_PCT,  # 5% default
    BTC_UPDOWN_MAX_COPY_SIZE,              # $50 default
    bankroll * 0.5,                        # Max 50% del bankroll
)
```

## Archivos

- `btc_updown_copytrading.py` — Implementación
- `bot/runner.py` — Integración en el ciclo
