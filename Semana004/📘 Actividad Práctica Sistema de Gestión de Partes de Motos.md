# 📘 **Actividad Práctica: Sistema de Gestión de Partes de Motos**

## 🧭 **Nombre del Proyecto:**

**MotoStock – Inventario Inteligente de Repuestos para Motos**

## 🎯 **Objetivos de Aprendizaje**

1. Crear una aplicación en Laravel para gestionar el inventario de partes de motos.
2. Implementar un modelo, migración, controlador, vistas y rutas con validación de datos.
3. Usar Bootstrap 5 para una interfaz amigable y profesional.

## 🧰 **Herramientas Necesarias**



| Herramienta | Propósito                             |
| ----------- | ------------------------------------- |
| XAMPP       | Servidor Apache y base de datos MySQL |
| Composer    | Gestor de dependencias de PHP         |
| Laravel 12  | Framework de desarrollo web           |
| VS Code     | Editor de código                      |
| phpMyAdmin  | Gestor visual de bases de datos       |
| Bootstrap 5 | Framework CSS para diseño visual      |



## 🏢 **Escenario Empresarial:**

**MotoStock**, un taller especializado en repuestos de motocicletas, necesita digitalizar su inventario. Requiere un sistema donde se puedan registrar nuevas piezas (nombre, marca, tipo, cantidad, precio) y visualizarlas en una interfaz clara.

## 🛠️ **Desarrollo Paso a Paso**

### ✅ **Paso 1: Crear el Proyecto Laravel**

1. Abre la terminal de Windows (CMD o PowerShell).
2. Dirígete a la carpeta de proyectos de XAMPP:

```bash
cd C:\xampp\htdocs
```

1. Crea el proyecto:

```bash
composer create-project --prefer-dist laravel/laravel MotoStock
```

### ✅ **Paso 2: Crear la Base de Datos**

1. Accede a `http://localhost/phpmyadmin` desde tu navegador.
2. Haz clic en "Nueva" y crea una base de datos llamada `motostock_db`.

### ✅ **Paso 3: Configurar el archivo `.env`**

1. Abre el proyecto `MotoStock` en **Visual Studio Code**.
2. Abre el archivo `.env` y configura la conexión con MySQL:

```bash
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=motostock_db
DB_USERNAME=root
DB_PASSWORD=
```

### ✅ **Paso 4: Crear el Modelo y la Migración**

```bash
php artisan make:model Parte -m
```

Edita el archivo de migración en `database/migrations/xxxx_create_partes_table.php`:

```bash
Schema::create('partes', function (Blueprint $table) {
    $table->id();
    $table->string('nombre');
    $table->string('marca');
    $table->string('tipo');
    $table->integer('cantidad');
    $table->decimal('precio', 8, 2);
    $table->timestamps();
});
```

Ejecuta la migración:

```php
php artisan migrate
```

### ✅ **Paso 5: Crear el Controlador**

```php
php artisan make:controller ParteController
```

Edita `app/Http/Controllers/ParteController.php`:

```html
use App\Models\Parte;
use Illuminate\Http\Request;

class ParteController extends Controller
{
    public function index() {
        $partes = Parte::all();
        return view('partes.index', compact('partes'));
    }

    public function create() {
        return view('partes.create');
    }

    public function store(Request $request) {
        $request->validate([
            'nombre' => 'required',
            'marca' => 'required',
            'tipo' => 'required',
            'cantidad' => 'required|integer|min:0',
            'precio' => 'required|numeric|min:0'
        ]);

        Parte::create($request->all());

        return redirect('/partes')->with('success', 'Parte registrada exitosamente');
    }
}
```

### ✅ **Paso 6: Agregar las Rutas**

En `routes/web.php`:

```bash
use App\Http\Controllers\ParteController;

Route::get('/partes', [ParteController::class, 'index']);
Route::get('/partes/create', [ParteController::class, 'create']);
Route::post('/partes', [ParteController::class, 'store']);
```

### ✅ **Paso 7: Crear las Vistas**

📁 Crea la carpeta `resources/views/partes/`

🔹 `create.blade.php`:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Agregar Parte de Moto</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
</head>
<body>
<div class="container py-5">
    <h2>Registrar Nueva Parte</h2>

    @if ($errors->any())
        <div class="alert alert-danger">
            <ul>
                @foreach ($errors->all() as $error) <li>{{ $error }}</li> @endforeach
            </ul>
        </div>
    @endif

    <form method="POST" action="/partes">
        @csrf
        <div class="mb-3">
            <label>Nombre</label>
            <input type="text" name="nombre" class="form-control" required>
        </div>
        <div class="mb-3">
            <label>Marca</label>
            <input type="text" name="marca" class="form-control" required>
        </div>
        <div class="mb-3">
            <label>Tipo</label>
            <input type="text" name="tipo" class="form-control" required>
        </div>
        <div class="mb-3">
            <label>Cantidad</label>
            <input type="number" name="cantidad" class="form-control" required>
        </div>
        <div class="mb-3">
            <label>Precio</label>
            <input type="number" step="0.01" name="precio" class="form-control" required>
        </div>
        <button type="submit" class="btn btn-primary">Guardar</button>
    </form>
</div>
</body>
</html>
```

🔹 `index.blade.php`:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Inventario de Partes</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
</head>
<body>
<div class="container py-5">
    <h2>Inventario de Partes de Motos</h2>
    <a href="/partes/create" class="btn btn-success mb-4">Agregar Nueva Parte</a>

    @if(session('success'))
        <div class="alert alert-success">{{ session('success') }}</div>
    @endif

    <table class="table table-bordered">
        <thead>
            <tr>
                <th>Nombre</th>
                <th>Marca</th>
                <th>Tipo</th>
                <th>Cantidad</th>
                <th>Precio</th>
            </tr>
        </thead>
        <tbody>
            @foreach($partes as $parte)
                <tr>
                    <td>{{ $parte->nombre }}</td>
                    <td>{{ $parte->marca }}</td>
                    <td>{{ $parte->tipo }}</td>
                    <td>{{ $parte->cantidad }}</td>
                    <td>${{ number_format($parte->precio, 2) }}</td>
                </tr>
            @endforeach
        </tbody>
    </table>
</div>
</body>
</html>
```

### ✅ **Paso 8: Ejecutar el Proyecto**

1. En la terminal, inicia el servidor Laravel:

```bash
php artisan serve
```

1. Abre en el navegador:
   - `http://127.0.0.1:8000/partes/create` para agregar partes.
   - `http://127.0.0.1:8000/partes` para visualizar el inventario.

## 📝 **Conclusión**

✅ Has creado una aplicación Laravel desde cero.

✅ Lograste conectar un formulario con validaciones y una base de datos.

✅ Aplicaste principios de seguridad y buenas prácticas.

✅ Usaste Bootstrap para mejorar la experiencia de usuario.