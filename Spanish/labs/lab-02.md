# Microsoft Fabric Real-Time Intelligence in a Day Laboratorio 2

# Contenido
- Estructura del documento	
- Introducción	
- Centro en tiempo real de Fabric	
  - Tarea 1: Crear un origen de flujo de eventos	
  - Tarea 2: Configurar destino de Eventstream	
- Lenguaje de consulta Kusto (KQL)	
  - Tarea 3: Creación de consultas de base de datos Kusto	
  - Tarea 4: Uso de consultas SQL en una base de datos de KQL	
- Conjunto de consultas KQL	
  - Tarea 5: Trabajar con un conjunto de consultas KQL	
- Resumen	
- Referencias	

 
# Estructura del documento

El laboratorio incluye pasos que el usuario debe seguir junto con capturas de pantalla asociadas que sirven de ayuda visual. En cada captura de pantalla, las secciones se resaltan con cuadros de color
naranja para indicar en qué áreas debe centrarse el usuario.

# Introducción
En este laboratorio, experimentará una forma de controlar un flujo continuo de datos en tiempo real. Usará un objeto de Fabric Real-Time Intelligence denominado Eventstream para ingerir estos datos en la Eventhouse que creó en el último laboratorio y escribir algunas consultas KQL básicas.

Al final de este laboratorio, habrá aprendido:

- Cómo crear un Eventstream
- Cargar datos en tiempo real en una base de datos KQL
- Escribir consultas básicas del lenguaje de consulta Kusto

# Centro en tiempo real de Fabric
## Tarea 1: Crear un origen de flujo de eventos

1. Abra el **espacio de trabajo de Fabric** que creó en el último laboratorio. Desde aquí podemos ver la Eventhouse que creamos.

2. Navegue hasta el Centro en tiempo real; para ello, seleccione el botón **Tiempo real** a la izquierda. Pese a que no veamos flujos de datos que vaya a cambiar próximamente.
 
3. Seleccione el botón verde **+ Conectar origen de datos** que debería estar en la esquina superior derecha.
 
4. Se abrirá una ventana donde podrá seleccionar un origen para los datos del flujo. Como hemos comentado antes, hay muchas opciones fantásticas entre las que elegir, pero para esta clase vamos a seleccionar la opción “Azure Event Hubs”. Si le resulta complicado encontrar “Azure Event Hubs”, seleccione **Orígenes de Microsoft** en la parte superior para filtrar las opciones que aparecen.

5. Ahora debe crear una conexión al Centro de eventos de Azure. Haga clic en el texto **Nueva conexión**, ya que actualmente no tiene una conexión.

6. Desde la página de detalles de su entorno, copie y pegue toda la configuración de conexión
necesaria en los campos correspondientes. Para estos laboratorios, nos conectaremos a un centro de eventos que tiene datos de transmisión que se envían desde un cuaderno de Python. Este cuaderno está creando transacciones de ventas falsas a un ritmo de unas 3100 transacciones por hora.

    - Espacio de nombres del centro de eventos: **rtiadhub{userid} (de cloudlabs)**
    - Centro de eventos: **rti-iad-fabrikam** 
    - Nombre de la clave de acceso compartida: **rti-reader**
    - clave de acceso compartida: **disponible en la pestaña Destalles del entorno**

7. Una vez que se hayan completado todas las propiedades, haga clic en **Conectar**.

8. En la configuración del origen de datos del Centro de eventos de Azure, es posible que deba
modificar el **Grupo de consumidores** del Centro de eventos para asegurarse de que obtiene acceso a un punto de acceso único al flujo de datos. Para este taller, puede dejar el valor “$Default” como se
indica a continuación.
 
9. Antes de finalizar este origen de datos y Eventstream, sigamos adelante y cambiemos el nombre de nuestro Eventstream por otra alternativa más práctica. En la sección “Detalles de la secuencia” a la derecha, seleccione el icono de lápiz junto a “Nombre de Eventstream” y llamemos a nuestro Eventstream “**es_Fabrikam_InternetSales**”.
 
10.	Ahora podemos hacer clic en **Siguiente**, que nos llevará a una página de información general final.

11.	En esta pantalla de información general, compruebe que el contenido se vea correctamente y haga clic en **Crear origen**.

>**Nota**: Los detalles serán diferentes a lo que aparece en la captura de pantalla.
 
12.	Una vez que se ha creado el Eventstream y el origen de Eventstream, seleccione la opción “**Abrir Eventstream**”

13.	Esto le llevará a la interfaz de usuario de Eventstream. Aquí es donde verá que el flujo de origen se dirige hacia el Eventstream y también podemos agregar eventos de transformación.

14.	Es posible que su origen tarde unos minutos en estar **Activo**, pero tras esperar unos instantes, haga clic en el icono central con el nombre del Eventstream y, después, en Actualizar si no aparece una vista preliminar de los datos.

>**Nota**: No pasa nada si recibe un estado de “Advertencia” y una directiva de auditoría. El flujo seguirá funcionando

15.	Ahora debería ver una muestra de los datos en la ventana inferior.
 
16.	Se mostrará una versión preliminar de los datos que se reciben del Centro de eventos de Azure. Si desliza la barra de desplazamiento horizontal inferior hasta el lado derecho de la vista previa,
podrá ver la hora en que se recibieron los datos en el Centro de eventos en dos columnas denominadas: **EventProcessedUtcTime** y **EventEnqueuedUtcTime**. Esto debería reflejar la fecha/hora actual en formato UTC.

## Tarea 2: Configurar destino de Eventstream

1. Haga clic en el icono dentro del área del lienzo con la etiqueta “Cambiar al modo de edición Evento de transformación o agregar destino

2. Dentro de la interfaz de usuario de Eventstream, haga clic en la opción Transformar eventos o agregar destino para abrir el menú desplegable.
 
3. Observe la lista de operaciones disponibles que se pueden realizar en el flujo.
 
4. Debajo de la sección Operaciones, está Destinos, donde podrá seleccionar la opción que indica
Eventhouse.

5. Se abrirá un nuevo menú en el lado derecho de la pantalla. Lo primero que debe modificar para el destino es el modo de ingesta de datos. Las dos opciones son Ingesta directa y Procesamiento de eventos antes de la ingesta. Puesto que no vamos a transformar nada en nuestro
Eventstream ni cargar directamente esta información en una tabla de base de datos KQL, compruebe que ha seleccionado la opción Ingesta directa.

6. Modifique el resto de la configuración con los datos que figuran a continuación.
  - Nombre del destino: **eh-kql-db-fabrikam**
  - Área de trabajo: **RTI_username**
  - Eventhouse: **eh_Fabrikam**
  - Base de datos KQL: **eh_Fabrikam**
 
7. Haga clic en Guardar.

8. Con el Eventstream configurado, haga clic en el botón **Publicar** para guardar este Eventstream y comenzar la ingesta.

9. Si observa que el origen de **AzureEventHub** pasa a estar inactivo, cambie el control de
alternancia a estado “Activo” y elija la opción “Ahora” cuando se abra el cuadro de diálogo.

10.	Elija la opción **Configurar** en el **Destino** para asignar correctamente el flujo a una tabla en la base de datos KQL.
 
11.	Haga clic en la opción **+ Nueva tabla** debajo de la base de datos **eh_Fabrikam**.
 
12.	Asigne el nombre **InternetSales** a la nueva tabla y luego haga clic en la marca de verificación.

13.	Es posible que tenga que actualizar su “**Nombre de conexión de datos**” para cumplir los requisitos. Cambiémosle el nombre a “**eh_Fabrikam_es_InternetSales**”. A continuación, podemos hacer clic en **Siguient**e.

14.	Después de unos minutos de buscar eventos, la interfaz de usuario debería permitirle ver que se encontraron datos de ejemplo. Haga clic en **Fin** en la parte inferior de la pantalla. 
 
15.	Después de esto, se le mostrará un resumen. Una vez que tenga todas las marcas de verificación verdes, haga clic en **Cerrar** para avanzar.

16.	Una vez que vea la interfaz de usuario que muestra las asignaciones desde el origen hasta el Eventstream y el destino, habrá configurado e iniciado correctamente un flujo de datos en su base de datos KQL.

# Lenguaje de consulta Kusto (KQL)
## Tarea 3: Creación de consultas de base de datos Kusto

1. Regrese a su área de trabajo **RTI_username**. Debería aparecer el nuevo Eventstream que acaba de crear junto con todos los elementos de Evenhouse.

2. Abra el elemento de la base de datos KQL **eh_Fabrikam**.

3. En este proceso, puede obtener información general de la estructura, el tamaño y el uso actuales de la base de datos KQL. Puesto que Eventstream envía datos a esta base de datos KQL de forma constante, observará que la cantidad de almacenamiento aumenta con el tiempo.
 
4. Haga clic en el **icono de actualización** en la esquina superior derecha de la pantalla.

5. El tamaño de la base de datos debería ser mayor. Es posible que el valor que vea no sea exacto en comparación con el que aparece en las capturas de pantalla en el resto del laboratorio. En función del tiempo que invierta en completar el contenido, recibirá más o menos registros que otros asistentes a la clase. Es no es importante y no afectará a la capacidad para seguir el laboratorio.

6. En el área de navegación de la base de datos, a la izquierda de la pantalla, haga clic en la tabla dentro de la base de datos KQL llamada **InternetSales** y verá una descripción general de la tabla.

7.	Esta información general le ofrece detalles de metadatos relativos a la tabla que ha creado
y cualquier dato en streaming activo con el Eventstream. Una vez más, el tamaño de la tabla y el número de filas dentro de la tabla variarán de un alumno a otro y no afectarán a los resultados
 
finales de este laboratorio ni de ningún otro. En este menú destacan algunos elementos adicionales:

- **Data Activity Tracker**: muestra el número de filas ingeridas, la hora en que se generó por última vez y el intervalo de visualización.

- **Data preview**: muestra una versión preliminar de los resultados de la ingesta de la tabla.

- **Schema insights**: incluye detalles sobre el nombre de la columna y los tipos de datos de la columna que se pueden consultar con KQL. También muestra el recuento de los 10 valores primeros de la columna seleccionada.

- **Detalles de la tabla**: muestra el tamaño comprimido y original de la tabla, la disponibilidad de OneLake, el número de filas en las tablas y otros detalles.

8. Vuelva a la vista de la base de datos y haga clic en **Explorar sus datos** en la esquina superior derecha.

9. Esto abrirá el conjunto de consultas KQL predeterminado que se creó junto con Eventhouse. Ya
se han creado algunas consultas generadas previamente por script, pero que necesitan una ligera personalización. También hay dos vínculos a la documentación de Microsoft que pueden resultar de utilidad al aprender sobre KQL o también al observar las conversiones de SQL a KQL, que se
tratarán más adelante a lo largo de esta clase.
 
10.	Haga clic en la **línea 8** y, donde la consulta indica, **YOUR_TABLE_HERE**, reemplácela por el nombre de la tabla, **InternetSales**.
 
11.	Resalte las **líneas 8 y 9** y haga clic en el botón **Run** en la esquina superior izquierda de la ventana.

12.	La consulta utiliza el operador **take** para devolver un número específico de filas. Cuando se
ejecute la consulta, se extraerán los datos de la tabla InternetSales y se recuperará el número de filas que haya conectado a la consulta. Para este ejemplo, solo se devolverán 100 filas, como una cláusula WHERE en SQL. Las filas específicas devueltas no se pueden determinar con este operador y los resultados de la consulta variarán respecto al resultado de otras.

13.	Haga clic en la **línea 12** y, donde la consulta dice, **YOUR_TABLE_HERE**, reemplácela por el nombre de la tabla, **InternetSales**.

14.	Resalte las **líneas 12 y 13 y** haga clic en el botón **Run** en la esquina superior izquierda de la ventana.

15.	Esta consulta utiliza el operador operador count. Esta consulta devolverá un número agregado de registros que existen en el momento de la ejecución de la consulta en la tabla de la base de datos KQL. No dude en ejecutar esta consulta unas cuantas veces más y debería observar que el número de registros aumenta cada pocos segundos.
 
16.	Repita los pasos anteriores para la consulta final que se crea automáticamente en la **línea 16/17**
y vuelva a ejecutar la consulta.

17.	Esta consulta arrojará el número de registros que se han ingerido en la tabla en una horquilla de una hora. La distribución general de estos registros para los datos que está ingiriendo
actualmente será de aproximadamente 4100 por hora. Sin embargo, habrá ligeras variaciones dentro de la cantidad de transacciones por hora y esta consulta detallará si se ingirieron menos o más registros en cada ventana.

## Tarea 4: Uso de consultas SQL en una base de datos de KQL

Es posible que esté trabajando con el lenguaje de consulta Kusto por primera vez. Aunque este lenguaje es intuitivo y fácil de aprender para consultas sencillas, es posible que desee devolver los resultados de consultas más complejas de las que puede hacer actualmente. Se han incluido varias herramientas útiles dentro de las funciones del conjunto de consultas KQL, incluida la conversión de consultas SQL en consultas KQL y la creación simple de consultas T-SQL dentro del conjunto de
consultas KQL. ¡Exploremos!

1. Debe crear una consulta que devuelva cada producto con el número de la cantidad de veces que se haya vendido. Esto se puede hacer rápidamente con T-SQL. Dentro de la ventana de consulta, puede traducir sus consultas SQL a KQL para entender mejor cómo crear consultas KQL en adelante. Empiece escribiendo el siguiente comando.

>**Nota: Haga doble clic en el objeto que figura a continuación para poder copiar el texto**


2. La línea de comentario “--” seguida de la palabra clave “explicar” le permitirá crear ahora una consulta SQL y devolver un resultado con la consulta KQL que podría utilizarse para lograr una consulta y un resultado similares. A continuación, introduzca la siguiente consulta para explicar el aspecto de la consulta KQL:

  
3. Se trata de una consulta SQL sencilla que recuperará resultados de la tabla InternetSales para devolver dos columnas, la clave del producto y un recuento del número de pedidos. Debido a que hay una columna agregada y una columna no agregada, debe usar un GROUP BY para
devolver resultados para cada producto individual. Ejecute toda la consulta desde “--” hasta el final de la consulta T-SQL.

4. La salida de la consulta de explicación debe ser un único registro con la consulta KQL traducida como resultado. Haga clic en el **icono de intercalación (>)** para expandir los resultados y facilitar la traducción.
 
5. Haga clic en el panel de consultas resaltado debajo en naranja. Esto le permitirá seleccionar la consulta KQL traducida y copiarla. Pegue esta consulta en el conjunto de consultas KQL que hemos estado usando

6. Con los resultados en su panel de consultas, resalte y ejecute la consulta para recuperar los
resultados. El operador **summarize** producirá una tabla que agrega el contenido de la tabla de entrada mientras determina cómo agrupar cada registro con la **clave de producto by** y el operador del **proyecto** seleccionará las columnas que desea incluir, cambiar de nombre o eliminar mientras inserta nuevas columnas calculadas.

7. No dude en explorar la lista de operaciones de hoja de referencia de SQL a KQL en la parte superior de su conjunto de consultas para obtener capacidades y conversiones adicionales.
 
8. En lugar de usar KQL, otra alternativa para consultar los resultados de la base de datos KQL
dentro de Fabric es escribir y ejecutar una consulta T-SQL. Resalte la instrucción SQL original que se utilizó para traducir la consulta KQL y ejecute solo esa.
 
9.	Esto también arrojará resultados perfectamente válidos sin tener que convertir a KQL de antemano.

# Conjunto de consultas KQL
## Tarea 5: Trabajar con un conjunto de consultas KQL

1. Si bien la mayoría de las consultas dentro de esta ventana se crearon automáticamente desde la interfaz de usuario, podrían darse situaciones en el futuro en las que desee crear sus propias consultas KQL desde cero. Esto se puede administrar a través de la característica de pestañas en la parte superior. También hay que tener en cuenta que este conjunto de consultas se guarda
automáticamente de forma periódica.

2. Observe que, en la parte superior del conjunto de consultas, el nombre predeterminado de nuestra primera página es el mismo que el de la base de datos.
 
3. Sigamos avanzando y vamos a cambiar el nombre de esta pestaña a “**My First KQL Query**” haciendo clic en el icono del lápiz.

4. A partir de ahora, si queremos aislar nuestro código, bastará con crear pestañas adicionales haciendo clic en el icono “+”.

5. Regrese a su área de trabajo **RTI_username**. Debe tener presentes los siguientes objetos

 
## Resumen
En este laboratorio, ha comenzado configurando una conexión a un Event Hub que tenga un flujo de datos en ejecución y ha usado un Eventstream para tomar esos datos e ingerirlos en una base de datos KQL. Una vez que se ingirieron los datos, pudo crear varias consultas KQL y ver la funcionalidad de usar T-SQL para ayudar a aprender la sintaxis KQL o simplemente devolver resultados con instrucciones SQL.

## Referencias
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

**COMENTARIOS**. Si envía comentarios a Microsoft sobre las características, funcionalidades o conceptos de tecnología descritos en esta demostración/laboratorio práctico, acepta otorgar a Microsoft, sin cargo alguno, el derecho a usar, compartir y comercializar sus comentarios de cualquier modo y para cualquier fin. También concederá a terceros, sin cargo alguno, los derechos de patente necesarios para que sus productos, tecnologías y servicios usen o interactúen con cualquier parte específica de un software o servicio de Microsoft que incluya los comentarios. No enviará comentarios que estén sujetos a una licencia que obligue a Microsoft a conceder su software o documentación bajo licencia a terceras partes porque incluyamos sus comentarios en ellos. Estos derechos seguirán vigentes después del vencimiento de este acuerdo.
 
MICROSOFT CORPORATION RENUNCIA POR LA PRESENTE A TODAS LAS GARANTÍAS Y CONDICIONES RELATIVAS A LA DEMOSTRACIÓN/LABORATORIO PRÁCTICO, INCLUIDA CUALQUIER GARANTÍA Y CONDICIÓN DE COMERCIABILIDAD (YA SEA EXPRESA, IMPLÍCITA O ESTATUTARIA), DE IDONEIDAD PARA UN FIN DETERMINADO, DE TITULARIDAD Y DE AUSENCIA DE INFRACCIÓN.MICROSOFT NO DECLARA NI GARANTIZA LA EXACTITUD DE LOS RESULTADOS, EL RESULTADO DERIVADO DE LA REALIZACIÓN DE LA DEMOSTRACIÓN/LABORATORIO PRÁCTICO NI LA IDONEIDAD DE LA INFORMACIÓN CONTENIDA EN ELLA CON NINGÚN PROPÓSITO.

## DECLINACIÓN DE RESPONSABILIDADES

Esta demostración/laboratorio práctico contiene solo una parte de las nuevas características y mejoras realizadas en Microsoft Power BI. Puede que algunas de las características cambien en versiones futuras del producto. En esta demostración/laboratorio práctico, conocerá algunas de estas nuevas características, pero no todas.
