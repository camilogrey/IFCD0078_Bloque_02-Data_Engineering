# **Explore Azure Storage**

**📌 1\. Objetivo del Laboratorio**  
El objetivo principal de este ejercicio es aprender a aprovisionar, configurar y explorar los diferentes servicios de almacenamiento no relacional dentro de una cuenta de Azure Storage. El laboratorio busca que experimentes de forma directa cómo cambia el comportamiento de la gestión de archivos al pasar de un almacenamiento plano (Flat Namespace) a uno jerárquico (Hierarchical Namespace / Data Lake Gen2) , además de comprender el uso de recursos compartidos de archivos.

**2\. Resumen del Ejercicio**  
El documento guía al usuario a través de un escenario dividido en 4 fases principales:

1. 🛠️ **Creación de la Infraestructura:** Se aprovisiona una cuenta de almacenamiento estándar dentro de un grupo de recursos controlado (dp900-lab-rg).  
2. 📁 **Exploración de Blob Storage (Estructura Plana):** Se crea un contenedor de datos privado y se experimenta con la subida de un archivo JSON. Aquí se demuestra que, en el Blob Storage tradicional, las carpetas son virtuales (no existen objetos de directorio reales, sino solo prefijos en el nombre del archivo).  
3. 🚀  **Actualización a Data Lake Storage Gen2:** Se realiza una actualización a nivel de cuenta para activar el "Espacio de nombres jerárquico" (Hierarchical Namespace). Al hacerlo, las carpetas se convierten en directorios reales, habilitando capacidades avanzadas de analítica de Big Data, renombrado seguro y asignación de permisos de estilo Linux (ACL).  
4.  🖧 y 🗑️ **Uso de Azure Files y Limpieza:** Se crea un recurso compartido de archivos en la nube (Classic file share) y se observa cómo obtener los scripts de conexión nativos para diferentes sistemas operativos. Al final, se elimina todo el grupo de recursos para evitar costes imprevistos.

**3\. Puntos Clave a Tener en Cuenta (¡Muy Importante para tu ejecución\!)**

⚠️ **Punto crítico en el Paso 5 de la creación:** El documento resalta que **debes desactivar obligatoriamente todas las opciones de *Soft Delete* (eliminación suave)** en la pestaña de *Data protection*. Si las dejas activadas por defecto, Azure **no te dejará** hacer la actualización a Data Lake Gen2 más adelante.

* **Estructura Plana vs. Jerárquica:** \* *Sin actualizar (Blob):* Si creas una carpeta vacía y no tiene archivos dentro, la carpeta desaparece porque es solo un prefijo de texto. No puedes renombrar carpetas ni darles permisos individuales.  
  * *Actualizado (Data Lake Gen2):* Las carpetas son directorios reales integrados. Puedes aplicar seguridad a nivel de carpeta y optimizar búsquedas masivas.  
* **El Nombre de la Cuenta:** Debe ser único a nivel mundial, por lo que el nombre que viene en la captura (dp900store43913) no te servirá. Inventa uno combinando letras minúsculas y números (ej: tunombrestorage99).  
* **Redundancia Económica:** El ejercicio utiliza **LRS (Locally-redundant storage)** , que mantiene tres copias idénticas en el mismo centro de datos. Es la opción ideal y más barata para laboratorios.

**Borrado Final:** Al terminar, borrar el grupo de recursos dp900-lab-rg eliminará absolutamente todo lo que creaste de un solo golpe, asegurándote de que no se consuma el crédito de tu suscripción. 

**🧠 Conclusiones Técnicas del Ejercicio**
* **Blob Storage** es óptimo y masivamente escalable para almacenar objetos binarios sueltos e independientes (imágenes, logs, vídeos) gracias a su indexación de nombres planos.
* **Data Lake Storage Gen2** es indispensable para entornos de analítica y Big Data (como clusters de Spark, Databricks o Synapse), ya que el espacio de nombres jerárquico permite realizar búsquedas y operaciones de renombrado masivo en milisegundos sin necesidad de reescribir los metadatos de miles de archivos individuales.

## 🛠️ **Realización Laboratorio**

En este laboratorio, creará una cuenta de **Azure Storage account**, que es un lugar seguro en la nube para guardar diferentes tipos de datos. Luego explorará sus cuatro servicios principales y verá para qué sirve cada uno:

* **Blob storage**, para almacenar archivos como imágenes, documentos y archivos de datos.  
* **Data Lake Storage Gen2**, un **blob storage** con carpetas reales, utilizado para análisis de Big Data.  
* **Azure Files**, recursos compartidos de archivos en la nube que se comportan como una unidad de red compartida.

Este tipo de almacenamiento se denomina no relacional porque, a diferencia de una base de datos relacional, los datos no tienen que organizarse en tablas relacionadas con una estructura fija. No se preocupe si estos términos son nuevos, cada paso se explica a medida que avanza.

Este laboratorio tardará aproximadamente 30 minutos en completarse.

💡 **Consejo:** A lo largo del laboratorio verá notas breves que explican *por qué* está realizando cada paso. Comprender la razón detrás de cada acción le ayudará más adelante a diseñar soluciones de almacenamiento que equilibren los objetivos de coste, rendimiento, seguridad y análisis.

## **Antes de empezar**

Necesitará una suscripción de [Azure subscription](https://azure.microsoft.com/free) en la que tenga acceso a nivel de administrador. Si no tiene una, puede registrarse para obtener una cuenta gratuita utilizando el enlace anterior.  
 

**¿Qué es Azure?** Azure es la plataforma en la nube de Microsoft. En lugar de comprar y ejecutar su propio ordenador servidor, alquila recursos informáticos (como el almacenamiento) a Microsoft y los utiliza a través de Internet. El **Azure portal** es el sitio web que utiliza para crear y gestionar esos recursos.

## **Aprovisionar una Azure Storage account**

“Provisioning” just means creating and setting up a new resource. The first step in using Azure Storage is to create a storage account, which acts as a container for everything you store.

***What is a storage account?** It’s the top-level “home” for all your Azure Storage services (blobs, files, queues, and tables). Settings like cost, how many copies of your data are kept, encryption, and who can access it are all controlled from here.*  
"Aprovisionar" solo significa crear y configurar un nuevo recurso. El primer paso para utilizar **Azure Storage** es crear una cuenta de almacenamiento, que actúa como un contenedor para todo lo que almacena.

**¿Qué es una cuenta de almacenamiento?** Es el "hogar" de nivel superior para todos sus servicios de **Azure Storage** (**blobs**, **files**, **queues** y **tables**). Ajustes como el coste, cuántas copias de sus datos se conservan, el cifrado y quién puede acceder a ellos se controlan desde aquí.

1. Si aún no lo ha hecho, inicie sesión en el **Azure portal**.  
2. En la página de inicio del **Azure portal**, seleccione **＋ Create a resource** en la esquina superior izquierda y busque **Storage account**. Luego, en la página resultante de **Storage account**, seleccione **Create**.  
![imagen1](/DP-900/Laboratorios/Lab3-Azure_Data_non-relational/Ej_1_Azure_Storage/imagenes/00CreateAccount.png)

![imagen2](/DP-900/Laboratorios/Lab3-Azure_Data_non-relational/Ej_1_Azure_Storage/imagenes/01CreateAccount.png)

3. Introduzca los siguientes valores en la pestaña **Basics** de la página **Create a storage account**:

   ○      **Subscription:** Seleccione su suscripción de Azure.

   ○      **Resource group:** Seleccione **Create new** e introduzca un nombre de su elección, como dp900-lab-rg.

**¿Qué es un grupo de recursos?** Es simplemente una carpeta que mantiene unidos los recursos de Azure relacionados. Cuando haya terminado, puede eliminar la carpeta para quitar todo en un solo clic.

○      **Storage account name:** Introduzca un nombre único para su cuenta de almacenamiento utilizando únicamente letras minúsculas y números (este nombre no debe estar ya en uso por nadie más).

○      **Region:** Seleccione cualquier ubicación disponible cerca de usted.

○      **Performance:** Standard

○      **Redundancy:** Locally-redundant storage (LRS)

*Nota:* Dependiendo de la región que elija, es posible que también vea una opción de **Preferred storage type** configurada en *Azure Blob Storage o Azure Data Lake Storage Gen 2*. Puede dejarla en el valor por defecto.

![imagen3](/DP-900/Laboratorios/Lab3-Azure_Data_non-relational/Ej_1_Azure_Storage/imagenes/02CreateAccount.png)

💡 **Consejo:** Un nuevo grupo de recursos facilita la limpieza. **Standard \+ LRS** es la base de menor coste, ideal para el aprendizaje. **LRS** mantiene tres copias síncronas en una región, lo cual es adecuado para datos de demostración no críticos sin pagar por la georréplica.

4. Seleccione **Next: Advanced \>** y visualice las opciones de configuración avanzada. En particular, observe que aquí es donde puede habilitar el espacio de nombres jerárquico (**hierarchical namespace**) para admitir **Data Lake Storage Gen2**.Deje la opción **Enable hierarchical namespace** desmarcada (la habilitará más tarde), y luego seleccione **Next: Networking \>** para ver las opciones de red de su cuenta de almacenamiento.

![image4](/DP-900/Laboratorios/Lab3-Azure_Data_non-relational/Ej_1_Azure_Storage/imagenes/03CreateAccount.png)

4. Seleccione **Next: Data protection \>** y luego, en la sección **Recovery**, desactive todas las opciones de **Enable soft delete…**.

Estas opciones retienen los archivos eliminados para su posterior recuperación, pero pueden causar problemas más adelante cuando habilite el espacio de nombres jerárquico.

![image5](/DP-900/Laboratorios/Lab3-Azure_Data_non-relational/Ej_1_Azure_Storage/imagenes/04CreateAccount.png)

 

5. Continúe a través de las páginas restantes de **Next \>** sin cambiar ninguno de los ajustes por defecto. Luego, en la página **Review**, espere a que se validen sus selecciones y seleccione **Create** para crear su **Azure Storage account**.![Screenshot of the Review tab summarizing the storage account settings with the Create button.][image5]  
6. Espere a que se complete el despliegue. A continuación, seleccione **Go to resource** para abrir la cuenta de almacenamiento que se ha desplegado.  
    ![Screenshot of the deployment complete page with the Go to resource button.][image6]

## **Explore blob storage**

Ahora que tiene una **Azure Storage account**, puede crear un contenedor para los datos de **blob**.

💡 **Consejo:** Un contenedor agrupa **blobs** y es el primer nivel de alcance para el control de acceso. Comenzar con un **blob storage** plano (sin espacio de nombres jerárquico) muestra el comportamiento de las carpetas virtuales que comparará con **Data Lake Gen2** más adelante.

1. Descargue el archivo JSON product1.json desde https://aka.ms/product1.json y guárdelo en su ordenador (puede guardarlo en cualquier carpeta; lo subirá al **blob storage** más tarde). Si el archivo JSON se muestra en su navegador, haga clic derecho en la página y seleccione **Guardar como**. Asigne al archivo el nombre product1.json y almacénelo en su carpeta de descargas.  
2. En la página del **Azure portal** de su contenedor de almacenamiento, en el lado izquierdo, en la sección **Data storage**, seleccione **Containers**.  
3. En la página de **Containers**, seleccione **＋ Add container**. En el panel de **New container**, introduzca el nombre data. Tenga en cuenta que el **Anonymous access level** se configura automáticamente en **Private (no anonymous access)** y no se puede cambiar, porque el acceso anónimo está deshabilitado por defecto en la cuenta de almacenamiento. Seleccione **Create**.

![Screenshot of the New container pane with the name data entered and the anonymous access level set to Private.][image7]  
💡 **Consejo:** El acceso **Private** mantiene seguros sus datos de muestra. El acceso público rara vez se necesita, excepto para sitios web estáticos o escenarios de datos abiertos. Nombrarlo data mantiene este ejemplo simple y legible.

4. Cuando se haya creado el contenedor data, verifique que aparece en la lista de la página **Containers**.  
    ![Screenshot of the Containers page showing the data container with Private access level.][image8]  
5. En el panel del lado izquierdo, en la sección superior, seleccione **Storage browser**. Esta página proporciona una interfaz basada en el navegador que puede utilizar para trabajar con los datos de su cuenta de almacenamiento.  
6. En la página del **storage browser**, seleccione **Blob containers** y verifique que su contenedor data aparece en la lista.  
7. Seleccione el contenedor data y observe que está vacío.  
    ![Screenshot of the storage browser showing the empty data container with the Add Directory and Upload toolbar buttons.][image9]  
8. Seleccione **＋ Add Directory** y lea la información sobre las carpetas antes de crear un nuevo directorio llamado products.  
9. En el **storage browser**, verifique que la vista actual muestra el contenido de la carpeta products que acaba de crear; observe que las "rutas de navegación" (*breadcrumbs*) en la parte superior de la página reflejan la ruta Blob containers \> data \> products.  
    ![Screenshot of the storage browser inside the products folder with the breadcrumb path Blob containers \> data \> products.][image10]  
10. En las rutas de navegación, seleccione data para cambiar al contenedor data, y note que no contiene una carpeta llamada products. Las carpetas en el **blob storage** son virtuales y solo existen como parte de la ruta de un **blob**. ¡Dado que la carpeta products no contenía **blobs**, en realidad no está ahí\!

💡 **Consejo:** Un espacio de nombres plano (*flat namespace*) significa que los directorios son solo prefijos de nombre (products/file.json). Este diseño permite una escala masiva porque el servicio indexa los nombres de los **blobs** en lugar de mantener una estructura de árbol real.

11. Utilice el botón **⤒ Upload** para abrir el panel **Upload blob**. En el panel **Upload blob**, seleccione el archivo product1.json que guardó previamente en su ordenador local. Luego, en la sección **Advanced**, en el cuadro **Upload to folder**, introduzca product\_data y seleccione el botón **Upload**.

![Screenshot of the Upload blob panel with product1.json selected and product\_data entered in the Upload to folder box.][image11]  
12\. 💡 **Consejo:** Proporcionar un nombre de carpeta durante la carga crea automáticamente la ruta virtual, lo que ilustra que la presencia de un **blob** hace que aparezca la "carpeta".

13. Cierre el panel **Upload blob** si aún está abierto, y verifique que se ha creado una carpeta virtual product\_data en el contenedor data.

![Screenshot of the data container showing the product\_data virtual folder.][image12]

14. Seleccione la carpeta product\_data y verifique que contiene el **blob** product1.json que subió.  
15. En el lado izquierdo, en la sección **Data storage**, seleccione **Containers**.  
16. Abra el contenedor data y verifique que la carpeta product\_data que creó aparece en la lista.  
17. Seleccione el icono de los tres puntos (**‧‧‧**) en el extremo derecho de la carpeta y note que el menú no muestra ninguna opción. Las carpetas en un contenedor de **blob** con espacio de nombres plano son virtuales y no se pueden gestionar.

💡 **Consejo:** No existe un objeto de directorio real, por lo que no hay operaciones de cambio de nombre o permisos; estas requieren un espacio de nombres jerárquico (**hierarchical namespace**).

18. Utilice el icono **X** en la parte superior derecha de la página data para cerrar la página y volver a la página de **Containers**.

## **Explore Azure Data Lake Storage Gen2**

El soporte de **Data Lake Store Gen2** le permite utilizar carpetas jerárquicas para organizar y gestionar el acceso a los **blobs**. También le permite utilizar el **Azure blob storage** para alojar sistemas de archivos distribuidos para plataformas comunes de análisis de Big Data.

💡 **Consejo:** Activar el espacio de nombres jerárquico (**hierarchical namespace**) hace que las carpetas se comporten como directorios reales. También le permite realizar acciones de carpeta de forma segura (todas a la vez, sin errores) y le ofrece controles de permisos de archivos similares a los de Linux. Esto es especialmente útil cuando se trabaja con herramientas de Big Data como Spark o Hadoop, o cuando se gestionan lagos de datos grandes y organizados.

1. Descargue el archivo JSON product2.json desde https://aka.ms/product2.json y guárdelo en su ordenador en la misma carpeta donde descargó product1.json anteriormente; lo subirá al **blob storage** más tarde.  
2. En la página del **Azure portal** de su cuenta de almacenamiento, en el lado izquierdo, desplácese hacia abajo hasta la sección **Settings** y seleccione **Data Lake Gen2 upgrade**.  
3. En la página **Data Lake Gen2 upgrade**, expanda y complete cada paso para actualizar su cuenta de almacenamiento para habilitar el espacio de nombres jerárquico y admitir **Azure Data Lake Storage Gen**. Esto puede tardar algún tiempo.

![Screenshot of the Data Lake Gen2 upgrade page showing the three upgrade steps: review account changes, validate account, and upgrade account.][image13]  
💡 **Consejo:** La actualización es un cambio de capacidad a nivel de cuenta; los datos permanecen, pero la semántica del directorio cambia para admitir operaciones avanzadas.

4. Cuando se complete la actualización, en el panel del lado izquierdo, en la sección superior, seleccione **Storage browser** y navegue de regreso a la raíz de su contenedor de **blob** data, que aún contiene la carpeta product\_data.  
5. Seleccione la carpeta product\_data y verifique que aún contiene el archivo product1.json que subió anteriormente.  
6. Utilice el botón **⤒ Upload** para abrir el panel **Upload blob**.  
7. En el panel **Upload blob**, seleccione el archivo product2.json que guardó en su ordenador local. A continuación, seleccione el botón **Upload**.  
8. Cierre el panel **Upload blob** si aún está abierto, y verifique que la carpeta product\_data ahora contiene el archivo product2.json.  
    ![Screenshot of the product\_data folder containing both product1.json and product2.json.][image14]

💡 **Consejo:** Añadir un segundo archivo después de la actualización confirma la continuidad sin interrupciones: los **blobs** existentes siguen funcionando y los nuevos obtienen beneficios jerárquicos como las ACL (Listas de Control de Acceso) de directorio.

9. En el lado izquierdo, en la sección **Data storage**, seleccione **Containers**.  
10. Abra el contenedor data y verifique que la carpeta product\_data que creó aparece en la lista.  
11. Seleccione el icono de los tres puntos (**‧‧‧**) en el extremo derecho de la carpeta y note que, con el espacio de nombres jerárquico habilitado, puede realizar tareas de configuración a nivel de carpeta, incluyendo cambiar el nombre de las carpetas y establecer permisos (**Manage ACL**).  
     ![][image15]

💡 **Consejo:** Las carpetas reales le permiten aplicar seguridad de mínimo privilegio con granularidad de carpeta, renombrar de forma segura y acelerar los listados recursivos en comparación con el escaneo de miles de nombres de **blobs** con prefijo.

12. Utilice el icono **X** en la parte superior derecha de la página data para cerrar la página y volver a la página de **Containers**.

## **Explore Azure Files**

**Azure Files** proporciona una forma de crear recursos compartidos de archivos basados en la nube.

💡 **Consejo:** **Azure Files** ofrece endpoints SMB/NFS para escenarios de migración directa (*lift-and-shift*) donde las aplicaciones esperan un sistema de archivos tradicional. Complementa (no reemplaza) al **blob storage** al admitir bloqueos de archivos y herramientas nativas del sistema operativo.

*Nota:* Debido a que habilitó el espacio de nombres jerárquico (**Data Lake Storage Gen2**) anteriormente, los recursos compartidos de archivos para esta cuenta se gestionan en **Classic file shares**. En una cuenta de almacenamiento sin espacio de nombres jerárquico, este elemento del menú se llama simplemente **File shares**, pero los pasos para crear y conectarse a un recurso compartido son los mismos.

1. En la página del **Azure portal** de su cuenta de almacenamiento, en el lado izquierdo, inicie sesión en la sección **Data storage**, seleccione **Classic file shares**.  
2. En la página de **Classic file shares**, seleccione **＋ Classic file share**. En la pestaña **Basics**, introduzca el nombre files y deje el **Access tier** configurado en **Transaction optimized**.  
    ![][image16]  
3. Seleccione **Next: Backup \>** y desmarque la casilla de verificación **Enable backup** para deshabilitar la copia de seguridad. Luego seleccione **Review \+ create**, y en la pestaña **Review \+ create**, seleccione **Create**.  
    ![][image17]

💡 **Consejo:** Deshabilitar la copia de seguridad mantiene bajos los costes para un entorno de laboratorio de corta duración; la habilitaría para la resiliencia en producción.

4. Cuando se haya creado el recurso compartido files, vuelva a la página de **Classic file shares** y abra su nuevo recurso compartido files.  
5. En la parte superior de la página, seleccione **Connect**. Luego, en el panel **Connect**, note que hay pestañas para los sistemas operativos comunes (Windows, Linux y macOS) que contienen scripts que puede ejecutar para conectarse a la carpeta compartida desde un ordenador cliente.

![Screenshot of the Connect pane for the files share with Windows, Linux, and macOS tabs showing the connection script.][image18]  
💡 **Consejo:** Los scripts generados muestran exactamente cómo montar el recurso compartido utilizando comandos nativos de la plataforma, ilustrando patrones de acceso híbridos desde máquinas virtuales, contenedores o servidores locales (*on-prem*).

6. Cierre el panel **Connect** y luego cierre la página de files para volver a la página de **Classic file shares** de su cuenta de almacenamiento de Azure.

## **Limpieza**

Cuando haya terminado de explorar **Azure Storage**, debe eliminar los recursos que creó para no incurrir en más costes.

1. En el **Azure portal**, navegue al grupo de recursos que creó al inicio del laboratorio (por ejemplo, dp900-lab-rg).  
2. Seleccione **Delete resource group**, confirme la eliminación introduciendo el nombre del grupo de recursos y seleccione **Delete**.![][image19]

💡 **Consejo:** Eliminar el grupo de recursos quita la cuenta de almacenamiento y todo lo que hay dentro en un solo paso. Esta es la forma más rápida de asegurarse de que no quede nada en ejecución y costando dinero.

3\.     En este laboratorio, creó una **Azure Storage account** y exploró **blob storage**, **Data Lake Storage Gen2** y **Azure Files**. ¡Ahora ha visto las principales formas en que Azure almacena datos no relacionales\!
