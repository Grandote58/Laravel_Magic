# 🧭 **Semana 4: Diseñando Formularios Inteligentes y Seguros en Laravel 12**

## 🌟 **“Del Formulario a la Base de Datos: Validación y Seguridad en Tiempo Real”**

## 🎯 **Objetivos de Aprendizaje**

1. Aprender a crear, procesar y validar formularios en Laravel 12, usando buenas prácticas y reglas de validación.

   

2. Comprender y aplicar mecanismos de **seguridad como CSRF y protección contra XSS** en formularios web.

## 🎯 **Propósito de la Actividad**

En el desarrollo de aplicaciones web, la **interacción del usuario a través de formularios** es uno de los pilares fundamentales. Laravel ofrece herramientas potentes para **crear, validar y proteger** estos formularios. 

Esta semana nos centraremos en implementar un formulario de **registro de usuarios**, reforzado con **validaciones integradas** y protegido ante ataques comunes como **CSRF y XSS**.

**Al finalizar esta semana, el estudiante será capaz de:**

 ✅ Crear formularios funcionales y bien estructurados con Blade.

 ✅ Aplicar **validaciones tanto en el controlador como en la vista**.

 ✅ Proteger formularios ante vulnerabilidades comunes.

 ✅ Usar las herramientas de Laravel 12 para gestionar datos de forma segura y efectiva.

## 🧠 **Conceptos Claves con Ejemplos Reales**

### 🔹 1. **Creación de Formularios con Blade**

Laravel usa su sistema de plantillas **Blade** para facilitar la creación de formularios HTML. En Blade, los formularios se pueden estructurar de manera clara y ordenada, y pueden incluir componentes visuales como campos, botones y mensajes de error.

Laravel no tiene helpers automáticos como en otros frameworks, pero **Blade facilita la construcción limpia y reutilizable de formularios HTML**. Laravel también interpreta correctamente los métodos `POST`, `PUT`, y `DELETE`.

```html
<form method="POST" action="/usuarios">
    @csrf
    <label>Nombre</label>
    <input type="text" name="nombre">
    <button type="submit">Guardar</button>
</form>
```

**Ejemplo Real:**
Una empresa de consultoría necesita que nuevos clientes se registren en su plataforma para acceder a sus servicios. El formulario de registro incluirá nombre, email y contraseña.

### 🔹 2. **Validación de Datos en Laravel 12**

Laravel ofrece múltiples formas de validar datos. La más común es a través del método `validate()` en el controlador o permite validar fácilmente campos antes de guardar en base de datos, usando la clase `Request` o directamente en el controlador..

**Reglas comunes:**

- `required` → campo obligatorio
- `email` → debe tener formato de email
- `min:x` / `max:x` → longitud mínima/máxima
- `confirmed` → se usa para validar campos repetidos (como contraseñas)

**Ejemplo Real:**

 Validar que el correo electrónico no esté ya registrado y que la contraseña tenga al menos 8 caracteres.

```php
$request->validate([
    'nombre' => 'required|string|max:255',
    'email' => 'required|email|unique:users',
    'password' => 'required|min:8|confirmed'
]);
```

La validación devuelve errores automáticamente que puedes mostrar en la vista:

```php
@if ($errors->any())
    <div class="alert alert-danger">
        <ul>
            @foreach ($errors->all() as $error)
                <li>{{ $error }}</li>
            @endforeach
        </ul>
    </div>
@endif
```

### 🔹 3. **Protección CSRF (Cross-Site Request Forgery)**

Laravel incluye protección automática contra CSRF usando tokens. Cada formulario debe incluir la directiva `@csrf`.

```php
<form method="POST" action="/registro">
    @csrf
    <!-- campos -->
</form>
```

Laravel valida automáticamente que el token sea válido antes de procesar el formulario.

### 🔹 4. **Protección contra XSS (Cross-Site Scripting)**

Laravel escapa automáticamente el contenido de variables mostradas con `{{ }}` para evitar que se inyecte código malicioso.

Si se desea mostrar HTML confiable, se puede usar `{!! !!}`, pero **debe evitarse con datos del usuario.**

```html
<!-- Segura -->
{{ $usuario->nombre }}

<!-- No segura -->
{!! $usuario->nombre !!}
```

# 🧪 **ACTIVIDAD PRÁCTICA PASO A PASO: Registro Seguro de Colaboradores para JUANES S.A.**

### 📌 **Escenario Empresarial:**

La empresa **JUANES S.A.** necesita una solución web interna para registrar a sus nuevos colaboradores. Este sistema debe permitir ingresar datos desde un formulario seguro, validado y con diseño profesional usando **Bootstrap 5**.

La solución debe:

- Guardar los datos de cada colaborador en la base de datos.
- Validar campos en el backend.
- Evitar vulnerabilidades (como CSRF y XSS).
- Proporcionar retroalimentación visual con mensajes de error o éxito.

## 🧰 **Herramientas Requeridas**



| Herramienta        | Propósito                       |
| ------------------ | ------------------------------- |
| XAMPP              | Servidor local (Apache + MySQL) |
| phpMyAdmin         | Interfaz visual para MySQL      |
| Composer           | Gestor de paquetes PHP          |
| Visual Studio Code | Editor de código                |
| Laravel 12         | Framework backend               |
| Bootstrap 5 (CDN)  | Estilos visuales frontend       |

## 🛠️ **DESARROLLO PASO A PASO**

### ✅ **Paso 1: Crear el Proyecto Laravel**

1. Asegúrate de que **XAMPP esté iniciado** con **Apache** y **MySQL activos**.
2. Abre la terminal de Windows (CMD o PowerShell).
3. Navega a la carpeta de proyectos en XAMPP:

```bash
cd C:\xampp\htdocs
```

1. Crea un nuevo proyecto Laravel:

```bash
composer create-project --prefer-dist laravel/laravel JuanesPortal
```

Esto generará una carpeta llamada `JuanesPortal` con el esqueleto del proyecto Laravel.

### ✅ **Paso 2: Crear la Base de Datos**

1. Abre tu navegador y accede a:

   `http://localhost/phpmyadmin`

   

2. Haz clic en "Nueva" y escribe como nombre de la base de datos:

    `juanes_db`

   

3. Deja el cotejamiento como está (`utf8mb4_unicode_ci`) y haz clic en "Crear".

### ✅ **Paso 3: Configurar el Archivo `.env`**

1. Abre el proyecto `JuanesPortal` en **Visual Studio Code**.
2. Busca el archivo `.env` en la raíz del proyecto.
3. Modifica los siguientes valores para conectar con la base de datos que creaste:

```bash
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=juanes_db
DB_USERNAME=root
DB_PASSWORD=
```

1. Guarda el archivo.

### ✅ **Paso 4: Crear el Modelo y la Migración para “Colaborador”**

1. En la terminal, ejecuta:

```bash
php artisan make:model Colaborador -m
```

1. Laravel creará:
   - El modelo: `app/Models/Colaborador.php`
   - La migración: `database/migrations/xxxx_create_colaboradors_table.php`
2. Abre la migración y reemplaza el contenido del método `up()`:

```php
Schema::create('colaboradors', function (Blueprint $table) {
    $table->id();
    $table->string('nombre');
    $table->string('email')->unique();
    $table->string('cargo');
    $table->timestamps();
});
```

1. Ejecuta la migración:

```bash
php artisan migrate
```

Esto creará la tabla `colaboradors` en la base de datos `juanes_db`.

### ✅ **Paso 5: Crear el Controlador**

1. En la terminal, ejecuta:

```bash
php artisan make:controller ColaboradorController
```

1. Abre `app/Http/Controllers/ColaboradorController.php` y agrega:

```php
use App\Models\Colaborador;
use Illuminate\Http\Request;

class ColaboradorController extends Controller
{
    public function create() {
        return view('colaboradores.create');
    }

    public function store(Request $request) {
        $request->validate([
            'nombre' => 'required|min:3|max:50',
            'email' => 'required|email|unique:colaboradors',
            'cargo' => 'required'
        ]);

        Colaborador::create($request->all());

        return redirect('/colaboradores/create')->with('success', 'Colaborador registrado con éxito.');
    }
}
```

### ✅ **Paso 6: Agregar las Rutas Web**

1. Abre `routes/web.php`
2. Agrega las siguientes líneas:

```php
use App\Http\Controllers\ColaboradorController;

Route::get('/colaboradores/create', [ColaboradorController::class, 'create']);
Route::post('/colaboradores', [ColaboradorController::class, 'store']);
```

### ✅ **Paso 7: Crear la Vista con Formulario y Bootstrap**

1. Crea la carpeta: `resources/views/colaboradores/`
2. Dentro, crea el archivo `create.blade.php`
3. Pega este contenido:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Registrar Colaborador</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
</head>
<body>
<div class="container py-5">
    <h2 class="mb-4">Registrar Colaborador - JUANES S.A.</h2>

    @if (session('success'))
        <div class="alert alert-success">{{ session('success') }}</div>
    @endif

    @if ($errors->any())
        <div class="alert alert-danger">
            <ul>
                @foreach ($errors->all() as $error) <li>{{ $error }}</li> @endforeach
            </ul>
        </div>
    @endif

    <form method="POST" action="/colaboradores">
        @csrf
        <div class="mb-3">
            <label class="form-label">Nombre</label>
            <input type="text" name="nombre" class="form-control" required>
        </div>
        <div class="mb-3">
            <label class="form-label">Correo Electrónico</label>
            <input type="email" name="email" class="form-control" required>
        </div>
        <div class="mb-3">
            <label class="form-label">Cargo</label>
            <input type="text" name="cargo" class="form-control" required>
        </div>
        <button type="submit" class="btn btn-primary">Registrar</button>
    </form>
</div>
</body>
</html>
```

### ✅ **Paso 8: Ejecutar el Proyecto**

1. En la terminal, ejecuta:

```bash
php artisan serve
```

1. Abre tu navegador y visita:

   `http://127.0.0.1:8000/colaboradores/create`

   

2. Rellena el formulario y haz pruebas:

   - Con datos válidos
   - Dejando campos vacíos
   - Duplicando emails

Verás cómo Laravel maneja las validaciones y muestra errores con Bootstrap.

## 📝 **Reflexión Final**

Esta actividad te permitió:

 ✅ Crear un flujo completo de entrada de datos en Laravel.

 ✅ Aplicar validaciones en tiempo real en el servidor.

 ✅ Usar Bootstrap para hacer la experiencia visual clara y amigable.

 ✅ Proteger tu aplicación usando `@csrf` y validación automática contra XSS.

