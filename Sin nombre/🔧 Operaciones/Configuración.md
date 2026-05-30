# Configuración del Cotizador Agent

## Variables de Entorno (.env)

### Deepseek API
| Variable | Valor | Requerido |
|----------|-------|-----------|
| `DEEPSEEK_API_KEY` | sk-ef3e50214c97441b8b7370372de394d1 | ✅ Configurado |
| `DEEPSEEK_MODEL` | deepseek-chat | No |
| `DEEPSEEK_BASE_URL` | https://api.deepseek.com | No |

**Modelos disponibles:**
| Modelo | Velocidad | Uso recomendado |
|--------|-----------|-----------------|
| `deepseek-chat` | ~18s/33 items | Default, cotizaciones rápidas |
| `deepseek-v4-pro` | ~3min/33 items | Análisis complejo, validación |

### Negocio
| Variable | Valor | Descripción |
|----------|-------|-------------|
| `MARGEN_DEFAULT` | 1.35 | Multiplicador sobre costo (35% markup) |
| `IVA_TASA` | 0.16 | 16% IVA México |
| `MONEDA` | USD | Moneda de cotización |

### Seguridad
| Variable | Valor | Descripción |
|----------|-------|-------------|
| `API_KEY` | cambiar-esta-clave-por-una-segura | Protección de endpoints (opcional) |

## Directorios
```
templates/     → Plantilla Formato Cotización
 data/input/   → Quotation Sheets subidos
data/output/   → Cotizaciones generadas
data/db/       → SQLite database
```

## Puertos
- **API REST**: 8000 (o 8001 si está ocupado)
- **Documentación**: http://localhost:8000/docs

## Deepseek API

### Obtener API Key
1. Ve a https://platform.deepseek.com/
2. Regístrate / Inicia sesión
3. Ve a "API Keys"
4. Crea una nueva key

### Costos Aproximados
- deepseek-chat: ~$0.14 por 1M tokens de entrada
- Una cotización típica (33 items) consume ~10K-15K tokens
- Costo estimado por cotización: ~$0.01-0.05 USD

## Cloudflare Tunnel (24/7)

### Instalación
```bash
# Descargar cloudflared.exe (ya incluido en el proyecto)
# O desde: https://github.com/cloudflare/cloudflared/releases
```

### Configuración (paso a paso)
1. Ejecuta `configure_cloudflare_tunnel.bat`
2. Sigue las instrucciones (login en navegador)
3. Crea el túnel con nombre deseado
4. Configura el hostname en el dashboard de Cloudflare

### Iniciar modo 24/7
```bash
start_24_7.bat
```
Esto inicia:
- API en localhost:8000
- Cloudflare tunnel conectado

### Alternativa rápida: ngrok
```bash
# Descarga ngrok desde https://ngrok.com/download
# Regístrate y configura tu authtoken
start_with_ngrok.bat
```
