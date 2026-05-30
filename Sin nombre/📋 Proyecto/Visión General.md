# Cotizador Mobiliti Agent

## Descripción
Agente de IA autónomo para generar cotizaciones comerciales a partir de Quotation Sheets de proveedores (principalmente SUNON Technology).

## Objetivo
Automatizar el proceso de generación de cotizaciones comerciales en formato Mobiliti, reduciendo el tiempo de preparación de 2-3 horas a menos de 1 minuto.

## Stack Tecnológico
- **Backend**: Python 3.11 + FastAPI
- **Procesamiento Excel**: openpyxl + pandas
- **IA**: Deepseek API (deepseek-chat)
- **Memoria/Estado**: SQLite
- **MCP Server**: Para integración con agentes de IA
- **Despliegue**: Local + Cloudflare Tunnel (24/7)

## Estado
- [x] Parser de Quotation Sheet
- [x] Generador de Formato Cotización
- [x] Cálculos de precios (margen, descuento, IVA)
- [x] API REST (FastAPI)
- [x] MCP Server
- [ ] Integración Deepseek (requiere API key)
- [ ] Cloudflare Tunnel configurado
- [ ] Tests completos

## Archivos del Proyecto
Ubicación: `C:\Users\pepem\Downloads\COTIZADOR AUTOMATICO\cotizador-agente`
