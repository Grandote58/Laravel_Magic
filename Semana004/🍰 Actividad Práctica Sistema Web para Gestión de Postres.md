# 🍰 **Actividad Práctica: Sistema Web para Gestión de Postres**

## 🧭 **Nombre del Proyecto:**

**DeliPostres – Sistema Integral de Administración de Postres**

## 🎯 **Objetivos de Aprendizaje**

1. Crear un sistema Laravel que permita gestionar postres con funcionalidades CRUD.
2. Conectar y administrar una base de datos MySQL utilizando migraciones.
3. Diseñar una interfaz responsiva con Bootstrap 5 y sistema de navegación.

## 🧰 **Herramientas Requeridas**



| Herramienta        | Propósito               |
| ------------------ | ----------------------- |
| XAMPP              | Servidor Apache + MySQL |
| phpMyAdmin         | Interfaz para MySQL     |
| Composer           | Gestor de dependencias  |
| Laravel 12         | Framework PHP           |
| Visual Studio Code | Editor de código        |
| Bootstrap 5        | Diseño de interfaz      |

## 🏢 **Escenario Empresarial:**

**DeliPostres**, una pastelería artesanal, desea automatizar la gestión de su catálogo de postres. Necesitan un sistema que permita **registrar nuevos postres**, **listar los existentes**, **editar sus detalles** y **eliminarlos** fácilmente.

## 🛠️ **Desarrollo Paso a Paso**

### ✅ **Paso 1: Crear el Proyecto Laravel**

1. Abre la terminal:

```bash
cd C:\xampp\htdocs
composer create-project --prefer-dist laravel/laravel DeliPostres
```

### ✅ **Paso 2: Crear la Base de Datos**

1. Abre tu navegador en `http://localhost/phpmyadmin`
2. Crea una nueva base de datos:
    `delipostres_db`

### ✅ **Paso 3: Configurar Laravel con la Base de Datos**

Edita el archivo `.env`:

```bash
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=delipostres_db
DB_USERNAME=root
DB_PASSWORD=
```

------

### ✅ **Paso 4: Crear el Modelo y Migración `Postre`**

```bash
php artisan make:model Postre -m
```

Edita el archivo `xxxx_create_postres_table.php`:

```bash
Schema::create('postres', function (Blueprint $table) {
    $table->id();
    $table->string('nombre');
    $table->string('tipo');
    $table->decimal('precio', 8, 2);
    $table->text('descripcion')->nullable();
    $table->timestamps();
});
```

Ejecuta la migración:

```bash
php artisan migrate
```

### ✅ **Paso 5: Crear el Controlador**

```bash
php artisan make:controller PostreController --resource
```

Edita `app/Http/Controllers/PostreController.php`:

```php+HTML
use App\Models\Postre;
use Illuminate\Http\Request;

class PostreController extends Controller
{
    public function index() {
        $postres = Postre::all();
        return view('postres.index', compact('postres'));
    }

    public function create() {
        return view('postres.create');
    }

    public function store(Request $request) {
        $request->validate([
            'nombre' => 'required',
            'tipo' => 'required',
            'precio' => 'required|numeric|min:0',
            'descripcion' => 'nullable'
        ]);

        Postre::create($request->all());
        return redirect()->route('postres.index')->with('success', 'Postre creado con éxito.');
    }

    public function edit(Postre $postre) {
        return view('postres.edit', compact('postre'));
    }

    public function update(Request $request, Postre $postre) {
        $request->validate([
            'nombre' => 'required',
            'tipo' => 'required',
            'precio' => 'required|numeric|min:0',
            'descripcion' => 'nullable'
        ]);

        $postre->update($request->all());
        return redirect()->route('postres.index')->with('success', 'Postre actualizado con éxito.');
    }

    public function destroy(Postre $postre) {
        $postre->delete();
        return redirect()->route('postres.index')->with('success', 'Postre eliminado.');
    }
}
```

### ✅ **Paso 6: Definir las Rutas**

En `routes/web.php`:

```php
use App\Http\Controllers\PostreController;

Route::get('/', fn() => redirect()->route('postres.index'));
Route::resource('postres', PostreController::class);
```

### ✅ **Paso 7: Crear Vistas y Sistema de Navegación**

📁 Crea la carpeta `resources/views/postres/`
 📄 Crea una vista base `layout.blade.php`:

```html
<!DOCTYPE html>
<html>
<head>
    <title>DeliPostres</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
</head>
<body>
<nav class="navbar navbar-expand-lg navbar-dark bg-dark">
    <div class="container">
        <a class="navbar-brand" href="{{ route('postres.index') }}">DeliPostres</a>
        <ul class="navbar-nav">
            <li class="nav-item"><a class="nav-link" href="{{ route('postres.index') }}">Inicio</a></li>
            <li class="nav-item"><a class="nav-link" href="{{ route('postres.create') }}">Agregar Postre</a></li>
        </ul>
    </div>
</nav>
<div class="container py-4">@yield('content')</div>
</body>
</html>
```

📄 `index.blade.php`:

```php+HTML
@extends('postres.layout')

@section('content')
<h2>Catálogo de Postres</h2>

@if(session('success'))
    <div class="alert alert-success">{{ session('success') }}</div>
@endif

<table class="table table-bordered">
    <thead>
        <tr>
            <th>Nombre</th>
            <th>Tipo</th>
            <th>Precio</th>
            <th>Acciones</th>
        </tr>
    </thead>
    <tbody>
        @foreach($postres as $postre)
        <tr>
            <td>{{ $postre->nombre }}</td>
            <td>{{ $postre->tipo }}</td>
            <td>${{ $postre->precio }}</td>
            <td>
                <a href="{{ route('postres.edit', $postre) }}" class="btn btn-warning btn-sm">Editar</a>
                <form action="{{ route('postres.destroy', $postre) }}" method="POST" style="display:inline;">
                    @csrf @method('DELETE')
                    <button type="submit" class="btn btn-danger btn-sm" onclick="return confirm('¿Eliminar este postre?')">Eliminar</button>
                </form>
            </td>
        </tr>
        @endforeach
    </tbody>
</table>
@endsection
```

📄 `create.blade.php` y `edit.blade.php` usan un formulario compartido:

📁 `form.blade.php`:

```php+HTML
<div class="mb-3">
    <label>Nombre</label>
    <input type="text" name="nombre" value="{{ old('nombre', $postre->nombre ?? '') }}" class="form-control" required>
</div>
<div class="mb-3">
    <label>Tipo</label>
    <input type="text" name="tipo" value="{{ old('tipo', $postre->tipo ?? '') }}" class="form-control" required>
</div>
<div class="mb-3">
    <label>Precio</label>
    <input type="number" step="0.01" name="precio" value="{{ old('precio', $postre->precio ?? '') }}" class="form-control" required>
</div>
<div class="mb-3">
    <label>Descripción</label>
    <textarea name="descripcion" class="form-control">{{ old('descripcion', $postre->descripcion ?? '') }}</textarea>
</div>
```

📄 `create.blade.php`:

```php+HTML
@extends('postres.layout')

@section('content')
<h2>Agregar Nuevo Postre</h2>
@if ($errors->any())
<div class="alert alert-danger"><ul>@foreach ($errors->all() as $error)<li>{{ $error }}</li>@endforeach</ul></div>
@endif
<form method="POST" action="{{ route('postres.store') }}">@csrf
    @include('postres.form')
    <button type="submit" class="btn btn-primary">Guardar</button>
</form>
@endsection
```

📄 `edit.blade.php`:

```php+HTML
@extends('postres.layout')

@section('content')
<h2>Editar Postre</h2>
@if ($errors->any())
<div class="alert alert-danger"><ul>@foreach ($errors->all() as $error)<li>{{ $error }}</li>@endforeach</ul></div>
@endif
<form method="POST" action="{{ route('postres.update', $postre) }}">
    @csrf @method('PUT')
    @include('postres.form')
    <button type="submit" class="btn btn-success">Actualizar</button>
</form>
@endsection
```

### ✅ **Paso 8: Ejecutar el Proyecto**

1. En la terminal:

```bash
php artisan serve
```

1. Abre tu navegador en:
   - `http://127.0.0.1:8000` → Catálogo
   - `http://127.0.0.1:8000/postres/create` → Crear

## 📝 **Conclusión**

✅ Has creado un **sistema completo CRUD** con Laravel 12

✅ Conectaste el sistema a una base de datos real

✅ Usaste **Blade** y **Bootstrap** para una interfaz limpia y profesional

✅ Aplicaste seguridad básica (CSRF, validaciones)