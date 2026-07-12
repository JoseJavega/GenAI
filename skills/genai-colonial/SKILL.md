---
name: genai-colonial
description: "Buscar registros coloniales americanos para ancestros pre-1800. Trigger: registros coloniales, ancestros americanos, época colonial."
license: MIT
metadata:
  author: JoseJavega
  version: "1.0.0"
---

## Activation Contract

Carga este skill cuando el usuario quiera:
- Buscar ancestros en registros coloniales americanos
- Investigar épocas anteriores a 1800
- Encontrar inmigrantes coloniales

## Hard Rules

1. Verificar contexto colonial (fechas, lugares, naming conventions).
2. Considerar variaciones de nombres (inglés/español/indígena).
3. Usar fuentes especializadas en genealogía colonial.
4. Registrar incertidumbre.

## Decision Gates

| Situación | Acción |
|-----------|--------|
| Registro colonial verificado | Añadir con fuente colonial |
| Referencia indirecta | Marcar como `moderate` |
| Sin acceso a registros | Marcar como pendiente |

## Execution Steps

1. **Identificar línea colonial**: Familias con raíces pre-1800.
2. **Buscar en archivos**: PARES (Archivos de Indias), FamilySearch colonial.
3. **Cruzar fuentes**: Registros eclesiásticos, civiles, notariales.
4. **Actualizar vault**.

## Output Contract

```markdown
## Investigación Colonial

### Archivos Consultados
| Archivo | Período | Contenido |
|---------|---------|-----------|

### Registros Encontrados
| Ancestro | Registro | Fuente | Datos |
|----------|----------|--------|-------|
```
