### ADR-05 — Uso de Azure Notification Hubs sobre Azure Communication Services para notificaciones push

#### Contexto

RapidGo requiere enviar notificaciones en tiempo real a dispositivos móviles (Android e iOS) para informar cambios en el estado de los pedidos, tales como:

* Pedido confirmado
* Pedido en camino
* Pedido entregado

El sistema debe garantizar:

* Alta tasa de entrega de notificaciones (>95%)
* Integración con plataformas móviles (FCM y APNs)
* Escalabilidad para manejar múltiples dispositivos concurrentes
* Bajo costo operativo
* Integración sencilla con el backend serverless

---

#### Alternativas evaluadas

**1. Azure Communication Services (ACS)**

* Plataforma de comunicación multicanal (SMS, email, voz, chat)
* Permite enviar notificaciones a través de distintos canales

**Ventajas:**

* Soporte para múltiples canales (SMS, email, voz)
* API moderna y flexible
* Integración con escenarios de comunicación complejos

**Desventajas:**

* No está especializado en notificaciones push móviles
* Mayor complejidad para integración con FCM y APNs
* Costos más altos dependiendo del canal
* Overkill para el caso de uso específico

---

**2. Azure Notification Hubs**

* Servicio especializado en envío de notificaciones push
* Integración directa con FCM (Android) y APNs (iOS)

**Ventajas:**

* Optimizado específicamente para notificaciones push
* Alta escalabilidad para envío masivo
* Integración directa con FCM y APNs
* Bajo costo en comparación con soluciones multicanal
* Soporte para segmentación de dispositivos

**Desventajas:**

* Limitado únicamente a notificaciones push
* Menor flexibilidad para otros canales de comunicación
* Configuración inicial puede requerir integración con proveedores externos (Firebase, Apple)

---

#### Decisión

Se selecciona **Azure Notification Hubs** como servicio principal para el envío de notificaciones push en el sistema RapidGo.

Esta decisión se basa en:

* La necesidad específica de notificaciones push móviles
* La integración directa y optimizada con FCM y APNs
* La capacidad de escalar a un alto volumen de dispositivos
* La optimización de costos frente a soluciones más generales

Azure Notification Hubs permitirá desacoplar la lógica de notificación del resto del sistema, garantizando una entrega eficiente y confiable.

---

#### Consecuencias

**Positivas:**

* Alta eficiencia en el envío de notificaciones push
* Integración directa con plataformas móviles
* Escalabilidad para soportar crecimiento del sistema
* Reducción de costos operativos

**Negativas / Trade-offs:**

* Limitación a notificaciones push (no cubre otros canales como SMS o email)
* Dependencia de servicios externos como Firebase y APNs
* Posible necesidad de complementar con otros servicios en el futuro si se amplían los canales de comunicación

---

#### Estado

Aceptado
