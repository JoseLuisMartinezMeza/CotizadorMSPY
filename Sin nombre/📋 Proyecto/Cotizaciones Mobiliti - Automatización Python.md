---
title: Cotizaciones Mobiliti - Automatización Python
tags:
  - python
  - excel
  - xlwings
  - automatizacion
  - cotizaciones
  - mobiliti
  - clasificador
  - fuzzy-matching
fecha: '2025-05-30'
estado: completado
---
# Cotizaciones Mobiliti - Automatización Python

> Proyecto de automatización de cotizaciones comerciales para Mobiliti. Transforma archivos Excel de proveedor (Quotation Sheet) en cotizaciones profesionales con dos hojas: **Cotizacion** (cliente) y **Mobiliti** (uso interno).

---

## Stack Tecnológico

| Librería | Uso |
|----------|-----|
| `xlwings` | Automatización nativa de Excel (formatos, imágenes, protección, PrintArea) |
| `openpyxl` | Lectura estructural del source y extracción de imágenes |
| `rapidfuzz` | Fuzzy matching para clasificación de productos (typos, sinónimos) |
| `zipfile` | Desprotección del template sin password |
| `xml.etree.ElementTree` | Parseo de XML de drawings para mapear imágenes |

---

## Estructura del Proyecto (Limpia)

### Archivos Vitales (raíz)

| Archivo | Rol |
|---------|-----|
| `generar_cotizacion_v5_xlwings.py` | Script principal (pipeline de 8 pasos) |
| `LOGO.png` | Logo insertado en encabezado de Cotizacion |
| `Formato Cotización 2026 GDL (1).xlsx` | Template principal (hojas Cotizacion + Mobiliti) |
| `clasificador.py` | Módulo de clasificación de productos |
| `diccionario_categorias.json` | Diccionario editable de categorías y términos |
| `test_clasificador.py` | Tests unitarios del clasificador |
| `KIVO BRAVANTE-Quotation Sheet - V1.xlsx` | Input de ejemplo (fuente) |

### Archivos movidos a `historial/`

- ~30 scripts de debug (`verificar_*.py`, `check_*.py`, `debug_*.py`)
- 12 archivos Excel de prueba (`TEST_*_Cotizacion.xlsx`)
- 4 screenshots de debug (`TEST_*.png`)

---

## Pipeline de 8 Pasos

```
[0] Desproteger template (zipfile)
[1] Extraer imágenes del source (XML parsing)
[2] Leer items de Quotation (categorías + productos)
[3] Iniciar Excel vía xlwings
[4] Abrir template y source + agregar categorías nuevas a lista Mobiliario
[5] Copiar hoja Quotation al template
[6] Llenar encabezado Cotizacion + insertar logo
[7] Generar Mobiliti (clasificación automática en columna E)
[8] Generar Cotizacion (items, imágenes, totales, términos)
```

---

## Cambios Implementados

### 1. Bug Corregido: Typo en import

```python
# Antes (error de sintaxis)
imSport tempfile

# Después (corregido)
import tempfile
```

### 2. Columna A de Mobiliti no se modifica

El usuario solicitó que la **columna A de Mobiliti quede exactamente igual al template**. Se eliminaron 3 bloques de código que modificaban la columna A:

- Escritura de fórmula de categoría en `A{cat_row}`
- Ajuste de fórmulas de A en productos (loop principal)
- Ajuste de A en todas las filas de producto (post-loop)

### 3. Clasificador de Productos (Columna E)

Sistema híbrido: **matching exacto por substring** + **fuzzy matching fallback** con `rapidfuzz`.

#### Categorías del Template (lista Mobiliario)

| # | Categoría | Tipo |
|---|-----------|------|
| 1 | Silla | Original |
| 2 | Mesas de Apoyo | Original |
| 3 | Escritorios | Original |
| 4 | Sillones | Original |
| 5 | Mesas de Juntas | Original |
| 6 | Librero - Locker - Gabinete | Original |
| 7 | Archiveros Moviles y Fijos | Original |
| 8 | Phonebooths | Original |
| 9 | Multicontactos | Original |
| 10 | Terminados | Original |
| 11 | **Bancos** | 🆕 Agregada |
| 12 | **Cocineta** | 🆕 Agregada |
| 13 | **Pizarrones** | 🆕 Agregada |

#### Correcciones de Clasificación

| Producto | Antes | Después |
|----------|-------|---------|
| Sala de estar / F80 Lounge Sofas | Mesas de Juntas | **Sillones** ✅ |
| Modit Whiteboard | Terminados | **Pizarrones** ✅ |
| Ducky Stool | Terminados | **Bancos** ✅ |
| cocinetas sunon | Terminados | **Cocineta** ✅ |
| Aveza Task Chair | Terminados/Silla | **Silla** ✅ |
| I-Key Reception Desk | Terminados | **Escritorios** ✅ |

#### Sinónimos y Variaciones Soportadas

- **Typos**: `sillla operativa` → Silla, `escrittorio` → Escritorios
- **Acentos**: `sofá` → Sillones, `sillón` → Sillones
- **Inglés**: `task chair` → Silla, `whiteboard` → Pizarrones, `kitchenette` → Cocineta
- **Nombres de modelo**: `aveza`, `caz83sw`, `f80`, `moji`, `dm24`, `dg65`

---

## Cómo ejecutar

```bash
cd "ARMADO DE CARATULA"

# Instalar dependencias
pip install xlwings openpyxl rapidfuzz

# Ejecutar
python generar_cotizacion_v5_xlwings.py \
    --source "KIVO BRAVANTE-Quotation Sheet - V1.xlsx" \
    --template "Formato Cotización 2026 GDL (1).xlsx" \
    --cotizacion "COT-2025-001" \
    --proyecto "Nombre del Proyecto" \
    --cliente "Nombre del Cliente" \
    --correo "cliente@email.com" \
    --telefono "81-1234-5678" \
    --direccion "Dirección" \
    --razon_social "Razón Social S.A. de C.V."
```

### Tests

```bash
python -m pytest test_clasificador.py -v
```

---

## Editar Categorías

Editar `diccionario_categorias.json`:

```json
{
  "categorias": {
    "NUEVA CATEGORIA": {
      "score": 90,
      "terminos": [
        "palabra clave",
        "sinonimo",
        "keyword en ingles"
      ]
    }
  }
}
```

No requiere modificar código. El script recarga el JSON en cada ejecución.

---

## Constantes Importantes

```python
Q_HEADER_ROW = 7
section_cats = [13, 48, 83, 118, 153, 188, 223, 258, 293, 328]
section_prod_starts = [14, 49, 84, 119, 154, 189, 224, 259, 294, 329]
max_prod_per_section = 32
```

---

## Riesgos Conocidos

| Riesgo | Mitigación |
|--------|-----------|
| `limpiar_excel()` mata **todos** los procesos Excel | Usar con precaución si hay otros Excel abiertos |
| Dependencia de Windows + Excel instalado | No portable a Linux/Mac |
| Template acoplado a filas fijas | Externalizar a `config.json` en futuro |
| Password hardcoded | `"M0b1l1t$"` en código — no crítico pero mejorable |

---

## Historial de Cambios

| Fecha | Cambio |
|-------|--------|
| 2025-05-30 | Limpieza: archivos no vitales movidos a `historial/` |
| 2025-05-30 | Bugfix: typo `imSport tempfile` → `import tempfile` |
| 2025-05-30 | Refactor: columna A de Mobiliti ya no se modifica |
| 2025-05-30 | Feature: clasificador de productos JSON + fuzzy matching |
| 2025-05-30 | Feature: 3 categorías nuevas (Bancos, Cocineta, Pizarrones) |
| 2025-05-30 | Feature: sinónimos ES/EN con tolerancia a typos y acentos |

---

## Enlaces Relacionados

- [[Arquitectura]]
- [[Visión General]]
