# 🧠 **Práctica Profesional: Desarrollo del Portal de “TechNova Solutions” con Laravel y Blade**

### 🏢 **Proyecto (Escenario Simulado)**

**TechNova Solutions** es una empresa tecnológica en expansión que necesita una interfaz web sencilla para presentar sus servicios, su equipo y una sección de contacto. Has sido contratado como **desarrollador Laravel** para crear un prototipo funcional del sitio corporativo.

### 🎯 **Objetivos de aprendizaje**

- 🧱 Aplicar los conocimientos de Laravel y Blade en un contexto simulado de empresa.
- 🌐 Desarrollar una estructura de vistas limpia y reutilizable usando layouts y componentes.
- 🔁 Integrar dinámicamente datos simulados en las vistas.
- 🚀 Preparar y ejecutar un proyecto funcional desde cero.

## 🛠️ **Parte 1: Preparación e Instalación del Proyecto**

### 📦 Requisitos previos

- PHP ≥ 8.2
- Composer
- Un servidor local como Laravel Herd, XAMPP, Valet o Docker

### 🧪 Instalación del proyecto Laravel

```bash
composer create-project laravel/laravel technova-portal
cd technova-portal
php artisan serve
```

Accede al sitio en: http://localhost:8000 🚀

## 🧱 **Parte 2: Estructura del Proyecto y Arquitectura de Vistas**

### 🗂️ Crea la siguiente estructura de vistas en `resources/views`:

```css
views/
├── layouts/
│   └── main.blade.php
├── pages/
│   ├── home.blade.php
│   ├── servicios.blade.php
│   ├── equipo.blade.php
│   └── contacto.blade.php
└── components/
    └── tarjeta-servicio.blade.php
```

------

## 📐 **Parte 3: Diseño del Layout Corporativo**

### 🧩 `layouts/main.blade.php`

```html
<!DOCTYPE html>
<html>
<head>
    <title>TechNova - @yield('titulo')</title>
    <link rel="stylesheet" href="https://cdn.simplecss.org/simple.css">
</head>
<body>
    <header>
        <h1>TechNova Solutions 🚀</h1>
        <nav>
            <a href="/">Inicio</a>
            <a href="/servicios">Servicios</a>
            <a href="/equipo">Nuestro Equipo</a>
            <a href="/contacto">Contacto</a>
        </nav>
    </header>
    <main>
        @yield('contenido')
    </main>
    <footer>
        <p>&copy; {{ date('Y') }} TechNova Solutions</p>
    </footer>
</body>
</html>
```

## 🧱 **Parte 4: Creación de las Vistas**

### 🏠 `pages/home.blade.php`

```css
@extends('layouts.main')

@section('titulo', 'Inicio')

@section('contenido')
    <h2>Bienvenido a TechNova</h2>
    <p>Impulsamos la innovación digital para tu empresa.</p>
@endsection
```

### 🛠️ `pages/servicios.blade.php`

```css
@extends('layouts.main')

@section('titulo', 'Servicios')

@section('contenido')
    <h2>Nuestros Servicios</h2>

    @foreach($servicios as $servicio)
        <x-tarjeta-servicio :titulo="$servicio['titulo']">
            {{ $servicio['descripcion'] }}
        </x-tarjeta-servicio>
    @endforeach
@endsection
```

## 🔧 **Parte 5: Creación del Componente Reutilizable**

### 🧩 `components/tarjeta-servicio.blade.php`

```html
<div style="border:1px solid #ccc; padding:15px; margin:10px 0; background:#f9f9f9;">
    <h3>{{ $titulo }}</h3>
    <p>{{ $slot }}</p>
</div>
```

## 👥 `pages/equipo.blade.php`

```html
@extends('layouts.main')

@section('titulo', 'Nuestro Equipo')

@section('contenido')
    <h2>Equipo de Innovación</h2>
    <ul>
        <li>Lucía Gómez - CTO</li>
        <li>Andrés Mejía - Ingeniero Backend</li>
        <li>Carla Díaz - Diseñadora UX</li>
    </ul>
@endsection
```

## 📬 `pages/contacto.blade.php`

```css
@extends('layouts.main')

@section('titulo', 'Contacto')

@section('contenido')
    <h2>Contáctanos</h2>
    <p>Correo: contacto@technova.com</p>
    <p>Teléfono: +52 55 1234 5678</p>
@endsection
```

## 🌐 **Parte 6: Definición de Rutas**

### Edita `routes/web.php`:

```php
use Illuminate\Support\Facades\Route;

Route::get('/', function () {
    return view('pages.home');
});

Route::get('/servicios', function () {
    $servicios = [
        ['titulo' => 'Desarrollo Web', 'descripcion' => 'Aplicaciones rápidas y seguras.'],
        ['titulo' => 'Consultoría TI', 'descripcion' => 'Soluciones estratégicas.'],
        ['titulo' => 'Transformación Digital', 'descripcion' => 'Digitaliza tu negocio.']
    ];
    return view('pages.servicios', compact('servicios'));
});

Route::get('/equipo', fn() => view('pages.equipo'));
Route::get('/contacto', fn() => view('pages.contacto'));
```

## 🚀 **Parte 7: Ejecución del Proyecto**

1. Asegúrate de estar en la raíz del proyecto.
2. Ejecuta el servidor:

```bash
php artisan serve
```

1. Accede a:

- **Inicio:** http://localhost:8000
- **Servicios:** http://localhost:8000/servicios
- **Equipo:** http://localhost:8000/equipo
- **Contacto:** http://localhost:8000/contacto

## 📝 **Actividad Reflexiva**

1. ¿Cómo ayuda Blade a mantener una estructura limpia y DRY?
2. ¿Qué ventajas tiene usar componentes para mostrar servicios?
3. ¿Cómo podrías agregar rutas dinámicas para mostrar un solo servicio con más detalle?