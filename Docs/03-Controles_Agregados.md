# Controles agregados en Power BI

Este documento explica cómo añadir y usar objetos visuales externos desde la galería "Obtener más objetos visuales" (Get more visuals) en Power BI Desktop, con énfasis en los controles Text Filter y HTML Content aplicados al proyecto DashboardV1_MGA.


## 1. Acceder a "Obtener más objetos visuales"
1. Abrir Power BI Desktop.
2. En el panel Visualizations, hacer clic en los tres puntos (...) en la parte inferior.
3. Seleccionar "Obtener más objetos visuales" (Get more visuals).
4. Se abrirá la AppSource de Power BI: buscar por nombre (ej. "Text Filter", "HTML Content") o navegar por categorías.
5. Hacer clic en "Agregar" (Add) para descargar e instalar el visual en el reporte actual.

## 2. Text Filter
### Descripción
Text Filter es un visual que permite filtrar datos por texto libre (coincidencia parcial, exacta, expresiones) y suele ser más flexible que el slicer nativo para texto.

### Instalación
- Buscar "Text Filter" en AppSource e instalar.

### Buenas prácticas
- Para grandes volúmenes de datos, limitar el campo a columnas indexadas o pre-filtradas para mejorar rendimiento.
- Añadir un botón o instrucción clara para resetear el filtro.

## 3. HTML Content
### Descripción
HTML Content permite renderizar HTML (texto enriquecido, tablas, imágenes embebidas) dentro de un visual en Power BI. Útil para mostrar descripciones ricas, informes formateados o contenido generado dinámicamente.

### Consideraciones de seguridad
- Power BI restringe ciertos elementos por seguridad (scripts, iframes). Evitar confiar en HTML no sanitizado.
- Revisar la política de la organización antes de subir contenido que provenga de fuentes externas.

### Instalación
- Buscar "HTML Content" en AppSource e instalar.

## 4. Integración recomendada para DashboardV1_MGA
- Añadir Text Filter en la parte superior de páginas con búsquedas textuales frecuentes (p. ej. búsqueda de cliente o producto).
- Usar HTML Content en paneles de detalle o modal-like (ficha enriquecida) para mostrar información contextual.
- Documentar en el repositorio (README de la carpeta Docs) las versiones de los visuales y la fecha de instalación.

## 5. Rendimiento y mantenimiento
- Monitorizar el rendimiento tras la incorporación de visuales externos.
- Mantener actualizados los visuales desde AppSource y revisar cambios de versión.

## 6. Licencias y cumplimiento
- Verificar licencia del visual en AppSource (algunos son de pago o requieren permisos adicionales).
