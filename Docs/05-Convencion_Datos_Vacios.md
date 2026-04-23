#  05 — Convención de Datos Vacíos
## Inventario TI | CUR Carazo — UNAN Managua

> **Proyecto:** DashboardV1_MGA  
> **Aplica a:** SharePoint List (origen) + Power BI / Power Query (presentación)  
> **Versión:** 1.0  
> **Última actualización:** Abril 2026

---

## NULL

Garantizar que ningún campo del inventario quede con valor **"En blanco"** al ser procesado en Power BI, estableciendo una convención estándar de valores nulos según el tipo de campo y su semántica dentro del dataset.

La estandarización opera en **dos capas**:

- 🔵 **Capa 1 — SharePoint List:** prevención en el origen al crear o editar registros
- 🟡 **Capa 2 — Power Query (Power BI):** limpieza defensiva sobre registros históricos o con datos incompletos

---

## Convenciones Definidas

| Convención | Significado | Cuándo usarla |
|------------|-------------|---------------|
| **Obligatorio** | El campo no puede quedar vacío. Se marca como *Required* en SharePoint | Campos que identifican o clasifican el activo de forma única |
| **`Pendiente`** | El dato existe en la realidad pero aún no fue capturado o verificado | Estado del equipo, información en proceso de levantamiento |
| **`S/D`** | Sin Dato — el campo aplica al dispositivo pero no se cuenta con la información | Nombre de usuario, marca, número de inventario |
| **`N/A`** | No Aplica — el campo no tiene sentido para ese tipo de dispositivo | Procesador/RAM en impresoras, teléfonos, UPS |
| **`Sin Conexión`** | El dispositivo no tiene red asignada o no está conectado | Dirección IP y MAC en equipos sin interfaz de red activa |

---

## Tabla de Convención por Campo

| # | Campo | Convención Vacío | Tipo en SharePoint | Notas |
|---|-------|------------------|--------------------|-------|
| 1 | `Titulo CURCarazo` | **Obligatorio** | Texto *(Required)* | Código institucional único. Formato: `CURC-XXXX` |
| 2 | `Nombre de usuario` | `S/D` | Texto con valor por defecto | Vacío si el equipo es compartido o de laboratorio |
| 3 | `Nombre del equipo` | `S/D` | Texto con valor por defecto | Hostname de red del dispositivo |
| 4 | `Estado` | `Pendiente` | Choice con valor por defecto | Opciones: `Bueno`, `Regular`, `Malo`, `Pendiente` |
| 5 | `Ubicación` | **Obligatorio** | Choice *(Required)* | Espacio físico dentro del campus |
| 6 | `Área` | **Obligatorio** | Choice *(Required)* | Unidad funcional o departamento responsable |
| 7 | `IP` | `Sin Conexión` | Texto con valor por defecto | Tratar siempre como texto, no como número |
| 8 | `MAC` | `Sin Conexión` | Texto con valor por defecto | Puede contener dos MACs (Ethernet + Wi-Fi) |
| 9 | `Dispositivo` | **Obligatorio** | Choice *(Required)* | Opciones: `Desktop`, `Laptop`, `Impresora`, `Telefono`, `UPS`, `CPU` |
| 10 | `Marca` | `S/D` | Texto con valor por defecto | Fabricante del equipo |
| 11 | `Procesador` | `N/A` | Choice con valor por defecto | `N/A` para impresoras, teléfonos, UPS |
| 12 | `Gen_Procesador` | `N/A` | Choice con valor por defecto | `N/A` para dispositivos sin CPU identificable |
| 13 | `RAM` | `N/A` | Choice con valor por defecto | `N/A` para dispositivos no computacionales |
| 14 | `Sistema Operativo` | `N/A` | Choice con valor por defecto | `N/A` para dispositivos sin SO de propósito general |
| 15 | `Edición del S.O` | `N/A` | Choice con valor por defecto | Opciones: `Pro`, `Enterprise`, `Education`, `N/A` |
| 16 | `Office` | `N/A` | Choice con valor por defecto | Opciones: `365`, `Office 2016`, `N/A` |
| 17 | `Edición de Office` | `N/A` | Choice con valor por defecto | Campo reservado para uso futuro |
| 18 | `Inventario` | `S/D` | Texto con valor por defecto | Número físico de inventario institucional |

---

## Capa 1 — Configuración en SharePoint List

### Campos Obligatorios *(Required)*
Configurar como **requeridos** en la configuración de columna de la lista:

```
✔ Titulo CURCarazo
✔ Dispositivo
✔ Ubicación
✔ Área
```

### Campos con Valor por Defecto
En la configuración de cada columna, establecer el **Default Value** según la tabla anterior:

- Columnas tipo **Choice** → agregar las opciones `N/A`, `S/D` o `Pendiente` según corresponda y marcar una como predeterminada
- Columnas tipo **Texto** → establecer el valor predeterminado directamente como `S/D` o `Sin Conexión`

---

## Capa 2 — Limpieza en Power Query (Power BI)

Aplicar los siguientes pasos en **Power Query Editor** para sanear registros históricos con valores nulos o en blanco. Agregar como pasos consecutivos en la consulta del dataset:

```m
// Paso 1 — Campos con convención S/D
= Table.ReplaceValue(
    #"Origen",
    null, "S/D",
    Replacer.ReplaceValue,
    {"Nombre de usuario", "Nombre del equipo", "Marca", "Inventario"}
)

// Paso 2 — Campos de red con Sin Conexión
= Table.ReplaceValue(
    #"Paso 1",
    null, "Sin Conexión",
    Replacer.ReplaceValue,
    {"IP", "MAC"}
)

// Paso 3 — Campos técnicos con N/A
= Table.ReplaceValue(
    #"Paso 2",
    null, "N/A",
    Replacer.ReplaceValue,
    {"Procesador", "Gen_Procesador", "RAM", "Sistema Operativo", "Edición del S.O", "Office", "Edición de Office"}
)

// Paso 4 — Campo Estado con Pendiente
= Table.ReplaceValue(
    #"Paso 3",
    null, "Pendiente",
    Replacer.ReplaceValue,
    {"Estado"}
)

// Paso 5 — Reemplazar también cadenas vacías "" (no solo null)
= Table.ReplaceValue(
    #"Paso 4",
    "", "S/D",
    Replacer.ReplaceValue,
    {"Nombre de usuario", "Nombre del equipo", "Marca", "Inventario"}
)
```

> **Nota:** Power BI distingue entre `null` (valor nulo) y `""` (cadena vacía). Ambos deben reemplazarse. SharePoint a veces exporta campos vacíos como cadena vacía en lugar de null.