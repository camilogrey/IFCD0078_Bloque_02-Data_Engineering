# Introducción a Microsoft Azure: datos no relacionales en Azure
Los datos no relacionales son una forma común en que las aplicaciones almacenan y consultan datos sin la complejidad de un esquema relacional.
En Microsoft Azure, puede usar Azure Storage y Azure Cosmos DB para crear almacenes de datos seguros y altamente escalables para datos no relacionales.
Esta ruta de aprendizaje le ayudará a prepararse para la certificación [Azure Data Fundamentals](https://learn.microsoft.com/en-us/certifications/azure-data-fundamentals?azure-portal=true).

**Explore Azure Storage para datos no relacionales**

Azure Storage es un servicio fundamental de Microsoft Azure que se utiliza habitualmente para almacenar datos no relacionales.

**Objetivos de aprendizaje**

En este módulo aprenderás a:

* Describa las características y capacidades del almacenamiento de blobs de Azure.

* Describa las características y capacidades de Azure Data Lake Storage Gen2.

* Describa las características y capacidades de Microsoft OneLake en Fabric.

* Describa las características y capacidades del almacenamiento de archivos de Azure.

* Describa las características y capacidades del almacenamiento de tablas de Azure.

* Configure y explore una cuenta de Azure Storage.

**Este módulo forma parte de estas rutas de aprendizaje.**

* [Introducción a Microsoft Azure: datos no relacionales en Azure](https://learn.microsoft.com/training/paths/azure-data-fundamentals-explore-non-relational-data/)

## INDICE

* [Introducción](#introducción)

* [Explora el almacenamiento de blobs de Azure](#explora-el-almacenamiento-de-blobs-de-azure)

* [Explora Azure Data Lake Storage Gen2](#explora-azure-data-lake-storage-gen2)

* [Explora Microsoft OneLake en Fabric](#explora-microsoft-onelake-en-fabric)

* [Explorar Azure Files](#explorar-azure-files)

* [Explorar Azure Tables](#explorar-azure-tables)

* [Evaluación del módulo](#evaluación-del-módulo)

* [Resumen](#resumen)

## Introducción {#introducción}

La mayoría de las aplicaciones de software necesitan almacenar datos. A menudo, estos datos se almacenan en una base de datos relacional, donde se organizan en tablas relacionadas y se gestionan mediante el Lenguaje de Consulta Estructurada (SQL). Sin embargo, muchas aplicaciones no requieren la estructura rígida de una base de datos relacional y utilizan almacenamiento no relacional (conocido comúnmente como NoSQL).

Azure Storage y Microsoft OneLake ofrecen diversas opciones para almacenar datos en la nube. En este módulo, explorará las capacidades fundamentales de Azure Storage y Microsoft OneLake, y aprenderá cómo se utilizan para dar soporte a aplicaciones que requieren almacenes de datos no relacionales.

[> !NOTE]** Nota**

Reconocemos que cada persona aprende de manera diferente. Puedes completar este módulo en formato de video o leer el contenido en formato de texto e imágenes. El texto contiene más detalles que los videos, por lo que en algunos casos te resultará útil como material complementario a la presentación en video.

## Explora el almacenamiento de blobs de Azure {#explora-el-almacenamiento-de-blobs-de-azure}

Azure Blob Storage es un servicio que permite almacenar grandes cantidades de datos no estructurados como objetos binarios de gran tamaño, o *blobs* , en la nube. Los blobs son una forma eficiente de almacenar archivos de datos en un formato optimizado para el almacenamiento en la nube, y las aplicaciones pueden leerlos y escribirlos mediante la API de Azure Blob Storage.

![Captura de pantalla de un contenedor de almacenamiento de blobs de Azure con dos blobs.](imagenes3_1/Imagen1.png)

En una cuenta de almacenamiento de Azure, los blobs se almacenan en *contenedores* . Un contenedor permite agrupar blobs relacionados de forma práctica. El acceso para leer y escribir blobs dentro de un contenedor se controla directamente en el contenedor. La autenticación con Microsoft Entra ID (el servicio de administración de identidades y accesos de Azure) es el método de inicio de sesión recomendado para Azure Blob Storage, ya que permite asignar permisos precisos mediante el control de acceso basado en roles (RBAC) de Azure, un sistema que controla quién puede realizar acciones específicas en los recursos de Azure.

Dentro de un contenedor, puede organizar los blobs en una jerarquía de carpetas virtuales, similar a los archivos en un sistema de archivos en disco. Sin embargo, por defecto, estas carpetas son simplemente una forma de usar el carácter "/" en el nombre del blob para organizarlos en espacios de nombres. Las carpetas son puramente virtuales y no se pueden realizar operaciones a nivel de carpeta para controlar el acceso ni realizar operaciones masivas.

Azure Blob Storage admite tres tipos diferentes de blobs:

* **Bloques binarios** . Un bloque binario se maneja como un conjunto de bloques. Cada bloque puede variar de tamaño, hasta 4000 MiB. Un bloque binario puede contener hasta 190,7 TiB (4000 MiB x 50 000 bloques). El bloque es la cantidad mínima de datos que se puede leer o escribir como una unidad individual. Los bloques binarios son ideales para almacenar objetos binarios grandes y discretos que cambian con poca frecuencia.

* **Blobs de página** . Un blob de página se organiza como una colección de páginas de tamaño fijo de 512 bytes. Está optimizado para admitir operaciones de lectura y escritura aleatorias; si es necesario, puede recuperar y almacenar datos de una sola página. Un blob de página puede almacenar hasta 8 TB de datos. Azure utiliza blobs de página para implementar el almacenamiento de disco virtual para máquinas virtuales.

* **Añadir blobs** . Un blob de adición es un blob de bloques optimizado para admitir operaciones de adición. Solo se pueden añadir bloques al final de un blob de adición; no se admite la actualización ni la eliminación de bloques existentes. Cada bloque puede variar de tamaño, hasta 4 MB. El tamaño máximo de un blob de adición es de poco más de 195 GB .

![Diagrama que explica los diferentes tipos de almacenamiento de blobs.][image2]

Imagen2

* El almacenamiento de blobs proporciona cuatro niveles de acceso, que ayudan a equilibrar la latencia de acceso y el coste de almacenamiento:

* El nivel **Hot** es el predeterminado. Este nivel se utiliza para los blobs a los que se accede con frecuencia. Los datos de los blobs se almacenan en medios de alto rendimiento.

* El nivel **Cool** tiene costos de almacenamiento más bajos, pero costos de acceso más altos en comparación con el nivel Hot. Utilice el nivel Cool para datos a los que se accede con poca frecuencia. Los datos deben permanecer en el nivel Cool durante un mínimo de 30 días para evitar penalizaciones por eliminación anticipada. Es común que los blobs recién creados se accedan con frecuencia inicialmente, pero con menos frecuencia a medida que pasa el tiempo. En estos casos, puede crear el blob en el nivel Hot y migrarlo al nivel Cool posteriormente. También puede migrar un blob del nivel Cool de vuelta al nivel Hot.

* El nivel **Cold** está optimizado para almacenar datos que se acceden o modifican con poca frecuencia, pero que requieren una recuperación rápida. El nivel Cold tiene menores costos de almacenamiento y mayores costos de acceso en comparación con el nivel Cool. Los datos deben permanecer en el nivel Cold durante un mínimo de 90 días para evitar penalizaciones por eliminación anticipada. Utilice este nivel para datos como copias de seguridad a corto plazo, datos de recuperación ante desastres o grandes conjuntos de datos que requieren un almacenamiento rentable.

* El nivel **de archivo** ofrece el menor coste de almacenamiento, pero con una mayor latencia. Está diseñado para datos históricos que no deben perderse, pero que solo se requieren ocasionalmente. Los datos deben permanecer en el nivel de archivo durante un mínimo de 180 días para evitar penalizaciones por eliminación anticipada. Los blobs en el nivel de archivo se almacenan efectivamente en estado sin conexión. La latencia de lectura típica para los niveles Hot, Cool y Cold es de unos pocos milisegundos, pero para el nivel de archivo, puede tardar hasta 15 horas en que los datos estén disponibles. Para recuperar un blob del nivel de archivo, debe cambiar el nivel de acceso a Hot, Cool o Cold. El blob se rehidratará. Solo podrá leer el blob cuando el proceso de rehidratación haya finalizado.

![Diagrama que explica los diferentes niveles de acceso al almacenamiento de blobs.][image3]

Imagen3

* Puedes crear directivas de gestión del ciclo de vida para los blobs en una cuenta de almacenamiento. Una directiva de gestión del ciclo de vida puede mover automáticamente un blob de la capa de acceso frecuente a la de acceso menos frecuente y, finalmente, a la de archivo, a medida que envejece y se usa con menos frecuencia (la directiva se basa en el número de días transcurridos desde su modificación). Una directiva de gestión del ciclo de vida también puede configurar la eliminación de blobs obsoletos.

* Azure Storage también ofrece opciones de redundancia integradas para mantener sus datos altamente disponibles y protegidos contra fallos. **El almacenamiento localmente redundante (LRS)** mantiene tres copias de sus datos en un único centro de datos. **El almacenamiento con redundancia de zona (ZRS)** distribuye las copias entre tres zonas de disponibilidad en la región principal, de modo que sus datos permanecen accesibles incluso si una zona deja de funcionar. Para protegerse contra desastres regionales, **el almacenamiento georredundante (GRS)** y **el almacenamiento georredundante de zona (GZRS)** replican sus datos de forma asíncrona en una región secundaria situada a cientos de kilómetros de distancia. También puede habilitar el acceso de lectura a la región secundaria (RA-GRS o RA-GZRS) para que su aplicación pueda leer datos de la región secundaria incluso antes de que se produzca una conmutación por error.

Imagen4

![Diagrama que explica las diferentes opciones de redundancia para el almacenamiento de blobs.][image4]

## Explora Azure Data Lake Storage Gen2 {#explora-azure-data-lake-storage-gen2}

Azure Data Lake Storage Gen2 es una solución de lago de datos a escala de nube integrada en Azure Storage. Combina la escalabilidad y el control de costes de Azure Blob Storage (incluidos los niveles de almacenamiento y la gestión del ciclo de vida) con un sistema de archivos jerárquico compatible con los principales sistemas de análisis.

![Captura de pantalla de un contenedor de almacenamiento de blobs de Azure con un espacio de nombres jerárquico.][image5]

Imagen5.png

Sistemas como Azure Databricks pueden montar un sistema de archivos distribuido alojado en Azure Data Lake Storage Gen2 y usarlo para procesar grandes volúmenes de datos. Los inquilinos de Microsoft Fabric aprovisionan automáticamente OneLake, que se basa en Azure Data Lake Storage Gen2.

El espacio de nombres jerárquico también permite listas de control de acceso (ACL) compatibles con POSIX, de modo que se pueden establecer permisos de lectura, escritura y ejecución detallados en archivos y carpetas individuales, independientemente del modelo de control de acceso basado en roles (RBAC) más amplio de Azure.

Para crear un sistema de archivos de Azure Data Lake Storage Gen2, debe habilitar la opción **Espacio de nombres jerárquico** de una cuenta de Azure Storage. Puede hacerlo al crear la cuenta de almacenamiento o actualizar una cuenta existente para que admita un espacio de nombres jerárquico. Tenga en cuenta que la actualización es irreversible: una vez actualizada, no podrá revertir la cuenta de almacenamiento a un espacio de nombres plano.

## Explora Microsoft OneLake en Fabric {#explora-microsoft-onelake-en-fabric}

Microsoft Fabric aprovisiona automáticamente OneLake, basado en Azure Data Lake Gen 2\.

![Diagrama que muestra la función y la estructura de OneLake.][image6]

Imagen6.png

OneLake es un lago de datos lógico, unificado y único, diseñado para toda su organización. OneLake se incluye automáticamente con cada inquilino de Microsoft Fabric y funciona como repositorio central para todos sus datos analíticos. Ya sean estructurados o no estructurados, OneLake admite cualquier tipo de archivo y le permite utilizar los mismos datos en varios motores analíticos sin necesidad de moverlos ni duplicarlos.

### Principales ventajas de OneLake

* **Lago de datos para toda la organización.** Antes de OneLake, era común crear varios lagos de datos para diferentes grupos empresariales. Ahora, OneLake ofrece una solución colaborativa que garantiza que toda la organización comparta un único lago de datos.

* **Propiedad distribuida y colaboración:** Dentro de un inquilino, puede crear espacios de trabajo que permiten a diferentes partes de su organización administrar sus elementos de datos. Esta propiedad distribuida fomenta la colaboración al tiempo que mantiene los límites de gobernanza.

* **OneLake, basado** en Azure Data Lake Storage (ADLS) Gen2, almacena datos en formato Delta Parquet, un formato de archivo abierto y eficiente ampliamente utilizado para datos analíticos. Es compatible con las API y los SDK de ADLS Gen2 existentes, lo que garantiza su compatibilidad con sus aplicaciones actuales.

* **Fácil de navegar** Es muy sencillo navegar por los datos de OneLake desde Windows utilizando [el explorador de archivos de OneLake](https://learn.microsoft.com/en-us/fabric/onelake/onelake-file-explorer) .

Para obtener más información, consulte [OneLake, el OneDrive para datos](https://learn.microsoft.com/en-us/fabric/onelake/onelake-overview) .

## Explorar Azure Files {#explorar-azure-files}

Muchos sistemas locales que comprenden una red de ordenadores internos utilizan recursos compartidos de archivos. Un recurso compartido de archivos permite almacenar un archivo en un ordenador y otorgar acceso a ese archivo a usuarios y aplicaciones que se ejecutan en otros ordenadores. Esta estrategia puede funcionar bien para ordenadores en la misma red de área local, pero no es escalable a medida que aumenta el número de usuarios o si estos se encuentran en diferentes ubicaciones.

Azure Files es, en esencia, una forma de crear recursos compartidos de red en la nube, como los que se suelen encontrar en organizaciones locales, para que varios usuarios tengan acceso a documentos y otros archivos. Al alojar los recursos compartidos de archivos en Azure, las organizaciones pueden eliminar los costos de hardware y mantenimiento, y beneficiarse de la alta disponibilidad y el almacenamiento en la nube escalable para sus archivos.

![Diagrama de una cuenta de almacenamiento de Azure con un recurso compartido de Azure Files.][image7]

Imagen7.png

Puedes crear Azure File Storage en una cuenta de almacenamiento. Azure Files te permite compartir grandes cantidades de datos en una sola cuenta de almacenamiento: hasta 256 TiB para cuentas basadas en SSD e incluso más para cuentas basadas en HDD. Estos datos se pueden distribuir entre cualquier número de recursos compartidos de archivos en la cuenta. El tamaño máximo de un solo archivo es de 4 TiB, pero puedes establecer cuotas para limitar el tamaño de cada recurso compartido por debajo de esta cifra. Actualmente, Azure File Storage admite hasta 2000 identificadores simultáneos por archivo o directorio.

Una vez creada una cuenta de almacenamiento, puede cargar archivos a Azure File Storage mediante el portal de Azure o herramientas como la utilidad **AzCopy** . También puede usar el servicio **Azure File Sync** para sincronizar copias locales en caché de archivos compartidos con los datos de Azure File Storage.

![Diagrama de la arquitectura de almacenamiento de archivos de Azure.][image8]

Imagen8.png

Azure File Storage ofrece dos niveles de almacenamiento. El nivel **HDD** utiliza hardware basado en discos duros en un centro de datos, y el nivel **SSD** utiliza discos de estado sólido. El nivel **SSD** ofrece un mayor rendimiento, pero tiene un coste más elevado.

Azure Files admite dos protocolos comunes para compartir archivos en red:

* **El uso compartido de archivos mediante el protocolo Server Message Block (SMB)** se utiliza habitualmente en múltiples sistemas operativos (Windows, Linux, macOS).

* **Los recursos compartidos del Sistema de Archivos de Red (NFS)** se utilizan en Linux (kernel 4.3 o posterior). Los recursos compartidos de archivos NFS de Azure no son compatibles con Windows ni macOS. Para crear un recurso compartido NFS, debe usar una cuenta de almacenamiento de nivel SSD y crear y configurar una red virtual que permita controlar el acceso al recurso compartido.

## Explorar Azure Tables {#explorar-azure-tables}

**Azure Table Storage** es una solución de almacenamiento **NoSQL** que utiliza tablas que contienen elementos de datos **clave/valor** . Cada elemento está representado por una fila que contiene columnas para los campos de datos que deben almacenarse.

** Nota**

Los conceptos de esta unidad también se aplican a **Azure Cosmos DB for Table** , un servicio más reciente que almacena datos en el mismo formato clave/valor, pero que ofrece mayor rendimiento y disponibilidad global. Para nuevas cargas de trabajo, Azure Cosmos DB for Table es la opción recomendada.

Sin embargo, no se deje engañar pensando que una tabla de Azure Table Storage es como una tabla en una base de datos relacional. Una tabla de Azure permite almacenar datos semiestructurados. Todas las filas de una tabla deben tener una clave única (compuesta por una clave de **partición** y una **clave de fila** ), y cuando se modifican los datos en una tabla, una columna **de marca de tiempo** registra la fecha y la hora de la modificación; pero aparte de eso, las columnas de cada fila pueden variar. Las tablas de Azure Table Storage no tienen el concepto de claves foráneas, relaciones, procedimientos almacenados, vistas u otros objetos que podría encontrar en una base de datos relacional.

Los datos en Azure Table Storage suelen estar **desnormalizados** , de modo que cada fila contiene la información completa de una entidad lógica. Por ejemplo, una tabla con información de clientes podría almacenar el nombre, el apellido, uno o más números de teléfono y una o más direcciones de cada cliente. El número de campos en cada fila puede variar según la cantidad de números de teléfono y direcciones de cada cliente, así como los detalles registrados para cada dirección. En una base de datos relacional, esta información se distribuiría en varias filas de diferentes tablas.

![Diagrama de una cuenta de almacenamiento de Azure con tablas de Azure.][image9]

Imagen9.png

** Nota**

Los precios que aparecen en este diagrama son ficticios y se utilizan únicamente con fines ilustrativos.

Para garantizar un acceso rápido, Azure Table Storage divide una tabla en particiones. **El particionamiento** es un mecanismo para agrupar filas relacionadas según la clave de partición **(PartitionKey)** , un valor que se elige para reflejar una propiedad común de las filas relacionadas. Las filas que comparten la misma clave de partición se almacenan juntas. El particionamiento no solo ayuda a organizar los datos, sino que también puede mejorar la escalabilidad y el rendimiento de las siguientes maneras:

* Las particiones son independientes entre sí y pueden aumentar o disminuir de tamaño a medida que se agregan o eliminan filas de una partición. Una tabla puede contener cualquier número de particiones.

* Al buscar datos, puede incluir la clave de partición en los criterios de búsqueda. Esto ayuda a reducir el volumen de datos a examinar y mejora el rendimiento al disminuir la cantidad de operaciones de entrada/salida ( *lecturas* y *escrituras* ) necesarias para localizar los datos.

La clave en una tabla de Azure Table Storage consta de dos elementos: la **clave de partición** , que identifica la partición que contiene la fila, y una **clave de fila** , única para cada fila en la misma partición. Los elementos de la misma partición se almacenan en orden de clave de fila. Si una aplicación agrega una nueva fila a una tabla, Azure garantiza que se coloque en la posición correcta. Este esquema permite a una aplicación realizar rápidamente consultas **puntuales** para identificar una sola fila y consultas **de rango** para recuperar un bloque contiguo de filas en una partición.

## Evaluación del módulo {#evaluación-del-módulo}

Esta evaluación valora tu comprensión del módulo. A diferencia de antes, no recibirás comentarios sobre cada respuesta, solo se te indicará si son correctas o incorrectas. El objetivo es medir lo que has aprendido. Dedica tiempo a revisar los materiales del módulo antes de empezar.

Aquí tienes el módulo completo con las preguntas y todas sus opciones traducidas al español, marcando la respuesta correcta y con una explicación directa:

**1\. Su organización necesita organizar los datos de manera eficiente en Azure Data Lake Storage Gen2. ¿Qué característica se debe implementar para lograr este objetivo?**

* A) Flat namespace (Espacio de nombres plano).

* **B) Hierarchical namespace (Espacio de nombres jerárquico).**

* C) Page blobs (Blobs de páginas).

* **Explicación:** El espacio de nombres jerárquico permite organizar los archivos en carpetas y directorios reales del sistema. Esto optimiza drásticamente el rendimiento de las consultas analíticas de Big Data al evitar que el motor tenga que escanear miles de rutas de texto plano. 

**2\. Una aplicación no puede escribir datos en un contenedor de Azure Blob Storage. ¿Qué componente es más probable que falte o esté mal configurado?**

* **A) Proper access permissions (Permisos de acceso adecuados).**

* B) Page blob configuration (Configuración de blobs de páginas).

* C) Container's virtual folder structure (Estructura de carpetas virtuales del contenedor).

* **Explicación:** Por defecto, el acceso a los contenedores de Azure Storage está securizado y configurado como privado. Si una aplicación falla al escribir datos, la causa principal suele ser la ausencia o mala configuración de las claves de acceso, tokens SAS o roles RBAC. 

**3\. ¿Qué característica es única de Microsoft OneLake en comparación con otras soluciones de almacenamiento de Azure?**

* A) Support for hierarchical namespaces (Soporte para espacios de nombres jerárquicos).

* **B) Unified data lake for organization-wide analytics (Lago de datos unificado para la analítica de toda la organización).**

* C) Compatibility with SMB and NFS protocols (Compatibilidad con los protocolos SMB y NFS).

* **Explicación:** A diferencia de las soluciones PaaS tradicionales de Azure donde cada proyecto o departamento crea cuentas aisladas, OneLake proporciona un único gran lago de datos lógico y gobernado para toda la empresa bajo un modelo puramente SaaS. 

**4\. Al solucionar problemas de acceso a datos en Azure Data Lake Storage Gen2, ¿qué característica se debe examinar para garantizar la configuración correcta de la jerarquía?**

* A) Access tier policies (Políticas de niveles de acceso).

* B) Blob properties and types (Propiedades y tipos de blobs).

* **C) Hierarchical namespace configuration (Configuración del espacio de nombres jerárquico).**

* **Explicación:** Para que las capacidades de Data Lake Gen2 estén activas, el "espacio de nombres jerárquico" debe estar explícitamente habilitado en la configuración de la cuenta de almacenamiento. Si aparece como deshabilitado, la cuenta operará únicamente como un Blob Storage plano tradicional. 

**5\. ¿Qué solución de almacenamiento de Azure está optimizada para el acceso de alto rendimiento a datos binarios actualizados con frecuencia?**

* A) Append blobs in Azure Blob Storage (Blobs de anexión en Azure Blob Storage).

* **B) Page blobs in Azure Blob Storage (Blobs de páginas en Azure Blob Storage).**

* C) Azure Files (Archivos de Azure).

* **Explicación:** Los *Page blobs* están estructurados para operaciones de lectura y escritura aleatorias en bloques de memoria de 512 bytes. Esto los convierte en la solución optimizada para almacenar archivos binarios de actualización constante, como los discos virtuales VHD de las máquinas virtuales.

**6\. Al diseñar un flujo de trabajo para Azure Data Lake Storage Gen2, ¿qué característica clave se debe utilizar para gestionar grandes volúmenes de datos de manera eficiente?**

* A) Flat namespace to simplify data access (Espacio de nombres plano para simplificar el acceso a los datos).

* **B) Hierarchical namespace to structure data logically (Espacio de nombres jerárquico para estructurar los datos lógicamente).**

* C) Block blobs for all types of data storage (Blobs de bloques para todo tipo de almacenamiento de datos).

* **Explicación:** La estructura jerárquica permite que los motores analíticos de Big Data (como Spark o Hadoop) realicen operaciones atómicas de renombrado, borrado o listado recursivo sobre directorios enteros en milisegundos , superando las limitaciones de rendimiento del indexado plano. 

**7\. Un desarrollador no puede añadir datos a un "append blob" utilizando la API de Azure Blob Storage. ¿Cuál es la causa más probable de este problema?**

* A) The blob access tier is set to Archive (El nivel de acceso del blob está configurado en Archivo).

* **B) The blob has reached its maximum size limit (El blob ha alcanzado su límite de tamaño máximo).**

* C) The blob is part of a virtual folder (El blob forma parte de una carpeta virtual).

* **Explicación:** Aunque los *Append blobs* están diseñados específicamente para añadir bloques de datos de forma continua (operaciones de *append* muy comunes en sistemas de logs), tienen un límite de tamaño y de cantidad de bloques máximos establecidos por la API de Azure. Al tocar ese límite, el servicio bloquea nuevas escrituras.

**8\. ¿Qué solución de almacenamiento de Azure se debe utilizar para gestionar eficazmente grandes volúmenes de datos no estructurados con diferentes requisitos de acceso y costes?**

* A) Azure Table Storage (Almacenamiento de tablas de Azure).

* B) Azure Files (Archivos de Azure).

* **C) Azure Blob Storage (Almacenamiento de blobs de Azure).**

* **Explicación:** Azure Blob Storage es el servicio especializado y optimizado para alojar datos no relacionales y no estructurados (imágenes, documentos, archivos raw) a escala masiva , permitiendo segmentar la información en niveles de precio según la frecuencia de consulta. 

**9\. Su organización planea almacenar un gran volumen de archivos de imagen en Azure Blob Storage y desea una solución rentable para datos a los que rara vez se accede. ¿Qué nivel de acceso se debe seleccionar?**

* A) Cool tier (Nivel esporádico / frío).

* **B) Archive tier (Nivel de archivo).**

* C) Hot tier (Nivel frecuente / caliente).

* **Explicación:** El nivel *Archive* ofrece los costes de almacenamiento por gigabyte más bajos de toda la nube de Azure, lo que lo hace ideal para almacenar volúmenes masivos de imágenes de respaldo o datos históricos que se consultan de manera muy excepcional y que toleran tiempos de recuperación de varias horas.

## Resumen {#resumen}

Azure Storage es un servicio clave en Microsoft Azure y permite una amplia gama de escenarios y soluciones de almacenamiento de datos.

En este módulo, aprendiste a:

* Describa las características y capacidades del almacenamiento de blobs de Azure.

* Describa las características y capacidades de Azure Data Lake Gen2.

* Describa las características y capacidades de Microsoft OneLake.

* Describa las características y capacidades del almacenamiento de archivos de Azure.

* Describa las características y capacidades del almacenamiento de tablas de Azure.

* Aprovisiona y utiliza una cuenta de Azure Storage.
