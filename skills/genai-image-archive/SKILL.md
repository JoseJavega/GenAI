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
2. Clasificar imágenes: `photo_only`, `printed_text`, `handwritten`, `mixed`.
3. Resolución mínima: 300 DPI para OCR.
4. **OBLIGATORIO**: Leer `vault-conventions.md` antes de crear archivos. Seguir convenciones de nombres, frontmatter y estructura.
5. **Ubicaciones**: Originales en `fuentes/certificados/` o `fuentes/fotos/`. Transcripciones en `fuentes/transcripciones/`.
6. Toda transcripción DEBE incluir campo `person` en frontmatter.
7. Extraer siempre: personas, fechas, lugares y relaciones.

## Decision Gates

| Tipo Imagen | Método |
|---|---|
| Foto/retrato | Solo catálogo |
| Texto impreso | OpenCode vision |
| Manuscrito | OpenCode vision |
| Mixto (postal) | Enfoque por capas |

## Execution Steps

1. **Catalogar**: `glob fuentes/certificados/*.pdf` + `glob fuentes/fotos/*.jpg|*.png`. Identificar transcritos vía `glob fuentes/transcripciones/*.md`. Calcular pendientes por resta.
2. **OCR**: Preparar imagen (≥300 DPI). Usar OpenCode vision con el prompt en `references/ocr-prompt.md`.
3. **Extraer entidades**: Identificar tipo documento (bautismo/matrimonio/defunción). Extraer persona principal, padres, abuelos, testigos según `references/entity-patterns.md`. Normalizar fechas a YYYY-MM-DD. Validar consistencia.
4. **Guardar**: Transcripción en `fuentes/transcripciones/` (formato en `references/vault-output-format.md`). Actualizar o crear persona en `personas/`. Registrar en Research_Log.md.

## Output Contract

```markdown
## Procesamiento de Imágenes

### Imágenes Procesadas
| Imagen | Tipo | Método | Entidades | Estado |
|--------|------|--------|-----------|--------|

### Resumen
- Total: [n] | Procesadas: [n] | Pendientes: [n] | Entidades: [n]

### Entidades Encontradas
| Persona | Documento | Fecha | Lugar |
|---------|-----------|-------|-------|
```

## References

- `references/ocr-prompt.md` — Prompt sugerido para OpenCode vision
- `references/entity-patterns.md` — Patrones de extracción para documentos españoles
- `references/vault-output-format.md` — Formato de salida al vault
- `../genai/references/vault-conventions.md` — Convenciones generales del vault
