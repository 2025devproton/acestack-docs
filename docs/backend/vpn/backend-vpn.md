---
icon: lucide/shield-check
---
# Configuración de VPN con Gluetun

La VPN del Orquestador se gestiona mediante el contenedor **Gluetun**, que actúa como un túnel seguro basado en **WireGuard** entre el sistema y el exterior. El objetivo es simple: ocultar el tráfico real del servidor, evitar bloqueos por IP, asegurar la comunicación y mantener una salida estable y privada.

---

## ¿Qué es Gluetun?

**Gluetun** es un contenedor Docker que crea y mantiene un túnel VPN mediante WireGuard. En este stack proporciona una salida de red protegida y estable para los Engines sin exponer su IP pública.

Si quieres usar otro proveedor de VPN diferente a Cloudflare Warp, Gluetun soporta muchos servicios y cada uno necesita parámetros específicos. Puedes consultar la lista completa de proveedores y sus configuraciones en la [Gluetun Wiki](https://github.com/qdm12/gluetun-wiki/tree/main/setup/providers).

---

## Configuración de VPN con Cloudflare Warp

Cloudflare Warp permite generar un perfil WireGuard que puede usarse directamente con Gluetun como VPN personalizada.

### 1. Generar el perfil WireGuard

Utiliza el generador automatizado disponible en el repositorio:

https://github.com/2025devproton/warp-gluetun-generator

Este proyecto proporciona una imagen Docker que genera automáticamente un perfil WireGuard compatible con Gluetun. Ejecuta:

```bash
docker run --rm -v $(pwd):/out 2025devproton/warp-gluetun-generator
```

Esto creará en el directorio actual:

  - `wg0.conf` → ya listo para usar en Gluetun
  - `account.toml (identidad Warp; solo necesario si quieres regenerar futuros perfiles)

No necesitas instalar `wgcf` ni editar archivos manualmente.` → contiene la configuración WireGuard completa

### 2. Ubicar el archivo `wg0.conf`

El generador ya produce un `wg0.conf` totalmente válido. Solo tienes que colocarlo en tu carpeta de despliegue:

```
files/compose/single-vpn/wg0.conf
```

No requiere modificaciones adicionales: contiene claves, direcciones, DNS y el peer de Cloudflare listo para usar.`

### 3. Usar Gluetun con el perfil generado

En el `docker-compose`, monta el archivo y activa el modo personalizado:

```yaml
volumes:
  - ./wg0.conf:/gluetun/wireguard/wg0.conf:ro

environment:
  - VPN_SERVICE_PROVIDER=custom
  - VPN_TYPE=wireguard
```

Gluetun cargará el túnel Cloudflare Warp y lo usará como salida de red del stack.

### 4. Rotar claves o regenerar el perfil

Para actualizar el perfil, vuelve a ejecutar:

```bash
docker run --rm -v $(pwd):/out 2025devproton/warp-gluetun-generator
```

Sustituye el nuevo `wg0.conf` y reinicia Gluetun:

```bash
docker compose restart gluetun
```
