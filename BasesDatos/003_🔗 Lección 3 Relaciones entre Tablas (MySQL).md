# 🔗 **Lección 3: Relaciones entre Tablas (MySQL)**

## 🎯 Objetivo de la Lección

Entender cómo conectar tablas en una base de datos relacional a través de claves foráneas, y aplicar estos conceptos para modelar relaciones **uno a uno**, **uno a muchos**, y **muchos a muchos** en MySQL.

## 🎓 Meta de Aprendizaje

Al completar esta lección, podrás:

- Comprender qué es una **clave foránea (FOREIGN KEY)**.
- Modelar relaciones entre tablas en un modelo real.
- Implementar relaciones 1:1, 1:N y N:N.
- Aplicar restricciones referenciales para asegurar la integridad de los datos.

## 🧠 Conceptos Clave

| Concepto                   | Descripción                                                  |
| -------------------------- | ------------------------------------------------------------ |
| `FOREIGN KEY`              | Columna que hace referencia a la clave primaria de otra tabla |
| **Integridad referencial** | Garantiza que los datos relacionados existan y estén consistentes |
| `ON DELETE` / `ON UPDATE`  | Reglas para el comportamiento de claves foráneas ante cambios en los datos relacionados |

## 🧠 Tipos de Relaciones y Cómo Modelarlas

### 1️⃣ Relación Uno a Uno (1:1)

- Ejemplo: Un empleado tiene un solo contrato.

```sql
CREATE TABLE Contrato (
  id INT PRIMARY KEY,
  empleado_id INT UNIQUE,
  salario DECIMAL(10,2),
  FOREIGN KEY (empleado_id) REFERENCES Empleado(id)
);
```

### 2️⃣ Relación Uno a Muchos (1:N)

- Ejemplo: Un cliente puede hacer muchos pedidos.

```sql
CREATE TABLE Pedido (
  id INT PRIMARY KEY,
  cliente_id INT,
  fecha DATE,
  FOREIGN KEY (cliente_id) REFERENCES Cliente(id)
);
```

### 3️⃣ Relación Muchos a Muchos (N:N)

- Ejemplo: Un estudiante puede estar en varios cursos, y un curso puede tener muchos estudiantes.

  ✅ Solución: Tabla intermedia `Inscripcion`

```sql
CREATE TABLE Inscripcion (
  id INT PRIMARY KEY,
  estudiante_id INT,
  curso_id INT,
  fecha_inscripcion DATE,
  FOREIGN KEY (estudiante_id) REFERENCES Estudiante(id),
  FOREIGN KEY (curso_id) REFERENCES Curso(id)
);
```

## 💡 Ejemplo Real: Sistema de Tienda

Entidades:

- `Cliente`
- `Pedido`
- `Producto`
- `DetallePedido` (relación entre `Pedido` y `Producto`)

```sql
CREATE TABLE Cliente (
  id INT PRIMARY KEY AUTO_INCREMENT,
  nombre VARCHAR(100) NOT NULL
);

CREATE TABLE Pedido (
  id INT PRIMARY KEY AUTO_INCREMENT,
  cliente_id INT,
  fecha DATE,
  FOREIGN KEY (cliente_id) REFERENCES Cliente(id)
);

CREATE TABLE Producto (
  id INT PRIMARY KEY AUTO_INCREMENT,
  nombre VARCHAR(100),
  precio DECIMAL(10,2)
);

CREATE TABLE DetallePedido (
  id INT PRIMARY KEY AUTO_INCREMENT,
  pedido_id INT,
  producto_id INT,
  cantidad INT,
  FOREIGN KEY (pedido_id) REFERENCES Pedido(id),
  FOREIGN KEY (producto_id) REFERENCES Producto(id)
);
```

## 🧪 Práctica Aplicada

🎯 Crea un modelo que relacione `Autor` y `Libro`:

- Un autor puede tener muchos libros.
- Cada libro tiene un solo autor.

### 🛠️ Solución:

```sql
CREATE TABLE Autor (
  id INT PRIMARY KEY AUTO_INCREMENT,
  nombre VARCHAR(100)
);

CREATE TABLE Libro (
  id INT PRIMARY KEY AUTO_INCREMENT,
  titulo VARCHAR(150),
  autor_id INT,
  FOREIGN KEY (autor_id) REFERENCES Autor(id)
);
```

## ⚙️ Comportamiento de Claves Foráneas (`ON DELETE`, `ON UPDATE`)

🎯 **Reglas opcionales de integridad**:

```sql
FOREIGN KEY (cliente_id) REFERENCES Cliente(id)
  ON DELETE CASCADE
  ON UPDATE CASCADE;
```

| Regla       | Descripción                                       |
| ----------- | ------------------------------------------------- |
| `CASCADE`   | Borra o actualiza en cascada                      |
| `SET NULL`  | Asigna `NULL` cuando se borra el dato relacionado |
| `RESTRICT`  | Impide borrar si hay dependencias                 |
| `NO ACTION` | Igual a `RESTRICT`                                |

## 🧩 🧠 Ejercicio Guiado: Relación Curso ↔ Profesor

🎯 Requisitos:

- Un profesor puede dictar varios cursos.
- Cada curso tiene un solo profesor.

```sql
CREATE TABLE Profesor (
  id INT PRIMARY KEY AUTO_INCREMENT,
  nombre VARCHAR(100)
);

CREATE TABLE Curso (
  id INT PRIMARY KEY AUTO_INCREMENT,
  nombre VARCHAR(100),
  profesor_id INT,
  FOREIGN KEY (profesor_id) REFERENCES Profesor(id)
);
```

## ✅ Evaluación de la Lección

✍️ **Preguntas clave**:

1. ¿Qué diferencia hay entre una clave primaria y una foránea?
2. ¿Cuál sería el problema de no usar claves foráneas en bases con múltiples tablas?
3. ¿Qué tipo de relación usarías para una biblioteca que presta libros a usuarios?

## 🎲 Mini-Reto

🧪 Crea las tablas necesarias para modelar un sistema donde:

- Un usuario puede reservar muchas habitaciones en un hotel.
- Cada habitación puede ser reservada por muchos usuarios (en diferentes fechas).
- La tabla intermedia debe tener: `fecha_inicio`, `fecha_fin`.

