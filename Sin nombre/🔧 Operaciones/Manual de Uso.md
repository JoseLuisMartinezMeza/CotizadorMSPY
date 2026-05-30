# Manual de Uso - Cotizador Mobiliti Agent

## Requisitos Previos
- Python 3.11+ instalado
- API key de Deepseek (opcional pero recomendado)
- Archivo Quotation Sheet del proveedor (.xlsx)

## Instalación

### 1. Clonar/Descargar el proyecto
```bash
cd cotizador-agente
```

### 2. Configurar variables de entorno
```bash
copy .env.example .env
```
Edita `.env` y añade:
```env
DEEPSEEK_API_KEY=sk-tu-api-key-aqui
```

### 3. Iniciar el servidor
Doble click en `start.bat` o:
```bash
python -m uvicorn src.main:app --host 0.0.0.0 --port 8000
```

## Uso vía API

### Generar una cotización
```bash
curl -X POST "http://localhost:8000/api/v1/cotizaciones/generar" \
  -F "quotation_file=@tu_quotation.xlsx" \
  -F "proyecto=Nombre del Proyecto" \
  -F "nombre=Nombre del Cliente" \
  -F "correo=cliente@ejemplo.com" \
  -F "telefono=5551234567" \
  -F "direccion=Dirección de entrega" \
  -F "razon_social=Razón Social SA de CV" \
  -F "margen=1.35" \
  -F "descuento_global=0" \
  -F "costo_flete=0"
```

### Descargar la cotización
```bash
curl "http://localhost:8000/api/v1/cotizaciones/download?filename=Cotizacion_100_XXXXX.xlsx" \
  --output cotizacion.xlsx
```

## Uso vía MCP Server

### Iniciar MCP Server
```bash
python -m src.mcp_server
```

### Herramientas disponibles

#### parse_quotation_sheet
Extrae la estructura de datos de un Quotation Sheet.

#### generate_cotizacion
Genera una cotización completa y devuelve la ruta al archivo.

#### list_cotizaciones
Lista las cotizaciones generadas recientemente.

#### get_cotizacion_details
Obtiene información de una cotización específica.

## Configuración de Márgenes

| Tipo de Producto | Margen Recomendado |
|------------------|-------------------|
| Mobiliario estándar | 1.35 (35%) |
| Mobiliario premium | 1.25 (25%) |
| Accesorios | 1.50 (50%) |

## Solución de Problemas

### Error: "API Key inválida"
Verifica que `DEEPSEEK_API_KEY` esté configurada en `.env`.

### Error: "Archivo no encontrado"
Asegúrate de que la ruta al Quotation Sheet sea correcta.

### Los precios no coinciden
Verifica que el margen aplicado sea el correcto. Default: 1.35.
