# Assets — Diagramas y Evidencias

Este directorio contiene todos los recursos gráficos del proyecto **RapidGo Serverless Backend**, incluyendo diagramas del modelo C4 y evidencias de la implementación.

---

## 📌 Propósito

Centralizar los archivos visuales utilizados en la documentación del proyecto, permitiendo:

* Referenciar imágenes desde el `README.md` principal
* Mantener versiones editables de los diagramas
* Organizar evidencias de implementación en Azure

---

## 🧠 Diagramas de Arquitectura (Modelo C4)

Los diagramas fueron diseñados utilizando **draw.io (diagrams.net)**.

Cada diagrama se almacena en dos formatos:

* `.drawio` o `.xml` → versión editable
* `.jpg` o `.png` → versión exportada para documentación

---

### 📊 C1 — Diagrama de Contexto

| Archivo             | Descripción                   |
| ------------------- | ----------------------------- |
| `c1-context.drawio` | Archivo editable del diagrama |
| `c1-context.jpg`    | Imagen utilizada en el README |

---

### 🧩 C2 — Diagrama de Contenedores

| Archivo                | Descripción                   |
| ---------------------- | ----------------------------- |
| `c2-containers.drawio` | Archivo editable del diagrama |
| `c2-containers.jpg`    | Imagen utilizada en el README |

---

### ⚙️ C3 — Diagrama de Componentes

| Archivo                | Descripción                   |
| ---------------------- | ----------------------------- |
| `c3-components.drawio` | Archivo editable del diagrama |
| `c3-components.jpg`    | Imagen utilizada en el README |

---

## 📸 Evidencias de Implementación

En esta sección se almacenarán capturas de pantalla requeridas para la evaluación del proyecto.

Ejemplos:

* Despliegue de servicios en Azure
* Logs de ejecución de Azure Functions
* Documentos almacenados en Cosmos DB
* Envío de notificaciones push
* Pruebas realizadas con Postman

---

## 🗂️ Convención de nombres

Para mantener consistencia en el repositorio:

* Diagramas:
  `c1-context`, `c2-containers`, `c3-components`

* Evidencias:
  `evidence-<descripcion>.jpg`
  Ejemplo:

  * `evidence-functions-log.jpg`
  * `evidence-cosmos-document.jpg`

---

## 🔗 Referencia en documentación

Todos los archivos de este directorio son referenciados desde el `README.md` principal del proyecto.

Ejemplo:

```markdown
![C1 Diagram](./assets/c1-context.jpg)
```

---

## 🚀 Notas

* No eliminar archivos `.drawio` o `.xml`, ya que son necesarios para futuras modificaciones.
* Mantener sincronizadas las versiones editables y exportadas.
* Agregar nuevas evidencias a medida que se implemente el sistema.

---
