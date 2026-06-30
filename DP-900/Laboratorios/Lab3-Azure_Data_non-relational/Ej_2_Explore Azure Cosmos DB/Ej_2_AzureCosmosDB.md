# Azure Cosmos DB (NoSQL)
# DP-900 - Lab 03 Ejercicio 2
# Laboratorio Práctico: Despliegue y Consulta en Azure Cosmos DB (NoSQL)

Este repositorio documenta el aprovisionamiento, inserción de documentos y consultas estructuradas utilizando la base de datos NoSQL global de Microsoft Azure.

---

## 📌 1. Objetivo del Laboratorio
El propósito de este ejercicio laboratorio es aprender a aprovisionar una cuenta de Azure Cosmos DB utilizando la API para NoSQL, interactuar con datos en formato JSON y ejecutar consultas estructuradas de tipo SQL a través de la herramienta interactiva Data Explorer dentro del portal de Azure.

## 2. Resumen del Ejercicio
El documento guía al estudiante a través de las siguientes fases operativas:
1. Aprovisionamiento NoSQL: Creación de una cuenta de Azure Cosmos DB configurando el tipo de carga de trabajo para aprendizaje (Learning) y la API nativa de NoSQL para el manejo elástico de documentos JSON.
2. Generación Automatizada de Muestras: Uso de la funcionalidad Launch quick start para instanciar de forma inmediata una base de datos (SampleDB) y un contenedor (SampleContainer) con datos de productos ya precargados.
3. Manipulación de Documentos JSON: Exploración de la naturaleza libre de esquema (schema-free) de NoSQL al insertar manualmente un nuevo artículo (un casco de carretera) con campos personalizados y observar cómo Cosmos DB añade metadatos del sistema automáticamente.
4. Ejecución de Consultas: Creación y ejecución de sentencias de filtrado utilizando una sintaxis familiar muy similar a SQL estándar (como la función CONTAINS), para extraer colecciones específicas de documentos en tiempo real.
5. Gobierno de Costes: Eliminación controlada del grupo de recursos para garantizar que no queden cargos de computación activos.

## 3. Puntos Clave a Tener en Cuenta (¡Críticos para tu práctica!)
Elección Inalterable de la API: El documento advierte que la API seleccionada (en este caso, Azure Cosmos DB for NoSQL) no se puede cambiar después de crear la cuenta. Si te equivocas aquí, tendrás que borrar el recurso y empezar de nuevo.
El Concepto de Clave de Partición (Partition Key): El ejercicio configura automáticamente /categoryId. Recuerda que en producción, elegir una buena clave de partición es la decisión de diseño más importante en Cosmos DB, ya que define cómo se distribuyen los datos y la velocidad de escalado horizontal.
Metadatos Automáticos: Al guardar un registro JSON, verás que aparecen campos que tú no escribiste (como _rid, _self, _etag, _ts). Estos son identificadores internos y marcas de tiempo Unix que Cosmos DB inyecta para controlar la consistencia y la indexación.
Workload de tipo Learning: Asegúrate de seleccionar Learning en la primera pestaña. Esto activa límites por defecto ideales para estudiantes y evita que consumas crédito innecesario por aprovisionar rendimiento excesivo.

# Explore Azure Cosmos DB

In this lab, you’ll create your first **NoSQL** database using **Azure Cosmos DB**. “NoSQL” databases store data in a flexible way, rather than in the strict rows-and-columns tables of a relational database. Cosmos DB stores each piece of data as a **JSON item** (a simple text format that lists properties and their values, like "price": 48.74).

You’ll create (“provision”) a Cosmos DB account, add some sample data, view it as JSON, and then run simple SQL-like queries to find what you’re looking for. Don’t worry if these terms are new, every step is explained as you go.

This lab will take approximately **30** minutes to complete.

## Before you start

You’ll need an [Azure subscription](https://azure.microsoft.com/free) in which you have administrative-level access. If you don’t have one, you can sign up for a free account using the link above.

***What is Azure?** Azure is Microsoft’s cloud platform. Instead of buying and running your own server computer, you rent computing resources (like a database) from Microsoft and use them over the internet. The **Azure portal** is the website you use to create and manage those resources.*

## Create a Cosmos DB account

“Provisioning” just means creating and setting up a new resource. To use Cosmos DB, you first create a Cosmos DB account. In this lab, you’ll create an account that uses **Azure Cosmos DB for NoSQL**, the option designed for storing and querying JSON data.

1. In the Azure portal, select **\+ Create a resource** at the top left, and search for Azure Cosmos DB. In the results, select **Azure Cosmos DB** and select **Create**.  
   ![][image1]  
2. In the **Azure Cosmos DB for NoSQL** tile, select **Create**.  
   ![][image2]

***Tip**: The account is the top level for your Cosmos DB resources. Choosing Azure Cosmos DB for NoSQL lets you store and query JSON data with a simple, SQL-like query language.*

3. Enter the following details, and then select **Review \+ create**:  
   * **Workload Type**: Learning  
   * **Subscription**: If you’re using a sandbox, select *Concierge Subscription*. Otherwise, select your Azure subscription.  
   * **Resource group**: If you’re using a sandbox, select the existing resource group (which will have a name like *learn-xxxx…*). Otherwise, create a new resource group with a name of your choice.  
   * **Account Name**: Enter a unique name  
   * **Availability Zones**: Disable  
   * **Location**: Choose any recommended location  
   * **Capacity mode**: Provisioned throughput  
   * **Apply Free-Tier Discount**: Select Apply if available  
   * **Limit total account throughput**: Unselected  
4. ***Why these choices?***  
5. *We’re setting the **workload type** to Learning because it comes with beginner-friendly defaults that make setup easier and keep costs low. Your **account name** needs to be unique across the whole service, since it becomes part of your service’s URL. We’re picking a **location** close to you so your tests run faster; which locations you see will depend on your subscription and whether certain availability zones are enabled. For **capacity mode**, we’re going with Provisioned throughput so performance stays predictable during this short lab—though Serverless can be fine if you only need it occasionally. If the **free tier** is available, we’ll use it so you can experiment without racking up charges. Finally, we’re keeping the “**limit total account throughput**” setting turned off so nothing gets slowed down unexpectedly while you work.*

*![Screenshot of the Create Azure Cosmos DB Account Basics tab with the workload type, subscription, resource group, account name, and location filled in.][image3]*

6. When the configuration has been validated, select **Create**.  
   ![][image4]

***Tip**: Azure portal will estimate how long it will take to provision this instance of Azure Cosmos DB. The estimated creation time is calculated based on the location you have selected.*

7. Wait for deployment to complete. Then go to the deployed resource.

## Create a sample database

*Throughout this procedure, close any tips that are displayed in the portal*.

1. On the page for your new Cosmos DB account, in the pane on the left, select **Data Explorer**.  
   ![][image5]  
2. In the **Data Explorer** page, select **Launch quick start**.  
   ![][image6]

***Tip**: Quick start creates a working database, container, and sample data so you can practice adding and querying items without designing a schema first.*

3. In the **New Container** pane, review the pre-populated settings for the sample database (a database named **SampleDB**, a container named **SampleContainer**, and a partition key of **/categoryId**), and then select **OK**. A short guided tutorial may appear alongside the pane; you can step through it with **Next** or simply select **OK** to continue.  
   ![][image7]  
4. ***What is a partition key?** When you create a container, Azure Cosmos DB asks for a **partition key**, a property in your data (for example, categoryId) that it uses to group related items together. Cosmos DB spreads these groups across the storage and compute behind the scenes so your database stays fast as it grows. You don’t need to choose one here because Quick Start picks a sensible partition key for you, but in a real project choosing a good partition key, one with many distinct values that your queries filter on, is one of the most important design decisions you’ll make.*  
5. Observe the status in the panel at the bottom of the screen until the **SampleDB** database and its **SampleContainer** container has been created (which may take a minute or so).

## View and create items

1. In the Data Explorer page, expand the **SampleDB** database and the **SampleContainer** container, and select **Items** to see a list of items in the container. The items represent product data, each with a unique id and other properties. Select any item to see a JSON representation of its data in the pane on the right.  
   ![Screenshot of the Items view listing sample product items, with the JSON of the selected item shown on the right.][image8]

![][image9]

2. At the top of the page, select **New Item** to create a new blank item.  
3. Modify the JSON for the new item as follows, and then select **Save**.  
4. code

{  
    "name": "Road Helmet,45",  
    "id": "123456789",  
    "categoryId": "123456789",  
    "SKU": "AB-1234-56",  
    "description": "The product called \\"Road Helmet,45\\" ",  
    "price": 48.74  
}

![][image10]

5. After saving the new item, notice that additional metadata properties are added automatically.

***Tip**: Cosmos DB stores items as JSON (JavaScript Object Notation), so you can add fields that fit your scenario without a rigid schema. The id must be unique within the container. After you save, Cosmos DB adds system properties (like timestamps and internal identifiers) to help manage and optimize your data:*

* *\_rid — The internal resource ID used by Cosmos DB to identify the item internally.*  
  * *\_self — The full resource link for the item.*  
  * *\_etag — The entity tag used for optimistic concurrency checks.*  
  * *\_ts — The Unix timestamp (in seconds) when the item was last modified.*  
  * *\_attachments — A link to the document’s attachments (if any).*

## Query the database

1. In the **Data Explorer** page, select the **New SQL Query** icon.  
   ![][image11]  
2. In the SQL Query editor, review the default query (SELECT \* FROM c) and use the **Execute Query** button to run it.  
3. Review the results, which includes the full JSON representation of all items.  
4. Modify the query as follows:  
   sql

SELECT \*  
FROM c  
WHERE CONTAINS(c.name,"Helmet")

***Tip**: The NoSQL API uses familiar, SQL-like queries to search JSON documents. SELECT \* FROM c lists all items, and CONTAINS filters by text inside a property—useful for quick searches without extra setup.*

5. Use the **Execute Query** button to run the revised query and review the results, which includes JSON entities for any items with a **name** field containing the text “Helmet”.  
   ![][image12]  
6. Close the SQL Query editor, discarding your changes.  
   You’ve seen how to create and query JSON entities in a Cosmos DB database by using the data explorer interface in the Azure portal. In a real scenario, an application developer would use one of the many programming language specific software development kits (SDKs) to call the NoSQL API and work with data in the database.

## Clean up

When you’ve finished exploring Azure Cosmos DB, you should delete the resources you created so you don’t incur any further costs.

1. In the Azure portal, navigate to the **resource group** that contains your Cosmos DB account.  
2. Select **Delete resource group**, confirm the deletion by entering the resource group name, and select **Delete**.

***Tip:** Deleting the resource group removes the Cosmos DB account and everything inside it in a single step. This is the quickest way to make sure nothing is left running and costing money.*

In this lab, you created an Azure Cosmos DB account, added JSON items, and queried them using a SQL-like language. You’ve taken your first steps with NoSQL data in the cloud\!

![Diapositiva 13](/DP-900/Laboratorios/Lab3-Azure_Data_non-relational/Ej_2_Explore%20Azure%20Cosmos%20DB/imagenes/10Limpieza.png)


