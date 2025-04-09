# 🧠 **¡Visualízalo con Blade!** – Tu primera capa de presentación en Laravel 🧩

## 🎯 **Objetivos de aprendizaje**

Al finalizar esta actividad, el estudiante será capaz de:

- ✅ Comprender la arquitectura de vistas en Laravel.
- 🧰 Utilizar Blade como motor de plantillas para crear interfaces dinámicas y limpias.
- 🧩 Implementar layouts reutilizables con Blade.
- 🔄 Integrar datos desde el controlador a la vista.
- 🚀 Configurar y lanzar un proyecto Laravel básico con vistas estructuradas profesionalmente.

## 📘 **Conceptos prácticos: Laravel y Blade**

Laravel utiliza el motor de plantillas **Blade** para crear vistas de forma elegante y eficiente. Las vistas en Laravel son archivos `.blade.php` ubicados en la carpeta `resources/views`.

### 🧩 ¿Qué es Blade?

Blade permite escribir lógica directamente en las vistas sin romper la estética del HTML. Ejemplos comunes incluyen:

```css
{{-- Mostrar una variable --}}
<h1>Hola, {{ $nombre }}</h1>

{{-- Condicionales --}}
@if($edad >= 18)
    <p>Eres mayor de edad</p>
@else
    <p>Aún eres menor de edad</p>
@endif

{{-- Bucles --}}
@foreach($productos as $producto)
    <li>{{ $producto->nombre }}</li>
@endforeach
```

### 🧱 Layouts y secciones

Permiten crear una estructura base para múltiples páginas:

**layout.blade.php**

```html
<!DOCTYPE html>
<html>
<head>
    <title>@yield('titulo')</title>
</head>
<body>
    <div class="container">
        @yield('contenido')
    </div>
</body>
</html>
```

**inicio.blade.php**

```html
@extends('layout')

@section('titulo', 'Inicio')

@section('contenido')
    <h1>Bienvenido a Laravel con Blade 🧡</h1>
@endsection
```

## 🧼 **Tips para un código limpio y eficiente**

- 🗂️ **Organiza tus vistas**: Usa subcarpetas dentro de `resources/views` para agrupar módulos, como `admin/dashboard.blade.php`.
- 🔁 **Evita la repetición**: Usa `@include` y layouts base para evitar duplicar HTML.
- 🧠 **Nombra inteligentemente**: Usa nombres representativos como `productos.index` o `usuarios.create`.
- 🚦 **Mantén lógica fuera de la vista**: Solo visualiza datos, la lógica va en el controlador o en el ViewModel.
- 💡 **Aplica componentes Blade**: Reutiliza pedazos de UI como botones o tarjetas.