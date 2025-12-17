---
hide:
  - navigation
  - toc
---

# Inicio
Bienvenido a la wiki de AceStack.

AceStack es un stack modular para la ingestión, procesamiento y distribución de streams AceStream y listas M3U/XC,
diseñado para funcionar de forma resiliente, escalable y automatizada.

## Empieza aquí

<div class="grid cards" markdown>

-   :material-rocket-launch:{ .lg .middle } __Guía de instalación__

    ---

    Instalación recomendada y configuraciones alternativas para desplegar AceStack con Docker.

    [:octicons-arrow-right-24: Empezar instalación](setup/index.md)

-   :material-video-wireless:{ .lg .middle } __Backend AceStream__

    ---

    Configuración avanzada del orquestador AceStream.

    [:octicons-arrow-right-24: Ver backend](backend/backend-index)

-   :lucide-airplay:{ .lg .middle } __Distribución IPTV__

    ---

    Configuración de Dispatcharr y microservicios para una experiencia IPTV completa.

    [:octicons-arrow-right-24: Ver distribución](distribución/distribution-index.md)

</div>

## Arquitectura general

El siguiente diagrama muestra la arquitectura global de AceStack y cómo los distintos servicios
colaboran para capturar streams AceStream, procesarlos y distribuirlos como canales IPTV
de forma (opcionalmente detrás de VPN) y automatizada..

<figure markdown>
  ![Arquitectura global de AceStack](assets/architecture.png)
  <figcaption>Arquitectura global del sistema AceStack</figcaption>
</figure>

**Claves del diagrama:**

- Todo el tráfico P2P y de streaming pasa por contenedores protegidos con **Gluetun VPN**.
- El backend AceStream escala motores bajo demanda.
- Dispatcharr actúa como punto central de distribución IPTV.
- Los canales de Dispatcharr se gestionan y actualizan automáticamente mediante **streamflow**.

## Objetivos del Stack

- Proporcionar un sistema AceStream P2P robusto y escalable, capaz de crear y mantener streams de forma automática según la demanda.
- Centralizar la gestión y distribución de canales IPTV a partir de listas M3U/XC mediante Dispatcharr.
- Automatizar la incorporación, actualización y ordenación de streams en los canales mediante **streamflow**, reduciendo la gestión manual.



