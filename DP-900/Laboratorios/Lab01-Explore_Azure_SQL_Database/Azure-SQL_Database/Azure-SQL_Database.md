# Azure-SQL Database
# DP-900 - Lab 01 - Explorar Azure SQL Database
# Descripción del Ejercicio
El laboratorio describe el ciclo de vida completo del aprovisionamiento, configuración e interacción con una base de datos relacional nativa de la nube mediante **Azure SQL Database**. El documento guía al usuario en la creación de una infraestructura optimizada para el control de costes en entornos de desarrollo, el establecimiento de perímetros de seguridad mediante reglas de firewall de IP, y la manipulación de datos relacionales a través de sentencias estructuradas SQL (DDL, DML y DQL) ejecutadas directamente desde el portal de Azure. El proceso concluye con el desmantelamiento atómico de la infraestructura para garantizar una gobernanza financiera eficiente dentro de la suscripción de Azure.
### 1. Aprovisionamiento de Infraestructura y Optimización de Costes
* **Selección del Recurso:** Se inicializa un recurso de tipo **Azure SQL** enfocado en una **Base de datos única** (*Single database*), idónea para cargas de trabajo de aprendizaje.
* **Aislamiento de carga de trabajo (Workload):** Se configura el entorno como **Development** (Desarrollo) y se asigna un tipo de almacenamiento de respaldo con redundancia local (**Locally-redundant backup storage - LRS**), minimizando el impacto económico del ejercicio.
* **Modelo Serverless:** La base de datos se despliega bajo el nivel *General Purpose - Serverless* de la serie estándar Gen5 (1 vCore, 32 GB de almacenamiento), lo que permite la tarificación basada en el uso por segundo.
* **Organización Lógica:** El servidor y la base de datos se encapsulan dentro de un único **Grupo de Recursos** (*Resource Group*), simplificando la administración y el borrado masivo.

### 2. Seguridad Perimetral y Redes
* **Capa de Conectividad:** Se habilita un **Punto de conexión público** (*Public endpoint*) para permitir que las herramientas de consulta interactúen con el motor.
* **Defensa en el Firewall:** Se configuran dos reglas estrictas en el cortafuegos de Azure SQL Server:
  1. Permitir que los servicios y recursos internos de Azure accedan al servidor.
  2. Registrar la **dirección IP pública del cliente actual** para abrir el acceso exclusivo al administrador de la práctica.
* **Mecanismo de Autenticación:** Se utiliza **SQL Authentication** como la vía más directa para laboratorios, estableciendo credenciales de administrador local (`mgueldba`) y contraseña.

### 3. Operaciones de Datos (Sintaxis Estructurada SQL)
A través de la herramienta nativa **Query editor (preview)**, se realizan interacciones sobre el plano de datos para dar vida a la base de datos `Dealership`:

* **Definición de Esquema (DDL):** Se ejecutan sentencias `CREATE TABLE` para la creación de las tablas de fabricantes (`Manufacturer`) y vehículos (`Vehicle`).
  * *Integridad referencial:* Se implementan restricciones de **PRIMARY KEY** para garantizar la unicidad de filas y de **FOREIGN KEY** para entrelazar ambas tablas a través del campo común `ManufacturerID`.
* **Población de Datos (DML):** Se inyectan filas de muestra mediante comandos `INSERT INTO`, registrando 4 fabricantes y 8 vehículos en el sistema.
* **Explotación de Información (DQL):** Se practican consultas de extracción mediante comandos `SELECT`:
  * Proyección de columnas específicas para mejorar la legibilidad frente al uso generalizado de `SELECT *`.
  * Filtrado condicional de registros a nivel de fila mediante la cláusula **`WHERE`**.
  * Ordenación de la salida de datos de menor a mayor precio con **`ORDER BY`**.
  * Combinación de entidades distribuidas en distintas tablas mediante la instrucción **`INNER JOIN ... ON`**.

### 4. Gobernanza Financiera y Desmantelamiento
* **Eliminación en Cascada:** Al concluir la validación técnica, se ejecuta la eliminación directa del **Grupo de Recursos** original.
* **Prevención de Costes:** Esta acción elimina de forma simultánea e irreversible la base de datos, el servidor lógico y las configuraciones de red asociadas, garantizando que **no queden recursos residuales activos** generando cargos en la suscripción.

# EJECUCIÓN LABORATORIO
# Fase 1: Aprovisionar un recurso de Azure SQL Database
"Aprovisionar" simplemente significa crear y configurar un nuevo recurso. En esta sección, crearás tu servidor de base de datos y una base de datos vacía para alojar tus datos.


1. Inicia sesión en el portal de Azure usando tu cuenta.
2. En la parte superior izquierda de la página, selecciona **+ Crear un recurso** (+ Create a resource). En el cuadro de búsqueda de Marketplace, escribe `Azure SQL` y presiona Enter. En los resultados de búsqueda, selecciona **Azure SQL (published by Microsoft)**.


![Seleccionar Crear SQL Database](imagenes/E1Imagen1.png)

![Seleccionar Crear SQL Database](imagenes/E1Imagen2.png)

3. En la página de Azure SQL, selecciona **Crear** (Create). En el mosaico de crear una base de datos, selecciona **Más detalles** (More details) y luego selecciona **Crear base de datos SQL** (Create SQL Database).

![Seleccionar Crear SQL Database](imagenes/E1Imagen3.png)



   > **Consejo:** Una base de datos SQL única es la opción más sencilla de configurar y es perfecta para aprender.
4. Introduce los siguientes valores en la página "Crear base de datos SQL" y deja las demás propiedades con su configuración predeterminada:
   * **Suscripción (Subscription):** Selecciona tu suscripción de Azure.
   * **Grupo de recursos (Resource group):** Crea un nuevo grupo de recursos con el nombre de tu elección. 
   
   > *Un grupo de recursos es una carpeta que mantiene juntos los recursos relacionados; al terminar, puedes eliminarla para borrar todo con un solo clic.*
   ![Configuración Básica de la Base de Datos](imagenes/E1Imagen4.png)
   
   * **Nombre de la base de datos (Database name):** `Dealership`.
  ![Configuración del Servidor SQL](imagenes/E1Imagen5.png)

   * **Servidor (Server):** Selecciona **Crear nuevo** (Create new) y dale un nombre único en cualquier ubicación disponible. Usa la autenticación de SQL (SQL authentication), especifica tu nombre como administrador del servidor y crea una contraseña compleja. Selecciona **Aceptar** (OK).

![Configuración de Seguridad](imagenes/E1Imagen6.png)

   * **¿Desea usar el grupo elástico de SQL? (Want to use SQL elastic pool?):** No.
   * **Entorno de carga de trabajo (Workload environment):** Desarrollo (Development).
   * **Proceso y almacenamiento (Compute + storage):** Dejar sin cambios.
   * **Redundancia del almacenamiento de copia de seguridad (Backup storage redundancy):** Almacenamiento con redundancia local (Locally-redundant backup storage).


5. Selecciona **Siguiente: Redes >** (Next: Networking >). En conectividad de red, selecciona **Punto de conexión público** (Public endpoint). En las reglas de firewall, establece en **Sí** (Yes) tanto "Permitir que los servicios y recursos de Azure accedan a este servidor" como "Agregar dirección IP del cliente actual".


![Configuración de Red y Firewall](imagenes/E1Imagen7.png)


   > **Consejo:** Estas configuraciones abren el acceso justo para que puedas conectarte a la base de datos durante el laboratorio.


6. Selecciona **Siguiente: Seguridad >** (Next: Security >) y asegúrate de que la opción "Habilitar Microsoft Defender para SQL" esté configurada en **Ahora no** (Not now).

![Revisar y Crear](imagenes/E1Imagen8.png)


7. Selecciona **Siguiente: Configuración adicional >** (Next: Additional settings >). Asegúrate de que la opción "Usar datos existentes" (Use existing data) sea **Ninguno** (None).

   > **Importante:** Dejar esto en "Ninguno" te da una base de datos completamente vacía.
![Despliegue completado](imagenes/E1Imagen9.png)

8. Selecciona **Revisar + crear** (Review + create), revisa la configuración y luego selecciona **Crear** (Create).

![Autenticación SQL en Query Editor](imagenes/E1Imagen10.png)


9. Espera unos minutos a que se complete la implementación y selecciona **Ir al recurso** (Go to resource).

![Panel de Nueva Consulta](imagenes/E1Imagen11.png)
![Panel de Nueva Consulta](imagenes/E1Imagen12.png)
# Fase 2: Crear las tablas de la base de datos y añadir datos de muestra


1. En el menú izquierdo de la base de datos, selecciona **Editor de consultas (versión preliminar)** (Query editor). En la pestaña de **Autenticación de SQL**, introduce el usuario y contraseña del administrador que creaste, y selecciona **Conectar**.


![Panel de Nueva Consulta](imagenes/E1Imagen13.png)


   *(Nota: Si ves un error sobre tu IP, selecciona el enlace en el mensaje para permitir el acceso a tu IP y vuelve a conectarte).*

![Panel de Nueva Consulta](imagenes/E1Imagen14.png)

2. Selecciona **+ Nueva consulta** (+ New query). Pega el siguiente código SQL para crear la tabla de Fabricantes (Manufacturer) y Vehículos (Vehicle).





```sql
CREATE TABLE Manufacturer
(
    ManufacturerID   INT          PRIMARY KEY,
    ManufacturerName NVARCHAR(50) NOT NULL,
    Country          NVARCHAR(50)
);


CREATE TABLE Vehicle
(
    VehicleID      INT            PRIMARY KEY,
    ModelName      NVARCHAR(50)   NOT NULL,
    ManufacturerID INT            NOT NULL,
    ModelYear      INT,
    BodyType       NVARCHAR(30),
    ListPrice      DECIMAL(10, 2),
    FOREIGN KEY (ManufacturerID) REFERENCES Manufacturer(ManufacturerID)
);
```

3. Selecciona ▷ Ejecutar (Run) en la parte superior. Deberías ver un mensaje confirmando que la consulta fue exitosa.
Ejecución de CREATE TABLE completada
![Panel de Nueva Consulta](imagenes/E1Imagen15.png)

Borra el código anterior, pega el siguiente bloque para insertar datos en las tablas, y selecciona ▷ Ejecutar (Run):
```sql
INSERT INTO Manufacturer (ManufacturerID, ManufacturerName, Country) VALUES
(1, 'Toyota',        'Japan'),
(2, 'Ford',          'United States'),
(3, 'Volkswagen',    'Germany'),
(4, 'Hyundai',       'South Korea');


INSERT INTO Vehicle (VehicleID, ModelName, ManufacturerID, ModelYear, BodyType, ListPrice) VALUES
(101, 'Corolla',     1, 2024, 'Sedan',      24500.00),
(102, 'RAV4',        1, 2024, 'SUV',        31200.00),
(103, 'F-150',       2, 2023, 'Pickup',     38900.00),
(104, 'Mustang',     2, 2024, 'Coupe',      42500.00),
(105, 'Golf',        3, 2023, 'Hatchback',  27800.00),
(106, 'Tiguan',      3, 2024, 'SUV',        33400.00),
(107, 'Elantra',     4, 2024, 'Sedan',      22300.00),
(108, 'Tucson',      4, 2023, 'SUV',        29600.00);
```
Ejecución de INSERT INTO completada

![Panel de Nueva Consulta](imagenes/E1Imagen16.png)

# Fase 3: Consultar los datos
Ahora puedes usar comandos SQL SELECT para recuperar y explorar los datos. Prueba ejecutando cada uno de estos bloques:
1. Ver todos los datos de los vehículos:
```sql
SELECT * FROM Vehicle;
```

Resultado de SELECT * FROM Vehicle

![Panel de Nueva Consulta](imagenes/E1Imagen17.png)

2. Ver solo columnas específicas:
```
SELECT ModelName, BodyType, ListPrice
FROM Vehicle;
```
Resultado de SELECT de columnas específicas

![Panel de Nueva Consulta](imagenes/E1Imagen18.png)

3. Filtrar y ordenar datos (vehículos de menos de $30,000):
```sql
SELECT ModelName, BodyType, ListPrice
FROM Vehicle
WHERE ListPrice < 30000
ORDER BY ListPrice;
```
Resultado de SELECT con WHERE y ORDER BY

![Panel de Nueva Consulta](imagenes/E1Imagen19.png)

4. Combinar datos de ambas tablas (JOIN) para ver el modelo junto al país de su fabricante:

```sql
SELECT
    v.ModelName,
    m.ManufacturerName,
    m.Country,
    v.ListPrice
FROM Vehicle AS v
INNER JOIN Manufacturer AS m
    ON v.ManufacturerID = m.ManufacturerID
ORDER BY m.ManufacturerName;
```

Resultado de consulta con INNER JOIN

![Panel de Nueva Consulta](imagenes/E1Imagen20.png)

5.    Tómate un momento para experimentar. Intenta cambiar el precio en la cláusula WHERE o ordenar los resultados por otra columna y, a continuación, vuelve a ejecutar la consulta para ver cómo cambian los resultados.

```sql
SELECT
    v.ModelName,
    m.ManufacturerName,
    m.Country,
    v.ListPrice
FROM Vehicle AS v
INNER JOIN Manufacturer AS m
    ON v.ManufacturerID = m.ManufacturerID
WHERE ListPrice > 30000
ORDER BY m.ManufacturerName;
```
Resultado de consulta

![Panel de Nueva Consulta](imagenes/E1Imagen21.png)

Cuando termines, cierra el panel del editor de consultas y descarta los cambios si se te solicita.

# ## Fase 4: Limpieza de recursos
Para no incurrir en costes adicionales, asegúrate de eliminar los recursos que creaste.
En el portal de Azure, navega hasta el grupo de recursos que creaste al inicio del laboratorio.
Selecciona Eliminar grupo de recursos (Delete resource group).
![Panel de Nueva Consulta](imagenes/E1Imagen22.png)
Eliminar grupo de recursos en el portal de Azure
Confirma escribiendo el nombre del grupo de recursos y selecciona Eliminar (Delete). Esto eliminará la base de datos, el servidor y todo su contenido en un solo paso.
![Panel de Nueva Consulta](imagenes/E1Imagen23.png)
Confirmación de eliminación

![Panel de Nueva Consulta](imagenes/E1Imagen24.png)

> Consejo: Al eliminar el grupo de recursos, se eliminan la base de datos, el servidor y todo lo demás que contenga en un solo paso. Esta es la forma más sencilla de asegurarte de que no quede nada en ejecución que suponga un gasto.
En este ejercicio práctico, has aprovisionado una base de datos SQL de Azure, has creado tus propias tablas, has añadido datos de ejemplo del sector de la automoción y has realizado consultas mediante SQL. ¡Ya has dado tus primeros pasos con los datos relacionales en la nube!

