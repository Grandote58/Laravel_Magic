# 🍰 **Actividad Práctica : Sistema Web Completo para Gestión de Postres**

## 🧭 **Nombre del Proyecto**

**DeliPostres+ – Gestión Avanzada de Catálogo de Postres**

## 🎯 **Objetivos de Aprendizaje**

1. Crear un sistema Laravel para gestionar postres y sus categorías (secciones).
2. Implementar operaciones completas **CRUD** (crear, leer, actualizar, eliminar).
3. Integrar bases de datos correctamente para evitar errores típicos.
4. Insertar datos de forma manual en phpMyAdmin para aprender gestión directa.

## 🧰 **Herramientas Requeridas**



| Herramienta        | Propósito                      |
| ------------------ | ------------------------------ |
| XAMPP              | Servidor local (Apache, MySQL) |
| phpMyAdmin         | Administrar la base de datos   |
| Composer           | Instalar Laravel               |
| Laravel 12         | Framework de desarrollo web    |
| Visual Studio Code | Editor de código               |
| Bootstrap 5        | Framework CSS para diseño      |

## 🏢 **Contexto Empresarial**

La empresa **DeliPostres** quiere organizar su catálogo digital de postres, separándolos por **secciones** (Ejemplo: Pasteles, Helados, Tartas).
 Cada postre debe tener:

- Nombre
- Tipo
- Precio
- Descripción
- Sección a la que pertenece

Además, necesitan una interfaz amigable y control total sobre el catálogo (CRUD completo).

## 🛠️ **Desarrollo Paso a Paso**

### ✅ Paso 1: Crear el Proyecto Laravel

1. Abre la terminal:

```bash
cd C:\xampp\htdocs
composer create-project --prefer-dist laravel/laravel DeliPostresPlus
```

### ✅ Paso 2: Crear la Base de Datos

1. Abre `http://localhost/phpmyadmin`
2. Crea una nueva base de datos: **Nombre:** `delipostresplus_db`

### ✅ Paso 3: Configurar `.env`

Edita tu archivo `.env`:

```css
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=delipostresplus_db
DB_USERNAME=root
DB_PASSWORD=
```

### ✅ Paso 4: Crear Modelos y Migraciones

#### 1. Crear modelo y migración de **Secciones**:

```bash
php artisan make:model Seccion -m
```

Migración `xxxx_create_seccions_table.php`:

```php
Schema::create('seccions', function (Blueprint $table) {
    $table->id();
    $table->string('nombre');
    $table->timestamps();
});
```

#### 2. Crear modelo y migración de **Postres**:

```bash
php artisan make:model Postre -m
```

Migración `xxxx_create_postres_table.php`:

```php
Schema::create('postres', function (Blueprint $table) {
    $table->id();
    $table->string('nombre');
    $table->decimal('precio', 8, 2);
    $table->text('descripcion')->nullable();
    $table->foreignId('seccion_id')->constrained('seccions')->onDelete('cascade');
    $table->timestamps();
});
```

*(Relación: Un postre pertenece a una sección.)*

### ✅ Paso 5: Ejecutar las Migraciones

```bash
php artisan migrate
```

Esto crea las tablas:
 ✔️ `seccions`
 ✔️ `postres`
 ✔️ `migrations`
 ✔️ `password_reset_tokens` (por Laravel)
 ✔️ `sessions` (opcional si creaste migración de sesión)

### ✅ Paso 6: Insertar Datos Manualmente en phpMyAdmin

Observa el video que esta en Youtube --: 

1. Entra en `phpMyAdmin` → `delipostresplus_db`.
2. Ve a la tabla `seccions`.
3. Inserta manualmente algunas secciones:
   - Nombre: **Pasteles**
   - Nombre: **Helados**
   - Nombre: **Tartas**
4. Opcional: Insertar también un par de postres usando `seccion_id`.

### ✅ Paso 7: Crear el Controlador

```bash
php artisan make:controller PostreController --resource
```

**En `app/Http/Controllers/PostreController.php`:**

```php
use App\Models\Postre;
use App\Models\Seccion;
use Illuminate\Http\Request;

class PostreController extends Controller
{
    public function index() {
        $postres = Postre::with('seccion')->get();
        return view('postres.index', compact('postres'));
    }

    public function create() {
        $secciones = Seccion::all();
        return view('postres.create', compact('secciones'));
    }

    public function store(Request $request) {
        $request->validate([
            'nombre' => 'required',
            'precio' => 'required|numeric|min:0',
            'descripcion' => 'nullable',
            'seccion_id' => 'required|exists:seccions,id'
        ]);
        Postre::create($request->all());
        return redirect()->route('postres.index')->with('success', 'Postre creado con éxito.');
    }

    public function edit(Postre $postre) {
        $secciones = Seccion::all();
        return view('postres.edit', compact('postre', 'secciones'));
    }

    public function update(Request $request, Postre $postre) {
        $request->validate([
            'nombre' => 'required',
            'precio' => 'required|numeric|min:0',
            'descripcion' => 'nullable',
            'seccion_id' => 'required|exists:seccions,id'
        ]);
        $postre->update($request->all());
        return redirect()->route('postres.index')->with('success', 'Postre actualizado.');
    }

    public function destroy(Postre $postre) {
        $postre->delete();
        return redirect()->route('postres.index')->with('success', 'Postre eliminado.');
    }
}
```

### ✅ Paso 8: Rutas

En `routes/web.php`:

```php
use App\Http\Controllers\PostreController;

Route::resource('postres', PostreController::class);
Route::get('/', function() {
    return redirect()->route('postres.index');
});
```

### ✅ Paso 9: Crear las Vistas



## ✅ 9.1. Crear la Carpeta de las Vistas

- En tu proyecto `DeliPostresPlus`, abre `resources/views/`
- Crea una nueva carpeta llamada: **postres**
  


```bash
resources/views/postres/
```

👉 Aquí guardaremos todas las vistas relacionadas a "postres".

## ✅ 9.2. Crear la Vista Base de Diseño: `layout.blade.php`

Crea el archivo:

```bash
resources/views/postres/layout.blade.php
```

Este archivo será **la base para todas las demás vistas**. Aquí definimos el **HTML común**, como el menú de navegación y los enlaces a Bootstrap.

### Contenido del archivo `layout.blade.php`:

```php
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>DeliPostres+</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
</head>
<body>

<nav class="navbar navbar-expand-lg navbar-dark bg-dark">
    <div class="container">
        <a class="navbar-brand" href="{{ route('postres.index') }}">DeliPostres+</a>
        <div class="collapse navbar-collapse">
            <ul class="navbar-nav ms-auto">
                <li class="nav-item">
                    <a class="nav-link" href="{{ route('postres.index') }}">Inicio</a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="{{ route('postres.create') }}">Agregar Postre</a>
                </li>
            </ul>
        </div>
    </div>
</nav>

<div class="container py-5">
    @yield('content')
</div>

</body>
</html>
```

📌 **Explicación:**

- `@yield('content')`: marca un lugar donde el contenido **de cada vista específica** será insertado.
- Bootstrap está enlazado por CDN para el estilo.
- Menú de navegación básico.

## ✅ 9.3. Crear la Vista para Listar Postres: `index.blade.php`

Crea el archivo:

```bash
resources/views/postres/index.blade.php
```

### Contenido del archivo `index.blade.php`:

```bash
@extends('postres.layout')

@section('content')
    <h2 class="mb-4">Catálogo de Postres</h2>

    @if(session('success'))
        <div class="alert alert-success">
            {{ session('success') }}
        </div>
    @endif

    <table class="table table-bordered table-striped">
        <thead class="table-dark">
            <tr>
                <th>Nombre</th>
                <th>Sección</th>
                <th>Precio</th>
                <th>Descripción</th>
                <th>Acciones</th>
            </tr>
        </thead>
        <tbody>
            @foreach($postres as $postre)
            <tr>
                <td>{{ $postre->nombre }}</td>
                <td>{{ $postre->seccion->nombre }}</td>
                <td>${{ number_format($postre->precio, 2) }}</td>
                <td>{{ $postre->descripcion }}</td>
                <td>
                    <a href="{{ route('postres.edit', $postre) }}" class="btn btn-warning btn-sm">Editar</a>

                    <form action="{{ route('postres.destroy', $postre) }}" method="POST" style="display:inline;">
                        @csrf
                        @method('DELETE')
                        <button type="submit" class="btn btn-danger btn-sm" onclick="return confirm('¿Seguro que quieres eliminar este postre?')">
                            Eliminar
                        </button>
                    </form>
                </td>
            </tr>
            @endforeach
        </tbody>
    </table>
@endsection
```

📌 **Explicación:**

- Listamos todos los postres en una **tabla bonita**.
- Incluimos botones **Editar** y **Eliminar**.

## ✅ 9.4. Crear el Formulario Común: `form.blade.php`

Crea el archivo:

```bash
resources/views/postres/form.blade.php
```

Este será **el formulario reutilizable** tanto para crear como para editar.

### Contenido del archivo `form.blade.php`:

```php
<div class="mb-3">
    <label class="form-label">Nombre del Postre</label>
    <input type="text" name="nombre" value="{{ old('nombre', $postre->nombre ?? '') }}" class="form-control" required>
</div>

<div class="mb-3">
    <label class="form-label">Sección</label>
    <select name="seccion_id" class="form-control" required>
        <option value="">-- Seleccione una sección --</option>
        @foreach($secciones as $seccion)
            <option value="{{ $seccion->id }}" @selected(old('seccion_id', $postre->seccion_id ?? '') == $seccion->id)>
                {{ $seccion->nombre }}
            </option>
        @endforeach
    </select>
</div>

<div class="mb-3">
    <label class="form-label">Precio</label>
    <input type="number" step="0.01" name="precio" value="{{ old('precio', $postre->precio ?? '') }}" class="form-control" required>
</div>

<div class="mb-3">
    <label class="form-label">Descripción</label>
    <textarea name="descripcion" class="form-control">{{ old('descripcion', $postre->descripcion ?? '') }}</textarea>
</div>
```

📌 **Explicación:**

- Formulario dinámico.
- Selección de sección.
- Campos de nombre, precio y descripción.

## ✅ 9.5. Crear la Vista para Crear Postre: `create.blade.php`

Crea el archivo:

```bash
resources/views/postres/create.blade.php
```

### Contenido del archivo `create.blade.php`:

```php
@extends('postres.layout')

@section('content')
    <h2 class="mb-4">Agregar Nuevo Postre</h2>

    @if($errors->any())
        <div class="alert alert-danger">
            <ul>@foreach($errors->all() as $error) <li>{{ $error }}</li> @endforeach</ul>
        </div>
    @endif

    <form method="POST" action="{{ route('postres.store') }}">
        @csrf
        @include('postres.form')

        <button type="submit" class="btn btn-primary">Guardar</button>
    </form>
@endsection
```

📌 **Explicación:**

- Incluimos el formulario `form.blade.php`.
- Manejamos errores si algún campo es inválido.

## ✅ 9.6. Crear la Vista para Editar Postre: `edit.blade.php`

Crea el archivo:

```bash
resources/views/postres/edit.blade.php
```

### Contenido del archivo `edit.blade.php`:

```php
@extends('postres.layout')

@section('content')
    <h2 class="mb-4">Editar Postre</h2>

    @if($errors->any())
        <div class="alert alert-danger">
            <ul>@foreach($errors->all() as $error) <li>{{ $error }}</li> @endforeach</ul>
        </div>
    @endif

    <form method="POST" action="{{ route('postres.update', $postre) }}">
        @csrf
        @method('PUT')
        @include('postres.form')

        <button type="submit" class="btn btn-success">Actualizar</button>
    </form>
@endsection
```

📌 **Explicación:**

- Mismo formulario de creación, pero ahora para actualizar.
- Se rellenan los valores antiguos.

# 🎯 **Con esto finalizamos toda la parte de vistas**

Ahora tu proyecto tiene:

| Vistas             | Función                        |
| ------------------ | ------------------------------ |
| `layout.blade.php` | Estructura base con navegación |
| `index.blade.php`  | Listar postres                 |
| `form.blade.php`   | Formulario reutilizable        |
| `create.blade.php` | Crear nuevo postre             |
| `edit.blade.php`   | Editar postre existente        |

### ✅ Paso 10: Ejecutar el Proyecto

```bash
php artisan serve
```

Abrir navegador:

 👉 `http://127.0.0.1:8000/postres`

# 🎯 **Resultado Final Esperado**

| Función          | Resultado                                  |
| ---------------- | ------------------------------------------ |
| Ver postres      | Listado de postres con su sección y precio |
| Agregar postres  | Formulario con validaciones                |
| Editar postres   | Actualizar datos existentes                |
| Eliminar postres | Borrar registros de forma segura           |
| Navegación       | Barra de menú arriba con Bootstrap         |



# 🎯 **Solucionando Errores**

# **🛠️error Add [_token] to fillable property to allow mass assignment on [App\Models\Postre]**



Laravel no deja que cualquier dato que venga del formulario sea guardado directamente en la base de datos **por seguridad** (protección de asignación masiva).

Cuando envías un formulario, Laravel **ve el campo `_token`** (el token CSRF de seguridad) **junto a tus datos**, pero como tu **modelo `Postre`** **no sabe** qué campos puede aceptar de manera masiva (`fillable`), **lanza ese error**.

## 🛠️ Solución Paso a Paso

### ✅ 1. Abrir el Modelo `Postre.php`

Ve a:

```bash
app/Models/Postre.php
```

Actualmente debe verse muy simple, como esto:

```php
namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Postre extends Model
{
    use HasFactory;
}
```

### ✅ 2. Agregar la propiedad `$fillable`

Debes **especificar qué campos se pueden asignar masivamente** (los campos que vienen del formulario).

Actualiza tu `Postre.php` a esto:

```php
namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Postre extends Model
{
    use HasFactory;

    protected $fillable = [
        'nombre',
        'precio',
        'descripcion',
        'seccion_id',
    ];
}
```

## 🎯 Explicación rápida:

| Propiedad   | Significado                                                  |
| ----------- | ------------------------------------------------------------ |
| `$fillable` | Son los nombres de las columnas que **Laravel permitirá guardar automáticamente** desde un formulario |



# **🛠️ error Call to undefined relationship [seccion] on model [App\Models\Postre].**

Significa que **Laravel está intentando usar una relación `seccion()`** en el modelo `Postre`, **pero no la has definido aún** en tu clase `Postre`.

## 🛠️ Solución Paso a Paso

## ✅ 1. ¿Por qué pasa el error?

Cuando hicimos en la vista algo como:

```php
{{ $postre->seccion->nombre }}
```

o en el controlador:

```php
Postre::with('seccion')->get();
```

Laravel **espera encontrar** en tu modelo **`Postre`** una relación llamada **`seccion()`**, pero **no existe todavía**.

## ✅ 2. ¿Cómo solucionarlo? → Definir la relación en el modelo `Postre`

Abre tu archivo:

```php
app/Models/Postre.php
```

Y **agrega** este método **dentro de la clase**:

```php
public function seccion()
{
    return $this->belongsTo(Seccion::class);
}
```

### Ejemplo completo de cómo debe quedar tu modelo `Postre.php`:

```php
namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;
use App\Models\Seccion; // 👈 Importar el modelo Seccion

class Postre extends Model
{
    use HasFactory;

    protected $fillable = [
        'nombre',
        'precio',
        'descripcion',
        'seccion_id',
    ];

    // Definir relación con Seccion
    public function seccion()
    {
        return $this->belongsTo(Seccion::class);
    }
}
```

## 🎯 Explicación rápida:

| Concepto      | Significado                                                  |
| ------------- | ------------------------------------------------------------ |
| `belongsTo`   | Indica que un `Postre` **pertenece a una Sección**           |
| Se usa porque | En la tabla `postres`, existe una columna `seccion_id` que es clave foránea a `seccions.id` |



✅ Con este método definido, Laravel podrá entender cómo encontrar **qué sección pertenece a cada postre**.

### **Ejecutar el Proyecto**

```bash
php artisan serve
```

Abrir navegador:

 👉 `http://127.0.0.1:8000/postres`



