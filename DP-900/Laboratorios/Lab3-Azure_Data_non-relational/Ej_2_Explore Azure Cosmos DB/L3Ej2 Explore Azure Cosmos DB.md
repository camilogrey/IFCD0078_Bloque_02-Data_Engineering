# **Explore Azure Cosmos DB**

## **1\.**     **📌Objetivo del Laboratorio**

El objetivo principal de este ejercicio es aprender a aprovisionar, configurar y explorar los diferentes servicios de almacenamiento no relacional dentro de una cuenta de Azure Storage. El laboratorio busca que experimentes de forma directa cómo cambia el comportamiento de la gestión de archivos al pasar de un almacenamiento plano (Flat Namespace) a uno jerárquico (Hierarchical Namespace / Data Lake Gen2) , además de comprender el uso de recursos compartidos de archivos.

## **2\.**     **Resumen del Ejercicio**

El documento guía al usuario a través de un escenario dividido en 4 fases principales:

2.1.  **Creación de la Infraestructura:** Se aprovisiona una cuenta de almacenamiento estándar dentro de un grupo de recursos controlado (dp900-lab-rg).  
2.2.  **Exploración de Blob Storage (Estructura Plana):** Se crea un contenedor de datos privado y se experimenta con la subida de un archivo JSON. Aquí se demuestra que, en el Blob Storage tradicional, las carpetas son virtuales (no existen objetos de directorio reales, sino solo prefijos en el nombre del archivo).  
2.3.  **Manipulación de Documentos JSON:** Exploración de la naturaleza libre de esquema (*schema-free*) de NoSQL al insertar manualmente un nuevo artículo (un casco de carretera) con campos personalizados y observar cómo Cosmos DB añade metadatos del sistema automáticamente.  
2.4.  **Ejecución de Consultas:** Creación y ejecución de sentencias de filtrado utilizando una sintaxis familiar muy similar a SQL estándar (como la función CONTAINS), para extraer colecciones específicas de documentos en tiempo real.  
2.5.  **Gobierno de Costes:** Eliminación controlada del grupo de recursos para garantizar que no queden cargos de computación activos.

## **3\.**     **Puntos Clave a Tener en Cuenta**

* **Elección Inalterable de la API:** El documento advierte que la API seleccionada (en este caso, *Azure Cosmos DB for NoSQL*) **no se puede cambiar** después de crear la cuenta. Si te equivocas aquí, tendrás que borrar el recurso y empezar de nuevo.  
* **El Concepto de Clave de Partición (*Partition Key*):** El ejercicio configura automáticamente /categoryId. Recuerda que en producción, elegir una buena clave de partición es la decisión de diseño más importante en Cosmos DB, ya que define cómo se distribuyen los datos y la velocidad de escalado horizontal.  
* **Metadatos Automáticos:** Al guardar un registro JSON, verás que aparecen campos que tú no escribiste (como \_rid, \_self, \_etag, \_ts). Estos son identificadores internos y marcas de tiempo Unix que Cosmos DB inyecta para controlar la consistencia y la indexación.  
* **Workload de tipo Learning:** Asegúrate de seleccionar **Learning** en la primera pestaña. Esto activa límites por defecto ideales para estudiantes y evita que consumas crédito innecesario por aprovisionar rendimiento excesivo.

## **4\.**     **Ejecución**

En este laboratorio, crearás tu primera base de datos **NoSQL** usando **Azure Cosmos DB.** Las bases de datos "NoSQL" almacenan los datos de forma flexible, en lugar de las estrictas tablas de filas y columnas de una base de datos relacional. Cosmos DB almacena cada dato como un **elemento JSON** (un formato de texto sencillo que lista propiedades y sus valores, como )."price": 48.74

Crearás ("provisionarás") una cuenta de Cosmos DB, añadirás algunos datos de muestra, los verás como JSON y luego ejecutarás consultas sencillas tipo SQL para encontrar lo que buscas. No te preocupes si estos términos son nuevos, cada paso se explica sobre la marcha.

Este laboratorio tardará aproximadamente **30** minutos en completarse.

### **4.1.**                         **Antes de empezar**

Necesitarás una [suscripción a Azure](https://azure.microsoft.com/free) en la que tengas acceso a nivel administrativo. Si no tienes uno, puedes registrarte gratis usando el enlace de arriba.

***¿Qué es Azure?** Azure es la plataforma en la nube de Microsoft. En lugar de comprar y ejecutar tu propio ordenador servidor, alquilas recursos informáticos (como una base de datos) a Microsoft y los usas a través de internet. El **portal de Azure** es la web que usas para crear y gestionar esos recursos.*

### **4.2.**                         **Crea una cuenta de Cosmos en la base de datos**

"Provisionar" simplemente significa crear y configurar un nuevo recurso. Para usar Cosmos DB, primero creas una cuenta en Cosmos DB. En este laboratorio, crearás una cuenta que utiliza **Azure Cosmos DB para NoSQL**, la opción diseñada para almacenar y consultar datos JSON.

1. En el portal de Azure, **selecciona \+ Crear un recurso** en la esquina superior izquierda y busca . En los resultados, selecciona **Azure Cosmos DB** y **selecciona Crear**.Azure Cosmos DB

![image1](/DP-900/Laboratorios/Lab3-Azure_Data_non-relational/Ej_2_Explore%20Azure%20Cosmos%20DB/imagenes/0CreateAzureCosmosDB.png)

2. En la **base de datos Azure Cosmos para NoSQL**, selecciona **Crear**.

![image2](/DP-900/Laboratorios/Lab3-Azure_Data_non-relational/Ej_2_Explore%20Azure%20Cosmos%20DB/imagenes/1CosmosDBNoSQL1.png)

***Consejo**: La cuenta es el nivel superior para tus recursos de Cosmos DB. Elegir Azure Cosmos DB para NoSQL te permite almacenar y consultar datos JSON con un lenguaje de consulta sencillo, similar a SQL.*

3. Introduce los siguientes datos y luego **selecciona Revisar \+ crear**:  
   * **Tipo de carga de trabajo**: Aprendizaje  
   * **Suscripción**: Si usas un sandbox, selecciona *Suscripción de Concierge*. Si no, selecciona tu suscripción a Azure.  
   * **Grupo de recursos**: Si usas un sandbox, selecciona el grupo de recursos existente (que tendrá un nombre como *learn-xxxx...*). Si no, crea un nuevo grupo de recursos con un nombre que elijas.  
   * **Nombre de cuenta**: Introduce un nombre único  
   * **Zonas de disponibilidad**: Desactivar  
   * **Ubicación**: Elige cualquier ubicación recomendada  
   * **Modo de capacidad**: Rendimiento provisionado  
   * **Solicitar Descuento de nivel gratuito**: Seleccionar Solicitar si está disponible  
   * **Límite total de rendimiento de la cuenta**: No seleccionado

***¿Por qué estas decisiones?***

*Configuramos el **tipo de carga de trabajo** en Aprendizaje porque viene con valores predeterminados para principiantes que facilitan la configuración y mantienen bajos los costes. El nombre de tu **cuenta** debe ser único en todo el servicio, ya que pasa a formar parte de la URL de tu servicio. Elegimos una **ubicación** cerca de ti para que tus pruebas se ejecuten más rápido; Las ubicaciones que veas dependerán de tu suscripción y de si ciertas zonas de disponibilidad están activadas. Para **el modo capacidad**, optamos por el rendimiento provisionado para que el rendimiento se mantenga predecible durante este laboratorio corto—aunque el modo serverless puede funcionar bien si solo lo necesitas ocasionalmente. Si el **nivel gratuito** está disponible, lo usaremos para que puedas experimentar sin acumular cargos. Por último, mantenemos desactivada la opción de "**limitar el rendimiento total de cuentas**" para que nada se ralentice inesperadamente mientras trabajas.*

![image3](/DP-900/Laboratorios/Lab3-Azure_Data_non-relational/Ej_2_Explore%20Azure%20Cosmos%20DB/imagenes/2CreateCosmosDBNoSQL1.png)

4. Cuando la configuración haya sido validada, seleccione **Crear**.

![image4](/DP-900/Laboratorios/Lab3-Azure_Data_non-relational/Ej_2_Explore%20Azure%20Cosmos%20DB/imagenes/4CreateCosmosDBNoSQL1.png)


***Consejo**: Azure Portal estimará cuánto tiempo tardará en provisionar esta instancia de Azure Cosmos DB. El tiempo estimado de creación se calcula en función de la ubicación que hayas seleccionado.*


5. Espera a que termine el despliegue. Luego ve al recurso desplegado.

![image5](/DP-900/Laboratorios/Lab3-Azure_Data_non-relational/Ej_2_Explore%20Azure%20Cosmos%20DB/imagenes/4CreateCosmosDBNoSQL2.png)

### **4.3.**                         **Crea una base de datos de ejemplo**

*Durante este proceso, cierra cualquier consejo que aparezca en el portal*.

1. En la página de tu nueva cuenta de Cosmos DB, en el panel de la izquierda, selecciona **Explorador de datos**.

![image6](/DP-900/Laboratorios/Lab3-Azure_Data_non-relational/Ej_2_Explore%20Azure%20Cosmos%20DB/imagenes/5CosmosDBNoOverView1.png)


 

2. En la página **del Explorador de Datos**, **selecciona Iniciar inicio rápido**.

![image7](/DP-900/Laboratorios/Lab3-Azure_Data_non-relational/Ej_2_Explore%20Azure%20Cosmos%20DB/imagenes/5CosmosDBNoOverView2.png)
 

***Consejo**: Quick Start crea una base de datos funcional, contenedor y datos de muestra para que puedas practicar añadir y consultar elementos sin diseñar un esquema primero.*

3. En el **panel de Nuevo Contenedor**, revisa la configuración pre-rellenada de la base de datos de ejemplo (una base de datos llamada **SampleDB,** un contenedor llamado **SampleContainer** y una clave de partición **de /categoryId**) y luego **selecciona OK.** Un breve tutorial guiado puede aparecer junto al panel; puedes seguirlo paso a paso con **Siguiente** o simplemente seleccionar **OK** para continuar.

![image8](/DP-900/Laboratorios/Lab3-Azure_Data_non-relational/Ej_2_Explore%20Azure%20Cosmos%20DB/imagenes/5CosmosDBNoOverView3.png)

 

***¿Qué es una clave de partición?** Cuando creas un contenedor, Azure Cosmos DB pide una clave de **partición**, una propiedad en tus datos (por ejemplo, ) que utiliza para agrupar elementos relacionados. Cosmos DB distribuye estos grupos por el almacenamiento y el cálculo entre bastidores para que tu base de datos se mantenga rápida a medida que crece. No necesitas elegir una aquí porque Quick Start elige una clave de partición sensata para ti, pero en un proyecto real elegir una buena clave de partición, con muchos valores distintos que te guían para filtrar, es una de las decisiones de diseño más importantes que tomarás.categoryId*

4. Observa el estado en el panel inferior de la pantalla hasta que se haya creado la base de datos **SampleDB** y su contenedor **SampleContainer** (lo que puede tardar un minuto aproximadamente).

![image9](/DP-900/Laboratorios/Lab3-Azure_Data_non-relational/Ej_2_Explore%20Azure%20Cosmos%20DB/imagenes/6SampleDB.png)

 

**Visualizar y crear elementos**

1. En la página del Explorador de Datos, amplía la base de datos **de SampleDB** y el contenedor **SampleContainer**, y selecciona **Elementos** para ver una lista de elementos en el contenedor. Los elementos representan datos de productos, cada uno con un id único y otras propiedades. Selecciona cualquier elemento para ver una representación JSON de sus datos en el panel de la derecha.

![image10](/DP-900/Laboratorios/Lab3-Azure_Data_non-relational/Ej_2_Explore%20Azure%20Cosmos%20DB/imagenes/6SampleDB1.png)

 

2. En la parte superior de la página, selecciona **Nuevo elemento** para crear un nuevo elemento en blanco.  
3. Modifica el JSON del nuevo elemento de la siguiente manera y luego **selecciona Guardar**.


```JSON
{

	"name": "Road Helmet,45",

	"id": "123456789",

    "categoryId": "123456789",

	"SKU": "AB-1234-56",

    "description": "The product called \\"Road Helmet,45\\" ",

	"price": 48.74

}
```
![imagen11](/DP-900/Laboratorios/Lab3-Azure_Data_non-relational/Ej_2_Explore%20Azure%20Cosmos%20DB/imagenes/7CrearItem.png)

 

4. Tras guardar el nuevo elemento, observa que se añaden automáticamente propiedades adicionales de metadatos.

***Consejo**: Cosmos DB almacena los elementos como JSON (JavaScript Object Notation), así que puedes añadir campos que se ajusten a tu escenario sin un esquema rígido. Debe ser único dentro del contenedor. Después de guardar, Cosmos DB añade propiedades del sistema (como marcas de tiempo e identificadores internos) para ayudar a gestionar y optimizar tus datos:id*

* *\_rid — El ID interno de recurso que utiliza Cosmos DB para identificar el elemento internamente.*  
  * *\_self — El enlace completo al recurso del objeto.*  
  * *\_etag — La etiqueta de entidad utilizada para comprobaciones de concurrencia optimista.*  
  * *\_ts — The Unix timestamp (in seconds) when the item was last modified.*  
  * *\_attachments — A link to the document’s attachments (if any).*

Query the database

1. In the **Data Explorer** page, select the **New SQL Query** icon.



2. In the SQL Query editor, review the default query () and use the **Execute Query** button to run it.
```sql
SELECT * FROM c  
```
3. Review the results, which includes the full JSON representation of all items.  

![imagen12](/DP-900/Laboratorios/Lab3-Azure_Data_non-relational/Ej_2_Explore%20Azure%20Cosmos%20DB/imagenes/8Select.png)

4. Modify the query as follows:
```sql
SELECT * FROM c WHERE CONTAINS(c.name,"Helmet")
```
***Tip**: The NoSQL API uses familiar, SQL-like queries to search JSON documents. lists all items, and filters by text inside a property—useful for quick searches without extra setup. SELECT * FROM c CONTAINS*

5. Use the **Execute Query** button to run the revised query and review the results, which includes JSON entities for any items with a **name** field containing the text “Helmet”.

![imagen13](/DP-900/Laboratorios/Lab3-Azure_Data_non-relational/Ej_2_Explore%20Azure%20Cosmos%20DB/imagenes/9Select.png)

 

6. Cierra el editor de consultas SQL y descarta tus cambios.

Has visto cómo crear y consultar entidades JSON en una base de datos de Cosmos DB usando la interfaz del explorador de datos en el portal de Azure. En un escenario real, un desarrollador de aplicaciones usaría uno de los muchos kits de desarrollo de software específicos de un lenguaje de programación (SDK) para llamar a la API NoSQL y trabajar con los datos de la base de datos.

### **4.4.**                         **Limpieza**

Cuando termines de explorar Azure Cosmos DB, deberías eliminar los recursos que creaste para no tener más costes.

1. En el portal de Azure, accede al **grupo de recursos** que contiene tu cuenta de Cosmos DB.  
2. Selecciona **Eliminar grupo de recursos**, confirma la eliminación introduciendo el nombre del grupo de recursos y **selecciona Eliminar**.

![imagen 14](/DP-900/Laboratorios/Lab3-Azure_Data_non-relational/Ej_2_Explore%20Azure%20Cosmos%20DB/imagenes/10Limpieza.png)

***TIP:** Eliminar el grupo de recursos elimina la cuenta de base de datos de Cosmos y todo lo que hay dentro de ella en un solo paso. Esta es la forma más rápida de asegurarte de que nada quede funcionando y que cueste dinero.*

En este laboratorio, creaste una cuenta de base de datos de Azure Cosmos, añadiste elementos JSON y los consultaste usando un lenguaje similar a SQL. ¡Has dado tus primeros pasos con los datos NoSQL en la nube\!
