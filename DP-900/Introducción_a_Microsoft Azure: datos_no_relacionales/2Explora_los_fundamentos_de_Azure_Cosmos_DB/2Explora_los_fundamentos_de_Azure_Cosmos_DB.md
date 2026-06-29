# Explora los fundamentos de Azure Cosmos DB

Azure Cosmos DB proporciona un almacenamiento altamente escalable para datos no relacionales.

**Objetivos de aprendizaje**

En este módulo, aprenderá a:

* Describa las características y capacidades clave de Azure Cosmos DB.

* Identifique las API compatibles con Azure Cosmos DB.

* Aprovisione y explore una base de datos Azure Cosmos DB.

**Este módulo forma parte de estas rutas de aprendizaje.**

* [Introducción a Microsoft Azure: datos no relacionales en Azure](https://learn.microsoft.com/training/paths/azure-data-fundamentals-explore-non-relational-data/)

# INDICE

* [Introducción](#introducción)

* [Describa Azure Cosmos DB](#describa-azure-cosmos-db)

* [Identificar las API de Azure Cosmos DB](#identificar-las-api-de-azure-cosmos-db)

* [Ejercicio: Explorar Azure Cosmos DB](https://learn.microsoft.com/en-us/training/modules/explore-non-relational-data-stores-azure/4-exercise-explore-cosmos-db)

* [Evaluación del módulo](https://learn.microsoft.com/en-us/training/modules/explore-non-relational-data-stores-azure/5-knowledge-check)

* [Resumen](https://learn.microsoft.com/en-us/training/modules/explore-non-relational-data-stores-azure/6-summary)

## Introducción {#introducción}

Las bases de datos relacionales almacenan datos en tablas relacionales, pero a veces la estructura impuesta por este modelo puede ser demasiado rígida y, a menudo, conlleva un rendimiento deficiente a menos que se dedique tiempo a una optimización detallada. Existen otros modelos, conocidos colectivamente como bases de datos *NoSQL* . Estos modelos almacenan datos en otras estructuras, como documentos, grafos, almacenes clave-valor y almacenes de familias de columnas.

Azure Cosmos DB es un servicio de base de datos en la nube altamente escalable para datos NoSQL.

** Nota**

Reconocemos que cada persona aprende de manera diferente. Puedes completar este módulo en formato de video o leer el contenido en formato de texto e imágenes. El texto contiene más detalles que los videos, por lo que en algunos casos te resultará útil como material complementario a la presentación en video.

## Describa Azure Cosmos DB {#describa-azure-cosmos-db}

En la unidad anterior, aprendiste que Azure Cosmos DB es un servicio de base de datos en la nube altamente escalable para datos NoSQL. En esta unidad, explorarás qué lo diferencia de las bases de datos relacionales tradicionales, cómo organiza los datos internamente y cuándo es la opción adecuada para tu aplicación.

### ¿Qué es Azure Cosmos DB?

**Azure Cosmos DB** es un servicio de base de datos NoSQL totalmente administrado en Azure, una plataforma como servicio (PaaS). Microsoft se encarga de toda la infraestructura subyacente: aprovisionamiento de servidores, parches, actualizaciones y copias de seguridad. Usted se centra en la lógica de su aplicación mientras Cosmos DB gestiona la parte operativa.

Cosmos DB es **independiente del esquema** . Los elementos almacenados en el mismo contenedor no necesitan compartir la misma estructura. Un elemento podría tener cinco propiedades; otro en el mismo contenedor podría tener quince completamente diferentes. Esta flexibilidad hace que Cosmos DB sea ideal para aplicaciones donde la estructura de los datos cambia con el tiempo o varía entre registros.

Microsoft utiliza Cosmos DB internamente para algunos de sus servicios más exigentes, como Xbox Live, Microsoft 365 y componentes clave de Azure. En conjunto, estos servicios gestionan miles de millones de operaciones diarias, lo que da una idea de la escala para la que está diseñado Cosmos DB.

### Cómo Azure Cosmos DB organiza los datos

Cosmos DB utiliza una jerarquía de recursos de cuatro niveles para organizar sus datos:

* **Cuenta** : El recurso de Azure de nivel superior. Una sola cuenta puede contener un número ilimitado de bases de datos.

* **Base de datos** : Un espacio de nombres lógico que agrupa contenedores relacionados.

* **Contenedor** : La unidad principal de almacenamiento y escalado. La clave de partición, el rendimiento, la política de indexación y un tiempo de vida (TTL) opcional se configuran a nivel de contenedor.

* **Elementos** : Entidades de datos individuales almacenadas dentro de un contenedor. Dependiendo de la API que utilice, los elementos pueden denominarse documentos, filas, nodos o aristas.

La **clave de partición** es una propiedad que se elige para distribuir los datos entre las particiones lógicas. Cada partición lógica puede almacenar hasta 20 GB de datos. Una clave de partición bien elegida —con muchos valores distintos y una distribución uniforme de los datos entre ellos— es importante para mantener un rendimiento equilibrado a medida que crece la base de datos.

![Diagrama que explica cómo Azure Cosmos DB organiza los datos.](imagenes3_2/1cosmos-db-hierarchy.png)



Cosmos DB crea y mantiene automáticamente índices en todas las propiedades de los elementos por defecto. No es necesario definir un esquema de antemano ni gestionar los índices manualmente; el servicio se encarga de todo.

### Distribución y rendimiento globales

Cosmos DB está diseñado para la distribución global. Añada regiones de Azure a su cuenta en cualquier momento y el servicio replicará automáticamente sus datos en cada una de ellas. Los usuarios en diferentes ubicaciones leen y escriben en la réplica regional más cercana, lo que mantiene la latencia baja independientemente de su ubicación.

Las cuentas de escritura multirregión ofrecen garantías de alta disponibilidad. En el percentil 99, las lecturas suelen completarse en unos 4 milisegundos y las escrituras en unos 5 milisegundos.

Dado que las réplicas existen en varias regiones, es necesario decidir el grado de consistencia que deben tener entre sí. Cosmos DB ofrece cinco **niveles de consistencia** para que pueda ajustar este equilibrio:

| Nivel de consistencia | Descripción |
| :---- | :---- |
| **Fuerte** | Cada lectura refleja la escritura más reciente. |
| **rancidez limitada** | Las lecturas se producen con un retraso respecto a las escrituras, con un intervalo configurable (tiempo o número de versiones). |
| **Sesión** | Se garantiza la consistencia dentro de una misma sesión con el cliente. Este es el nivel más utilizado. |
| **Prefijo consistente** | Las lecturas nunca ven escrituras fuera de orden, pero pueden ver datos obsoletos. |
| **Eventual** | Las réplicas convergen con el tiempo; la garantía más débil, pero la mayor disponibilidad. |

Para la mayoría de las aplicaciones transaccionales, la consistencia de sesión es el punto de partida recomendado.

![Diagrama que explica la distribución y el rendimiento a nivel mundial.](imagenes3_2/2cosmos-db-global-consistency.png)

### Modos de procesamiento y precios

Cosmos DB mide la capacidad en **unidades de solicitud por segundo (RU/s)** . Una RU/s equivale aproximadamente al costo de leer un elemento de 1 KB. Cada operación (lecturas, escrituras, consultas y eliminaciones) consume una cierta cantidad de RU/s, lo que proporciona una métrica única para evaluar tanto el rendimiento como el costo.

Hay tres modos de rendimiento disponibles:

| Modo de rendimiento | Descripción |
| :---- | :---- |
| **Dedicado** | El ancho de banda está reservado exclusivamente para un solo contenedor. |
| **Compartido** | El rendimiento se aprovisiona a nivel de base de datos y se comparte entre hasta 25 contenedores. |
| **Sin servidor** | No requiere configuración previa; se paga por solicitud. Ideal para cargas de trabajo con tráfico impredecible o bajo. |

** Nota**

Las cuentas sin servidor están limitadas a una sola región de Azure. Si su aplicación requiere una distribución global en varias regiones, utilice una cuenta de rendimiento aprovisionado.

La opción **de escalado automático** le permite establecer un máximo de RU/s, y Cosmos DB ajusta la capacidad automáticamente dentro de ese rango en función de la demanda real.

![image3](imagenes3_2/3cosmos-db-throughput-modes.png)


### Cuándo usar Cosmos DB

Cosmos DB es una excelente opción para aplicaciones que necesitan un esquema flexible, alcance global y una latencia baja y constante:

* **IoT y telemetría** : Ingestión rápida de datos de dispositivos de alta frecuencia, disponibles para su procesamiento casi en tiempo real.

* **Videojuegos** : Perfiles de jugadores, tablas de clasificación y estadísticas dentro del juego que requieren tiempos de respuesta de un solo dígito en milisegundos.

* **Comercio minorista y electrónico** : catálogos de productos, carritos de compra y procesos de pedidos a cualquier escala.

* **Aplicaciones web y móviles** : experiencias de usuario personalizadas, funciones sociales e integraciones con terceros.

Algunas cargas de trabajo no son adecuadas. Si su aplicación depende de uniones complejas de varias tablas, Azure SQL Database es más apropiado. Para análisis históricos a gran escala, considere Microsoft Fabric o Azure Synapse Analytics.

En la siguiente unidad, analizará las diferentes API que admite Cosmos DB y cómo cada una le permite trabajar con sus datos utilizando herramientas y lenguajes de consulta conocidos.

## Identificar las API de Azure Cosmos DB {#identificar-las-api-de-azure-cosmos-db}

En la unidad anterior, exploraste qué es Azure Cosmos DB y cómo organiza los datos. Una de sus características más prácticas es que no te obliga a usar un único lenguaje de consulta o modelo de datos. En cambio, Cosmos DB expone múltiples API compatibles con protocolos de comunicación, lo que permite a los desarrolladores usar herramientas, bibliotecas y sintaxis familiares para trabajar con sus datos, incluso al migrar una aplicación existente.

### ¿Por qué Cosmos DB admite múltiples API?

Al crear una cuenta de Cosmos DB, usted elige qué API utilizar. Esta elección determina cómo interactúa su aplicación con la base de datos: qué formato de datos envía, qué lenguaje de consulta utiliza y con qué bibliotecas cliente trabaja. Internamente, Cosmos DB almacena los datos en su propio formato; la API actúa como una capa de abstracción.

La principal ventaja es la portabilidad. Si tu equipo ya tiene una aplicación basada en MongoDB o Apache Cassandra, puedes migrarla a Cosmos DB con cambios mínimos en el código y, de paso, obtener distribución global, rendimiento gestionado y las ventajas inherentes a los acuerdos de nivel de servicio (SLA).

Las cinco API compatibles son: **NoSQL** , **MongoDB** , **Table** , **Apache Cassandra** y **Apache Gremlin** . Cada una está diseñada para un tipo de datos o caso de uso diferente.

![Diagrama que muestra Azure Cosmos DB en el centro, con cinco API que se ramifican hacia afuera: NoSQL, MongoDB, Table, Apache Cassandra y Apache Gremlin.](imagenes3_2/4cosmos-db-apis.png)


### Cosmos DB para NoSQL

**Azure Cosmos DB para NoSQL** es la API nativa de Cosmos DB. Almacena datos como documentos JSON y permite realizar consultas con una sintaxis similar a SQL.

* Nota*

*Esta API se conocía anteriormente como API SQL. En 2023, pasó a llamarse API NoSQL. Si encuentra documentación antigua que haga referencia a la "API SQL", se trata del mismo servicio.*

Se recomienda utilizar la API NoSQL para nuevas aplicaciones.

Una consulta tiene este aspecto:

'''SQL

SELECT \*

FROM customers c

WHERE c.id \= "joe@litware.com"
'''

El resultado es un documento JSON:

**JSON**

{

   "id": "joe@litware.com",

   "name": "Joe Jones",

   "address": {

        "street": "1 Main St.",

        "city": "Seattle"

    }

}

* Consejo*

*Cosmos DB para NoSQL admite la replicación de Fabric, que reproduce automáticamente sus datos operativos en Microsoft Fabric, sin necesidad de canalizaciones. Esto facilita la ejecución de análisis en datos en tiempo real sin afectar su carga de trabajo transaccional.*

### Cosmos DB para MongoDB

**Azure Cosmos DB para MongoDB** es compatible con los controladores y las bibliotecas cliente de MongoDB, por lo que sus aplicaciones MongoDB existentes pueden conectarse a Cosmos DB sin necesidad de realizar cambios significativos en el código.

Los datos se almacenan en formato BSON (JSON binario: una codificación binaria de JSON), y las consultas utilizan el lenguaje de consulta de MongoDB (MQL), una sintaxis compacta y orientada a objetos donde se llaman métodos en objetos de colección. Para encontrar un producto por ID:

**JavaScript**

db.products.find({id: 123})

El resultado:

**JSON**

{

   "id": 123,

   "name": "Hammer",

   "price": 2.99

}

Esta API es la opción ideal cuando su equipo ya cuenta con experiencia en MongoDB o cuando está migrando una carga de trabajo de MongoDB existente a un servicio en la nube administrado.

### Cosmos DB para tabla

**Azure Cosmos DB for Table** almacena los datos como pares clave-valor en tablas, utilizando el mismo modelo de programación que Azure Table Storage. Si ya dispone de una aplicación de Azure Table Storage, puede conectarse a Cosmos DB for Table con cambios mínimos en el código.

Las ventajas que ofrece en comparación con Azure Table Storage incluyen mayor escalabilidad, distribución global, índices secundarios automáticos y escalado automático instantáneo. Una tabla podría tener este aspecto:

| Clave de partición | Clave de fila | Nombre | Correo electrónico |
| :---- | :---- | :---- | :---- |
| 1 | 123 | Joe Jones | joe@litware.com |
| 1 | 124 | Samir Nadoy | samir@northwind.com |

Cada fila se identifica mediante una combinación **de PartitionKey** y **RowKey** . Se recupera una fila específica a través de un punto final de estilo REST:

**texto**

https://endpoint/Customers(PartitionKey='1',RowKey='124')

* Consejo*

*Azure Table Storage sigue estando disponible y cuenta con soporte, pero Cosmos DB for Table es la opción recomendada para nuevas cargas de trabajo que requieren almacenamiento de clave-valor a gran escala.*

### Cosmos DB para Apache Cassandra

**Azure Cosmos DB para Apache Cassandra** es compatible con Apache Cassandra, una base de datos de código abierto que utiliza un modelo de almacenamiento por familias de columnas. En las tablas por familias de columnas, las filas no tienen por qué contener las mismas columnas, a diferencia de una tabla relacional donde el esquema es fijo para todas las filas.

Por ejemplo, una tabla **de empleados** podría tener este aspecto:

| IDENTIFICACIÓN | Nombre | Gerente |
| :---- | :---- | :---- |
| 1 | Sue Smith |  |
| 2 | Ben Chan | Sue Smith |

Cassandra utiliza CQL (Cassandra Query Language), cuya sintaxis es similar a la de SQL. Para recuperar un registro específico:

sql'''

SELECT \* FROM Employees WHERE ID \= 2
'''

Esta API es ideal para equipos que migran una carga de trabajo de Apache Cassandra a una base de datos en la nube totalmente administrada.

### Cosmos DB para Apache Gremlin

**Azure Cosmos DB para Apache Gremlin** está diseñado para **datos de grafos** , un modelo donde las entidades se representan como **vértices** (nodos) y las relaciones como **aristas** . Las bases de datos de grafos son útiles cuando las conexiones entre los datos son tan importantes como los datos mismos: por ejemplo, en redes sociales, sistemas de recomendación, detección de fraude y jerarquías organizativas.

![Diagrama gráfico que muestra los vértices de los empleados conectados por aristas que representan las relaciones jerárquicas y la pertenencia a departamentos.][image5]

5graph.png

En el ejemplo anterior, los vértices de empleados y departamentos están conectados por aristas que representan las relaciones jerárquicas y la pertenencia a un departamento.

Gremlin es el lenguaje de consulta utilizado para recorrer y manipular datos de grafos. Para agregar un vértice de empleado y conectarlo a uno existente:

**apache**

g.addV('employee').property('id', '3').property('firstName', 'Alice')

g.V('3').addE('reports to').to(g.V('1'))

Para recuperar todos los vértices de empleados en orden de ID:

**apache**

g.V().hasLabel('employee').order().by('id')

Con cinco API para elegir, puedes adaptar la interfaz de Cosmos DB a tu modelo de datos, las habilidades de tu equipo y el código de tu aplicación. En la siguiente unidad, tendrás la oportunidad de trabajar directamente con Cosmos DB.

## Evaluación del módulo

Esta evaluación valora tu comprensión del módulo. A diferencia de antes, no recibirás comentarios sobre cada respuesta, solo se te indicará si son correctas o incorrectas. El objetivo es medir lo que has aprendido. Dedica tiempo a revisar los materiales del módulo antes de empezar.

Entendido. Ajusto el formato para que incluya tanto la pregunta en español como su versión original en inglés, manteniendo las opciones en su inglés original, la respuesta correcta resaltada y la explicación breve.

Aquí tienes el bloque configurado con este nivel de detalle:

**1.Pregunta (Español):** Su organización está construyendo un sistema de telemática que ingiere grandes volúmenes de datos en ráfagas frecuentes. ¿Qué característica de Azure Cosmos DB garantiza un procesamiento de datos eficiente en este escenario?

**Pregunta (Inglés):** Your organization is building a telematics system that ingests large amounts of data in frequent bursts. Which Azure Cosmos DB feature ensures efficient data processing in this scenario?

* **Opciones:**

  * **A) Automatic partitioning**

  * B) Multi-region writes

  * C) Azure Cosmos DB for Apache Gremlin

* **Explicación:** El particionamiento automático distribuye el almacenamiento y el rendimiento de manera dinámica entre múltiples nodos físicos detrás de escena. Esto permite al motor absorber picos masivos y repentinos de ingesta de datos sin penalizar la velocidad de procesamiento.

**2.Pregunta (Español):** ¿Por qué Azure Cosmos DB es una opción ideal para plataformas de comercio electrónico (retail) con usuarios globales?

**Pregunta (Inglés):** Why is Azure Cosmos DB an ideal choice for retail platforms with global users?

* **Opciones:**

  * **A) Its ability to provide fast local reads and writes across multiple regions.**

  * B) Its integrated support for large video file storage.

  * C) Its focus on maintaining ACID compliance for all transactions.

* **Explicación:** Al permitir la replicación geográfica activa y la escritura multi-región simultánea, Cosmos DB garantiza que cualquier usuario en el mundo, sin importar su ubicación, obtenga latencias de lectura y escritura inferiores a 10 milisegundos en el plano local.

**3.Pregunta (Español):** ¿Qué modelo de datos utiliza Azure Cosmos DB for Table?

**Pregunta (Inglés):** What data model does Azure Cosmos DB for Table use?

* **Opciones:**

  * A) Document-based data model

  * **B) Key-value tables**

  * C) Graph-based data model

* **Explicación:** Esta API está diseñada específicamente para almacenar datos semiestructurados bajo el modelo clásico de clave-valor, ofreciendo compatibilidad total con las aplicaciones heredadas que ya consumían el servicio tradicional de *Azure Table Storage*.

**4.Pregunta (Español):** ¿Qué caso de uso está MENOS alineado con las características de Azure Cosmos DB?

**Pregunta (Inglés):** Which use case is least aligned with the features of Azure Cosmos DB?

* **Opciones:**

  * A) High throughput and low latency for gaming applications.

  * **B) Complex transactional processing requiring strict ACID compliance.**

  * C) Global data distribution for retail catalogs.

* **Explicación:** Las bases de datos NoSQL distribuidas globalmente no están diseñadas para sistemas transaccionales relacionales complejos que exijan bloqueos rigurosos entre múltiples tablas jerárquicas (como un core bancario tradicional). Para este fin, es mejor utilizar motores relacionales como *Azure SQL Database*.

**5\. Pregunta (Español):** Una empresa requiere un servicio de base de datos que pueda manejar tasas de solicitudes variables sin intervención manual. ¿Qué servicio debería elegir?

**Pregunta (Inglés):** A company requires a database service that can handle variable request rates without manual intervention. Which service should they choose?

* **Opciones:**

  * **A) Azure Cosmos DB**

  * B) Azure SQL Database

  * C) Azure Data Factory

* **Explicación:** Cosmos DB dispone de opciones de capacidad autogestionadas (*Autoscale* y *Serverless*) que escalan o reducen de manera inmediata y automática las unidades de rendimiento (RUs) según la demanda en tiempo real, eliminando por completo la necesidad de mantenimiento manual.

**6.Pregunta (Español):** Su empresa necesita almacenar documentos JSON para su aplicación web, utilizando la sintaxis SQL para las consultas. ¿Qué API de Azure Cosmos DB debería seleccionarse?

**Pregunta (Inglés):** Your company needs to store JSON documents for its web application, using SQL syntax for queries. Which Azure Cosmos DB API should be selected?

* **Opciones:**

  * A) Azure Cosmos DB for Cassandra

  * B) Azure Cosmos DB for MongoDB

  * **C) Azure Cosmos DB for NoSQL**

* **Explicación:** La API para NoSQL es el motor nativo de Cosmos DB. Almacena de forma directa la información en formato de documentos JSON y expone una interfaz que permite explotar e interrogar dichos documentos utilizando la sintaxis declarativa de SQL clásico.

**7.Pregunta (Español):** ¿Qué característica es común tanto a Azure Cosmos DB for Apache Cassandra como a Azure Cosmos DB for PostgreSQL?

**Pregunta (Inglés):** Which feature is common to both Azure Cosmos DB for Apache Cassandra and Azure Cosmos DB for PostgreSQL?

* **Opciones:**

  * **A) Support for SQL-based querying**

  * B) Binary JSON (BSON) data storage

  * C) Key-value table structure

* **Explicación:** Ambas APIs se gestionan mediante variaciones de lenguajes declarativos estructurados muy similares. Mientras que PostgreSQL utiliza SQL puro, Cassandra se apoya en CQL (*Cassandra Query Language*), cuya sintaxis y comandos de consulta son prácticamente idénticos a SQL.

**8.Pregunta (Español):** ¿Qué API de Azure Cosmos DB debería utilizarse para el almacenamiento de clave-valor con una escalabilidad mayor que Azure Table Storage?

**Pregunta (Inglés):** Which Azure Cosmos DB API should be used for key-value storage with greater scalability than Azure Table Storage?

* **Opciones:**

  * A) Azure Cosmos DB for PostgreSQL

  * B) Azure Cosmos DB for Apache Gremlin

  * **C) Azure Cosmos DB for Table**

* **Explicación:** *Azure Cosmos DB for Table* mantiene exactamente los mismos patrones de desarrollo de clave-valor que el almacenamiento clásico de Azure, pero introduce los beneficios avanzados de rendimiento dedicado, latencia ultra-baja garantizada por SLA y escalabilidad horizontal masiva.

**9\. Pregunta (Español):** Su organización está migrando su base de datos Apache Cassandra existente a Azure Cosmos DB. ¿Qué API de Azure Cosmos DB debería utilizarse para garantizar la compatibilidad?

**Pregunta (Inglés):** Your organization is migrating its existing Apache Cassandra database to Azure Cosmos DB. Which Azure Cosmos DB API should be used to ensure compatibility?

* **Opciones:**

  * **A) Azure Cosmos DB for Apache Cassandra**

  * B) Azure Cosmos DB for NoSQL

  * C) Azure Cosmos DB for PostgreSQL

* **Explicación:** Al activar la API para Apache Cassandra, Cosmos DB emula el protocolo de red de Cassandra de forma nativa. Esto permite que el código de la aplicación de origen migrada siga funcionando de inmediato sin modificaciones, requiriendo solo una actualización en la cadena de conexión.

## Resumen

Azure Cosmos DB proporciona una solución de base de datos a escala global para datos no relacionales.

En este módulo, aprendiste a:

* Describa las características y capacidades clave de Azure Cosmos DB.

* Identificar las API de Azure Cosmos DB

* Aprovisione y utilice una instancia de Azure Cosmos DB.