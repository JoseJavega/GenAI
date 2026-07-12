---
name: genai-unresolved
description: "Identificar y resolver personas sin nombre mencionadas en documentos. Trigger: personas sin nombre, identificar personas, resolver desconocidos."
license: MIT
metadata:
  author: JoseJavega
  version: "1.0.0"
---

## Activation Contract

Carga este skill cuando el usuario quiera:
- Identificar personas mencionadas pero no completamente documentadas
- Resolver identidades de testigos o personas secundarias
- Investigar referencias a personas desconocidas

## Hard Rules

1. NO asumir identidad sin evidencia.
2. Registrar todas las posibles identidades encontradas.
3. Mantener el estado de cada persona (investigando/resuelta/descartada).
4. Buscar conexiones familiares o de comunidad.

## Decision Gates

| Situación | Acción |
|-----------|--------|
| Identidad confirmada | Mover a Family_Tree.md |
| Múltiples posibilidades | Mantener en Unresolved_Persons.md |
| Sin pistas | Marcar como `descartada` temporalmente |

## Execution Steps

1. **Leer Unresolved_Persons.md**: Identificar personas pendientes.
2. **Leer documentos fuente**: Buscar más contexto sobre cada persona.
3. **Buscar conexiones**: Testigos, vecinos, familiares.
4. **Cruzar información**: Con otros documentos del vault.
5. **Actualizar estado**: Marcar como resuelta o继续 investigación.

## Output Contract

```markdown
## Resolución de Personas

### Resueltas
| Persona | Identificación | Fuente |
|---------|---------------|--------|

### En Investigación
| Persona | Pistas | Próximos Pasos |
|---------|--------|----------------|

### Descartadas
| Persona | Motivo |
|---------|--------|
```
