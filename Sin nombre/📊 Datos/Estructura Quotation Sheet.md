# Estructura del Quotation Sheet (Entrada)

## Fuente
**SUNON TECHNOLOGY CO.,LTD.**
Building 6, Sunon Technological Innovation Park, 1666 Zhixing Road, Xiaoshan District, Hangzhou City, Zhejiang Province, China

## Formato del Excel

### Hoja: `Quotation`

#### Filas de encabezado
- Fila 1: Nombre del proveedor
- Fila 2: Dirección del proveedor
- Fila 4: `Project: {nombre}` | `REF:`
- Fila 5: `To: {destinatario}` | `DATE:`
- Fila 6: (vacía) | `PAGE:`
- Fila 7: **Header de columnas**

#### Columnas (Fila 7)
| Columna | Nombre | Descripción |
|---------|--------|-------------|
| A | No. | Número de item |
| B | Item Name | Código + nombre del producto |
| C | Photo | Imagen (no procesable) |
| D | Description | Descripción técnica completa |
| E | Dimension | Dimensiones (ej: 3,000*1,200*750 mm) |
| F | Color | Color/es |
| G | Q'ty | Cantidad |
| H | Vol. | Volumen unitario m³ |
| I | Tot.Vol. | Volumen total |
| J | Unit Price | Precio unitario USD (costo) |
| K | Tot.Price | Precio total |
| L | Remark | Observaciones |

#### Filas de Área
Las filas de área empiezan con `- ` y tienen el formato:
```
- NOMBRE DEL ÁREA  Tot.Price  : $XXXX.XX
```

Ejemplos de áreas:
- SALA DE JUNTAS ELITE
- SALA DE JUNTAS EJECUTIVA
- SALA INTERNACIONAL
- TT PHONEBOOTHS
- COWORK
- AREA DE CAFE
- AREA ADMVA

#### Items
Los items son las filas numéricas bajo cada área. Contienen:
- Código del producto (ej: `DUSXX.048006`)
- Descripción técnica extensa (múltiples líneas)
- Dimensiones
- Colores
- Cantidades
- Precios unitarios en USD

## Datos Clave del Proyecto de Ejemplo (IZA Monterrey)
- **Total áreas**: 7
- **Total items**: 33
- **Total costo**: $29,770.50 USD
- **Volumen total**: 41.115 m³
