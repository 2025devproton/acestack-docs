# Gestión y Distribución de Streams

### Esquema General

<figure><img src=".gitbook/assets/dispatcharr (1).png" alt="" width="563"><figcaption></figcaption></figure>

### Objetivo general

El objetivo general es ofrecer la experiencia más similar a un proveedor de TV. En vez de ofrecer 10 opciones distintas para cada canal, Dispatcharr nos permite automatizar todo el proceso de cambiar de Stream cada vez que se queda parado (buffering).

Opcionalmente (y recomendado) se puede agregar un contenedor Gluetun para que estos servicios funcionen a través de una VPN.

### Dispatcharr

Dispatcharr nos ofrece las siguientes funcionalidades:

* Agregar distintas listas m3u y mantenerlas actualizadas
* Agregar fuentes de EPG
* Crear canales en los que incluir múltiples streams a modo de backup
* Juntar todo esto en una nueva lista M3U o XCodes que podemos poner en nuestros reproductores

{% hint style="info" %}
No voy a incluir capturas por riesgos con el copyright, pero mi recomendación personal es crear canales manualmente (incluyendo el logo, tvg-id y el nombre) y luego utilizando StreamFlow y expresiones regulares mantener esos canales llenos de streams
{% endhint %}

Para más información sobre Dispatcharr, aquí os dejo enlaces útiles con los que podéis aprender a utilizarlo:

{% embed url="https://github.com/Dispatcharr/Dispatcharr" %}

{% embed url="https://dispatcharr.github.io/Dispatcharr-Docs/" %}

### m3usource

Este pequeño middleware sirve para adaptar listas AceStream a el formato necesario para que Dispatcharr se comunique con el Backend AceStream.&#x20;

Al tenerlo desplegado, podemos agregar en Dispatcharr nuestras listas AceStream de la siguiente forma (sustituyendo \<enlace> por nuestro enlace:

```
http://m3usource:5000/modify_m3u?host=acexy&port=8080&m3u_url=<enlace>
```

{% embed url="https://github.com/krinkuto11/m3usource" %}

### zeronet-eguzkilore

Este contenedor es una fork del proyecto [zeronet-conservancy](https://github.com/zeronet-conservancy/zeronet-conservancy) que permite acceder a contenidos de la red Zeronet mediante un endpoint en el puerto `43110`

Para acceder a estos enlaces, el formato es así:

```
http://<nombre-del-contenedor>:43110/<path>
```

Este enlace se puede incluir en la URL de m3usource para que Dispatcharr pueda obtener una playlist de ZeroNet.

{% embed url="https://github.com/krinkuto11/zeronet-eguzkilore" %}

### StreamFlow

Esta herramienta utiliza la API de Dispatcharr para actualizar playlists automáticamente, y mediante expresiones regulares, asignar streams a canales. Esto es muy útil ya al haber cambios en una playlist cada poco tiempo puede volverse tedioso el proceso manual.

Además, ofrece la opción de que cada vez que se añada un stream a una canal, comprobar sus estadísticas (y si funciona o no) y ordenar los streams de esos canales para que se reproduzcan antes los mejores.

{% embed url="https://github.com/krinkuto11/streamflow" %}
