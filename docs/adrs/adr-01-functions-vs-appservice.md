### ADR-01 — Uso de Azure Functions sobre Azure App Service para la lógica de negocio

#### Contexto

RapidGo requiere una arquitectura backend capaz de manejar variaciones significativas en la carga de trabajo, con picos de hasta miles de solicitudes concurrentes durante horas de alta demanda.
La solución debe cumplir con los siguientes requerimientos:

* Escalabilidad automática sin intervención manual
* Modelo de costos basado en uso real
* Alta disponibilidad (99.9%)
* Despliegues sin downtime
* Baja carga operativa (equipo de infraestructura reducido)

Además, el equipo de desarrollo tiene experiencia en Node.js y Python, y el presupuesto inicial es limitado.

---

#### Alternativas evaluadas

**1. Azure App Service**

* Plataforma PaaS para aplicaciones web
* Escalado manual o automático basado en instancias
* Costos fijos asociados a instancias activas

**Ventajas:**

* Mayor control sobre el entorno de ejecución
* Soporte para aplicaciones monolíticas o APIs tradicionales
* Integración sencilla con pipelines CI/CD

**Desventajas:**

* Escalado menos granular
* Costos constantes incluso en baja demanda
* Requiere gestión de capacidad

---

**2. Azure Functions (Serverless)**

* Modelo serverless basado en ejecución por eventos
* Escalado automático desde 0 instancias
* Facturación basada en número de ejecuciones

**Ventajas:**

* Escalabilidad automática y granular
* Modelo de pago por uso
* Reducción significativa de costos en baja demanda
* Integración nativa con otros servicios Azure

**Desventajas:**

* Mayor complejidad en diseño (arquitectura distribuida)
* Cold start en ciertas condiciones
* Limitaciones en ejecución prolongada

---

#### Decisión

Se selecciona **Azure Functions** como plataforma principal para la implementación de la lógica de negocio del sistema RapidGo.

Esta decisión se fundamenta en la necesidad de:

* Escalar automáticamente ante picos de tráfico sin intervención manual
* Optimizar costos operativos mediante un modelo pay-per-use
* Reducir la carga operativa del equipo de infraestructura
* Facilitar la integración con servicios como Cosmos DB, Notification Hubs y Blob Storage

---

#### Consecuencias

**Positivas:**

* Escalabilidad automática sin necesidad de aprovisionamiento previo
* Reducción de costos en periodos de baja demanda
* Arquitectura altamente desacoplada y orientada a eventos
* Alta integración con el ecosistema Azure

**Negativas / Trade-offs:**

* Mayor complejidad en el diseño del sistema
* Necesidad de gestionar problemas como cold start
* Requiere un enfoque distinto al desarrollo tradicional (stateless, event-driven)

---

#### Estado

Aceptado
