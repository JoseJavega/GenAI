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
cr_type: person                  # Para Charted Roots (recomendado)
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: [genealogy, person]
name: "Nombre Completo"
sosa: 1                          # Número Sosa (ascendientes directos)
daboville: "1"                   # Número d'Aboville (descendientes/colaterales)
sosa_daboville: "1"              # Híbrido Sosa-d'Aboville
born: "YYYY-MM-DD"              # O "Living" si está vivo
died: "YYYY-MM-DD"              # O omitir si está vivo
birth_place: "Lugar"
death_place: "Lugar"
life_status: "deceased"          # living | deceased | unknown
family: "Apellido"
evidence_tier: "strong"          # strong | moderate | speculative
profile_status: "complete"       # complete | partial | draft
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

### Sistema de Numeración Sosa-d'Aboville

Cada persona recibe un número compuesto por:
- **Sosa** (3 dígitos): posición en línea directa ascendente
- **d'Aboville** (2 dígitos): orden de nacimiento entre hermanos

**Formato**: `SSS-HH_Nombre_Apellido.md`

| Tipo | Ejemplo | Significado |
|------|---------|-------------|
| De cujus | `001_Nombre.md` | Yo (raíz del árbol) |
| Padre | `002_Padre.md` | Sosa 2 |
| Madre | `003_Madre.md` | Sosa 3 |
| Abuelo paterno | `004_Abuelo.md` | Sosa 4 |
| Tío paterno | `004-01_Tio.md` | Primer hijo de Sosa 4 |
| Primo paterno | `004-01.02_Primo.md` | Segundo hijo del tío 4-01 |

**Reglas**:
- Sosa siempre 3 dígitos: `001`, `002`, `004`, `032`, `128`
- d'Aboville siempre 2 dígitos: `01`, `02`, `12`
- Separador Sosa-d'Aboville: guión (`-`)
- Separador generaciones d'Aboville: punto (`.`)
- Sin espacios: usar guiones bajos (`_`) en nombre y apellido
- Ejemplo completo: `006-01.02_Juan_García.md`

**Ejemplos**:
```
001_Luis_Jávega.md              # De cujus
002_Padre_de_Luis.md            # Padre (Sosa 2)
003_Madre_de_Luis.md            # Madre (Sosa 3)
004_Abuelo_Paterno.md           # Abuelo paterno (Sosa 4)
004-01_Tío_Paterno.md           # Tío paterno (hermano del abuelo 4)
004-01.02_Primo_Paterno.md      # Primo paterno (hijo del tío 4-01)
005_Abuela_Paterna.md           # Abuela paterna (Sosa 5)
006_Abuelo_Materno.md           # Abuelo materno (Sosa 6)
006-01_Tío_Materno.md           # Tío materno (hermano del abuelo 6)
006-01.01_Primo_Materno.md      # Primo materno (hijo del tío 6-01)
```

## Wikilinks

Usar wikilinks de Obsidian para conectar archivos. Usar el número Sosa como identificador:

```markdown
Ver también: [[004_Abuelo_Paterno]], [[006_Abuelo_Materno]]
Padres: [[002_Padre_de_Luis]] y [[003_Madre_de_Luis]]
Tíos: [[004-01_Tío_Paterno]], [[006-01_Tío_Materno]]
Primos: [[004-01.02_Primo_Paterno]], [[006-01.01_Primo_Materno]]
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

> **Nota**: Estas reglas se aplican SOLO si `.genai-config.json` tiene `"public_study": true`. Si el estudio es privado (`public_study: false`), no son obligatorias.

1. **Personas vivas**: Marcar como `Living` en `born` y `died`
2. **No exponer** (si `public_study: true`): Fechas exactas de nacimiento, direcciones, teléfonos
3. **Año solamente**: Si es necesario, usar solo el año de nacimiento
4. **Investigación autónoma**: Nunca buscar personas vivas en web durante ejecución autónoma (siempre aplica, independientemente de `public_study`)

## Plantillas Disponibles

| Plantilla | Uso |
|-----------|-----|
| `person.md` | Nuevo archivo de persona |
| `transcription.md` | Transcripción de documento |
| `certificate.md` | Certificado oficial |
| `region.md` | Investigación regional |
| `surname.md` | Origen de apellido |
| `hypothesis.md` | Hipótesis de investigación |
