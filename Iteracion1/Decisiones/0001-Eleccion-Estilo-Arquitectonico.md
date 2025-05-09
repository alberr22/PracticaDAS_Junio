# Elección del estilo de la arquitectura utilizada
* Status: Accepted
* Date: 6/02/2025
* Decision-Makers: Alejandro Rico, Elena Ceinos
* Consulted: Gaizka Aranbarri, Alberto Acebes
* Informed: Jon Mazcuñán, Daniel Rong, Pablo Villamayor
---
## Drivers de Decisión
* **RD-04:** La arquitectura deberá contar necesariamente con microservicios
## Context and Problem Statement
La empresa de productos alimenticios busca migrar su arquitectura monolítica hacia una basada en microservicios para mejorar la flexibilidad y escalabilidad del sistema. Actualmente, el sistema cuenta con clientes en PC y móvil que acceden a una base de datos SQL. El acceso a los datos se realizará a través de HTTP/REST mediante un Gateway adecuado, permitiendo una comunicación eficiente entre los clientes y los distintos servicios.
 

## Considered Options

* **0001-1-utilizar-arquitectura-cliente-servidor**

## Decision Outcome

Chosen option: "0001-1-utilizar-arquitectura-cliente-servidor"

### Consequences

### Positive Consequences

* Reducción de duplicación: Al centralizar la lógica en el servidor, se evita duplicar funciones críticas como la gestión de rutas o el procesamiento de pagos en los clientes.
* Escalabilidad del cliente: Permite soportar una mayor variedad de plataformas de cliente (PC, móvil, tabletas) con una interacción mínima, ya que la lógica principal está en el servidor.
* Soporte a múltiples clientes simultáneos: Es ideal para manejar un gran número de usuarios simultáneos con peticiones diversas, como pedidos o consultas de rutas.


### Negative Consequences

* Requerimientos de conectividad: Los clientes necesitan una conexión constante y confiable para interactuar con el servidor, lo que podría ser problemático en áreas con baja conectividad.
* Complejidad en la sincronización: Si los clientes necesitan operar parcialmente offline, mantener la consistencia de datos entre cliente y servidor añade complejidad.

### Aplicaciones en la arquitectura
En un diseño arquitectónico de software basado en cuatro capas, estas se dividen de la siguiente manera:

- Capa de Cliente:
Esta capa es la componentes con la que interactúan los usuarios finales. En este caso, se presentan dos componentes principales: una para dispositivos móviles y otra para PC.

- Capa de Negocio:
Aquí se encuentra la lógica principal de la aplicación.

- Capa de Acceso a Base de Datos:
Esta capa se encarga de interactuar con la base de datos.

- Capa de Seguridad:
Aquí se gestionan aspectos como la autenticación, autorización, etc.
