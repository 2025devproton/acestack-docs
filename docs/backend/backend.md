# Backend AceStream

### Esquema General

<figure><img src=".gitbook/assets/acestream-backend.png" alt=""><figcaption></figcaption></figure>

El sistema lo conforman 4 componentes principales:

* AceStream Engine: contenedor que permite reproducir streams P2P exponiendo un endpoint desde el que emite un stream HTTP.
* Acexy: proxy que permite canalizar a través de un único endpoint streams procedentes de los AceStream Engines.
* AceStream-Orchestrator: utilidad que permite la creación, gestión y mantenimiento de instancias de AceStream Engines.
* Gluetun (opcional): contenedor que permite conectar contenedores a una red VPN



### AceStream Engine

{% embed url="https://github.com/krinkuto11/acestream-http-proxy" %}

Esta fork del proxy de Martin permite la especificación de los siguientes parámetros:

* Puerto P2P (útil para su uso con Gluetun y una VPN que soporte P2P)
* Puerto HTTP y HTTPS
* Módulo de buffering interno
* Límite de tamaño de caché para el módulo de buffering
* Modo Bind All para poder acceder a más funciones de la API desde fuera del contenedor

### AceStream Orchestrator

{% embed url="https://github.com/krinkuto11/acestream-orchestrator" %}

El cometido principal de este programa es abordar la innata inestabilidad de los streams P2P. El contenedor AceStream Engine es muy propenso a errores y cuenta con un manejo de estos reducido. Para ello, el orquestrador crea una "pool" de engines y asegura que en todo momento una solicitud de reproducción sea atendida.&#x20;

El orquestrador mantiene un número mínimo de engines listos para ser utilizados y expone a través de un endpoint una lista de sus direcciones y puertos, así como estadísticas tales como estado de salud, última vez usado, etc...

Esta misma API provee de acceso programático a la utilidad, permitiendo que otras herramientas soliciten nuevos engines según las necesidades.

Cuenta con un sistema de mantenimiento que mantiene todos los engines a punto, comprobando periódicamente su estado de salud y el tamaño de su caché.&#x20;

Cuenta con un módulo que, de estar habilitado, permite el provisionamiento de Engines que estén conectados a una VPN mediante Gluetun (y que usen su puerto P2P).

Para visualizar todo esto, existe una dashboard:

<div data-with-frame="true"><figure><img src=".gitbook/assets/Screenshot 2025-10-29 at 12.04.31.png" alt=""><figcaption></figcaption></figure></div>

<figure><img src=".gitbook/assets/Screenshot 2025-10-29 at 12.12.27.png" alt=""><figcaption></figcaption></figure>

### Acexy

{% embed url="https://github.com/krinkuto11/acexy" %}

Esta fork del (maravilloso) proyecto hecho por [Javinator](https://github.com/Javinator9889/acexy) tiene como objetivo permitir la multiplexación de streams y a diferencia de el programa original permite, gracias a la integración con el Orchestrator:

* Balanceo de carga: reparte y asigna streams entre todos los Engines que ofrece el Orchestrator
* Escalada de capacidad: según las necesidades, puede solicitar más Engines y asignar los streams dinámicamente.

Todo esto se gestiona internamente y solamente ofrece un endpoint `/ace/getstream?id=<id>`
