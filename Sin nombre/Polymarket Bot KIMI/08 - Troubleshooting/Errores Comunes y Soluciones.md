# Errores Comunes y Soluciones

## 401 Unauthorized — Polymarket CLOB API

**Síntoma:**
```
HTTP/2 401 Unauthorized
{"error":"Unauthorized/Invalid api key"}
```

**Causa:** Credenciales API inválidas o vencidas.

**Solución:**
1. Ve a https://polymarket.com/portfolio → API Keys
2. Genera nuevas credenciales
3. Actualiza tu `.env`

## 400 Could not create api key

**Síntoma:**
```
HTTP/2 400 Bad Request
{"error":"Could not create api key"}
```

**Causa:** Sin MATIC/POL para gas.

**Solución:** Deposita MATIC en tu wallet o usa credenciales existentes.

## Modo SIMULACIÓN sin querer

**Síntoma:**
```
[SIMULACION] No se ejecuta trade real
```

**Causas:**
1. `YOLO_MODE=false`
2. `CONFIG.can_trade=false`
3. Balance < $1.00
4. Error de inicialización CLOB

## AI Exit Advisor no consume tokens

**Síntoma:** No se ven llamadas a DeepSeek.

**Causas:**
1. `ENABLE_AI_EXIT_ADVISOR=false`
2. No hay posiciones abiertas
3. `MOCK_AI_MODE=true`

## Target del chart no se actualiza

**Síntoma:** Target muestra `--` o valor incorrecto.

**Causa:** Bug de timezone (UTC vs local).

**Solución:** Ya arreglado. Reinicia `control.pyw`.

## Error de IA: 404 Rust bridge

**Síntoma:**
```
Error consultando nodo Rust: 404
```

**Causa:** Bridge Rust no está corriendo.

**Solución:** No crítico. El bot usa DeepSeek como fallback.

## 'LivePosition' object has no attribute 'entry_price'

**Síntoma:**
```
AttributeError: 'LivePosition' object has no attribute 'entry_price'
```

**Causa:** `LivePosition` usa `avg_price` en lugar de `entry_price`.

**Solución:** Ya arreglado.

## Diagnóstico rápido

```bash
cd POLIMARKET-BOT
python -c "
from config import CONFIG
from trading_engine import trading_engine

print('yolo_mode:', CONFIG.yolo_mode)
print('can_trade:', CONFIG.can_trade)
print('mock_ai_mode:', CONFIG.mock_ai_mode)
print('enable_ai_exit_advisor:', CONFIG.enable_ai_exit_advisor)
print('simulation_mode:', trading_engine.simulation_mode)
print('positions:', len(trading_engine.positions))
"
```
