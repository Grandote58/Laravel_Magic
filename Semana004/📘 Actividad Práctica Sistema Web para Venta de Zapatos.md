# 📘 **Actividad Práctica: Sistema Web para Venta de Zapatos**

## 🧭 **Nombre del Proyecto:**

**ZapaStore – Gestión y Venta de Calzado Online**

## 🎯 **Objetivos de Aprendizaje**

1. Desarrollar un sistema básico de gestión de productos (zapatos) en Laravel 12.
2. Implementar un sistema de navegación simple usando Blade y Bootstrap 5.
3. Crear vistas para registrar, listar y visualizar productos.

## 🧰 **Herramientas Requeridas**



| Herramienta        | Propósito                             |
| ------------------ | ------------------------------------- |
| XAMPP              | Servidor Apache y base de datos MySQL |
| phpMyAdmin         | Gestión visual de base de datos       |
| Composer           | Gestor de dependencias PHP            |
| Laravel 12         | Framework backend                     |
| Visual Studio Code | Editor de código                      |
| Bootstrap 5        | Estilos visuales para la interfaz     |

## 🏢 **Escenario Empresarial:**

La tienda **ZapaStore**, dedicada a la venta de calzado, necesita un sistema web que permita registrar nuevos modelos de zapatos, visualizarlos en un catálogo interno y administrar la información de manera básica. También desean tener un **menú de navegación superior** para mejorar la experiencia del usuario.

## 🛠️ **Desarrollo Paso a Paso**

### ✅ **Paso 1: Crear el Proyecto Laravel**

1. Abre la terminal (CMD o PowerShell).
2. Dirígete a la carpeta de proyectos de XAMPP:

```bash
cd C:\xampp\htdocs
```

1. Crea el proyecto:

```bash
composer create-project --prefer-dist laravel/laravel ZapaStore
```

### ✅ **Paso 2: Crear la Base de Datos**

1. Abre tu navegador y ve a `http://localhost/phpmyadmin`

2. Crea una base de datos llamada:

    `zapastore_db`

### ✅ **Paso 3: Configurar Laravel con MySQL**

1. Abre el proyecto en **Visual Studio Code**.
2. Modifica el archivo `.env`:

```bash
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=zapastore_db
DB_USERNAME=root
DB_PASSWORD=
```

### ✅ **Paso 4: Crear el Modelo y Migración `Zapato`**

```bash
php artisan make:model Zapato -m
```

Edita el archivo de migración `xxxx_create_zapatos_table.php`:

```mysql
Schema::create('zapatos', function (Blueprint $table) {
    $table->id();
    $table->string('modelo');
    $table->string('marca');
    $table->decimal('precio', 8, 2);
    $table->integer('stock');
    $table->timestamps();
});
```

Ejecuta la migración:

```bash
php artisan migrate
```

### ✅ **Paso 5: Crear el Controlador**

```bash
php artisan make:controller ZapatoController
```

Edita `app/Http/Controllers/ZapatoController.php`:

```php+HTML
use App\Models\Zapato;
use Illuminate\Http\Request;

class ZapatoController extends Controller
{
    public function index() {
        $zapatos = Zapato::all();
        return view('zapatos.index', compact('zapatos'));
    }

    public function create() {
        return view('zapatos.create');
    }

    public function store(Request $request) {
        $request->validate([
            'modelo' => 'required',
            'marca' => 'required',
            'precio' => 'required|numeric|min:0',
            'stock' => 'required|integer|min:0'
        ]);

        Zapato::create($request->all());

        return redirect('/zapatos')->with('success', 'Zapato agregado exitosamente.');
    }
}
```

### ✅ **Paso 6: Configurar las Rutas Web**

En `routes/web.php`:

```php+HTML
use App\Http\Controllers\ZapatoController;

Route::get('/', fn() => redirect('/zapatos'));

Route::get('/zapatos', [ZapatoController::class, 'index']);
Route::get('/zapatos/create', [ZapatoController::class, 'create']);
Route::post('/zapatos', [ZapatoController::class, 'store']);
```

### ✅ **Paso 7: Crear Vistas y Sistema de Navegación**

📁 Crea la carpeta `resources/views/zapatos/`

🔹 `layout.blade.php` (base para reutilizar):

```php+HTML
<!DOCTYPE html>
<html>
<head>
    <title>ZapaStore</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
</head>
<body>
<nav class="navbar navbar-expand-lg navbar-dark bg-dark">
    <div class="container">
        <a class="navbar-brand" href="/zapatos">ZapaStore</a>
        <div class="collapse navbar-collapse">
            <ul class="navbar-nav">
                <li class="nav-item"><a class="nav-link" href="/zapatos">Inicio</a></li>
                <li class="nav-item"><a class="nav-link" href="/zapatos/create">Agregar Zapato</a></li>
            </ul>
        </div>
    </div>
</nav>

<div class="container py-4">
    @yield('content')
</div>
</body>
</html>
```

🔹 `index.blade.php`:

```php+HTML
@extends('zapatos.layout')

@section('content')
<h2>Catálogo de Zapatos</h2>

@if(session('success'))
    <div class="alert alert-success">{{ session('success') }}</div>
@endif

<table class="table table-bordered">
    <thead>
        <tr>
            <th>Modelo</th>
            <th>Marca</th>
            <th>Precio</th>
            <th>Stock</th>
        </tr>
    </thead>
    <tbody>
        @foreach($zapatos as $zapato)
            <tr>
                <td>{{ $zapato->modelo }}</td>
                <td>{{ $zapato->marca }}</td>
                <td>${{ number_format($zapato->precio, 2) }}</td>
                <td>{{ $zapato->stock }}</td>
            </tr>
        @endforeach
    </tbody>
</table>
@endsection
```

🔹 `create.blade.php`:

```php
@extends('zapatos.layout')

@section('content')
<h2>Agregar Nuevo Zapato</h2>

@if ($errors->any())
    <div class="alert alert-danger">
        <ul>@foreach ($errors->all() as $error) <li>{{ $error }}</li> @endforeach</ul>
    </div>
@endif

<form method="POST" action="/zapatos">
    @csrf
    <div class="mb-3">
        <label>Modelo</label>
        <input type="text" name="modelo" class="form-control" required>
    </div>
    <div class="mb-3">
        <label>Marca</label>
        <input type="text" name="marca" class="form-control" required>
    </div>
    <div class="mb-3">
        <label>Precio</label>
        <input type="number" name="precio" step="0.01" class="form-control" required>
    </div>
    <div class="mb-3">
        <label>Stock</label>
        <input type="number" name="stock" class="form-control" required>
    </div>
    <button type="submit" class="btn btn-primary">Guardar</button>
</form>
@endsection
```

------

### ✅ **Paso 8: Ejecutar el Proyecto**

1. En la terminal, ejecuta:

```bash
php artisan serve
```

1. Visita en tu navegador:

    🔹 `http://127.0.0.1:8000/zapatos` para ver el catálogo

    🔹 `http://127.0.0.1:8000/zapatos/create` para agregar nuevos zapatos

## 📝 **Conclusión**

✅ Se construyó un sistema funcional de venta de zapatos.

✅ Se utilizó Bootstrap para una interfaz moderna y responsiva.

✅ Se implementó un **menú de navegación reutilizable** para mejorar la experiencia de usuario.

✅ Se aplicaron validaciones y estructura MVC de Laravel 12.