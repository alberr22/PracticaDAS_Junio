# Cómo gestionar la cantidad de intentos de un usuario
* Status: Accepted
* Date: 11/02/2025
* Decision-Makers: Alejandro Rico, Elena Ceinos
* Consulted: Gaizka Aranbarri, Alberto Acebes
* Informed: Jon Mazcuñán, Daniel Rong, Pablo Villamayor
---

## Context and Problem Statement

El sistema de pedidos es un componente crítico en la nueva arquitectura basada en microservicios. Un cliente puede intentar realizar un pedido un máximo de 3 veces en una sesión antes de ser bloqueado temporalmente. Dado que la escalabilidad del sistema depende del número de pedidos por hora, es crucial manejar estos intentos de manera eficiente y asegurar la estabilidad del sistema ante cargas elevadas.

## Considered Options

* **0004-1-Utilizar patrón Circuit Braker**
* **0004-2-Utilizar patrón State**

## Drivers de Decisión

* **RF-03: Realizar pedidos de los productos de la empresa**
* **RF-03.1: Gestionar intentos máximos de los pedidos**

### Consequences

## Pros and Cons of the Options

### 0004-1-Utilizar patrón Circuit Breaker

* **Good** porque permite evitar sobrecarga del sistema al bloquear intentos fallidos automáticamente.
* **Good** porque es una solución ampliamente utilizada en entornos de microservicios para garantizar estabilidad.
* **Bad** porque está diseñado más para gestionar fallos en servicios externos, no tanto para la lógica interna del negocio.
* **Bad** porque la lógica de intentos de pedido quedaría acoplada a la infraestructura en lugar de estar modelada en la aplicación.

### 0004-2-Utilizar patrón State
* **Good** porque permite modelar de manera clara cada intento y controlar el flujo sin afectar otros microservicios.
* **Good** porque se puede integrar con una caché distribuida (ej. Redis) para mejorar rendimiento sin sobrecargar la base de datos.
* **Bad** porque requiere más código para gestionar transiciones de estado.
* **Bad** porque añade un nivel adicional de complejidad en la lógica de negocio del módulo de pedidos.

## Decision Outcome
**Chosen option: "0004-2-Utilizar patrón State"**
Se implementará un sistema basado en el patrón State donde cada intento de pedido tendrá un estado asociado y las transiciones se manejarán mediante una capa de control en caché. Esto garantizará un control claro sobre los intentos y permitirá modificaciones futuras sin afectar el resto del sistema. Los estados posibles son: iniciado, 1er intento, 2º intento, 3er intento, fallido, expirado y completado.

## Positive consequences
* Se mantiene un control preciso de los intentos por usuario.
* Reduce la carga en la base de datos al gestionar los estados en caché.
* Facilita la extensibilidad del sistema si se requieren nuevas reglas de intentos.
* Se alinea mejor con la lógica de negocio y permite personalizar el comportamiento en función de la sesión del usuario.

## Negative consequences

Requiere una implementación más detallada para gestionar correctamente las transiciones de estado.
Puede necesitar integración con un sistema de caché distribuida (ej. Redis) para optimizar el rendimiento.
