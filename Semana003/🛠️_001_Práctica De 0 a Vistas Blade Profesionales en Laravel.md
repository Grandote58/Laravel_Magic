# 🛠️ **Práctica: De 0 a Vistas Blade Profesionales en Laravel**

### ✅ **Propósito de la práctica**

Desarrollar un mini sitio web en Laravel utilizando vistas Blade, layouts reutilizables y buenas prácticas de organización. Al finalizar, tendrás un proyecto funcional que podrás escalar o adaptar a tus propios proyectos.



### 🧩 **Paso 1: Instalar Laravel 12**

#### Requisitos previos:

- PHP ≥ 8.2
- Composer
- Servidor local (XAMPP, Laravel Valet, Docker o Laravel Herd)

#### Comandos:

```bash
composer create-project laravel/laravel vista-blade
cd vista-blade
php artisan serve
```

🧠 Esto iniciará tu servidor en `http://localhost:8000`



### 📁 **Paso 2: Estructura del proyecto**

Organiza tu carpeta `resources/views` así:

```css
views/
├── layouts/
│   └── app.blade.php
├── pages/
│   ├── inicio.blade.php
│   └── contacto.blade.php
└── bienvenida.blade.php
```

Usar subcarpetas te ayudará a mantener todo ordenado 🧹

### 🛤️ **Paso 3: Crear la ruta y enviar datos**

Edita `routes/web.php`:

```php+HTML
use Illuminate\Support\Facades\Route;

Route::get('/', function () {
    $nombre = 'Estudiante Laravel';
    return view('bienvenida', compact('nombre'));
});

Route::get('/inicio', function () {
    return view('pages.inicio');
});

Route::get('/contacto', function () {
    return view('pages.contacto');
});
```

------

### 🎨 **Paso 4: Crear tu primera vista con Blade**

Crea `resources/views/bienvenida.blade.php`:

```php+HTML
<!DOCTYPE html>
<html>
<head>
    <title>Bienvenida</title>
</head>
<body>
    <h1>¡Hola, {{ $nombre }}! 🎉</h1>
    <p>Bienvenido a tu primer sitio con Laravel y Blade.</p>
    <a href="/inicio">Ir a Inicio</a> | <a href="/contacto">Contacto</a>
</body>
</html>
```

🎯 Aquí practicas:

- Paso de variables desde el controlador
- Uso de `{{ }}` para imprimir
- Enlaces entre vistas



### 🧱 **Paso 5: Crear un layout base y heredarlo**

**Archivo:** `resources/views/layouts/app.blade.php`

```php+HTML
<!DOCTYPE html>
<html>
<head>
    <title>@yield('titulo', 'Mi sitio Laravel')</title>
</head>
<body>
    <header>
        <h1>🌐 Mi Web Laravel</h1>
        <nav>
            <a href="/">Inicio</a> |
            <a href="/contacto">Contacto</a>
        </nav>
        <hr>
    </header>

    <main>
        @yield('contenido')
    </main>

    <footer>
        <hr>
        <p>&copy; {{ date('Y') }} Mi Web Laravel</p>
    </footer>
</body>
</html>
```

### 🧬 **Paso 6: Crear vistas que extiendan el layout**

**Archivo:** `resources/views/pages/inicio.blade.php`

```php+HTML
@extends('layouts.app')

@section('titulo', 'Inicio')

@section('contenido')
    <h2>Bienvenido a la página de inicio 🏠</h2>
    <p>Laravel hace que todo sea más fácil.</p>
@endsection
```

**Archivo:** `resources/views/pages/contacto.blade.php`

```php+HTML
@extends('layouts.app')

@section('titulo', 'Contacto')

@section('contenido')
    <h2>Contacto 📞</h2>
    <p>Puedes escribirnos a contacto@miweb.com</p>
@endsection
```

### 🧪 **Paso 7: Añadir componentes reutilizables (opcional)**

**Crear un botón reutilizable**

Archivo: `resources/views/components/boton.blade.php`

```php
<a href="{{ $url }}" class="btn">{{ $slot }}</a>
```

Usar en vista:

```html
<x-boton url="/contacto">Ir a Contacto</x-boton>
```

### 🚀 **Paso 8: Verifica tu sitio**

Abre en el navegador:

- http://localhost:8000 → Vista de bienvenida
- http://localhost:8000/inicio → Layout + contenido dinámico
- http://localhost:8000/contacto → Segundo contenido

### 🔁 **Extensión sugerida**

- Crea una vista `about.blade.php` con tu biografía profesional.
- Implementa un layout `admin.blade.php` con estilo diferente.
- Usa componentes Blade para crear una tarjeta reutilizable de producto o usuario.

------

### 📣 **Consejos finales**

- 📦 Usa `php artisan make:controller` para separar lógica de vistas.
- 🧪 Integra Blade con controladores y modelos para proyectos más robustos.
- 🤝 Comparte tu proyecto en GitHub o entre compañeros y pide feedback.



# 🧠 Extensión de Práctica: Ampliando tu Sitio con Biografía, Layout de Admin y Componentes

### 🎯 Objetivos de la extensión

- 🧩 Crear nuevas vistas que amplíen el sitio.
- 🎨 Diseñar y aplicar un segundo layout exclusivo para la sección de administración.
- ♻️ Desarrollar componentes Blade reutilizables que mejoren la eficiencia del código.
- 👨‍💻 Fomentar la personalización del proyecto como parte del aprendizaje activo.

### 🧬 Paso 1: Crear la vista de biografía (`about`)

#### 1.1 Crear la ruta en `routes/web.php`

```css
Route::get('/sobre-mi', function () {
    return view('pages.about');
});
```

#### 1.2 Crear la vista `resources/views/pages/about.blade.php`

```php
@extends('layouts.app')

@section('titulo', 'Sobre mí')

@section('contenido')
    <h2>Sobre mí 👨‍💻</h2>
    <p>Mi nombre es {{ $nombre ?? 'Tu nombre' }}. Soy desarrollador con pasión por Laravel y la educación.</p>
    <p>He trabajado en múltiples proyectos donde Laravel fue la columna vertebral del backend, aprovechando su potencia, claridad y rapidez.</p>
@endsection
```

💡 Puedes pasar el nombre desde la ruta para practicar `compact()`:

```php+HTML
Route::get('/sobre-mi', function () {
    $nombre = 'Luis Dev';
    return view('pages.about', compact('nombre'));
});
```

### 🧱 Paso 2: Crear un nuevo layout para administración

#### 2.1 Crear archivo: `resources/views/layouts/admin.blade.php`

```css
<!DOCTYPE html>
<html>
<head>
    <title>Admin - @yield('titulo', 'Panel')</title>
    <style>
        body { font-family: Arial; background-color: #f2f2f2; margin: 0; }
        .admin-header { background: #333; color: white; padding: 10px; }
        .admin-nav { background: #444; color: white; padding: 5px; }
        .admin-content { padding: 20px; }
    </style>
</head>
<body>
    <div class="admin-header">
        <h1>Panel de Administración 🛠️</h1>
    </div>
    <div class="admin-nav">
        <a href="/admin/usuarios">Usuarios</a> |
        <a href="/">Ir al sitio</a>
    </div>
    <div class="admin-content">
        @yield('contenido')
    </div>
</body>
</html>
```

------

### 🔐 Paso 3: Crear una vista que use el layout `admin.blade.php`

#### 3.1 Crear la ruta:

```php
Route::get('/admin/usuarios', function () {
    $usuarios = ['Ana', 'Carlos', 'Lucía'];
    return view('admin.usuarios', compact('usuarios'));
});
```

#### 3.2 Crear archivo: `resources/views/admin/usuarios.blade.php`

```php
@extends('layouts.admin')

@section('titulo', 'Usuarios')

@section('contenido')
    <h2>Lista de Usuarios 👥</h2>
    <ul>
        @foreach($usuarios as $usuario)
            <li>{{ $usuario }}</li>
        @endforeach
    </ul>
@endsection
```

📘 Aquí se aplican:

- Uso de layouts alternativos
- Paso de datos desde rutas
- Bucle `@foreach` en Blade

### ♻️ Paso 4: Crear un componente Blade reutilizable

#### 4.1 Crear archivo: `resources/views/components/tarjeta.blade.php`

```php+HTML
<div style="border: 1px solid #ccc; padding: 10px; margin: 10px 0; background: white;">
    <h3>{{ $titulo }}</h3>
    <div>{{ $slot }}</div>
</div>
```

#### 4.2 Usar componente en `about.blade.php` o cualquier otra vista:

```php+HTML
<x-tarjeta titulo="¿Por qué Laravel?">
    Laravel permite construir aplicaciones robustas con código limpio y sintético.
</x-tarjeta>

<x-tarjeta titulo="Habilidades">
    Experiencia en PHP, Laravel, Livewire, y bases de datos como MySQL y PostgreSQL.
</x-tarjeta>
```

🧠 Este patrón estimula el diseño DRY (*Don’t Repeat Yourself*) y enseña a abstraer elementos visuales.

### 🧪 Paso 5: Actividad de reflexión práctica

💬 **Preguntas para el estudiante**:

1. ¿Qué ventajas encuentras al tener múltiples layouts?
2. ¿Cuándo sería útil usar un componente Blade en un proyecto real?
3. ¿Cómo reutilizarías el layout `admin` para otros módulos administrativos?

### 🏁 Resultado esperado

El estudiante habrá creado:

- 1 nueva vista informativa (`/sobre-mi`)
- 1 layout personalizado para administración
- 1 sección de usuarios administrada desde un layout independiente
- 1 componente Blade reutilizable para mostrar contenido dinámico





