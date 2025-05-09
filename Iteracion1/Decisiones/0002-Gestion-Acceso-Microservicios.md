# Elección del patrón utilizado para el acceso de los usuarios a los microservicios
* Status: Accepted
* Date: 7/02/2025
* Decision-Makers: Alejandro Rico, Elena Ceinos
* Consulted: Gaizka Aranbarri, Alberto Acebes
* Informed: Jon Mazcuñán, Daniel Rong, Pablo Villamayor
---

## Context and Problem Statement

La arquitectura basada en microservicios requiere una solución eficiente para gestionar el acceso de los clientes (PC y móvil) a los distintos servicios. Sin una capa intermedia, los clientes tendrían que interactuar directamente con múltiples microservicios, aumentando la complejidad, el tráfico de red y la necesidad de gestionar múltiples autenticaciones y autorizaciones.

Para resolver este problema, se evalúa el uso de un **API Gateway**, que actuará como punto de entrada único, gestionando autenticación, autorización, balanceo de carga y enrutamiento inteligente de solicitudes a los microservicios adecuados.

## Considered Options

* **0002-1 - Utilizar patrón Gateway**

## Drivers de Decisión

* **RF-01.1:** Gestión centralizada de peticiones a microservicios.
* **RD-01.2:** Uso de la API Gateway.

## Decision Outcome

Chosen option: **"0002-1 - Utilizar patrón Gateway"**, porque proporciona un punto de acceso único que mejora la seguridad, escalabilidad y gestión del tráfico de la arquitectura de microservicios.

### Consequences

### Positive Consequences

* **Gestión centralizada de autenticación y autorización:** Los clientes solo interactúan con la API Gateway, que maneja la autenticación mediante OAuth 2.0 / JWT.
* **Balanceo de carga:** Distribuye automáticamente las peticiones a las instancias disponibles de los microservicios.
* **Reducción del acoplamiento:** Los clientes no necesitan conocer la ubicación o detalles internos de los microservicios.
* **Seguridad reforzada:** Protección contra ataques mediante rate limiting, firewalls y

