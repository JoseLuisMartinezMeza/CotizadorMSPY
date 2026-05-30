# Hallazgos del Análisis

## Archivos Examinados
- `apps/web/src/components/ChatComposer.tsx` — Componente principal del composer de chat

## Problema Raíz Identificado (3 causas)

### 1. Slash commands solo funcionan al INICIO del texto
**Línea original**: `const slashMatch = /^\/([^\s/]*)$/.exec(before);`

El regex `^\/([^
/]*)$` requiere que `/` sea el primer carácter del texto (`^`). Si el usuario escribe algo primero y luego quiere usar `/`, no funciona.

**Ejemplo**:
- ✅ `/search algo` → funciona
- ❌ `hazme un logo /search referencias` → NO funciona

### 2. Los SKILLS no están en el slash command picker
Los slash commands hardcodeados solo incluyen:
- `/mcp` (si hay MCP configurado)
- `/search` (si research está disponible)
- `/pet`, `/hatch` (si pets están habilitados)

Los **skills** del proyecto (los 19+ skills de open-design) solo se invocan vía `@` (mention picker), NO vía `/`.

Esto es confuso para usuarios que vienen de Claude Code u otras apps donde `/` invoca skills.

### 3. No hay skills en el popover de `/`
Aunque el usuario escriba `/` correctamente al inicio, no verá sus skills en el picker — solo verá los comandos hardcodeados (mcp, search, pet).

## Cómo Funciona el Mention Picker (@)
- `@` sí funciona en cualquier posición (regex: `/(^|\s)@([^\s@]*)$/`)
- Abre un popover con pestañas: All, Plugins, Skills, MCP, Connectors, Files
- Los skills aparecen bajo la pestaña "Skills"

## Conclusión
El usuario espera que `/` funcione como en Claude Code: invocar skills. En Open Design, el sistema de `/` está diseñado solo para "comandos de sistema" (mcp, search, pet), mientras que los skills usan `@`.

**Fix propuesto**:
1. Relajar regex de `/` para que funcione después de espacio también
2. Agregar todos los skills al catálogo de slash commands
3. Cuando se selecciona un skill vía `/`, insertar `@skill` como mention
