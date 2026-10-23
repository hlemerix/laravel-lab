<div align="center">

<img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/laravel/laravel-original.svg" width="90" alt="Laravel">

# Laravel Lab

**Aplicación web de laboratorio · Autenticación y patrón MVC**

| | |
|---|---|
| **Framework** | Laravel 13.36 |
| **Autenticación** | Laravel UI 4 |
| **Base de datos** | SQLite de ejemplo; MySQL configurable |
| **Repositorio** | [hlemerix/laravel-lab](https://github.com/hlemerix/laravel-lab) |

</div>

---

## 📑 Índice

1. [Introducción y objetivo](#1--introducción-y-objetivo)
2. [Resultado visible del proyecto](#2--resultado-visible-del-proyecto)
3. [Requisitos previos](#3--requisitos-previos)
4. [Instalación y ejecución](#4--instalación-y-ejecución)
5. [Base de datos](#5--base-de-datos)
6. [Funcionalidades y controles](#6--funcionalidades-y-controles)
7. [Estructura del proyecto y MVC](#7--estructura-del-proyecto-y-mvc)
8. [Evidencias de migraciones y registros](#8--evidencias-de-migraciones-y-registros)
9. [Seguridad y datos sensibles](#9--seguridad-y-datos-sensibles)
10. [Referencias](#10--referencias)

---

## 1. 📘 Introducción y objetivo

### Descripción

**Laravel Lab** es una aplicación web desarrollada con Laravel. Incluye las páginas de registro e inicio de sesión, recuperación de contraseña y un panel protegido para usuarios autenticados. El proyecto también contiene la estructura inicial de un módulo de productos y su migración de base de datos.

La aplicación permite practicar el flujo **Modelo–Vista–Controlador (MVC)**, el enrutamiento, la autenticación, las vistas Blade y las migraciones de Laravel.

### Objetivos

- Reconocer las capas principales de una aplicación Laravel y cómo se relacionan.
- Usar el sistema de autenticación para registrar usuarios e iniciar y cerrar sesión.
- Proteger páginas mediante middleware de autenticación.
- Configurar una base de datos y aplicar migraciones con Artisan.

### Componentes MVC

| Capa | Carpeta o archivo | Función |
|---|---|---|
| **Modelo** | `app/Models/` | Representa los datos de la aplicación, como usuarios y productos. |
| **Vista** | `resources/views/` | Contiene las páginas Blade, los formularios de autenticación y el panel. |
| **Controlador** | `app/Http/Controllers/` | Atiende solicitudes y coordina las respuestas de la aplicación. |
| **Rutas** | `routes/web.php` | Define las páginas web y las rutas de autenticación. |
| **Migraciones** | `database/migrations/` | Define las tablas necesarias para la aplicación. |

**Flujo de una petición:** el navegador solicita una ruta → Laravel la dirige al controlador correspondiente → la aplicación consulta o actualiza datos cuando es necesario → se devuelve una vista al navegador.

---

## 2. 🖼️ Resultado visible del proyecto

### Servidor local

La aplicación puede ejecutarse localmente y visitarse en `http://127.0.0.1:8000`.

![Servidor de desarrollo ejecutándose](docs/img/02-servidor-local.png)

### Página inicial

![Página inicial de Laravel](docs/img/03-pagina-inicial.png)

### Registro

El formulario solicita nombre, correo electrónico y contraseña. La captura con datos ingresados fue protegida antes de incluirse aquí.

![Formulario de registro vacío](docs/img/04-formulario-registro.png)

![Formulario de registro con datos protegidos](docs/img/05-registro-datos-protegidos.png)

### Panel de usuario

La ruta `/home` muestra el panel después de iniciar sesión.

![Panel de usuario autenticado](docs/img/07-panel-usuario.png)

---

## 3. 🧰 Requisitos previos

| Tecnología | Requisito |
|---|---|
| **PHP** | 8.3 o superior |
| **Composer** | Versión compatible con Laravel |
| **Node.js y npm** | Para instalar y compilar los recursos frontend |
| **Base de datos** | SQLite para la configuración de ejemplo o MySQL si se configura |
| **Git** | Para clonar el repositorio |

El proyecto usa Laravel Framework 13.36.0, Laravel UI 4.6.3 y Vite para compilar los recursos frontend.

Para comprobar las herramientas en Windows PowerShell:

```powershell
php -v
composer --version
node --version
npm --version
```

---

## 4. ⚙️ Instalación y ejecución

### 4.1 Clonar el repositorio

```powershell
git clone https://github.com/hlemerix/laravel-lab.git
cd laravel-lab
```

### 4.2 Instalar y preparar la aplicación

```powershell
composer run setup
```

El comando instala las dependencias de PHP, crea `.env` desde `.env.example` si hace falta, genera la clave de la aplicación, ejecuta las migraciones e instala y compila los recursos frontend.

### 4.3 Iniciar el entorno de desarrollo

```powershell
composer run dev
```

También puedes iniciar el servidor de Laravel y Vite por separado, cada uno en su propia terminal:

```powershell
php artisan serve
```

```powershell
npm run dev
```

Después, abre `http://127.0.0.1:8000` en el navegador.

### Rutas principales

| Ruta | Función |
|---|---|
| `/` | Página de bienvenida |
| `/register` | Registro de usuario |
| `/login` | Inicio de sesión |
| `/password/reset` | Solicitud de restablecimiento de contraseña |
| `/home` | Panel protegido; requiere iniciar sesión |

---

## 5. 🗄️ Base de datos

### Configuración

La plantilla `.env.example` está preparada para SQLite. Si eliges MySQL, crea primero una base de datos vacía y configura las variables `DB_CONNECTION`, `DB_HOST`, `DB_PORT`, `DB_DATABASE`, `DB_USERNAME` y `DB_PASSWORD` en tu archivo `.env`.

Después de modificar la configuración del entorno, limpia la caché y ejecuta las migraciones:

```powershell
php artisan config:clear
php artisan migrate
```

> No publiques ni compartas `.env`: puede contener la clave de la aplicación y credenciales. Utiliza `.env.example` como plantilla y reemplaza cualquier dato de ejemplo por valores locales.

### Migraciones incluidas

El proyecto contiene migraciones para usuarios, caché, trabajos en cola y la tabla `productos`. La migración de productos crea actualmente el identificador y las marcas de tiempo; el módulo CRUD de productos todavía no está implementado.

![Estructura de tablas en la base de datos, con identificadores locales protegidos](docs/img/06-estructura-base-datos.png)

Para borrar y volver a crear todas las tablas de la base configurada existe:

```powershell
php artisan migrate:fresh
```

> **Precaución:** `migrate:fresh` elimina todas las tablas de esa base de datos. No lo ejecutes en una base con información que necesites conservar.

---

## 6. 🎛️ Funcionalidades y controles

| Funcionalidad | Descripción |
|---|---|
| **Registro** | Crea una cuenta con nombre, correo y contraseña. |
| **Inicio y cierre de sesión** | Gestiona el acceso a la aplicación. |
| **Restablecimiento de contraseña** | Incluye las rutas y vistas de recuperación de contraseña de Laravel UI. |
| **Panel protegido** | `/home` requiere autenticación. |
| **Recursos frontend** | Vite compila los estilos y recursos JavaScript. |
| **Productos** | Hay modelo, controlador y migración iniciales; las operaciones CRUD están pendientes. |

La autenticación de las vistas se genera con Laravel UI y las plantillas Blade correspondientes.

---

## 7. 🗂️ Estructura del proyecto y MVC

```text
laravel-lab/
├── app/
│   ├── Http/Controllers/     # Controladores web y de autenticación
│   └── Models/               # Modelos User y Producto
├── database/
│   └── migrations/           # Estructura de las tablas
├── docs/
│   └── img/                  # Capturas de pantalla protegidas
├── resources/
│   ├── js/                   # Recursos JavaScript
│   ├── sass/                 # Estilos
│   └── views/                # Vistas Blade
├── routes/
│   └── web.php               # Rutas web y autenticación
├── .env.example              # Plantilla de configuración local
├── composer.json
└── package.json
```

---

## 8. ✅ Evidencias de migraciones y registros

La captura de la terminal muestra las migraciones ejecutadas, incluida la migración inicial de productos.

![Migraciones completadas](docs/img/09-migraciones.png)

La captura de la tabla de usuarios oculta los nombres, correos y hashes de contraseña. No se incluyen contraseñas ni identificadores de sesión en las evidencias.

![Registros de usuarios con datos protegidos](docs/img/08-registros-protegidos.png)

---

## 9. 🔐 Seguridad y datos sensibles

- Mantén `.env` fuera del repositorio y nunca pegues credenciales reales en documentación o capturas.
- Antes de compartir capturas, oculta nombres, correos, contraseñas, hashes, tokens, cookies, direcciones privadas y rutas de usuario.
- Las imágenes de esta documentación son copias protegidas; los archivos originales no se usan como imágenes del README.
- En producción, configura `APP_DEBUG=false` y utiliza valores seguros para las variables del entorno.

---

## 10. 📚 Referencias

1. Laravel. [Documentación oficial](https://laravel.com/docs).
2. Laravel. [Instalación](https://laravel.com/docs/installation).
3. Laravel. [Migraciones](https://laravel.com/docs/migrations).
4. Laravel UI. [Repositorio oficial](https://github.com/laravel/ui).
5. Vite. [Guía oficial](https://vite.dev/guide/).
6. Bootstrap. [Documentación oficial](https://getbootstrap.com/docs/).

---

<div align="center">

**Repositorio:** [github.com/hlemerix/laravel-lab](https://github.com/hlemerix/laravel-lab)

</div>
