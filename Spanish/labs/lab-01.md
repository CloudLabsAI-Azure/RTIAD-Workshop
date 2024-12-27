# Contenido

- Estructura del documento	
- Escenario/planteamiento del problema	
- Introducción	
- Licencia de Fabric	
    -Tarea 1: Habilitar una licencia de prueba de Microsoft Fabric	
- Real-Time Intelligence y Centro en tiempo real	
    - Tarea 2: Elemento de experiencia de Real-Time Intelligence	
    - Tarea 3: Centro en tiempo real	
- Crear espacio de trabajo y Eventhouse	
    - Tarea 4: Crear un espacio de trabajo de Fabric	
    - Tarea 5: Crear una Eventhouse	
- Referencias	

 
# Estructura del documento

El laboratorio incluye pasos que el usuario debe seguir junto con capturas de pantalla asociadas que sirven de ayuda visual. En cada captura de pantalla, las secciones se resaltan con cuadros de color naranja para indicar en qué áreas debe centrarse el usuario.

# Escenario/planteamiento del problema

Fabrikam es una empresa de comercio electrónico especializada en una amplia gama de equipos y accesorios para actividades al aire libre. La empresa atiende a clientes minoristas a nivel mundial a través de su plataforma en línea y planea mejorar su presencia en nuevos mercados internacionales. Hay una nueva iniciativa que consiste en proporcionar información en tiempo real
desde un sitio de comercio electrónico para brindar a los ejecutivos la capacidad de tomar decisiones oportunas basadas en información actual.

Como ingeniero de análisis en el equipo de ventas, entre sus responsabilidades se incluyen la recopilación, limpieza e interpretación de conjuntos de datos para resolver problemas comerciales. Usted crea y mantiene canalizaciones de datos por lotes, desarrolla visualizaciones como cuadros y gráficos, crea y optimiza informes y modelos semánticos integrales, y presenta sus hallazgos a los responsables de toma de decisiones de la organización.

## Desafíos actuales

- Debe controlar un flujo continuo de datos en tiempo real del sitio web de comercio electrónico, lo que requiere una arquitectura sólida y escalable.

- Garantizar el procesamiento y análisis de datos en tiempo real para mantenerse al día con la naturaleza acelerada de las ventas en línea.

- Manejar el volumen y la velocidad de los datos generados por las interacciones de los usuarios, las transacciones y la actividad del sitio web.

- Integrar datos de transmisión en tiempo real con datos históricos para un análisis exhaustivo.

- Usar la arquitectura Medallion en un entorno de Eventhouse para estructurar el flujo de datos de manera eficiente.

- Aprovechar los datos de Eventhouse dentro de un almacén de lago

- Le interesa aprovechar Microsoft Fabric para abordar los desafíos anteriores, utilizando Eventhouse, KQL Database y Eventstream para crear una canalización de procesamiento de datos resistente y eficiente.
 
# Introducción

Hoy tendrá ocasión de aprender diversas características clave de Microsoft Fabric. Este es un taller introductorio destinado a presentarle las diversas experiencias de productos y artículos disponibles en Fabric. Al final de este taller, habrá aprendido a usar Eventhouse, la canalización de datos, Eventstream, el conjunto de consultas KQL y el panel de información en tiempo real.

Al final de este laboratorio, habrá aprendido a:

- Explorar roles de Fabric

- Crear un área de trabajo de Fabric

- Crear una Eventhouse

# Licencia de Fabric

## Tarea 1: Habilitar una licencia de prueba de Microsoft Fabric

1. Abra el explorador de **Microsoft Edge** en el escritorio y vaya a https://app.fabric.microsoft.com/. Se le llevará a la página de inicio de sesión. 

    > **Nota:** Si no está utilizando el entorno de laboratorio y tiene una cuenta de Power BI existente, es posible que desee utilizar el explorador en modo
privado/incógnito.

2. Introduzca el **Nombre de usuario** disponible en la pestaña **Variables de entorno** (al lado de la Guía de laboratorio) como **Correo electrónico** y haga clic en **Enviar**.
 
3. Esto le llevará a la pantalla de **Contraseña**. Introduzca la **Contraseña** disponible en la pestaña
**Variables de entorno** (al lado de la Guía de laboratorio) compartida con usted por el instructor.

4. Haga clic en **Iniciar sesión** y siga las indicaciones para iniciar sesión en Fabric.
 
 
5. Se le dirigirá a la página Inicio de **Microsoft Fabric**.

    > **Nota:** Para trabajar con elementos de Fabric, necesitará una licencia de prueba y un área de trabajo que tenga una licencia de Fabric. Configurémosla.

6. En la esquina superior derecha de la pantalla, seleccione el **icono** del **usuario**.

7. Seleccione **Prueba gratuita**.
 
8. Se abre el cuadro de diálogo Actualizar a una prueba Microsoft Fabric gratuita. Seleccione
**Activar**.

9. Se abre el cuadro de diálogo Se ha actualizado con éxito a una prueba Microsoft Fabric gratuita. Seleccione **Fabric Home Page**.

10.	Se le volverá a dirigir a la **página Inicio de Microsoft Fabric**.
 
# Real-Time Intelligence y Centro en tiempo real

## Tarea 2: Elemento de experiencia de Real-Time Intelligence

1. Haga clic en la experiencia de Real-Time Intelligence.

2. Se le dirigirá a la **página Inicio de Real-Time Intelligence. Verá Plantillas de flujo de tareas,
Elementos recomendados para crear y Obtener más información sobre las categorías de Real- Time Intelligence**. Con la categoría **Recomendado**, observe los elementos:

    a. **Casa de eventos:** se utiliza para crear un área de trabajo de una o varias bases de datos KQL, que se puede compartir entre proyectos. También crea una base de datos KQL dentro de Eventhouse.

    b. **Conjunto de consultas KQL:** se utiliza para ejecutar consultas sobre los datos para producir tablas y objetos visuales que se pueden compartir.

    c. **Panel en tiempo real:** una colección de iconos, organizados de forma opcional en páginas, donde cada icono tiene una consulta subyacente y una representación visual.

    d. **Eventstream:** se utiliza para capturar, transformar y enrutar secuencias de eventos en tiempo real.

    e. **Reflex:** para tomar acciones automáticamente cuando se detectan patrones o condiciones en los datos cambiantes.

 
## Tarea 3: Centro en tiempo real

1. Haga clic en **Centro en tiempo real** dentro del panel de navegación de Fabric, en el lado izquierdo de la pantalla.

2. Se abrirá el cuadro de diálogo **Le damos la bienvenida al Centro en tiempo real** donde le invitamos a seleccionar **Realizar una visita guiada** o, si lo prefiere, opte por **Empiece ya**.
 
3. El Centro en tiempo real es el único lugar para el streaming de datos en movimiento en toda su organización. Cada inquilino de Microsoft Fabric se aprovisiona automáticamente con este centro. Le permite detectar, ingerir, administrar y consumir fácilmente datos en movimiento de una amplia variedad de orígenes.

4. Dentro del Centro en tiempo real, tiene acceso a tres tipos diferentes de integración de datos.

    - **Todos los flujos de datos:** para sus Eventstreams y bases de datos KQL en ejecución, todas las salidas de flujo de Eventstreams y tablas de bases de datos KQL aparecen automáticamente en el Centro en tiempo real.

    - **Orígenes de streaming:** muestra todos los recursos de streaming de los servicios de Microsoft. Ya sean Centros de eventos de Azure, centro de IoT de Azure u otros servicios, puede ingerir datos sin problemas en el Centro en tiempo real.

    - **Eventos de Fabric:** los eventos que se generan a través de artefactos de Fabric y orígenes externos están disponibles en Fabric para admitir escenarios basados en eventos, como alertas en tiempo real y el desencadenamiento de acciones descendentes. Puede
    supervisar y reaccionar a eventos, incluidos eventos de elementos del área de trabajo de Fabric y eventos de Azure Blob Storage.

    - **Eventos de Azure:** en esta lista, se incluyen eventos del sistema generados en Azure a los que se puede acceder. Se puede supervisar un evento y establecer reglas para enviar notificaciones o realizar acciones cuando se activen.

5. En la esquina superior derecha del Centro en tiempo real, haga clic en el botón **+ Conectar origen de datos**.

6. Aparecerá una ventana en la que se detallarán los flujos de datos disponibles actualmente para integrar en el Centro en tiempo real. Esto incluye una combinación de orígenes de Azure, así como orígenes externos de streaming en la nube como Amazon Kinesis, Confluent Cloud Kafka y Google Cloud Pub/Sub. Incluso hay datos de ejemplo que puede explorar.

7. **Cierre** la ventana Obtener eventos haciendo clic en la “X” de la esquina superior derecha.

# Crear espacio de trabajo y Eventhouse

## Tarea 4: Crear un espacio de trabajo de Fabric

1. Ahora creemos un área de trabajo con licencia de Fabric. Seleccione **Áreas de trabajo** en la barra de navegación de la izquierda.

2. Seleccione **+ Nueva área de trabajo.**
 
3. El cuadro de diálogo **Crear un área de trabajo** se abre en el lado derecho del explorador.

4. En el campo **Nombre**, introduzca **RTI_username**. Use el nombre de usuario que se le proporcionó en los detalles del entorno.

    > **Nota:** El espacio de trabajo debe ser único. Asegúrese de que aparezca una marca de verificación verde con “**Este nombre está disponible**” debajo del campo Nombre.

5. Si lo desea, puede escribir una **Descripción** para el área de trabajo. Este campo es opcional.

6. Haga clic para expandir la sección **Avanzado**.

7. En **Modo de licencia**, asegúrese de que **Prueba** esté seleccionado. (Debería estar seleccionado por defecto).

8. Seleccione **Aplicar** para crear una nueva área de trabajo.

    > **Nota:** Si se abre el cuadro de diálogo **Introducción a los flujos de trabajo**, haga clic en **Entendido**.
 
 
## Tarea 5: Crear una Eventhouse

1. Haga clic en el cuadro **+ Nuevo** para abrir el nuevo panel con todos los elementos que puede crear en esta área de trabajo de Fabric.

2. Seleccione el botón **Casa de eventos** desde la sección **Almacenar datos** en el panel. Como hemos comentado, esto puede entenderse de forma similar a un almacén de lago en el que podemos almacenar datos, pero esta Eventhouse se centra en los datos en tiempo real.
 
3. En la ventana que aparece, asigne a su Eventhouse el nombre **eh_Fabrikam** y haga clic en **Crear**.

4. Aquí es donde finalmente transmitirá datos de varios orígenes durante el resto de la formación de hoy. Cuando se cree el elemento, aparecerá una ventana que le dará algunos detalles sobre la Eventhouse. Haga clic en el botón **Get started**.

5. Realice un recorrido rápido por el Eventhouse siguiendo la información sobre herramientas verde en su pantalla. El primero muestra que se creó una base de datos de lenguaje de consulta Kusto (KQL) vacía con Eventhouse.
 
6. Siga lo indicado en el resto de la información sobre herramientas en la pantalla para mostrarle dónde crear bases de datos adicionales, comprobar el almacenamiento en OneLake de la Eventhouse, comprobar el uso de los recursos de Fabric en minutos de proceso y, finalmente, ver qué acciones se han producido en la Eventhouse.
 
7. Dentro del panel de navegación a la izquierda de la Eventhouse, busque la base de datos KQL que se creó junto con la Eventhouse y bastará con hacer clic en ella para ver los detalles de la base de datos.

8. De este modo, seguiremos teniendo una pestaña en el navegador de la izquierda para ver la descripción general de toda la Eventhouse y una nueva pestaña dedicada a las propiedades de la base de datos KQL. Un objetivo que deseamos lograr en nuestro caso es garantizar que se pueda acceder a los datos transmitidos a la base de datos KQL a través de OneLake. Al habilitar esta característica, hacemos que los datos de esta base de datos KQL se puedan detectar con facilidad a través de accesos directos para utilizarlos en cualquier almacén de lago que deseemos. Busque la sección **Detalles de la base de datos** a la derecha y **active** la opción “Disponibilidad”.

9. Vuelva al área de trabajo **RTI_username**; para ello, selecciónelo a la izquierda del explorador. 

10.	Si ve que la opción **Flujos de tareas** ocupa la mayor parte del espacio, seleccione la flecha arriba doble en el lado derecho para minimizarla.

11.	Ahora sabe los aspectos básicos sobre cómo comenzar a ingerir los datos de streaming en OneLake. El siguiente paso es crear un flujo de datos que pueda recibir los datos en movimiento.

En este laboratorio, exploramos la interfaz de Real-Time Intelligence, examinamos el Centro en tiempo real, creamos un espacio de trabajo de Fabric y una Eventhouse que venía con una base de datos KQL. En el próximo laboratorio, comenzará a explorar técnicas que ingieren datos de varios orígenes de todo su estado de datos en OneLake y realizará un análisis básico con el lenguaje de consulta Kusto (KQL).
 
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
