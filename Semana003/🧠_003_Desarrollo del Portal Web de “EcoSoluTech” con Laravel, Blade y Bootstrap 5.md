# 🧠 **Desarrollo del Portal Web de “EcoSoluTech” con Laravel, Blade y Bootstrap 5**

### 🏢 **Escenario empresarial (simulado)**

**EcoSoluTech** es una empresa emergente especializada en soluciones ecológicas para hogares y oficinas. Busca lanzar su primera **plataforma web informativa**, donde se destaquen sus servicios ecológicos, el equipo de trabajo, casos de éxito y un formulario de contacto.

Has sido contratado como **desarrollador Laravel Junior** para crear el prototipo funcional, usando Laravel 12, Blade y Bootstrap.

## 🎯 **Objetivos de aprendizaje**

- 📦 Instalar y configurar un proyecto Laravel 12 con estructura profesional.
- 🧱 Crear y aplicar layouts y componentes Blade personalizados.
- 🎨 Diseñar interfaces elegantes y responsivas con Bootstrap 5.
- 🔁 Integrar datos dinámicos en las vistas de forma estructurada.
- ✍️ Crear una experiencia realista de desarrollo profesional.

## 🛠️ **Parte 1: Instalación del Proyecto**

### 🧪 Requisitos previos

- PHP ≥ 8.2
- Composer
- Laravel Herd / Valet / XAMPP
- Navegador moderno

### 📦 Comandos para instalación

```bash
composer create-project laravel/laravel ecosolutech
cd ecosolutech
php artisan serve
```

Asegúrate de que Laravel funcione en:

 http://localhost:8000 🚀

## 📁 **Parte 2: Estructura de carpetas de vistas**

```css
resources/views/
├── layouts/
│   └── main.blade.php
├── pages/
│   ├── home.blade.php
│   ├── servicios.blade.php
│   ├── equipo.blade.php
│   ├── casos.blade.php
│   └── contacto.blade.php
└── components/
    ├── tarjeta.blade.php
    └── navbar.blade.php
```

## 🌐 **Parte 3: Agregar Bootstrap al layout principal**

### 🧱 `layouts/main.blade.php`

```css
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>EcoSoluTech - @yield('titulo')</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
</head>
<body>
    <x-navbar />

    <div class="container mt-4">
        @yield('contenido')
    </div>

    <footer class="text-center mt-5 p-4 bg-light">
        &copy; {{ date('Y') }} EcoSoluTech 🌿 - Todos los derechos reservados
    </footer>
</body>
</html>
```

## 🧩 **Componente Navbar Bootstrap**

### `components/navbar.blade.php`

```html
<nav class="navbar navbar-expand-lg navbar-dark bg-success">
    <div class="container-fluid">
        <a class="navbar-brand" href="/">EcoSoluTech</a>
        <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navContent">
            <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navContent">
            <ul class="navbar-nav me-auto mb-2 mb-lg-0">
                <li class="nav-item"><a class="nav-link" href="/">Inicio</a></li>
                <li class="nav-item"><a class="nav-link" href="/servicios">Servicios</a></li>
                <li class="nav-item"><a class="nav-link" href="/equipo">Equipo</a></li>
                <li class="nav-item"><a class="nav-link" href="/casos">Casos de Éxito</a></li>
                <li class="nav-item"><a class="nav-link" href="/contacto">Contacto</a></li>
            </ul>
        </div>
    </div>
</nav>
```

## 📄 **Parte 4: Crear las vistas con contenido dinámico y diseño Bootstrap**

### 🏠 `pages/home.blade.php`

```css
@extends('layouts.main')

@section('titulo', 'Inicio')

@section('contenido')
    <div class="jumbotron text-center">
        <h1 class="display-4">Bienvenido a EcoSoluTech 🌿</h1>
        <p class="lead">Líderes en soluciones ecológicas y sostenibles para hogares y empresas.</p>
    </div>
@endsection
```

### 🛠️ `pages/servicios.blade.php`

```css
@extends('layouts.main')

@section('titulo', 'Servicios')

@section('contenido')
    <h2 class="mb-4">Nuestros Servicios</h2>
    <div class="row">
        @foreach($servicios as $servicio)
            <div class="col-md-4">
                <x-tarjeta :titulo="$servicio['titulo']" :descripcion="$servicio['descripcion']" />
            </div>
        @endforeach
    </div>
@endsection
```

### 📦 `components/tarjeta.blade.php`

```css
<div class="card mb-4 shadow-sm">
    <div class="card-body">
        <h5 class="card-title">{{ $titulo }}</h5>
        <p class="card-text">{{ $descripcion }}</p>
    </div>
</div>
```

### 👥 `pages/equipo.blade.php`

```css
@extends('layouts.main')

@section('titulo', 'Equipo')

@section('contenido')
    <h2>Nuestro Equipo 💼</h2>
    <ul class="list-group">
        <li class="list-group-item">Lucía Martínez – CEO</li>
        <li class="list-group-item">David Álvarez – Especialista en Energía Solar</li>
        <li class="list-group-item">Karen Orozco – Ingeniera Ambiental</li>
    </ul>
@endsection
```

------

### 📈 `pages/casos.blade.php`

```css
@extends('layouts.main')

@section('titulo', 'Casos de Éxito')

@section('contenido')
    <h2>Casos de Éxito ⭐</h2>
    <p>Más de 100 empresas han implementado nuestras soluciones con resultados sobresalientes.</p>
    <p>Ejemplo: <strong>GreenOffice S.A.</strong> redujo su huella de carbono en un 40% tras instalar paneles solares y sistemas de ahorro de agua.</p>
@endsection
```

### 📬 `pages/contacto.blade.php`

```css
@extends('layouts.main')

@section('titulo', 'Contacto')

@section('contenido')
    <h2>Contáctanos 📞</h2>
    <form>
        <div class="mb-3">
            <label class="form-label">Nombre</label>
            <input type="text" class="form-control" placeholder="Tu nombre completo">
        </div>
        <div class="mb-3">
            <label class="form-label">Correo electrónico</label>
            <input type="email" class="form-control" placeholder="correo@ejemplo.com">
        </div>
        <div class="mb-3">
            <label class="form-label">Mensaje</label>
            <textarea class="form-control" rows="4" placeholder="Escribe tu mensaje..."></textarea>
        </div>
        <button class="btn btn-success">Enviar</button>
    </form>
@endsection
```

## 🧭 **Parte 5: Definición de rutas**

### `routes/web.php`

```css
use Illuminate\Support\Facades\Route;

Route::get('/', fn() => view('pages.home'));

Route::get('/servicios', function () {
    $servicios = [
        ['titulo' => 'Paneles Solares', 'descripcion' => 'Energía limpia y renovable.'],
        ['titulo' => 'Gestión de Residuos', 'descripcion' => 'Programas de reciclaje eficientes.'],
        ['titulo' => 'Consultoría Ambiental', 'descripcion' => 'Diagnósticos ecológicos y estrategias sostenibles.']
    ];
    return view('pages.servicios', compact('servicios'));
});

Route::get('/equipo', fn() => view('pages.equipo'));
Route::get('/casos', fn() => view('pages.casos'));
Route::get('/contacto', fn() => view('pages.contacto'));
```

## 🧪 **Parte 6: Evaluación y Reflexión**

📝 **Preguntas para el estudiante**:

1. ¿Qué beneficios aporta Bootstrap a la experiencia del usuario?
2. ¿Cómo mejorarías el formulario de contacto para hacerlo funcional?
3. ¿Cómo separarías este proyecto en módulos para escalarlo?