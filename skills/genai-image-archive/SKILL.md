---
name: genai-image-archive
description: "Explorar archivos de imágenes, leer entradas y recortar evidencia. Trigger: imágenes, fotos antiguas, escanear, OCR."
license: MIT
metadata:
  author: JoseJavega
  version: "1.0.0"
---

## Activation Contract

Carga este skill cuando el usuario quiera:
- Procesar imágenes de documentos
- Realizar OCR en fotos antiguas
- Clasificar imágenes por tipo
- Extraer información de imágenes

## Hard Rules

1. Usar el método OCR apropiado según el tipo de imagen.
2. Mantener metadatos de la imagen original.
3. Clasificar: photo_only, printed_text, handwritten, mixed.
4. Resolución mínima: 300 DPI para OCR.
5. **OBLIGATORIO**: Leer `vault-conventions.md` ANTES de crear cualquier archivo o directorio en el vault. Seguir estrictamente las convenciones de nombres, frontmatter y estructura de carpetas.
6. **Ubicaciones**: Las fuentes originales van en `sources/certificados/` o `sources/fotos/`. Las transcripciones .md van en `sources/transcripciones/`.

## Decision Gates

| Tipo de Imagen | Método OCR |
|----------------|------------|
| Foto/retrato | Solo catálogo |
| Texto impreso | ocrmypdf (Tesseract) |
| Manuscrito | OpenCode vision |
| Mixto (postal) | Enfoque por capas |

## Execution Steps

1. **Escanear fuentes**: `glob sources/certificados/*.pdf` y `glob sources/fotos/*.jpg|*.png` para identificar originales.
2. **Identificar transcritos**: `glob sources/transcripciones/*.md` para ver cuáles ya tienen transcripción.
3. **Calcular faltantes**: Resta de originales menos transcritos = lista de pendientes.
4. **Clasificar pendientes**: Por tipo de contenido (texto impreso, manuscrito, foto, mixto).
5. **Procesar OCR**: Según el tipo, usar método apropiado.
6. **Crear transcripción**: Guardar en `sources/transcripciones/NOMBRE_ORIGINAL.md` siguiendo `vault-conventions.md`.
7. **Vincular persona**: En frontmatter, incluir campo `person` con el nombre exacto del archivo en `personas/`.
8. **Registrar**: En Data_Inventory.md.

## Output Contract

```markdown
## Procesamiento de Imágenes

### Imágenes Procesadas
| Imagen | Tipo | Método | Texto Extraído |
|--------|------|--------|----------------|

### Resumen
- Total: [n]
- photo_only: [n]
- printed_text: [n]
- handwritten: [n]
- mixed: [n]
```
