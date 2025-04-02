# **📘 Guía Práctica para Aprender Rutas y Controladores en Laravel**

## 🎯 Objetivo

Aprender a crear, entender y usar **Rutas y Controladores** en Laravel de forma práctica, usando ejemplos reales y progresivos. Al finalizar, sabrás cómo dirigir solicitudes (URLs) a la lógica correcta de tu aplicación.

## 🧠 ¿Qué son las Rutas?

En Laravel, **una ruta conecta una URL con una acción específica**, como mostrar una vista o ejecutar código.

🔎 **Ejemplo real:** Cuando escribes `https://miapp.com/contacto`, Laravel necesita saber qué hacer con esa URL. Eso lo define en `routes/web.php`.

```php
Route::get('/contacto', function () {
    return view('contacto');
});
```

## 🧱 Tipos de Rutas

1. **GET** – Para obtener información (páginas, formularios)
2. **POST** – Para enviar datos (formularios, acciones)
3. **PUT/PATCH** – Para actualizar datos
4. **DELETE** – Para eliminar datos

```php
Route::post('/enviar-formulario', [FormularioController::class, 'enviar']);
```

## 🔧 ¿Qué es un Controlador?

Un **Controlador** organiza la lógica de tu aplicación. En vez de poner todo el código en la ruta, lo delegamos a un archivo llamado *controlador*.

📁 Laravel guarda los controladores en `app/Http/Controllers`.

### 🛠 Crear un controlador

```bash
php artisan make:controller ContactoController
```

Esto crea: `app/Http/Controllers/ContactoController.php`

```php
class ContactoController extends Controller
{
    public function index() {
        return view('contacto');
    }
}
```

Y ahora la ruta se ve así:

```php
Route::get('/contacto', [ContactoController::class, 'index']);
```

## 🌎 Ejemplo de Proyecto Real

Imagina una app de blog:

| URL            | Acción                     | Método | Controlador             |
| -------------- | -------------------------- | ------ | ----------------------- |
| `/posts`       | Mostrar lista de posts     | GET    | `PostController@index`  |
| `/posts/crear` | Mostrar formulario de post | GET    | `PostController@create` |
| `/posts`       | Guardar nuevo post         | POST   | `PostController@store`  |



### 🧩 Mini Reto Práctico

1. Crea un nuevo proyecto Laravel.
2. Agrega esta ruta en `routes/web.php`:

```
Route::get('/hola', function () {
    return '¡Hola Laravel!';
});
```

1. Crea un controlador llamado `HolaController`:

```php
php artisan make:controller HolaController
```

1. Agrega un método `saludo()` que retorne una vista simple.
2. Crea la vista `resources/views/hola.blade.php`.
3. Asocia la ruta `/hola-controlador` al método `saludo()`.

## 🧪 Buenas Prácticas

✅ Usa controladores para organizar lógica de negocio

✅ Agrupa rutas relacionadas con `Route::controller` o `Route::resource`

✅ Usa nombres descriptivos para métodos: `index`, `show`, `store`, etc.

## 🎓 Conclusión

Las rutas son el punto de entrada de tu app. Los controladores son los encargados de responder con acciones. Al dominarlos, tu aplicación será **más limpia, mantenible y escalable**.