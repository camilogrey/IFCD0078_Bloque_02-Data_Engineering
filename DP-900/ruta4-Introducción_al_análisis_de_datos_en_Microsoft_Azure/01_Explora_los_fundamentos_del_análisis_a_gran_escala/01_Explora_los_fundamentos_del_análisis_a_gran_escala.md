Explora los fundamentos del análisis a gran escala 

## Índice

* [1. Introducción](https://www.google.com/search?q=%231-introducci%C3%B3n)
* [2. Análisis a gran escala](https://www.google.com/search?q=%232-an%C3%A1lisis-a-gran-escala)
* [3. Arquitectura del almacenamiento de datos](https://www.google.com/search?q=%233-arquitectura-del-almacenamiento-de-datos)
* [4. La pila de análisis moderna en Azure](https://www.google.com/search?q=%234-la-pila-de-an%C3%A1lisis-moderna-en-azure)
* [5. Explorar los flujos de ingesta de datos](https://www.google.com/search?q=%235-explorar-los-flujos-de-ingesta-de-datos)
* [6. Explorar almacenes de datos analíticos](https://www.google.com/search?q=%236-explorar-almacenes-de-datos-anal%C3%ADticos)
* [7. Ejercicio: Explora el análisis de datos en Microsoft Fabric](https://www.google.com/search?q=%237-ejercicio-explora-el-an%C3%A1lisis-de-datos-en-microsoft-fabric)
* [8. Verificación de conocimientos](https://www.google.com/search?q=%238-verificaci%C3%B3n-de-conocimientos)
* [9. Resumen](https://www.google.com/search?q=%239-resumen)

---

1. Introducción 

Las organizaciones utilizan plataformas analíticas para crear soluciones de análisis de datos a gran escala que generan información valiosa e impulsan el éxito. Microsoft ofrece diversas tecnologías que se pueden combinar para crear una solución de análisis de datos a gran escala.

En este módulo aprenderás a:

* Describir la arquitectura del almacenamiento de datos.


* Describir las características clave de los flujos de ingesta de datos.


* Identificar los tipos comunes de almacenes de datos analíticos y los servicios de Azure relacionados.


* Utilizar Microsoft Fabric para recopilar y analizar datos.



Antes de explorar cómo se construyen las soluciones de análisis a gran escala, presentemos las dos principales plataformas de análisis en Azure que encontrará a lo largo de este módulo:

* 
**Microsoft Fabric:** Es la plataforma unificada de análisis de software como servicio (SaaS) de Microsoft. Integra ingeniería de datos, almacenamiento de datos, análisis en tiempo real, ciencia de datos y Power BI en un único espacio de trabajo basado en navegador, sobre una capa de almacenamiento compartido llamada OneLake. No es necesario administrar servidores ni clústeres: se crean espacios de trabajo y elementos, y Microsoft gestiona la infraestructura.


* 
**Azure Databricks:** Es una plataforma de análisis en la nube basada en Apache Spark. Está optimizada para la ingeniería de datos, la ciencia de datos y el análisis SQL a gran escala mediante formatos abiertos de almacenamiento en la nube (Lakehouse). Utiliza Delta Lake, un formato de almacenamiento de código abierto que añade transacciones, aplicación de esquemas y control de versiones a los archivos Parquet, lo que proporciona una fiabilidad similar a la de Lakehouse a gran escala. Databricks se ejecuta como un servicio gestionado dentro de su suscripción de Azure y es una opción común para equipos que necesitan flujos de trabajo basados en Spark y notebooks con enfoque en código.



> [!NOTE]
> Reconocemos que cada persona aprende de manera diferente. Puedes completar este módulo en formato de video o leer el contenido en formato de texto e imágenes.
> El texto contiene más detalles que los videos, por lo que en algunos casos te resultará útil como material complementario a la presentación en video.
> 
> 

---

2. Análisis a gran escala 

Las soluciones de análisis de datos a gran escala combinan el almacenamiento de datos convencional, utilizado para respaldar la inteligencia empresarial (BI), con técnicas empleadas en el análisis de macrodatos (big data).

* Una solución de **almacenamiento de datos** convencional generalmente implica copiar datos de almacenes de datos transaccionales a una base de datos relacional con un esquema optimizado para consultas y la creación de modelos multidimensionales.


* Las soluciones de **procesamiento de macrodatos** se utilizan con grandes volúmenes de datos en múltiples formatos, que se cargan por lotes o se capturan en tiempo real y se almacenan en un lago de datos desde donde motores de procesamiento distribuido como Apache Spark los procesan.





La combinación de almacenamiento flexible de lagos de datos y análisis SQL de almacenes de datos ha dado lugar al surgimiento de un diseño de análisis a gran escala, a menudo denominado "**data lakehouse**".

---

3. Arquitectura del almacenamiento de datos 

La arquitectura de análisis de datos a gran escala puede variar, al igual que las tecnologías específicas utilizadas para implementarla; pero, en general, incluye los siguientes elementos:



1. 
**Ingesta y procesamiento de datos:** Los datos de uno o más almacenes de datos transaccionales, archivos, flujos en tiempo real u otras fuentes se cargan en un lago de datos o un almacén de datos relacional. La operación de carga generalmente implica un proceso de extracción, transformación y carga (ETL) o extracción, carga y transformación (ELT) en el que los datos se limpian, filtran y reestructuran para su análisis. En los procesos ETL, los datos se transforman antes de cargarse en un almacén analítico, mientras que en un proceso ELT los datos se copian al almacén y luego se transforman. En ambos casos, la estructura de datos resultante está optimizada para consultas analíticas. El procesamiento de datos a menudo se realiza mediante sistemas distribuidos que pueden procesar grandes volúmenes de datos en paralelo utilizando clústeres de múltiples nodos. La ingesta de datos incluye tanto el procesamiento por lotes de datos estáticos como el procesamiento en tiempo real de datos en flujo.


2. 
**Almacenamiento de datos analíticos:** Los almacenes de datos para análisis a gran escala include almacenes de datos relacionales, lagos de datos basados en sistemas de archivos y arquitecturas híbridas que combinan características de almacenes de datos y lagos de datos (a veces denominados bases de datos de lagos de datos o data lakehouses).


3. 
**Modelo de datos analítico:** Si bien los analistas y científicos de datos pueden trabajar directamente con los datos en el almacén de datos analítico, es común crear uno o más modelos de datos para facilitar la generación de informes, paneles y visualizaciones interactivas. Históricamente, estos modelos se describían como cubos, en los que los valores de datos numéricos se preagregaban en una o más dimensiones utilizando tecnologías como SQL Server Analysis Services (SSAS) Multidimensional. Actualmente, el enfoque preferido es un **modelo semántico**, el modelo tabular que sustenta Power BI y Microsoft Fabric. Un modelo semántico define tablas, relaciones, jerarquías y medidas escritas en DAX (Data Analysis Expressions), con agregaciones calculadas en el momento de la consulta en lugar de almacenarse con antelación. En Microsoft Fabric, los modelos semánticos pueden usar el modo Direct Lake para leer tablas Delta de OneLake directamente, sin importar ni preagregar datos.


4. 
**Visualización de datos:** Los analistas de datos consumen datos de modelos analíticos y directamente de almacenes de datos para crear informes, paneles y otras visualizaciones. Además, los usuarios de una organización que no sean profesionales de la tecnología pueden realizar análisis y generar informes de datos de forma autónoma. Las visualizaciones de los datos muestran tendencias, comparaciones e indicadores clave de rendimiento (KPI) para una empresa u organización.


5. 
**Análisis asistido por IA:** Las organizaciones amplían cada vez más su conjunto de herramientas analíticas con capacidades de lenguaje natural e inteligencia artificial. En Power BI, la función de preguntas y respuestas permite a los usuarios escribir preguntas en lenguaje natural y recibir respuestas en forma de gráficos. Copilot en Microsoft Fabric permite a los usuarios describir lo que desean en lenguaje natural para generar medidas DAX, escribir consultas SQL o resumir informes. En Azure Databricks, Genie ofrece una funcionalidad similar sobre los datos de Delta Lake.



---

4. La pila de análisis moderna en Azure 

Azure ofrece de forma principal dos plataformas líderes para el análisis a gran escala que admiten la arquitectura completa:

Microsoft Fabric 

Microsoft Fabric organiza todas estas capas en un único espacio de trabajo SaaS unificado. El almacenamiento lo proporciona OneLake y los servicios leen y escriben directamente usando Delta Lake como formato estándar de código abierto. Dentro de un espacio de trabajo de Fabric, los componentes principales se corresponden con las capas superiores:

* 
**Fabric Lakehouse:** Combina el almacenamiento de lago de datos con consultas SQL expuestas a través de un punto final de análisis SQL.


* 
**Fabric Warehouse:** Un almacén de datos relacional totalmente administrado y compatible con SQL Server para análisis estructurados con estricta aplicación del esquema.


* 
**Fabric Data Factory:** Crea y programa flujos de trabajo y transformaciones de bajo código para la ingesta y el movimiento de datos.


* 
**Power BI:** Ofrece informes, paneles y modelos semánticos con el modo Direct Lake para leer tablas de OneLake de forma directa.



Azure Databricks 

Plataforma de análisis en la nube basada en Apache Spark, optimizada para la ingeniería de datos a gran escala, la ciencia de datos y el análisis SQL, que se ejecuta de forma gestionada y utiliza Delta Lake de forma nativa. Sus componentes principales son:

* 
**Databricks Lakehouse:** Capa de almacenamiento unificada que admite análisis SQL y aprendizaje automático en el mismo entorno.


* 
**Databricks SQL:** Almacén de datos SQL sin servidor para ejecutar consultas analíticas sobre tablas Delta, con paneles y alertas integrados.


* 
**Cuadernos de Databricks:** Cuadernos colaborativos de Python, SQL, Scala y R para flujos de trabajo e ingeniería de datos.


* 
**Unity Catalog:** Capa de gobernanza unificada para datos y activos de IA que proporciona control de acceso detallado y linaje.


* 
**Genie:** Interfaz conversacional impulsada por IA que permite formular preguntas en lenguaje natural y genera el SQL de forma automática.



---

5. Explorar los flujos de ingesta de datos 

La ingesta de datos transporta la información desde una o más fuentes hacia un almacén de datos analíticos.



Microsoft Fabric 

Ofrece múltiples formas de incorporar datos a OneLake, desde ETL basado en canalizaciones hasta federación sin copia y transmisión en tiempo real.

* 
**Fabric Data Factory:** Punto de partida recomendado para la ingesta basada en canalizaciones, ofreciendo:


* 
*Pipelines:* Orquestan flujos de trabajo de transformación y movimiento de datos en varios pasos encadenando actividades. Utilizan servicios vinculados para conectarse a orígenes y destinos.


* 
*Dataflows Gen2:* Experiencia visual y de bajo código para crear lógica de transformación mediante Power Query.




* 
**Atajos de OneLake (Shortcuts):** Referencia en tiempo real al almacenamiento externo (ADLS Gen2, Amazon S3, Google Cloud Storage) que hace aparecer los datos externos como si ya estuvieran en su Lakehouse sin necesidad de copiarlos o duplicarlos.


* 
**Reflejo (Mirroring):** Replica continuamente una base de datos externa (Azure SQL Database, Snowflake, Azure Cosmos DB) directamente en OneLake prácticamente en tiempo real y en formato Delta Lake sin crear canalizaciones.


* 
**Eventstream:** Para la ingesta de datos en tiempo real, se conecta a fuentes de eventos como Azure Event Hubs o Apache Kafka para enrutar, filtrar y transformar eventos en flujo.


* 
**Cuadernos de Fabric (Notebooks):** Opción orientada a código mediante Apache Spark para cuando no existe un conector integrado o se requiere lógica personalizada en PySpark, Python, Scala, R o SQL.



Azure Data Factory 

Servicio independiente de Azure para crear canalizaciones de integración de datos fuera de Fabric, por ejemplo, cuando el destino es Azure SQL Database o fuentes de datos locales en entornos híbridos. Utiliza el mismo modelo de servicios vinculados que Fabric Data Factory.

Azure Databricks 

* 
**Tuberías declarativas de Lakeflow:** Marco declarativo para crear pipelines fiables y con actualizaciones incrementales en Delta Lake, gestionando dependencias y orden de ejecución de forma automática.


* 
**Cuadernos de Databricks:** Compatibles con múltiples lenguajes e ideales para tareas de ingesta de datos puntuales o exploratorias, con capacidad de programarse como tareas.



> [!NOTE]
> Azure Databricks ofrece mecanismos de ingesta especializados adicionales a los que se describen aquí.
> 
> 

---

6. Explorar almacenes de datos analíticos 

Existen dos tipos comunes de almacenamiento de datos analíticos junto con los enfoques híbridos.

Almacenes de datos (Data Warehouses) 

Base de datos relacional donde los datos se almacenan en un esquema optimizado para el análisis de datos. Generalmente, los datos transaccionales se transforman en un esquema donde los valores numéricos se guardan en **tablas de hechos** centrales, relacionadas con una o más **tablas de dimensiones**.

* 
**Esquema de estrella (Star Schema):** Modelo donde las tablas de dimensiones se conectan directamente a la tabla de hechos.


* 
**Esquema de copo de nieve (Snowflake Schema):** Extensión del esquema de estrella que agrega tablas adicionales relacionadas con las tablas de dimensiones para representar jerarquías.





Lagos de datos (Data Lakes) 

Un repositorio de archivos, generalmente en un sistema de archivos distribuido, para un acceso a datos de alto rendimiento. Utiliza motores como Apache Spark para procesar consultas aplicando un enfoque de **esquema en lectura** (schema-on-read) en el momento en que se leen los datos para su análisis, sin imponer restricciones al almacenarlos.



Enfoques híbridos (Data Lakehouses) 

Combina características de lagos de datos y almacenes de datos. Los datos sin procesar se almacenan como archivos en un lago de datos, y un punto de conexión de análisis SQL expone esos archivos como tablas relacionales. Se habilitan mediante **Delta Lake**, el cual agrega capacidades relacionales sobre archivos Parquet (imposición de esquemas, coherencia transaccional y API SQL).



Servicios de Azure para almacenes analíticos 

| Servicio | Tipo de Experiencia / Componente | Caso de Uso Común |
| --- | --- | --- |
| <br>**Microsoft Fabric** 

 | <br>**Fabric Lakehouse** 

 | Ideal para trabajar con datos mixtos o semiestructurados y flujos de aprendizaje automático con Spark.

 |
|  | <br>**Fabric Warehouse** 

 | Ideal cuando los datos están estructurados, el equipo escribe SQL y se necesita sólida aplicación del esquema.

 |
| <br>**Azure Databricks** 

 | <br>**Databricks SQL Warehouse** 

 | Punto de conexión SQL dedicado optimizado para herramientas de BI y cargas de trabajo concurrentes basados en código Spark.

 |

> [!NOTE]
> Cada uno de estos servicios puede considerarse un almacén de datos analíticos, ya que proporciona un esquema y una interfaz para consultar los datos. Algunas soluciones combinan ambos servicios: un proceso de ingesta ELT podría copiar los datos en el lago de datos, usar un cuaderno en Azure Databricks para procesar un gran volumen de datos y, finalmente, cargar los resultados en tablas de Microsoft Fabric Warehouse.
> 
> 

---

7. Ejercicio: Explora el análisis de datos en Microsoft Fabric 

⏱️ **Duración aproximada:** 30 minutos 

En este ejercicio, crearás un repositorio de datos de Microsoft Fabric y lo usarás para ingerir y analizar algunos datos. Este ejercicio está diseñado para familiarizarte con algunos elementos clave de una solución de análisis de datos a gran escala, no como una guía completa para realizar análisis de datos avanzados con Microsoft Fabric.

> [!NOTE]
> Para completar este laboratorio, necesitarás una cuenta de Microsoft Fabric o, si aún no tienes una, regístrate para una prueba gratuita.
> 
> 

Inicie el ejercicio y siga las instrucciones.

 *(Enlace del recurso web)* 

---

8. Verificación de conocimientos 

⏱️ **Tiempo estimado:** 3 minutos | 🏆 **Puntuación:** 200 XP 

1. ¿Qué término se utiliza para describir una solución analítica que combina la funcionalidad de un lago de datos y un almacén de datos relacional? 

* [ ] A) Canalización de datos 


* [x] B) Data lakehouse 


* [ ] C) Nube 



2. ¿Qué solución de análisis SaaS puede utilizar para crear un flujo de trabajo para la ingesta y el procesamiento de datos? 

* [ ] A) Azure SQL Database y Azure Cosmos DB 


* [x] B) Microsoft Fabric 


* [ ] C) Azure Cosmos DB 



3. ¿Qué motor de procesamiento distribuido de código abierto incluye Microsoft Fabric? 

* [ ] A) Apache Hadoop 


* [x] B) Apache Spark 


* [ ] C) Tormenta Apache 



---

9. Resumen 

El almacenamiento de datos a gran escala es una tarea compleja que puede involucrar diversas tecnologías. Este módulo ofreció una visión general de las características clave de una solución moderna de almacenamiento de datos y exploró algunos de los servicios de Azure que se pueden usar para implementarla.

En este módulo, aprendiste a:

* Identificar los elementos comunes de una solución de almacenamiento de datos a gran escala.


* Describir las características clave de los flujos de ingesta de datos.


* Identificar los tipos comunes de almacenes de datos analíticos y los servicios de Azure relacionados.


* Configurar Microsoft Fabric y utilizarlo para ingerir, procesar y consultar datos.
