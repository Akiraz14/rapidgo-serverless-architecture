### ADR-03 — Uso de Azure API Management como gateway de entrada sobre exposición directa de Azure Functions

#### Contexto

RapidGo requiere exponer un conjunto de endpoints para ser consumidos por aplicaciones móviles (clientes y repartidores), así como potencialmente por otros canales futuros (web, integraciones externas).

El sistema debe cumplir con:

* Gestión centralizada de autenticación y autorización
* Control de tráfico y protección ante abuso (rate limiting / throttling)
* Versionamiento de APIs sin afectar clientes existentes
* Monitoreo y trazabilidad de llamadas
* Posibilidad de exponer APIs públicas de forma controlada

Además, el backend está basado en Azure Functions, que por defecto permite exponer endpoints HTTP directamente.

---

#### Alternativas evaluadas

**1. Exposición directa de Azure Functions**

* Cada Function expone su endpoint HTTP directamente
* Se gestionan claves o autenticación a nivel de función

**Ventajas:**

* Menor complejidad inicial
* Menor latencia (menos saltos en la red)
* Configuración rápida para entornos simples

**Desventajas:**

* Falta de un punto central de control
* Dificultad para implementar políticas de seguridad consistentes
* Limitado control de versionamiento
* Mayor exposición a ataques si no se gestionan correctamente los endpoints
* Escalabilidad limitada en términos de gobernanza de API

---

**2. Azure API Management (API Gateway)**

* Actúa como punto de entrada único al sistema
* Centraliza políticas de seguridad, control de tráfico y versionamiento

**Ventajas:**

* Centralización de autenticación (JWT, OAuth, etc.)
* Implementación de rate limiting y protección ante abuso
* Versionamiento de APIs sin impactar consumidores
* Mayor control y visibilidad del tráfico
* Posibilidad de publicar APIs de forma segura

**Desventajas:**

* Incremento en complejidad arquitectónica
* Costos adicionales asociados al servicio
* Latencia adicional mínima debido a un salto adicional

---

#### Decisión

Se selecciona **Azure API Management** como gateway de entrada único para el sistema RapidGo.

Esta decisión se fundamenta en:

* La necesidad de centralizar la seguridad y control de acceso
* La importancia de proteger el backend ante tráfico no controlado
* La capacidad de gestionar versiones de la API sin afectar clientes existentes
* La proyección de crecimiento del sistema hacia múltiples canales de consumo

Azure API Management actuará como fachada del sistema, mientras que Azure Functions permanecerá desacoplado y protegido detrás del gateway.

---

#### Consecuencias

**Positivas:**

* Mayor control y seguridad en la exposición de la API
* Implementación uniforme de políticas de acceso
* Escalabilidad en la gestión de consumidores de la API
* Mejor observabilidad del sistema

**Negativas / Trade-offs:**

* Incremento en complejidad operativa
* Costos adicionales en comparación con exposición directa
* Ligera latencia adicional en cada solicitud

---

#### Estado

Aceptado
