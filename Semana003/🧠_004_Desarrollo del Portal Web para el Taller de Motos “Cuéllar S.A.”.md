# 🧠 **Desarrollo del Portal Web para el Taller de Motos “Cuéllar S.A.”**

### 🏍️ **Contexto empresarial (simulado)**

**Cuéllar S.A.** es un reconocido taller de motocicletas que ofrece servicios de mantenimiento, reparación y personalización de motos. El dueño desea un **sitio web informativo** que muestre los servicios, mecánicos disponibles, casos de motos restauradas, y un formulario de contacto para agendar citas.

Tu misión como desarrollador Laravel es construir una solución escalable con **Blade, Bootstrap 5 y estructura profesional**, ideal para que el sitio pueda crecer más adelante.

## 🎯 **Objetivos de aprendizaje**

- ⚙️ Instalar y configurar un proyecto Laravel 12 desde cero.
- 🧩 Aplicar layouts, componentes y estructuras Blade de forma profesional.
- 💡 Diseñar interfaces responsivas con Bootstrap 5.
- 🛠️ Comprender cómo Laravel estructura las vistas, datos y rutas.
- 🎨 Incorporar buenas prácticas de desarrollo limpio y ordenado.

## 🛠️ **Parte 1: Instalación del Proyecto**

### 📋 Requisitos

- PHP 8.2+
- Composer
- Laravel Herd / Valet / XAMPP
- Navegador moderno

### 📦 Instalación del proyecto

```bash
composer create-project laravel/laravel cuellar-workshop
cd cuellar-workshop
php artisan serve
```

Accede a http://localhost:8000 para confirmar que todo funcione ✅

## 🗂️ **Parte 2: Organización de Carpetas Blade**

```css
resources/views/
├── layouts/
│   └── main.blade.php
├── pages/
│   ├── home.blade.php
│   ├── servicios.blade.php
│   ├── mecanicos.blade.php
│   ├── restauraciones.blade.php
│   └── contacto.blade.php
└── components/
    ├── navbar.blade.php
    └── card-servicio.blade.php
```

## 🎨 **Parte 3: Layout Principal con Bootstrap**

### 📄 `layouts/main.blade.php`

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>Cuéllar S.A. - @yield('titulo')</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
</head>
<body>
    <x-navbar />

    <div class="container mt-4">
        @yield('contenido')
    </div>

    <footer class="text-center bg-dark text-white mt-5 p-4">
        &copy; {{ date('Y') }} Cuéllar S.A. - Taller de Motos 🛠️
    </footer>
</body>
</html>
```

## 🧩 **Navbar Reutilizable**

### 📄 `components/navbar.blade.php`

```css
<nav class="navbar navbar-expand-lg navbar-dark bg-dark">
    <div class="container-fluid">
        <a class="navbar-brand" href="/">Cuéllar S.A.</a>
        <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navContent">
            <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navContent">
            <ul class="navbar-nav me-auto">
                <li class="nav-item"><a class="nav-link" href="/">Inicio</a></li>
                <li class="nav-item"><a class="nav-link" href="/servicios">Servicios</a></li>
                <li class="nav-item"><a class="nav-link" href="/mecanicos">Mecánicos</a></li>
                <li class="nav-item"><a class="nav-link" href="/restauraciones">Restauraciones</a></li>
                <li class="nav-item"><a class="nav-link" href="/contacto">Contacto</a></li>
            </ul>
        </div>
    </div>
</nav>
```

## 📄 **Vistas de Página con Bootstrap**

### 🏠 `pages/home.blade.php`

```css
@extends('layouts.main')

@section('titulo', 'Inicio')

@section('contenido')
    <div class="text-center">
        <h1>Bienvenido a Cuéllar S.A. 🏍️</h1>
        <p class="lead">Expertos en reparación, mantenimiento y personalización de motocicletas.</p>
        <a href="/contacto" class="btn btn-primary">Agendar Cita</a>
    </div>
@endsection
```

### 🔧 `pages/servicios.blade.php`

```css
@extends('layouts.main')

@section('titulo', 'Servicios')

@section('contenido')
    <h2 class="mb-4">Servicios Destacados</h2>
    <div class="row">
        @foreach($servicios as $servicio)
            <div class="col-md-4">
                <x-card-servicio :titulo="$servicio['titulo']" :descripcion="$servicio['descripcion']" />
            </div>
        @endforeach
    </div>
@endsection
```

### 📄 `components/card-servicio.blade.php`

```css
<div class="card border-secondary mb-3">
    <div class="card-header bg-dark text-white">{{ $titulo }}</div>
    <div class="card-body">
        <p class="card-text">{{ $descripcion }}</p>
    </div>
</div>
```

### 🧑‍🔧 `pages/mecanicos.blade.php`

```css
@extends('layouts.main')

@section('titulo', 'Nuestro Equipo')

@section('contenido')
    <h2>Mecánicos Profesionales 👨‍🔧</h2>
    <ul class="list-group">
        <li class="list-group-item">Carlos Cuéllar – Jefe de Taller</li>
        <li class="list-group-item">Laura Martínez – Especialista en Motores</li>
        <li class="list-group-item">Rafael López – Técnico en Suspensión</li>
    </ul>
@endsection
```

### 🛠️ `pages/restauraciones.blade.php`

```css
@extends('layouts.main')

@section('titulo', 'Restauraciones')

@section('contenido')
    <h2>Casos de Éxito</h2>
    <p>Transformamos motos clásicas en verdaderas joyas mecánicas. ✨</p>
    <img src="https://via.placeholder.com/800x400" class="img-fluid my-3" alt="Restauración">
    <p>Honda CB750 restaurada por completo en 3 semanas.</p>
@endsection
```

### 📬 `pages/contacto.blade.php`

```css
@extends('layouts.main')

@section('titulo', 'Contacto')

@section('contenido')
    <h2>Agendar Cita 📅</h2>
    <form>
        <div class="mb-3">
            <label class="form-label">Nombre</label>
            <input type="text" class="form-control" placeholder="Juan Pérez">
        </div>
        <div class="mb-3">
            <label class="form-label">Correo Electrónico</label>
            <input type="email" class="form-control" placeholder="correo@ejemplo.com">
        </div>
        <div class="mb-3">
            <label class="form-label">Motivo</label>
            <textarea class="form-control" rows="4" placeholder="Describe tu problema o necesidad..."></textarea>
        </div>
        <button class="btn btn-dark">Enviar</button>
    </form>
@endsection
```

## 🗺️ **Parte Final: Rutas**

### 📄 `routes/web.php`

```css
use Illuminate\Support\Facades\Route;

Route::get('/', fn() => view('pages.home'));

Route::get('/servicios', function () {
    $servicios = [
        ['titulo' => 'Cambio de Aceite', 'descripcion' => 'Incluye filtro y revisión general.'],
        ['titulo' => 'Reparación de Motor', 'descripcion' => 'Diagnóstico y solución completa.'],
        ['titulo' => 'Restauración Clásica', 'descripcion' => 'Deja tu moto como nueva.']
    ];
    return view('pages.servicios', compact('servicios'));
});

Route::get('/mecanicos', fn() => view('pages.mecanicos'));
Route::get('/restauraciones', fn() => view('pages.restauraciones'));
Route::get('/contacto', fn() => view('pages.contacto'));
```

## 💡 **Tips para entender mejor Laravel 12 y Blade**

- 🧩 **Piensa en bloques**: Blade te permite dividir tu sitio en piezas reutilizables (layouts, componentes). Esto ahorra tiempo y mejora el mantenimiento.
- 🧠 **El layout es tu esqueleto**: todo lo que repites (navbar, footer, estilos) va allí.
- 🛠️ **El slot y las props** en componentes Blade te permiten personalizar cada instancia.
- 🔁 **El `@foreach`** es clave para mostrar múltiples elementos (servicios, productos, etc.).
- 📦 **Usa `compact()`** para enviar variables a tus vistas de forma clara.

## 🧪 **Ejercicio Final**

- Crea un nuevo componente `x-alerta` que reciba un mensaje y lo muestre con una clase Bootstrap.
- Agrega una sección "Clientes Satisfechos" con testimonios dinámicos.