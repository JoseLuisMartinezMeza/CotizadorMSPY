# Control Panel (control.pyw)

GUI de escritorio principal del bot. Doble-click para abrir en Windows.

## Tecnología

- `tkinter` + `ttk` — Widgets nativos
- `matplotlib` (TkAgg) — Chart embebido
- Temas: dark mode manual

## Paneles

### Header
- Estado del bot (Running/Stopped)
- Botón Iniciar/Detener

### Stats
- Bankroll Total / Disponible / Asignado
- P&L Total
- Trades Hoy

### Chart BTC
- Precio BTC en tiempo real (Kraken 2s)
- Línea azul/roja: Precio spot
- Línea punteada horizontal: Target (precio de apertura)
- Línea punteada vertical: Inicio de ventana
- Label: Target + Spot + Diff %
- Timeframe: 1h / 15m

### Estrategias
Checkboxes: YOLO, Crypto Quant, Whale, Copy, BTC UP/DOWN Copy, AI Analysis, Crypto Feed, AI Exit Advisor

### Smart Copy Trading
- Wallets en watchlist
- Botón Descubrir Wallets
- Posiciones trackeadas

### AI Exit Advisor
- Estado (ACTIVO/INACTIVO)
- Stats: Sells, Buys, P&L acumulado

### Posiciones Activas
Tabla: Mercado, Outcome, Tamaño, Entrada, Actual, P&L $, P&L %
- Verde: TP >= +60%
- Rojo: SL <= -33%

### Historial de Trades
Tabla: Hora, Mercado, Acción, Tamaño, Estado

## Redibujado del chart

Thread-safe:
1. Thread poller: `_chart_poll_loop()` → fetch cada 2s
2. Main thread: `_schedule_chart_redraw()` → redraw cada 2s via `after()`

## Archivos de estado

- `gui_params.json` — Parámetros guardados
