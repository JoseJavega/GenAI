---
name: genai-image-archive
description: "Explorar archivos de imágenes, leer entradas, recortar evidencia y extraer entidades genealógicas. Trigger: imágenes, fotos antiguas, escanear, OCR, certificados, transcribir."
license: MIT
metadata:
  author: JoseJavega
  version: "2.1.0"
---

## Activation Contract

Carga este skill cuando quieras procesar imágenes de documentos genealógicos: OCR, clasificación, extracción de entidades y transcripción para el vault.

## Hard Rules

1. **NO scripts, NO external tools**: Solo OpenCode vision para OCR. Nada de Python, Tesseract, ocrmypdf, ImageMagick.
2. **Pipeline estricto**: Ejecutar pasos en orden. Clasificar → extraer → validar → guardar. No saltar pasos.
3. Resolución mínima: 300 DPI para OCR. Para PDFs, extraer cada página como imagen independiente y procesar una a una.
4. **OBLIGATORIO**: Leer `vault-conventions.md` antes de crear archivos. Seguir convenciones de nombres, frontmatter y estructura.
5. **Ubicaciones**: Originales en `fuentes/certificados/` o `fuentes/fotos/`. Transcripciones en `fuentes/transcripciones/`.
6. Toda transcripción DEBE incluir `source_file` en frontmatter apuntando al original.
7. **Extracción estructurada**: NO extraer texto plano y luego parsear con regex. Usar los prompts tipo-específicos (`references/ocr-prompts/{tipo}.md`) para obtener YAML estructurado directamente desde la imagen.
8. **Validación obligatoria**: Todo YAML extraído debe pasar por `references/entity-patterns.md` (validación de coherencia interna) antes de guardarse. NO se cruza con vault — eso corresponde a `genai-cross-reference`.

## Decision Gates

| Tipo Imagen | Método |
|---|---|
| Foto/retrato | Solo catálogo |
| Texto impreso | OpenCode vision + extracción estructurada |
| Manuscrito | OpenCode vision + extracción estructurada |
| Mixto (postal) | Enfoque por capas |

### Por tipo de documento (tras clasificación)

| Documento | Prompt de extracción |
|---|---|
| `bautismo` | `references/ocr-prompts/bautismo.md` |
| `matrimonio` | `references/ocr-prompts/matrimonio.md` |
| `defuncion` | `references/ocr-prompts/defuncion.md` |
| Desconocido | Clasificar primero con `references/ocr-prompts/classify.md` |
| Foto sin texto | Saltar extracción, solo catalogar |

## Execution Steps

Para PDFs multi-página: extraer cada página como imagen independiente y procesar una a una siguiendo los pasos siguientes.

1. **Catalogar**: `glob fuentes/certificados/*.pdf` + `glob fuentes/fotos/*.jpg|*.png`. Identificar transcritos vía `glob fuentes/transcripciones/*.md`. Calcular pendientes por resta.

2. **Clasificar tipo de documento**: Usar OpenCode vision con el prompt en `references/ocr-prompts/classify.md` para determinar: `bautismo`, `matrimonio`, `defuncion` o `otro`.
   - Si `otro` → catalogar como "pendiente de clasificación manual", pasar a siguiente imagen.
   - Si `foto` (sin texto) → catalogar, saltar extracción.

3. **Extracción estructurada por tipo**: Usar OpenCode vision con el prompt específico según el tipo detectado:
   - Bautismo → `references/ocr-prompts/bautismo.md` — devuelve YAML con persona, padres, abuelos, padrinos
   - Matrimonio → `references/ocr-prompts/matrimonio.md` — devuelve YAML con novios, padres, testigos
   - Defunción → `references/ocr-prompts/defuncion.md` — devuelve YAML con difunto, cónyuge, padres, causa
   
   Cada prompt devuelve YAML estructurado directamente desde la imagen. NO se extrae texto plano para parsear después.

4. **Validar coherencia interna**: Aplicar `references/entity-patterns.md`:
   - Validar coherencia interna: fechas, edades, relaciones familiares
   - Evaluar confianza por campo y confianza combinada
   - Si `ocr_confidence_final < 0.60` → marcar para revisión manual, no guardar automáticamente

5. **Guardar**: Transcripción en `fuentes/transcripciones/` (formato en `references/vault-output-format.md`). Registrar en Research_Log.md.

## Output Contract

```markdown
## Procesamiento de Imágenes

### Imágenes Procesadas
| Imagen | Tipo | Método | Entidades | Confianza | Estado |
|--------|------|--------|-----------|--------|

### Resumen
- Total: [n] | Procesadas: [n] | Pendientes: [n] | Entidades: [n]

### Entidades Encontradas
| Persona | Documento | Fecha | Lugar |
|---------|-----------|-------|-------|
```

## References

- `references/ocr-prompts/classify.md` — Prompt para clasificar tipo de documento (bautismo/matrimonio/defunción)
- `references/ocr-prompts/bautismo.md` — Extracción estructurada para partidas de bautismo
- `references/ocr-prompts/matrimonio.md` — Extracción estructurada para actas de matrimonio
- `references/ocr-prompts/defuncion.md` — Extracción estructurada para actas de defunción
- `references/entity-patterns.md` — Validación de coherencia interna y evaluación de confianza
- `references/vault-output-format.md` — Formato de salida al vault
