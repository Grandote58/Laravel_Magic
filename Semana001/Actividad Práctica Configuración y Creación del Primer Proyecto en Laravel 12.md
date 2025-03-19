


![logo](https://github.com/Grandote58/Laravel_Magic/blob/main/Img/LogoGR58_1.png)




# 🏢 Actividad Práctica 

## Configuración y Creación del Primer Proyecto en Laravel 12



📌La startup **TechCorp** busca crear un **portal interno para empleados**, donde puedan acceder a noticias, documentos y recursos. 

Como desarrollador web, te han asignado la tarea de **configurar Laravel en XAMPP y desarrollar una página de bienvenida funcional**.

###### **Herramientas Requeridas:**



✅ **XAMPP** como servidor web.

✅ **Visual Studio Code (VS Code)** como editor de código.

✅ **Composer** como gestor de dependencias de PHP.

## 🛠️ **Paso a Paso de la Actividad**

### ✅ **Paso 1: Instalación y Configuración del Entorno**

#### **1. Instalar XAMPP**



1️⃣ Descarga XAMPP desde Apache Friends.

2️⃣ Instala XAMPP y habilita los módulos **Apache y MySQL** en el Panel de Control.

3️⃣ Abre tu navegador y verifica que Apache funciona visitando `http://localhost/`.



#### **2. Instalar Composer (Administrador de Dependencias de PHP)**



Laravel requiere Composer para manejar sus paquetes.

 1️⃣ Descarga Composer desde [getcomposer.org](https://getcomposer.org/).

 2️⃣ Instálalo y verifica su instalación con:





```shell
composer -V
```



Si devuelve un número de versión, está instalado correctamente.

#### **3. Instalar Visual Studio Code (VS Code)**



1️⃣ Descarga VS Code desde https://code.visualstudio.com/.

2️⃣ Instala la extensión **PHP Intelephense** para mejorar la experiencia de desarrollo en PHP.



### ✅ **Paso 2: Instalación de Laravel 12 en XAMPP**

#### **1. Abrir la Terminal y Acceder a la Carpeta `htdocs` de XAMPP**





```shell
cd C:\xampp\htdocs
```

#### **2. Crear un Nuevo Proyecto Laravel 12**



Ejecuta el siguiente comando para instalar Laravel en XAMPP:



```shell
composer create-project --prefer-dist laravel/laravel TechCorpSystem
```

#### **3. Configurar la Base de Datos en phpMyAdmin**



1️⃣ Abre **XAMPP** y accede a phpMyAdmin (`http://localhost/phpmyadmin`).

2️⃣ Crea una nueva base de datos llamada `techcorp_db`.



#### **4. Configurar Laravel para Conectar la Base de Datos**



Abre el archivo `.env` en VS Code y edita los siguientes valores:





```php
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=techcorp_db
DB_USERNAME=root
DB_PASSWORD=
```

### ✅ **Paso 3: Ejecutar Laravel y Verificar que Funciona**



1️⃣ **Levantar el servidor de Laravel:**



```shell
php artisan serve
```



2️⃣ **Abrir el navegador en `http://127.0.0.1:8000/`** para ver la página de inicio.

### ✅ **Paso 4: Crear la Página de Bienvenida Personalizada**

#### **1. Crear una Ruta para la Página de Inicio**



Edita `routes/web.php` para modificar la ruta principal:



```php+HTML
use Illuminate\Support\Facades\Route;

Route::get('/', function () {
    return view('welcome', ['empresa' => 'TechCorp']);
});
```

#### **2. Personalizar la Vista `welcome.blade.php`**



Edita `resources/views/welcome.blade.php`:



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

#### 🔸 **3. Comprobar los Cambios en el Navegador**



Abre `http://127.0.0.1:8000/` y verifica la página personalizada.



🎯 **¡Felicidades!** Has instalado y configurado Laravel 12 en XAMPP con éxito.

