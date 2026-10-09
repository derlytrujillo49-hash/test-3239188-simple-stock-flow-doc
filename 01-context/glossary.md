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

