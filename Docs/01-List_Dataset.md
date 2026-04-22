# Estructura del DataSet
# 01 — Estructura del DataSet
## Inventario TI | CUR Carazo — UNAN Managua

> **Proyecto:** DashboardV1_MGA  
> **Institución:** Centro Universitario Regional de Carazo — UNAN Managua  
> **Versión:** 1.2  
> **Archivo fuente:** `Inventario_CurCarazo.csv`  
> **Total de registros:** ~195 activos TI  
> **Última actualización de este documento:** Abril 2026  

---

## Descripción General

El dataset `Inventario_CurCarazo.csv` contiene el registro completo de los activos tecnológicos asignados a las diferentes áreas, departamentos y laboratorios del Centro Universitario Regional de Carazo (CUR Carazo) de la UNAN Managua. Incluye equipos de cómputo (Desktop, Laptop), impresoras, teléfonos, UPS y otros dispositivos TI distribuidos en los distintos edificios e instalaciones de la institución.

---

## Estructura de Columnas

El dataset está compuesto por **19 columnas** organizadas de la siguiente manera:

| # | Nombre del Campo | Tipo de Dato | Descripción | Ejemplo |
|---|-----------------|--------------|-------------|---------|
| 1 | `ID` | Entero | Identificador numérico único autoincremental de cada registro en la tabla | `1`, `25`, `160` |
| 2 | `Titulo CURCarazo` | Texto | Código de identificación institucional del activo. Sigue el formato `CURC-XXXX` donde XXXX es un número secuencial de 4 dígitos. Los registros con `CURC-0000` indican código pendiente de asignar. | `CURC-0001`, `CURC-0033` |
| 3 | `Nombre de usuario` | Texto | Nombre completo del usuario o área responsable asignada al equipo. Puede estar vacío si el equipo no tiene usuario asignado o es de uso compartido (laboratorio). | `Daysi Marilu Hernandez Guevara` |
| 4 | `Nombre del equipo` | Texto | Nombre de red (hostname) del dispositivo tal como aparece en el sistema operativo o en el dominio de red institucional. | `CONTABCA-03`, `LABCA01-PC01`, `CARAZO007` |
| 5 | `Estado` | Texto | Condición física y funcional actual del activo. Valor predominante: `Bueno`. Puede quedar vacío en registros sin verificación reciente. | `Bueno` |
| 6 | `Ubicación` | Texto | Lugar físico donde se encuentra el activo dentro del campus (edificio, sala, oficina o instalación). Incluye tanto ubicaciones internas como sedes externas. | `Contabilidad`, `Edificio B`, `Torreón Eliseo Carranza, Jinotepe - Carazo` |
| 7 | `Área` | Texto | Departamento, unidad académica o área funcional a la que pertenece el activo. Se usa como categoría para filtros y agrupaciones en el dashboard. | `Contabilidad`, `RRHH`, `Registro`, `Laboratorio C1`, `Ciencias Económicas` |
| 8 | `IP` | Texto | Dirección IP asignada al dispositivo en la red institucional. Puede corresponder a las subredes `10.1.x.x`, `172.16.x.x` o `192.168.x.x`. Puede estar vacía si el equipo no tiene IP fija o no está conectado. | `10.1.136.197`, `172.16.135.74` |
| 9 | `MAC` | Texto | Dirección(es) MAC de los adaptadores de red del equipo. Para equipos con doble adaptador (Ethernet + Wi-Fi), ambas direcciones se registran en el mismo campo separadas por espacio. El formato puede variar entre registros (`XX-XX-XX-XX-XX-XX` o `XX:XX:XX:XX:XX:XX`). | `Ethernet A4-BB-6D-A1-40-2B WI-FI D0-37-45-A8-95-8B` |
| 10 | `Dispositivo` | Texto | Tipo o categoría del activo tecnológico registrado. | `Desktop`, `Laptop`, `Impresora`, `Telefono`, `UPS`, `CPU` |
| 11 | `Marca` | Texto | Fabricante o marca comercial del dispositivo. Puede estar vacío para equipos sin etiqueta identificable o ensamblados. | `Dell`, `HP`, `Lenovo`, `Grandstream`, `CDP`, `AOC` |
| 12 | `Procesador` | Texto | Modelo de procesador instalado en el equipo. Se registra en formato normalizado para facilitar el filtrado por tipo de CPU. | `Intel_Core_i3`, `Intel_Core_i5`, `Intel_Core_i7` |
| 13 | `Gen_Procesador` | Texto | Generación del procesador. Permite segmentar la antigüedad tecnológica del parque informático. | `Gen_5`, `Gen_6`, `Gen_7`, `Gen_8`, `Gen_9`, `Gen_10`, `Gen_11`, `Gen_12`, `Gen_13` |
| 14 | `RAM` | Texto | Capacidad de memoria RAM instalada en el equipo, expresada en GB. | `4 GB RAM`, `8 GB RAM`, `12 GB RAM`, `16 GB RAM`, `24 GB RAM` |
| 15 | `Sistema Operativo` | Texto | Sistema operativo instalado en el dispositivo. | `Windows 10`, `Windows 11` |
| 16 | `Edición del S.O` | Texto | Edición específica del sistema operativo. | `Pro`, `Enterprise`, `Education` |
| 17 | `Office` | Texto | Suite de productividad de Microsoft instalada en el equipo. Vacío si no tiene Office instalado o no fue verificado. | `365`, `Office 2016` |
| 18 | `Edición de Office` | Texto | Edición específica de Microsoft Office. Campo reservado para uso futuro; actualmente sin valores en los registros. | *(vacío)* |
| 19 | `Inventario` | Texto | Número de inventario físico asignado institucionalmente al activo. Puede incluir sufijos alfabéticos (`-A`) para activos relacionados o con variantes. | `85316`, `85316-A` |

---

## Observaciones y Consideraciones de Calidad

| Observación | Detalle |
|-------------|---------|
| **Código CURC-0000** | Indica registros donde aún no se ha asignado un código institucional formal. Requiere revisión. |
| **Campos MAC combinados** | El campo `MAC` puede contener dos direcciones (Ethernet + Wi-Fi) en un solo valor de texto, separadas por espacio. Para análisis se recomienda normalizar o dividir en dos columnas. |
| **IPs en campo MAC** | Algunos registros presentan direcciones IP dentro del campo `MAC` por error de captura (ej: `ETHERNET 169.254.54.11`). |
| **Inconsistencia en formato MAC** | Se usan indistintamente los separadores `-` y `:` en las direcciones MAC. |
| **Campos vacíos en dispositivos no-PC** | Impresoras, teléfonos y UPS tienen la mayoría de campos técnicos (procesador, RAM, SO) vacíos por no aplicar. |
| **Campo `Edición de Office`** | Se encuentra vacío en todos los registros actuales. Está reservado para uso futuro. |
| **Ubicación vs Área** | En algunos registros la `Ubicación` es la descripción del espacio físico mientras que el `Área` es la unidad funcional. En otros casos se superponen o difieren (ej: `Ubicación: Bioanálisis`, `Área: Contabilidad`). |

---

## 📌 Notas de Implementación para Power BI

- El campo `IP` debe tratarse como **texto**, no como número, para preservar el formato de red.
- El campo `MAC` requiere transformación en Power Query para separar la dirección Ethernet de la Wi-Fi si se desea analizar por separado.
- Se recomienda crear una columna calculada **`Antigüedad_Generacional`** derivada de `Gen_Procesador` para clasificar equipos como: *Obsoleto*, *En transición* o *Vigente*.
- El campo `Inventario` debe mantenerse como texto para conservar los sufijos `-A`, `-B`.

---
