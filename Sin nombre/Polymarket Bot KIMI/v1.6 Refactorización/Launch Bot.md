# Launch Bot — Lanzador Multiplataforma

**Archivo:** `launch_bot.py`

Reemplaza a `Lanzar Bot.bat` y `Lanzar Bot.vbs` con una solución Python puro que funciona en Windows, macOS y Linux.

## Uso

```bash
# Interactivo (pregunta modo)
python launch_bot.py

# Forzar simulación
python launch_bot.py --sim

# Forzar YOLO (trades reales)
python launch_bot.py --yolo

# Una sola iteración
python launch_bot.py --once

# Sin APIs de IA (gratis)
python launch_bot.py --mock-ai

# Modo consola (sin GUI)
python launch_bot.py --no-gui
```

## Parámetros

| Flag | Descripción |
|------|-------------|
| `--sim` | Forzar modo simulación |
| `--yolo` | Forzar modo YOLO (trades reales) |
| `--once` | Una sola iteración y salir |
| `--mock-ai` | Usar IA mock sin APIs externas |
| `--markets` | Mercados específicos (slug1,slug2) |
| `--no-gui` | Modo consola, sin tkinter |

## Lógica

1. Detecta el ejecutable de Python disponible (`python3`, `python`, `pythonw`)
2. Pregunta modo si no se especifica `--sim` o `--yolo`
3. Intenta lanzar `control.pyw` (GUI) si tkinter está disponible
4. Si no hay tkinter, cae a `bot.py` (modo consola)
5. Usa `os.execv` para reemplazar el proceso actual

## Ventajas sobre .bat / .vbs

- ✅ Multiplataforma (Windows, macOS, Linux)
- ✅ No hardcodea rutas de Python (`C:\Python314\pythonw.exe`)
- ✅ Detecta automáticamente el entorno
- ✅ Permite argumentos de línea de comandos
- ✅ Código legible y mantenible
