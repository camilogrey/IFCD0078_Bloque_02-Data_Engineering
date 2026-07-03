# Explora los fundamentos del análisis a gran escala

## Índice

- [Introducción](#introducción)
- [Análisis a gran escala](#análisis-a-gran-escala)
- [Describa la arquitectura del almacenamiento de datos](#describa-la-arquitectura-del-almacenamiento-de-datos)
- [La pila de análisis moderna en Azure](#la-pila-de-análisis-moderna-en-azure)
- [Explorar los flujos de ingesta de datos](#explorar-los-flujos-de-ingesta-de-datos)
- [Explorar almacenes de datos analíticos](#explorar-almacenes-de-datos-analíticos)
- [Ejercicio: Explora el análisis de datos en Microsoft Fabric](#ejercicio-explora-el-análisis-de-datos-en-microsoft-fabric)
- [Verificación de conocimientos](#verificación-de-conocimientos)
- [Resumen](#resumen)

---

## Introducción

Las organizaciones utilizan plataformas analíticas para crear soluciones de análisis de datos a gran escala que generan información valiosa e impulsan el éxito. Microsoft ofrece diversas tecnologías que se pueden combinar para crear una solución de análisis de datos a gran escala.

### Objetivos de aprendizaje
En este módulo aprenderás a:
- Describa la arquitectura del almacenamiento de datos.
- Describa las características clave de los flujos de ingesta de datos.
- Identifique los tipos comunes de almacenes de datos analíticos y los servicios de Azure relacionados.
- Utilice Microsoft Fabric para recopilar y analizar datos.

Antes de explorar cómo se construyen las soluciones de análisis a gran escala, presentemos las dos principales plataformas de análisis en Azure que encontrará a lo largo de este módulo.

**Microsoft Fabric** es la plataforma unificada de análisis de software como servicio (SaaS) de Microsoft. Integra ingeniería de datos, almacenamiento de datos, análisis en tiempo real, ciencia de datos y Power BI en un único espacio de trabajo basado en navegador, sobre una capa de almacenamiento compartido llamada OneLake. No es necesario administrar servidores ni clústeres: se crean espacios de trabajo y elementos, y Microsoft gestiona la infraestructura.

**Azure Databricks** es una plataforma de análisis en la nube basada en Apache Spark. Está optimizada para la ingeniería de datos, la ciencia de datos y el análisis SQL a gran escala mediante formatos abiertos de almacenamiento en la nube (Lakehouse). Utiliza Delta Lake, un formato de almacenamiento de código abierto que añade transacciones, aplicación de esquemas y control de versiones a los archivos Parquet, lo que proporciona una fiabilidad similar a la de Lakehouse a gran escala. Databricks se ejecuta como un servicio gestionado dentro de su suscripción de Azure y es una opción común para equipos que necesitan flujos de trabajo basados ​​en Spark y notebooks con enfoque en código.

> [!NOTE]
> Reconocemos que cada persona aprende de manera diferente. Puedes completar este módulo en formato de video o leer el contenido en formato de texto e imágenes. El texto contiene más detalles que los videos, por lo que en algunos casos te resultará útil como material complementario a la presentación en video.

---

## Análisis a gran escala

Las soluciones de análisis de datos a gran escala combinan el almacenamiento de datos convencional, utilizado para respaldar la inteligencia empresarial (BI), con técnicas empleadas en el análisis de macrodatos (big data). Una solución de almacenamiento de datos convencional generalmente implica copiar datos de almacenes de datos transaccionales a una base de datos relacional con un esquema optimizado para consultas y la creación de modelos multidimensionales. Las soluciones de procesamiento de macrodatos se utilizan con grandes volúmenes de datos en múltiples formatos, que se cargan por lotes o se capturan en tiempo real y se almacenan en un lago de datos desde donde motores de procesamiento distribuido como Apache Spark los procesan.

![Análisis a gran escala](imagenes/01large-scale-analytics.png)

La combinación de almacenamiento flexible de lagos de datos y análisis SQL de almacenes de datos ha dado lugar al surgimiento de un diseño de análisis a gran escala, a menudo denominado "data lakehouse".

---

## Describa la arquitectura del almacenamiento de datos

La arquitectura de análisis de datos a gran escala puede variar, al igual que las tecnologías específicas utilizadas para implementarla; pero, en general, incluye los siguientes elementos:

![Capas de la arquitectura analítica](imagenes/02analytics-architecture-layers.png)

1. **Ingesta y procesamiento de datos:** Los datos de uno o más almacenes de datos transaccionales, archivos, flujos en tiempo real u otras fuentes se cargan en un lago de datos o un almacén de datos relacional. La operación de carga generalmente implica un proceso de extracción, transformación y carga (ETL) o extracción, carga y transformación (ELT) en el que los datos se limpian, filtran y reestructuran para su análisis. En los procesos ETL, los datos se transforman antes de cargarse en un almacén analítico, mientras que en un proceso ELT los datos se copian al almacén y luego se transforman. En ambos casos, la estructura de datos resultante está optimizada para consultas analíticas. El procesamiento de datos a menudo se realiza mediante sistemas distribuidos que pueden procesar grandes volúmenes de datos en paralelo utilizando clústeres de múltiples nodos. La ingesta de datos incluye tanto el procesamiento por lotes de datos estáticos como el procesamiento en tiempo real de datos en flujo.
2. **Almacenamiento de datos analíticos:** Los almacenes de datos para análisis a gran escala incluyen almacenes de datos relacionales, lagos de datos basados en sistemas de archivos y arquitecturas híbridas que combinan características de almacenes de datos y lagos de datos (a veces denominados bases de datos de lagos de datos o data lakehouses). Hablaremos de esto con más detalle más adelante.
3. **Modelo de datos analítico:** Si bien los analistas y científicos de datos pueden trabajar directamente con los datos en el almacén de datos analítico, es común crear uno o más modelos de datos para facilitar la generación de informes, paneles y visualizaciones interactivas. Históricamente, estos modelos se describían como cubos, en los que los valores de datos numéricos se preagregaban en una o más dimensiones (por ejemplo, ventas totales por producto y región) utilizando tecnologías como SQL Server Analysis Services (SSAS) Multidimensional. Actualmente, el enfoque preferido es un modelo semántico, el modelo tabular que sustenta Power BI y Microsoft Fabric. Un modelo semántico define tablas, relaciones, jerarquías y medidas escritas en DAX (Data Analysis Expressions, un lenguaje de fórmulas utilizado para definir cálculos), con agregaciones calculadas en el momento de la consulta en lugar de almacenarse con antelación. En Microsoft Fabric, los modelos semánticos pueden usar el modo Direct Lake para leer tablas Delta de OneLake directamente, sin importar ni preagregar datos.
4. **Visualización de datos:** Los analistas de datos consumen datos de modelos analíticos y directamente de almacenes de datos para crear informes, paneles y otras visualizaciones. Además, los usuarios de una organización que no sean profesionales de la tecnología pueden realizar análisis y generar informes de datos de forma autónoma. Las visualizaciones de los datos muestran tendencias, comparaciones e indicadores clave de rendimiento (KPI) para una empresa u organización, y pueden presentarse en forma de informes impresos, gráficos y diagramas en documentos o presentaciones de PowerPoint, paneles web y entornos interactivos donde los usuarios pueden explorar los datos visualmente.
5. **Análisis asistido por IA:** Las organizaciones amplían cada vez más su conjunto de herramientas analíticas con capacidades de lenguaje natural e inteligencia artificial. En Power BI, la función de preguntas y respuestas permite a los usuarios escribir preguntas en lenguaje natural y recibir respuestas en forma de gráficos. Copilot en Microsoft Fabric permite a los usuarios describir lo que desean en lenguaje natural; Copilot genera medidas DAX, escribe consultas SQL, resume informes y explica las tendencias de los datos. En Azure Databricks, Genie ofrece una funcionalidad similar sobre los datos de Delta Lake: los usuarios formulan preguntas en lenguaje natural mediante una interfaz conversacional y Genie genera y ejecuta el SQL. Estas funciones amplían el autoservicio analítico a los usuarios que no escriben consultas ni crean informes.

---

## La pila de análisis moderna en Azure

Azure ofrece dos plataformas líderes para el análisis a gran escala: Microsoft Fabric y Azure Databricks. Ambas admiten la arquitectura de análisis completa descrita anteriormente, pero adoptan enfoques diferentes.

### Microsoft Fabric
Microsoft Fabric organiza todas estas capas en un único espacio de trabajo SaaS unificado. El almacenamiento lo proporciona OneLake, un lago de datos compartido por todas las cargas de trabajo de Fabric. En lugar de copiar datos entre silos de almacenamiento, cada servicio de Fabric lee y escribe directamente en OneLake, utilizando Delta Lake como formato estándar de código abierto para los datos del lago de datos.

Dentro de un espacio de trabajo de Fabric, los componentes principales se corresponden con las capas de arquitectura superiores:
- **Fabric Lakehouse:** Combina el almacenamiento de lago de datos con consultas SQL; los datos se almacenan en formato Delta Lake y se exponen a través de un punto final de análisis SQL.
- **Fabric Warehouse:** Un almacén de datos relacional totalmente administrado y compatible con SQL Server para análisis estructurados con una estricta aplicación del esquema.
- **Fabric Data Factory:** Crea y programa flujos de trabajo y transformaciones de bajo código para la ingesta y el movimiento de datos.
- **Power BI:** Ofrece informes, paneles y modelos semánticos; el modo Direct Lake lee las tablas Delta de OneLake directamente sin importar datos ni actualizarlas programadamente.

### Azure Databricks
Azure Databricks es una plataforma de análisis en la nube basada en Apache Spark, optimizada para la ingeniería de datos a gran escala, la ciencia de datos y el análisis SQL. Se ejecuta como un servicio administrado dentro de su suscripción de Azure y utiliza Delta Lake como su formato de almacenamiento abierto nativo.

Los componentes principales de Azure Databricks son:
- **Databricks Lakehouse:** Una capa de almacenamiento unificada construida sobre Delta Lake que admite cargas de trabajo tanto de análisis SQL como de aprendizaje automático en el mismo entorno.
- **Databricks SQL:** Un almacén de datos SQL sin servidor para ejecutar consultas analíticas directamente sobre tablas Delta, con historial de consultas, paneles de control y alertas integrados.
- **Cuadernos de Databricks:** Cuadernos colaborativos de Python, SQL, Scala y R para flujos de trabajo de ingeniería de datos, análisis exploratorio y aprendizaje automático.
- **Unity Catalog:** Una capa de gobernanza unificada para datos y activos de IA en todo el espacio de trabajo de Databricks, que proporciona un control de acceso detallado, linaje de datos y descubrimiento.
- **Genie:** Una interfaz conversacional impulsada por IA que permite a los usuarios formular preguntas en lenguaje natural sobre sus datos; Genie genera y ejecuta SQL automáticamente, devolviendo resultados sin necesidad de escribir ninguna consulta.

---

## Explorar los flujos de ingesta de datos

Ahora que comprende la arquitectura de una solución de almacenamiento de datos a gran escala, es momento de explorar cómo se ingieren los datos en un almacén de datos analíticos desde una o más fuentes.

![Opciones de ingesta de datos](imagenes/03data-ingestion-options.png)

### Microsoft Fabric
Microsoft Fabric ofrece múltiples formas de incorporar datos a OneLake, abarcando desde ETL basado en canalizaciones hasta federación sin copia y transmisión en tiempo real.

#### Fábrica de datos de tejidos (Fabric Data Factory)
Fabric Data Factory es el punto de partida recomendado para la ingesta basada en canalizaciones. Ofrece dos herramientas complementarias:
- **Pipelines:** Orquestan flujos de trabajo de transformación y movimiento de datos en varios pasos, encadenando actividades que se ejecutan en secuencia o en paralelo.
- **Dataflows Gen2:** Una experiencia visual y de bajo código para crear lógica de transformación de datos reutilizable mediante Power Query, sin necesidad de escribir código.

Las canalizaciones constan de una o más actividades que operan sobre datos. Un conjunto de datos de entrada proporciona los datos de origen, y las actividades se pueden definir como un flujo de datos que manipula los datos de forma incremental hasta generar un conjunto de datos de salida. Las canalizaciones utilizan servicios vinculados para conectarse a orígenes y destinos, lo que permite cargar datos, ejecutar procedimientos almacenados, invocar cuadernos y aplicar lógica personalizada en un único flujo de trabajo. Puede guardar la salida en Microsoft Fabric Lakehouse o Warehouse, o en cualquier otro destino compatible. Las canalizaciones también pueden incluir actividades integradas que no requieren un servicio vinculado.

#### Atajos de OneLake (OneLake Shortcuts)
Un acceso directo es una referencia en tiempo real al almacenamiento externo: ADLS Gen2, Amazon S3, Google Cloud Storage u otra ubicación de OneLake. En lugar de copiar los datos a Fabric, un acceso directo hace que los datos externos aparezcan como si ya estuvieran en su Lakehouse. Sin canalizaciones, sin movimiento, sin duplicación. Esto es especialmente útil cuando los datos deben permanecer en su ubicación original por motivos de cumplimiento normativo o costes, pero aún así deben poder consultarse desde Fabric.

#### Reflejo (Mirroring)
Fabric Mirroring replica continuamente una base de datos externa (Azure SQL Database, Snowflake, Azure Cosmos DB, entre otras) directamente en OneLake prácticamente en tiempo real. Solo necesita configurar la conexión de origen una vez y Fabric se encarga automáticamente del seguimiento de cambios y la replicación, sin necesidad de crear canalizaciones. Los datos replicados se almacenan en formato Delta Lake y se pueden consultar inmediatamente a través del punto de conexión de análisis SQL de Lakehouse.

#### Eventstream
Para la ingesta de datos en tiempo real, Fabric Eventstream se conecta a fuentes de eventos como Azure Event Hubs, Apache Kafka, Azure IoT Hub y puntos de conexión personalizados. Enruta, filtra y transforma los eventos en tiempo real antes de enviarlos a un Fabric Lakehouse, una base de datos KQL (un almacén optimizado para series temporales consultado con Kusto Query Language) o un destino de Real-Time Intelligence, lo que permite realizar análisis casi en tiempo real sobre datos que llegan continuamente.

#### Cuadernos de tela (Fabric Notebooks)
Los Fabric Notebooks (con tecnología Apache Spark) son una opción de programación orientada al código para la ingesta de datos cuando no existe un conector integrado o cuando se requiere lógica de transformación personalizada. Puede escribir código PySpark, Python, Scala, R o SQL para leer desde API REST, bases de datos, archivos o cualquier punto final accesible, y luego escribir los resultados directamente en una tabla o almacén de Lakehouse Delta. Los Notebooks se pueden ejecutar de forma interactiva o programarse como parte de una canalización, lo que los convierte en una solución flexible para escenarios que otras herramientas de Fabric no cubren de forma predefinada.

### Azure Data Factory
Azure Data Factory es el servicio independiente de Azure para crear canalizaciones de integración de datos fuera de Fabric; por ejemplo, cuando el destino es Azure SQL Database o un servicio externo, o cuando se conecta a fuentes de datos locales en un entorno híbrido. Al igual que Fabric Data Factory, utiliza el mismo modelo basado en canalizaciones con servicios vinculados, por lo que los conocimientos adquiridos son transferibles entre ambos.

### Azure Databricks
Azure Databricks ofrece varios enfoques de ingesta, que van desde canalizaciones declarativas hasta cuadernos ad hoc y mecanismos especializados de carga de archivos.

#### Tuberías declarativas de Lakeflow
Lakeflow Spark Declarative Pipelines ofrece un marco declarativo para crear pipelines fiables y con actualizaciones incrementales en Delta Lake. Usted define el contenido de las tablas de salida; Databricks gestiona automáticamente el orden de ejecución, el seguimiento de dependencias y el procesamiento incremental. Este es el enfoque recomendado para pipelines de ingesta de nivel de producción que se ejecutan continuamente.

#### Cuadernos de Databricks
Los cuadernos de Databricks son compatibles con PySpark, Python, Scala, SQL y R, y resultan ideales para tareas de ingesta de datos puntuales o exploratorias: lectura desde API REST, fuentes JDBC, almacenamiento en la nube o fuentes de transmisión, y escritura en tablas de Delta Lake. Los cuadernos también pueden implementarse programándolos como tareas o integrándolos en una canalización de Lakeflow como paso de transformación.

> [!NOTE]
> Azure Databricks ofrece mecanismos de ingesta especializados adicionales a los que se describen aquí.

---

## Explorar almacenes de datos analíticos

Existen dos tipos comunes de almacenamiento de datos analíticos.

### Almacenes de datos
Un almacén de datos es una base de datos relacional donde los datos se almacenan en un esquema optimizado para el análisis de datos, en lugar de para cargas de trabajo transaccionales. Generalmente, los datos de un almacén transaccional se transforman en un esquema donde los valores numéricos se almacenan en tablas de hechos centrales, las cuales están relacionadas con una o más tablas de dimensiones que representan entidades mediante las cuales se pueden agregar los datos. Por ejemplo, una tabla de hechos podría contener datos de pedidos de venta, que se pueden agregar por cliente, producto, tienda y dimensión temporal (lo que permite, por ejemplo, encontrar fácilmente los ingresos totales mensuales por ventas de cada producto en cada tienda).

Este tipo de esquema de tablas de hechos y dimensiones se denomina **esquema de estrella**; aunque a menudo se extiende a un **esquema de copo de nieve** agregando tablas adicionales relacionadas con las tablas de dimensiones para representar jerarquías dimensionales (por ejemplo, el producto podría estar relacionado con las categorías de productos).

![Esquema en estrella y de copo de nieve](imagenes/04star-snowflake-schema.png)

Un almacén de datos es una excelente opción cuando se tienen datos transaccionales que se pueden organizar en un esquema estructurado de tablas y se desea utilizar SQL para consultarlos.

### Lagos de datos
Un lago de datos es un repositorio de archivos, generalmente en un sistema de archivos distribuido, para un acceso a datos de alto rendimiento. Se utilizan motores de procesamiento distribuido nativos de la nube, como Apache Spark, para procesar consultas sobre los archivos almacenados y devolver datos para informes y análisis. Estos sistemas suelen aplicar un enfoque de **esquema en lectura** para definir esquemas tabulares en archivos de datos semiestructurados en el momento en que se leen los datos para su análisis, sin aplicar restricciones al almacenarlos.

![Estructura de un Data Lake](imagenes/05data-lake.png)

Los lagos de datos son excelentes para admitir una combinación de datos estructurados, semiestructurados e incluso no estructurados que se desean analizar sin necesidad de aplicar un esquema cuando los datos se escriben en el almacenamiento.

### Enfoques híbridos
Puedes usar un enfoque híbrido que combine características de lagos de datos y almacenes de datos en un **data lakehouse**. Los datos sin procesar se almacenan como archivos en un lago de datos, y un punto de conexión de análisis SQL expone esos archivos como tablas que puedes consultar con SQL. Los data lakehouses se habilitan mediante Delta Lake, el formato estándar de código abierto para lakehouses utilizado tanto por Microsoft Fabric como por Azure Databricks. Delta Lake agrega capacidades de almacenamiento relacional sobre archivos Parquet, lo que permite definir tablas que imponen esquemas y coherencia transaccional, admiten fuentes de datos cargadas por lotes y en tiempo real, y proporciona una API SQL para realizar consultas.

En Microsoft Fabric, todos los datos de Lakehouse se almacenan en OneLake, una única capa de almacenamiento compartida entre todas las cargas de trabajo de Fabric. Al crear un Lakehouse de Fabric, se crea automáticamente un punto de conexión de análisis SQL. Al crear un Almacén de Fabric, los datos también residen en OneLake, lo que proporciona a ambas experiencias una base de almacenamiento unificada.

![Arquitectura Data Lakehouse](imagenes/06data-lakehouse.png)

### Servicios de Azure para almacenes analíticos
En Azure, existen varios servicios que puede utilizar para implementar un almacén analítico a gran escala, entre ellos:

**Microsoft Fabric** es una plataforma de análisis SaaS unificada e integral construida sobre OneLake. Ofrece dos experiencias principales de almacenamiento analítico:
- **Fabric Lakehouse:** Almacena datos en formato Delta Lake en OneLake, admite notebooks y Spark para cargas de trabajo de ciencia de datos y ofrece un punto final de análisis SQL para consultas estructuradas. Es la opción ideal cuando se trabaja con datos mixtos o semiestructurados, o cuando se ejecutan flujos de trabajo de aprendizaje automático.
- **Fabric Warehouse:** Un almacén de datos relacional totalmente administrado y compatible con SQL Server, respaldado también por OneLake. Es la opción ideal cuando sus datos están estructurados, su equipo escribe SQL y necesita una sólida aplicación del esquema y concurrencia para muchos usuarios simultáneos.

Fabric también incluye canalizaciones de datos integradas (Fabric Data Factory) para la ingesta y transformación de datos, así como inteligencia en tiempo real nativa para el análisis de registros y telemetría. Fabric Mirroring permite replicar continuamente datos de sistemas operativos, como Azure Cosmos DB, Azure SQL Database y Snowflake, directamente en OneLake sin necesidad de crear canalizaciones personalizadas.

**Azure Databricks** es una plataforma de análisis en la nube basada en Apache Spark, optimizada para la ingeniería de datos a gran escala, la ciencia de datos y el análisis SQL. Utiliza Delta Lake como formato de almacenamiento nativo, lo que proporciona soporte para transacciones, cumplimiento de esquemas y control de versiones en todas las tablas. Para la generación de informes e inteligencia empresarial basados ​​en SQL, Azure Databricks ofrece Databricks SQL Warehouse, un punto de conexión SQL dedicado optimizado para herramientas de BI y cargas de trabajo de consultas concurrentes. Databricks es una opción común cuando se necesitan flujos de trabajo Spark con enfoque en código, portabilidad en múltiples nubes o cuando la organización ya cuenta con experiencia en la plataforma.

Tanto Fabric como Azure Databricks incluyen funciones de asistente de IA para escribir SQL y generar código para notebooks.

> [!NOTE]
> Cada uno de estos servicios puede considerarse un almacén de datos analíticos, ya que proporciona un esquema y una interfaz para consultar los datos. En muchos casos, los datos se almacenan en un lago de datos y el servicio los procesa y ejecuta consultas. Algunas soluciones combinan ambos servicios: un proceso de ingesta ELT podría copiar los datos en el lago de datos, usar un cuaderno en Azure Databricks para procesar un gran volumen de datos y, finalmente, cargar los resultados en tablas de Microsoft Fabric Warehouse.

---

## Ejercicio: Explora el análisis de datos en Microsoft Fabric

**Duración:** 30 minutos

En este ejercicio, crearás un repositorio de datos de Microsoft Fabric y lo usarás para ingerir y analizar algunos datos.

Este ejercicio está diseñado para familiarizarte con algunos elementos clave de una solución de análisis de datos a gran escala, no como una guía completa para realizar análisis de datos avanzados con Microsoft Fabric. El ejercicio debería tomar alrededor de 30 minutos.

> [!NOTE]
> Para completar este laboratorio, necesitarás una cuenta de Microsoft Fabric o, si aún no tienes una, regístrate para una prueba gratuita. Para obtener más información, consulte la sección "Primeros pasos con Fabric".

Inicie el ejercicio y siga las instrucciones.

![Launch Exercise](imagenes/launch_exercise.png)

---

## Verificación de conocimientos

**Valor:** 200 XP  
**Duración:** 3 minutos  

Elige la mejor respuesta para cada una de las siguientes preguntas.

### 1. ¿Qué término se utiliza para describir una solución analítica que combina la funcionalidad de un lago de datos y un almacén de datos relacional?
- [x] Data lakehouse
- [ ] Canalización de datos
- [ ] Nube

### 2. ¿Qué solución de análisis SaaS puede utilizar para crear un flujo de trabajo para la ingesta y el procesamiento de datos?
- [ ] Azure SQL Database y Azure Cosmos DB
- [x] Microsoft Fabric
- [ ] Azure Cosmos DB

### 3. ¿Qué motor de procesamiento distribuido de código abierto incluye Microsoft Fabric?
- [ ] Apache Hadoop
- [x] Apache Spark
- [ ] Tormenta Apache

---

## Resumen

El almacenamiento de datos a gran escala es una tarea compleja que puede involucrar diversas tecnologías. Este módulo ofreció una visión general de las características clave de una solución moderna de almacenamiento de datos y exploró algunos de los servicios de Azure que se pueden usar para implementarla.

En este módulo, aprendiste a:
- Identificar los elementos comunes de una solución de almacenamiento de datos a gran escala.
- Describa las características clave de los flujos de ingesta de datos.
- Identificar los tipos comunes de almacenes de datos analíticos y los servicios de Azure relacionados.
- Configure Microsoft Fabric y utilícelo para ingerir, procesar y consultar datos.

---

⬅️ **Anterior:** [00. Ruta Introducción al análisis de datos en Microsoft Azure](../Ruta_Introducción_al_análisis_de_datos_en_Microsoft_Azure/Introducción_al_análisis_de_datos_en_Microsoft_Azure.md)

🏠 **Inicio del módulo:** [README](../README.md)

➡️ **Siguiente:** [02. Explora los fundamentos del análisis en tiempo real](../02_Analisis_en_tiempo_real/02_Explora_los_fundamentos_del_análisis_en_tiempo_real.md)