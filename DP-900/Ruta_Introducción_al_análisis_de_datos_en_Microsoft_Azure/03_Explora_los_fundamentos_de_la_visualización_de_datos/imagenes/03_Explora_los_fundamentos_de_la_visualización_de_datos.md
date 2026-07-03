# Explora los fundamentos de la visualización de datos

## Índice

- [Introducción](#introducción)
- [Describa las herramientas y el flujo de trabajo de Power BI](#describa-las-herramientas-y-el-flujo-de-trabajo-de-power-bi)
- [Describa los conceptos básicos del modelado de datos](#describa-los-conceptos-básicos-del-modelado-de-datos)
- [Describa las consideraciones para la visualización de datos](#describas-las-consideraciones-para-la-visualización-de-datos)
- [Ejercicio: Explora los fundamentos de la visualización de datos con Power BI](#ejercicio-explora-los-fundamentos-de-la-visualización-de-datos-con-power-bi)
- [Verificación de conocimientos](#verificación-de-conocimientos)
- [Resumen](#resumen)

---

## Introducción

El modelado y la visualización de datos son fundamentales para las cargas de trabajo de inteligencia empresarial (BI) que utilizan soluciones de análisis de datos a gran escala. La visualización de datos facilita la elaboración de informes y la toma de decisiones en las organizaciones.

### Objetivos de aprendizaje
En este módulo aprenderás a:
- Describa un proceso general para crear soluciones de informes con Microsoft Power BI.
- Describa los principios básicos del modelado de datos analíticos.
- Identificar los tipos comunes de visualización de datos y sus usos.
- Crea un informe interactivo con Power BI Desktop.

> [!NOTE]
> Reconocemos que cada persona aprende de manera diferente. Puedes completar este módulo en formato de video o leer el contenido en formato de texto e imágenes. El texto contiene más detalles que los videos, por lo que en algunos casos te resultará útil como material complementario a la presentación en video.

---

## Describa las herramientas y el flujo de trabajo de Power BI

Existen numerosas herramientas de visualización de datos que los analistas pueden utilizar para explorar datos y resumir información visualmente; entre ellas, la compatibilidad con gráficos en herramientas de productividad como Microsoft Excel y los widgets de visualización de datos integrados en los blocs de notas utilizados para explorar datos en servicios como Microsoft Fabric y Azure Databricks. 

Sin embargo, para el análisis empresarial a gran escala, suele ser necesaria una solución integrada que admita el modelado de datos complejo, la generación de informes interactivos y el intercambio seguro de información.

### Microsoft Power BI
Microsoft Power BI es un conjunto de herramientas y servicios que constituye la carga de trabajo principal de Microsoft Fabric, que los analistas de datos pueden utilizar para crear visualizaciones de datos interactivas para que las consuman los usuarios empresariales.

![Flujo de trabajo de Power BI](imagenes/01power-bi-flow.png)

Un flujo de trabajo típico para crear una solución de visualización de datos comienza con **Power BI Desktop**, una aplicación de Microsoft Windows en la que se pueden importar datos de una amplia gama de fuentes de datos, combinar y organizar los datos de estas fuentes en un modelo de datos analítico y crear informes que contengan visualizaciones interactivas de los datos.

Una vez creados los modelos de datos y los informes, puede publicarlos en el **servicio Power BI**, un servicio en la nube donde los usuarios empresariales pueden publicar e interactuar con los informes. También puede realizar modelado de datos básico y editar informes directamente en el servicio mediante un navegador web, aunque esta funcionalidad es limitada en comparación con la herramienta Power BI Desktop. 

Puede usar el servicio para programar actualizaciones de los orígenes de datos en los que se basan sus informes y para compartirlos con otros usuarios. Además, puede definir paneles y aplicaciones que combinen informes relacionados en una única ubicación de fácil acceso.

Los usuarios pueden acceder a informes, paneles y aplicaciones del servicio Power BI a través de un navegador web o en dispositivos móviles mediante la **aplicación móvil de Power BI**.

### Power BI en Microsoft Fabric
Power BI está totalmente integrado en **Microsoft Fabric**, la plataforma de análisis unificada de Microsoft. Dentro de Fabric, el contenido de Power BI reside en **espacios de trabajo** compartidos junto con otros elementos de ingeniería de datos y análisis, todos respaldados por la misma capa de almacenamiento OneLake. 

Las principales funcionalidades específicas de Fabric include:
- **Espacios de trabajo:** Entornos compartidos donde los equipos colaboran en informes y modelos semánticos en un navegador, sin necesidad de software de escritorio.
- **Modelos semánticos:** Un modelo semántico define las medidas, las relaciones y las jerarquías sobre las que se construyen los informes, y puede compartirse entre varios informes.
- **Modo Direct Lake:** Un modo de almacenamiento que permite a Power BI consultar datos directamente desde archivos OneLake, combinando la velocidad del análisis en memoria con la escala de un lago de datos, sin necesidad de una importación de datos o un ciclo de actualización independiente.
- **Edición de informes basada en la web:** Los analistas pueden crear y actualizar informes completamente en el navegador, lo que hace que Power BI sea accesible sin necesidad de instalar Power BI Desktop.

### Asistencia de IA en Power BI
Power BI incluye funciones de IA integradas que ayudan a los analistas a trabajar de forma más eficiente.

#### Copilot en Power BI
Las funcionalidades de Copilot requieren capacidad de Fabric (F2 o superior) o Power BI Premium (P1 o superior) y están disponibles tanto en Power BI Desktop como en el servicio Power BI:
- **Resumir un informe:** Copilot genera un resumen en lenguaje sencillo de lo que muestra un informe.
- **Crear páginas de informe:** Copilot crea páginas de informe y selecciona los tipos de gráficos adecuados en función de sus datos y las indicaciones recibidas.
- **Generar medidas DAX:** Copilot escribe expresiones de medidas DAX a partir de una descripción en lenguaje natural.

#### Otras características de IA
Las siguientes funciones con tecnología de IA están disponibles sin una licencia de Copilot:
- **Visualización narrativa inteligente:** Genera automáticamente un resumen de texto que describe lo que muestran los datos de un informe y se actualiza dinámicamente a medida que cambian los datos.

Estas características hacen que la IA se convierta en una parte natural del flujo de trabajo diario del analista dentro de Power BI y Microsoft Fabric.

---

## Describa los conceptos básicos del modelado de datos

Los modelos analíticos —también llamados **modelos semánticos** en Microsoft Fabric y Power BI— estructuran los datos para facilitar el análisis. Un modelo se crea a partir de tablas de datos relacionadas. Define los valores numéricos que se desean analizar o sobre los que se desea generar informes, conocidos como **medidas**, y las entidades que se utilizan para agregarlos, conocidas como **dimensiones**.

Por ejemplo, un modelo podría incluir medidas numéricas para las ventas (como ingresos o cantidad) y dimensiones para productos, clientes y tiempo. Esto permite agregar medidas en una o más dimensiones; por ejemplo, para identificar los ingresos totales por cliente o el total de artículos vendidos por producto al mes.

### Tablas y esquemas
- **Tablas de dimensiones:** Representan las entidades que desea agrupar o filtrar, por ejemplo, productos o clientes. Cada fila tiene un valor clave único, y las columnas restantes almacenan atributos como nombres de productos, categorías o ciudades de clientes. La mayoría de los modelos analíticos incluyen una dimensión de tiempo para que pueda agregar medidas en diferentes períodos de tiempo.
- **Tablas de hechos:** Almacenan las medidas numéricas que desea analizar. Cada fila representa un evento registrado; por ejemplo, una transacción de venta con valores para la cantidad vendida y los ingresos.

![Esquema de estrella](imagenes/02star-schema.png)

Cuando una tabla de hechos se relaciona con una o más tablas de dimensiones, el diseño se denomina **esquema de estrella**. Si las tablas de dimensiones se relacionan además con tablas de detalles adicionales (por ejemplo, una tabla de categorías vinculada a una tabla de productos), el diseño se denomina **esquema de copo de nieve**.

Al cargar datos en un modelo semántico, Power BI los almacena en un eficiente almacén columnar en memoria mediante el motor VertiPaq. Las agregaciones se calculan en el momento de la consulta, lo que permite un análisis y una generación de informes rápidos.

### Jerarquías de atributos
Las jerarquías permiten explorar en profundidad los valores agregados en diferentes niveles de una dimensión. Por ejemplo:
- En la tabla de productos, una jerarquía podría agrupar los productos por categorías.
- En la tabla de clientes, una jerarquía podría agrupar a los clientes por ciudad.
- En el tiempo, una jerarquía podría agrupar los días en meses y los meses en años.

 Cuando visualiza las ventas totales por año y luego profundiza para ver un desglose mensual, el motor de VertiPaq calcula valores agregados en cada nivel en el momento de la consulta.

![Jerarquía](imagenes/03hierarchy.png)

### Modelado analítico en Microsoft Power BI
En Power BI, se define un modelo semántico a partir de tablas importadas de una o más fuentes de datos. Utilice la **vista Modelo** en Power BI Desktop para crear relaciones entre tablas de hechos y dimensiones, definir jerarquías, establecer tipos de datos y formatos de visualización, y configurar otras propiedades que dan forma al modelo para su análisis.

![Modelo de Power BI](imagenes/04power-bi-model.png)

Si sus datos se almacenan en OneLake (el lago de datos compartido de Microsoft Fabric), utilice el modo de almacenamiento **Direct Lake** para conectar su modelo semántico directamente a los archivos del lago. Esto le proporciona un rendimiento de consulta en memoria sin necesidad de importar los datos por separado.

---

## Describa las consideraciones para la visualización de datos

Una vez creado el modelo, puede utilizarlo para generar visualizaciones de datos que se pueden incluir en un informe.

Existen diversos tipos de visualización de datos, algunos de uso común y otros más especializados. Power BI incluye un amplio conjunto de visualizaciones integradas, que se pueden ampliar con visualizaciones personalizadas y de terceros. El resto de esta unidad analiza algunas visualizaciones de datos comunes, pero no se trata de una lista exhaustiva.

### Tablas y texto
![Tablas y texto](imagenes/05text-table.png)

Las tablas y el texto suelen ser la forma más sencilla de comunicar datos. Las tablas son útiles cuando se deben mostrar numerosos valores relacionados, y los valores de texto individuales en tarjetas pueden ser una forma práctica de mostrar cifras o métricas importantes.

### Gráficos de barras y columnas
![Gráficos de barras y columnas](imagenes/06bar-column-chart.png)

Los gráficos de barras y columnas son una buena manera de comparar visualmente valores numéricos para categorías discretas.

### Gráficos de líneas
![Gráficos de líneas](imagenes/07line-chart.png)

Los gráficos de líneas también se pueden utilizar para comparar valores categorizados y son útiles cuando se necesita examinar tendencias, a menudo a lo largo del tiempo.

### Gráficos circulares
![Gráficos circulares](imagenes/08pie-chart.png)

Los gráficos circulares se utilizan con frecuencia en los informes empresariales para comparar visualmente valores categorizados como proporciones de un total.

### Diagramas de dispersión
![Diagramas de dispersión](imagenes/09scatter-plot.png)

Los diagramas de dispersión son útiles cuando se desea comparar dos medidas numéricas e identificar una relación o correlación entre them.

### Mapas
![Mapas](imagenes/10map.png)

Los mapas son una excelente manera de comparar visualmente valores para diferentes áreas o ubicaciones geográficas.

### Informes interactivos en Power BI
![Informe de Power BI](imagenes/11power-bi-report.png)

En Power BI, los elementos visuales de datos relacionados en un informe se vinculan automáticamente entre sí y ofrecen interactividad. Por ejemplo, al seleccionar una categoría en una visualización, esta se filtrará y resaltará automáticamente en otras visualizaciones relacionadas del informe. 

En la imagen superior, se ha seleccionado la ciudad de Seattle en el gráfico de columnas «Ventas por ciudad y categoría», y las demás visualizaciones se han filtrado para mostrar únicamente los valores de Seattle.

### Visualizaciones basadas en IA en Power BI
Power BI incluye varias funciones de visualización basadas en inteligencia artificial que ayudan a los analistas a explorar y explicar los datos de forma más eficiente:
- **Narrativas inteligentes:** Generan automáticamente resúmenes escritos que describen lo que muestra una visualización, y se actualizan dinámicamente a medida que cambian los datos.
- **Visualización de preguntas y respuestas:** Permite que los usuarios de informes hagan preguntas en lenguaje sencillo y reciban respuestas instantáneas en forma de gráfico basadas en el modelo semántico.
- **Factores clave:** Identifica qué factores influyen con mayor fuerza en una métrica seleccionada y muestra los resultados como una representación visual interactiva.
- **Árbol de descomposición:** Permite explorar de forma interactiva múltiples dimensiones para identificar qué factores contribuyen a un valor.

Estas funciones son las que permiten a Power BI integrar la IA directamente en el flujo de trabajo diario del analista.

---

## Ejercicio: Explora los fundamentos de la visualización de datos con Power BI

Ahora tienes la oportunidad de explorar el modelado y la visualización de datos con Microsoft Power BI.

> [!NOTE]
> Para completar este ejercicio, necesitarás un ordenador con Microsoft Windows instalado.

Inicie el ejercicio y siga las instrucciones.

![Launch Exercise](imagenes/launch_exercise.png)

---

## Verificación de conocimientos

Elige la mejor respuesta para cada una de las siguientes preguntas.

### 1. ¿Qué herramienta debería utilizar para importar datos de múltiples fuentes y crear un informe?
- [x] Power BI Desktop
- [ ] Aplicación móvil de Power BI
- [ ] Azure Data Factory

### 2. ¿Qué elementos debería definir en su modelo de datos para permitir el análisis de desglose ascendente/descendente?
- [ ] Una medida
- [x] Una jerarquía
- [ ] Una relación

### 3. ¿Qué tipo de visualización debería utilizar para analizar las tasas de aprobación de varios exámenes a lo largo del tiempo?
- [ ] Un gráfico circular
- [ ] Un diagrama de dispersión
- [x] Un gráfico de líneas

---

## Resumen

El modelado y la visualización de datos permiten a las organizaciones extraer información valiosa de los datos.

En este módulo, aprendiste a:
- Describa un proceso de alto nivel para crear soluciones de informes con Microsoft Power BI.
- Describa los principios básicos del modelado de datos analíticos.
- Identificar los tipos comunes de visualización de datos y sus usos.
- Crea un informe interactivo con Power BI Desktop.

---

⬅️ **Anterior:** [02. Explora los fundamentos del análisis en tiempo real](../02_Analisis_en_tiempo_real/02_Explora_los_fundamentos_del_análisis_en_tiempo_real.md)

🏠 **Inicio del módulo:** [README](../README.md)