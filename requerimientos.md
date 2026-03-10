# Especificación de Requerimientos: Software de Formulación Cosmética (CosmeticLab)

## 1. Introducción
Este documento define los requisitos para un software especializado en la formulación y creación de productos cosméticos. El objetivo es automatizar el cálculo de porcentajes, asegurar la estabilidad química de las mezclas y profesionalizar la gestión de laboratorios artesanales o industriales mediante una interfaz técnica e intuitiva.

---

## 2. Stack Tecnológico Sugerido
* **Lenguaje:** Python 3.10+ (Motor de cálculo y lógica de negocio).
* **Interfaz:** [NiceGUI](https://nicegui.io/) (Framework de UI basado en web para Python, ideal para dashboards técnicos).
* **Base de Datos:** SQLite (Almacenamiento local ligero para ingredientes, fórmulas y lotes).
* **Librerías Adicionales:** * `Pandas` (Para manipulación de tablas de datos).
    * `ReportLab` o `FPDF2` (Para generación de fichas técnicas en PDF).

---

## 3. Requerimientos Funcionales (RF)

### RF1 - Gestión de Materias Primas (Inventario)
El sistema debe permitir el CRUD (Crear, Leer, Actualizar, Borrar) de ingredientes con los siguientes atributos:
* **Nombre Comercial e INCI:** Identificación técnica del ingrediente.
* **Función:** Clasificación (Emulsionante, Activo, Conservante, Quelante, etc.).
* **Dosis de Uso:** Rango permitido (mínimo y máximo %) según ficha técnica.
* **Solubilidad:** Hidrosoluble, Liposoluble o Alcohosoluble (para validación de fases).
* **Costo por Unidad:** Para cálculo automático de costo de producción.

### RF2 - Calculadora de Fórmulas Dinámica
* **Ajuste Base 100:** El software debe permitir ingresar porcentajes y calcular automáticamente los gramos en base a un peso total deseado (ej. 500g, 1kg).
* **Gestión de Fases:** Organización de la fórmula en:
    * **Fase A:** Acuosa (resistente al calor).
    * **Fase B:** Oleosa (resistente al calor).
    * **Fase C:** Termolábiles (activos y conservantes que se añaden en frío).
* **Validación de Totales:** Alerta visual si la suma de los ingredientes no es igual al 100.00%.

### RF3 - Control de pH y Estabilidad Química
* **Registro de pH:** Campo para anotar el pH medido del lote final.
* **Alertas de Incompatibilidad:** Notificar si el pH final está fuera del rango de actividad de los conservantes o activos seleccionados en la fórmula.
* **Sensibilidad Térmica:** Advertir si un ingrediente de la "Fase C" se está asignando a una fase de calentamiento.

### RF4 - Historial de Lotes y Trazabilidad
* **Generación de Lotes:** Crear una entrada de "Producción" que descuente automáticamente el stock de materias primas.
* **Fecha de Expiración:** Cálculo automático de la fecha de consumo preferente basado en el ingrediente con menor vida útil.

---

## 4. Requerimientos No Funcionales (RNF)

* **RNF1 - Usabilidad (Reactividad):** La interfaz en NiceGUI debe actualizar los gramos y costos en tiempo real mientras el usuario modifica los porcentajes (sin recargar la página).
* **RNF2 - Portabilidad:** El sistema debe ser capaz de compilarse como un ejecutable (.exe) para funcionar de forma local en el laboratorio sin conexión a internet.
* **RNF3 - Integridad de Datos:** Validaciones estrictas para asegurar que solo se ingresen números en los campos de porcentaje y gramos.

---

## 5. Diseño de la Base de Datos (Esquema Sugerido)

### Tabla: `ingredientes`
| Campo | Tipo | Descripción |
| :--- | :--- | :--- |
| `id` | INTEGER (PK) | Identificador único. |
| `nombre_inci` | TEXT | Nombre técnico internacional. |
| `pct_min` | FLOAT | Límite inferior de uso. |
| `pct_max` | FLOAT | Límite superior de uso. |
| `solubilidad` | TEXT | Agua / Aceite / Alcohol. |
| `precio_gramo`| FLOAT | Costo para el cálculo de presupuesto. |

### Tabla: `formulas`
| Campo | Tipo | Descripción |
| :--- | :--- | :--- |
| `id` | INTEGER (PK) | Identificador de la receta. |
| `nombre` | TEXT | Nombre del producto (ej. Crema Hidratante). |
| `instrucciones`| TEXT | Notas de elaboración paso a paso. |

---

## 6. Próximos Pasos de Desarrollo
1.  **Prototipo de UI:** Crear la tabla de entrada de datos con `ui.table` de NiceGUI.
2.  **Motor de Cálculo:** Programar la función en Python que reciba el peso total y devuelva los gramos proporcionales.
3.  **Persistencia:** Implementar la conexión con SQLite usando `sqlite3` o `SQLAlchemy`.
