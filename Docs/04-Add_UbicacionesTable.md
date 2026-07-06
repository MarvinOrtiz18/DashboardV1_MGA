# Creación de la tabla Edificios y relación con Inventario_CurCarazo

## Edificios

La tabla **Edificios** se utiliza para describir los espacios físicos del CUR Carazo donde se concentran los activos del inventario. En lugar de repetir la misma información de ubicación en cada registro del inventario, se crea una tabla independiente con un registro por edificio o ubicación principal.

Con este enfoque se logra:

- mantener una sola versión de la información de ubicación;
- mejorar la limpieza y normalización del modelo;
- facilitar el filtrado por edificio en reportes y visualizaciones;
- habilitar una relación maestro-detalle entre ubicaciones y activos.

---

## 2. Campos de la tabla Edificios

En la imagen del modelo, la tabla **Edificios** contiene los siguientes campos:

| Campo | Descripción |
|---|---|
| `Ubicación_Maps` | Nombre de la ubicación o edificio. Es el campo principal de referencia y el que se relaciona con `Inventario_CurCarazo`.
| `Centro Universitario` | Sede o centro al que pertenece el edificio. En este proyecto el valor observado es `Carazo`.
| `Latitud` | Coordenada geográfica en formato decimal para ubicar el edificio en mapas o visuales geoespaciales.
| `Longitud` | Coordenada geográfica en formato decimal para ubicar el edificio en mapas o visuales geoespaciales.
| `URLImagen` | Dirección web de una imagen asociada al edificio, útil para tarjetas, paneles informativos o visuales enriquecidos.

### Ejemplos observados

En los registros mostrados en la imagen aparecen ubicaciones como:

- Biblioteca Lic. Rafael Sanchez R UNAN FAREM-Carazo.
- UNAN MANAGUA, CUR-CARAZO. Edificio de la Dirección, UNICAM y el Dep. de Ciencia Tecnología y Salud.
- UNAN Managua, CUR-Carazo. Edificio Dr. Juan José Sánchez Flores (Edificio A).
- UNAN - CUR Carazo Campus Augusto C. Sandino.
- UNAN-Managua, FAREM-Carazo. Edificio Msc. Fernando Fernandez. (Edificio C).
- Torreón Universitario de la Facultad Regional Multidisciplinaria de Carazo.
- Hospital Regional Santiago.
- Academia de Idiomas Héroe Brian Willson-CUR Carazo.

Estos valores son los que se usarán como referencia visual y de navegación dentro del dashboard.

---

## 3. Creación de la tabla Edificios

La tabla puede crearse en el modelo semántico de Power BI como una tabla independiente con los edificios o ubicaciones maestras del campus. El objetivo no es almacenar inventario, sino definir la referencia espacial que luego consumirá la tabla detalle.

### Pasos generales

1. Crear una nueva tabla en el modelo.
2. Definir una fila por cada edificio o ubicación principal.
3. Capturar el nombre de la ubicación en `Ubicación_Maps`.
4. Asignar el `Centro Universitario` correspondiente.
5. Registrar la `Latitud` y `Longitud`.
6. Incluir la `URLImagen` del edificio si se utilizará en visuales.
7. Guardar la tabla y validar que cada ubicación sea única.

---

## 4. Relación con Inventario_CurCarazo

La tabla **Edificios** se relaciona con **Inventario_CurCarazo** mediante el campo `Ubicación_Maps`.

### Tipo de relación

La relación configurada en el modelo es:

- **Edificios**: lado **uno** (`1`)
- **Inventario_CurCarazo**: lado **muchos** (`*`)

Esto significa que un edificio puede tener muchos registros de inventario asociados, pero cada registro del inventario debe apuntar a una sola ubicación principal.

### Cardinalidad

La cardinalidad mostrada es **varios a uno (*:1)** desde la tabla detalle hacia la tabla maestra.

### Dirección de filtro cruzado

En la configuración observada, la dirección de filtro cruzado está establecida en **Ambas**.

Eso permite que los filtros aplicados sobre cualquiera de las dos tablas impacten la otra. En un escenario maestro-detalle, esto facilita el análisis, aunque conviene validar su uso en medidas complejas para evitar ambigüedades en el modelo.

### Activación de la relación

La relación debe permanecer **activa** para que el modelo use correctamente la tabla maestra en el análisis de inventario.

---

## 5. Cómo se usa en el modelo

Esta relación permite que la tabla **Edificios** actúe como dimensión o catálogo de ubicaciones, mientras que **Inventario_CurCarazo** almacena los activos asignados a cada edificio.

## 6. Ventajas del enfoque maestro-detalle

Usar **Edificios** como maestro y **Inventario_CurCarazo** como detalle aporta estas ventajas:

- elimina duplicidad en datos de ubicación;
- mejora la consistencia de nombres de edificios;
- permite agregar atributos geográficos sin tocar la tabla de inventario;
- facilita reportes por sede, edificio o mapa;
- hace más mantenible el modelo cuando cambian las ubicaciones.

---

## 7. Reglas de calidad de datos

Para que la relación funcione correctamente, se recomienda cumplir con estas reglas:

- `Ubicación_Maps` debe tener el mismo texto en ambas tablas.
- No deben existir valores duplicados en la tabla **Edificios** para la misma ubicación.
- Las coordenadas `Latitud` y `Longitud` deben mantenerse en formato numérico decimal.
- `URLImagen` debe contener rutas válidas y accesibles.
- Si un activo no tiene ubicación asignada, debe revisarse antes de cargarlo al modelo.

---