### ADR-04 — Uso de Azure Blob Storage sobre Azure Files para el almacenamiento de archivos

#### Contexto

RapidGo requiere almacenar archivos asociados a pedidos, tales como:

* Comprobantes de entrega (imágenes)
* Imágenes de productos
* Archivos generados para reportes operacionales

Estos archivos deben ser accesibles desde el backend, escalables y con un costo optimizado.

RapidGo opera bajo una arquitectura serverless basada en Azure Functions y requiere almacenar imágenes y documentos asociados al flujo de pedidos.

Los requerimientos clave incluyen:

* Alta escalabilidad
* Acceso eficiente a archivos desde servicios serverless
* Optimización de costos
* Soporte para almacenamiento de objetos binarios (imágenes, documentos)
* Integración con Azure Functions

---

#### Alternativas evaluadas

**1. Azure Files**

* Sistema de archivos compartido en la nube (SMB/NFS)
* Proporciona un sistema de archivos compartido similar a un entorno tradicional

**Ventajas:**

* Compatible con aplicaciones legacy
* Montaje directo como unidad de red
* Familiar para entornos tradicionales

**Desventajas:**

* Menor eficiencia para acceso desde arquitecturas serverless
* Escalabilidad limitada en comparación con almacenamiento de objetos
* Mayor costo relativo para grandes volúmenes de archivos
* No optimizado para acceso vía HTTP

---

**2. Azure Blob Storage**

* Servicio de almacenamiento de objetos altamente escalable
* Optimizado para datos no estructurados (imágenes, documentos, videos)

**Ventajas:**

* Escalabilidad altamente elástica y preparada para grandes volúmenes de datos
* Costos optimizados para almacenamiento masivo
* Acceso directo vía HTTP/HTTPS
* Integración nativa con Azure Functions
* Soporte para distintos tiers de almacenamiento (Hot, Cool, Archive)

**Desventajas:**

* No es compatible con sistemas de archivos tradicionales
* Requiere manejo explícito de rutas y contenedores
* No soporta operaciones tipo file system (como locking tradicional)

---

#### Decisión

Se selecciona **Azure Blob Storage** como mecanismo principal de almacenamiento de archivos en el sistema RapidGo.

Esta decisión se fundamenta en:

* La necesidad de almacenar archivos binarios de forma eficiente
* La integración directa con servicios serverless como Azure Functions
* La optimización de costos para almacenamiento a gran escala
* La simplicidad en el acceso vía HTTP para clientes y servicios. Además, Azure Blob Storage corresponde al servicio recomendado en la arquitectura de referencia oficial del proyecto para el almacenamiento de imágenes y reportes operacionales

Para minimizar costos durante la fase piloto se utilizará el tier [Standard LRS](../glossary.md)

---

#### Consecuencias

**Positivas:**

* Alta escalabilidad sin necesidad de gestión manual
* Costos optimizados para almacenamiento de archivos
* Integración sencilla con el backend serverless
* Acceso eficiente a través de endpoints HTTP

**Negativas / Trade-offs:**

* Mayor complejidad en la gestión de rutas y organización de archivos
* No es adecuado para aplicaciones que requieren un sistema de archivos tradicional
* El acceso seguro a los archivos deberá gestionarse mediante [SAS Tokens temporales](../glossary.md) y control de acceso basado en roles ([RBAC](../glossary.md))

---

#### Estado

Aceptado
