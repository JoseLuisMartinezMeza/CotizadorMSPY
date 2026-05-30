# Bugs Conocidos y Mejoras Pendientes

## Resueltos ✅

### Merged Cells en Plantilla
- **Problema**: La plantilla Formato Cotización tiene muchas celdas combinadas que impiden escribir datos
- **Solución**: Deshacer (unmerge) las celdas combinadas en la zona de items (filas ≥15) antes de escribir
- **Estado**: ✅ Resuelto en `excel_generator.py`

### Encoding de Caracteres
- **Problema**: Caracteres especiales (tildes, ñ) se muestran mal en algunos outputs
- **Solución**: Usar UTF-8 en todos los archivos
- **Estado**: ✅ Parcialmente resuelto

## Pendientes 📝

### Falta API Key de Deepseek
- **Prioridad**: Alta
- **Descripción**: El motor de IA no funciona sin API key
- **Acción**: Configurar `DEEPSEEK_API_KEY` en archivo `.env`

### Imágenes en Cotización
- **Prioridad**: Media
- **Descripción**: Las imágenes de la plantilla no se copian al Excel generado
- **Acción**: Implementar copia de imágenes o placeholders

### Formato Condiciones Comerciales
- **Prioridad**: Baja
- **Descripción**: Las condiciones comerciales del pie de página no se personalizan
- **Acción**: Hacerlas configurables vía API

### Cloudflare Tunnel
- **Prioridad**: Media
- **Descripción**: Scripts creados pero no configurados
- **Acción**: Ejecutar `setup_cloudflare_tunnel.bat` y seguir instrucciones

### Tests Automatizados
- **Prioridad**: Media
- **Descripción**: No hay tests unitarios ni de integración
- **Acción**: Crear suite de tests con pytest
