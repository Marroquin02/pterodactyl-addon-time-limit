# Pterodactyl Addon: Server Time Limit

Este addon permite establecer un límite de tiempo en **segundos** para los servidores creados en Pterodactyl. El límite de tiempo puede ser configurado al momento de crear un servidor a través de la API. Una vez que el servidor excede este tiempo, puede ser gestionado para detenerse automáticamente o ser administrado según las necesidades.

## Requisitos

Este addon está diseñado para Pterodactyl v1.x. Asegúrate de estar utilizando una versión compatible del panel de Pterodactyl antes de proceder.

## Funcionalidades

- **Campo `time_limit` en segundos**: Ahora puedes establecer un límite de tiempo en segundos cuando creas un servidor a través de la API.
- **Migración de Base de Datos**: Se añade una nueva columna `time_limit` en la tabla `servers` para almacenar el límite de tiempo de cada servidor.
  
## Instalación

Sigue estos pasos para instalar el addon en tu panel de Pterodactyl:

### 1. Clonar o Descargar este Repositorio

Clona el repositorio o descarga el código como un archivo `.zip` en tu instalación de Pterodactyl:

```bash
git clone https://github.com/tu-usuario/pterodactyl-addon-time-limit.git
```

### 2. Copiar los Archivos Modificados

Dentro de la carpeta descargada, encontrarás una estructura similar a la de Pterodactyl. Copia los archivos del addon en sus respectivas rutas dentro de tu instalación de Pterodactyl.

- Copia los archivos dentro de `app/` a `app/` en tu servidor de Pterodactyl.
- Copia las migraciones dentro de `database/migrations/` a `database/migrations/`.

**Importante:** Asegúrate de que los archivos reemplazados sean respaldados antes de continuar.

### 3. Ejecutar la Migración

Después de copiar los archivos, es necesario ejecutar las migraciones para agregar la columna `time_limit` a la tabla `servers`. Ejecuta el siguiente comando dentro del directorio de tu instalación de Pterodactyl:

```bash
php artisan migrate
```

### 4. Reiniciar el Panel

Para que los cambios surtan efecto, reinicia el servicio de Pterodactyl:

```bash
systemctl restart pteroq
```
### 5. Uso del Campo `time_limit`

Cuando crees un servidor a través de la API de Pterodactyl, ahora podrás incluir un nuevo campo `time_limit`, el cual define el límite de tiempo en segundos para el servidor.

#### Ejemplo de Solicitud a la API:

```json
{
  "name": "Mi Servidor",
  "user": 1,
  "egg": 3,
  "docker_image": "quay.io/pterodactyl/core:nodejs",
  "startup": "npm start",
  "environment": {
    "SERVER_PORT": "8080"
  },
  "limits": {
    "memory": 512,
    "swap": 0,
    "disk": 10240,
    "io": 500,
    "cpu": 100
  },
  "feature_limits": {
    "databases": 1,
    "allocations": 1,
    "backups": 1
  },
  "allocation": {
    "default": 1
  },
  "time_limit": 3600  # Limite de tiempo en segundos (1 hora)
}
```

### 6. Verificar el Límite de Tiempo

El campo `time_limit` es opcional. Si no se especifica, el servidor no tendrá un límite de tiempo establecido. Si deseas implementar una lógica que detenga automáticamente los servidores al superar este límite, deberás integrar esa funcionalidad personalizada.
