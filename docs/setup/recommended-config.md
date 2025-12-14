---
hide:
    - navigation
---

# Configuración recomendada



Esta sección asume que has completado previamente la **[Instalación recomendada](recommended-amd64-vpn.md)** y que todos los servicios están en ejecución.

El objetivo es dejar el sistema operativo con el menor número de ajustes manuales,
utilizando **configuraciones probadas por usuarios**, válido para la mayoría de escenarios habituales.

 
## 1. Orchestrator Panel

Accede al panel del orquestador:
http://localhost:8000/panel/

### 1.1 Templates de motores

Configura los templates de motores desde: 
http://localhost:8000/panel/advanced-engine-settings<br>
En la sección **Template Management**, pulsa **Import** y carga el fichero en el **slot 1** 
**[:material-file-cog-outline: orchestrator_template_1.json](https://github.com/acestack-org/acestack-docs/blob/main/files/recommended/orchestrator_template_1.json)**.<br>
Este template incluye valores predeterminados probados por usuarios y no requiere ajustes adicionales.

### 1.2 Detección de streams inactivos

Accede a: 
http://localhost:8000/panel/settings

En la sección **Inactive Stream Detection**, configura los valores recomendados:

| Parámetro                              | ✅ Recomendado | Valor defecto |
|----------------------------------------|-------|-------|
| Live Position Unchanged Threshold (s)  | 60    | 15    |
| Prebuffering Status Threshold (s)      | 10    | 10    |
| Zero Speed Threshold (s)               | 10    | 10    |
| Low Speed Threshold (KB/s)             | 300   | 400   |
| Low Speed Duration Threshold (s)       | 20    | 20    |

Estos valores priorizan la detección rápida de streams inactivos sin afectar a streams estables.

## 2. Dispatcharr

Accede a Dispatcharr desde:
http://localhost:9191

En el primer acceso, Dispatcharr solicitará la creación de un **super usuario**. Guarda estas credenciales, se usarán para administrar toda la instalación.

### 2.1 Crear lista M3U
Accede a "M3U & EPG Manager" y pulsa "Add M3U".

| Campo                         | Valor |
|------------------------------|-------|
| Name                         | M3U `XXXX` |
| URL                          | `http://gluetun:5000/modify_m3u?host=acexy&port=8080&m3u_url=XXXX` |
| Account Type                 | Standard |
| Max Streams                  | 0 |
| User-Agent                   | Use Default |
| Refresh Interval             | 0 |
| Stale Stream Retention (days)| 0 |
| VOD Priority                 | 0 |
| Is Active                    | ✅ |

!!! tip "No olvides modificar XXXX"
    Ajusta el **nombre** y la **URL** del M3U original (`XXXX`) según tu proveedor.

### 2.2 Configurar EPG

En la misma pantalla, pulsa "Add EPG" y selecciona "Standard EPG Source".

| Campo                   | Valor |
|-------------------------|-------|
| Name                    | EPG `XXXX` |
| Source Type             | XMLTV |
| Refresh Interval (hours)| 24 |
| URL                     | `XXXXX` |
| Priority                | 0 |
| Status                  | ✅ Enable this EPG source |


!!! tip "No olvides modificar XXXX"
    Puedes usar listas públicas como las disponibles en https://github.com/davidmuma/EPG_dobleM<br>
    Ejemplo:
    https://raw.githubusercontent.com/davidmuma/EPG_dobleM/master/guiatv_sincolor3.xml.gz


### 2.3 Crear usuario de streaming

Desde el menú lateral, entra en "Users" → "Add User".

- Username: XXXXX
- XC Password: XXXXX
- User Level: Streamer

Estas credenciales se usan para acceder vía Xtream Codes desde:
http://localhost:9191 (usa la IP del servidor si accedes desde otro dispositivo).


### 2.4 Proxy Settings

Accede a "Settings" → "Proxy Settings". 

Estos valores deben alinearse con el Orchestrator, estos ajustes reducen cortes y reinicios innecesarios de canales.

| Parámetro                              | ✅ Recomendado | Valor defecto |
|----------------------------------------|----------------|-------|
| Buffering Timeout                      | 10             | 15    |
| Buffering Speed                        | 1              | 1     |
| Buffer Chunk TTL                       | 60             | 60    |
| Channel Shutdown Delay                 | 0              | 0     |
| Channel Initialization Grace Period    | 30             | 5     |

### 2.5 Stream Profiles

Desde **Settings → Stream Profiles**, se recomienda configurar perfiles de streaming
para adaptar el comportamiento de Dispatcharr según la estabilidad de los streams
y los recursos disponibles.

En esta instalación se proponen dos perfiles:

- **Copia directa (por defecto)**: menor consumo de CPU, ideal para streams estables.
- **Transcodificación por GPU (opcional)**: más tolerante a streams problemáticos,
  requiere aceleración por hardware.

El perfil de copia es suficiente en la mayoría de casos y se recomienda como valor por defecto.

**Copia directa (por defecto):**

| Campo       | Valor |
|------------|-------|
| Name       | `COPY_PASSTHROUGH_SAFE` |
| Command    | `ffmpeg` |
| Parameters | `-user_agent {userAgent} -analyzeduration 15M -probesize 15M -fflags +discardcorrupt -i {streamUrl} -map 0:v -map 0:a? -c:v copy -c:a aac -b:a 128k -ac 2 -mpegts_copyts 1 -muxdelay 0 -muxpreload 0 -max_interleave_delta 0 -flush_packets 1 -f mpegts pipe:1` |
| User-Agent | *(dejar en blanco)* |

**Transcodificación por GPU (opcional):**

| Campo       | Valor |
|------------|-------|
| Name       | `H264_QSV_QUALITY_SMOOTH` |
| Command    | `ffmpeg` |
| Parameters | `-init_hw_device qsv=hw:/dev/dri/renderD128 -filter_hw_device hw -hwaccel qsv -hwaccel_output_format qsv -user_agent {userAgent} -fflags +genpts+discardcorrupt -i {streamUrl} -c:v h264_qsv -profile:v high -level:v 4.1 -rc_mode vbr_hq -look_ahead 1 -b:v 6M -maxrate 6M -bufsize 12M -g 50 -vsync cfr -c:a aac -b:a 128k -ac 2 -f mpegts pipe:1` |
| User-Agent | *(dejar en blanco)* |

> Este perfil requiere aceleración por hardware correctamente configurada.<br>
> Si no dispones de GPU compatible, utiliza el perfil por defecto.

### 2.6 Stream Settings (perfil por defecto)

Desde **Settings → Stream Settings**, ajusta los siguientes valores:

| Opción                 | Valor recomendado       |
| ---------------------- | ----------------------- |
| Preferred Region       | `ES`                    |
| Default Stream Profile | `COPY_PASSTHROUGH_SAFE` |

!!! tip "¿Problemas con algún stream?"
    Si detectas cortes o inestabilidad:<br>
    - Prueba el perfil **H264_QSV_QUALITY_SMOOTH** *(requiere GPU activa)*  
    - Como último recurso, puedes usar el profile **Proxy**, un perfil sin procesado que expone directamente el stream de AceStream.

## 3. Smart M3U Manager

Accede a **Smart M3U Manager** desde: http://localhost:5001

1. Inicia sesión utilizando las **credenciales de superusuario de Dispatcharr**.
2. Selecciona la **lista M3U** que deseas procesar.
3. Se mostrará el listado de canales detectados a partir del M3U.

### 3.1 Creación automática de canales

- Selecciona **todos los canales** o solo los que quieras crear en Dispatcharr.
- Pulsa **Normalize** para limpiar y unificar los nombres de los canales *(recomendado)*.
- Pulsa **Sync** para preparar la sincronización.
- Crea o selecciona un **perfil de canales**.
- Pulsa **Start Sync** para iniciar el proceso.

Al finalizar, los canales se crearán automáticamente en **Dispatcharr**, con sus streams correctamente agrupados y listos para su uso.

!!! tip "Uso recomendado"
    Smart M3U Manager está pensado para evitar la creación manual de canales.<br>
    En la mayoría de casos basta con **Normalize + Start Sync** para tener todo operativo.

## 4. Streamflow

Accede a Streamflow desde:
http://localhost:3000

!!! danger "Sin completar"
    Pendiente de completar esta sección.

## 5. Acceso IPTV – Xtream Codes

Accede a los canales IPTV desde tu cliente compatible utilizando el modo de conexión **Xtream Codes**.

Usa las credenciales del **usuario de streaming** creado en el punto anterior y
configura la conexión con la siguiente URL: http://localhost:9191

Si accedes desde otro dispositivo de la red, utiliza la **IP del servidor** en lugar de `localhost`.

