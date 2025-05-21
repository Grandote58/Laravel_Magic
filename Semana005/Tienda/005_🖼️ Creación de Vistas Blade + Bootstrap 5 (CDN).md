# **🖼️ Creación de Vistas Blade + Bootstrap 5 (CDN)**

## 🎯 Objetivo

> Diseñar e implementar todas las vistas necesarias para el CRUD completo del módulo **Productos**, usando el motor Blade de Laravel y el framework CSS Bootstrap 5 para mejorar la experiencia visual del usuario.

## 🎨 1. Integración de Bootstrap 5 vía CDN

### 🗂️ Archivo: `resources/views/layouts/app.blade.php`

Creamos una plantilla base que será reutilizada en todas las vistas.

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>@yield('title', 'Tienda Online')</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
</head>
<body>

<nav class="navbar navbar-expand-lg navbar-dark bg-dark mb-4">
    <div class="container-fluid">
        <a class="navbar-brand" href="/">🛒 Tienda Online</a>
        <div class="collapse navbar-collapse">
            <ul class="navbar-nav ms-auto">
                <li class="nav-item"><a class="nav-link" href="{{ route('productos.index') }}">Productos</a></li>
                <li class="nav-item"><a class="nav-link" href="{{ route('categorias.index') }}">Categorías</a></li>
            </ul>
        </div>
    </div>
</nav>

<div class="container">
    @yield('content')
</div>

<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>
</body>
</html>
```

## 🧩 2. Vista: `index.blade.php` (Lista de productos)

📁 `resources/views/productos/index.blade.php`

```php+HTML
@extends('layouts.app')

@section('title', 'Listado de Productos')

@section('content')
<div class="d-flex justify-content-between mb-3">
    <h2>🛍️ Productos Disponibles</h2>
    <a href="{{ route('productos.create') }}" class="btn btn-primary">➕ Nuevo Producto</a>
</div>

@if (session('success'))
    <div class="alert alert-success">{{ session('success') }}</div>
@endif

<table class="table table-striped table-bordered">
    <thead class="table-dark">
        <tr>
            <th>#</th>
            <th>Nombre</th>
            <th>Categoría</th>
            <th>Precio</th>
            <th>Stock</th>
            <th>Acciones</th>
        </tr>
    </thead>
    <tbody>
        @foreach($productos as $producto)
            <tr>
                <td>{{ $producto->id }}</td>
                <td>{{ $producto->nombre }}</td>
                <td>{{ $producto->categoria->nombre }}</td>
                <td>${{ $producto->precio }}</td>
                <td>{{ $producto->stock }}</td>
                <td>
                    <a href="{{ route('productos.show', $producto) }}" class="btn btn-sm btn-info">👁️ Ver</a>
                    <a href="{{ route('productos.edit', $producto) }}" class="btn btn-sm btn-warning">✏️ Editar</a>
                    <form action="{{ route('productos.destroy', $producto) }}" method="POST" style="display:inline-block;">
                        @csrf
                        @method('DELETE')
                        <button class="btn btn-sm btn-danger" onclick="return confirm('¿Seguro que deseas eliminar este producto?')">🗑️ Eliminar</button>
                    </form>
                </td>
            </tr>
        @endforeach
    </tbody>
</table>

{{ $productos->links() }}
@endsection
```

## 📝 3. Vista: `create.blade.php` (Formulario de creación)

📁 `resources/views/productos/create.blade.php`

```php+HTML
@extends('layouts.app')

@section('title', 'Crear Producto')

@section('content')
<h2>➕ Crear Nuevo Producto</h2>

@if ($errors->any())
    <div class="alert alert-danger">
        <ul>
            @foreach ($errors->all() as $error) <li>{{ $error }}</li> @endforeach
        </ul>
    </div>
@endif

<form method="POST" action="{{ route('productos.store') }}">
    @csrf
    <div class="mb-3">
        <label>Nombre:</label>
        <input type="text" name="nombre" class="form-control" value="{{ old('nombre') }}">
    </div>
    <div class="mb-3">
        <label>Descripción:</label>
        <textarea name="descripcion" class="form-control">{{ old('descripcion') }}</textarea>
    </div>
    <div class="mb-3">
        <label>Precio:</label>
        <input type="number" name="precio" class="form-control" step="0.01" value="{{ old('precio') }}">
    </div>
    <div class="mb-3">
        <label>Stock:</label>
        <input type="number" name="stock" class="form-control" value="{{ old('stock') }}">
    </div>
    <div class="mb-3">
        <label>Categoría:</label>
        <select name="categoria_id" class="form-select">
            @foreach($categorias as $categoria)
                <option value="{{ $categoria->id }}">{{ $categoria->nombre }}</option>
            @endforeach
        </select>
    </div>
    <button type="submit" class="btn btn-success">💾 Guardar</button>
    <a href="{{ route('productos.index') }}" class="btn btn-secondary">🔙 Volver</a>
</form>
@endsection
```

## ✏️ 4. Vista: `edit.blade.php` (Editar producto)

📁 `resources/views/productos/edit.blade.php`

```php+HTML
@extends('layouts.app')

@section('title', 'Editar Producto')

@section('content')
<h2>✏️ Editar Producto</h2>

<form method="POST" action="{{ route('productos.update', $producto) }}">
    @csrf
    @method('PUT')

    <div class="mb-3">
        <label>Nombre:</label>
        <input type="text" name="nombre" class="form-control" value="{{ $producto->nombre }}">
    </div>
    <div class="mb-3">
        <label>Descripción:</label>
        <textarea name="descripcion" class="form-control">{{ $producto->descripcion }}</textarea>
    </div>
    <div class="mb-3">
        <label>Precio:</label>
        <input type="number" name="precio" class="form-control" step="0.01" value="{{ $producto->precio }}">
    </div>
    <div class="mb-3">
        <label>Stock:</label>
        <input type="number" name="stock" class="form-control" value="{{ $producto->stock }}">
    </div>
    <div class="mb-3">
        <label>Categoría:</label>
        <select name="categoria_id" class="form-select">
            @foreach($categorias as $categoria)
                <option value="{{ $categoria->id }}" @selected($producto->categoria_id == $categoria->id)>
                    {{ $categoria->nombre }}
                </option>
            @endforeach
        </select>
    </div>
    <button type="submit" class="btn btn-primary">💾 Actualizar</button>
    <a href="{{ route('productos.index') }}" class="btn btn-secondary">🔙 Volver</a>
</form>
@endsection
```

## 👁️ 5. Vista: `show.blade.php` (Ver detalle de producto)

📁 `resources/views/productos/show.blade.php`

```php+HTML
@extends('layouts.app')

@section('title', 'Detalle del Producto')

@section('content')
<h2>👁️ Detalle del Producto</h2>

<div class="card mb-3">
    <div class="card-header">
        {{ $producto->nombre }}
    </div>
    <div class="card-body">
        <p><strong>Descripción:</strong> {{ $producto->descripcion }}</p>
        <p><strong>Precio:</strong> ${{ $producto->precio }}</p>
        <p><strong>Stock:</strong> {{ $producto->stock }}</p>
        <p><strong>Categoría:</strong> {{ $producto->categoria->nombre }}</p>
    </div>
</div>

<a href="{{ route('productos.index') }}" class="btn btn-secondary">🔙 Volver al listado</a>
@endsection
```

## 🧩 3. Vista: `index.blade.php` (Lista de categorías)

📁 `resources/views/categorias/index.blade.php`

```css
@extends('layouts.app')

@section('title', 'Categorías')

@section('content')
<div class="d-flex justify-content-between mb-3">
    <h2>📁 Listado de Categorías</h2>
    <a href="{{ route('categorias.create') }}" class="btn btn-success">➕ Nueva Categoría</a>
</div>

@if (session('success'))
    <div class="alert alert-success">{{ session('success') }}</div>
@endif

<table class="table table-bordered">
    <thead class="table-dark">
        <tr>
            <th>#</th>
            <th>Nombre</th>
            <th>Descripción</th>
            <th>Acciones</th>
        </tr>
    </thead>
    <tbody>
        @foreach($categorias as $categoria)
        <tr>
            <td>{{ $categoria->id }}</td>
            <td>{{ $categoria->nombre }}</td>
            <td>{{ $categoria->descripcion }}</td>
            <td>
                <a href="{{ route('categorias.show', $categoria) }}" class="btn btn-info btn-sm">👁️ Ver</a>
                <a href="{{ route('categorias.edit', $categoria) }}" class="btn btn-warning btn-sm">✏️ Editar</a>
                <form action="{{ route('categorias.destroy', $categoria) }}" method="POST" style="display:inline-block">
                    @csrf
                    @method('DELETE')
                    <button class="btn btn-danger btn-sm" onclick="return confirm('¿Eliminar esta categoría?')">🗑️ Eliminar</button>
                </form>
            </td>
        </tr>
        @endforeach
    </tbody>
</table>

{{ $categorias->links() }}
@endsection

```

📁 `resources/views/categorias/create.blade.php`

```css
@extends('layouts.app')

@section('title', 'Crear Categoría')

@section('content')
<h2>📁 Crear Nueva Categoría</h2>

@if ($errors->any())
<div class="alert alert-danger">
    <ul>@foreach ($errors->all() as $error)<li>{{ $error }}</li>@endforeach</ul>
</div>
@endif

<form action="{{ route('categorias.store') }}" method="POST">
    @csrf
    <div class="mb-3">
        <label>Nombre:</label>
        <input type="text" name="nombre" class="form-control" value="{{ old('nombre') }}">
    </div>
    <div class="mb-3">
        <label>Descripción:</label>
        <textarea name="descripcion" class="form-control">{{ old('descripcion') }}</textarea>
    </div>
    <button type="submit" class="btn btn-success">💾 Guardar</button>
    <a href="{{ route('categorias.index') }}" class="btn btn-secondary">🔙 Volver</a>
</form>
@endsection
```

📁 `resources/views/categorias/edit.blade.php`

```css
@extends('layouts.app')

@section('title', 'Crear Categoría')

@section('content')
<h2>📁 Crear Nueva Categoría</h2>

@if ($errors->any())
<div class="alert alert-danger">
    <ul>@foreach ($errors->all() as $error)<li>{{ $error }}</li>@endforeach</ul>
</div>
@endif

<form action="{{ route('categorias.update', $categoria) }}" method="POST">
    @csrf
    @method('PUT')
    <input type="text" name="nombre" value="{{ $categoria->nombre }}" class="form-control">
    <textarea name="descripcion" class="form-control">{{ $categoria->descripcion }}</textarea>
    <button type="submit" class="btn btn-success">💾 Guardar</button>
    <a href="{{ route('categorias.index') }}" class="btn btn-secondary">🔙 Volver</a>
</form>
@endsection
```

📁 `resources/views/categorias/show.blade.php`

```css
@extends('layouts.app')

@section('title', 'Detalle de Categoría')

@section('content')
<h2>👁️ Detalle de Categoría</h2>

<div class="card">
    <div class="card-body">
        <h5 class="card-title">{{ $categoria->nombre }}</h5>
        <p class="card-text">{{ $categoria->descripcion }}</p>
    </div>
</div>

<a href="{{ route('categorias.index') }}" class="btn btn-secondary mt-3">🔙 Volver</a>
@endsection
```

📁 `resources/views/home.blade.php`

```css
@extends('layouts.app')

@section('title', 'Inicio')

@section('content')
<div class="text-center mt-5">
    <h1 class="display-4">🛍️ Bienvenido a la Tienda Online</h1>
    <p class="lead">Explora nuestros productos y encuentra lo que necesitas.</p>
    <a href="{{ route('productos.index') }}" class="btn btn-primary btn-lg mt-3">Ver Productos</a>
</div>
@endsection
```

## ✅ Buenas prácticas

- ✅ Reutiliza `layouts.app` para consistencia visual
- ✅ Usa `@csrf` en todos los formularios
- ✅ Usa clases Bootstrap como `form-control`, `btn`, `alert` y `card`
- ✅ Agrega validaciones y mensajes de sesión con `session('success')` y `@error`

## 🧠 Conclusión

Crear las vistas Blade con Bootstrap 5 permite construir interfaces limpias, modernas y responsivas, alineadas con las mejores prácticas de diseño y usabilidad. Además, promueve el aprendizaje visual y significativo del estudiante, al ver sus datos reflejados de forma clara en el navegador.