# Microservicio de Estadísticas

* Status: Accepted  
* Date: 27/05/2025  
* Decision-Makers: Alejandro Rico, Elena Ceinos  
* Consulted: Gaizka Aranbarri, Alberto Acebes  
* Informed: Jon Mazcuñán, Pablo Villamayor  
---

## Context and Problem Statement

El sistema necesita mostrar estadísticas en tiempo real sobre:
- Estado de pedidos (completados, en proceso, retrasados).  
- Situación de camiones (ubicación, rutas activas, incidencias).  
- Históricos para análisis (tendencias, eficiencia logística).

## Drivers de decisión

* RF-05: Mostrar estadísticas a clientes y empleados.  
* RD-03: Sistema de pago seguro utilizando Stripe.  

## Considered Options

* **EST-1: Estadísticas embebidas en microservicio de Pedidos/Repartos**  
* **EST-2: Microservicio independiente con base de datos analítica**  
* **EST-3: Solución externa (Power BI, Google Analytics)**  

## Pros and Cons of the Options

## Pros and Cons of the Options

### EST-1: Estadísticas embebidas en microservicio de Pedidos/Repartos  
* **Good** porque minimiza la duplicación de datos.  
* **Good** porque simplifica el número de microservicios, reduciendo la cantidad de componentes en el diagrama UML.  
* **Bad** porque degrada el rendimiento de servicios críticos al añadirles lógica analítica no esencial.  
* **Bad** porque rompe el principio de responsabilidad única (SRP), acoplando lógica de negocio y lógica analítica.  
* **Bad** porque dificulta la escalabilidad individual del servicio de estadísticas.  
* **Bad** porque reduce la modularidad en el UML, complicando futuras ampliaciones o refactorizaciones.

### EST-2: Microservicio independiente con base de datos analítica  
* **Good** porque permite escalabilidad dedicada y tecnologías optimizadas (PostgreSQL).  
* **Good** porque mantiene bajo acoplamiento, alineándose con la arquitectura de microservicios.  
* **Good** porque es fácilmente representable como un microservicio autónomo en el diagrama UML.  
* **Bad** porque requiere sincronización mediante eventos, lo que añade complejidad arquitectónica.  
* **Bad** porque introduce consistencia eventual entre servicios, afectando la fiabilidad inmediata de los datos.  
* **Bad** porque añade carga cognitiva en el UML al incorporar nuevos componentes, bases de datos y mecanismos de cacheo.

### EST-3: Solución externa (Power BI, Google Analytics)  
* **Good** porque ofrece visualización avanzada sin desarrollo interno.  
* **Good** porque reduce el número de componentes implementados directamente en el sistema, simplificando el UML.  
* **Good** porque puede modelarse claramente como un sistema externo desacoplado.  
* **Bad** porque dificulta la integración con microservicios internos, lo que complica los flujos de datos en la arquitectura.  

## Decision Outcome

* **Chosen option: "EST-2 - Microservicio independiente"**

La solución seleccionada alinea con la arquitectura basada en microservicios, permitiendo escalar el servicio de estadísticas de forma independiente. Se utilizará PostgreSQL con extensiones como TimescaleDB para gestionar series temporales, Redis para acelerar consultas frecuentes y Kafka para consumir eventos de Pedidos y Repartos.

El microservicio de estadísticas estará compuesto por:
- Un consumidor de eventos (`EventConsumer`) que capturará eventos de Kafka relacionados con pedidos y repartos.
- Un controlador REST (`StatsController`) que ofrecerá una API para empleados y clientes con estadísticas personalizadas.
- Un modelo de datos (`RepartosStats`) para almacenar y consultar la información analítica de pedidos y entregas.
- Un DTO (`StatsResponseDTO`) como estructura de respuesta de la API pública.

## Positive Consequences

* El servicio puede escalar de forma independiente, sin afectar a los módulos críticos como Pedidos o Repartos.  
* Refuerza la arquitectura modular al añadir un microservicio desacoplado que se representa claramente en el UML.  
* Permite la reutilización del microservicio desde otros módulos (ej. marketing, logística) sin impacto directo en el core.

## Negative Consequences

* Aumenta el número de componentes en la arquitectura, lo que puede hacer el sistema más difícil de desplegar y mantener.  
* El crecimiento del diagrama UML puede dificultar su interpretación si no se organiza correctamente por dominios o capas.

