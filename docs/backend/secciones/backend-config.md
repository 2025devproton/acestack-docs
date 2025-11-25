# Configuración del Orquestrador

La configuración del Orquestrador se define en el fichero `.env` que se encuentra, por defecto, en la carpeta raíz donde se ubica el fichero `docker-compose.yml`.

Secciones de la guía:

  - Configuración General
    - Configuración Esencial
    - Configuración Avanzada
  - Configuración VPN
    - Modo simple
    - Modo redundante

## Configuración General
En esta sección de la guía se abordan las opciones que comprenden y/o afectan el funcionamiento general del Orquestrador.

### Configuración Esencial
Estas opciones deben configurarse acorde a las necesidades de tu host.

#### Clave API
**Variable:** `API_KEY`<br/>
**Tipo:** String<br/>
**Explicación:** Clave que utilizan los Proxy (Acexy) y el panel para acceder a los datos del Orquestrador. Muy importante definir una segura.<br/>
**Valor por defecto:** `ninguno, sin autenticación`<br/><br/>


#### Puerto del Orquestrador
**Variable:** `APP_PORT`<br/>
**Tipo:** Integer<br/>
**Explicación:** Define el puerto utilizado por el Orquestrador. Es necesario que encaje con el segundo número de la definición `ports`en el docker-compose.<br/>
**Valor por defecto:** `APP_PORT=8000`<br/><br/>

#### Red de Docker
**Variable:** `DOCKER_NETWORK`<br/>
**Tipo:** String<br/>
**Explicación:** Nombre de la red de Docker donde se instanciarán los contenedores. Debe ser la misma que la del Orquestrador.<br/>
**Valor por defecto:** `red por defecto del contenedor (auto-generada por docker o especificada en el compose)`<br/><br/>

#### Variante de AceStream Engine
**Variable:** `ENGINE_VARIANT`<br/>
**Tipo:** Enum <br/>
**Posibles Valores:** `krinkuto11-amd64, jopsis-amd64, jopsis-arm32, jopsis-arm64`<br/>
**Explicación:** Define la variante pre-hecha para utilizar, dependiendo de la plataforma del host y las preferencias del usuario.<br/>
**Valor por defecto:** `krinkuto11-amd64`<br/><br/>

#### Número mínimo de Engines 
**Variable:** `MIN_REPLICAS`<br/>
**Tipo:** Integer (>0) <br/>
**Explicación:** Define la cantidad mínima de Engines que debe mantener activos el Orquestrador.<br/>
**Valor por defecto:** `1`<br/><br/>

#### Número máximo de Engines 
**Variable:** `MAX_REPLICAS`<br/>
**Tipo:** Integer (>0) <br/>
**Explicación:** Define la cantidad máxima de Engines que puede mantener activos el Orquestrador. Importante si el sistema es limitado en cuanto a capacidad de procesamiento<br/>
**Valor por defecto:** `20`<br/><br/>

#### Rango de puertos para los Engines 
**Variable:** `PORT_RANGE_HOST`<br/>
**Tipo:** Integer-Integer <br/>
**Explicación:** Define el rango de puertos en el que el Orquestrador puede crear los engines. Importante que estos puertos estén libres.<br/>
**Valor por defecto:** `19000-19999`<br/><br/>

### Configuración Avanzada
Configuración opcional cuyo objetivo es optimizar al máximo el funcionamiento del Orquestrador. Es posible que cambiar estas opciones cause un comportamiento errático del mismo.

**Variable:** `PORT_RANGE_HOST`<br/>
**Tipo:** Integer-Integer <br/>
**Explicación:** Define el rango de puertos en el que el Orquestrador puede crear los engines. Importante que estos puertos estén libres.<br/>
**Valor por defecto:** `19000-19999`<br/><br/>