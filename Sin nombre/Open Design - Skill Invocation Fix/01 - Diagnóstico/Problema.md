# Problema: Slash Command "/" No Funciona en Composer

## Síntomas
- Usuario abre Open Design desktop v41.3.0
- Intenta escribir "/" en el chat composer para invocar skills
- El skill picker / slash command menu no aparece

## Hipótesis Iniciales
1. **Evento de teclado bloqueado**: El componente textarea/input no escucha el evento `"/"` correctamente
2. **Focus issue**: El composer no tiene el foco adecuado cuando se presiona "/"
3. **Versión desactualizada**: La versión 41.3.0 puede tener un bug conocido ya fixeado en main
4. **Configuración regional**: El layout de teclado o locale puede interferir
5. **Desktop-specific**: El wrapper Electron puede estar interceptando la tecla "/"

## Información del Sistema
- OS: Windows
- App: Open Design release-stable-win
- Versión: 41.3.0
- Ubicación install: `AppData/Roaming/@open-codesign/desktop/`
- Backup disponible: `Downloads/KIMI OPEN DESING/backup-open-design-20260514-175635`

## Próximos Pasos
- [ ] Obtener código fuente del composer/slash handler
- [ ] Identificar el bug específico
- [ ] Validar si existe issue reportado en upstream
- [ ] Preparar patch o workaround
