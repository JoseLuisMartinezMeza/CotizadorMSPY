# Información del Repositorio

## nexu-io/open-design
- **URL**: https://github.com/nexu-io/open-design
- **Descripción**: Local-first, open-source Claude Design alternative
- **Skills**: 19 built-in, extendible
- **Design Systems**: 71 brand-grade
- **Licencia**: Apache-2.0 (inferida del contexto)

## Arquitectura
- **apps/web**: Next.js app (versión 0.8.0 en package.json)
- **apps/desktop**: Electron wrapper
- **apps/daemon**: Backend Node.js
- **packages/**: Shared packages (contracts, sidecar, etc.)
- **skills/**: Skill definitions (markdown-based)
- **design-systems/**: Design system definitions

## Versión del Usuario vs Repo
| Componente | Versión Usuario | Versión Repo |
|-----------|----------------|--------------|
| Desktop | 41.3.0 | ? |
| Web | ? | 0.8.0 |

## Notas
- El usuario tiene un backup de la versión release-stable-win del 2026-05-14
- La app está instalada en `AppData/Roaming/@open-codesign/desktop/`

## Problemas Conocidos en Upstream (issues abiertos relevantes)
- Issue #2801: "Source install (pnpm tools-dev) is broken end-to-end" - indica que la instalación desde fuente tiene problemas
- Issue #2794: "Inline Text Editing in the Edit Tab would be a dream in 0.9.0"
- Varios PRs relacionados con fixes de UI, MCP, y runtimes
