---
name: genai-image-archive
description: "Explorar archivos de imágenes, leer entradas, recortar evidencia y extraer entidades genealógicas. Trigger: imágenes, fotos antiguas, escanear, OCR, certificados, transcribir."
license: MIT
metadata:
  author: JoseJavega
  version: "2.0.0"
---

## Activation Contract

Carga este skill cuando el usuario quiera:
- Procesar imágenes de documentos genealógicos
- Realizar OCR en fotos antiguas
- Clasificar imágenes por tipo
- Extraer información de imágenes
- **Extraer entidades de certificados** (bautismo, matrimonio, defunción)
- **Crear transcripciones en el vault**

## Hard Rules

1. Usar el método OCR apropiado según el tipo de imagen.
2. Mantener metadatos de la imagen original.
3. Clasificar: photo_only, printed_text, handwritten, mixed.
4. Resolución mínima: 300 DPI para OCR.
5. **OBLIGATORIO**: Leer `vault-conventions.md` ANTES de crear cualquier archivo o directorio en el vault. Seguir estrictamente las convenciones de nombres, frontmatter y estructura de carpetas.
6. **Ubicaciones**: Las fuentes originales van en `fuentes/certificados/` o `fuentes/fotos/`. Las transcripciones .md van en `fuentes/transcripciones/`.
7. **Extraer entidades**: Siempre extraer personas, fechas, lugares y relaciones del texto OCR.
8. **Vincular persona**: Cada transcripción DEBE incluir campo `person` en frontmatter.

## Decision Gates

| Tipo de Imagen | Método OCR |
|----------------|------------|
| Foto/retrato | Solo catálogo |
| Texto impreso | OpenCode vision |
| Manuscrito | OpenCode vision |
| Mixto (postal) | Enfoque por capas |

## Execution Steps

### Fase 1: Catálogo de Imágenes

1. **Escanear fuentes**: `glob fuentes/certificados/*.pdf` y `glob fuentes/fotos/*.jpg|*.png` para identificar originales.
2. **Identificar transcritos**: `glob fuentes/transcripciones/*.md` para ver cuáles ya tienen transcripción.
3. **Calcular faltantes**: Resta de originales menos transcritos = lista de pendientes.
4. **Clasificar pendientes**: Por tipo de contenido (texto impreso, manuscrito, foto, mixto).

### Fase 2: OCR con OpenCode Vision

5. **Preparar imagen**: Asegurar resolución ≥300 DPI. Si es PDF, extraer página relevante.
6. **Ejecutar OCR**: Usar la herramienta de visión de OpenCode para analizar la imagen.
7. **Prompt de OCR**: Pedir al modelo que extraiga TODO el texto visible, manteniendo el formato original.

**Prompt sugerido para OCR:**
```
Extrae todo el texto de esta imagen de documento genealógico.
Mantén el formato original incluyendo:
- Fechas completas
- Nombres y apellidos
- Lugares
- Relaciones familiares (hijo de, casado con, etc.)
- Cargos y testigos
Responde SOLO con el texto extraído, sin comentarios adicionales.
```

### Fase 3: Extracción de Entidades

8. **Identificar tipo de documento**: Bautismo, matrimonio, defunción, otro.
9. **Extraer entidades** según patrones en `references/entity-patterns.md`:
   - **Persona principal** (nombre, fecha nacimiento, lugar)
   - **Padres** (nombre padre, nombre madre)
   - **Abuelos** (si aparecen)
   - **Otros** (padrinos, testigos, oficiante)
10. **Normalizar fechas**: Convertir texto a formato YYYY-MM-DD.
11. **Validar consistencia**: Verificar que las entidades tienen sentido (ej: padre mayor que hijo).

### Fase 4: Guardado en Vault

12. **Crear transcripción** en `fuentes/transcripciones/` siguiendo formato en `references/vault-output-format.md`.
13. **Actualizar persona** en `personas/` si existe, o crear nueva si es ancestor desconocido.
14. **Registrar** en Research_Log.md.

## Output Contract

```markdown
## Procesamiento de Imágenes

### Imágenes Procesadas
| Imagen | Tipo | Método | Entidades | Estado |
|--------|------|--------|-----------|--------|

### Resumen
- Total: [n]
- Procesadas: [n]
- Pendientes: [n]
- Entidades extraídas: [n]

### Entidades Encontradas
| Persona | Tipo Documento | Fecha | Lugar |
|---------|---------------|-------|-------|
```

## References

- `references/entity-patterns.md` — Patrones de extracción para documentos españoles
- `references/vault-output-format.md` — Formato de salida al vault
- `../genai/references/vault-conventions.md` — Convenciones generales del vault
