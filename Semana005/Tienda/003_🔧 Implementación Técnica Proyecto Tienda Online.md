# **🔧 Implementación Técnica: Proyecto Tienda Online**

## 🧱 Crear Modelos Eloquent y Migraciones en Laravel 12

### 🧠 ¿Qué son los modelos y las migraciones?

- **Modelo Eloquent:** Es una clase que representa una tabla en la base de datos. Se usa para consultar, insertar, actualizar o eliminar registros.
- **Migración:** Es un archivo PHP que define la estructura de una tabla (campos, tipos de datos, claves foráneas, etc.), permitiendo versionar los cambios.

## 📂 Paso a Paso para Crear Modelos y Migraciones

### 1️⃣ Crear modelo y migración para **Categoría**

📌 Una categoría agrupa productos, por ejemplo: "Electrónica", "Ropa", "Hogar".

```css
php artisan make:model Categoria -m
```

👉 Laravel creará:

- `app/Models/Categoria.php`
- `database/migrations/xxxx_xx_xx_create_categorias_table.php`

✍️ Edita la migración:

```css
Schema::create('categorias', function (Blueprint $table) {
    $table->id();
    $table->string('nombre');
    $table->text('descripcion')->nullable();
    $table->timestamps();
});
```

🧠 *Tips del profe:*

- `nullable()` permite que la descripción sea opcional.
- `timestamps()` añade `created_at` y `updated_at`.

------

### 2️⃣ Crear modelo y migración para **Producto**

📌 Un producto pertenece a una categoría, y tiene precio, stock, descripción, etc.

```css
php artisan make:model Producto -m
```

✍️ Edita la migración:

```css
Schema::create('productos', function (Blueprint $table) {
    $table->id();
    $table->string('nombre');
    $table->text('descripcion');
    $table->decimal('precio', 8, 2);
    $table->integer('stock');
    $table->string('imagen_url')->nullable();
    $table->foreignId('categoria_id')->constrained()->onDelete('cascade');
    $table->timestamps();
});
```

✅ Aquí relacionamos con `categorias` usando `foreignId(...)->constrained()`.

### 3️⃣ Crear modelo y migración para **Usuario (Laravel lo incluye por defecto)**

Laravel ya incluye el modelo `User` y su migración. Verifica que la tabla tenga campos como:

```css
$table->id();
$table->string('name');
$table->string('email')->unique();
$table->timestamp('email_verified_at')->nullable();
$table->string('password');
$table->rememberToken();
$table->timestamps();
```

Puedes agregar:

```css
$table->enum('rol', ['cliente', 'administrador'])->default('cliente');
```

### 4️⃣ Crear modelo y migración para **Carrito y DetalleCarrito**

📌 El carrito guarda los productos seleccionados por el usuario antes de pagar.

```css
php artisan make:model Carrito -m
php artisan make:model DetalleCarrito -m
```

✍️ Migración `carritos`:

```css
Schema::create('carritos', function (Blueprint $table) {
    $table->id();
    $table->foreignId('user_id')->constrained()->onDelete('cascade');
    $table->timestamp('fecha_creacion')->useCurrent();
    $table->timestamps();
});
```

✍️ Migración `detalle_carritos`:

```css
Schema::create('detalle_carritos', function (Blueprint $table) {
    $table->id();
    $table->foreignId('carrito_id')->constrained()->onDelete('cascade');
    $table->foreignId('producto_id')->constrained()->onDelete('cascade');
    $table->integer('cantidad');
    $table->decimal('precio_unitario', 8, 2);
    $table->timestamps();
});
```

### 5️⃣ Crear modelo y migración para **Pedido y DetallePedido**

📌 Pedido = compra finalizada. Guarda la info de los productos comprados.

```bash
php artisan make:model Pedido -m
php artisan make:model DetallePedido -m
```

✍️ Migración `pedidos`:

```css
Schema::create('pedidos', function (Blueprint $table) {
    $table->id();
    $table->foreignId('user_id')->constrained()->onDelete('cascade');
    $table->timestamp('fecha_pedido')->useCurrent();
    $table->enum('estado', ['pendiente', 'pagado', 'enviado', 'entregado'])->default('pendiente');
    $table->decimal('total', 10, 2);
    $table->timestamps();
});
```

✍️ Migración `detalle_pedidos`:

```css
Schema::create('detalle_pedidos', function (Blueprint $table) {
    $table->id();
    $table->foreignId('pedido_id')->constrained()->onDelete('cascade');
    $table->foreignId('producto_id')->constrained()->onDelete('cascade');
    $table->integer('cantidad');
    $table->decimal('precio_unitario', 8, 2);
    $table->timestamps();
});
```

### 6️⃣ Crear modelo y migración para **Pago y Dirección de Envío**

```css
php artisan make:model Pago -m
php artisan make:model DireccionEnvio -m
```

✍️ Migración `pagos`:

```css
Schema::create('pagos', function (Blueprint $table) {
    $table->id();
    $table->foreignId('pedido_id')->constrained()->onDelete('cascade');
    $table->enum('metodo_pago', ['tarjeta', 'pse', 'contraentrega']);
    $table->enum('estado_pago', ['pendiente', 'aprobado', 'rechazado'])->default('pendiente');
    $table->timestamp('fecha_pago')->nullable();
    $table->timestamps();
});
```

✍️ Migración `direccion_envios`:

```css
Schema::create('direccion_envios', function (Blueprint $table) {
    $table->id();
    $table->foreignId('user_id')->constrained()->onDelete('cascade');
    $table->string('direccion');
    $table->string('ciudad');
    $table->string('departamento');
    $table->string('codigo_postal');
    $table->string('telefono_contacto');
    $table->timestamps();
});
```

## ⚙️ Ejecutar las migraciones

Una vez configuradas todas las migraciones, ejecuta:

```css
php artisan migrate
```

✅ Esto creará todas las tablas automáticamente en la base de datos `tienda`.

# 🔗 Creación de Relaciones Eloquent en Laravel 12 🧠🔍

> 📚 *Referencia oficial Laravel 12:*
>  https://laravel.com/docs/12.x/eloquent-relationships

## 🛠️ 1. `User.php`

Un usuario puede tener:

- Muchos pedidos 🧾
- Muchos carritos 🛒
- Muchas direcciones de envío 🏠

```php
// app/Models/User.php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Foundation\Auth\User as Authenticatable;
use Illuminate\Notifications\Notifiable;

class User extends Authenticatable
{
    /** @use HasFactory<\Database\Factories\UserFactory> */
    use HasFactory, Notifiable;

    /**
     * The attributes that are mass assignable.
     *
     * @var list<string>
     */
    protected $fillable = [
        'name',
        'email',
        'password',
    ];

    /**
     * The attributes that should be hidden for serialization.
     *
     * @var list<string>
     */
    protected $hidden = [
        'password',
        'remember_token',
    ];

    /**
     * Get the attributes that should be cast.
     *
     * @return array<string, string>
     */
    protected function casts(): array
    {
        return [
            'email_verified_at' => 'datetime',
            'password' => 'hashed',
        ];
    }

public function pedidos() {
    return $this->hasMany(Pedido::class);
}

public function carritos() {
    return $this->hasMany(Carrito::class);
}

public function direccionEnvios() {
    return $this->hasMany(DireccionEnvio::class);
}


}
```

## 🛠️ 2. `Categoria.php`

Una categoría tiene muchos productos:

```php
// app/Models/Categoria.php

public function productos() {
    return $this->hasMany(Producto::class);
}
```

## 🛠️ 3. `Producto.php`

Un producto:

- Pertenece a una categoría
- Está en muchos detalles de carrito
- Está en muchos detalles de pedido

```php
// app/Models/Producto.php
namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class Producto extends Model
{
    //
        protected $fillable = [
        'nombre',
        'descripcion',
        'precio',
        'stock',
        'imagen_url',
        'categoria_id'
    ];

    public function categoria()
    {
        return $this->belongsTo(Categoria::class);
    }


    public function pedidos()
    {
        return $this->belongsToMany(Pedido::class)->withPivot('cantidad');
    }
}

```

## 🛠️ 4. `Carrito.php`

Un carrito:

- Pertenece a un usuario
- Tiene muchos detalles de carrito

```php
// app/Models/Carrito.php

public function usuario() {
    return $this->belongsTo(User::class, 'user_id');
}

public function detalleCarritos() {
    return $this->hasMany(DetalleCarrito::class);
}
```

## 🛠️ 5. `DetalleCarrito.php`

Cada detalle de carrito:

- Pertenece a un carrito
- Pertenece a un producto

```php
// app/Models/DetalleCarrito.php

public function carrito() {
    return $this->belongsTo(Carrito::class);
}

public function producto() {
    return $this->belongsTo(Producto::class);
}
```

## 🛠️ 6. `Pedido.php`

Un pedido:

- Pertenece a un usuario
- Tiene muchos detalles de pedido
- Tiene un pago
- Tiene una dirección de envío

```php
// app/Models/Pedido.php

<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class Pedido extends Model
{
    //
    public function carrito() {
    return $this->belongsTo(Carrito::class);
}

public function producto() {
    return $this->belongsTo(Producto::class);
}


public function usuario() {
    return $this->belongsTo(User::class, 'user_id');
}

public function detallePedidos() {
    return $this->hasMany(DetallePedido::class);
}

public function pago() {
    return $this->hasOne(Pago::class);
}

}
```

## 🛠️ 7. `DetallePedido.php`

Un detalle de pedido:

- Pertenece a un pedido
- Pertenece a un producto

```php
// app/Models/DetallePedido.php

public function pedido() {
    return $this->belongsTo(Pedido::class);
}

public function producto() {
    return $this->belongsTo(Producto::class);
}
```

## 🛠️ 8. `Pago.php`

Un pago:

- Pertenece a un pedido

```php
// app/Models/Pago.php

public function pedido() {
    return $this->belongsTo(Pedido::class);
}
```

## 🛠️ 9. `DireccionEnvio.php`

Una dirección de envío:

- Pertenece a un usuario

```php
// app/Models/DireccionEnvio.php

public function usuario() {
    return $this->belongsTo(User::class, 'user_id');
}
```

### **🔗 Modificando las Routes**

```php
// routes/web.php

use Illuminate\Http\Request;

use App\Http\Controllers\ProductoController;
use App\Http\Controllers\CategoriaController;
use Illuminate\Support\Facades\Route;
use Illuminate\Support\Facades\Auth;

Route::get('/', function () {
    return view('home'); // Puedes cambiar luego por tu propia vista de inicio
});

Route::resource('productos', ProductoController::class);
Route::resource('categorias', CategoriaController::class);
```







## 🧠 Consejos del Profe Laravel 👨‍🏫

✅ Siempre incluye la propiedad `$fillable` o `$guarded` para proteger el modelo contra asignación masiva.

✅ Define las relaciones aunque no las uses de inmediato, ¡te ahorrará tiempo después!

✅ Usa nombres descriptivos y consistentes para tus funciones.



🎉 ¡Listo! Ahora tus modelos están completamente relacionados usando Eloquent ORM. Con esto puedes navegar entre tablas fácilmente, crear formularios relacionales, y desarrollar controladores RESTful de forma elegante y eficiente.