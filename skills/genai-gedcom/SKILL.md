---
name: genai-gedcom
description: "Verificar completitud de archivos GEDCOM contra el vault. Trigger: GEDCOM, exportar árbol, verificar formato."
license: MIT
metadata:
  author: JoseJavega
  version: "1.0.0"
---

## Activation Contract

Carga este skill cuando el usuario quiera:
- Verificar un archivo GEDCOM existente
- Exportar el árbol a formato GEDCOM
- Comprobar que el GEDCOM coincida con el vault

## Hard Rules

1. El GEDCOM debe reflejar exactamente el contenido del vault.
2. Nada de personas en GEDCOM que no estén en Family_Tree.md.
3. Todas las fuentes del vault deben aparecer en el GEDCOM.
4. Formato GEDCOM 5.5.1 estándar.
5. **OBLIGATORIO**: Leer `vault-conventions.md` ANTES de crear o modificar archivos en el vault. Seguir estrictamente las convenciones de nombres, frontmatter y estructura de carpetas.

## Decision Gates

| Situación | Acción |
|-----------|--------|
| Persona en GEDCOM no está en vault | Marcar para revisión |
| Persona en vault no está en GEDCOM | Añadir al GEDCOM |
| Fuente faltante | Añadir al GEDCOM |

## Execution Steps

0. **Cargar convenciones**: Leer `vault-conventions.md` para entender formato de nombres, frontmatter y estructura de carpetas.
1. **Leer Family_Tree.md**: Extraer todas las personas y relaciones.
2. **Leer GEDCOM** (si existe): Comparar estructura.
3. **Identificar gaps**: Personas o fuentes faltantes.
4. **Generar/actualizar GEDCOM**: Con formato correcto, siguiendo las convenciones del vault para nombres de archivos y rutas.
5. **Reportar**: Diferencias encontradas y cambios realizados.

## Output Contract

```markdown
## Verificación GEDCOM

### Personas
- En vault: [n]
- En GEDCOM: [n]
- Faltantes en GEDCOM: [n]
- Sobrantes en GEDCOM: [n]

### Fuentes
- En vault: [n]
- En GEDCOM: [n]

### Archivo Generado
- [Ruta del GEDCOM]
```
