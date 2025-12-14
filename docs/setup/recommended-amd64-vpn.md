---
hide:
    - navigation
---

# Instalación recomendada (Docker · AMD64 · VPN)

Esta guía describe la **instalación recomendada de AceStack** para sistemas **AMD64 (x86_64)**,
basada en **Docker y docker-compose**.

El despliegue se ha simplificado integrando en un **único `docker-compose.yml`** los servicios
del backend y de la distribución IPTV, facilitando una puesta en marcha rápida y mantenible.

La conectividad se protege mediante **Gluetun con Cloudflare WARP**, una solución VPN gratuita
suficiente para evitar la inspección del tráfico por parte del proveedor de Internet.

---

## 1. Requisitos

Antes de comenzar, asegúrate de cumplir los siguientes requisitos:

- Sistema **Linux** sobre plataforma **AMD64 (x86_64)**
- Docker
- Docker Compose (plugin o binario)
- GPU **Intel o AMD** *(opcional, para aceleración por hardware con FFmpeg)*

## 2. Componentes incluidos

La instalación recomendada despliega los siguientes componentes, integrados en un único stack:

- **AceStream Orchestrator**: gestiona la creación, escalado y mantenimiento de los motores AceStream según la demanda.
- **AceStream Engines**: motores encargados de reproducir y servir los streams AceStream.
- **Gluetun (VPN)**: proporciona conectividad de red protegida mediante VPN para los servicios que gestionan tráfico P2P y streaming.
- **Dispatcharr**: capa central de distribución IPTV. Procesa playlists, gestiona canales y expone los endpoints M3U/XC.
- **Streamflow**: automatiza la incorporación, actualización y ordenación de streams en los canales de Dispatcharr.
- **ZeroNet / M3USource**: acceso y transformación de listas M3U desde fuentes descentralizadas (ZeroNet) para su uso en el stack.

## 3. Descarga de ficheros

Coloca los siguientes ficheros en un mismo directorio (por ejemplo, `/srv/acestack`):

- :material-file-code-outline: **[docker-compose.yml](https://github.com/acestack-org/acestack-docs/blob/main/files/recommended/amd64-vpn/docker-compose.yml)**
- :material-file-cog-outline: **[.env](https://github.com/acestack-org/acestack-docs/blob/main/files/recommended/amd64-vpn/.env)**

## 4. Generación del perfil WireGuard (Cloudflare WARP)

Esta instalación utiliza **Gluetun** como contenedor VPN, configurado con
**Cloudflare WARP** mediante un perfil **WireGuard personalizado**.

Para simplificar el proceso, se utiliza un **generador automatizado**, sin necesidad
de instalar herramientas adicionales ni editar archivos manualmente.
Este paso solo es necesario **una vez**, antes de arrancar el stack.

**Generar el perfil WireGuard**

Ejecuta el siguiente comando desde el directorio donde se encuentran el
`docker-compose.yml` y el `.env`:

```bash
docker run --rm -v $(pwd):/app 2025dev/gen-warp:latest
```

Este comando generará automáticamente los siguientes archivos:

- `wg0.conf` → perfil WireGuard listo para usar en Gluetun
- `account.toml` → identidad WARP (solo necesario si deseas regenerar el perfil en el futuro)

No es necesario realizar ninguna modificación adicional sobre estos archivos.

## 5. Configuración incluida

La instalación recomendada ya incluye una serie de ajustes pensados para
simplificar el despliegue y ofrecer un comportamiento estable por defecto.

- El escalado de motores AceStream se ha ajustado a un rango moderado
  (`MIN_REPLICAS=2`, `MAX_REPLICAS=10`), suficiente para un uso básico.
- El stack se despliega mediante un **único `docker-compose.yml`**,
  integrando backend y distribución IPTV.
- Gluetun está configurado para usar **WireGuard** con el perfil de
  **Cloudflare WARP** generado en el paso anterior.

No es necesario modificar ninguna de estas configuraciones para continuar
con la instalación.

!!! info "Aceleración por hardware (GPU)"
    El servicio **Dispatcharr** incluye habilitado el acceso a `/dev/dri` para
    permitir aceleración por hardware en sistemas **Intel o AMD** compatibles.

    Si tu sistema no es compatible, basta con comentar estas líneas en el
    `docker-compose.yml`:

    ```yaml
    dispatcharr:
      devices:
        - /dev/dri:/dev/dri
    ```

## 6. Arranque del stack

Una vez completados los pasos anteriores, arranca el stack ejecutando el siguiente
comando desde el directorio donde se encuentran el `docker-compose.yml` y el `.env`:

```bash
sudo docker compose pull && sudo docker compose up -d
```

Docker descargará las imágenes necesarias y arrancará todos los servicios en segundo plano.
El proceso puede tardar unos minutos la primera vez.

**Acceso a los servicios**

Cuando el stack esté en funcionamiento, los servicios estarán disponibles en las
siguientes URLs:

* **Orchestrator Panel**: [http://localhost:8000/panel/](http://localhost:8000/panel/)
* **Dispatcharr**: [http://localhost:9191](http://localhost:9191)
* **Streamflow**: [http://localhost:3000](http://localhost:3000)

## 7. Configuración inicial de las aplicaciones

Una vez arrancado el stack, es necesario realizar una **configuración inicial**
de las aplicaciones para que el sistema empiece a funcionar correctamente.

Esta sección describe los pasos básicos de configuración en el orden recomendado.

### 7.1 Orchestrator Panel

Accede al panel del orquestador desde:

- http://localhost:8000/panel/

Desde aquí podrás:

- Ver el estado del backend AceStream
- Comprobar que los motores se crean correctamente
- Verificar que el escalado funciona según los valores configurados

No es necesario modificar ninguna opción en este punto para continuar.

### 7.2 Dispatcharr

Accede a Dispatcharr desde:

- http://localhost:9191

Configuración inicial recomendada:

1. Accede a la interfaz web.
2. Configura los parámetros básicos de funcionamiento (si aplica).
3. Verifica que el servicio está accesible y operativo.

Dispatcharr actuará como punto central de distribución de canales IPTV.

### 7.3 Streamflow

Accede a Streamflow desde:

- http://localhost:3000

Streamflow se encarga de:

- Incorporar nuevos streams
- Actualizar canales existentes
- Mantener la coherencia de los canales en Dispatcharr

En esta fase inicial:

- Verifica que Streamflow puede conectarse correctamente a Dispatcharr.
- Comprueba que no aparecen errores en el panel principal.

### 7.4 Verificación básica del sistema

Una vez configuradas las aplicaciones:

- Asegúrate de que todos los servicios están accesibles vía web.
- Verifica que no hay errores visibles en las interfaces.
- Comprueba que el backend AceStream mantiene motores activos.

Si todo es correcto, el stack está listo para su uso básico.
