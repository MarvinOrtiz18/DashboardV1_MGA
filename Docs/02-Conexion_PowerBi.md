# 02. Conexión a Power BI desde SharePoint privado
## Crear Conexión
Esta guía aplica para escenarios de análisis de datos en Power BI donde el origen es una lista privada de SharePoint Online y se requiere, como en nuestro c:

- Conectar Power BI a una lista de SharePoint.
- Validar autenticación y permisos.
- Transformar columnas desde Power Query.
- Normalizar el campo **Nombre de usuario**.
- Preparar el modelo para análisis y visualización.

## Origen de datos
- **Plataforma:** SharePoint Online
- **Sitio privado:** TICFAREMCARAZO2
- **Lista:** Inventario_CurCarazo

## Requisitos previos
Antes de conectar Power BI, asegúrese de contar con lo siguiente:

1. Acceso al sitio privado de SharePoint.
2. Permisos de lectura sobre la lista **Inventario_CurCarazo**.
3. Credenciales institucionales válidas.
4. Power BI Desktop instalado.

## Conexión de SharePoint a Power BI
### 1. Abrir Power BI Desktop

En Power BI Desktop, vaya a:

**Inicio > Obtener datos > Más**
### 2. Seleccionar el conector correcto

Use el conector **SharePoint Online List** para conectar directamente a la lista.

### 3. Ingresar la URL del sitio
Use la URL raíz del sitio SharePoint, no la URL específica de la lista.

```text
https://<tenant>.sharepoint.com/sites/TICFAREMCARAZO2
```

### 4. Autenticación
Seleccione el método de autenticación adecuado, normalmente:

- **Cuenta organizacional**

Inicie sesión con la cuenta institucional autorizada.
### 5. Elegir la lista

Cuando se cargue el contenido, localice la lista:

- **Inventario_CurCarazo**

Seleccione la lista y haga clic en **Transformar datos**.
## Carga inicial o transformación en Power Query

Al abrir el editor de Power Query, se recomienda revisar el origen antes de cargarlo al modelo. La práctica correcta en análisis de datos es:

1. Revisar nombres de columnas.
2. Validar tipos de datos.
3. Eliminar columnas innecesarias.
4. Normalizar valores nulos o inconsistentes.
5. Crear columnas derivadas solo cuando aporten valor analítico.

## Transformación correcta de datos

- Mantener las columnas originales mientras se valida la calidad de la información.
- Asignar tipos de datos adecuados:
	- Texto para nombres, correos y descripciones.
	- Número para cantidades o identificadores numéricos.
	- Fecha para campos temporales.
- Evitar transformar el dato original si se necesita trazabilidad.
- Crear columnas nuevas para análisis cuando una columna contenga más de un dato lógico.

### Limpieza inicial sugerida
En Power Query, aplique estas acciones según sea necesario:

- Quitar espacios al inicio y al final.
- Reemplazar valores vacíos por nulos cuando aplique.
- Homologar textos en mayúsculas/minúsculas si el análisis lo requiere.
- Verificar duplicados en campos clave.
- Revisar columnas con formato mixto.

## Transformación del campo `Nombre de usuario`
En este escenario, el campo **Nombre de usuario** almacena el correo de la persona dentro del dominio institucional. El objetivo es separar este valor en dos columnas independientes:

- **Nombre del usuario**
- **Correo**

## Reglas de transformación

El contenido del campo puede venir en un formato como este:

```text
nombre.apellido@institucion.edu.ni
```

Si el campo contiene únicamente el correo institucional, entonces:
- La columna **Correo** conserva el valor completo.
- La columna **Nombre del usuario** se deriva a partir del prefijo del correo.

### Lógica recomendada
1. Tomar el valor completo del campo **Nombre de usuario**.
2. Crear una columna nueva para **Correo** con el valor original.
3. Separar el texto antes del símbolo `@` para obtener el nombre de usuario técnico.
4. Si se requiere un nombre más legible, transformar el texto con reemplazos:
	- cambiar puntos `.` por espacios;
	- capitalizar palabras si corresponde;
	- conservar el criterio institucional definido para nombres.

## Ejemplo de implementación en Power Query
### Columna Correo

Crear una columna personalizada con el valor original:

```powerquery
[Nombre de usuario]
```

### Columna Nombre del usuario
Extraer el texto antes de `@`:

```powerquery
Text.BeforeDelimiter([Nombre de usuario], "@")
```

### Columna Nombre del usuario legible
Si se desea convertir el identificador en un nombre más amigable:

```powerquery
Text.Proper(Text.Replace(Text.BeforeDelimiter([Nombre de usuario], "@"), ".", " "))
```

## Resultado esperado
Si el valor original es:

```text
juan.perez@unan.edu.ni
```

El resultado debe ser:
| Nombre de usuario técnico | Nombre del usuario | Correo |
|---|---|---|
| juan.perez | Juan Perez | juan.perez@unan.edu.ni |

### 6. Validar la calidad de los datos
Revisar:

- valores vacíos;
- correos mal formados;
- registros duplicados;
- diferencias en formatos de fecha o texto.