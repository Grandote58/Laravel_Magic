# **🧪 Práctica Guiada: Crear una Agenda de Contactos en MySQL**

> **Propósito de la actividad:** Aplicar lo aprendido sobre entidades, atributos, relaciones y creación de tablas en MySQL mediante un proyecto funcional: una base de datos para gestionar contactos personales y profesionales.

## 🧩 Situación de Aprendizaje

Imagina que estás desarrollando una aplicación web para una agenda de contactos. Cada usuario puede registrar múltiples contactos, y cada contacto puede tener uno o varios números telefónicos (personal, oficina, celular), además de su dirección de correo electrónico.

## 🎯 Objetivos de Aprendizaje

- Diseñar un diagrama MER sencillo para una agenda de contactos.
- Transformar el modelo MER en tablas relacionales.
- Crear e insertar datos en MySQL.
- Consultar datos usando relaciones simples (JOIN).

## 🔍 Paso 1: Definir Entidades y Relaciones

### ✍️ Entidades

1. **Usuario**: quien gestiona su agenda.
2. **Contacto**: una persona registrada por el usuario.
3. **Telefono**: uno o varios teléfonos por contacto.

### 📄 Atributos sugeridos

| Entidad  | Atributos                                                    |
| -------- | ------------------------------------------------------------ |
| Usuario  | id_usuario (PK), nombre                                      |
| Contacto | id_contacto (PK), nombre, correo, id_usuario (FK)            |
| Telefono | id_telefono (PK), numero, tipo (celular, oficina, etc), id_contacto (FK) |

### 🔗 Relaciones

- **Un usuario** puede tener **muchos contactos** → (1:N).
- **Un contacto** puede tener **muchos teléfonos** → (1:N).

## 🧭 Paso 2: Diagrama MER (representación textual)

```tex
Usuario( id_usuario PK, nombre )
    |
    |---< Contacto( id_contacto PK, nombre, correo, id_usuario FK )
              |
              |---< Telefono( id_telefono PK, numero, tipo, id_contacto FK )
```

## 🏗️ Paso 3: Crear la Base de Datos y Tablas en MySQL

### ✅ Crear Base de Datos

```mysql
CREATE DATABASE agenda_contactos;
USE agenda_contactos;
```

### 👤 Crear tabla Usuario

```mysql
CREATE TABLE Usuario (
    id_usuario INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(100) NOT NULL
);
```

### 📇 Crear tabla Contacto

```mysql
CREATE TABLE Contacto (
    id_contacto INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(100) NOT NULL,
    correo VARCHAR(100),
    id_usuario INT,
    FOREIGN KEY (id_usuario) REFERENCES Usuario(id_usuario)
);
```

### ☎️ Crear tabla Telefono

```mysql
CREATE TABLE Telefono (
    id_telefono INT PRIMARY KEY AUTO_INCREMENT,
    numero VARCHAR(20) NOT NULL,
    tipo VARCHAR(20),
    id_contacto INT,
    FOREIGN KEY (id_contacto) REFERENCES Contacto(id_contacto)
);
```

## 🧾 Paso 4: Insertar Datos

### 👥 Insertar Usuarios

```mysql
INSERT INTO Usuario(nombre) VALUES ('Ana Torres'), ('Carlos Jiménez');
```

### 📬 Insertar Contactos

```mysql
INSERT INTO Contacto(nombre, correo, id_usuario)
VALUES 
    ('Lucía Méndez', 'lucia@mail.com', 1),
    ('Pedro Suárez', 'pedro@mail.com', 1),
    ('María López', 'maria@mail.com', 2);
```

### 📱 Insertar Teléfonos

```mysql
INSERT INTO Telefono(numero, tipo, id_contacto)
VALUES 
    ('555-1111', 'casa', 1),
    ('555-2222', 'oficina', 1),
    ('555-3333', 'celular', 2),
    ('555-4444', 'celular', 3);
```

## 🔍 Paso 5: Consultas Básicas

### 🧾 Mostrar todos los contactos de un usuario (por nombre)

```mysql
SELECT Contacto.nombre AS Contacto, Contacto.correo
FROM Contacto
JOIN Usuario ON Contacto.id_usuario = Usuario.id_usuario
WHERE Usuario.nombre = 'Ana Torres';
```

### ☎️ Mostrar todos los teléfonos de un contacto

```mysql
SELECT Contacto.nombre AS Contacto, Telefono.numero, Telefono.tipo
FROM Telefono
JOIN Contacto ON Telefono.id_contacto = Contacto.id_contacto
WHERE Contacto.nombre = 'Lucía Méndez';
```

## 🎓 Actividad Final: Evalúate

> Responde estas preguntas en tu cuaderno o portafolio digital:

1. ¿Qué representa una clave foránea y por qué es importante?
2. ¿Cómo te aseguras de que un contacto no exista sin estar ligado a un usuario?
3. ¿Qué pasaría si no defines correctamente las relaciones entre las tablas?

