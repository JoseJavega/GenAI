---
name: genai-tree-expansion
description: "Expandir árbol genealógico con fuentes verificadas para ancestros fallecidos. Trigger: expandir árbol, buscar ancestros, añadir familia."
license: MIT
metadata:
  author: JoseJavega
  version: "1.0.0"
---

## Activation Contract

Carga este skill cuando el usuario quiera:
- Expandir el árbol genealógico
- Buscar ancestros de una persona
- Añadir nuevas ramas familiares
- Descubrir padres, abuelos o bisabuelos

## Hard Rules

1. Cada persona añadida DEBE citar al menos una fuente.
2. NUNCA confiar en árboles de usuarios (Geni, Ancestry hints) sin corroboración.
3. NO añadir relaciones confirmadas solo de árboles de usuarios → tratarlas como leads.
4. NO modificar fechas o nombres existentes → eso es trabajo de cross-reference audit.
5. Marcar adiciones no verificadas como `(speculative)` o `evidence_tier: speculative`.
6. Preferir añadir a `Open_Questions.md` sobre añadir un ancestro débil a `Family_Tree.md`.
7. NO buscar personas vivas en web. Tratar al punto de partida, hermanos, padres y cualquier persona sin fecha de muerte como vivos.
8. **OBLIGATORIO**: Leer `vault-conventions.md` ANTES de crear cualquier archivo o directorio. Seguir estrictamente las convenciones de nombres, frontmatter y estructura de carpetas definidas ahí.
9. **Privacidad (si `public_study: true`)**: Si `.genai-config.json` tiene `"public_study": true`, aplicar protección de datos a personas vivas:
   - Nombres: usar iniciales o " [Vivo] "
   - Fechas: año completo o " ca. XXXX " (sin día/mes exacto)
   - Lugares: solo municipio/provincia (sin dirección exacta)
   - NO incluir: teléfonos, emails, direcciones postales
   Si `public_study: false`, no aplicar estas restricciones.

## Decision Gates

| Situación | Acción |
|-----------|--------|
| Fuente primaria (certificado, registro) | Añadir a Family_Tree.md con tier `strong` |
| Fuente secundaria con corroboración | Añadir a Family_Tree.md con tier `moderate` |
| Fuente secundaria sin corroboración | Añadir a Open_Questions.md |
| Sin fuentes | NO añadir, registrar en Research_Log.md |
| Persona posiblemente viva | Marcar como `Living`, sin fechas exactas. Si `public_study: true`, aplicar protección de datos completa (Hard Rule #9) |

## Execution Steps

0. **Cargar configuración**: Leer `.genai-config.json` para determinar si `public_study` es `true` o `false`. Esto afecta las reglas de privacidad (Hard Rule #9).
1. **Cargar convenciones**: Leer `vault-conventions.md` para entender el formato de nombres de archivos, frontmatter requerido y estructura de carpetas.
2. **Baseline**: Leer Family_Tree.md completamente. Escanear `personas/` para ver quiénes ya existen. Registrar posture inicial en Research_Log.md.
3. **Identificar objetivos**: Para cada nodo hoja (ancestro sin padres listados), anotar nombre, fechas y lugar.
4. **Estrategia de búsqueda**: Para cada objetivo, buscar en orden:
   - Revisar `sources/transcripciones/` por si ya existe documentación
   - Buscar defunción/entierro con `genai-burial-spain` (Registro Civil → parroquial → cementerio municipal)
   - "[NOMBRE]" padres nacidos [RANGO_AÑOS] [LUGAR]
   - site:geni.com "[NOMBRE]" [AÑO_NACIMIENTO]
   - site:wikitree.com "[NOMBRE]"
   - "[NOMBRE]" genealogía [LUGAR] [APELLIDO]
5. **Evaluar resultados**: Verificar nombre, fechas Y lugar. Todos tres deben alinearse.
6. **Actualizar vault**: Añadir relaciones `strong` o `moderate` a Family_Tree.md. Añadir relaciones débiles a Open_Questions.md.
7. **Crear archivos**: Para cada nueva persona, crear archivo en `personas/` siguiendo las convenciones de `vault-conventions.md` (nombre Sosa-d'Aboville, frontmatter). Si `public_study: true`, aplicar protección de datos (Hard Rule #9).
8. **Registrar búsqueda**: En Research_Log.md, fecha, consultas, resultados (positivos y negativos).
9. **Reportar**: Conteo de candidatos fuertes, moderados, especulativos, rechazados.

## Output Contract

```markdown
## Expansión del Árbol

### Candidatos Revisados
| Nombre | Relación | Tier | Fuente | Acción |
|--------|----------|------|--------|--------|

### Resumen
- Strong: [n]
- Moderate: [n]
- Speculative: [n]
- Rechazados: [n]

### Archivos Modificados
- [archivos]

### Pendiente
- [acciones]
```
