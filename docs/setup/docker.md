---
hide:
    - navigation
---

# Despliegue con Docker

Para usar el stack son necesarios los siguientes requisitos:

* Linux x64, ARM32/64 (Dispatcharr no soporta ARM32)
* Docker
* GPU (Para Dispatcharr si quieres usar FFmpeg como proxy)

Después de desplegar todo el stack se puede acceder a los servicios mediante las siguientes URLs:

* Orchestrator Panel: [http://localhost:8000/panel/](http://localhost:8000/panel/)
* Dispatcharr: [http://localhost:9191](http://localhost:9191/)
* StreamFlow: [http://localhost:3000](http://localhost:3000/)

### 1. Despliegue del Backend AceStream

Según tus requsitos y tu plataforma, escoje un fichero `docker-compose.yml` y un fichero `.env`

| Arquitectura | VPN                 | docker-compose                                                          | .env                                          |
| ------------ | ------------------- | ----------------------------------------------------------------------- | --------------------------------------------- |
| x64          | No                  | [docker-compose.yml](https://github.com/acestack-org/acestack-docs/blob/main/files/compose/standalone/docker-compose.yml)    | [.env](https://github.com/acestack-org/acestack-docs/blob/main/files/env/x64/standalone/.env)      |
|              | Simple              | [docker-compose.yml](https://github.com/acestack-org/acestack-docs/blob/main/files/compose/single-vpn/docker-compose.yml)    | [.env](https://github.com/acestack-org/acestack-docs/blob/main/files/env/x64/single-vpn/.env)      |
|              | Alta Disponibilidad | [docker-compose.yml](https://github.com/acestack-org/acestack-docs/blob/main/files/compose/redundant-vpn/docker-compose.yml) | [.env](https://github.com/acestack-org/acestack-docs/blob/main/files/env/x64/redundant-vpn/.env)   |
| ARM64        | No                  | [docker-compose.yml](https://github.com/acestack-org/acestack-docs/blob/main/files/compose/standalone/docker-compose.yml)  | [.env](https://github.com/acestack-org/acestack-docs/blob/main/files/env/ARM64/standalone/.env)    |
|              | Simple              | [docker-compose.yml](https://github.com/acestack-org/acestack-docs/blob/main/files/compose/single-vpn/docker-compose.yml)    | [.env]((https://github.com/acestack-org/acestack-docs/blob/main/files/env/ARM64/single-vpn/.env))    |
|              | Alta Disponibilidad | [docker-compose.yml](https://github.com/acestack-org/acestack-docs/blob/main/files/compose/redundant-vpn/docker-compose.yml) | [.env](https://github.com/acestack-org/acestack-docs/blob/main/files/env/ARM64/redundant-vpn/.env) |
| ARM32        | No                  | [docker-compose.yml](https://github.com/acestack-org/acestack-docs/blob/main/files/compose/standalone/docker-compose.yml)    | [.env](https://github.com/acestack-org/acestack-docs/blob/main/files/env/ARM32/standalone/.env)    |
|              | Simple              | [docker-compose.yml](https://github.com/acestack-org/acestack-docs/blob/main/files/compose/single-vpn/docker-compose.yml)    | [.env](https://github.com/acestack-org/acestack-docs/blob/main/files/env/ARM32/single-vpn/.env)    |
|              | Alta Disponibilidad | [docker-compose.yml](https://github.com/acestack-org/acestack-docs/blob/main/files/compose/redundant-vpn/docker-compose.yml) | [.env](https://github.com/acestack-org/acestack-docs/blob/main/files/env/ARM32/redundant-vpn/.env)    |

Si has elegido una versión del stack con VPN, debes configurar el contenedor Gluetun. Solo tienes que completar la sección correspondiente en el `docker-compose` y seguir la guía específica de [Configuración de VPN con Gluetun](../backend/vpn/backend-vpn.md).

Para modificar el número mínimo de engines para equipos más débiles, modificar la línea `MIN_REPLICAS` en el fichero `.env`

El siguiente paso es desplegar el stack. Para ello, situamos los dos ficheros en un directorio y dentro de ese directorio ejecutamos:

```bash
sudo docker compose pull && sudo docker compose up -d
```

### 2. Despliegue de Dispatcharr y microservicios (solo 64 bits)

Según hayamos desplegado o no un backend que use una VPN o no, debemos escoger un `docker-compose`:

| VPN? | docker-compose                                                       |
| ---- | -------------------------------------------------------------------- |
| ✅    | [docker-compose.yml](https://github.com/acestack-org/acestack-docs/blob/main/files/iptv-suite/vpn/docker-compose.yml)     |
| ❌    | [docker-compose.yml](https://github.com/acestack-org/acestack-docs/blob/main/files/iptv-suite/non-vpn/docker-compose.yml) |

Si se utiliza la opción VPN hay que tener en cuenta lo siguiente:

* M3USource accede a ZeroNet a través de `localhost:43110`
* Dispatcharr accede a M3USource través de `gluetun:5000`

Es necesario configurar varias cosas en el docker-compose:

* Los mounts de los contenedores (sección volumes de cada contenedor) tienen que coincidir con directorios existentes en el sistema
* En Streamflow hay que indicar las credenciales de Dispatcharr

El siguiente paso es desplegar el stack. Para ello, situamos el fichero en un directorio y dentro de ese directorio ejecutamos:

```bash
sudo docker compose pull && sudo docker compose up -d
```
