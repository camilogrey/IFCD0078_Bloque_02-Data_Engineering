# Azure-Storage y Data Lake Gen2
# DP-900 - Lab 03 Ejercicio 1
# Laboratorio Práctico: Exploración de Azure Storage y Data Lake Gen2

Este repositorio contiene la documentación paso a paso y la evidencia visual del laboratorio práctico sobre arquitecturas de almacenamiento no relacional en Microsoft Azure.

---

## 📌 1. Objetivo del Laboratorio
El propósito de este ejercicio es aprovisionar y configurar una cuenta de Azure Storage para analizar las diferencias críticas de rendimiento, gobernanza y estructura entre un espacio de nombres plano (*Blob Storage*) y un espacio de nombres jerárquico (*Azure Data Lake Storage Gen2*), además de explorar los servicios de archivos compartidos (*Azure Files*).

---

## 🛠️ 2. Fase 1: Aprovisionamiento de la Cuenta de Almacenamiento

1. **Creación del Grupo de Recursos:** Se creó un contenedor lógico denominado `dp900-lab-rg` para centralizar los costes y la posterior eliminación del entorno.
2. **Configuración de la Cuenta:** Se parametrizó una cuenta con redundancia local (**LRS** - *Locally-redundant storage*) para optimizar los costes del laboratorio.
3. **Protección de Datos Crítica:** Se **desactivaron** todas las políticas de *Soft Delete* (eliminación suave) en la sección de *Recovery* para garantizar la compatibilidad con la futura actualización a Data Lake Gen2.

### Evidencia Visual
![Configuración Básica de Azure Storage](./imagenes/01_basics_config.png)
*Nota: Inserta aquí la captura de pantalla de la pestaña 'Basics' o del despliegue completado con éxito.*

---

## 📁 3. Fase 2: Exploración de Blob Storage (Espacio de Nombres Plano)

1. Se creó un contenedor de datos privado llamado `data`.
2. Se experimentó con la creación de un directorio virtual llamado `products`.
3. **Hallazgo Técnico:** Se comprobó el comportamiento de un *Flat Namespace*: al estar vacío el directorio virtual, este no se materializa de forma física en la raíz del contenedor, demostrando que las "carpetas" en Blob Storage son solo prefijos de texto en el nombre del objeto (`products/archivo.json`).
4. Se realizó la carga del archivo `product1.json` forzando la ruta virtual `product_data/`.

### Evidencia Visual
![Estructura de Contenedores y Carga de Blobs](./imagenes/02_blob_storage_flat.png)
*Nota: Inserta aquí la captura del Storage Browser mostrando la carpeta virtual con el archivo cargado.*

---

## 🚀 4. Fase 3: Actualización y Migración a Azure Data Lake Storage Gen2

1. Se navegó a la sección de configuración y se ejecutó la herramienta **Data Lake Gen2 upgrade**.
2. Al habilitar el **Espacio de Nombres Jerárquico** (*Hierarchical Namespace*), los prefijos virtuales se transformaron de manera inmediata en **directorios y carpetas reales de sistema de archivos**.
3. Se cargó el segundo archivo analítico (`product2.json`).
4. **Validación de Funcionalidades:** Al hacer clic en las propiedades de la carpeta, se desbloquearon características avanzadas ausentes en el Blob tradicional: la capacidad de renombrar directorios de forma segura y el control de accesos granulares mediante Listas de Control de Acceso de estilo Linux (**Manage ACL**).

### Evidencia Visual
![Habilitación de Data Lake Gen2 y Menú ACL](./imagenes/03_datalake_hierarchy.png)
*Nota: Inserta aquí la captura del menú contextual (...) de la carpeta donde se aprecian las opciones 'Rename' y 'Manage ACL' activas.*

---

## 🖧 5. Fase 4: Despliegue de Azure Files

1. Se configuró un recurso compartido clásico en la nube denominado `files` utilizando la categoría de rendimiento *Transaction optimized*.
2. Se desactivaron los respaldos (*Backup*) para mantener bajo el consumo de recursos de pruebas.
3. Se revisaron los scripts de conectividad nativos generados automáticamente por Azure para su montaje persistente mediante el protocolo **SMB (Puerto 445)** en sistemas Windows (PowerShell), Linux y macOS.

### Evidencia Visual
![Script de Conexión de Azure Files](./imagenes/04_azure_files_connect.png)
*Nota: Inserta aquí la captura de la pantalla 'Connect' mostrando el código de Powershell/Bash.*

---

## 🗑️ 6. Fase 5: Gobierno de Costes y Limpieza de Infraestructura

Para evitar cobros residuales en la suscripción de Azure, se procedió a la eliminación absoluta del grupo de recursos `dp900-lab-rg`, lo que destruye en bucle la cuenta de almacenamiento, los lagos de datos y los recursos compartidos generados de un solo golpe.

### Evidencia Visual
![Confirmación de Borrado del Grupo de Recursos](./imagenes/05_resource_cleanup.png)
*Nota: Inserta la captura del portal solicitando escribir el nombre del grupo de recursos para confirmar la eliminación.*

---

## 🧠 7. Conclusiones Técnicas del Ejercicio
* **Blob Storage** es óptimo y masivamente escalable para almacenar objetos binarios sueltos e independientes (imágenes, logs, vídeos) gracias a su indexación de nombres planos.
* **Data Lake Storage Gen2** es indispensable para entornos de analítica y Big Data (como clusters de Spark, Databricks o Synapse), ya que el espacio de nombres jerárquico permite realizar búsquedas y operaciones de renombrado masivo en milisegundos sin necesidad de reescribir los metadatos de miles de archivos individuales.