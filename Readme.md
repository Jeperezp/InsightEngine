# Proyecto de Ingesta y Análisis de Datos en Azure

## Tabla de Contenido

1.  [Introducción](#introduccion)
2.  [Arquitectura y Servicios Usados](#Arquitectura)
    - 2.1.    [Key Vault](#azure-key-vault) 
    - 2.2     [Azure Data Factory](#azure-data-factory)
    - 2.3     [Azure Databricks](#azure-databricks)
    - 2.4     [Sql Database](#azure-sql-database)
    - 2.5     [Azure Data Lake](#azure-datalake)

3.  [Implementacion](#Implementacion)

    3.1 [Azure Key Vault](#azure-key-vault) 
    -   3.1.1  [Creacion del Servicio](#creacion-del-servicio)
    -   3.1.2 [Guardar Secretos](#guardar-secretos)
    -   3.1.3 [Asignacion Permisos](#asignacion-de-permisos)

    3.2 [Azure DataLake](#creacion-azure-datalake-storage)
    -    3.2.1 [Crear un Recurso de Azure Data Lake Storage](#crear-un-recurso-de-azure-data-lake-storage)
    -    3.2.2 [Crear un Contenedor en el Data Lake](#crear-un-contenedor-en-el-data-lake)

    3.3     [Azure Databricks](#azure-databricks)
    -   3.3.1   [Crear el Recurso de Azure Databricks](#crear-el-recurso-de-azure-databricks)
    -   3.3.2   [Creacion del Cluster](#crear-un-cluster)
    -   3.3.3   [Crear un Scope en Databricks para Acceder a los Secretos](#crear-un-scope-en-databricks-para-acceder-a-los-secretos)
    -   3.3.4   [Leer Secretos desde Azure Key Vault en Databricks](#leer-secretos-desde-azure-key-vault-en-databricks)
    -   3.3.5   [Asignación de Permisos en Azure Key Vault](#5-asignación-de-permisos-en-azure-key-vault)
    -   3.3.6   [Montaje DataLake](#montaje-datalake)
    
    3.4 [Azure SQL Database](#azure-sql-database)
    -    3.4.1 [Creacion Servidor](#crear-un-servidor-de-azure-sql)
    -    3.4.2 [Creacion base de Datos](#crear-la-base-de-datos-sql)
    
    3.5 [Azure Data Factory](#azure-data-factory)
    -    3.5.1 [Creacion Recurso](#crear-el-recurso-de-azure-data-factory)
    -    3.5.2 [Ingesta Datos](#crear-una-ingesta-de-datos)
    -    3.5.3 [Creación de Pipeline](#crear-el-pipeline-en-azure-data-factory)
4.  [Recursos](#Recursos)
    4.1  [Data](/DataEngineer/data/)
    4.2  [Notebooks ETL](/DataEngineer/Notebookcs/)
    4.3  [Reporte BI](/DataEngineer/ReporteBI/)


## Introduccion
En este proyecto se aborda el desafío de manejar grandes volúmenes, variedades y velocidades de datos, un reto común para las empresas en la era digital. La problemática principal es que las soluciones tradicionales de análisis de datos no son capaces de soportar el creciente volumen de información y las demandas de tiempo real que exigen los procesos de negocio. Esto genera una limitación en la capacidad de obtener información valiosa a partir de los datos disponibles, lo que afecta la toma de decisiones estratégicas.

Para resolver este problema, hemos diseñado e implementado una arquitectura moderna basada en Azure que permite almacenar, procesar y analizar datos de manera eficiente y escalable. El proyecto utiliza servicios en la nube de Azure, como Azure Data Factory (ADF) para la orquestación de flujos de datos, Azure Databricks para el procesamiento y análisis, Azure Key Vault para el manejo seguro de secretos, Azure Data Lake para almacenamiento masivo y económico, y Azure SQL Database para almacenar los resultados estructurados.

La solución no solo es escalable, sino que también permite obtener insights valiosos de manera rápida y precisa, lo que permite a la empresa tomar decisiones informadas en un entorno dinámico. Este enfoque optimizado y flexible proporciona una base sólida para la toma de decisiones estratégicas en tiempo real, ayudando a la empresa a mantenerse competitiva y ágil frente a los cambios del mercado.

El flujo de trabajo de datos se organiza a través de pipelines en ADF, que orquestan la ingesta, transformación y almacenamiento de datos. Los notebooks de Databricks realizan las transformaciones necesarias y los datos procesados se visualizan en reportes de BI para facilitar la interpretación y análisis por parte de los usuarios finales.

## Arquitectura y Servicios Usados

![Arquitectura](./image/Arquitectura.svg)

### Servicios Azure Usados

- ### **Azure Data Factory** 
Es un servicio en la nube que permite la orquestación de flujos de trabajo de datos. Se utiliza para mover, transformar y cargar datos desde diversas fuentes hacia un destino, todo en un entorno escalable y gestionado.
- ### **Azure Databricks** 
Es una plataforma de análisis basada en Apache Spark, que permite realizar grandes análisis de datos y aprendizaje automático (machine learning). Facilita la colaboración entre equipos de datos, desarrolladores y analistas en un entorno optimizado para procesar grandes volúmenes de datos.
- ### **Azure Key vault** 
Servicio que ayuda a proteger claves, contraseñas y otros secretos. Permite almacenar y acceder de manera segura a información sensible que es utilizada por las aplicaciones y servicios en la nube.
- ### **Azure DataLake** 
Es un almacenamiento escalable y de alto rendimiento para grandes volúmenes de datos. Permite almacenar datos sin procesar (raw data) de manera económica, ideal para análisis y procesamiento de grandes cantidades de datos.
- ### **Azure SQL Database** 
Es una base de datos relacional en la nube que ofrece escalabilidad, alta disponibilidad y seguridad. Está basada en SQL Server y se utiliza para almacenar y gestionar datos estructurados en aplicaciones empresariales.

## Implementacion


####  ![Key_Vault](icons/10245-icon-service-Key-Vaults.svg)Azure Key Vault

#### Creacion del Servicio

-   Inicia sesión en el portal de Azure

-   En el portal de Azure, haz clic en el ícono de la lupa o busca "Key Vault" en el cuadro de búsqueda.

-   Selecciona Key Vault en los resultados de búsqueda.

-   Haz clic en Crear.
-   Selecciona la suscripción en la que deseas crear el Key Vault.
-   Selecciona el Grupo de recursos que previamente has creado
-   Asigna un nombre único para tu Key Vault
-   Elige la región en la que deseas crear el Key Vault
-   haz clic en Crear.

### Guardar Secretos

-   Acceder a tu Key Vault:
-   Ve a la sección Recursos del portal de Azure y selecciona el Key Vault que acabas de crear.
-   Agregar un secreto:
-   Dentro del Key Vault, ve a la opción Secrets (Secretos) en el menú lateral izquierdo.
-   Haz clic en + Generar/Importar en la parte superior.
-   Completa la siguiente información:
    -   **Nombre:** Asigna un nombre a tu secreto (por  ejemplo, "MiSecreto").
    -   **Valor:** Introduce el valor de tu secreto (por ejemplo, una contraseña o cadena de conexión).
-   Configurar opciones del secreto (opcional):
-   Puedes establecer una fecha de expiración para el secreto si lo deseas.
-   También puedes habilitar o deshabilitar la versión del secreto.
-   Guardar el secreto:
-   Haz clic en Crear para almacenar el secreto en el Key Vault.

### Asignacion de Permisos

-   Asignar permisos a una identidad:
-   Si estás utilizando una aplicación o una identidad administrada para acceder al Key Vault, debes configurar las políticas de acceso.
-   En el portal de Azure, ve a la sección Políticas de acceso de tu Key Vault y haz clic en + Agregar acceso.
-   Asigna los permisos necesarios (como leer secretos, gestionar claves, etc.) a la identidad de la aplicación. para el caso que nos ocupa Databricks

### ![Datalake](./icons/10150-icon-service-Data-Lake-Store-Gen1.svg) Azure Datalake Storage

#### Crear un Recurso de Azure Data Lake Storage

1. **Iniciar sesión en el Portal de Azure**:
   - Ve a [Azure Portal](https://portal.azure.com/) e inicia sesión con tu cuenta de Microsoft.

2. **Crear una Cuenta de Almacenamiento**:
   - En el portal de Azure, busca **"Cuentas de almacenamiento"** en la barra de búsqueda y selecciona **"Cuentas de almacenamiento"**.
   - Haz clic en **+ Crear** para crear una nueva cuenta de almacenamiento.

3. **Configurar la Cuenta de Almacenamiento**:
   - **Suscripción**: Selecciona la suscripción donde deseas crear el recurso.
   - **Grupo de Recursos**: Elige un grupo de recursos existente o crea uno nuevo.
   - **Nombre de la cuenta de almacenamiento**: Asigna un nombre único a la cuenta de almacenamiento. Este nombre debe ser globalmente único.
   - **Región**: Elige la región donde deseas que se cree el Data Lake.
   - **Tipo de rendimiento**: Selecciona **"Estándar"** para la mayoría de los casos.
   - **Tipo de cuenta**: Elige **"Data Lake Storage Gen2"**.
   - **Redundancia**: Elige el tipo de redundancia que más te convenga, como **LRS (Locally Redundant Storage)**.

4. **Revisar y Crear**:
   - Revisa la configuración y haz clic en **Crear**.
   - El proceso de creación tomará unos minutos y luego podrás acceder a tu cuenta de almacenamiento.

#### Crear un Contenedor en el Data Lake

1. **Acceder a la Cuenta de Almacenamiento**:
   - Una vez que la cuenta de almacenamiento esté creada, ve a **"Cuentas de almacenamiento"** en el portal de Azure y selecciona la cuenta que creaste.

2. **Crear un Contenedor**:
   - En el menú de la cuenta de almacenamiento, selecciona **"Contenedores"** bajo la sección **"Datos"**.
   - Haz clic en **+ Contenedor** para crear un nuevo contenedor.

3. **Configurar el Contenedor**:
   - **Nombre del contenedor**: Asigna un nombre único a tu contenedor. Los nombres de los contenedores deben ser en minúsculas y pueden contener solo letras, números y guiones.
   - **Nivel de acceso**: Elige el nivel de acceso que deseas. Las opciones son:
     - **Privado (solo para cuentas con permisos)**: Solo los usuarios con acceso explícito pueden leer o escribir.
     - **Blob (lectura pública)**: Los archivos dentro del contenedor pueden ser leídos por cualquier persona, pero no se pueden escribir ni modificar sin autenticación.
     - **Contenedor (lectura pública y listado de blobs)**: Cualquier persona puede listar y leer los archivos dentro del contenedor.

4. **Crear el Contenedor**:
   - Haz clic en **Crear** y se creará tu contenedor en el Data Lake.


### ![Azure_Databricks](./icons/10787-icon-service-Azure-Databricks.svg) Azure Databricks


#### Crear el Recurso de Azure Databricks

1. **Iniciar sesión en el Portal de Azure**:
   - Ve a [Azure Portal](https://portal.azure.com/) e inicia sesión con tu cuenta de Microsoft.

2. **Crear el Recurso de Azure Databricks**:
   - En el portal de Azure, haz clic en la **barra de búsqueda** y busca "Azure Databricks".
   - Selecciona **Azure Databricks** y haz clic en **Crear**.
   
3. **Configurar los Detalles**:
   - **Suscripción**: Selecciona la suscripción donde deseas crear el recurso.
   - **Grupo de Recursos**: Elige un grupo de recursos existente o crea uno nuevo.
   - **Nombre**: Asigna un nombre único al servicio de Databricks.
   - **Ubicación**: Elige la región donde deseas desplegar el servicio.
   
4. **Revisar y Crear**:
   - Revisa la configuración y haz clic en **Crear**.
   - El recurso se creará en unos minutos y podrás acceder a él desde el portal de Azure.

#### Crear un Cluster

1. **Acceder a Azure Databricks**:
   - Una vez creado el recurso, ve al **Recurso de Azure Databricks** y haz clic en **Ir al servicio**.

2. **Crear un Cluster**:
   - En el panel de Databricks, selecciona la opción **Clusters** en el menú lateral izquierdo.
   - Haz clic en **Create Cluster** (Crear Cluster).

3. **Configurar el Cluster**:
   - **Nombre**: Asigna un nombre al cluster.
   - **Tipo de Cluster**: Elige el tipo de cluster (por ejemplo, **Standard**).
   - **Versión de Spark**: Selecciona la versión de Apache Spark que deseas utilizar.
   - **Tamaño de Cluster**: Configura el tamaño de tu cluster seleccionando el número de nodos y el tipo de nodo.
   
4. **Crear el Cluster**:
   - Haz clic en **Create** para crear el cluster.
   - Una vez creado, el cluster aparecerá en la lista y podrás iniciar su uso.

#### Crear un Scope en Databricks para Acceder a los Secretos

1. **Acceder a la Interfaz de Databricks CLI**:
   - Asegúrate de tener configurado el Databricks CLI en tu máquina o accede a la interfaz de comandos desde la plataforma Databricks.
   - Si no tienes configurado el CLI, sigue la documentación oficial de [Databricks CLI](https://docs.databricks.com/dev-tools/cli/index.html).

2. **Crear el Scope**:
   - En Databricks, puedes crear un **Scope** que sea utilizado para acceder a los secretos almacenados en Azure Key Vault.
   - Ejecuta el siguiente comando para crear un scope que se conecte al Key Vault:

   ```bash
   databricks secrets create-scope --scope <nombre_del_scope> --scope-backend-type AZURE_KEYVAULT --resource-id <ID_del_recurso_de_Key_Vault> --azure-keyvault-resource-id <ID_del_recurso_de_Key_Vault>
   ```

   - **`<nombre_del_scope>`**: Nombre que deseas asignar al scope en Databricks.
   - **`<ID_del_recurso_de_Key_Vault>`**: El **ID del recurso** de Key Vault. Puedes obtenerlo desde el portal de Azure en la sección de **Información general** de tu Key Vault.
   - **`<azure-keyvault-resource-id>`**: ID del recurso de Azure Key Vault (también disponible en el portal de Azure).

3. **Verificar el Scope**:
   - Una vez creado el scope, puedes verificar que se ha creado correctamente con el siguiente comando:

   ```bash
   databricks secrets list-scopes
   ```

#### Leer Secretos desde Azure Key Vault en Databricks

1. **Acceder a un Secreto desde Databricks**:
   - Puedes acceder a los secretos almacenados en el Azure Key Vault desde un notebook de Databricks.

2. **Leer el Secreto en un Notebook**:
   - Abre un nuevo notebook en Databricks y usa el siguiente código para leer el secreto:

   ```python
   secret_value = dbutils.secrets.get(scope="<nombre_del_scope>", key="<nombre_del_secreto>")
   print(secret_value)
   ```

   - **`<nombre_del_scope>`**: El nombre del scope que creaste previamente.
   - **`<nombre_del_secreto>`**: El nombre del secreto que deseas leer desde el Key Vault.

3. **Ejemplo**:
   - Si tienes un secreto llamado "DB_PASSWORD" en tu Key Vault, el código se vería así:

   ```python
   db_password = dbutils.secrets.get(scope="my_scope", key="DB_PASSWORD")
   print(db_password)
   ```

4. **Manejo Seguro**:
   - Ten en cuenta que nunca debes imprimir ni almacenar los secretos de manera insegura. Usa los secretos de manera segura dentro de tu aplicación o procesamiento de datos.

#### Asignación de Permisos en Azure Key Vault

1. **Configurar Políticas de Acceso para Databricks**:
   - En el portal de Azure, navega a tu **Key Vault**.
   - En el menú **Configuración**, selecciona **Políticas de acceso**.
   - Haz clic en **+ Agregar nueva política** y configura los permisos para Databricks.

2. **Asignar Permisos de Lectura a Databricks**:
   - Asegúrate de asignar permisos de **Lectura** en **Secretos** a la identidad de Azure Databricks (ya sea identidad administrada o asignada manualmente).
   - Guarda los cambios.

### Montaje DataLake

1.  **Obtener la URL de tu Data Lake**:
   - En el portal de Azure, navega hasta tu cuenta de almacenamiento.
   - En la sección **"Puntos de acceso"** de la cuenta de almacenamiento, encontrarás la URL de tu Data Lake. Esta URL tiene un formato similar a:  
     `https://<nombre_de_cuenta>.dfs.core.windows.net/`
2. **Obtener la clave de acceso**:
   - Ve a **"Cuentas de almacenamiento"** y selecciona tu cuenta de almacenamiento.
   - En la sección de **"Claves de acceso"** o **"Identidad administrada"** (si utilizas la autenticación administrada), podrás encontrar la **clave de acceso** necesaria para conectar Databricks con tu Data Lake.

#### Configurar Databricks para Acceder al Data Lake


1. **Crear un secreto en Databricks para la clave de almacenamiento**:
   - Si no has configurado **Azure Key Vault** en tu cuenta de Databricks, puedes almacenar la clave de acceso directamente como un secreto.
   - Desde el menú lateral de Databricks, ve a **"Databricks Utilities"** (dbutils) y selecciona **"Secret"**.
   - Guarda tu clave de acceso de almacenamiento en el **Databricks Secret Manager**.

2. **Montar el Data Lake con dbutils en un Notebook**:
   - Una vez tengas tu clave de acceso configurada, puedes montar el Data Lake en Databricks utilizando el siguiente código en un notebook:

```python
   # Definir la URL de tu Data Lake
   storage_account_name = "<nombre_de_cuenta>"
   storage_account_key = dbutils.secrets.get(scope="<nombre_del_scope>", key="<nombre_de_secreto>")
   container_name = "<nombre_del_contenedor>"
   
   # Configuración de Spark para la conexión con el Data Lake
   spark.conf.set(f"spark.hadoop.fs.azure.account.key.{storage_account_name}.dfs.core.windows.net", storage_account_key)
   
   # Montar el Data Lake en Databricks
   dbutils.fs.mount(
     source = f"abfss://{container_name}@{storage_account_name}.dfs.core.windows.net/",
     mount_point = "/mnt/<nombre_del_punto_de_montaje>",
     extra_configs = {"<conf clave>" : "<valor>"}
   )
```

  - **Explicación**:
     - **`<nombre_de_cuenta>`**: Es el nombre de tu cuenta de almacenamiento de Azure.
     - **`<nombre_del_scope>`**: El scope del Databricks Secret Manager donde almacenaste la clave de acceso.
     - **`<nombre_del_secreto>`**: El nombre del secreto que contiene la clave de acceso.
     - **`<nombre_del_contenedor>`**: El nombre de tu contenedor en el Data Lake.
     - **`/mnt/<nombre_del_punto_de_montaje>`**: La ruta donde se montará el Data Lake en Databricks (por ejemplo, `/mnt/mydatalake`).

#### Verificar que el Data Lake se ha montado correctamente

Para verificar que el Data Lake se ha montado correctamente, puedes listar el contenido del punto de montaje:

```python
# Listar el contenido del punto de montaje
display(dbutils.fs.ls("/mnt/<nombre_del_punto_de_montaje>"))
```
### Acceder a los Archivos del Data Lake

Una vez montado el Data Lake, puedes acceder a sus archivos utilizando el sistema de archivos de Databricks (`dbutils.fs`). Por ejemplo:

```python
# Leer un archivo CSV desde el Data Lake
df = spark.read.csv("/mnt/<nombre_del_punto_de_montaje>/archivo.csv")
df.show()
```
### ![SQL](./icons/10130-icon-service-SQL-Database.svg) Azure SQL Database 

### Crear un Servidor de Azure SQL

1. **Iniciar sesión en el Portal de Azure**:
   - Ve al [Portal de Azure](https://portal.azure.com/) e inicia sesión con tu cuenta de Microsoft.

2. **Crear un servidor de SQL**:
   - En la barra de búsqueda del portal de Azure, busca **"SQL servers"** y selecciona **"Servidores SQL"**.
   - Haz clic en **+ Crear** para crear un nuevo servidor.

3. **Configurar el servidor de SQL**:
   - **Suscripción**: Selecciona la suscripción donde deseas crear el recurso.
   - **Grupo de recursos**: Elige un grupo de recursos existente o crea uno nuevo.
   - **Nombre del servidor**: Asigna un nombre único al servidor. Este nombre debe ser globalmente único, ya que se usará en la URL del servidor (por ejemplo, `mi-servidor.database.windows.net`).
   - **Región**: Elige la región donde deseas crear el servidor SQL.
   - **Autenticación**: 
     - **Usuario del administrador del servidor**: Ingresa un nombre de usuario que se usará para conectarte al servidor SQL.
     - **Contraseña**: Ingresa una contraseña segura para el administrador del servidor SQL.
   - **Revisar y Crear**:
     - Revisa la configuración y haz clic en **Crear**. Este proceso tomará unos minutos.

### Crear la Base de Datos SQL

1. **Acceder al servidor de SQL**:
   - Una vez creado el servidor, ve a la sección **"Bases de datos"** dentro de la página del servidor de SQL en el portal de Azure.

2. **Crear una nueva base de datos**:
   - Haz clic en **+ Agregar** para crear una nueva base de datos.

3. **Configurar la base de datos**:
   - **Nombre de la base de datos**: Asigna un nombre a la base de datos (por ejemplo, `mi_base_datos`).
   - **Selección del servidor**: Elige el servidor SQL que creaste anteriormente. Si no aparece, puedes buscarlo y seleccionarlo.
   - **Grupo de recursos**: Elige el grupo de recursos en el que se creará la base de datos (puede ser el mismo grupo de recursos del servidor).
   - **Ajustes de rendimiento**:
     - **Nivel de servicio**: Elige el nivel de servicio (por ejemplo, **"Standard"** o **"Premium"**) según tus necesidades de rendimiento.
     - **Cantidad de DTUs**: Elige la cantidad de DTUs según el rendimiento que necesites (puedes elegir entre opciones como **1, 5, 10, 20, 50, 100, 200**).
     - **Opción de almacenamiento**: Si lo deseas, puedes elegir opciones avanzadas como almacenamiento escalable o cambios en el tamaño de la base de datos.
   - **Revisar y Crear**:
     - Revisa la configuración y haz clic en **Crear**.

#### Configurar el Firewall y el Acceso Remoto

1. **Configurar reglas de firewall**:
   - Una vez que el servidor SQL y la base de datos estén creados, debes configurar las reglas de firewall para permitir el acceso a tu servidor SQL desde las direcciones IP que necesiten conectarse.
   - Ve a **"Firewall y redes virtuales"** dentro de la configuración de tu servidor SQL.
   - Agrega las direcciones IP que deseas permitir, como la dirección IP pública de tu computadora.

2. **Habilitar acceso remoto a través de Azure**:
   - Si deseas acceder a la base de datos SQL desde aplicaciones o clientes externos, asegúrate de habilitar la opción de **"Acceso público"** en la configuración de **Firewall y redes virtuales**.

#### Conectar a la Base de Datos SQL desde SQL Server Management Studio (SSMS)

1. **Obtener el nombre del servidor y la base de datos**:
   - En el portal de Azure, dentro de tu servidor SQL, obtén la **URL del servidor**. Será algo como `mi-servidor.database.windows.net`.
   - El nombre de la base de datos es el que asignaste al crear la base de datos (por ejemplo, `mi_base_datos`).

2. **Conectarse usando SSMS**:
   - Abre **SQL Server Management Studio (SSMS)**.
   - En el campo **Servidor**, ingresa la **URL del servidor** (por ejemplo, `mi-servidor.database.windows.net`).
   - En **Autenticación**, selecciona **Autenticación SQL Server**.
   - Ingresa el **nombre de usuario** y la **contraseña** del administrador del servidor SQL.
   - Haz clic en **Conectar**.

3. **Verificar la conexión**:
   - Una vez conectado, deberías poder ver la base de datos que creaste en el panel de navegación de SSMS.

### ![ADF](./icons/10126-icon-service-Data-Factories.svg) Azure Data Factory

#### Crear el Recurso de Azure Data Factory

1. **Iniciar sesión en el Portal de Azure**:
   - Ve al [Portal de Azure](https://portal.azure.com/) e inicia sesión con tu cuenta de Microsoft.

2. **Crear un recurso de Azure Data Factory**:
   - En la barra de búsqueda del portal de Azure, busca **"Data Factory"** y selecciona **"Azure Data Factory"**.
   - Haz clic en **+ Crear** para crear un nuevo recurso de Data Factory.

3. **Configurar el recurso de Data Factory**:
   - **Suscripción**: Selecciona la suscripción donde deseas crear el recurso.
   - **Grupo de recursos**: Elige un grupo de recursos existente o crea uno nuevo.
   - **Nombre**: Asigna un nombre único para tu instancia de Data Factory.
   - **Región**: Selecciona la región en la que deseas crear tu Data Factory.
   - **Versión**: Selecciona la versión **V2** (la más reciente).
   - **Revisar y Crear**: Revisa la configuración y haz clic en **Crear**.

4. **Acceder a Data Factory**:
   - Una vez creado, ve al recurso de Data Factory desde el portal de Azure y accede al **Azure Data Factory Studio** para comenzar a trabajar en tus flujos de datos.

### Crear un Linked Service en Azure Data Factory para Conectar con Databricks

1. **Acceder a la sección de "Manage"**:
   - Dentro de Azure Data Factory Studio, ve a la sección de **"Manage"** (ícono de engranaje en la barra lateral).

2. **Crear un Linked Service para Databricks**:
   - En la sección **"Linked services"**, haz clic en **+ New**.
   - Selecciona **Azure Databricks** como el tipo de linked service.
   - Configura los siguientes campos:
     - **Nombre**: Asigna un nombre para el linked service (por ejemplo, `DatabricksLinkedService`).
     - **Cluster URL**: Ingresa la URL de tu clúster de Databricks.
     - **Token de Autenticación**: Usa un **Personal Access Token** (PAT) de tu cuenta de Databricks para autenticarte.
     - Haz clic en **Test connection** para verificar la conexión.

### Crear una Ingesta de Datos

1. **Crear un Dataset de Entrada (origen de datos)**:
   - En el **Azure Data Factory Studio**, ve a la sección **"Author"** (ícono de lápiz en la barra lateral).
   - Dentro de la sección de **Datasets**, haz clic en **+ New Dataset**.
   - Selecciona el tipo de fuente de datos desde donde deseas hacer la ingesta (por ejemplo, **Azure Blob Storage** si los datos están almacenados en blobs).
   - Configura la conexión a tu fuente de datos y especifica la ruta del archivo que deseas ingerir.

2. **Crear un Dataset de Salida (destino de datos)**:
   - De manera similar, crea un nuevo dataset para el destino de los datos, como una base de datos SQL, un Data Lake, o incluso otro almacenamiento en **Azure**.

### Crear el Pipeline en Azure Data Factory

1. **Crear un nuevo Pipeline**:
   - En la sección **"Author"**, selecciona **+ New Pipeline**.
   - Asigna un nombre al pipeline, por ejemplo, `DatabricksPipeline`.

2. **Agregar una actividad de Databricks**:
   - Dentro del pipeline, haz clic en **+** para agregar una actividad.
   - Selecciona **"Databricks"** y luego **"Notebooks"**.

3. **Configurar la actividad de Databricks**:
   - **Nombre**: Asigna un nombre a la actividad (por ejemplo, `RunDatabricksNotebook`).
   - **Linked Service**: Selecciona el **Linked Service** de Databricks que creaste anteriormente.
   - **Path del Notebook**: Selecciona el notebook de Databricks que quieres ejecutar desde el pipeline.
   - Si es necesario, pasa parámetros al notebook de Databricks que pueden ser utilizados dentro del código.

4. **Guardar y publicar el pipeline**:
   - Haz clic en **"Publish All"** para guardar y publicar el pipeline.

### Crear y Configurar un Notebook de Databricks

1. **Acceder a tu Workspace de Databricks**:
   - Ve a [Azure Databricks](https://databricks.azure.com/) e inicia sesión con tu cuenta.
   - En el **Workspace**, crea un nuevo notebook que se ejecutará en el pipeline de Data Factory.

2. **Escribir el código en el notebook**:
   - El código de tu notebook puede realizar las operaciones de procesamiento necesarias sobre los datos ingeridos. Por ejemplo, si estás procesando un archivo CSV, el código podría verse así:

```python
   # Leer datos desde un CSV en Databricks
   df = spark.read.csv("/mnt/<nombre_del_punto_de_montaje>/datos.csv", header=True, inferSchema=True)
   
   # Realizar alguna transformación
   df_transformed = df.filter(df['column_name'] > 10)
   
   # Guardar los datos transformados
   df_transformed.write.parquet("/mnt/<nombre_del_punto_de_montaje>/datos_transformados")
```

3. **Guardar el notebook**:
   - Una vez que hayas escrito el código, guarda el notebook en tu workspace de Databricks.

### Ejecutar el Pipeline

1. **Ejecutar el pipeline manualmente**:
   - Ve a la sección de **"Monitor"** en Azure Data Factory.
   - Selecciona el pipeline que creaste (`DatabricksPipeline`) y haz clic en **"Trigger"** para ejecutar el pipeline manualmente.

2. **Verificar la ejecución**:
   - En la misma sección de **Monitor**, puedes ver el estado de ejecución del pipeline.
   - Si todo está configurado correctamente, el pipeline ejecutará el notebook de Databricks que procesará los datos.

