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
   - Si el directorio tiene archivos: **NO hagas nada**. Informa al usuario que el vault ya tiene contenido y que la inicialización no se realizará para evitar sobrescribir datos existentes.
   - Si el directorio está vacío: continúa al paso 4.

4. **Preguntar sobre publicidad del estudio**: Pregunta al usuario:
   > ¿Prevees hacer público este estudio genealógico (subirlo a GitHub, blogs, portafolios, etc.)?
   >
   > - **Sí, será público**: Se aplicará protección de datos a personas vivas (nombres ocultos, fechas parciales).
   > - **No, será privado**: No se aplicará protección adicional.
   
   Si el usuario elige "público", muestra este recordatorio:
   > **Recordatorio importante**: Si usas GitHub o similar, asegúrate de que el repositorio esté en **privado**. Los datos genealógicos contienen información personal sensible.

5. **Copiar template**: Copia toda la estructura de `vault-template/` al directorio del vault:
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
   └── templates/
       ├── person.md
       ├── transcription.md
       ├── certificate.md
       ├── region.md
       ├── surname.md
       └── hypothesis.md
   ```

6. **Crear .genai-config.json**: En el vault, crea un archivo de configuración con la ruta de vuelta al proyecto y la configuración de privacidad:
   ```json
   {
     "project_path": "F:\\Proyectos\\ai_packages\\GenAI",
     "vault_initialized": "YYYY-MM-DD",
     "template_version": "1.0.0",
     "public_study": false,
     "privacy_reminder": true
   }
   ```
   - `public_study`: `true` si el usuario dijo que será público, `false` si será privado.
   - `privacy_reminder`: `true` siempre (recordatorio sobre repositorios privados).

7. **Reportar**: Informa al usuario que el vault ha sido inicializado correctamente y muestra la estructura creada.

## IMPORTANTE

- **NUNCA** copies el template si el directorio tiene archivos.
- **SIEMPRE** verifica que el directorio esté completamente vacío antes de proceder.
- Si hay dudas, **NO** ejecutes la copia. Pide confirmación al usuario.
