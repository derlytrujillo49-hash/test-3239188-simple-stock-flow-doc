# Project Glossary — Simple Stock Flow

> **Instructions:** Define here all technical and business terms used in the project.
> This is the official dictionary — if there is ambiguity, this document wins.
> Add terms throughout the project, not only at the start.

---

## How to use this glossary

1. Before using a technical or business term in code, docs, or conversations: look it up here.
2. If it's not there: add it with its definition.
3. If there is disagreement about the definition: discuss it as a team and update this document.

---

## Domain terms

| Term | Definition | Notes / Synonyms |
|------|-----------|-----------------|
| **Producto** | Artículo del catálogo con nombre, precio, stock, categoría e imagen opcional (§1). | No incluir atributos adicionales como SKU o descripción (DP-03). |
| **Categoría** | Clasificación fija de cinco opciones a la que pertenece un producto, sembrada desde la base y sin mantenimiento (§1 / D-10). | No posee un CRUD operativo para creación o borrado (§2.1). |
| **Precio** | Valor monetario vigente del producto en el catálogo. Debe ser estrictamente positivo (§1). | Representado por el objeto de valor `Money` (§2.2). |
| **Stock** | Unidades disponibles de un producto en el catálogo. Nunca puede ser negativo (§1 / §2.2). | Restricción impuesta por el motor mediante `ck_product_stock_non_negative` (§4). |
| **Imagen del producto** | Clave opaca del archivo binario almacenado de manera externa; su ausencia es `NULL` (§1 / D-08). | Evitar almacenar rutas relativas o bytes en crudo (§1). |


| **Venta** | Hecho comercial consumado e inmutable que registra quién, cuándo y qué se vendió. Una vez registrada no se edita ni se borra (§1 / §2.3). | No confundir con cotizaciones o estados intermedios. |
| **Línea de venta** | Renglón de la venta que guarda una copia congelada del producto, cantidad y precio del momento de la transacción (§1 / §2.4). | Objeto interno; no tiene vida independiente fuera de su venta (§2.4 / FK-2). |
| **Cantidad** | Unidades vendidas en una línea de venta. Debe ser estrictamente positiva (§1). | Encapsulado en el objeto de valor `Quantity` (§2.4). |
| **Usuario** | Operador interno que se autentica en la plataforma y registra las transacciones comerciales (§1 / §2.5). | No existe entidad cliente ni comprador final (§1). |
| **Rol** | Atribución del usuario restringida a un conjunto cerrado de dos opciones: `admin` o `seller` (§1 / §2.5). | El rol `admin` es provisionado por el entorno de despliegue y no en ejecución (§11.1). |

---

## Technical terms of the project

| Term | Definition |
|------|-----------|
| **Arquitectura Hexagonal** | Estilo arquitectónico (Puertos y Adaptadores) que aísla las reglas puras del dominio de los detalles de infraestructura y base de datos (§2.5 / §12). |
| **Raíz de Agregado** | Entidad principal del dominio que define los límites de consistencia y controla el acceso a sus componentes internos (§2.2 / §2.3). |
| **Nombre Congelado** | Copia literal de los atributos del producto en el instante de la venta para proteger el reporte histórico ante cambios del catálogo (§1 / ADR-004). |
| **Baja Lógica** | Mecanismo que marca un producto como inactivo mediante la columna `deleted_at` sin eliminar la fila física, preservando la integridad del histórico (§2.2 / ADR-003). |
| **Concurrencia Optimista** | Patrón para controlar modificaciones simultáneas en el motor de base de datos utilizando la columna de sistema `xmin` como testigo (§3 / D-04 / T-10). |
| **Propiedad Sombra** | Campo definido puramente en el mapeo de persistencia que no se expone en las entidades del dominio de la aplicación (ej. `deleted_at`, `xmin` — §2.2 / §3). |
| **Datos Semilla** | Información estática requerida para la operación inicial del sistema cargada directamente en la migración de la base de datos (§2.1 / §9.1). |
| **Valor Monomoneda** | Restricción del sistema que asume una única moneda operativa por construcción en todas las tablas y objetos financieros (§1 / D-05). |


---

## Acronyms

| Acronym | Meaning |
|---------|---------|
| **API** | Application Programming Interface (Interfaz de Programación de Aplicaciones) |
| **CRUD** | Create, Read, Update, Delete (Crear, Leer, Actualizar, Borrar) |
| **SDD** | Software Design Description (Descripción de Diseño de Software — §12) |
| **ADR** | Architecture Decision Record (Registro de Decisión de Arquitectura — §12) |
| **PR** | Pull Request (Solicitud de Extracción de Código) |
| **DoD** | Definition of Done (Definición de Terminado) |
| **DoR** | Definition of Ready (Definición de Listo) |
| **FK** | Foreign Key (Clave Foránea — §5) |
| **PK** | Primary Key (Clave Primaria — §4) |
| **UTC** | Coordinated Universal Time (Huso Horario Estándar del Servidor — §3 / §12) |
| **ORM** | Object-Relational Mapping (Mapeador de Objetos Relacionales — §0) |
| **UUID** | Universally Unique Identifier (Identificador Único Universal — §3) |
| **D-xx** | Decisión Técnica Específica del Proyecto (ej. D-05 — §1) |
| **T-xx** | Tarea Técnica Programada en el Backlog de Trabajo (ej. T-20 — §4) |

