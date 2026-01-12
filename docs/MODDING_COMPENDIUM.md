# Victoria 3 Modding Compendium
Este documento sirve como base de conocimiento técnico extraída de la Wiki Oficial de Victoria 3. Está diseñado para ser leído por humanos y modelos de IA para mantener la consistencia en el desarrollo del mod.

## 1. Estructura del Mod
Los mods se cargan desde la carpeta `Documents\Paradox Interactive\Victoria 3\mod`.
- **Estructura de Carpetas:** Debe imitar la carpeta del juego original (ej: `common/decisions`, `localization/spanish`).
- **Metadatos:** Es vital tener un archivo `metadata.json` dentro de una carpeta `.metadata` para que el launcher reconozca el mod.
- **Sobrescritura:** Los archivos con el mismo nombre que los originales los reemplazan.
- **Inyección (1.12+):** Se pueden usar bloques `INJECT` o `REPLACE` para modificar archivos originales de forma más modular.

## 2. Modificadores (Modifier Modding)
Los modificadores son contenedores de valores que afectan al juego.

### Modificadores Estáticos
Se definen en `common/static_modifiers`.
- **Sintaxis:**
  ```pdx
  mi_nombre_de_modifier = {
      icon = "path/to/icon.dds"
      key_del_modificador = valor
  }
  ```
- **Aplicación:** Se añaden mediante el efecto `add_modifier`.
  ```pdx
  add_modifier = {
      name = mi_nombre_de_modifier
      months = 12 # Duración (opcional)
  }
  ```

## 3. Decisiones (Decision Modding)
Las decisiones son scripts que el jugador o la IA activan manualmente.
- **is_shown:** Determina si la decisión aparece en el menú.
- **possible:** Determina si el botón está habilitado para hacer click.
- **when_taken:** El bloque de efectos que ocurre al pulsar el botón.
- **ai_chance:** Probabilidad de que la IA tome la decisión.

## 4. Scripting (Triggers y Effects)
El "idioma" de modding de Victoria 3 se basa en **Scopes**, **Triggers** y **Effects**.

### Scopes (Ámbitos)
Toda línea de código corre en un ámbito específico:
- `country`: El país completo.
- `state`: Una región específica.
- `character`: Un líder o comandante.
- `pop`: Un grupo de población.

### Triggers (Condiciones)
Devuelven "Verdadero" o "Ffalso".
- `has_technology_researched = tech_name`
- `has_law = law_name`
- `owns_entire_state_region = s:STATE_ID`
- `is_ai = no`

### Effects (Acciones)
Ejecutan un cambio en el juego.
- `activate_law = law_name`
- `add_treasury = 5000`
- `annex = c:TAG`
- `add_modifier = { name = x days = y }`
- `create_incident = incident_name`
- `create_diplomatic_pact = { country = c:TAG type = puppet }`

## 5. Biblioteca de Modificadores (Referencias de Valor)
Lista de comandos técnicos verificados:

### País (Globales)
- `country_infamy_decay_mult`: Decaimiento de infamia.
- `country_law_enactment_success_add`: Éxito en leyes.
- `country_minting_mult`: Acuñación de moneda.
- `country_prestige_mult`: Prestigio nacional.

### Estado (Locales)
- `state_construction_mult`: Velocidad de construcción.
- `state_migration_pull_mult`: Atracción migratoria.
- `state_birth_rate_mult`: Tasa de natalidad (+0.02 = 2%).
- `state_standard_of_living_add`: Nivel de vida plano.

## 6. Herramientas de Desarrollo (Debug Mode)
- **Activar:** Iniciar el juego con el parámetro `-debug_mode`.
- **Consola:** Tecla `^` o `~`.
- **Comandos útiles:**
  - `script_docs`: Genera toda la documentación técnica de triggers y efectos en la carpeta `Documents/.../Victoria 3/logs`.
  - `DumpDataTypes`: Genera los tipos de datos para autocompletado en VS Code.

---
*Este documento es una síntesis técnica. Para detalles visuales o de balanceo, consultar la Wiki online.*

## 7. Solución de Problemas (Troubleshooting)
- **Decisión no aparece:** 
    1. Asegurarse de que el archivo tenga codificación **UTF-8 con BOM**.
    2. Intentar consolidar la decisión en un archivo existente que sí funcione (ej: `force_usa_borders.txt`).
    3. Verificar que el bloque `is_shown` no sea demasiado restrictivo.
