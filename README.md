# RapidGo — Serverless Backend Architecture on Azure

> Proyecto académico — Tecnológico de Antioquia (TdeA)
> Asignatura: Computación en la Nube — 2026-1

---

## 📌 1. Introducción

Este proyecto consiste en el **análisis, diseño e implementación de una arquitectura serverless en Microsoft Azure** para la empresa ficticia **RapidGo**, una startup colombiana de domicilios.

El objetivo principal es **resolver problemas reales de escalabilidad, disponibilidad y costos** mediante el uso de servicios cloud nativos, aplicando buenas prácticas de arquitectura como:

* Modelo C4 para documentación
* ADRs (Architectural Decision Records)
* Enfoque serverless (pago por uso)
* Desacoplamiento de componentes

La solución propuesta busca reemplazar un backend monolítico con una arquitectura moderna basada en servicios gestionados de Azure.

---

## 🎯 2. Objetivos del Proyecto

### Objetivo General

Diseñar e implementar un backend serverless escalable en Azure que soporte la operación de RapidGo.

### Objetivos Específicos

* Modelar la arquitectura usando el enfoque C4 (C1, C2, C3)
* Documentar decisiones arquitectónicas mediante ADRs
* Implementar un flujo funcional de pedidos
* Garantizar alineación con requerimientos del negocio
* Aplicar principios de arquitectura cloud moderna

---

## 🏢 3. Contexto del Problema

RapidGo es una plataforma de domicilios que presenta múltiples problemas en su arquitectura actual:

* Baja escalabilidad en horas pico
* Alto costo de infraestructura fija
* Caídas durante despliegues
* Notificaciones poco confiables
* Falta de tolerancia a fallos

Esto impacta directamente en sus ingresos y experiencia de usuario.

La solución propuesta utiliza **Azure Serverless** para:

* Escalar automáticamente
* Reducir costos operativos
* Mejorar la disponibilidad
* Implementar notificaciones en tiempo real

---

## 🧠 4. Modelo C4

### 4.1 C1 — Diagrama de Contexto

Describe el sistema como una caja negra, mostrando actores y sistemas externos.

*(Insertar imagen en `/assets/c1-context.jpg`)*

Descripción:
El sistema RapidGo interactúa con clientes, repartidores y administradores a través de una aplicación móvil, integrándose con servicios externos como pasarelas de pago y sistemas de notificación.

---

### 4.2 C2 — Diagrama de Contenedores

Describe los principales servicios que componen la arquitectura.

*(Insertar imagen en `/assets/c2-containers.jpg`)*

Contenedores principales:

* **API Management** → Gateway de entrada
* **Azure Functions** → Lógica de negocio
* **Cosmos DB** → Persistencia
* **Blob Storage** → Almacenamiento de archivos
* **Notification Hubs** → Notificaciones push

---

### 4.3 C3 — Diagrama de Componentes

Describe la estructura interna de Azure Functions.

*(Insertar imagen en `/assets/c3-components.jpg`)*

Componentes principales:

* `registrarPedido`
* `actualizarEstado`
* `consultarHistorial`
* `notificarCliente`

---

## 🧾 5. Decisiones Arquitectónicas (ADRs)

Se documentan las decisiones clave tomadas durante el diseño del sistema.

### ADR-01 — Uso de Azure Functions vs App Service

*(Pendiente)*

### ADR-02 — Uso de Cosmos DB vs Azure SQL

*(Pendiente)*

### ADR-03 — Uso de API Management vs exposición directa

*(Pendiente)*

### ADR-04 — Uso de Blob Storage vs Azure Files

*(Pendiente)*

### ADR-05 — Uso de Notification Hubs vs Azure Communication Services

*(Pendiente)*

---

## 🔄 6. Implementación del Flujo Crítico

Se implementa el flujo principal del sistema:

1. Cliente realiza un pedido (POST /pedidos)
2. Azure Functions procesa la solicitud
3. Se almacena el pedido en Cosmos DB
4. Se actualiza el estado del pedido
5. Se envía notificación al cliente

*(Se agregarán evidencias en la sección siguiente)*

---

## 📸 7. Evidencias de Implementación

*(Pendiente de completar)*

Se incluirán capturas de:

* Servicios desplegados en Azure
* Logs de ejecución
* Documentos en Cosmos DB
* Notificaciones enviadas
* Colección Postman

📁 Ver detalle en: `/assets/README.md`

---

## ⚙️ 8. Arquitectura Técnica (Implementación)

El código fuente del proyecto sigue principios de **Clean Architecture**, separando responsabilidades en distintas capas.

📁 Ver detalle en: `/src/README.md`

---

## 📂 9. Estructura del Repositorio

```
/
├── README.md        # Documentación principal
├── /src             # Código fuente
├── /assets          # Diagramas y evidencias
```

---

## 👥 10. Integrantes

* Nombre 1
* Nombre 2
* Nombre 3
* Juan Antonio Mira Zapata

---

## 📌 11. Conclusiones

*(Pendiente)*

Se analizarán los beneficios obtenidos con la arquitectura serverless, incluyendo mejoras en escalabilidad, disponibilidad y costos.

---

## 🚀 Estado del Proyecto

En desarrollo — construcción progresiva mediante commits documentados.
