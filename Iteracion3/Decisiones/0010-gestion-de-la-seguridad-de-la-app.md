# Sistema para la capa de seguridad

* Status: Accepted  
* Date: 27/05/2025  
* Decision-Makers: Alejandro Rico, Elena Ceinos  
* Consulted: Gaizka Aranbarri, Alberto Acebes  
* Informed: Jon Mazcuñán, Pablo Villamayor  
---

## Context and Problem Statement

Se debe implementar un sistema de seguridad para la aplicación. Se ha decidido que esta seguridad debe implementarse como una **capa externa desacoplada**, al igual que con la base de datos o la pasarela de pagos, que proporcione autenticación de usuarios, autenticación de dos factores (2FA) y generación/validación de tokens. Esta solución debe integrarse de forma sencilla y clara en la arquitectura de microservicios y quedar representada en el diagrama UML como un componente adicional.

## Decision drivers

* RF-10: Implementar un sistema de seguridad

## Considered Options

* **0010.1-Usar Keycloak como sistema de autenticación autoalojado**
* **0010.2-Usar Firebase Authentication como servicio en la nube**
* **0010.3-Usar Auth0 como sistema externo de seguridad**

## Pros and Cons of the Options

### 0010.1-Usar Keycloak como sistema de autenticación autoalojado

* **Good** porque puede integrarse como un componente independiente en la arquitectura y representarse claramente en el UML como un microservicio adicional.
* **Good** porque permite centralizar la autenticación de todos los servicios y mantener la lógica dentro del ecosistema de microservicios.
* **Bad** porque añade complejidad al despliegue y mantenimiento de la infraestructura, lo que complica la arquitectura a largo plazo.
* **Bad** porque requiere definir y mantener flujos de autenticación y autorización internamente, lo cual dispersa responsabilidades en el diseño.

### 0010.2-Usar Firebase Authentication como servicio en la nube

* **Good** porque permite representar la seguridad como un proveedor externo desacoplado, ideal para el diseño modular.
* **Good** porque facilita la conexión directa desde clientes móviles o web, eliminando acoplamiento con microservicios internos.
* **Bad** porque no encaja bien en el diagrama de componentes si se necesita incluir lógica interna de seguridad como temporizadores de sesión o gestión de tokens entre servicios.
* **Bad** porque su orientación a frontend lo convierte en una solución limitada en la arquitectura backend de microservicios.

### 0010.3-Usar Auth0 como sistema externo de seguridad

* **Good** porque se integra como un **bloque externo bien definido en la arquitectura**, representable en el UML como un componente “Security” o “Auth0” al mismo nivel que Stripe o PostgreSQL.
* **Good** porque mantiene el diseño desacoplado: el gateway o los microservicios simplemente consumen tokens ya validados, sin lógica de autenticación interna.
* **Good** porque estandariza el flujo de autenticación para todos los clientes (web y móvil) y servicios, simplificando el diseño UML y las relaciones entre componentes.
* **Bad** porque introduce una **dependencia externa** que debe indicarse explícitamente en el diagrama, lo cual puede ser percibido como un punto débil en términos de control.
* **Bad** porque no permite extender internamente la lógica sin salir del esquema SaaS, lo cual limita algunas decisiones futuras reflejadas en arquitectura.

## Decision Outcome

* **Chosen option: "0010.3-Usar Auth0 como sistema externo de seguridad"**

### Positive Consequences

* La solución se representa claramente en la arquitectura como una capa externa reutilizable, coherente con el estilo del resto de la solución (como Stripe o la BBDD).
* Mejora la legibilidad y modularidad del diagrama UML, simplificando las relaciones entre el gateway, los clientes y el sistema de seguridad.
* Refuerza el principio de separación de responsabilidades, dejando la seguridad fuera del núcleo de microservicios.
* Reduce la complejidad arquitectónica al eliminar la necesidad de microservicios de seguridad internos.

### Negative Consequences

* El componente externo (Auth0) debe documentarse claramente en la arquitectura, incluyendo su relación con el gateway y los servicios críticos.
* La integración de un proveedor externo introduce una dependencia sobre la que no se tiene control directo, lo que podría afectar al diseño futuro si cambian sus condiciones o disponibilidad.
