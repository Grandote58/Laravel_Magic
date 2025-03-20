![logo](https://github.com/Grandote58/Laravel_Magic/blob/main/Img/LogoGR58_1.png)

# 🏢 **Actividad Práctica: Configuración y Creación del Primer Proyecto en Laravel 12 con XAMPP y Visual Studio Code**

### **📌 Contexto Empresarial**

La startup **TechCorp** busca crear un **portal interno para empleados**, donde puedan acceder a noticias, documentos y recursos. Como desarrollador web, te han asignado la tarea de **configurar Laravel 12 en XAMPP, realizar la instalación de Git, Composer y Visual Studio Code, y construir una página de bienvenida funcional**.

## 📌 Herramientas Requeridas:

 ✅ **XAMPP** (para el servidor Apache y MySQL).

 ✅ **Git** (para control de versiones).

 ✅ **Composer** (para gestionar paquetes PHP).

 ✅ **Visual Studio Code (VS Code)** (como editor de código).



## 🛠️ **Paso a Paso de la Actividad**

## ✅ **Paso 1: Instalación y Configuración del Entorno**

### **🔸 1. Instalación de Git en Windows**

Git es necesario para gestionar versiones y clonar repositorios.

1️⃣ Descarga Git desde [git-scm.com](https://git-scm.com/downloads).

2️⃣ Ejecuta el instalador y sigue los pasos.

3️⃣ Durante la instalación, selecciona la opción:

- "Use Git from the Windows Command Prompt".

  

   4️⃣ Verifica que Git está instalado correctamente:

  

```shell
git --version
```

Debería mostrar la versión instalada.



### **🔸 2. Agregar el Path de PHP en las Variables de Entorno de Windows**



Para ejecutar Laravel correctamente, PHP debe estar en el **PATH del sistema**.



1️⃣ Abre el **Explorador de Archivos** y navega a la carpeta donde está instalado XAMPP.
 **Ruta usual:** `C:\xampp\php`

2️⃣ Copia la ruta completa de la carpeta `php`.

3️⃣ En Windows, abre **"Configuración avanzada del sistema"** → **"Variables de Entorno"**.

4️⃣ En **Variables del sistema**, busca la variable `Path` y selecciona **Editar**.

5️⃣ Haz clic en **Nuevo** y pega la ruta `C:\xampp\php`.

6️⃣ Guarda los cambios y reinicia tu computadora.

7️⃣ Verifica que PHP está en el PATH ejecutando en CMD:



```shell
php -v
```

Debería mostrar la versión de PHP instalada.

### **🔸 3. Habilitar la Extensión ZIP en `php.ini`**

Laravel necesita la extensión ZIP habilitada en PHP para algunas funcionalidades.

1️⃣ Abre el archivo **`php.ini`** en `C:\xampp\php\php.ini` con un editor de texto.

2️⃣ Busca la línea:

```shell
;extension=zip
```

3️⃣ **Elimina el `;` (punto y coma) para habilitar la extensión**:

```shell
extension=zip
```

4️⃣ Guarda los cambios y reinicia Apache en el Panel de Control de XAMPP.



## ✅ **Paso 2: Instalación de Laravel 12 en XAMPP**

### **🔸 1. Abrir la Terminal y Acceder a la Carpeta `htdocs` de XAMPP**

```shell
cd C:\xampp\htdocs
```

### **🔸 2. Crear un Nuevo Proyecto Laravel 12**

Ejecuta el siguiente comando para instalar Laravel en XAMPP:

```shell
composer create-project --prefer-dist laravel/laravel TechCorpSystem
```

### **🔸 3. Configurar la Base de Datos en phpMyAdmin**



1️⃣ Abre **XAMPP** y accede a phpMyAdmin (`http://localhost/phpmyadmin`).

2️⃣ Crea una nueva base de datos llamada `techcorp_db`.

### **🔸 4. Configurar Laravel para Conectar la Base de Datos**



Abre el archivo `.env` en VS Code y edita los siguientes valores:

```php
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=techcorp_db
DB_USERNAME=root
DB_PASSWORD=
```



## ✅ **Paso 3: Realizar la Migración de la Base de Datos**



Laravel permite definir la estructura de la base de datos con **migraciones**.

### **🔸 1. Crear Migraciones para una Tabla de Empleados**

Ejecuta en la terminal:

```
php artisan make:migration create_empleados_table
```

Esto generará un archivo en `database/migrations/`.

### **🔸 2. Editar la Migración para Definir la Tabla**

Abre el archivo recién creado en `database/migrations/` y modifica el método `up()`:

```php
use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration {
    public function up()
    {
        Schema::create('empleados', function (Blueprint $table) {
            $table->id();
            $table->string('nombre');
            $table->string('email')->unique();
            $table->string('cargo');
            $table->timestamps();
        });
    }

    public function down()
    {
        Schema::dropIfExists('empleados');
    }
};
```

### **🔸 3. Ejecutar la Migración**

```shell
php artisan migrate
```

Esto creará la tabla `empleados` en `techcorp_db`.

------

## ✅ **Paso 4: Ejecutar Laravel y Verificar que Funciona**

1️⃣ **Levantar el servidor de Laravel:**

```shell
php artisan serve
```

2️⃣ **Abrir el navegador en `http://127.0.0.1:8000/`** para ver la página de inicio.

------

## ✅ **Paso 5: Crear la Página de Bienvenida Personalizada**

### **🔸 1. Crear una Ruta para la Página de Inicio**

Edita `routes/web.php` para modificar la ruta principal:

```php
use Illuminate\Support\Facades\Route;

Route::get('/', function () {
    return view('welcome', ['empresa' => 'TechCorp']);
});
```

### **🔸 2. Personalizar la Vista `welcome.blade.php`**

Edita el archivo `resources/views/welcome.blade.php`:

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bienvenido a TechCorp</title>
</head>
<body>
    <h1>¡Bienvenido a {{ $empresa }}!</h1>
    <p>Estamos desarrollando una plataforma innovadora para nuestros empleados.</p>
</body>
</html>
```

### **🔸 3. Comprobar los Cambios en el Navegador**

Abre `http://127.0.0.1:8000/` y verifica la página personalizada.

🎯 **¡Felicidades!** Has instalado y configurado Laravel 12 en XAMPP con éxito.

# 📌 **Conclusión**

Esta actividad permitió:

✅ **Instalar Git** y configurarlo en Windows.

✅ **Agregar el Path de PHP en las variables de entorno** para poder ejecutarlo desde cualquier terminal.

✅ **Habilitar la extensión ZIP en `php.ini`**, requerida por Laravel.

✅ **Instalar Laravel 12 en XAMPP** y configurar su base de datos en phpMyAdmin.

✅ **Ejecutar la migración de la base de datos**, creando la tabla `empleados`.

✅ **Personalizar la página de bienvenida** con Blade.



Este conocimiento será la base para profundizar en **rutas y controladores en la próxima semana**. 🚀

------

## 📚 **Bibliografía 2024**

1. **Laravel 12 Official Documentation** – https://laravel.com/docs/12.x

   

2. Stauffer, M. (2024). *Laravel: Up & Running* (3rd Ed.). O'Reilly Media.

   

3. Redmond, P. (2024). *REST API Development with Laravel*. Leanpub.

   

4. Martin, R. C. (2024). *Clean Code: A Handbook of Agile Software Craftsmanship*. Pearson.