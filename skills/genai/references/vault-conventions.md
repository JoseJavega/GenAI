# Convenciones del Vault Obsidian

## Estructura del Vault

```
vault/
├── _Index.md                    # Índice maestro
├── Family_Tree.md               # Árbol completo
├── Research_Log.md              # Registro cronológico de investigación
├── Open_Questions.md            # Preguntas pendientes
├── Data_Inventory.md            # Inventario de fuentes
├── Timeline.md                  # Línea temporal de eventos
├── Genetic_Profile.md           # Perfil genético
├── Chromosome_Painting.md       # Pintura cromosómica
├── Witness_Network.md           # Red de testigos
├── Unresolved_Persons.md        # Personas sin resolver
├── Research_Strategy.md         # Estrategia de investigación
├── [Apellido]/                  # Carpetas por apellido
│   ├── Nombre_Apellido.md      # Archivo de persona
│   └── ...
├── [Región]/                    # Carpetas por región
│   └── ...
└── templates/                   # Plantillas
    ├── person.md
    ├── transcription.md
    ├── certificate.md
    ├── region.md
    ├── surname.md
    └── hypothesis.md
```

## Frontmatter YAML

### Archivo de Persona

```yaml
---
type: person
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: [genealogy, person]
name: "Nombre Completo"
born: "YYYY-MM-DD"           # O "Living" si está vivo
died: "YYYY-MM-DD"           # O omitir si está vivo
birth_place: "Lugar"
death_place: "Lugar"
life_status: "deceased"      # living | deceased | unknown
family: "Apellido"
evidence_tier: "strong"      # strong | moderate | speculative
profile_status: "complete"   # complete | partial | draft
sources:
  - "Fuente 1"
  - "Fuente 2"
---
```

### Archivo de Transcripción

```yaml
---
type: transcription
created: YYYY-MM-DD
tags: [genealogy, document]
source: "Archivo/Referencia"
document_type: "bautismo"    # bautismo | matrimonio | defunción | otro
person: "Nombre"
date: "YYYY-MM-DD"
ocr_method: "manual"         # manual | tesseract | claude
ocr_quality: "good"          # good | fair | poor
---
```

## Convenciones de Nombres de Archivos

- Usar guiones bajos, no espacios: `Juan_García.md` (no `Juan García.md`)
- Formato: `Nombre_Apellido.md`
- Para hijos: `Nombre_Apellido_Hijo.md` si hay homónimos
- Los apellidos van en mayúsculas en carpetas: `García/`

## Wikilinks

Usar wikilinks de Obsidian para conectar archivos:

```markdown
Ver also: [[Juan_García]], [[María_López]]
Padres: [[Pedro_García]] y [[Ana_Martínez]]
```

## Niveles de Evidencia

| Tier | Descripción | Uso |
|------|-------------|-----|
| `strong` | Fuente primaria verificada | Registros civiles, parroquiales |
| `moderate` | Fuente secundaria confiable | Árboles de otros investigadores con corroboración |
| `speculative` | Evidencia circunstancial | Conexiones inferidas, no confirmadas |

## Estados de Vida

| Valor | Significado |
|-------|-------------|
| `living` | Persona viva (NO exponer fechas exactas) |
| `deceased` | Persona fallecida |
| `unknown` | No se sabe si está viva o muerta |

## Reglas de Privacidad

1. **Personas vivas**: Marcar como `Living` en `born` y `died`
2. **No exponer**: Fechas exactas de nacimiento, direcciones, teléfonos
3. **Año solamente**: Si es necesario, usar solo el año de nacimiento
4. **Investigación autónoma**: Nunca buscar personas vivas en web durante ejecución autónoma

## Plantillas Disponibles

| Plantilla | Uso |
|-----------|-----|
| `person.md` | Nuevo archivo de persona |
| `transcription.md` | Transcripción de documento |
| `certificate.md` | Certificado oficial |
| `region.md` | Investigación regional |
| `surname.md` | Origen de apellido |
| `hypothesis.md` | Hipótesis de investigación |
