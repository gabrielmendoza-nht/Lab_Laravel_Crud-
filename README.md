<p align="center">
  <img src="https://laravel.com/img/logomark.min.svg" alt="Laravel Logo" width="80">
</p>

# Laboratorio CRUD de Productos con Laravel

![Laravel](https://img.shields.io/badge/Laravel-PHP-red?logo=laravel)
![MySQL](https://img.shields.io/badge/Base%20de%20datos-MySQL-blue?logo=mysql)
![Entorno](https://img.shields.io/badge/Entorno-Windows%20%7C%20WampServer-lightgrey)
![CRUD](https://img.shields.io/badge/Proyecto-CRUD-success)

## Universidad Tecnológica de Panamá

### Facultad de ingeniería en Sistemas

**Laboratorio:** CRUD
## Estudiante: Gabriel Mendoza 
---

## 🎯 Objetivos

- Comprender la estructura de una aplicación desarrollada con Laravel.
- Crear y configurar un proyecto Laravel desde un entorno local.
- Aplicar el patrón Modelo-Vista-Controlador (MVC) en un módulo de productos.
- Trabajar con migraciones y una base de datos MySQL.
- Implementar las operaciones CRUD: crear, consultar, actualizar y eliminar productos.
- Revisar las rutas, el controlador, el modelo y las vistas que conectan las distintas partes de la aplicación.

## 📝 Introducción

Este proyecto es el resultado del laboratorio **"[Título del laboratorio]"**, en el que se construye un módulo para administrar productos usando las herramientas de Laravel, evitando programar manualmente cada parte desde cero.

Se trabajó desde PowerShell en Windows, con WampServer, dentro de la carpeta `C:\wamp64\www`. El proyecto utiliza una base de datos MySQL y se revisaron la configuración del entorno, las migraciones, el modelo `Product`, el controlador `ProductController`, las rutas y las vistas Blade.

## ⚙️ Requisitos y herramientas

- **Windows y PowerShell:** para ejecutar los comandos.
- **WampServer:** entorno local de desarrollo.
- **PHP:** versión compatible con tu versión de Laravel (por ejemplo, PHP 8.3).
- **Composer:** administrador de dependencias de PHP.
- **Laravel Installer:** para crear el proyecto con `laravel new`.
- **MySQL:** motor de base de datos.
- **Editor de código:** para modificar los archivos PHP y Blade.

## 🔧 Procedimiento realizado

### 1. Abrir PowerShell y ubicarse en `www`

```powershell
cd C:\wamp64\www
```

`www` es el directorio donde WampServer guarda los proyectos web locales.

![IMG1: PowerShell ubicado en la carpeta www] <img width="1357" height="464" alt="image" src="https://github.com/user-attachments/assets/3fa7772c-9c06-4f1c-8298-933cf49a96d3" />


### 2. Crear el proyecto Laravel

```powershell
laravel new nombre_del_proyecto
```

El instalador crea la carpeta del proyecto con la estructura inicial de la aplicación. Si el proyecto ya existe, no se debe volver a ejecutar este comando sobre la misma carpeta.

![IMG2: Creación del proyecto](Imagenes/Img2.png)

### 3. Entrar en la carpeta del proyecto

```powershell
cd C:\wamp64\www\nombre_del_proyecto
```

Desde aquí se ejecutan los comandos de Artisan. En la raíz se encuentra el archivo `artisan`, y las carpetas principales son `app`, `database`, `resources` y `routes`.

![IMG3: Estructura inicial del proyecto](Imagenes/Img3.png)

### 4. Revisar el archivo `.env`

Antes de ejecutar las migraciones, se revisa el archivo `.env`, que contiene la configuración del entorno local, incluida la conexión con MySQL:

```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=nombre_del_proyecto
DB_USERNAME=root
DB_PASSWORD=
```

> ⚠️ No subas contraseñas reales al repositorio. Usa `.env.example` como referencia.

![IMG4: Configuración del archivo .env](Imagenes/Img4.png)

### 5. Ajustar la longitud de cadenas (si es necesario)

Si aparecen errores de migración relacionados con la longitud de las cadenas, se puede ajustar en `app/Providers/AppServiceProvider.php`:

```php
use Illuminate\Support\Facades\Schema;

// Dentro del método boot():
Schema::defaultStringLength(191);
```

Este ajuste solo se aplica cuando el entorno lo requiere.

### 6. Preparar la base de datos y ejecutar las migraciones

Las migraciones describen la estructura de las tablas en archivos PHP.

```bash
php artisan migrate
php artisan migrate:status
```

- `migrate` ejecuta las migraciones pendientes.
- `migrate:status` muestra cuáles ya se ejecutaron y cuáles faltan.
- `migrate:fresh` elimina todas las tablas y las vuelve a crear (**no usar si hay datos que conservar**).
- `migrate:rollback` revierte el último lote de migraciones.

![IMG6: Ejecución de las migraciones](Imagenes/Img6.png)

### 7. Crear el modelo `Product` y su migración

```bash
php artisan make:model Product -m
```

El modelo se ubica en `app/Models/Product.php` y la migración en `database/migrations`. El modelo representa los productos y permite acceder a sus registros mediante Eloquent.

![IMG7: Modelo Product y migración generada](Imagenes/Img7.png)

### 8. Definir las columnas de la tabla de productos

En la migración se definen los campos de cada producto:

```php
$table->id();
$table->string('name');
$table->string('description');
$table->double('price', 8, 2);
$table->integer('stock');
$table->timestamps();
```

Los nombres del formulario, el modelo y la base de datos deben coincidir. Si la tabla ya existe y le falta alguna columna, hay que crear una migración de modificación; cambiar solo el formulario no añade columnas a MySQL.

```bash
php artisan migrate
```

![IMG8: Columnas definidas para la tabla de productos](Imagenes/Img8.png)

### 9. Instalar el generador de CRUD

```bash
composer require ibex/crud-generator --dev
```

Este paquete puede generar controladores, vistas, solicitudes y rutas. Si ya existen esos archivos, conviene revisar su contenido antes de volver a generarlos para no sobrescribir cambios.

![IMG9: Instalación del generador CRUD](Imagenes/Img9.png)

### 10. Publicar los archivos del paquete

```bash
php artisan vendor:publish --tag=crud
```

![IMG10: Publicación de recursos del generador](Imagenes/Img10.png)

### 11. Generar el CRUD para `products`

```bash
php artisan make:crud products
```

Después de ejecutarlo, se deben revisar los archivos creados y adaptarlos a los campos y necesidades reales del proyecto.

![IMG11: Ejecución del generador](Imagenes/Img11.png)

### 12. Revisar las rutas del recurso

En `routes/web.php`:

```php
use App\Http\Controllers\ProductController;

Route::resource('products', ProductController::class);
```

La ruta de recurso asocia las direcciones habituales con los métodos del controlador. Para comprobarlas:

```bash
php artisan route:list --path=products
```

![IMG12: Rutas del recurso products](Imagenes/Img12.png)

## 🏗️ Cómo se organiza el CRUD (MVC)

### Modelo: `Product`

Ubicado en `app/Models/Product.php`. Representa los productos y se comunica con la tabla mediante Eloquent. Si se usan métodos como `create()` o `update()` con arreglos de datos, hay que definir el atributo `$fillable` con los campos permitidos.

### Controlador: `ProductController`

Ubicado en `app/Http/Controllers/ProductController.php`:

| Método | Función |
|--------|---------|
| `index` | Obtiene y muestra el listado. |
| `create` | Presenta el formulario de creación. |
| `store` | Valida y guarda un producto nuevo. |
| `show` | Muestra los detalles de un producto. |
| `edit` | Presenta el formulario de edición. |
| `update` | Procesa los cambios de un producto. |
| `destroy` | Elimina el registro seleccionado. |

### Vistas Blade

Ubicadas en `resources/views/product/`:

- `index.blade.php`: listado de productos.
- `create.blade.php`: formulario de creación.
- `edit.blade.php`: formulario de edición.
- `show.blade.php`: detalle de un producto.
- `form.blade.php`: formulario compartido entre crear y editar.

### Validaciones y nombres de campos

Las reglas de validación deben corresponder con los campos que envía el formulario. Si un campo se llama `description` en la migración pero la vista envía otro nombre, el valor podría no guardarse o no mostrarse.

## 🧩 Funciones principales del CRUD

### 1. Crear un producto

En el formulario se introducen nombre, descripción, precio y stock. El controlador valida los datos y los guarda mediante el modelo.

![IMG13: Formulario de creación](Imagenes/Img13.png)

### 2. Consultar el listado

La vista `index.blade.php` muestra los productos en una tabla, con acceso a las opciones de consultar, editar y eliminar.

![IMG14: Listado de productos](Imagenes/Img14.png)

### 3. Consultar los detalles

La vista `show.blade.php` presenta los datos del producto seleccionado sin permitir modificarlos.

![IMG15: Detalles de un producto](Imagenes/Img15.png)

### 4. Editar un producto

El formulario de edición carga los datos actuales y, al guardar, el método `update` procesa los cambios.

![IMG16: Formulario de edición](Imagenes/Img16.png)

### 5. Eliminar un producto

El método `destroy` borra el registro seleccionado. Conviene verificar que se eligió el producto correcto antes de confirmar.

![IMG17: Eliminación de un producto](Imagenes/Img17.png)

## ▶️ Levantar el sistema

Desde la carpeta del proyecto:

```bash
php artisan serve
```

La terminal muestra una dirección local (normalmente `http://127.0.0.1:8000`) que se abre en el navegador. Si el proyecto tiene configurado el script, también se puede usar:

```bash
composer run dev
```

![IMG18: Aplicación en ejecución](Imagenes/Img18.png)

## 🗄️ Base de datos y mantenimiento

La tabla `products` debe contener las columnas que usa la aplicación. Si el listado muestra valores vacíos o el precio aparece como `0.00`, comprueba:

- Que la migración haya creado las columnas necesarias.
- Que el formulario envíe los nombres de campo esperados.
- Que las validaciones acepten los valores recibidos.
- Que `$fillable` permita asignar los campos requeridos.
- Que los métodos `store` y `update` guarden los valores correctos.

![IMG19: Estructura o datos de la base de datos](Imagenes/Img19.png)

Como respaldo, se puede exportar la base de datos desde phpMyAdmin en formato SQL.

## 🛠️ Comandos de referencia

| Comando | Función |
|---------|---------|
| `cd C:\wamp64\www` | Ubicarse en la carpeta `www` de WampServer. |
| `laravel new nombre_del_proyecto` | Crear el proyecto Laravel. |
| `php artisan config:clear` | Limpiar la caché de configuración. |
| `php artisan cache:clear` | Limpiar la caché de la aplicación. |
| `php artisan config:cache` | Volver a generar la caché de configuración. |
| `php artisan migrate` | Ejecutar las migraciones pendientes. |
| `php artisan migrate:fresh` | Eliminar y recrear las tablas. Usar con precaución. |
| `php artisan migrate:rollback` | Revertir el último lote de migraciones. |
| `php artisan make:model Product -m` | Crear el modelo y su migración. |
| `composer require ibex/crud-generator --dev` | Instalar el generador de CRUD. |
| `php artisan vendor:publish --tag=crud` | Publicar los recursos del generador. |
| `php artisan make:crud products` | Generar el CRUD para `products`. |
| `php artisan route:list --path=products` | Revisar las rutas de productos. |
| `composer dump-autoload` | Regenerar el mapa de autocarga de Composer. |
| `php artisan serve` | Iniciar el servidor de desarrollo. |

## ⚠️ Dificultades y comprobaciones

Fue importante verificar que la estructura de la tabla coincidiera con los campos usados en la interfaz. Una migración que solo cree el identificador y las fechas no basta si el formulario necesita guardar nombre, descripción, precio y stock.

Si los datos no se guardan o se muestran incompletos, revisar en este orden:

1. Las columnas existentes en `products`.
2. Los atributos `name`, `description`, `price` y `stock` en la vista y la migración.
3. Las reglas de validación.
4. El atributo `$fillable` del modelo `Product`.
5. Los métodos `store` y `update` de `ProductController`.

Para evitar perder datos, primero se debe revisar el esquema y preparar una migración correctiva en lugar de usar `migrate:fresh`.

![IMG21: Resultado final del laboratorio](Imagenes/Img21.png)

## 📚 Referencias

- **Guía de laboratorio:** *[Título de la guía]*, material proporcionado para la práctica.
- Laravel. *Documentación oficial*. <https://laravel.com/docs>
- Laravel. *Migraciones*. <https://laravel.com/docs/migrations>
- Laravel. *Eloquent ORM*. <https://laravel.com/docs/eloquent>

## 👤 Información del estudiante

- **Universidad:** Universidad Tecnológica de Panama
- **Facultad:** Facultad de Ingeniería en Sistemas 
- **Nombre:** Gabriel Mendoza
- **Curso:** Desarrollo Web
- **Profesora:** Irina Fong

---

*Laboratorio CRUD con Laravel · *
