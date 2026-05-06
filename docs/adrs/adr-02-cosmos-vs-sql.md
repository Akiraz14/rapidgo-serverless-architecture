### ADR-02 — Uso de Azure Cosmos DB sobre Azure SQL Database para la persistencia de pedidos

#### Contexto

RapidGo requiere un sistema de persistencia capaz de soportar un alto volumen de solicitudes concurrentes, con picos de hasta cientos de operaciones por segundo.
El modelo de datos incluye información de pedidos, estados de entrega y atributos variables dependiendo del tipo de negocio (restaurantes, tiendas, etc.).

Los requerimientos clave son:

* Alta escalabilidad horizontal
* Baja latencia en operaciones de lectura y escritura
* Flexibilidad en el esquema de datos
* Alta disponibilidad
* Modelo de costos alineado al uso
* Capacidad de manejar datos semi-estructurados

Además, existe una base de datos relacional existente (MySQL) con datos históricos, lo que implica evaluar el impacto de migración.

---

#### Alternativas evaluadas

**1. Azure SQL Database (Relacional)**

* Base de datos relacional administrada
* Soporte para transacciones ACID
* Esquema estructurado y normalizado

**Ventajas:**

* Consistencia fuerte
* Soporte para consultas complejas (JOINs)
* Familiaridad para equipos con experiencia en SQL
* Integración con herramientas tradicionales

**Desventajas:**

* Escalabilidad vertical limitada
* Mayor rigidez en el esquema
* Menor flexibilidad para atributos variables
* Puede requerir tuning manual para alto rendimiento

---

**2. Azure Cosmos DB (NoSQL)**

* Base de datos NoSQL distribuida globalmente
* Escalado horizontal automático mediante throughput (RU/s)
* Modelo basado en documentos (JSON)

**Ventajas:**

* Escalabilidad horizontal nativa
* Baja latencia garantizada (single-digit ms)
* Flexibilidad en el modelo de datos
* Alta disponibilidad (multi-región opcional)
* Integración natural con arquitecturas serverless

**Desventajas:**

* Modelo de consultas limitado comparado con SQL
* Necesidad de diseñar correctamente particiones
* Costos asociados a throughput (RU/s)
* Curva de aprendizaje para modelado NoSQL

---

#### Decisión

Se selecciona **Azure Cosmos DB** como sistema de persistencia principal para pedidos y estados del sistema.

Esta decisión se basa en:

* La necesidad de soportar cargas variables con escalado automático
* La naturaleza flexible de los datos de pedidos
* La integración nativa con Azure Functions
* La capacidad de garantizar baja latencia en operaciones críticas

El diseño del modelo de datos se orientará a documentos JSON, optimizando consultas por claves de acceso frecuentes (por ejemplo, `customerId` y `orderId`).

---

#### Consecuencias

**Positivas:**

* Alta escalabilidad sin necesidad de gestión manual
* Modelo de datos flexible adaptable a distintos tipos de negocio
* Baja latencia en operaciones críticas del sistema
* Reducción de complejidad en la infraestructura

**Negativas / Trade-offs:**

* Mayor responsabilidad en el diseño del modelo de datos (denormalización)
* Limitaciones en consultas complejas
* Dependencia del modelo de partición para el rendimiento
* Posible necesidad de estrategias adicionales para migración de datos históricos desde MySQL

---

#### Estado

Aceptado
