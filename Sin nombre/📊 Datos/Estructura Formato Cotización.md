# Estructura del Formato Cotización (Salida)

## Plantilla
**Formato Cotización 2026 GDL** - Plantilla oficial de Mobiliti

## Hoja Principal: `Cotizacion`

### Encabezado
- **Fila 3**: `COTIZACION #  ` + Número de cotización (formato: 100-XXXXX)
- **Fila 4**: Fecha

### Datos del Cliente
| Campo | Celda | Descripción |
|-------|-------|-------------|
| PROYECTO | B7 | Nombre del proyecto |
| Nombre | B8 | Nombre del contacto |
| Correo | B9 | Email |
| Teléfono | B10 | Teléfono |
| Dirección | B11 | Dirección de entrega |
| Razón Social | B12 | Razón social para facturación |

### Tabla de Items (Fila 15)
**Header (Fila 15):**
| Columna | Nombre |
|---------|--------|
| A | CÓDIGO |
| B | IMAGEN |
| C | DESCRIPCIÓN |
| D | MEDIDAS |
| E | CANT. |
| F | P. UNIT |
| G | % DESC. |
| H | DESCUENTO |
| I | SUBTOTAL |
| J | TOTAL |

### Totales
- **SUBTOTAL**: Suma de subtotales de items
- **COSTO DE FLETE**: Costo de envío
- **SUBTOTAL** (con flete): Subtotal + Flete
- **IVA**: 16% sobre subtotal con flete
- **TOTAL**: Subtotal con flete + IVA

### Condiciones Comerciales (Pie de página)
1. Moneda: USD
2. Tarifas arancelarias vigentes
3. Entrega en una sola etapa
4. Condiciones de pago: 60% anticipo + 20% contra aviso de embarque + 20% contra entrega
5. 3% mensual por mora

## Fórmulas de Negocio
```
P. Unit (venta) = Precio costo × Margen (default: 1.35)
Descuento = P. Unit × % Descuento
Subtotal item = (P. Unit - Descuento) × Cantidad
Subtotal general = Σ Subtotales items
IVA = (Subtotal + Flete) × 0.16
TOTAL = Subtotal + Flete + IVA
```
