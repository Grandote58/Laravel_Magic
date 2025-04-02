# 📘 **Práctica Guiada: Gestión de Proyectos en JUANES S.A.**

### 🧭 Semana 2 Extendida – Uso de Rutas, Controladores y Bootstrap en Laravel 12

## 🧩 **Nombre de la Práctica:**

**“JUANES S.A.: Construyendo un Panel de Proyectos con Laravel y Bootstrap”**

## 🎯 **Objetivos de Aprendizaje**

1. Implementar rutas y controladores en Laravel 12 con conexión a base de datos.
2. Integrar Bootstrap 5 para mejorar el diseño y usabilidad de la interfaz.
3. Desarrollar una vista dinámica que muestre proyectos de una base de datos con diseño profesional.

## 🧰 **Aplicaciones Necesarias**

| Herramienta            | Función                          |
| ---------------------- | -------------------------------- |
| **XAMPP**              | Servidor Apache + MySQL          |
| **Composer**           | Gestor de dependencias           |
| **Laravel 12**         | Framework PHP                    |
| **phpMyAdmin**         | Gestión de base de datos         |
| **Visual Studio Code** | Editor de código                 |
| **Bootstrap 5 (CDN)**  | Framework CSS para diseño visual |

## 🧪 **Caso de Estudio: Panel de Proyectos para JUANES S.A.**

**Contexto:**

 **JUANES S.A.** es una empresa que gestiona múltiples proyectos tecnológicos. Necesitan una **interfaz web** para listar y consultar los detalles de sus proyectos. La presentación debe ser visualmente amigable para sus ejecutivos, usando **Bootstrap**.



## 🧠 **Desarrollo Paso a Paso**

### ✅ **Paso 1: Crear Base de Datos y Configuración**

1. Abre `http://localhost/phpmyadmin`
2. Crea una base de datos llamada: `juanes_db`
3. Deja el cotejamiento por defecto y presiona “Crear”

### ✅ **Paso 2: Crear el Proyecto Laravel**

Desde la terminal:

```bash
cd C:\xampp\htdocs
composer create-project --prefer-dist laravel/laravel JuanesPortal
cd JuanesPortal
```

### ✅ **Paso 3: Configurar Laravel para Conectar a MySQL**

1. Abre el proyecto en VS Code
2. En el archivo `.env`, edita:

```php
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=juanes_db
DB_USERNAME=root
DB_PASSWORD=
```

### ✅ **Paso 4: Crear Migración y Modelo para “Proyectos”**

```bash
php artisan make:model Proyecto -m
```

Edita la migración en `database/migrations/xxxx_create_proyectos_table.php`:

```php
Schema::create('proyectos', function (Blueprint $table) {
    $table->id();
    $table->string('nombre');
    $table->string('lider');
    $table->text('descripcion');
    $table->string('estado'); // Activo, En pausa, Finalizado
    $table->timestamps();
});
```

Aplica la migración:

```bash
php artisan migrate
```

### ✅ **Paso 5: Crear Controlador**

```php
php artisan make:controller ProyectoController
```

Edita `app/Http/Controllers/ProyectoController.php`:

```html
namespace App\Http\Controllers;

use App\Models\Proyecto;

class ProyectoController extends Controller
{
    public function index()
    {
        $proyectos = Proyecto::all();
        return view('proyectos.index', compact('proyectos'));
    }

    public function show($id)
    {
        $proyecto = Proyecto::findOrFail($id);
        return view('proyectos.show', compact('proyecto'));
    }
}
```

### ✅ **Paso 6: Rutas Web**

En `routes/web.php`:

```php
use App\Http\Controllers\ProyectoController;

Route::get('/proyectos', [ProyectoController::class, 'index']);
Route::get('/proyectos/{id}', [ProyectoController::class, 'show']);
```

### ✅ **Paso 7: Crear Vistas con Bootstrap**

📁 Crea la carpeta `resources/views/proyectos/`

🔹 `index.blade.php`:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Proyectos - JUANES S.A.</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
</head>
<body class="bg-light">
<div class="container py-5">
    <h1 class="mb-4">Listado de Proyectos - JUANES S.A.</h1>
    <div class="row row-cols-1 row-cols-md-2 g-4">
        @foreach ($proyectos as $proyecto)
            <div class="col">
                <div class="card h-100 shadow-sm">
                    <div class="card-body">
                        <h5 class="card-title">{{ $proyecto->nombre }}</h5>
                        <p class="card-text">{{ Str::limit($proyecto->descripcion, 100) }}</p>
                        <span class="badge bg-info">{{ $proyecto->estado }}</span>
                        <a href="/proyectos/{{ $proyecto->id }}" class="btn btn-sm btn-primary mt-3">Ver más</a>
                    </div>
                </div>
            </div>
        @endforeach
    </div>
</div>
</body>
</html>
```

🔹 `show.blade.php`:

```php+HTML
<!DOCTYPE html>
<html>
<head>
    <title>Proyecto - {{ $proyecto->nombre }}</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
</head>
<body class="bg-white">
<div class="container py-5">
    <a href="/proyectos" class="btn btn-secondary mb-4">← Volver</a>
    <h2>{{ $proyecto->nombre }}</h2>
    <p><strong>Líder del Proyecto:</strong> {{ $proyecto->lider }}</p>
    <p><strong>Descripción:</strong> {{ $proyecto->descripcion }}</p>
    <span class="badge bg-success">{{ $proyecto->estado }}</span>
</div>
</body>
</html>
```

### ✅ **Paso 8: Agregar Datos Manuales (Opcional para pruebas)**

Desde `phpMyAdmin` o usando Tinker:

```php+HTML
\App\Models\Proyecto::create([
  'nombre' => 'Plataforma Interna',
  'lider' => 'Carla Torres',
  'descripcion' => 'Sistema para automatizar procesos internos.',
  'estado' => 'Activo'
]);
```

## 📝 **Resumen de Aprendizajes**

✅ Creaste una **base de datos** real conectada con Laravel.

✅ Usaste rutas y controladores para organizar la lógica.

✅ Implementaste **Bootstrap** para una interfaz más profesional.

✅ Simulaste un caso de uso real en JUANES S.A.

