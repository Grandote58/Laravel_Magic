# **🛠️ Preparación del Entorno**

## Paso a Paso 👣

### **📄 Paso 1:Instalar XAMPP**

 Activar Apache y MySQL.

- ✅ Apache
- ✅ MySQL

💡 *Tip:* Si ambos servicios están en verde, ¡todo va bien! Si alguno no inicia, puede estar siendo usado por otro programa (como Skype o IIS). Cambia el puerto o detén el servicio conflictivo.



### **📄 Paso 2 Instalar Composer 📦**

Composer es el gestor de dependencias de PHP. Laravel lo necesita para instalar sus componentes.

#### 🔹 Acciones:

1. Ve a https://getcomposer.org/
2. Descarga el instalador para Windows
3. Ejecuta el instalador y asegúrate de que detecte PHP automáticamente en:
    `C:\xampp\php\php.exe`
4. Finaliza la instalación

✅ Verifica que Composer está instalado abriendo la consola (cmd) y escribiendo:

```css
composer -V
```

🔁 Deberías ver algo como:

​		`Composer version 2.x.x`

### **📄 Paso 3: Crear el Proyecto Laravel**

```css
composer create-project laravel/laravel tiendaOnline
```

⌛ Esto descargará e instalará Laravel en la carpeta `tiendaOnline`. 



### **📄 Paso 4: Configurar el archivo `.env` de Laravel**

El archivo `.env` es la configuración de entorno. Aquí se define la conexión a la base de datos.

🔹 Acciones:

1. Abre el archivo `.env` ubicado en la raíz del proyecto
2. Cambia las siguientes líneas:

```css
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=tienda
DB_USERNAME=root
DB_PASSWORD=
```

⚠️ *Asegúrate de que tu base de datos se llame `tienda` en phpMyAdmin*

### **🗃️ Paso 5: Crear la base de datos en phpMyAdmin 🧩**

phpMyAdmin es una herramienta visual para manejar bases de datos MySQL.

#### 🔹 Acciones:

1. Abre tu navegador y entra a:
    `http://localhost/phpmyadmin`
2. Haz clic en “Base de datos”
3. Crea una base de datos llamada:
    `tienda`
4. Usa cotejamiento: `utf8mb4_general_ci`









