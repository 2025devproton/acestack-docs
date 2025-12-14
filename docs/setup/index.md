---
hide:
  - navigation
  - toc
---

# Guía de instalación

En esta sección encontrarás las distintas formas de instalar AceStack según tu entorno y necesidades.

Si no sabes por dónde empezar, utiliza la **instalación recomendada**, que es la opción más utilizada y probada por la mayoría de usuarios.


<div class="grid cards" markdown>

-   :material-star-circle:{ .lg .middle } **Instalación recomendada**

    ---

    Puesta en marcha recomendada para la mayoría de despliegues en **Docker sobre AMD64 con VPN**.

    Incluye backend AceStream, **VPN con Cloudflare WARP**, Dispatcharr y Streamflow, con un despliegue simplificado basado en un único `docker-compose.yml`.

    [:octicons-arrow-right-24: Instalación recomendada](recommended-amd64-vpn.md)

-   :material-tune-variant:{ .lg .middle } **Configuración recomendada**

    ---

    Guía paso a paso para **dejar el sistema completamente operativo**
    tras la instalación.

    Incluye la configuración recomendada de Orchestrator, Dispatcharr,
    Smart M3U Manager y perfiles de streaming, utilizando **valores probados
    por usuarios** para un uso estable y habitual.

    [:octicons-arrow-right-24: Configuración recomendada](recommended-config.md)

-   :material-docker:{ .lg .middle } __Instalaciones Docker__

    ---

    Guía completa de instalaciones Docker con **configuraciones alternativas** y escenarios especiales.

    Incluye todas las combinaciones soportadas: con y sin VPN, arquitecturas **AMD64** y **ARM64**,
    VPN en alta disponibilidad, despliegues simples y topologías personalizadas.

    [:octicons-arrow-right-24: Ver configuraciones](docker.md)

</div>

---

!!! info "Sobre las instalaciones"
    Todas las instalaciones utilizan `Docker` y `docker-compose` como base.
    Los detalles de cada componente (backend, VPN, distribución) se explican
    en sus secciones específicas de la documentación.
