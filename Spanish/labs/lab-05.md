# Microsoft Fabric Real-Time Intelligence in a Day Laboratorio 5
 
 ![](../media/lab-05/spanish.png)

# Contenido
- Estructura del documento	
- Introducción	
- Paneles de información en tiempo real	
  - Tarea 1: Crear un panel de información en tiempo real	
  - Tarea 2: Conectar un origen de datos al panel de información en tiempo real	
  - Tarea 3: Crear un icono del panel de información en tiempo real con KQL	
  - Tarea 4: Agregar más iconos de panel información al panel de información en tiempo real	
  - Tarea 5: Agregar un objeto visual de mapa para Impressions by Location	
  - Tarea 6: Configurar la actualización automática en el panel de información en tiempo real	
  - Tarea opcional 7: Agregar el logotipo de la empresa	
  - Tarea opcional 8: Aplicar formato condicional a un objeto visual	
- Resumen	
- Referencias
 
# Estructura del documento

El laboratorio incluye pasos que el usuario debe seguir junto con capturas de pantalla asociadas que sirven de ayuda visual. En cada captura de pantalla, las secciones se resaltan con cuadros de color naranja para indicar en qué áreas debe centrarse el usuario.

# Introducción

En este laboratorio, utilizará los datos que ha transmitido y cargado en su base de datos KQL y que ha vinculado de forma concisa a un almacén de lago mediante accesos directos para crear un panel de información en tiempo real a fin de visualizar y compartir su información de los flujos de datos a los que accedió.

Al final de este laboratorio, habrá aprendido:

- Crear un panel de información en tiempo real en Fabric.
- Usar KQL para escribir consultas para rellenar objetos visuales de un panel.
- Agregar formato condicional a objetos visuales del panel de información.

# Paneles de información en tiempo real

## Tarea 1: Crear un panel de información en tiempo real

1. Abra el **área de trabajo de Fabric** para el curso.

   ![](../media/lab-05/image003.jpg)

2. Haga clic en el botón **+ Nuevo elemento** para crear un elemento nuevo.

   ![](../media/lab-05/image005.png)
 
3. Verá una categoría para **Visualizar datos**. Haga clic en el elemento denominado **Panel en tiempo real**

   ![](../media/lab-05/image007.jpg)

4. Asigne a su panel de control en tiempo real el nombre **RTI Dashboard** y luego haga clic en **Crear**.
   
   ![](../media/lab-05/image010.png)

6. Debería pasar inmediatamente a una instancia en blanco de un panel de información en tiempo real.

   ![](../media/lab-05/image013.png)
 
## Tarea 2: Conectar un origen de datos al panel de información en tiempo real

1. En la cinta de opciones de Inicio, busque la opción denominada **New data source** y haga clic en ella.

   ![](../media/lab-05/image016.png)

2. En el panel flotante que aparece en el lado derecho de la pantalla, haga clic en **Agregar + y** luego elija **Centro de datos de OneLake**.

   ![](../media/lab-05/image019.jpg)

3. Aparecerá una lista de orígenes disponibles en OneLake, solo se mostrarán los orígenes de las bases de datos KQL, por lo que habrá una opción disponible, la base de datos KQL **eh_Fabrikam**. Seleccione esa opción.

   ![](../media/lab-05/image021.jpg)

4. Haga clic en **Conectar** en la parte inferior de la pantalla.

   ![](../media/lab-05/image024.png)
 
5. Ahora ya podrá crear el origen de datos. Haga clic en el botón **Agregar** situado en la parte inferior del panel flotante.

   ![](../media/lab-05/image026.jpg)

6. Ahora verá que se ha agregado un origen de datos al panel de información en tiempo real. Desde aquí, puede agregar bases de datos KQL adicionales en caso de que surja la necesidad. Por ahora, haga clic en **Cerrar** en la parte inferior de la ventana.

   ![](../media/lab-05/image028.jpg)
 
## Tarea 3: Crear un icono del panel de información en tiempo real con KQL

1. Haga clic en el icono en blanco dentro del panel de información para rellenar el icono con un objeto visual.

   ![](../media/lab-05/image031.jpg)

2. De forma predeterminada, se conectará a la base de datos KQL que creó anteriormente como origen. Desde aquí, puede escribir su propia consulta KQL para rellenar este objeto visual con datos. Elimine todo el KQL de Markdown anterior que esté ahí de forma predeterminada. Copie y pegue la siguiente consulta en la ventana de la consulta.
  
    ```
    //Clicks by hour Clicks
    | where eventDate between (_startTime.._endTime)
    | summarize date_count = count() by bin(eventDate, 1h)
    | render timechart
    | top 30 by date_count
    | sort by eventDate
    ```

3. Ejecute la consulta una vez que la haya configurado correctamente para ver los resultados.

   ![](../media/lab-05/image033.png)

4. Tenga en cuenta que es posible que solo tenga un resultado en la salida. Esto se debe al **Intervalo de tiempo** que se establece de forma predeterminada para este icono. Dispone de un parámetro con el que puede modificar el intervalo de tiempo desde el que devuelve los datos. La eventDate entre (_startTime.._endTime) es lo que le permite aprovechar este parámetro. Modifique el parámetro **Intervalo de tiempo** a **Últimos 3 horas** y observe cómo cambia la salida.

   ![](../media/lab-05/image036.png)

5. En la salida de la consulta ahora debería ver los resultados de los clics de las últimas 3 horas.

   ![](../media/lab-05/image039.png)

6. Aunque este parámetro se puede modificar, es posible que desee que tenga como valor predeterminado un intervalo de tiempo específico en lugar de obligar a los usuarios a modificarlo. Encima de la opción de intervalo de tiempo, haga clic en la opción **@ Parámetros**.

   ![](../media/lab-05/image041.png)
 
7. Haga clic en el **icono de lápiz** para editar el parámetro **Intervalo de tiempo**.

   ![](../media/lab-05/image043.jpg)

8. Cambie el **Valor predeterminado** a **Últimos 24 horas** para mostrar siempre el último día de forma predeterminada. Haga clic en **Listo** cuando haya terminado.

   ![](../media/lab-05/image045.png)

9. Cierre el panel de parámetros.
 
10. Ahora haga clic en el **botón + Agregar objeto visual** encima de los resultados de la consulta.

    ![](../media/lab-05/image048.png)

11. Aparecerá un nuevo control flotante en el lado derecho de la pantalla. Haga clic en el cuadro de texto debajo de la opción **Tile name** para asignar a este objeto visual el nombre **Clicks by Hour**.

    ![](../media/lab-05/image051.png)

12. De forma predeterminada, el objeto visual que está usando para mostrar los resultados de esta consulta KQL es una tabla. Es posible que esta no sea la mejor manera para que alguien consuma y comprenda rápidamente lo que está sucediendo con los resultados de sus datos. Cambie el tipo de objeto visual de tabla a **Area chart**.

    ![](../media/lab-05/image054.png)

13. Con este objeto visual con nuevo formato, puede comprender mejor los picos y valles de clics de su sitio de comercio electrónico mediante el flujo de datos que creó anteriormente en esta clase.

    ![](../media/lab-05/image057.jpg)

14. Para guardar este objeto visual en el panel de información, haga clic en el botón **Aplicar cambios** en la esquina superior derecha de la pantalla.

    ![](../media/lab-05/image060.png)
 
15. Una vez que este objeto visual se haya colocado dentro del panel de información, es posible que el objeto visual solo muestre la última hora de resultados. Modifique el panel de información para mostrar el **Intervalo de tiempo** de las **Últimos 24 horas**.

    ![](../media/lab-05/image062.png)

16. Actualice el objeto visual y observe que los resultados cambiarán ligeramente para reflejar los datos que se han recibido desde la última ejecución de la consulta.

    ![](../media/lab-05/image065.png)

## Tarea 4: Agregar más iconos de panel información al panel de información en tiempo real

1. Haga clic en el botón **New tile** desde la **cinta de opciones de Inicio** en el panel de información en tiempo real.

   ![](../media/lab-05/image068.jpg)

2. Introduzca la siguiente consulta KQL en el panel de información:

    ```
    //Impressions by hour Impressions
    | where eventDate between (_startTime.._endTime)
    | summarize date_count = count() by bin(eventDate, 1h)
    | render timechart
    | top 30 by date_count
    | sort by eventDate
    ```

3. **Ejecute** la consulta.

    ![](../media/lab-05/image070.png)

4. Haga clic en el botón **+ Agregar** objeto visual.

   ![](../media/lab-05/image073.png)
 
5. Edite el objeto visual para cambiar el **Tile name** a **Impressions by Hour** y el **Visual type** a **Area chart**.

   ![](../media/lab-05/image076.png)

6. Aplique cambios al objeto visual.

   ![](../media/lab-05/image079.png)

7. Agregue otro **+ New tile**.

   ![](../media/lab-05/image082.jpg)

8. Copie y pegue la siguiente consulta en el panel de la consulta. Tenga en cuenta que se trata de una consulta de varias instrucciones que utiliza varias instrucciones let y una consulta combinada con punto y coma.

   ```
   //Clicks, Impressions, CTR

   let imp = Impressions
   | where eventDate between (_startTime.._endTime)
   | extend dateOnly = substring(todatetime(eventDate).tostring(), 0, 10)
   | summarize imp_count = count() by dateOnly;

   let clck = Clicks
   | where eventDate between (_startTime.._endTime)
   | extend dateOnly = substring(todatetime(eventDate).tostring(), 0, 10)
   | summarize clck_count = count() by dateOnly;

   imp
   | join clck on $left.dateOnly == $right.dateOnly
   | project selected_date = dateOnly , impressions = imp_count , clicks = clck_count, CTR = clck_count * 100 / imp_count
   ```

9. **Ejecute** la consulta para ver los resultados.

    ![](../media/lab-05/image084.jpg)
 
10. Haga clic en el botón **+ Agregar objeto visual**.

11. Cuando aparezca la configuración del objeto visual, modifique la siguiente configuración para crear un recuento de impresiones.
     
     - **Tile name**: Impressions
     
     - **Visual type**: Stat
     
     - **Value column**: impressions (long)

       ![](../media/lab-05/image087.png)

12. Seleccione **Aplicar cambios** cuando todos los ajustes estén configurados correctamente.

    ![](../media/lab-05/image090.png)
 
13. En el nuevo icono, haga clic en los puntos suspensivos (...) y seleccione la opción **Icono duplicado**.

    ![](../media/lab-05/image093.jpg)

14. Haga clic en el **icono de lápiz** para el icono duplicado para editar las configuraciones.

    ![](../media/lab-05/image096.png)

15. Cambie este **Tile name a Clicks** y cambie la **Value column a clicks (long)**.

    ![](../media/lab-05/image099.png)

16. Aplique los cambios a este objeto visual.
 
17. Duplique cualquiera de los nuevos iconos una vez más para crear un objeto visual estadístico final.

    ![](../media/lab-05/image102.png)
 
18. Edite el nuevo icono para cambiar el **Tile name** a **Click Through Rate** y la **Value column a CTR (long)**.

    ![](../media/lab-05/image105.png)

19. Aplique los cambios.

20. Si los iconos están separados o desea reorganizarlos, puede mantener el puntero sobre el icono hasta que aparezca un icono de mano y arrastrar y soltar el objeto visual donde desee.

    ![](../media/lab-05/image108.jpg)

## Tarea 5: Agregar un objeto visual de mapa para Impressions by Location

1. Agregue un **New tile** al panel de información en tiempo real.

   ![](../media/lab-05/image111.jpg)

2. Copie y pegue la siguiente consulta en el panel de la consulta. Esta consulta extrae la latitud y la longitud de la columna de dirección IP de este flujo de datos para generar una ubicación que puede trazar en un mapa. Esta consulta puede tardar un poco más que las anteriores.
 
    ```
    //Impressions by location
    
    Impressions
    | where eventDate between (_startTime.._endTime)
    | join external_table('products') on $left.productId == $right.ProductID
    | project lon = toreal(geo_info_from_ip_address(ip_address).longitude), lat = toreal(geo_info_from_ip_address(ip_address).latitude), Name
    | render scatterchart with (kind = map) //, xcolumn=lon, ycolumns=lat)
    ```

3. Ejecute la consulta para validar que esté configurada correctamente. Haga clic en el botón **+ Agregar objeto visual**.

   ![](../media/lab-05/image114.png)

4. Cambie el **Tile name** a **Impressions by Location** y el **Visual type** a **Map**.

   ![](../media/lab-05/image117.png)
 
5. En el área **Visual type**, asegúrese de que la latitud y la longitud estén seleccionadas correctamente modificando la opción **Definir ubicación por a Latitud y Longitud** y compruebe que los campos restantes coincidan con la siguiente imagen.

   ![](../media/lab-05/image120.png)

6. Aplique los cambios.

7. Agarre el punto de anclaje de la parte inferior izquierda del objeto visual de mapa dentro del panel de información para aumentar el tamaño del objeto visual.

   ![](../media/lab-05/image123.jpg)
 
8. Todos los objetos visuales son redimensionables y móviles. No dude en reorganizar el suyo como desee.

   ![](../media/lab-05/image126.jpg)

9. Guarde los cambios.

   ![](../media/lab-05/image129.png)

## Tarea 6: Configurar la actualización automática en el panel de información en tiempo real

1. Haga clic en la **cinta de opciones Administrar** y luego seleccione la opción **Actualizar automáticamente**.

   ![](../media/lab-05/image132.jpg)

2. Active el botón de alternancia para habilitar **Actualizar automáticamente**.

   ![](../media/lab-05/image135.png)
 
3. Modifique el **intervalo de tiempo mínimo** a 30 segundos y la **Frecuencia de actualización predeterminada** a 1 minuto.

   ![](../media/lab-05/image138.jpg)

4. Haga clic en **Aplicar** en la parte inferior de la ventana.

5. En la esquina superior derecha de su menú, haga clic en el **botón Edición** y modifíquelo a **Visualización** para ver lo que sus usuarios finales experimentarán con este panel de información en tiempo real.

   ![](../media/lab-05/image141.png)

6. Si dispone de tiempo y le interesa recuperar el logotipo de una empresa o aplicar formato condicional a sus objetos visuales, tal y como se muestra a continuación, no dude en realizar las siguientes tareas opcionales. Por lo demás, el laboratorio está completo.

   ![](../media/lab-05/image143.jpg)
 
## Tarea opcional 7: Agregar el logotipo de la empresa

1. Al igual que hicimos antes, cambie del modo **Visualización** del panel de información al modo **Edición**

   ![](../media/lab-05/image145.jpg)

2. Haga clic en el botón de la cinta de opciones de Inicio denominado **New text tile**.

   ![](../media/lab-05/image148.jpg)

3. Copie y pegue el siguiente código Markdown en la ventana de la consulta.
![Fabrikam](https://github.com/PragmaticWorksTraining/DIAD/blob/main/Logos/Fabrikam. png?raw=true "Fabrikam")

   ![](../media/lab-05/image150.jpg)

4. Aplique los cambios.

5. Cambie el tamaño y mueva el icono para que quepa en algún lugar dentro de su panel de información en tiempo real.

   ![](../media/lab-05/image152.jpg)
 
6. Guarde los cambios.

   ![](../media/lab-05/Picture1.png)

## Tarea opcional 8: Aplicar formato condicional a un objeto visual

1. Haga clic en el **icono de lápiz** en el objeto visual **Click Through Rate**.

   ![](../media/lab-05/image156.jpg)

2. En la parte inferior del panel de formato de objeto visual, haga clic en el botón **+ Add rule** debajo de **Formato condicional**.

   ![](../media/lab-05/image159.png)

3. Haga clic en el **icono de lápiz** para editar la regla de formato condicional.

    ![](../media/lab-05/image162.png)

4. Modifique las condiciones de la regla para que apunten a la **Columna** denominada **CTR (long) y** haga la regla **> 10** para el operador y el valor.

   ![](../media/lab-05/image165.png)

5. No dude en modificar el formato como desee. Siempre que el valor del CTR sea mayor que 10, aparecerá en ese objeto visual.

   ![](../media/lab-05/image168.png)

6. Haga clic en el botón **Guardar** dentro del panel de formato condicional.

   ![](../media/lab-05/image170.jpg)

7.	Aplique los cambios.

8.	Guarde los cambios.

      ![](../media/lab-05/Picture1.png)

# Resumen

En este laboratorio, los usuarios crearon un panel de información en tiempo real y lo conectaron a nuestra base de datos KQL. Pudimos ver que usamos el lenguaje KQL para mantener consultas y luego podemos visualizar los resultados de muchas maneras, ya que cada objeto visual tiene su propia configuración. Además, vimos cómo podíamos modificar el parámetro predeterminado disponible en el panel de información y hacer que este se actualizara automáticamente.

## Referencias
Fabric Real-Time Intelligence in a Day (RTIIAD) le presenta algunas funciones clave disponibles en Microsoft Fabric.En el menú del servicio, la sección Ayuda (?) tiene vínculos a algunos recursos excelentes.

![](../media/lab-02/image129.jpg)

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
