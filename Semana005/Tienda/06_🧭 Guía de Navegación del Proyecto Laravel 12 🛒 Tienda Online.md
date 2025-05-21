# **🧭 Guía de Navegación del Proyecto Laravel 12: 🛒 *Tienda Online***

### Enfoque: Aprendizaje práctico con Bootstrap 5 y arquitectura MVC

## 🎯 Objetivo de la Guía

> Permitir al usuario abrir correctamente el sistema Laravel 12 en un entorno local y desplazarse por los módulos clave de la tienda online (Productos, Categorías, Carrito y más) usando la interfaz visual desarrollada con Blade + Bootstrap.

## 🏠 2. Configurar la Vista de Inicio Personalizada

Por defecto, Laravel muestra `resources/views/welcome.blade.php`. Cambiémoslo por una **vista más significativa**.

### 🔧 Paso 1: Crear la vista `home.blade.php`

📁 `resources/views/home.blade.php`

```css
@extends('layouts.app')

@section('title', 'Inicio')

@section('content')
<div class="text-center">
    <h1 class="display-4">🛍️ Bienvenido a la Tienda Online</h1>
    <p class="lead">Explora nuestros productos y encuentra lo que necesitas.</p>
    <a href="{{ route('productos.index') }}" class="btn btn-primary btn-lg mt-4">Ver Productos</a>
</div>
@endsection
```

### 🔧 Paso 2: Modificar la ruta raíz `/` en `routes/web.php`

```css
use Illuminate\Support\Facades\Route;

Route::get('/', function () {
    return view('home');
});
```

✅ Ahora al entrar a `http://localhost:8000`, verás la vista de bienvenida personalizada de la tienda.

## 🚀 1. Compilar y Ejecutar la Aplicación con `php artisan serve`

Antes de navegar en el navegador, asegúrate de que Laravel esté corriendo:

### 🛠️ En la terminal de Visual Studio Code:

```css
cd C:\xampp\htdocs\tiendaOnline
php artisan serve
```

✅ Esto levantará el servidor de desarrollo en:

```css
http://127.0.0.1:8000
```

> Si ves la pantalla de bienvenida de Laravel, ¡todo está funcionando correctamente! 👏🧭 PASO A PASO: Apertura y Navegación

### ✅ 1. **Encender los servicios de Apache y MySQL**

Abre el panel de control de **XAMPP** y haz clic en:

- 🔵 **Start Apache**
- 🔵 **Start MySQL**

Verifica que estén en color **verde**.

### ✅ 2. **Abrir la aplicación en el navegador**

Abre tu navegador y accede a:

```css
http://localhost:8080
```

> 🎯 Esto cargará la vista de inicio (welcome o la que definas como home personalizada).

### ✅ 3. **Navegar desde el menú superior**

El menú principal está definido en `views/layouts/app.blade.php`, y se muestra como una **barra de navegación Bootstrap** en la parte superior del sitio.

#### 🔹 Opción: Productos

- **Ruta:** `/productos`
- **Acción:** Muestra la lista de productos disponibles.
- **Interacción:**
  - Botón `➕ Nuevo Producto`: crea uno nuevo
  - Botón `✏️ Editar`: modifica un producto existente
  - Botón `🗑️ Eliminar`: borra un producto
  - Botón `👁️ Ver`: muestra los detalles del producto

#### 🔹 Opción: Categorías

- **Ruta:** `/categorias`
- **Acción:** Administra las categorías de productos.

### ✅ 4. **Crear un nuevo producto**

1. Ir a la opción del menú: **Productos**
2. Clic en `➕ Nuevo Producto`
3. Llenar los campos del formulario (nombre, descripción, precio, stock, categoría)
4. Clic en `💾 Guardar`

> ✔️ Al guardar correctamente, serás redirigido a la lista con un mensaje de confirmación.

### ✅ 5. **Editar o eliminar productos**

Desde la vista `index` de productos (`/productos`):

- Haz clic en `✏️ Editar` para modificar un registro
- Haz clic en `🗑️ Eliminar` para removerlo (confirmación incluida)

### ✅ 6. **Ver detalle de un producto**

Desde `/productos` haz clic en `👁️ Ver`
 Esto te llevará a `/productos/{id}`, donde podrás consultar la información detallada del producto seleccionado.

### ✅ 7. **Volver a la lista principal**

Desde cualquier vista, utiliza el botón `🔙 Volver` o navega usando el menú superior.

## 📌 Estructura de URLs (Resumen)

| Módulo          | URL                    | Función                      |
| --------------- | ---------------------- | ---------------------------- |
| Inicio          | `/`                    | Página de bienvenida         |
| Productos       | `/productos`           | Listado de productos         |
| Crear Producto  | `/productos/create`    | Formulario de creación       |
| Ver Producto    | `/productos/{id}`      | Detalles del producto        |
| Editar Producto | `/productos/{id}/edit` | Formulario de edición        |
| Categorías      | `/categorias`          | Administración de categorías |

## 🎓 Conclusión

Esta guía de navegación permite a los estudiantes y usuarios abrir el proyecto en su entorno local, comprender su arquitectura visual y funcional, y desplazarse de forma fluida por los distintos componentes de la aplicación. Es una herramienta esencial para el aprendizaje significativo y el desarrollo profesional con Laravel 12.

