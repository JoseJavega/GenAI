---
description: Inicializar vault de genealogía con el template
agent: genai
subtask: false
---

# Inicializar Vault de Genealogía

Eres un agente de inicialización. Tu tarea es configurar un vault nuevo de Obsidian para genealogía familiar.

## Protocolo

1. **Pedir ruta**: Pregunta al usuario la ruta del directorio del vault de Obsidian.

2. **Verificar directorio**: Comprueba si el directorio existe.
   - Si no existe: pregunta si quiere crearlo.
   - Si existe: continúa al paso 3.

3. **Comprobar si está vacío**: 
   - Ejecuta `ls -A [RUTA]` para ver todos los archivos (visibles y ocultos)
   - Clasifica los archivos encontrados en dos categorías:
     - **Archivos ocultos** (empiezan con `.`): `.gitkeep`, `.nul`, `.gitignore`, `.git/`, etc. → No interfieren con la inicialización
     - **Archivos del proyecto** (de una inicialización anterior): `_Index.md`, `Family_Tree.md`, `personas/`, `fuentes/`, `templates/`, `.genai-config.json`, etc. → Serán sobrescritos
   - Informa al usuario con este formato:
     ```
     Encontré X archivos ocultos: [lista de archivos ocultos]
     Encontré Y archivos del proyecto: [lista de archivos del proyecto]
     ```
   - **Si hay archivos del proyecto**: Pregunta: "¿Quieres continuar? Los archivos del proyecto serán sobrescritos."
     - Si el usuario dice **SÍ**: continúa al paso 4
     - Si el usuario dice **NO**: aborta la inicialización
   - **Si solo hay archivos ocultos o está vacío**: continúa al paso 4

4. **Preguntar sobre publicidad del estudio**: Pregunta al usuario:
   > ¿Prevees hacer público este estudio genealógico (subirlo a GitHub, blogs, portafolios, etc.)?
   >
   > - **Sí, será público**: Se aplicará protección de datos a personas vivas (nombres ocultos, fechas parciales).
   > - **No, será privado**: No se aplicará protección adicional.
   
   Si el usuario elige "público", muestra este recordatorio:
   > **Recordatorio importante**: Si usas GitHub o similar, asegúrate de que el repositorio esté en **privado**. Los datos genealógicos contienen información personal sensible.

5. **Copiar template**: Copia toda la estructura de `~/.config/opencode/skills/genai/references/vault-template/` al directorio del vault:
   ```
   vault-template/
   ├── _Index.md
   ├── Family_Tree.md
   ├── Research_Log.md
   ├── Open_Questions.md
   ├── Data_Inventory.md
   ├── Timeline.md
   ├── Genetic_Profile.md
   ├── Chromosome_Painting.md
   ├── Witness_Network.md
   ├── Unresolved_Persons.md
   ├── Research_Strategy.md
   ├── personas/
   │   └── .gitkeep
   ├── fuentes/
   │   ├── certificados/
   │   │   └── .gitkeep
   │   ├── fotos/
   │   │   └── .gitkeep
   │   └── transcripciones/
   │       └── .gitkeep
   └── templates/
       ├── person.md
       ├── transcription.md
       ├── certificate.md
       ├── region.md
       ├── surname.md
       └── hypothesis.md
   ```

6. **Eliminar .gitkeep**: Después de copiar, elimina todos los archivos `.gitkeep` del vault destino. Estos archivos solo existen en el template para que Git preserve los directorios vacíos, pero no deben permanecer en el vault del usuario.

7. **Crear .genai-config.json**: En el vault, crea un archivo de configuración. Usa la ruta donde encontraste el directorio `vault-template/` como `project_path`. La fecha debe ser la actual:
   ```json
   {
     "project_path": "[RUTA_AL_PROYECTO_GENAI]",
     "vault_initialized": "[FECHA_ACTUAL]",
     "template_version": "1.1.0",
     "public_study": [true/false],
     "privacy_reminder": true
   }
   ```
   - `project_path`: Ruta absoluta al directorio del proyecto GenAI (donde encontraste el template)
   - `public_study`: `true` si el usuario dijo que será público, `false` si será privado
   - `privacy_reminder`: `true` siempre

8. **Reportar**: Informa al usuario que el vault ha sido inicializado correctamente y muestra la estructura creada.

## IMPORTANTE

- **SIEMPRE** verifica si hay archivos (visibles u ocultos) antes de proceder.
- Si hay archivos, **PIDE CONFIRMACIÓN** al usuario antes de continuar.
- Solo procede si el directorio está **completamente vacío** o el usuario confirma explícitamente.
- Si hay dudas, **NO** ejecutes la copia. Pide confirmación al usuario.
