# Plataforma de pago para procesar pedidos

* Status: Accepted
* Date: 18/02/2025
* Decision-Makers: Alejandro Rico, Daniel Rong
* Consulted: Elena Ceinos, Gaizka Aranbarri
* Informed: Jon Mazcuñán, Alberto Acebes, Pablo Villamayor

---

## Context and Problem Statement

La empresa ha decidido que debemos utilizar la plataforma de pago Stripe para procesar todos los pedidos. Esta decisión se basa en varios factores clave que hacen de Stripe una opción segura y confiable para nuestras necesidades de procesamiento de pagos. 
Stripe es conocida por sus altos estándares de seguridad, cumpliendo con las normativas PCI DSS, lo que garantiza que los datos de las tarjetas de crédito y débito de nuestros clientes estén protegidos mediante encriptación avanzada. Además, Stripe ofrece una infraestructura robusta que permite manejar grandes volúmenes de transacciones de manera eficiente y segura.
Además se necesita un factor extra de seguridad, encriptar los datos de las tarjetas al pagar y limitar el tiempo de pago.

## Drivers de decisión

* **RF-08: Pagar un pedido a través de una plataforma de pago**
* **RF-08.1: Encriptar los datos de la tarjeta y limitar el tiempo de pago**
* **RD-03: Sistema de pago seguro utilizando Stripe**

## Considered Options

* **0006-1-Stripe**

## Pros and Cons of the Options

### 0006-1-Stripe

* **Good** porque permite la implementación de medidas adicionales de seguridad como la encriptación de datos de tarjetas y la limitación del tiempo de pago, lo que reduce el riesgo de fraude.
* **Good** porque ofrece una API moderna y fácil de integrar con sistemas basados en microservicios.
* **Good** porque cumple con los más altos estándares de seguridad y encriptación de datos (PCI DSS).
* **Bad** porque puede tener comisiones más altas en comparación con otras opciones en ciertas transacciones.
* **Bad** porque requiere una cuenta específica en Stripe para operar.
* **Bad** porque la implementación de estas medidas adicionales puede aumentar la complejidad del desarrollo y el tiempo necesario para la integración completa.

## Decision Outcome

* **Chosen option: "0006-1-Stripe"**
Se ha decidido utilizar Stripe como la pasarela de pago principal debido a su fácil integración con la arquitectura basada en microservicios, altos estándares de seguridad y soporte para múltiples métodos de pago. Aunque introduce ciertas tarifas, su fiabilidad y escalabilidad justifican la elección. Habrá que importar la API de Stripe con sus clases. Según la documentación de API Stripe, la importación de este componente necesita de una clase Intent (La llamaremos IntentoPago) y una clase PaymentMethod, que no se implementará ya que solamente hay una posibilidad, además de una clase Cliente (todavía no decidida) y una clase que implemente los endpoints que usará API Stripe, a la que llamaremos GestorStripe.

### Positive Consequences

* Mejora la seguridad de las transacciones al proteger los datos sensibles de los clientes mediante encriptación avanzada y limitar el tiempo de pago, reduciendo así el riesgo de fraude.
* Implementación rápida y alineada con la arquitectura del sistema.
* Flexibilidad en métodos de pago permitiendo tarjetas de crédito, débito y billeteras digitales como Apple Pay y Google Pay.
* Posibilidad de manejar disputas y reembolsos de manera centralizada dentro del mismo ecosistema Stripe.
* Integración con microservicios utilizando Webhooks para la gestión de pagos.

### Negative Consequences

* Costos de transacción más elevados en comparación con otras opciones.
* Dependencia de la infraestructura y soporte de Stripe, limitando el control sobre la pasarela de pago y su personalización completa.
