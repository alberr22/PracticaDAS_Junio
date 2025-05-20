# Elección del gestor SQL para microservicios

* **Status:** Aprobado  
* **Fecha:** 07/05/2025  
* **Decision-Makers:** Alejandro Rico, Elena Ceinos  
* **Consulted:** Gaizka Aranbarri, Alberto Acebes  
* **Informed:** Jon Mazcuñán, Daniel Rong, Pablo Villamayor  

---

## Context and Problem Statement

Como parte de la migración de una arquitectura monolítica a microservicios, es necesario elegir un **sistema de gestión de bases de datos SQL (RDBMS)** adecuado. La nueva arquitectura requiere bases de datos independientes por servicio (Database-per-Service), transacciones consistentes, y buen rendimiento en operaciones concurrentes.

---

## Drivers de Decisión

- **RF-02.2:**  Acceder a los datos de los pedidos.
- **RF-02.1:** Acceder a los datos personales de los clientes.
- **RF-02:** Acceder a los datos de los pedidos.

---

## Considered Options
* **0007-1-PostgreSQL**
* **0007-2-MySQL**
* **0007-3-SQL Server**


## Pros and Cons of the Options

### 0007-1-PostgreSQL
- **Bueno:** Madurez, potencia y soporte completo de características SQL.
- **Bueno:** Ideal para microservicios, transacciones complejas, datos relacionales.
- **Bueno:** Compatibilidad con herramientas modernas (Docker, ORM, CI/CD).
- **Malo:** Curva de aprendizaje inicial mayor que MySQL.

### 0007-2-MySQL
- **Bueno:** Muy usado, fácil de configurar.
- **Malo:** Menor soporte para operaciones complejas (ej. subconsultas avanzadas).
- **Malo:** Peor rendimiento en concurrencia elevada (dependiendo del motor).

### 0007-3-SQL Server
- **Bueno:** Integración nativa con tecnologías Microsoft.
- **Malo:** Requiere licencia, menos flexible para contenedores.

---

## Decision Outcome

**Chosen option: "0007-1-PostgreSQL"**
Se selecciona PostgreSQL como gestor SQL principal para los microservicios debido a su fiabilidad, riqueza funcional, escalabilidad y soporte en entornos modernos de desarrollo distribuido. Se empleará el componente 17.5 (latest)

---

## Arquitectura propuesta

- Cada microservicio tendrá su **propia instancia de PostgreSQL**, siguiendo el patrón **Database-per-Service**.
- Se gestionará el acceso con un ORM compatible (Entity Framework, SQLAlchemy, Hibernate, etc.).
- Las conexiones estarán protegidas por autenticación por usuario y contraseña, y roles específicos por servicio.
- Se implementará el patrón Repository para desacoplar lógica de negocio y persistencia.

---
