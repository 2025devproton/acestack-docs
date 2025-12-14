---
hide:
  - navigation
  - toc
---

# Guía de instalación

En esta sección encontrarás las distintas formas de instalar AceStack según tu entorno y necesidades.

Si no sabes por dónde empezar, utiliza la **instalación recomendada**, que es la opción más utilizada y probada por la mayoría de usuarios.


<div class="grid cards" markdown>

-   :material-star-circle:{ .lg .middle } **Instalación recomendada (Docker + AMD64 + VPN)**

    ---

    Configuración recomendada para la mayoría de despliegues en **Docker sobre AMD64**.

    Incluye backend AceStream, **VPN con Cloudflare WARP**, Dispatcharr y Streamflow,
    con valores de configuración probados por usuarios y un despliegue simplificado
    basado en un único `docker-compose.yml`.

    [:octicons-arrow-right-24: Usar esta instalación](recommended-amd64-vpn.md)

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
