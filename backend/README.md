# III-UNIDAD-ISC305

API CRUD de personas en Laravel 13 para la asignatura **ISC-305 Programación Web**. Responde únicamente en JSON (sin vistas Blade) y, en esta práctica inicial, trabaja con datos ficticios y sin autenticación.

## Requisitos

- PHP 8.3 o superior, con las extensiones que requiere Laravel y `pdo_mysql`
- Composer 2
- Git
- MySQL activo
- Postman (para probar los endpoints)

## Estructura del repositorio

```
III-UNIDAD-ISC305/
├── README.md
├── postman/      Colección de Postman exportada (JSON)
└── backend/      Proyecto Laravel (aquí se ejecutan los comandos de Artisan)
```

Todos los comandos de `php artisan` se ejecutan dentro de `backend/`.

## Configuración de MySQL

1. Cree la base de datos:

   ```sql
   CREATE DATABASE isc305_unidad3
   CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
   ```

2. Copie `backend/.env.example` como `backend/.env` y reemplace el usuario y la contraseña por los de su MySQL local:

   ```env
   APP_NAME="API Personas ISC305"
   APP_URL=http://127.0.0.1:8000
   DB_CONNECTION=mysql
   DB_HOST=127.0.0.1
   DB_PORT=3306
   DB_DATABASE=isc305_unidad3
   DB_USERNAME=TU_USUARIO_MYSQL
   DB_PASSWORD="TU_CLAVE_MYSQL"
   SESSION_DRIVER=file
   CACHE_STORE=file
   QUEUE_CONNECTION=sync
   ```

   El archivo `.env` contiene credenciales locales y **no se publica** en el repositorio.

## Instalación y ejecución

Desde la raíz del repositorio:

```bash
cd backend
composer install
cp .env.example .env      # luego edite DB_USERNAME y DB_PASSWORD
php artisan key:generate
php artisan config:clear
php artisan migrate
php artisan serve
```

La API queda disponible en `http://127.0.0.1:8000`. Si Laravel informa otro puerto, utilícelo en las URL.

## Endpoints

Las rutas de `routes/api.php` llevan el prefijo `/api` automáticamente. En todas las solicitudes envíe el encabezado `Accept: application/json`; en POST y PUT agregue también `Content-Type: application/json`.

| Método | Ruta | Descripción | Respuesta esperada |
|--------|------|-------------|--------------------|
| GET | `/api/hola` | Mensaje de bienvenida | 200 |
| GET | `/api/personas` | Lista todas las personas | 200 (arreglo, vacío si no hay registros) |
| GET | `/api/personas/{id}` | Consulta una persona | 200, o 404 si no existe |
| POST | `/api/personas` | Crea una persona | 201 con la persona creada, o 422 si la validación falla |
| PUT | `/api/personas/{id}` | Actualiza una persona | 200, 404 o 422 |
| DELETE | `/api/personas/{id}` | Elimina una persona | 204 sin cuerpo, o 404 |

### Modelo Persona

| Campo | Tipo | Reglas |
|-------|------|--------|
| `id` | entero | Clave primaria automática |
| `dni` | texto (13) | Obligatorio, exactamente 13 dígitos, único |
| `nombre` | texto (100) | Obligatorio, máximo 100 caracteres |
| `fecha_nacimiento` | fecha | Obligatoria, no puede ser futura |

### Ejemplo de cuerpo para POST y PUT

```json
{
  "dni": "0000000000001",
  "nombre": "Ana López",
  "fecha_nacimiento": "2000-05-15"
}
```

### Respuesta de error de validación (422)

```json
{
  "message": "...",
  "errors": {
    "nombre": ["..."]
  }
}
```

## Archivos principales (dentro de `backend/`)

| Ruta | Responsabilidad |
|------|-----------------|
| `routes/api.php` | Relaciona las URL con las operaciones de la API |
| `app/Http/Controllers/PersonaController.php` | Recibe peticiones y construye las respuestas |
| `app/Models/Persona.php` | Representa los registros mediante Eloquent |
| `database/migrations/` | Define la estructura de las tablas |
| `.env` | Configura la conexión y el entorno local (no se publica) |

## Colección de Postman

La colección exportada está en la carpeta `postman/`. Impórtela en Postman para probar los endpoints.