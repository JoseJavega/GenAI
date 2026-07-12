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

## Decision Gates

| Tipo de Imagen | Método OCR |
|----------------|------------|
| Foto/retrato | Solo catálogo |
| Texto impreso | ocrmypdf (Tesseract) |
| Manuscrito | Claude multimodal |
| Mixto (postal) | Enfoque por capas |

## Execution Steps

1. **Identificar imágenes**: En el vault o directorio indicado.
2. **Clasificar**: Por tipo de contenido.
3. **Procesar OCR**: Según el tipo.
4. **Extraer texto**: Transcribir contenido.
5. **Añadir al vault**: Como transcripción.
6. **Registrar**: En Data_Inventory.md.

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
