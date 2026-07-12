---
name: genai-open-questions
description: "Atacar sistemáticamente todas las preguntas abiertas de investigación. Trigger: preguntas abiertas, resolver dudas, investigación pendiente."
license: MIT
metadata:
  author: JoseJavega
  version: "1.0.0"
---

## Activation Contract

Carga este skill cuando el usuario quiera:
- Resolver preguntas pendientes en Open_Questions.md
- Atacar sistemáticamente la investigación pendiente
- Priorizar qué investigar primero

## Hard Rules

1. Seguir el orden de prioridad en Open_Questions.md.
2. Cada pregunta resuelta debe tener fuente.
3. Si no se resuelve, mantener como pendiente con nuevas pistas.
4. Registrar todo en Research_Log.md.

## Decision Gates

| Situación | Acción |
|-----------|--------|
| Pregunta resuelta con fuente | Mover a Family_Tree.md |
| Pregunta parcialmente resuelta | Actualizar en Open_Questions.md |
| Sin resolución | Mantener con nuevas pistas |

## Execution Steps

1. **Leer Open_Questions.md**: Identificar preguntas por prioridad.
2. **Seleccionar pregunta**: Empezar por alta prioridad.
3. **Investigar**: Búsquedas web, archivos, cruces de datos.
4. **Evaluar**: ¿Se resolvió? ¿Con qué confianza?
5. **Actualizar vault**: Mover resueltas, actualizar pendientes.

## Output Contract

```markdown
## Resolución de Preguntas

### Resueltas
| Pregunta | Respuesta | Fuente |
|----------|-----------|--------|

### En Investigación
| Pregunta | Pistas Encontradas | Próximos Pasos |
|----------|-------------------|----------------|

### Estadísticas
- Resueltas: [n]
- Pendientes: [n]
- Nuevas: [n]
```
