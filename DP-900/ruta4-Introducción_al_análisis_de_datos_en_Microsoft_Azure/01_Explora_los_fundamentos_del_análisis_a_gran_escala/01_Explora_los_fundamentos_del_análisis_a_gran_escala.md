te subiré un fichero .docx por ejemplo "01Explora_los_fundamentos_del_análisis_a_gran_escala.docx"  contiene texto e imagénes, y formato de Microsoft Learn.
Se corresponde con el fichero "Introducción_al_análisis_de_datos_en_Microsoft_Azure.md" que me será el que devolverás creado con los parámetros que te indico mas adelante.

Parámetros:
adoptada como estructura oficial del proyecto DP-900 y la utilizarás en todas las conversiones futuras.

DP-900/
│
├── README.md
├── GUIA_DE_ESTILO.md
│
├── 00_Ruta_Introducción_al_análisis_de_datos_en_Microsoft_Azure/
│   └── Introducción_al_análisis_de_datos_en_Microsoft_Azure.md
│
├── 01_Analisis_a_gran_escala/
│   ├── 01_Explora_los_fundamentos_del_análisis_a_gran_escala.md
│   └── imagenes/
│
├── 02_Analisis_en_tiempo_real/
│   ├── 02_Explora_los_fundamentos_del_análisis_en_tiempo_real.md
│   └── imagenes/
│
├── 03_Visualizacion_de_datos/
│   ├── 03_Explora_los_fundamentos_de_la_visualización_de_datos.md
│   └── imagenes/
│
└── assets/

📄 Un único archivo .md por módulo.
📁 Una carpeta imagenes/ dentro de cada módulo.
🌍 Traducción fiel al español, manteniendo en inglés los nombres oficiales de productos, servicios y tecnologías (por ejemplo: Microsoft Fabric, Data Lakehouse, Apache Spark, OneLake...).

🖼️ Conversión automática de las imágenes al formato:

![Texto alternativo](imagenes/nombre_imagen.png)
📑 Índice con enlaces internos al inicio del documento.
💡 Bloques de Microsoft Learn convertidos a > [!NOTE], > [!TIP], > [!IMPORTANT], etc.
📊 Tablas optimizadas para Markdown y GitHub.
💻 Bloques de código con el lenguaje correspondiente.
🔗 Enlaces relativos compatibles con GitHub, Docsify y GitBook.
🧹 Limpieza del formato procedente de Word (espacios, listas, puntuación...), sin alterar el contenido técnico.
Además, aplicaré una convención de trabajo

Cuando me envíes un .docx, no tendrás que volver a indicarme las reglas. Asumiré automáticamente esta estructura y estas convenciones, de modo que todas las entregas mantengan el mismo estilo y sean coherentes entre sí.

Ejemplo de fichero markdown
# Explore Azure Cosmos DB

## 1. Objetivo del Laboratorio

El objetivo principal de este laboratorio es aprender a:

- Aprovisionar una cuenta de **Azure Cosmos DB** utilizando la API para NoSQL.
- Interactuar con documentos en formato **JSON**.
- Ejecutar consultas con sintaxis similar a **SQL** mediante **Data Explorer** en el Portal de Azure.

---

## 2. Resumen del ejercicio

El laboratorio consta de las siguientes fases:

### 2.1 Aprovisionamiento NoSQL

Creación de una cuenta de **Azure Cosmos DB** configurando:

- Tipo de carga de trabajo: **Learning**
- API: **NoSQL**

### 2.2 Generación automática de datos de ejemplo

Uso de **Launch Quick Start** para crear automáticamente:

- Base de datos: `SampleDB`
- Contenedor: `SampleContainer`
- Datos de ejemplo

### 2.3 Manipulación de documentos JSON

Creación de un nuevo documento JSON para comprobar el funcionamiento de una base de datos **schema-free**.

### 2.4 Ejecución de consultas

Ejecución de consultas mediante una sintaxis similar a SQL utilizando funciones como:

```sql
SELECT *
FROM c
WHERE CONTAINS(c.name, "Helmet")
```

### 2.5 Gobierno de costes

Eliminación del grupo de recursos para evitar costes adicionales.

---

# 3. Aspectos importantes

> [!IMPORTANT]
> La API seleccionada durante la creación de la cuenta **no puede modificarse posteriormente**.

> [!IMPORTANT]
> La **Partition Key** es la decisión de diseño más importante en Azure Cosmos DB.

> [!TIP]
> Cosmos DB añade automáticamente propiedades del sistema como:

- `_rid`
- `_self`
- `_etag`
- `_ts`

> [!TIP]
> Selecciona el tipo de carga **Learning** para minimizar el consumo de créditos durante el laboratorio.

---

# 4. Ejecución

En este laboratorio aprenderás a crear tu primera base de datos NoSQL utilizando Azure Cosmos DB.

Duración aproximada:

**30 minutos**

---

## 4.1 Antes de empezar

Necesitarás:

- Una suscripción de Azure.
- Permisos administrativos.
- Acceso al Portal de Azure.

---

## 4.2 Crear una cuenta de Azure Cosmos DB

1. En el Portal de Azure selecciona **Crear un recurso**.
2. Busca **Azure Cosmos DB**.
3. Selecciona **Crear**.

### Configuración

| Parámetro | Valor |
|-----------|-------|
| Tipo de carga | Learning |
| API | NoSQL |
| Modo de capacidad | Rendimiento provisionado |
| Zonas de disponibilidad | Desactivadas |
| Nivel gratuito | Solicitar si está disponible |

> [!NOTE]
> El nombre de la cuenta debe ser único en todo Azure.