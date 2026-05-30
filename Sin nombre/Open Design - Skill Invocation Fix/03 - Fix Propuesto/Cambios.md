# Fix Propuesto — Skill Invocation via "/"

## Resumen de Cambios
Archivo: `apps/web/src/components/ChatComposer.tsx`

### Cambio 1: Relajar regex de detección de `/`
**Antes**:
```typescript
const slashMatch = /^\/([^\s/]*)$/.exec(before);
```

**Después**:
```typescript
const slashMatch = /(?:^|\s)\/([^\s/]*)$/.exec(before);
```

Esto permite que `/` funcione tanto al inicio del texto como después de un espacio.

### Cambio 2: Agregar skills al catálogo de slash commands
En la función `slashCommands`, después de los comandos hardcodeados (mcp, search, pet), agregar un loop que mapee cada skill disponible a un slash command:

```typescript
for (const skill of skills) {
  list.push({
    id: `skill-${skill.id}`,
    label: `/${skill.id}`,
    insert: `${inlineMentionToken(skill.id)} `,
    desc: skill.description || localizeSkillName(locale, skill),
    icon: 'sparkles',
  });
}
```

Cuando el usuario selecciona un skill vía `/`, se inserta `@skill` en el draft (como si hubiera usado el mention picker).

### Cambio 3: Ajustar `pickSlash` para reemplazo correcto
**Antes**:
```typescript
const replaced = before.replace(/\/[^\s/]*$/, cmd.insert);
```

**Después**:
```typescript
const replaced = before.replace(/(^|\s)\/[^\s/]*$/, (_match, leading) => leading + cmd.insert);
```

Esto maneja tanto `/foo` al inicio como `bar /foo` después de espacio.

### Cambio 4: Extender interfaz `SlashCommand` para descripciones directas
Agregar campo opcional `desc` que bypass i18n:
```typescript
desc?: string;
```

Y en `SlashPopover`, usar `cmd.desc ?? t(cmd.descKey)` para mostrar la descripción.

### Cambio 5: Importar `locale` en el componente
Agregar `const { locale } = useI18n();` en el cuerpo de `ChatComposer` para poder localizar los nombres de skills.

## Diff Completo
Ver archivo adjunto: `chat-composer-fix.patch`
