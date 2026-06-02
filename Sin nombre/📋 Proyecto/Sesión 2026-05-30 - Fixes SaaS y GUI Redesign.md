---
tags:
  - mobiliti
  - saas
  - cotizador
  - sesion
  - build
  - pyinstaller
  - xlwings
  - tkinter
  - vercel
  - supabase
  - fix
fecha: '2026-05-30'
tipo: sesion-tecnica
estado: completado
---
# Sesión 2026-05-30 — Fixes SaaS y GUI Redesign

> Resumen compactado de toda la sesión de trabajo en el Cotizador Mobiliti SaaS. Incluye arquitectura actual, bugs resueltos, decisiones técnicas y estado final del build.

---

## Contexto del Sistema

| Componente | Detalle |
|---|---|
| **OS** | Windows 11 |
| **Python** | 3.14.4 |
| **Excel** | Instalado (requerido por xlwings) |
| **Build** | PyInstaller 6.20.0, single `.exe` (~95 MB) |
| **Backend** | FastAPI + Mangum en Vercel |
| **Database** | Supabase PostgreSQL |
| **Cliente** | Tkinter desktop con ttk.Style moderno |

---

## Arquitectura Actual

```
Cliente Windows (.exe)
  └── entry_point.py  → detecta --generate
      ├── GUI mode    → main_cliente.py (Tkinter + ttk)
      └── GEN mode    → generar_cotizacion_v5_xlwings.py (xlwings/COM)

Vercel Backend (Python/FastAPI)
  ├── /health
  ├── /login
  ├── /verificar-sesion
  ├── /generar-cotizacion
  └── /admin/*

Supabase PostgreSQL
  ├── saas_usuarios
  ├── saas_suscripciones
  └── saas_sesiones
```

### Flujo crítico: Excel generation en .exe
xlwings usa COM/Excel automation, el cual **falla en threads daemon de Tkinter**. La solución es que el `.exe` se relanza a sí mismo con `--generate <json_args>`, creando un proceso limpio sin Tkinter donde xlwings inicializa COM correctamente.

---

## Bugs Resueltos en esta Sesión

### 1. Supabase connection fix
- **Problema**: Proyecto original `amarztcyhgtszmwazxgl` pausado/eliminado. DNS no resolvía.
- **Fix**: Migrado a proyecto activo `MOBILITI Cotizador` (`hcdspekajlszcycecpml.supabase.co`).
- **Acción**: Actualizar Vercel env vars (`SUPABASE_URL`, `SUPABASE_SERVICE_KEY`) y redeployar.

### 2. Backend 500 fix
- **Problema**: `[Errno 16] Device or resource busy` (DNS fallaba al viejo Supabase).
- **Fix**: Backend usa `urllib.request` (sincrónico) para llamar Supabase REST API. Fallback a SQLite si Supabase devuelve 503.
- **Nota**: Evitar `supabase-py` async en serverless; mejor REST directo.

### 3. PyInstaller `datetime` NameError
- **Problema**: `from datetime import datetime` estaba dentro de `main()`, no al nivel del módulo.
- **Fix**: Mover import al top-level de `main_cliente.py`.

### 4. Template not found en .exe
- **Problema**: PyInstaller extrae archivos a `sys._MEIPASS` temporal, pero el template no se encontraba.
- **Fix**: `get_resource_path()` ahora busca en este orden:
  1. Directorio del `.exe` (permite archivos personalizados externos)
  2. `sys._MEIPASS` (archivos empaquetados internos)
  3. Project root (modo desarrollo)
- **Empaquetado**: Template incluido en ZIP junto al `.exe`.

### 5. Excel generation hanging en .exe
- **Problema**: xlwings/COM se congela al ejecutar desde un thread daemon de Tkinter.
- **Fix**: `entry_point.py` detecta `--generate` en `sys.argv`. Si está presente, parsea JSON args y llama `generar_cotizacion()` directamente sin importar Tkinter.
- **Comando subprocess** (desde GUI):
  ```python
  [sys.executable, "--generate", json.dumps(args_dict)]
  ```

### 6. OLE error `0xe0000002` al cerrar workbook
- **Problema**: `wb_template.close()` lanza `pywintypes.com_error` después de un `save()` exitoso. El archivo ya está generado; el error es solo al cerrar Excel.
- **Fix**: Envolver `wb_template.close()`, `wb_source.close()` y `app.quit()` en `try/except` individuales que imprimen advertencia en vez de tirar excepción fatal. Mover mensaje de éxito ANTES del cierre.
- **Archivo**: `generar_cotizacion_v5_xlwings.py`, líneas ~919-940.

---

## GUI Redesign (Tkinter Moderno)

Aplicado en `main_cliente.py` usando `ttk.Style` con tema `clam`:

| Elemento | Configuración |
|---|---|
| **Font base** | Segoe UI, 10pt |
| **Primary** | `#1a237e` (deep indigo) |
| **Primary light** | `#3949ab` |
| **Accent** | `#00acc1` (cyan) |
| **Background** | `#f8f9fa` (off-white) |
| **Surface** | `#ffffff` |
| **Text** | `#212529` |
| **Text secondary** | `#6c757d` |
| **Success** | `#2e7d32` |
| **Error** | `#c62828` |

- Botón primario: `Primary.TButton` (blanco sobre indigo, bold 11pt)
- Botón secundario: `Secondary.TButton` (gris sobre blanco)
- LabelFrame estilizado: `Card.TLabelframe` con borde sutil
- Pantallas: login con logo, main screen con formulario de generación, progress bar y log de consola

---

## Credenciales y URLs (Entorno Actual)

| Recurso | Valor |
|---|---|
| **API URL** | `https://verceldeploy-pied.vercel.app` |
| **Supabase URL** | `https://hcdspekajlszcycecpml.supabase.co` |
| **Admin email** | `proyectosjlmm@gmail.com` |
| **Admin password** | `Jose144267mz.2000` |
| **Suscripción activa** | Hasta 2036 (10 años) |

---

## Build y Distribución

### Comando de build
```bash
cd mobiliti_saas
python -m PyInstaller Mobiliti_SaaS.spec --noconfirm
```

### Contenido del ZIP (`Mobiliti_Generador_Windows.zip`)
- `Mobiliti_Generador.exe` (~95-108 MB)
- `config.json`
- `Formato Cotización 2026 GDL (1).xlsx`

### Generar ZIP (Python)
```python
import zipfile, os
files = [
    'mobiliti_saas/dist/Mobiliti_Generador.exe',
    'mobiliti_saas/config.json',
    'Formato Cotización 2026 GDL (1).xlsx'
]
with zipfile.ZipFile('Mobiliti_Generador_Windows.zip', 'w') as zf:
    for f in files:
        zf.write(f, os.path.basename(f))
```

---

## Notas Técnicas Críticas

1. **xlwings = Windows-only**: Requiere Excel instalado. No funciona en macOS/Linux.
2. **Proceso Excel persistente**: Si el script falla o se interrumpe, Excel puede quedar en segundo plano. El script ejecuta `taskkill /F /IM EXCEL.EXE` al inicio como mitigación.
3. **Protección de hojas**: `Mobiliti` se protege con password `M0b1l1t$`. `Cotizacion` NO se protege para permitir edición manual.
4. **Hidden import warning**: PyInstaller reporta `verificador` not found — es non-fatal, módulo puede estar sin usar.
5. **Template resolution priority**: `.exe` directory > `sys._MEIPASS` > project root. Esto permite que usuarios reemplacen el template simplemente colocando un archivo con el mismo nombre junto al `.exe`.

---

## Archivos Clave del Proyecto

```
ARMADO DE CARATULA/
├── generar_cotizacion_v5_xlwings.py   # Script principal xlwings
├── clasificador.py                     # Clasificador por diccionario
├── diccionario_categorias.json         # 13 categorías Mobiliti
├── test_clasificador.py                # 37 tests unitarios
├── LOGO.png                            # Logo corporativo
├── Formato Cotización 2026 GDL (1).xlsx # Template
├── mobiliti_saas/
│   ├── cliente/
│   │   ├── entry_point.py              # Entry point PyInstaller
│   │   ├── main_cliente.py             # GUI Tkinter
│   │   └── config.json                 # URL del API
│   ├── Mobiliti_SaaS.spec              # Spec PyInstaller
│   └── dist/
│       └── Mobiliti_Generador.exe      # Build final
└── Mobiliti_Generador_Windows.zip      # Distribución
```

---

## Cambios Adicionales Post-Sesión (2026-06-01)

### 7. Diccionario actualizado: términos lido/mall + pax + categoría renombrada
- **Términos agregados** a `Escritorios-WorkStation`:
  - `lido ejecutivo`, `lido izq`, `lido izquierdo`, `lido derecho` + typos (`liddo`, `izquiero`, `isquierdo`)
  - `mall ejecutivo`, `mall izq`, `mall izquierdo`, `mall derecho` + typos (`maal`)
  - `pax`, `plazas`, `usuarios`, `puestos`, `workstation`, `workstations`
- **Categoría renombrada**: `"Escritorios"` → `"Escritorios-WorkStation"`
- **Template Excel sincronizado**: `Fletes!I8` y `Fletes!M8` actualizados + script ahora auto-sincroniza en runtime.
- **`lido`** removido de `"Mesas de Juntas"` (es marca, no categoría).

### 8. Campo de descuento en GUI
- Nuevo campo **"Descuento %"** en `main_cliente.py` (default: 30)
- Parámetro `--descuento` en `generar_cotizacion_v5_xlwings.py`
- Fórmula: `factor = 1 - (descuento / 100)` → escrito en `Cotizacion!G{primera_fila_producto}`
- Ej: 30% → 0.7, 0% → 1.0, 50% → 0.5

### 9. Template Excel restaurado (corrupción por openpyxl)
- **Problema**: `openpyxl` corrompió `Formato Cotización 2026 GDL (1).xlsx` al modificar `Fletes!I8/M8`.
- **Fix**: Restaurado desde backup en `COTIZADOR AUTOMATICO/`. Script ahora actualiza Fletes en runtime vía xlwings, sin tocar el template con openpyxl.
- **Lección**: No usar `openpyxl.save()` en templates con imágenes WMF, validaciones de datos, o macros complejas.

---

## Próximos Pasos / TODO

- [x] Agregar términos lido/mall/pax al diccionario
- [x] Renombrar categoría "Escritorios" → "Escritorios-WorkStation"
- [x] Campo de descuento % en GUI
- [x] Restaurar template Excel corrupto
- [ ] Pulir `show_about()` para que no use `messagebox.showinfo` básico; hacer ventana custom con estilo visual consistente
- [ ] Agregar tests unitarios para el cliente desktop (mock del API)
- [ ] Considerar firma de código (code signing) para evitar alertas de Windows Defender en el `.exe`
- [ ] Evaluar migración de Supabase REST directo a `supabase-py` si el proyecto vuelve a ser auto-hospedado (no serverless)
