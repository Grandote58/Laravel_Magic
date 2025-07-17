# 🔰 **Lección 1: Fundamentos del Modelo Relacional**

## 🎯 Objetivo de la Lección

Comprender cómo MySQL organiza y representa la información en una base de datos relacional a través de **entidades**, **atributos**, **claves primarias**, y **relaciones** entre tablas.

## 🎓 Meta de Aprendizaje

Al finalizar esta lección, el estudiante será capaz de:

- Explicar qué es una base de datos relacional.
- Identificar entidades y atributos en un sistema real.
- Comprender la función de las claves primarias.
- Diseñar un esquema conceptual básico de una base de datos.
- Entender cómo este modelo se traduce en estructuras MySQL.

## 🧠 Conceptos Clave

| Concepto           | Descripción                                                  |
| ------------------ | ------------------------------------------------------------ |
| **Entidad**        | Representa una cosa u objeto del mundo real. Ej: Cliente, Producto, Libro. |
| **Atributo**       | Característica que describe una entidad. Ej: nombre, precio, autor. |
| **Clave primaria** | Un identificador único por fila/registro.                    |
| **Relación**       | Conexión entre dos entidades. Ej: Un cliente hace muchos pedidos. |
| **Tabla**          | Representación de una entidad en MySQL.                      |

## 💡 Ejemplo Real: Sistema de Biblioteca

Imaginemos una biblioteca digital. Algunas entidades podrían ser:

| Entidad      | Atributos                                                  |
| ------------ | ---------------------------------------------------------- |
| **Libro**    | id, título, autor, año_publicación                         |
| **Usuario**  | id, nombre, correo, fecha_registro                         |
| **Préstamo** | id, id_usuario, id_libro, fecha_prestamo, fecha_devolucion |

## 📋 Esquema Conceptual Visual

![image-20250717113952797](C:/Users/juanc/AppData/Roaming/Typora/typora-user-images/image-20250717113952797.png)

Este modelo representa:

- Cada usuario puede hacer varios préstamos.
- Cada libro puede estar en varios préstamos.
- La tabla **Préstamo** actúa como relación *muchos a muchos* entre **Usuario** y **Libro**.

## 🧪 Práctica Aplicada (🌐 Entorno Real: Sistema Escolar)

🎯 **Ejercicio**: Identifica las entidades, atributos y claves primarias de un sistema escolar.

### 🗂️ Entidades posibles:

- **Estudiante**
- **Curso**
- **Inscripción**

### 🧾 Atributos sugeridos:

```tex
Estudiante: id_Estudiante, nombre, apellido, fecha_nacimiento
Curso: id_Curso, nombre, duración
Inscripción: id_Inscripcion, id_estudiante, id_curso, fecha_inscripcion
```

🧠 **¿Quién es la clave primaria?**

 Generalmente el campo `id` de cada entidad, para garantizar unicidad.

## 🛠️ Ejercicio Guiado en MySQL

Vamos a crear el **modelo lógico** de las entidades mencionadas arriba en MySQL:

```sql
CREATE TABLE Estudiante (
  id INT PRIMARY KEY,
  nombre VARCHAR(100),
  apellido VARCHAR(100),
  fecha_nacimiento DATE
);

CREATE TABLE Curso (
  id INT PRIMARY KEY,
  nombre VARCHAR(100),
  duracion INT -- en semanas
);

CREATE TABLE Inscripcion (
  id INT PRIMARY KEY,
  id_estudiante INT,
  id_curso INT,
  fecha_inscripcion DATE,
  FOREIGN KEY (id_estudiante) REFERENCES Estudiante(id),
  FOREIGN KEY (id_curso) REFERENCES Curso(id)
);
```

## 🎲 Mini-Reto 🔨🤖🔧

✍️ **Crea un modelo relacional para una tienda de videojuegos en línea**, que incluya:

- Videojuego
- Cliente
- Compra

📬 Indica:

- Atributos de cada entidad
- Clave primaria
- Relaciones

