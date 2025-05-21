# 📚 **“Construyendo mi Primera Tienda Online con Laravel 12”** 🚀🛒

### 🎯 Objetivos de Aprendizaje

1. Comprender e implementar un modelo entidad-relación en Laravel 12 mediante Eloquent ORM y CRUDs completos.
2. Desarrollar una tienda online funcional aplicando Bootstrap 5, arquitectura MVC y buenas prácticas de codificación.

### 🧩 Propósito de la Actividad

Esta actividad busca que los estudiantes construyan una **tienda online desde cero**, integrando conceptos fundamentales de Laravel 12, Eloquent, controladores RESTful, vistas con Blade, Bootstrap 5, y arquitectura MVC.

 👉 **Conocimiento adquirido:** Aprenderás a conectar el modelo de datos con la lógica de negocio y la interfaz visual, de forma funcional y pedagógica.

## 🧠 Conceptos Clave y su Aplicación Real

### 1. Eloquent ORM 📚

Es el sistema ORM (Mapeo Objeto-Relacional) de Laravel. Permite representar tablas como clases y sus relaciones como métodos.

 **Ejemplo Real:** La tabla `productos` se gestiona desde el modelo `Producto.php`. Puedes hacer `Producto::all()` para consultar todos los productos disponibles.

### 2. Controladores RESTful 🧩

Permiten separar la lógica de negocio por recursos.

**Ejemplo Real:** El controlador `ProductoController` maneja las operaciones como ver, crear, editar y eliminar productos (`index`, `create`, `store`, `edit`, `update`, `destroy`).

### 3. Vistas Blade 🖼️

El sistema de plantillas de Laravel. Blade te permite incrustar lógica PHP en HTML fácilmente.
 **Ejemplo Real:** Mostrar una lista de productos con `@foreach ($productos as $producto)`.

### 4. Bootstrap 5 🎨

Herramienta de diseño CSS. Se integra por CDN en `layouts/app.blade.php` para mejorar la apariencia de la tienda.

**Ejemplo Real:** Crear tarjetas de productos (`card`) y un menú de navegación (`navbar`).