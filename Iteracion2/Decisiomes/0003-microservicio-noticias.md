# Arquitectura del Microservicio de Noticias

* Status: Accepted
* Date: 13/05/2025
* Decision-Makers: Alejandro Rico, Elena Ceinos
* Consulted: Gaizka Aranbarri, Alberto Acebes
* Informed: Jon Mazcuñán, Daniel Rong, Pablo Villamayor
---

## Context and Problem Statement

El microservicio de Noticias es responsable de gestionar las suscripciones de los clientes y mostrarles actualizaciones relevantes relacionadas con sus pedidos y repartos.

---

## Drivers de decisión

- **RF-06**: Suscripción a un sistema de noticias por parte de los clientes

---

## Considered Options

*  **0003-1-Noticias generadas directamente desde los microservicios de Pedidos y Repartos**
* **0003-2-Almacenar las noticias sin suscripción, mostrando todas las relevantes al cliente**
* **0003-3-Microservicio independiente con suscripción y persistencia**
---

## Pros and Cons of the Options

### 0003-1-Noticias generadas directamente desde los microservicios de Pedidos y Repartos

**Good** porque elimina la necesidad de un microservicio dedicado a noticias.  
**Good** porque reduce el número de componentes desplegables.  
**Bad** porque acopla innecesariamente la lógica de notificaciones a servicios críticos.  
**Bad** porque complica el mantenimiento y evolución individual de cada microservicio.

### 0003-2-Almacenar las noticias sin suscripción, mostrando todas las relevantes al cliente

**Good** porque simplifica la lógica de suscripción.  
**Good** porque evita errores por falta de eventos o registros perdidos.  
**Bad** porque puede generar ruido informativo para el cliente (ver noticias irrelevantes).  
**Bad** porque no ofrece personalización de contenido, lo que afecta a la experiencia de usuario.

### 0003-3-Microservicio independiente con suscripción y persistencia

**Good** porque separa claramente las responsabilidades y desacopla el servicio de otros microservicios críticos.  
**Good** porque permite una gestión flexible y escalable de las notificaciones y suscripciones.  
**Bad** porque introduce cierta latencia y complejidad para asegurar consistencia eventual.

---

## Decision Outcome

**Chosen option: "0003-3-Microservicio independiente con suscripción y persistencia"**
Ya que garantiza desacoplamiento, personalización y escalabilidad. Este servicio se alimenta de eventos publicados por otros microservicios y expone una API REST para la gestión de preferencias del cliente.
Las clases del microservicio se encargan de asociar un clienteId con tipos de noticias ('PEDIDO', 'REPARTO'), escuchar eventos desde otros servicios y generar Objetos 'Noticia' y almacenarlos.

---


### Positive Consequences

- Desacoplamiento completo del microservicio de otros componentes críticos.
- Experiencia personalizada para cada cliente gracias al sistema de suscripciones.
- Arquitectura preparada para escalar y manejar volumen de eventos en tiempo real.
- Integración futura sencilla con sistemas de notificaciones externas (ej. Twilio, Telegram).

---

### Negative Consequences

- Se requiere infraestructura de mensajería y consumidores de eventos.
- Gestión adicional para asegurar consistencia eventual y entrega confiable de eventos.
- Añade un punto adicional de persistencia y mantenimiento.
