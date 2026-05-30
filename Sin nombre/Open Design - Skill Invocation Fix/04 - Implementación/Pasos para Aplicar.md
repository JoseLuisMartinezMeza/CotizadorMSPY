# Pasos para Aplicar el Fix

## Opción A: Ejecutar desde código fuente (Recomendada)

### Requisitos
- Node.js 24 (descargar de https://nodejs.org)
- Git
- PNPM (`npm install -g pnpm`)
- Visual Studio Build Tools 2022 (solo Windows, para compilar better-sqlite3)

### Pasos

1. **Clonar el repositorio**:
```bash
git clone https://github.com/nexu-io/open-design.git
cd open-design
```

2. **Aplicar el patch**:
```bash
git apply chat-composer-fix.patch
```

3. **Instalar dependencias**:
```bash
pnpm install
```

4. **Iniciar en modo desarrollo**:
```bash
pnpm tools-dev
```

Esto abrirá Open Design en el navegador (usualmente http://localhost:17573) con el fix aplicado.

## Opción B: Recompilar la app Desktop

Si quieres una versión empaquetada (.exe) con el fix:

1. Seguir los pasos 1-3 de la Opción A
2. Ejecutar:
```bash
pnpm tools-pack
```
3. El instalador se generará en el directorio de salida configurado

## Opción C: Parchear instalación existente (AVANZADO / NO RECOMENDADO)

La app instalada tiene código JavaScript compilado y minificado en:
```
%LOCALAPPDATA%\Programs\Open Design release-stable-win\resources\open-design-web-standalone\apps\web\.next\static\chunks\
```

Parchear estos archivos es extremadamente frágil porque el código está minificado y dividido en chunks. No se recomienda a menos que sea absolutamente necesario.

## Verificación del Fix

Después de aplicar el fix, al escribir en el composer:

1. Escribe `/` al inicio o después de un espacio
2. Debería aparecer un popover con:
   - Comandos del sistema (`/mcp`, `/search`, `/pet`, `/hatch`)
   - **Todos tus skills** (`/agent-browser`, `/blog-post`, `/critique`, etc.)
3. Al seleccionar un skill, se inserta `@skill` en el texto
4. Presiona Enter para enviar con el skill activo
