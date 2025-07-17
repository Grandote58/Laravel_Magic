# 🏗️ **Lección 2: Creación de Tablas SQL (MySQL)**

## 🎯 Objetivo de la Lección

Aprender a crear tablas correctamente en MySQL utilizando la instrucción `CREATE TABLE`, comprendiendo la importancia de los **tipos de datos**, **claves primarias**, **restricciones** y **estructura relacional**.

## 🎓 Meta de Aprendizaje

Al finalizar esta lección, el estudiante podrá:

- Crear tablas MySQL utilizando sintaxis correcta.
- Elegir tipos de datos adecuados para los atributos.
- Establecer claves primarias y restricciones como `NOT NULL` y `UNIQUE`.
- Comprender la diferencia entre `CHAR`, `VARCHAR`, `INT`, `DATE`, etc.
- Comenzar con buenas prácticas de diseño de bases de datos.

## 🧠 Conceptos Clave

| Concepto         | Descripción                                                  |
| ---------------- | ------------------------------------------------------------ |
| `CREATE TABLE`   | Instrucción para crear una tabla en MySQL                    |
| **Atributo**     | Una columna en una tabla                                     |
| **Tipo de Dato** | Define el tipo de valor que puede almacenarse (ej: `INT`, `VARCHAR`) |
| `PRIMARY KEY`    | Identificador único de cada fila                             |
| `NOT NULL`       | Evita valores nulos                                          |
| `DEFAULT`        | Valor por defecto si no se proporciona uno                   |
| `UNIQUE`         | El valor debe ser único en toda la columna                   |

## 💡 Ejemplo Real: Creando Tabla Cliente

Supongamos que queremos crear una tabla  `Cliente` para una tienda en línea:

```sql
CREATE TABLE Cliente (
  id_cliente INT AUTO_INCREMENT PRIMARY KEY,
  nombre VARCHAR(100) NOT NULL,
  correo VARCHAR(100) UNIQUE,
  telefono VARCHAR(15),
  fecha_registro DATE DEFAULT CURRENT_DATE
);
```

![image-20250717115408667](C:/Users/juanc/AppData/Roaming/Typora/typora-user-images/image-20250717115408667.png)

✅ Buenas prácticas utilizadas:

- `AUTO_INCREMENT`: el ID se genera automáticamente.
- `NOT NULL`: el nombre es obligatorio.
- `UNIQUE`: el correo no se puede repetir.
- `DEFAULT CURRENT_DATE`: la fecha de registro se asigna automáticamente.

## 🧪 Práctica Aplicada

🎯 **Crea una tabla `Producto` con los siguientes campos:**

- id_producto (clave primaria, autoincrementable)
- nombre (obligatorio)
- descripcion (opcional)
- precio (decimal, obligatorio)
- stock (entero, valor por defecto 0)

### 🛠️ Ejemplo de solución:

```sql
CREATE TABLE Producto (
  id INT AUTO_INCREMENT PRIMARY KEY,
  nombre VARCHAR(100) NOT NULL,
  descripcion TEXT,
  precio DECIMAL(10,2) NOT NULL,
  stock INT DEFAULT 0
);
```

![image-20250717115759801](C:/Users/juanc/AppData/Roaming/Typora/typora-user-images/image-20250717115759801.png)

## 🧱 Tipos de Datos Más Usados en MySQL

| Tipo       | Uso                                 |
| ---------- | ----------------------------------- |
| `INT`      | Números enteros                     |
| `DECIMAL`  | Números con decimales (ej. precios) |
| `VARCHAR`  | Cadenas de texto variables          |
| `CHAR`     | Texto fijo (códigos, siglas)        |
| `TEXT`     | Texto largo (descripciones)         |
| `DATE`     | Fechas sin hora                     |
| `DATETIME` | Fechas con hora                     |
| `BOOLEAN`  | 0 ó 1 (falso o verdadero)           |

## 🧩 🧠 Ejercicio Guiado: Crear Tabla `Empleado`

🎯 Requerimientos:

- id_Empleado (clave primaria)
- nombre completo
- cargo
- salario
- fecha_contratacion
- activo (valor booleano por defecto `1`)

```sql
CREATE TABLE Empleado (
  id INT AUTO_INCREMENT PRIMARY KEY,
  nombre_completo VARCHAR(150) NOT NULL,
  cargo VARCHAR(50),
  salario DECIMAL(10,2) NOT NULL,
  fecha_contratacion DATE,
  activo BOOLEAN DEFAULT 1
);
```

## 🔐 Buenas Prácticas en el Diseño de Tablas

💡 Tips útiles:

- Siempre define una `PRIMARY KEY`.
- Usa `NOT NULL` para columnas obligatorias.
- `AUTO_INCREMENT` es ideal para identificadores numéricos.
- Usa `VARCHAR` en vez de `TEXT` si sabes la longitud máxima.
- Define `DEFAULT` cuando tiene sentido lógico (fechas, booleanos, stock).

## 🧭 Evaluación de la Lección

✅ Responde:

1. ¿Cuál es la diferencia entre `VARCHAR(100)` y `TEXT`?
2. ¿Qué pasa si no usas `AUTO_INCREMENT` en una clave primaria?
3. ¿Puedes crear una tabla sin clave primaria?

## 🎲 Mini-Reto

🧪 Crea la tabla `Proveedor` con esta estructura:

- id (entero, autoincremental, clave primaria)
- nombre_empresa (obligatorio)
- ruc (único)
- telefono (opcional)
- fecha_registro (fecha actual por defecto)