# DeepSeek API

Proveedor de IA principal del bot.

## Configuración

```env
AI_PROVIDER=deepseek
AI_MODEL=deepseek-chat
DEEPSEEK_API_KEY=sk-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

## Modelos soportados

| Modelo | Uso |
|--------|-----|
| `deepseek-chat` | Análisis de mercados (default) |
| `deepseek-reasoner` | Razonamiento más profundo |

## Prompts del sistema

### 1. SYSTEM_PROMPT
Para análisis general de mercados. Agresivo, busca edge real.

### 2. COPY_TRADING_SYSTEM_PROMPT
Para validar señales de copy trading. Acepta/Rechaza basado en contexto.

### 3. EXIT_ADVISOR_PROMPT
Para decidir si mantener o cerrar posiciones abiertas.

## Temperatura

- Análisis de mercados: `0.3`
- Validación copy trading: `0.25`
- Exit advisor: `0.25`

## Retry

3 intentos con backoff exponencial (2s - 10s).

## Endpoints

- Base URL: `https://api.deepseek.com/v1`
- Compatible con OpenAI SDK
- `/chat/completions` — Chat completions

## Fallback

Si DeepSeek falla, el bot usa `OPENROUTER_API_KEY` como fallback:
- Base URL: `https://openrouter.ai/api/v1`
- Extra headers: `HTTP-Referer`, `X-Title`
