# 🧩 Implementación Técnica: Proyecto Tienda Online

## 📦 Crear y Consumir Controladores RESTful en Laravel 12

## 🎯 ¿Qué es un Controlador RESTful en Laravel?

Un **controlador RESTful** es una clase que agrupa toda la lógica relacionada con un recurso (producto, categoría, etc.), dividiendo las operaciones según el verbo HTTP:

| Verbo HTTP | Método Laravel | Propósito                   |
| ---------- | -------------- | --------------------------- |
| GET        | `index()`      | Mostrar listado             |
| GET        | `create()`     | Mostrar formulario creación |
| POST       | `store()`      | Guardar recurso nuevo       |
| GET        | `show()`       | Ver detalle individual      |
| GET        | `edit()`       | Mostrar formulario edición  |
| PUT/PATCH  | `update()`     | Actualizar recurso          |
| DELETE     | `destroy()`    | Eliminar recurso            |

## ⚙️ Paso a Paso: Crear Controladores RESTful

### 1️⃣ Crear controlador para `Producto`

```css
php artisan make:controller ProductoController --resource
```

📁 Esto genera el archivo:

```css
app/Http/Controllers/ProductoController.php
```

🔁 Laravel creará los 7 métodos RESTful vacíos. Ejemplo inicial:

```php
class ProductoController extends Controller
{
    public function index() {
        //
    }

    public function create() {
        //
    }

    public function store(Request $request) {
        //
    }

    public function show(string $id) {
        //
    }

    public function edit(string $id) {
        //
    }

    public function update(Request $request, string $id) {
        //
    }

    public function destroy(string $id) {
        //
    }
}
```

### 2️⃣ Registrar la ruta del recurso en `routes/web.php`

```css
use App\Http\Controllers\ProductoController;

Route::resource('productos', ProductoController::class);
```

Esto automáticamente crea rutas como:

| Método | Ruta                 | Acción  | Nombre            |
| ------ | -------------------- | ------- | ----------------- |
| GET    | /productos           | index   | productos.index   |
| GET    | /productos/create    | create  | productos.create  |
| POST   | /productos           | store   | productos.store   |
| GET    | /productos/{id}      | show    | productos.show    |
| GET    | /productos/{id}/edit | edit    | productos.edit    |
| PUT    | /productos/{id}      | update  | productos.update  |
| DELETE | /productos/{id}      | destroy | productos.destroy |

## 🛠️ Detalle de cada método con explicación y ejemplo

### 🛠️ App\Http\Controllers\ProductoController

### ✅ `index()`: Mostrar todos los productos

```php
// recuerda agregar lo que hace falta 
namespace App\Http\Controllers;
use Illuminate\Http\Request;
use App\Http\Controllers\ProductoController;
use App\Models\Producto;
use App\Models\Categoria;


public function index()
{
    $productos = Producto::with('categoria')->paginate(10);
    return view('productos.index', compact('productos'));
}
```

> 🎓 *Concepto aprendido:* Uso de Eloquent para traer datos con relaciones y paginación.

### ✅ `create()`: Mostrar formulario de creación

```php
public function create()
{
    $categorias = Categoria::all();
    return view('productos.create', compact('categorias'));
}
```

> 🧠 Se pasa la lista de categorías para llenar un `<select>` en el formulario.

### ✅ `store()`: Guardar nuevo producto

```php
public function store(Request $request)
{
    $request->validate([
        'nombre' => 'required|string|max:100',
        'descripcion' => 'required',
        'precio' => 'required|numeric',
        'stock' => 'required|integer',
        'categoria_id' => 'required|exists:categorias,id'
    ]);

    Producto::create($request->all());

    return redirect()->route('productos.index')
                     ->with('success', 'Producto creado exitosamente.');
}
```

📌 *Tip de seguridad:* Usa `$fillable` en el modelo `Producto` para evitar vulnerabilidades de asignación masiva.

### ✅ `edit($id)`: Mostrar formulario de edición

```php
public function edit($id)
{
    $producto = Producto::findOrFail($id);
    $categorias = Categoria::all();
    return view('productos.edit', compact('producto', 'categorias'));
}
```

> 🎯 Permite al usuario actualizar datos manteniendo la relación con su categoría.

### ✅ `update(Request $request, $id)`: Guardar edición

```php
public function update(Request $request, string $id)
{
            $request->validate([
            'nombre' => 'required|string|max:100',
            'descripcion' => 'required',
            'precio' => 'required|numeric',
            'stock' => 'required|integer',
            'categoria_id' => 'required|exists:categorias,id'
        ]);
    
        $producto = Producto::findOrFail($id);
        $producto->update($request->all());
        return redirect()->route('productos.index')
                        ->with('success', 'Producto actualizado exitosamente.');
}
```

### ✅ `destroy($id)`: Eliminar producto

```php
public function destroy($id)
{
    $producto = Producto::findOrFail($id);
    $producto->delete();

    return redirect()->route('productos.index')
                     ->with('success', 'Producto eliminado correctamente.');
}
```

> ⚠️ *Pro tip:* Laravel usa `softDeletes()` si quieres permitir recuperación futura.

### ✅ `show($id)`: Presentar producto

```php
public function show(string $id)
    {
        $producto = Producto::with('categoria')->findOrFail($id);
        return view('productos.show', compact('producto'));
    }
```

## 🧠 Para cada controlador (Producto, Categoría, Pedido, etc.):

```css
php artisan make:controller CategoriaController --resource
php artisan make:controller PedidoController --resource
php artisan make:controller CarritoController --resource
```

✍️ Cada uno implementará una lógica similar, reutilizando plantillas y buenas prácticas. Así, los estudiantes asimilan el patrón RESTful como modelo mental y práctico.

### 🛠️ App\Http\Controllers\CategoriaController

```php+HTML
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use App\Models\Producto;
use App\Models\Categoria;


class CategoriaController extends Controller
{
    /**
     * Display a listing of the resource.
     */
    public function index()
    {
        //
        //$categorias = Categoria::all();
        $categorias = Categoria::paginate(10);
        return view('categorias.index', compact('categorias'));

    }

    /**
     * Show the form for creating a new resource.
     */
    public function create()
    {
        //
        return view('categorias.create');
    }

    /**
     * Store a newly created resource in storage.
     */
    public function store(Request $request)
    {
        //
        $request->validate([
            'nombre' => 'required|string|max:255',
            'descripcion' => 'nullable|string|max:255',
        ]);

        Categoria::create($request->all());

        return redirect()->route('categorias.index')
                         ->with('success', 'Categoría creada correctamente.');


    }

    /**
     * Display the specified resource.
     */
    public function show(Categoria $categoria)
    {
        return view('categorias.show', compact('categoria'));
    }
    /**
     * Show the form for editing the specified resource.
     */
    public function edit(Categoria $categoria)
    {
        return view('categorias.edit', compact('categoria'));
    }

    /**
     * Update the specified resource in storage.
     */
    public function update(Request $request, Categoria $categoria)
    {
        $request->validate([
            'nombre' => 'required|string|max:100',
            'descripcion' => 'nullable|string|max:255',
        ]);

        $categoria->update($request->all());

        return redirect()->route('categorias.index')
                         ->with('success', 'Categoría actualizada correctamente.');
    }

    /**
     * Remove the specified resource from storage.
     */
    public function destroy(Categoria $categoria)
    {
        $categoria->delete();

        return redirect()->route('categorias.index')
                         ->with('success', 'Categoría eliminada.');
    }

    
}
```











### 🎓 Conclusión

Crear controladores RESTful en Laravel 12 permite mantener tu código limpio, modular y fácil de mantener. A través del patrón MVC y los verbos HTTP, se estructura la lógica del proyecto de manera intuitiva y profesional.