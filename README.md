# Guía de Desarrollo y Documentación Técnica - Mod Force Borders

Este documento detalla el proceso técnico, los scripts y la lógica interna descubierta durante el desarrollo del mod **Force Borders** para Victoria 3.

## 1. El Problema de los IDs de Estados
En Victoria 3, los nombres que ves en el mapa ("Mendoza", "Córdoba") **no coinciden** con los IDs internos que usa el código. Esto se debe a razones históricas del desarrollo del juego o decisiones de diseño.

**Ejemplos descubiertos:**
*   **Visual:** "Mendoza" -> **Código:** `s:STATE_RIO_NEGRO`
*   **Visual:** "Córdoba" -> **Código:** `s:STATE_LA_PAMPA`
*   **Visual:** "La Pampa" -> **Código:** *Probablemente parte de otro estado mayor como Buenos Aires o Patagonia en 1836.*

Para crear decisiones de fronteras, es **obligatorio** usar el ID técnico (`s:STATE_...`), no el nombre visual.

## 2. El "Circuito" Manual de Descubrimiento (Método Inicial)
Cuando no conocíamos los IDs, utilizamos un proceso de triangulación en 3 pasos:

### Paso A: Búsqueda por Tag (`c:ARG`)
Buscamos qué estados pertenecían a Argentina al inicio del juego en `00_states.txt`.
> `Select-String -Path "00_states.txt" -Pattern "c:ARG"`

### Paso B: Búsqueda por Cultura (`cu:platinean`)
Para encontrar estados que Argentina *debería* tener (pero no tiene al inicio, como la Patagonia), buscamos estados con la cultura "platinean" como patria (`add_homeland`).
> `Select-String -Path "00_states.txt" -Pattern "cu:platinean"`

### Paso C: La Revelación (Archivos de Localización)
Cuando un ID no tenía sentido (ej: `STATE_RIO_NEGRO` apareciendo donde debería estar `MENDOZA`), fuimos a los archivos de traducción del juego (`localization/spanish/map/states_l_spanish.yml`).
Allí se encuentra la "piedra rosetta":
`STATE_RIO_NEGRO: "Mendoza"` -> **Esto confirma que para el juego, Mendoza ES Río Negro.**

## 3. Método Optimizado (Script de PowerShell)
Para implementar Chile, automatizamos los 3 pasos anteriores en un solo script de PowerShell. Este script cruza la información interna con las traducciones automáticamente.

Puedes guardar este código como `buscar_estados.ps1` y ejecutarlo para cualquier país cambiando la variable `$tag`.

```powershell
# Configuración
$tag = "c:CHL" # Cambia esto por el tag del país (ej: c:BRA, c:PER)
$statesFile = "d:\SteamLibrary\steamapps\common\Victoria 3\game\common\history\states\00_states.txt" # Ajusta tu ruta
$locFile = "d:\SteamLibrary\steamapps\common\Victoria 3\game\localization\spanish\map\states_l_spanish.yml" # Ajusta tu ruta

# 1. Obtener IDs de estados relacionados con el tag
$content = Get-Content $statesFile
$relatedStates = New-Object System.Collections.Generic.List[string]
$currentState = ""

foreach ($line in $content) {
    # Captura el ID del estado actual (s:STATE_NOMBRE)
    if ($line -match "s:STATE_(.*) = \{") {
        $currentState = $matches[1]
    }
    # Si el estado tiene el tag del país O su cultura principal (ajustar segun necesario)
    if ($line -match $tag) {
        if ($currentState -and !$relatedStates.Contains($currentState)) {
            $relatedStates.Add($currentState)
        }
    }
}

# 2. Buscar nombres visuales en español
$locContent = Get-Content $locFile
$results = New-Object System.Collections.Generic.List[string]

foreach ($stateId in $relatedStates) {
    # Busca la traducción exacta en el archivo YML
    $pattern = "STATE_$stateId`: `"(.*)`""
    foreach ($locLine in $locContent) {
        if ($locLine -match $pattern) {
            $results.Add("ID: STATE_$stateId -> Nombre Visual: $($matches[1])")
            break
        }
    }
}

# 3. Mostrar resultados
$results
```

**Por qué funciona mejor:**
Evita tener que adivinar. Te dice exactamente: *"El estado que el juego llama X, el jugador lo ve como Y"*.

## 4. Estructura y Codificación (Critical)
Dos reglas de oro aprendidas durante el proceso:

1.  **Codificación UTF-8 BOM:** Victoria 3 requiere que los archivos de texto (`.txt`) y localización (`.yml`) se guarden con **UTF-8 con BOM (Byte Order Mark)**. Si usas UTF-8 simple, el juego puede mostrar caracteres extraños o no leer el archivo.
    *   *En PowerShell:* `New-Object System.Text.UTF8Encoding $true` activa el BOM.

2.  **Estructura Modular:**
    *   Es mejor separar cada país en su propio archivo (`force_arg_borders.txt`, `force_chl_borders.txt`) en lugar de tener uno gigante. Esto facilita borrar o corregir un país sin romper los demás.
    *   La carpeta `.metadata` es vital para que el Launcher reconozca el mod.

## 5. Sintaxis de Decisiones (`when_taken`)
Un error común es colocar los efectos fuera del bloque correcto.
*   **Correcto:**
    ```pdx
    when_taken = {
        c:ARG = { activate_law = ... } # El efecto ocurre AL TOMAR la decisión
    }
    ```
*   **Incorrecto:**
    ```pdx
    when_taken = { ... }
    c:ARG = { activate_law = ... } # Esto queda flotando y el juego lo ignora o da error
    ```

## 6. Aprendizajes del Caso Australia (Conceptos Avanzados)
Durante la implementación de Australia (`c:AST`), descubrimos matices importantes:

### A. Cultura vs Tag
Al buscar por el Tag `c:AST`, inicialmente encontramos pocos resultados. Sin embargo, al extender la búsqueda a su cultura principal (`cu:australian`), el script encontró un **superset** de estados que incluía Nueva Zelanda.
*   **Lección:** Si estás creando un país "formable" (que no existe al inicio, como Australia), buscar por **Cultura** (`add_homeland`) suele ser más efectivo que buscar por Tag de propietario.

### B. Territorios "Opcionales"
A veces, lo "histórico" es debatible (ej: ¿Australia incluye Nueva Zelanda?).
Para manejar esto sin borrar código útil, aprendimos a dejar líneas **comentadas** en el archivo de decisión.
```pdx
# Nueva Zelanda (Opcional)
# s:STATE_NORTH_ISLAND = { ... }
```

## 7. Solución de Problemas (Troubleshooting)

### El juego muestra el código en vez del texto (ej: `force_historical_ast_borders`)
Si ves el código interno en la interfaz en lugar del nombre bonito, significa que el archivo de localización (`.yml`) tiene un problema.
**Posibles causas:**
1.  **Faltan las líneas:** Asegúrate de haber agregado la entrada en el archivo `localization/spanish/force_borders_l_spanish.yml`.
2.  **Indigo / Espacios:** YAML es muy estricto. Todas las claves deben tener **2 espacios** de sangría respecto a `l_spanish:`.
    ```yaml
    l_spanish:
      mi_clave: "Texto"  # Correcto
    mi_clave: "Texto"    # Incorrecto (falta sangría)
    ```
3.  **Comillas:** Asegúrate de que el texto esté entre comillas dobles `"` y si usas comillas dentro del texto, escápalas.
4.  **Codificación:** Recuerda siempre guardar como **UTF-8 con BOM**.
