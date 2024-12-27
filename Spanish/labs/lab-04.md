# Microsoft Fabric Real-Time Intelligence in a Day Laboratorio 4

# Contenido

- Estructura del documento	
- Introducción	
- Marco Medallion dentro de las bases de datos KQL	
    - Tarea 1: Crear tablas Bronce	
    - Tarea 2: Cargar tablas Bronce mediante una canalización de datos	
    - Tarea 3: Transformar tablas en Silver Layer	
    - Tarea 4: Crear una Gold Layer con vistas materializadas	
- Disponibilidad de OneLake y almacén de lago de Fabric	
    - Tarea 5: Crear un almacén de lago	18
    - Tarea 6: Acceso directo a tablas de base de datos KQL	
- Resumen	
- Referencias	

 
# Estructura del documento

El laboratorio incluye pasos que el usuario debe seguir junto con capturas de pantalla asociadas que sirven de ayuda visual. En cada captura de pantalla, las secciones se resaltan con cuadros de color naranja para indicar en qué áreas debe centrarse el usuario.

# Introducción

En este laboratorio, creará un marco de trabajo Medallion mediante el enfoque de Bronze Layer, Plata y Oro para controlar datos en sus diferentes etapas de desarrollo y su uso en análisis.

A continuación, conectará los datos de su base de datos KQL a un almacén de lago para demostrar la rapidez con la que puede compartir sus datos en tiempo real con aquellos de su organización que deseen usarlos para informes de Power BI.

Al final de este laboratorio, habrá aprendido:

- Crear tablas de base de datos KQL con el lenguaje de consulta Kusto.

- Cargar datos en bases de datos KQL con Data Factory Pipelines.

- Crear vistas materializadas en bases de datos KQL.

- Crear un almacén de lago y usar accesos directos a la base de datos KQL.


# Marco Medallion dentro de las bases de datos KQL

## Tarea 1: Crear tablas Bronce

1. Abra el **área de trabajo de Fabric** para el curso y el conjunto de consultas KQL que creó en el último laboratorio, **Create Tables**.

2. Dentro de este conjunto de consultas KQL, cambiemos el nombre de la pestaña original de “eh_Fabrikam” a “Crear tablas externas”, lo que facilitará la organización y comprensión de lo que tenemos en este conjunto de consultas.
 
3. Ahora vamos a seleccionar el icono “+” para crear una nueva pestaña y llamaremos a la nueva pestaña “Bronze Layer”.

4. En esta nueva pestaña, pegue y resalte el siguiente código y seleccione “Ejecutar” para crear cuatro nuevas tablas que servirán de Bronze Layer del marco de trabajo Medallion.

```
//BRONZE LAYER
.execute database script <|

.create table [Address] (AddressID:int,AddressLine1:string,AddressLine2:string,City: string, StateProvince:string, CountryRegion:string, PostalCode: string, rowguid: guid, ModifiedDate:datetime)
.create table [Customer](CustomerID:int, NameStyle: string, Title: string, FirstName: string, MiddleName: string, LastName: string,Suffix:string, CompanyName: string, SalesPerson: string, EmailAddress: string, Phone: string, ModifiedDate: datetime)
.create table [SalesOrderHeader](SalesOrderID: int, OrderDate: datetime, DueDate: datetime, ShipDate: datetime, ShipToAddressID: int, BillToAddressID: int, SubTotal: decimal, TaxAmt: decimal, Freight: decimal, TotalDue: decimal, ModifiedDate: datetime)
.create table [SalesOrderDetail](SalesOrderID: int, SalesOrderDetailID: int, OrderQty: int, ProductID: int, UnitPrice: decimal , UnitPriceDiscount: decimal,LineTotal: decimal, ModifiedDate: datetime)
```
 
5. Una vez que se ejecute, debería ver inmediatamente cuatro tablas nuevas creadas en el Explorador de objetos de la base de datos.

    - Address

    - Customer

    - SalesOrderDetail

    - SalesOrderHeader


6. Para expandir la **tabla Address**, haga clic en el icono “>” junto al nombre.
 
7.	Se muestra el esquema (nombres de columna y tipos de datos) de la tabla. Una cosa que sería útil agregar a esta tabla en la base de datos KQL sería una columna oculta para el tiempo de ingesta que se utilizará más adelante en la arquitectura Medallion. Vamos a agregarla ahora. Copie y pegue el
siguiente script para modificar las tablas que acaba de crear agregando una columna de tiempo de ingesta.

```
//adds a hidden field showing ingestion time
.execute database script <|
.alter table Address policy ingestiontime true
.alter table Customer policy ingestiontime true
.alter table SalesOrderHeader policy ingestiontime true
.alter table SalesOrderDetail policy ingestiontime true
```
8. Las cuatro tablas nuevas son tablas en blanco con su esquema definido. Ahora es necesaria una forma de cargar correctamente estas tablas. Vuelva a su área de trabajo **RTI_username**.

## Tarea 2: Cargar tablas Bronce mediante una canalización de datos

1. En el área de trabajo, seleccione la opción “**+ Nuevo elemento**” para abrir el panel de selección. A continuación, busque y seleccione la opción llamada **Data pipeline**.
 
2. Asigne a la nueva canalización el nombre **Load KQL Database Bronze Layer**.

3. Haga clic en **Crear**.

4. Cuando aparezca el menú de canalización, haga clic en la opción **Asistente para copiar datos**.

5. Para empezar, deberá crear una conexión a la base de datos de origen de dónde desea extraer los datos. Haga clic en la opción **Base de datos de Azure SQL** en “Nuevos orígenes”. Si no lo ve de inmediato, puede usar la barra de búsqueda en la parte superior para filtrar los orígenes. Nos conectaremos a la misma base de datos externa de Azure SQL del laboratorio anterior,
pero a diferentes tablas.

6. Deberá introducir los datos de conexión de la base de datos. Siga usando la información de su entorno o como se indica a continuación.

    - fabrikamdemo.database.windows.net
    - fabrikamdb
    - demouser
    - fabrikam@123456
 
7. Haga clic en **Siguiente** cuando se haya completado todo.

8. Desde la lista de tablas disponibles, seleccione lo siguiente:

    - SalesLT.Address
    - SalesLT.Customer
    - SalesLT.SalesOrderDetail
    - SalesLT.SalesOrderHeader

9. Haga clic en **Siguiente**.

10.	Ahora deberá configurar su destino para determinar a dónde desea que la canalización envíe los datos. Busque el **Centro de datos de OneLake** y seleccione su base de datos KQL **eh_Fabrikam**.

11.	Si se le solicita que inicie sesión, basta con utilizar las credenciales que encontrará en la página de detalles del entorno.

12.	Haga clic en la tabla **SalesLT.Address** si aún no está seleccionada y luego haga clic en el menú desplegable junto a la opción **Tabla**. Haga clic en la opción de la tabla **Address**.

13. Ahora verá información general de las **Asignaciones de columnas**. Esto le permitirá visualizar todos los campos procedentes de la base de datos de origen que está enviando a su base de datos KQL. Tiene la opción de eliminar campos específicos si no desea que se asignen desde el origen.

14. Siga los mismos pasos que en el paso 11-12 para las tablas **SalesLT.Customer, SaleLT.SalesOrderDetail y SalesLT.SalesOrderHeader**. No será necesario realizar asignaciones de columnas, así que simplemente haga coincidir los nombres de las tablas. Una vez que todas las tablas se hayan asignado adecuadamente, haga clic en Siguiente.

15. La última página en la que se usa el asistente de copia de datos para copiar datos es una página de información general para comprobar todas las configuraciones que ha seleccionado. Asegúrese de que el número de tablas de origen y el número de tablas de destino sean los mismos.

16.	Haga clic en **Guardar y ejecutar**.

17.	Después de unos minutos, aparecerá una ventana flotante que incluye un **Parámetro**.
El asistente de copia que acabamos de completar creó una lista de las tablas para recorrer en iteración y cargar en las tablas KQL. Simplemente haga clic en el botón **Aceptar** para ejecutar la canalización tal como está configurada actualmente desde el Asistente de copia de datos.

18.	Deje que se ejecute la canalización y, después de aproximadamente un minuto, el movimiento de datos debería estar completo. Habrá transferido los datos una vez que vea que todas las actividades dentro de la canalización se hayan **completado correctamente**.
 
19.	Vamos a revisar una de nuestras tablas y verificar los datos. Vuelva al conjunto de consultas KQL que hemos estado usando, denominado **Create Tables**, asegúrese de que está en la pestaña
**Bronze Layer** y ejecute el siguiente script.

```
//Query the Bronze layer Customer table 

Customer
| take 100
```

20.	Debería ver algunos datos como la imagen siguiente, pero es posible que no sean exactos


# Tarea 3: Transformar tablas en Silver Layer

1.	Ahora que las tablas Bronce están cargadas, crearemos una nueva pestaña dentro de, conjunto de consultas KQL que se llamará “Silver Layer”.

2. Ejecute lo siguiente Script KQL dentro de la pestaña “Silver Layer” para crear cuatro tablas nuevas que servirán de Silver Layer del marco de trabajo Medallion.

```
//SILVER LAYER

.execute database script <|

.create table [SilverAddress] (AddressID:int,AddressLine1:string,AddressLine2:string,City: string, StateProvince:string, CountryRegion:string, PostalCode: string, rowguid: guid, ModifiedDate:datetime, IngestionDate: datetime)

.create table [SilverCustomer](CustomerID:int, NameStyle: string, Title: string, FirstName: string, MiddleName: string, LastName: string,Suffix:string, CompanyName: string, SalesPerson: string, EmailAddress: string, Phone: string, ModifiedDate: datetime, IngestionDate: datetime)

.create table [SilverSalesOrderHeader](SalesOrderID: int, OrderDate: datetime, DueDate: datetime, ShipDate: datetime, ShipToAddressID: int, BillToAddressID: int, SubTotal: decimal, TaxAmt: decimal, Freight: decimal, TotalDue: decimal, ModifiedDate: datetime, DaysShipped: long, IngestionDate: datetime)

.create table [SilverSalesOrderDetail](SalesOrderID: int, SalesOrderDetailID: int, OrderQty: int, ProductID: int, UnitPrice: decimal, UnitPriceDiscount: decimal,LineTotal: decimal, ModifiedDate: datetime, IngestionDate: datetime)
```

3. Para ejecutar ese script, resalte el nuevo script y haga clic en **Ejecutar**.

4. Una vez que se ejecute ese script, verá cuatro nuevas tablas agregadas al menú de tablas de la base de datos KQL.

5. Ahora que se han creado las tablas, debe cargar datos en ellas. Creará una directiva de
actualización para transformar los datos y transferirlos cuando se ingieran en la Bronze Layer. Copie y pegue el siguiente script y luego **Ejecute** el código.

```
// use update policies to transform data during Ingestion

.execute database script <|

.create function ifnotexists with (docstring = 'Add ingestion time to raw data') ParseAddress (){ Address
| extend IngestionDate = ingestion_time()
}

.alter table SilverAddress policy update @'[{"Source": "Address", "Query": "ParseAddress", "IsEnabled" : true, "IsTransactional": true }]'

.create function ifnotexists with (docstring = 'Add ingestion time to raw data') ParseCustomer (){ Customer
| extend IngestionDate = ingestion_time()
}

.alter table SilverCustomer policy update @'[{"Source": "Customer", "Query": "ParseCustomer", "IsEnabled" : true, "IsTransactional": true }]'

.create function ifnotexists with (docstring = 'Add ingestion time to raw data') ParseSalesOrderHeader (){ SalesOrderHeader
| extend DaysShipped = datetime_diff('day', ShipDate, OrderDate)
| extend IngestionDate = ingestion_time()
}

.alter table SilverSalesOrderHeader policy update @'[{"Source": "SalesOrderHeader", "Query": "ParseSalesOrderHeader", "IsEnabled" : true, "IsTransactional": true }]'

.create function ifnotexists with (docstring = 'Add ingestion time to raw data') ParseSalesOrderDetail () { SalesOrderDetail
| extend IngestionDate = ingestion_time()
}

.alter table SilverSalesOrderDetail policy update @'[{"Source": "SalesOrderDetail", "Query": "ParseSalesOrderDetail", "IsEnabled" : true, "IsTransactional": true }]'
```

6. Aunque podrá ver los resultados de la ejecución de la consulta, la mejor prueba de que su consulta se completó es que verá una nueva carpeta expandible en su panel Objetos de base de datos. Haga clic en el **icono >** junto a la **Functions**. Estas funciones permitirán que los datos cargados en la Bronze Layer de la base de datos KQL se reflejen, transformen y carguen en la Silver Layer.
 
7. Ahora simulemos este proceso, ejecutará de nuevo la canalización que creó anteriormente en este laboratorio. Ahora regrese a la canalización **Load KQL Database**.

8. Simplemente haga clic en el botón **Ejecutar** dentro de la **cinta de opciones de Inicio** para ejecutar de nuevo la canalización y cargar los datos en la Bronze Layer, donde luego serán transformados por las funciones que creó y cargarán en las tablas Plata.

9. Haga clic en **Aceptar** en este control flotante para ejecutar la canalización con los mismos parámetros que antes.
 
10.	De nuevo, espere aproximadamente un minuto a que la canalización complete su carga y cuando todos los elementos de su menú Salida indiquen **Correcto**, continúe con el siguiente paso.

11.	Una vez que se haya completado la canalización de datos, vaya y valide los resultados en la base de datos KQL. Vuelva al conjunto de consultas KQL **Crear tablas** y vaya a la pestaña **Silver Layer**.

12.	En una nueva línea, consulte la tabla SilverAddress escribiendo la siguiente consulta y ejecutando el código.

```
SilverAddress
| take 100
```

13.	Observe en los resultados que la tabla **SilverAddress** tiene una columna adicional, **IngestionDate**, que no se encuentra físicamente presente en la tabla **Address**.

## Tarea 4: Crear una Gold Layer con vistas materializadas

Ahora que tiene su capa de datos transformada dentro de la Silver Layer, puede comenzar a realizar análisis con datos fiables, validados y enriquecidos dentro de un informe de Power BI, conjunto de datos RTI o simplemente creando algunas consultas KQL. Sin embargo, puede haber ocasiones en las que crea necesario agregar sus datos para que sean más útiles para los usuarios finales. Veamos cómo se logra esto dentro de una base de datos KQL.
 
1. Si aún no lo está, abra el conjunto de consultas KQL **Crear tablas** y cree una nueva pestaña con la etiqueta “Gold Layer”.

2.	Pegue el siguiente código en el conjunto de consultas para crear una vista materializada.

```
//GOLD LAYER
// use materialized views to view the latest changes in the SilverAddress table
.create materialized-view with (backfill=true) GoldAddress on table SilverAddress
{
SilverAddress
| summarize arg_max(IngestionDate, *) by AddressID
}
```

3. Una vez pegado el código, resáltelo y ejecútelo haciendo clic en el botón **Ejecutar**.

4. Verá una salida en los resultados de la consulta que detalla información sobre cómo se creó esta vista materializada.

5. También observará que se creó otra carpeta en el explorador de objetos de la base de datos KQL. Expanda la carpeta **Materialized View** y encontrará su vista **GoldAddress** dentro.

6. En la ventana de la consulta, ejecute el código siguiente para consultar la nueva vista materializada.

```
GoldAddress
| take 1000
```

7. Esta consulta devolverá la fila con la última **IngestionDate** para cada **AddressID** única de la tabla
**SilverAddress**.

8.	Ahora, pegue y ejecute las siguientes consultas para crear más vistas materializadas de la Gold Layer para las otras tablas.

```
//Create additional Gold Materialized Views
.execute database script <|

.create materialized-view with (backfill=true) GoldCustomer on table SilverCustomer
{
SilverCustomer
| summarize arg_max(IngestionDate, *) by CustomerID
}

.create materialized-view with (backfill=true) GoldSalesOrderHeader on table SilverSalesOrderHeader
{
SilverSalesOrderHeader
| summarize arg_max(IngestionDate, *) by SalesOrderID
}

.create materialized-view with (backfill=true) GoldSalesOrderDetail on table SilverSalesOrderDetail
{
SilverSalesOrderDetail
| summarize arg_max(IngestionDate, *) by SalesOrderDetailID
}

.create async materialized-view with (backfill=true) GoldDailyClicks on table Clicks
{
Clicks
| extend dateOnly = substring(tostring(todatetime(eventDate)), 0, 10)
| summarize count() by dateOnly
}

.create async materialized-view with (backfill=true) GoldDailyImpressions on table Impressions
{
Impressions
| extend dateOnly = substring(tostring(todatetime(eventDate)), 0, 10)
| summarize count() by dateOnly
}
```

9. Ahora debería tener seis vistas materializadas dentro de su base de datos KQL.

10.	Ahora ha creado correctamente un marco de trabajo Medallion dentro de una base de datos KQL. Aunque estos datos se consumen con facilidad, tendrá usuarios que nunca han trabajado con Kusto y preferirán acceder a los datos de estas tablas por otros medios. En la siguiente tarea creará un lago de datos. A continuación, mediante la característica Disponibilidad de OneLake, que activamos en el laboratorio 1, haga que algunas de las tablas de nuestra base de datos de KQL sean accesibles a través del almacén de lago mediante accesos directos

# Disponibilidad de OneLake y almacén de lago de Fabric

## Tarea 5: Crear un almacén de lago

1. Vuelva al área de trabajo **RTI_username**.

2. Haga clic en la opción **+ Nuevo elemento** y seleccione **Lakehouse** desde la lista de opciones disponibles.

3.	Asigne el nombre **al lh_Fabrikam** al almacén de lago y luego haga clic en **Crear**. No habilitar la característica en vista previa de los esquemas de almacén de lago.

## Tarea 6: Acceso directo a tablas de base de datos KQL

Dentro de la interfaz de usuario del almacén de lago, tiene un par de opciones sobre cómo puede llevar los datos de transmisión al propio almacén de lago. Una opción mencionada
anteriormente en la clase es usar un Eventstream para cargar datos directamente en el almacén de lago desde el Event Hub en lugar de la base de datos KQL. Puesto que ya ha decidido que las bases de datos KQL se utilizarán para cumplir objetivos y requisitos específicos, no desea volver a copiar esos datos. En su lugar, vamos a usar un **acceso directo** para traer esos datos de la base de datos KQL que ya tenemos, de modo que los usuarios más familiarizados con esta experiencia puedan tener acceso a los datos que hemos estado usando en la base de datos KQL

1. Elija la opción del menú que dice **Nuevo acceso directo**.

2. Seleccione la opción **Microsoft OneLake** en los **Orígenes internos**.

3. En el menú, seleccione la base de datos KQL **eh_Fabrikam** para llevar tablas de ese almacenamiento al almacén de lago sin duplicar ni copiar los datos.

4. Haga clic en **Siguiente** en la parte inferior del menú.
 
5. Abra las tablas dentro de **eh_Fabrikam**; para ello, haga clic en el **icono >** y seleccione las siguientes tablas para que traerlas.

    - Clicks

    - Impressions

    - InternetSales

6. Estas tablas podrían resultar muy útiles para cualquier usuario que esté aprovechando los cuadernos dentro de Fabric. Estos datos se pueden utilizar en experimentos de la ciencia de datos para entrenar un modelo que prediga qué vínculos pueden interesar a los usuarios.

7. Haga clic en **Siguiente**.

8. Ap arecerá una última pantalla de validación. Una vez que esté satisfecho con su selección, haga clic en el botón **Crear** en la parte inferior de la pantalla.
 
9. Ahora verá que todas las tablas que seleccionó de la base de datos KQL han aparecido en el almacén de lago.

10.	Haga clic en la tabla denominada **Clicks**.

11.	Puede ver un ejemplo de los registros de esa tabla que han aparecido en su interfaz de usuario.

    > **Nota:** Pueden transcurrir varias horas hasta que aparezcan los datos en OneLake. (https://learn.microsoft.com/en-us/fabric/real-time-intelligence/event-house-onelake- availability)


# Resumen

En este laboratorio, los usuarios crearon un marco de trabajo Medallion dentro de una base de datos de lenguaje de consulta Kusto (KQL). Los usuarios ingirieron datos sin procesar en la Bronze Layer
de la arquitectura Medallion mediante una canalización de datos. Transformaron estos datos y los cargaron en la Silver Layer para su posterior procesamiento y refinamiento. Finalmente, los usuarios agregaron y optimizaron los datos para el análisis en la Gold Layer mediante Vistas materializadas.
 
Después de crear el marco Medallion, los usuarios emplearon accesos directos de Microsoft Fabric para vincular los datos de la base de datos KQL a un almacén de lago. Esta integración permitió un acceso y análisis de los datos sin problemas en ambos entornos. El laboratorio concluyó con la
verificación por parte de los usuarios de la vinculación de datos y la realización de consultas básicas para garantizar la funcionalidad del marco.

# Referencias

Fabric Real-Time Intelligence in a Day (RTIIAD) le presenta algunas funciones clave disponibles en Microsoft Fabric.

En el menú del servicio, la sección Ayuda (?) tiene vínculos a algunos recursos excelentes.

Estos son algunos recursos más que podrán ayudarle a seguir avanzando con Microsoft Fabric.

- Vea la publicación del blog para leer el [anuncio de disponibilidad general de Microsoft Fabric completo](https://aka.ms/Fabric-Hero-Blog-Ignite23)

- Explore Fabric a través de la [Visita guiada](https://aka.ms/Fabric-GuidedTour)

- Regístrese en la [prueba gratuita de Microsoft Fabric](https://aka.ms/try-fabric)

- Visite el [sitio web de Microsoft Fabric](https://aka.ms/microsoft-fabric)

- Adquiera nuevas capacidades mediante la exploración de los [módulos de aprendizaje de Fabric](https://aka.ms/learn-fabric)

- Explore la [documentación técnica de Fabric](https://aka.ms/fabric-docs)

- Lee el [libro electrónico gratuito sobre cómo empezar a usar Fabric](https://aka.ms/fabric-get-started-ebook)

- Únase a la [comunidad de Fabric](https://aka.ms/fabric-community) para publicar sus preguntas, compartir sus comentarios y aprender de otros.

Obtenga más información en los blogs de anuncios de la experiencia Fabric:

- [Experiencia de Data Factory en el blog de Fabric](https://aka.ms/Fabric-Data-Factory-Blog)

- [Experiencia de Synapse Data Engineering en el blog de Fabric](https://aka.ms/Fabric-DE-Blog)

- [Experiencia de Synapse Data Science en el blog de Fabric](https://aka.ms/Fabric-DS-Blog)

- [Experiencia de Synapse Data Warehousing en el blog de Fabric](https://aka.ms/Fabric-DW-Blog)

- [Experiencia de Real-Time Intelligence en el blog de Fabric](https://blog.fabric.microsoft.com/en-us/blog/category/real-time-intelligence)

- [Blog de anuncios de Power BI](https://aka.ms/Fabric-PBI-Blog)

- [Experiencia de Data Activator en el blog de Fabric](https://aka.ms/Fabric-DA-Blog)

- [Administración y gobernanza en el blog de Fabric](https://aka.ms/Fabric-Admin-Gov-Blog)

- [OneLake en el blog de Fabric](https://aka.ms/Fabric-OneLake-Blog)

- [Blog de integración de Dataverse y Microsoft Fabric](https://aka.ms/Dataverse-Fabric-Blog)

© 2024 Microsoft Corporation. Todos los derechos reservados.

Al participar en esta demostración o laboratorio práctico, acepta las siguientes condiciones:

Microsoft Corporation pone a su disposición la tecnología o funcionalidad descrita en esta demostración/laboratorio práctico con el fin de obtener comentarios por su parte y de facilitarle una experiencia de aprendizaje. Esta demostración/laboratorio práctico solo se puede usar para evaluar las características de tal tecnología o funcionalidad y para proporcionar comentarios a Microsoft. No se puede usar para ningún otro propósito. Ninguna parte de esta demostración/laboratorio práctico se puede modificar, copiar, distribuir, transmitir, mostrar, realizar, reproducir, publicar, licenciar, transferir ni vender, ni tampoco crear trabajos derivados de ella.

LA COPIA O REPRODUCCIÓN DE ESTA DEMOSTRACIÓN/LABORATORIO PRÁCTICO (O PARTE DE ELLA) EN CUALQUIER OTRO SERVIDOR O UBICACIÓN PARA SU REPRODUCCIÓN O DISTRIBUCIÓN POSTERIOR QUEDA EXPRESAMENTE PROHIBIDA.

ESTA DEMOSTRACIÓN/LABORATORIO PRÁCTICO PROPORCIONA CIERTAS FUNCIONES Y CARACTERÍSTICAS DE PRODUCTOS O TECNOLOGÍAS DE SOFTWARE (INCLUIDOS POSIBLES NUEVOS CONCEPTOS Y CARACTERÍSTICAS) EN UN ENTORNO SIMULADO SIN INSTALACIÓN O CONFIGURACIÓN COMPLEJA PARA EL PROPÓSITO ARRIBA DESCRITO. LA TECNOLOGÍA/ CONCEPTOS DESCRITOS EN ESTA DEMOSTRACIÓN/LABORATORIO PRÁCTICO NO REPRESENTAN LA FUNCIONALIDAD COMPLETA DE LAS CARACTERÍSTICAS Y, EN ESTE SENTIDO, ES POSIBLE QUE NO FUNCIONEN DEL MODO EN QUE LO HARÁN EN UNA VERSIÓN FINAL. ASIMISMO, PUEDE QUE NO SE PUBLIQUE UNA VERSIÓN FINAL DE TALES CARACTERÍSTICAS O CONCEPTOS. DE IGUAL MODO, SU EXPERIENCIA CON EL USO DE ESTAS CARACTERÍSTICAS Y FUNCIONALIDADES EN UN
ENTORNO FÍSICO PUEDE SER DIFERENTE.

**COMENTARIOS**. Si envía comentarios a Microsoft sobre las características, funcionalidades o conceptos de tecnología descritos en esta demostración/laboratorio práctico, acepta otorgar a Microsoft, sin cargo alguno, el derecho a usar, compartir y comercializar sus comentarios de cualquier modo y para cualquier fin. También concederá a terceros, sin cargo alguno, los derechos de patente necesarios para que sus productos, tecnologías y servicios usen o interactúen con cualquier parte específica de un software o servicio de Microsoft que incluya los comentarios. No enviará comentarios que estén sujetos a una licencia que obligue a Microsoft a conceder su software o documentación bajo licencia a terceras partes porque incluyamos sus comentarios en ellos. Estos derechos seguirán vigentes después del vencimiento de este
acuerdo.
 
MICROSOFT CORPORATION RENUNCIA POR LA PRESENTE A TODAS LAS GARANTÍAS Y CONDICIONES RELATIVAS A LA DEMOSTRACIÓN/LABORATORIO PRÁCTICO, INCLUIDA CUALQUIER GARANTÍA Y CONDICIÓN DE COMERCIABILIDAD (YA SEA EXPRESA, IMPLÍCITA O ESTATUTARIA), DE IDONEIDAD PARA UN FIN DETERMINADO, DE TITULARIDAD Y DE AUSENCIA DE INFRACCIÓN.
MICROSOFT NO DECLARA NI GARANTIZA LA EXACTITUD DE LOS RESULTADOS, EL RESULTADO DERIVADO DE LA REALIZACIÓN DE LA DEMOSTRACIÓN/LABORATORIO PRÁCTICO NI LA IDONEIDAD DE LA INFORMACIÓN CONTENIDA EN ELLA CON NINGÚN PROPÓSITO.

## DECLINACIÓN DE RESPONSABILIDADES

Esta demostración/laboratorio práctico contiene solo una parte de las nuevas características y mejoras realizadas en Microsoft Power BI. Puede que algunas de las características cambien en versiones futuras del producto. En esta demostración/laboratorio práctico, conocerá algunas de estas nuevas características, pero no todas.
