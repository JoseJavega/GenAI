# GenAI — Orquestador de Investigación Genealógica

GenAI es un agente para OpenCode especializado en investigación genealógica familiar. No ejecuta trabajo directamente: actúa como **orquestador** que recibe tu petición, la delega en el sub-agente especializado adecuado y sintetiza los resultados.

Inspirado en [autoresearch-genealogy](https://github.com/mattprusak/autoresearch-genealogy), adaptado al español y a los recursos genealógicos españoles.

## Arquitectura

```
Usuario
  │
  ▼
┌──────────────────────────────────────────┐
│  GenAI (orquestador)                     │
│  - Clasifica tu petición                 │
│  - Delega al sub-agente especializado    │
│  - Sintetiza resultados                  │
│  - Nunca ejecuta trabajo inline          │
└────┬──────┬──────┬──────────┬───────────┘
     │      │      │          │
     ▼      ▼      ▼          ▼
  expansion  images  investigation  analysis
```

| Sub-agente | Función | Skills asociadas |
|---|---|---|
| `genai-expansion` | Expandir árbol, buscar ancestros, cementerios, inmigración | tree-expansion, findagrave, burial-spain, immigration, colonial, gedcom |
| `genai-images` | OCR, transcripción de documentos, extracción de entidades | image-archive |
| `genai-investigation` | Investigación web, historia local, preguntas abiertas | local-history, open-questions, unresolved |
| `genai-analysis` | Auditoría de fuentes, referencias cruzadas, ADN, huecos temporales | cross-reference, source-citation, dna-analysis, timeline-gap |

## Quick Start

### 1. Clonar el repositorio

```bash
git clone https://github.com/JoseJavega/GenAI.git
cd GenAI
```

### 2. Configurar en OpenCode

Añade a tu `opencode.json` (global o de proyecto):

```json
{
  "agent": {
    "genai": {
      "description": "Orquestador de investigación genealógica",
      "mode": "primary",
      "model": "opencode/big-pickle",
      "prompt": "{file:./agents/genai.md}",
      "tools": {
        "delegate": true,
        "task": true
      }
    }
  },
  "skills": {
    "paths": ["./skills"]
  }
}
```

El orquestador solo necesita `delegate` y `task`. Los sub-agentes se encargan del resto.

### 3. Inicializar un vault

Desde OpenCode, ejecuta:

```
/genai-init
```

Crea la estructura del vault de Obsidian en la ruta que indiques (solo si está vacío).

## Estructura del Proyecto

```
GenAI/
├── opencode.json                   # Configuración del agente
├── agents/
│   ├── genai.md                    # System prompt del orquestador
│   ├── genai-expansion.md          # Sub-agente de expansión de árbol
│   ├── genai-images.md             # Sub-agente de procesamiento de imágenes
│   ├── genai-investigation.md      # Sub-agente de investigación web
│   └── genai-analysis.md           # Sub-agente de análisis y auditoría
├── commands/
│   └── genai-init.md               # Comando de inicialización de vault
├── skills/
│   ├── genai/                      # Skill base (referencias compartidas)
│   │   └── references/
│   ├── genai-tree-expansion/       # Expansión de árbol genealógico
│   ├── genai-cross-reference/      # Auditoría de referencias cruzadas

│   ├── genai-gedcom/               # Verificación de archivos GEDCOM
│   ├── genai-source-citation/      # Verificación de citas de fuentes
│   ├── genai-unresolved/           # Identificación de personas sin nombre
│   ├── genai-timeline-gap/         # Detección de huecos temporales
│   ├── genai-open-questions/       # Resolución de preguntas abiertas
│   ├── genai-local-history/        # Extracción de historia local
│   ├── genai-colonial/             # Búsqueda de registros coloniales
│   ├── genai-immigration/          # Búsqueda de registros de inmigración
│   ├── genai-dna-analysis/         # Análisis de ADN por cromosoma
│   └── genai-image-archive/        # Procesamiento de imágenes y OCR
└── vault-template/                 # Template para vault de Obsidian
    ├── _Index.md
    ├── Family_Tree.md
    └── ...
```

## Skills

Cada skill es un conjunto de instrucciones especializadas que los sub-agentes cargan según la tarea.

| Skill | Descripción | Usada por |
|---|---|---|
| `genai-tree-expansion` | Expandir árbol con fuentes verificadas | expansion |
| `genai-cross-reference` | Auditar discrepancias entre árbol y fuentes | analysis |
| `genai-gedcom` | Verificar completitud de archivos GEDCOM | expansion |
| `genai-source-citation` | Verificar que cada persona cite fuentes | analysis |
| `genai-unresolved` | Identificar y resolver personas sin nombre | investigation |
| `genai-timeline-gap` | Encontrar huecos donde debería haber registros | analysis |
| `genai-open-questions` | Atacar sistemáticamente preguntas abiertas | investigation |
| `genai-local-history` | Extraer datos de libros de historia local | investigation |
| `genai-colonial` | Buscar registros coloniales americanos | expansion |
| `genai-immigration` | Localizar manifiestos y naturalizaciones | expansion |
| `genai-dna-analysis` | Analizar segmentos genéticos por cromosoma | analysis |
| `genai-image-archive` | Procesar imágenes, OCR y extraer entidades | images |

## Recursos Españoles

Recursos clave para investigación genealógica en España:

- **PARES**: https://pares.culturaydeporte.gob.es
- **Hemeroteca BNE**: https://hemerotecadigital.bne.es
- **FamilySearch**: https://www.familysearch.org
- **HISPAGEN**: http://www.hispagen.es

Referencia completa en `skills/genai/references/spanish-genealogy-resources.md`.

## Licencia

MIT

## Créditos

Basado en [autoresearch-genealogy](https://github.com/mattprusak/autoresearch-genealogy) de Matt Prusak. Adaptado al español y a los recursos genealógicos españoles.
