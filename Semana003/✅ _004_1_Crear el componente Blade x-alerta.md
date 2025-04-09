💪 Te voy a mostrar cómo resolver **el ejercicio final** propuesto, que consiste en:

1. Crear un componente Blade llamado `x-alerta` que reciba un mensaje.
2. Agregar una nueva sección **Clientes Satisfechos**, que muestre testimonios dinámicos usando Bootstrap.

Todo esto sigue la estructura profesional con Laravel 12 y Blade.



## ✅ **1. Crear el componente Blade `x-alerta`**

### 📁 Ubicación:

```bash
resources/views/components/alerta.blade.php
```

### 📄 Código:

```css
<div class="alert alert-{{ $tipo ?? 'info' }} alert-dismissible fade show" role="alert">
    {{ $mensaje }}
    <button type="button" class="btn-close" data-bs-dismiss="alert" aria-label="Cerrar"></button>
</div>
```

### 💡 Cómo usarlo en una vista:

```css
<x-alerta tipo="success" mensaje="¡Cita registrada exitosamente!" />
<x-alerta tipo="danger" mensaje="Error al enviar el formulario. Revisa los campos." />
```

### 🎨 Tipos disponibles de Bootstrap:

- `primary`, `success`, `danger`, `warning`, `info`, `secondary`, etc.

## ✅ **2. Sección: Clientes Satisfechos**

### 📁 Vista: `pages/clientes.blade.php`

```css
@extends('layouts.main')

@section('titulo', 'Clientes Satisfechos')

@section('contenido')
    <h2 class="mb-4">Clientes Satisfechos 👏</h2>

    @foreach($testimonios as $cliente)
        <div class="card mb-3">
            <div class="card-header bg-success text-white">
                {{ $cliente['nombre'] }}
            </div>
            <div class="card-body">
                <blockquote class="blockquote mb-0">
                    <p>"{{ $cliente['comentario'] }}"</p>
                    <footer class="blockquote-footer">Servicio: <cite>{{ $cliente['servicio'] }}</cite></footer>
                </blockquote>
            </div>
        </div>
    @endforeach
@endsection
```

------

## ✅ **Ruta correspondiente en `web.php`**

```php
Route::get('/clientes', function () {
    $testimonios = [
        [
            'nombre' => 'Luis Hernández',
            'comentario' => 'Excelente atención y rapidez. Mi moto quedó como nueva.',
            'servicio' => 'Reparación de Motor'
        ],
        [
            'nombre' => 'Sandra Torres',
            'comentario' => 'La restauración superó mis expectativas. ¡Gracias Cuéllar!',
            'servicio' => 'Restauración Clásica'
        ],
        [
            'nombre' => 'Erick Jiménez',
            'comentario' => 'Atención al cliente inmejorable. Muy recomendable.',
            'servicio' => 'Mantenimiento General'
        ]
    ];
    return view('pages.clientes', compact('testimonios'));
});
```

## 📍 **Tip adicional**

➡️ Si quieres mostrar una alerta **sólo si existe una condición**, puedes usar:

```css
@if(session('mensaje'))
    <x-alerta tipo="success" :mensaje="session('mensaje')" />
@endif
```

Esto es útil luego de enviar formularios.