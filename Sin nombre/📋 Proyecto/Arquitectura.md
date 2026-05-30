# Arquitectura del Cotizador Mobiliti Agent

## Diagrama de Flujo

```
Quotation Sheet (SUNON)
    ↓
[API REST / MCP Server]
    ↓
┌─────────────────────────────────────────────────────┐
│  MODO INTEGRADO (default)                           │
│  1. Copiar Formato Cotización como base             │
│  2. Incrustar Quotation como hoja "Quotation"       │
│  3. Configurar fórmulas en "Cotizacion":            │
│     • Código:     ='Quotation'!B{row}               │
│     • Descripción:='Quotation'!D{row}               │
│     • Medidas:    ='Quotation'!E{row}               │
│     • Cantidad:   ='Quotation'!G{row}               │
│     • Precio:     ='Quotation'!J{row}*1.35          │
│  4. Calcular totales con fórmulas                   │
└─────────────────────────────────────────────────────┘
    ↓
Archivo Excel con múltiples hojas vinculadas
```

## Estructura del Excel Generado (Modo Integrado)

```
📄 Cotizacion_100-XXXXX.xlsx
├── 📋 Cotizacion          ← Carátula con fórmulas referenciando Quotation
├── 🐑 sheep               ← Price List
├── 📊 Mobiliti            ← Cálculos de precios internos
├── 📈 Estrategia Comercial ← Configuración de descuentos
├── 🚚 Fletes              ← Cálculo de fletes
├── 🏢 Proveedores         ← Lista de proveedores con márgenes
├── 📐 SPEC LAMINADO JOME  ← Especificaciones técnicas
├── 📐 SPEC-GUIDE-LUMBRO   ← Especificaciones técnicas
├── 📐 SPEC-GUIDE ESTRUCTURAS ← Especificaciones técnicas
├── 📐 Spec Guide Estructura    ← Especificaciones técnicas
├── 📐 SPEC-GUIDE-CR GLOBAL     ← Especificaciones técnicas
├── 💳 Meses Sin Intereses Tarjetas ← Financiamiento
└── 📦 Quotation           ← ⭐ Hoja del proveedor incrustada
```

## Ventajas del Modo Integrado

1. **Fórmulas vinculadas**: Cambia el precio en Quotation y la cotización se actualiza
2. **Mantiene el formato original**: Preserva el diseño corporativo de Mobiliti
3. **Incluye especificaciones**: Todas las hojas de specs del Formato Cotización
4. **Editable**: El usuario puede modificar datos en Quotation sin romper la cotización

## Tecnologías

| Componente | Tecnología |
|------------|-----------|
| Backend | Python 3.11 + FastAPI |
| Excel | openpyxl |
| IA | Deepseek API (deepseek-chat / deepseek-v4-pro) |
| Protocolo | MCP (Model Context Protocol) |
| Despliegue | Local + Cloudflare Tunnel |
