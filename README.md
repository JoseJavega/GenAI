# GenAI - Agente de Investigación Genealógica

Agente especializado en investigación genealógica familiar para OpenCode. Inspirado en [autoresearch-genealogy](https://github.com/mattprusak/autoresearch-genealogy), adaptado al español y recursos españoles.

## Características

- **13 skills especializados** para diferentes tareas de genealogía
- **Vault template** para Obsidian con estructura completa
- **Recursos españoles**: PARES, Hemeroteca BNE, FamilySearch, Archivos Provinciales
- **Términología en español**: Glosario completo de términos genealógicos
- **Privacidad**: Protección de datos de personas vivas

## Instalación

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
      "description": "Agente de investigación genealógica familiar",
      "mode": "primary",
      "model": "opencode/big-pickle",
      "prompt": "{file:./agents/genai.md}",
      "tools": {
        "bash": true,
        "edit": true,
        "read": true,
        "write": true,
        "webfetch": true,
        "websearch": true
      }
    }
  },
  "skills": {
    "paths": ["./skills"]
  }
}
```

### 3. Inicializar tu vault

Desde OpenCode, ejecuta:

```
/genai-init
```

Esto creará la estructura del vault en la ruta que indiques (solo si está vacío).

## Uso

### Seleccionar el agente

En OpenCode, selecciona el agente `genai` en el selector de agentes.

### Tareas disponibles

| Comando | Descripción |
|---------|-------------|
| "Expandir rama de los García" | Carga skill de expansión de árbol |
| "Auditar fuentes del árbol" | Carga skill de referencias cruzadas |
| "Buscar en Find a Grave" | Carga skill de cementerios |
| "Verificar GEDCOM" | Carga skill de verificación GEDCOM |
| "Analizar ADN" | Carga skill de análisis genético |

### Comando de inicialización

```
/genai-init [ruta-del-vault]
```

Inicializa un vault nuevo con el template.

## Estructura del Proyecto

```
GenAI/
├── opencode.json                 # Configuración del agente
├── agents/
│   └── genai.md                  # System prompt del agente
├── commands/
│   └── genai-init.md             # Comando de inicialización
├── skills/
│   ├── genai/                    # Skill orquestador
│   │   ├── SKILL.md
│   │   └── references/
│   ├── genai-tree-expansion/     # Prompt 01
│   ├── genai-cross-reference/    # Prompt 02
│   ├── genai-findagrave/         # Prompt 03
│   ├── genai-gedcom/             # Prompt 04
│   ├── genai-source-citation/    # Prompt 05
│   ├── genai-unresolved/         # Prompt 06
│   ├── genai-timeline-gap/       # Prompt 07
│   ├── genai-open-questions/     # Prompt 08
│   ├── genai-local-history/      # Prompt 09
│   ├── genai-colonial/           # Prompt 10
│   ├── genai-immigration/        # Prompt 11
│   ├── genai-dna-analysis/       # Prompt 12
│   └── genai-image-archive/      # Prompt 13 - Imágenes, OCR y entidades
└── vault-template/               # Template para Obsidian
    ├── _Index.md
    ├── Family_Tree.md
    ├── Research_Log.md
    └── ...
```

## Skills (Prompts)

| # | Skill | Descripción |
|---|-------|-------------|
| 01 | genai-tree-expansion | Expandir árbol con fuentes verificadas |
| 02 | genai-cross-reference | Auditar discrepancias en datos |
| 03 | genai-findagrave | Buscar en cementerios |
| 04 | genai-gedcom | Verificar archivos GEDCOM |
| 05 | genai-source-citation | Verificar completitud de fuentes |
| 06 | genai-unresolved | Resolver personas sin nombre |
| 07 | genai-timeline-gap | Encontrar huecos temporales |
| 08 | genai-open-questions | Resolver preguntas abiertas |
| 09 | genai-local-history | Extraer de historia local |
| 10 | genai-colonial | Buscar registros coloniales |
| 11 | genai-immigration | Buscar registros de inmigración |
| 12 | genai-dna-analysis | Analizar ADN por cromosoma |
| 13 | genai-image-archive | Procesar imágenes, OCR y extraer entidades genealógicas |

## Recursos Españoles

- **PARES**: https://pares.culturaydeporte.gob.es
- **Hemeroteca BNE**: https://hemerotecadigital.bne.es
- **FamilySearch**: https://www.familysearch.org
- **HISPAGEN**: http://www.hispagen.es

## Licencia

MIT

## Créditos

Basado en [autoresearch-genealogy](https://github.com/mattprusak/autoresearch-genealogy) de Matt Prusak. Adaptado para español y recursos españoles.
