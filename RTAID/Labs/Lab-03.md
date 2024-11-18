# Microsoft Fabric - Real-Time Intelligence in a Day

![](../media/Lab-03/lab-03.png)

## Contents

- Document Structure
- Introduction
  
- Fabric Eventstreams 
  - Task 1: Create an Eventstream
  - Task 2: Transform the Eventstream
  - Task 3: Split Eventstream and Load Two Destinations

- Adding More Data to KQL Database
  - Task 4: Validate Event Data Tables
  - Task 5: Create KQL Database Shortcuts for Dimension tables

- Summary 
- References 

# Document Structure 

The lab includes steps for the user to follow along with associated screenshots that provide visual aid. In each screenshot, sections are highlighted with orange boxes to indicate the area(s) user should focus on.

# Introduction 

In this lab, you will create another Eventstream to ingest additional data into our existing Eventhouse. We will see how to include transformations within the Eventstream to control what data we want to
be adding to the KQL database By the end of this lab, you will have learned:

-   Eventstream Processing and Transformation

-   Write KQL Queries to Join Data from an external Database

-   Use KQL to query data and view within Power BI

# Fabric Eventstreams

## Task 1: Create an Eventstream 

1. Open the **Fabric workspace** you have been using for the course today.

    ![A screenshot of a computer](../media/Lab-03/image5.png)

2. There is additional streaming data for us to Ingest related to our e-commerce store. For this Eventstream though we will want to transform the data prior to loading it into the Eventhouse. Instead of going to the **"Real-Time"** we can create a new Eventstream directly from the workspace. Click on **+ New Item** from the workspace page menu, search for 'Eventstream,' and create a new **Eventstream**.

    ![A screenshot of a computer](../media/Lab-03/image6.png)

3.  Give the new Eventstream the name, **es_Fabrikam_ClickEvents**, Check the **"Enhanced Capabilities"** option, and then click on **Create**.

    ![A screenshot of a computer](../media/Lab-03/image7.png)

4.  Under the Home ribbon, click on the **Add source** dropdown and then select **External sources**.

    ![A screenshot of a computer Description automatically generated](../media/Lab-03/image8-1.png)

5. Similarly to the previous lab we will be connecting to an Azure event Hub which has data being streamed to from a python notebook. Select the **"Azure Event Hubs"** tile and click on **connect**.

   ![A screenshot of a web page Description automatically generated](../media/Lab-03/eventhubs.png)

 >**Note**:If you do not see “Azure Event Hubs” in the recommended section, then select “View all source” to discover it.

6.  Create a **New connection**.

    ![A screenshot of a computer](../media/Lab-03/image10.png)

7. From your environment details page, copy and paste all the necessary connection settings into the appropriate fields.

   | Setting | Value | 
   | --- | --- |
   | Event Hub namespace | rtiadhub<inject key="DeploymentID" enableCopy="true"/> |
   | Event Hub | rti-iad-clicks |
   | Shared Access Key Name | rti-reader |
   | Shared Access Key | <inject key="rti-iad-clicks Primary Key" enableCopy="true"/> |

   ![A screenshot of a computer Description automatically generated](../media/Lab-03/clicks.png)

9. Once all the properties have been filled out click on **Connect**.

10. In the configuration of the Azure Event Hub data source, you may need to modify the **Consumer group** of the Event Hub to ensure that you gain access to a unique access point to the stream of data. For this workshop you can leave the "\$Default" value as shown below

    ![A screenshot of a computer](../media/Lab-03/image12.png)

11. Click on **Next**.

12. On the review and create window, verify that everything is configured correctly and click **Add**.

    ![A screenshot of a computer Description automatically generated](../media/Lab-03/image13.png)

13. Once the stream is configured, you will be able to see a preview of the data coming from the Event Hub.

    ![A table of numbers and numbers Description automatically generated](../media/Lab-03/image14-1.png)

14. Examine the data being received. There are two types of events that are logged from the e-commerce website, click and impressions.

-   **IMPRESSION** - An impression event is recorded each time an advertisement or a product listing is displayed to a user.Impressions are a measure of how many times an item (ad or product) is viewed, regardless of whether it is interacted with.

-   **CLICK** - A click event is recorded when a user interacts with an item by clicking on it. This usually indicates a higher level of
    engagement compared to an impression.

In addition to the clicks and impression events that are logged there are details about what product the click or impression was for, what
device and browser the webpage was loaded from, and what IP address accessed the page and how long the page took to load.

## Task 2: Transform the Eventstream

1.  You will now take this stream of data and transform it to fit into your KQL Database in a way that can be easily understood for
    analysts looking to derive insights from this data. Within the Eventstream canvas, click on the dropdown for the **Transform events** o 
    object.

     ![A screenshot of a chat](../media/Lab-03/image15.png)

2.  From the list of available operations, select the **Manage fields** option.

    ![A screenshot of a search box Description automatically generated](../media/Lab-03/image16-1.png)

3.  On the new icon that appears called **Managefields** click on the **pencil icon** to select what fields you wish to add to your stream
    from the source.

    ![A screen shot of a computer Description automatically generated](../media/Lab-03/manage-1.png)

4.  In the flyout pane that appears, click on the button the option to **Add all fields**.

    ![A screenshot of a computer Description automatically generated](../media/Lab-03/manage-2.png)

5.  From the list of fields, select the one called **PartitionId** and click on the ellipses (...) that appears when you hover over the
    field

    ![A screenshot of a computer Description automatically generated](../media/Lab-03/manage-3.png)

6.  Choose the option to **Remove** that field. For this stream of data coming from the Event Hub, partitioning is not being used so this
    column is not helpful to us, thus we are removing it.

    ![A screenshot of a computer Description automatically generated](../media/Lab-03/image20-1.png)

7.  Remove all the following fields that will not be needed for this stream.

      -   userAgent

      -   page_loading_seconds

      -   EventProcessedUtcTime

      -   EventEnqueredUtcTime

    You should be left with the following fields in the image below.

    ![A screenshot of a computer](../media/Lab-03/manage-4.png)

8. Hover over the eventDate field and when an ellipses (...) appears on the right-hand side of the window, click it.

   ![A screenshot of a computer Description automatically generated](../media/Lab-03/image22-1.png)

9. Choose the option, **Edit**.

   ![A screen shot of a computer Description automatically generated](../media/Lab-03/image23-1.png)

10. Click on the **Change type toggle** to modify the data type of this field. The original type is a String, you need to modify the
    **Converted Type** to **DateTime**. After you have finished click on **Save**.

    ![A screenshot of a computer Description automatically generated](../media/Lab-03/image24-1.png)

## Task 3: Split Eventstream and Load Two Destinations

1.  While you could load this stream of data to a KQL Database for analysis, you may want to have another way to consume this data to
    differentiate CLICK events and IMPRESSION events. Add another transformation activity to the user interface by hovering over the
    end of the **ManageFields** transform

    ![A screen shot of a computer screen Description automatically generated](../media/Lab-03/manage-add.png)

2.  Choose the **Filter** transform from the available list of operations.

    ![A screenshot of a computer Description automatically generated](../media/Lab-03/manage-add-1.png)

3.  Click on the **pencil icon** on the new transformation, **Filter1**.

    ![A screen shot of a computer Description automatically generated](../media/Lab-03/image27-1.png)

4.  In the flyout that appears on the right-hand side of the screen, customize the filter conditions to reflect a way to only return CLICK values by using the settings below. It is important to note that the Filter transform is case sensitive

      -   **Operation name** - Clicks
    
      -   **Select a field to filter on** - eventType
    
      -   **Keep events when the value** - equals -- CLICK **(Important! This is a case sensitive field, ensure to input in all capitals for this example)**

          ![](../media/Lab-03/image28-1.png)

5.  Choose the **Save** option to keep your changes.

6.  Click on the **Refresh button** again to verify the data has been filtered to CLICK eventTypes.

    ![](../media/Lab-03/image29-1.png)

7.  These may be the only rows that you're interested in sending to a table but another option is to instead create two separate streams
    to route different information to two or more tables. From the **Home** ribbon of the Eventstream click on the **Transform events**
    dropdown and then select **Filter**.

    ![](../media/Lab-03/image30-1.png)

8.  A new object called **Filter1 (Name may differ)** will appear on your canvas. You will need to connect the **Managefields stream**
    to the new filter transformation. Drag a line from the green dot on one transform to another to make that connection.

    ![A screenshot of a computer](../media/Lab-03/manage-final.png)
   
9.  Click on the **pencil icon** for **Filter2** to edit its settings.

    ![A screenshot of a computer Description automatically generated](../media/Lab-03/image32.png)

10. In the flyout that appears on the right-hand side of the screen, customize the filter conditions to reflect a way to only return
    IMPRESSION values by using the values below. Remember that the Filter transform is case sensitive

      -   **Operation name** - Impressions
    
      -   **Select a field to filter on** - eventType
    
      -   **Keep events when the value** - equals -- IMPRESSION **(Important!
        This is a case sensitive field, ensure to input in all capitals for
        this example)**

    ![](../media/Lab-03/image33-1.png)

11. Choose the **Save** option to keep your changes.

12. Before we load the data into new tables within our KQL Database we can remove additional columns which are not needed. In this case for
    the stream of data which is filtered for our **CLICK** records we no longer need the "eventType" column since every row holds the same
    value. For our "IMPRESSION" stream of data we can remove the "eventType" column for the same reasons mentioned above and we can
    also remove the "referrer" column as it is empty for every row in this table.

13. Click on the **+ icon** after the **Clicks** filter operation.

    ![A white board with writing on it Description automatically generated](../media/Lab-03/image34-1.png)

14. In the dropdown menu select **Manage fields**.

    ![](../media/Lab-03/image35.png)

15. Click on the **pencil icon** to select what fields you wish to add/remove to your stream
    
    ![A white rectangular sign with black text Description automatically generated](../media/Lab-03/image36.png)

16. Rename the operation to **Manage_Clicks**. As well select **Add all
    fields** then remove **eventType**. Once done click **Save**.

    ![A screenshot of a computer](../media/Lab-03/image37.png)

17. Next, let's add another **Manage fields** transform connected to the
    **Impressions** filter. 

    ![A rectangle with a rectangle with](../media/Lab-03/image38.png)

18. Click on the **pencil icon** to select what fields you wish to add/remove to your stream.
    
    ![A white card with a pen and a red box Description automatically generated](../media/Lab-03/image39.png)

19. Rename the operation to **Manage_Impressions**. Then select **Add all fields** then remove **eventType** and **referrer**. Your **Manage fields**
    transform should look like the following:

    ![A screenshot of a computer](../media/Lab-03/image40.png)

20. Now that you have cleaned up the data for the streams for each of the types of events, you need to load each stream into a new table
    on the KQL Database. Click on the **+ icon** after the **Manage_Clicks** manage fields operation.

    ![A screen shot of a computer Description automatically generated](../media/Lab-03/image41.png)

21. In the dropdown list that appears, go to the Destinations and select **Eventhouse**.

    ![A screenshot of a computer Description automatically generated](../media/Lab-03/image42.png)

22. Click on the **pencil icon** for the Eventhouse destination.

    ![A white sign with black text Description automatically generated](../media/Lab-03/image43.png)

23. Configure the following properties for this destination and select the details from the dropdown.
      -   **Destination name** -- dbo-Clicks
      -   **Workspace** - RTI_<inject key="DeploymentID" enableCopy="copy"/>
      -   **Eventhouse** -- eh_Fabrikam
      -   **KQL Database** - eh_Fabrikam
      -   **Destination Table** - Create a new table called **Clicks**

    ![A screenshot of a computer Description automatically generated](../media/Lab-03/image44.png)

24. Click on **Save** at the bottom of the flyout.

25. Configure the following properties for this destination and select the details from the dropdown.

      - **Destination name** -- dbo-Impressions
      - **Workspace** - RTI_<inject key="DeploymentID" enableCopy="copy"/>
      - **Eventhouse** -- eh_Fabrikam
      - **KQL Database** - eh_Fabrikam
      - **Destination Table** - Create a new table called **Impressions**

    ![A screenshot of a computer](../media/Lab-03/image45.png)

27. Save your changes.

28. This Eventstream is now ready to begin streaming. Click on **Publish** to begin that stream.

    ![A screenshot of a computer Description automatically generated](../media/Lab-03/publish-1.png)

29. With the Eventstream now running, you should see the Eventstream user interface slightly change to signify that you are streaming the data from Event Hub transforming and splitting that data stream and loading it into two separate KQL Database tables.

    ![A screenshot of a computer](../media/Lab-03/image47-1.png)

# Adding More Data to KQL Database

## Task 4: Validate Event Data Tables

1.  Return to your **RTI_<inject key="DeploymentID" enableCopy="false"/>** workspace.

2.  Open the **eh_Fabrikam** KQL Database.

    ![A screenshot of a computer Description automatically generated](../media/Lab-03/image48.png)

3.  With the Eventstream running you should now see two new tables on the KQL Database Overview page. After letting the Eventstream run for several hours you will see that the **Top tables** within the  KQL Database will be displayed on the Overview page and show how much data is stored within the table.

    ![](../media/Lab-03/image49.png)

4.  Click on the **Impressions** table. This table receives about 1.5 million records every 24 hours. There are many more impressions than Clicks so this will be your largest table for the purposes of this class.

    ![A screenshot of a computer Description automatically generated](../media/Lab-03/image50.png)

## Task 5: Create KQL Database Shortcuts for Dimension tables

Up to this point you have been working with streaming data, but are  still missing some critical elements to be able to derive intelligence
from the data you have brought in. In this task we will bring in data from an external Azure SQL Database which will serve as dimension tables
within our KQL Database. This will allow us to better describe the data we are streaming currently. For example, all our tables contain a form
of Product ID which is a numeric field, but it would be better if we had some sort of Product name to be able to display. The data we need to
support this is currently in an external Azure SQL Database, let's see how easy it is to make connections to some of these dimension tables.

1.  In the **eh_Fabrikam** database, click on the drop-down menu called **New related item**. Then choose the option that says KQL Queryset.

    ![A screenshot of a computer](../media/Lab-03/image51.png)

2.  Give the KQL Queryset the name **Create Tables** and then click the **Create** button.

    ![A screenshot of a computer Description automatically generated](../media/Lab-03/image52-1.png)

3.  The OneLake data hub will open and the only option to select with be the **"eh_Fabrikam"** kql database. Select this database and click **"Connect"**.

    ![A screenshot of a chat](../media/Lab-03/image53.png)

4.  In the new interface click once within the query window and highlight all the text by using the keyboard shortcut **Ctrl + A**.
    Once everything has been highlighted, delete everything.

     ![A screenshot of a computer Description automatically generated](../media/Lab-03/image54-1.png)

5.  In the blank query window enter the following KQL script. This script will create a connection to an external Azure SQL Database
    and make it available within our KQL database as a **Shortcut**. A **Shortcut** is attached in a read-only mode, making it possible to
    view and run queries alongside the streaming data that was ingested into the KQL database.

      ```
    .execute database script <|
    //External tables - shortcuts
    // connect to operational Database with external table Product
    .create external table products (ProductID: int, ProductNumber: string,  Name: string) 
    kind=sql
    table=[SalesLT.Product]
    ( 
    h@'Server= fabrikamdemo.database.windows.net,1433;Initial Catalog=fabrikamdb;User Id=demouser;Password=fabrikam@123456'
    )
    with 
    (
    createifnotexists = true
    )  
    // connect to operational Database with external table ProductCategory
    .create external table productCategories (ProductCategoryID: int, Name: string) 
    kind=sql
    table=[SalesLT.ProductCategory]
    ( 
     h@'Server= fabrikamdemo.database.windows.net,1433;Initial Catalog=fabrikamdb;User Id=demouser;Password=fabrikam@123456'    )
    with 
    (
    createifnotexists = true
    )

      ```

    ![A screenshot of a computer Description automatically generated](../media/Lab-03/P4T5S5.png)

6.  Click the **Run** button to execute the script.

    ![A screenshot of a computer Description automatically generated](../media/Lab-03/image57-1.png)

7.  In your Database Explorer window, you will now see a new folder called **Shortcuts** and within the folder you should see two
    additional tables that are linked to this KQL Database. These tables exist within an Azure SQL Database, but through the script you
    executed, you now have them linked to this KQL Database to be joined with your InternetSales and event tables.

    ![A screenshot of a search box](../media/Lab-03/image58.png)

8. Now that you have dimensional qualities to your database, you can answer questions and give more context to consumers of the reports
    and queries these tables off insights on across your business. Run the following KQL query to see one of them.

      ```
    InternetSales
    | join kind=inner 
    (external_table("products")) on ($left.ProductKey == $right.ProductID)
    | summarize SalesPerProduct=sum(SalesAmount) by Name
    | project Name, SalesPerProduct
    
      ```

9.  You will now see in your query results values for each individual product that your company has sold.

     ![A screenshot of a computer](../media/Lab-03/image60.png)

10. With your query highlighted click on the button in your toolbar, **Build Power BI report**.

    ![A screenshot of a computer Description automatically generated](../media/Lab-03/image61.png)

11. This gives you the opportunity to create a Power BI report using the data within your KQL Database. Feel free to explore this for a few
    moments, but you will not need to create a report from this data just yet. Click the **X button** in the top-right corner when you
    are ready to move forward.

    ![A screenshot of a computer Description automatically generated](../media/Lab-03/image62-1.png)

12. Navigate back to the **eh_Fabrikam** KQL Database.

    ![A screenshot of a computer](../media/Lab-03/blur.png)

13. Click on the **Shortcuts** option within the **eh_Fabrikam** navigation pane. This will show you all the shortcuts you havecreated to this KQL Database. It should be noted that these Shortcuts are considered classical Azure Data Explorer externaltables using Azure SQL external table syntax and are constructed differently than OneLake, ADLS, or S3 shortcuts which are also supported in KQL Database within Fabric.

    ![A screenshot of a computer Description automatically generated](../media/Lab-03/image64.png)

# Summary

In this lab, you created another stream of data but were able to transform the stream using the user interface of the Eventstream in
Fabric. Loading the data to two separate tables has allowed you to track all the clicks and impressions within your e-commerce system for
marketing, advertising, and analysis purposes. You also created a shortcut to an external Azure SQL Database using the KQL Queryset
external table feature. You now have a few dimensions to better understand the context of the sales and clicks within your KQL Database.

# References 

Fabric Real-Time Intelligence in a Day (RTIIAD) introduces you to some of the key functions available in Microsoft Fabric.
In the menu of the service, the Help (?) section has links to some great resources.

 ![A screenshot of a computer](../media/Lab-03/refrence-1.png)
 
Here are a few more resources that will help you with your next steps with Microsoft Fabric.
-   See blog post to read the full [Microsoft Fabric GA announcement](https://aka.ms/Fabric-Hero-Blog-Ignite23)
-   Explore Fabric through the [Guided tour](https://aka.ms/Fabric-GuidedTour)
-   Sign up for the [Microsoft Fabric free trial](https://aka.ms/try-fabric)
-   Visit the [Microsoft Fabric website](https://aka.ms/microsoft-fabric)
-   Learn new skills by exploring the [Fabric Learning  modules](https://aka.ms/learn-fabric)
-   Explore the [Fabric technical documentation](https://aka.ms/fabric-docs)
-   Read the [free e-book on getting started with fabric](https://aka.ms/fabric-get-started-ebook)
-   Join the [Fabric  community](https://aka.ms/fabric-community) to post  your questions, share your feedback, and learn from others

Read the more in-depth Fabric experience announcement blogs:

-   [Data Factory experience in Fabric blog](https://aka.ms/Fabric-Data-Factory-Blog)
-   [Synapse Data Engineering experience in Fabric  blog](https://aka.ms/Fabric-DE-Blog)
-   [Synapse Data Science experience in Fabric  blog](https://aka.ms/Fabric-DS-Blog)
-   [Synapse Data Warehousing experience in Fabric  blog](https://aka.ms/Fabric-DW-Blog)
-   [Real-](https://aka.ms/Fabric-RTA-Blog)[Time Intelligence  experience in Fabric blog](https://blog.fabric.microsoft.com/en-us/blog/category/real-time-intelligence)
-   [Power BI announcement blog](https://aka.ms/Fabric-PBI-Blog)
-   [Data Activator experience in Fabric  blog](https://aka.ms/Fabric-DA-Blog)
-   [Administration and governance in Fabric  blog](https://aka.ms/Fabric-Admin-Gov-Blog)
-   [OneLake][in Fabric  blog](https://aka.ms/Fabric-OneLake-Blog)
-   [Dataverse and Microsoft Fabric integration  blog](https://aka.ms/Dataverse-Fabric-Blog)

© 2024 Microsoft Corporation. All rights reserved.

By using this demo/lab, you agree to the following terms:

The technology/functionality described in this demo/lab is provided by Microsoft Corporation for purposes of obtaining your feedback and to
provide you with a learning experience. You may only use the demo/lab to evaluate such technology features and functionality and provide feedback to Microsoft. You may not use it for any other purpose. You may not modify, copy, distribute, transmit, display, perform,reproduce, publish, license, create derivative works from, transfer,or sell this demo/lab or any portion thereof.

COPYING OR REPRODUCTION OF THE DEMO/LAB (OR ANY PORTION OF IT) TO ANY OTHER SERVER OR LOCATION FOR FURTHER REPRODUCTION OR REDISTRIBUTION IS EXPRESSLY PROHIBITED.

THIS DEMO/LAB PROVIDES CERTAIN SOFTWARE TECHNOLOGY/PRODUCT FEATURES AND FUNCTIONALITY, INCLUDING POTENTIAL NEW FEATURES AND CONCEPTS, IN A SIMULATED ENVIRONMENT WITHOUT COMPLEX SET-UP OR INSTALLATION FOR THE PURPOSE DESCRIBED ABOVE. THE TECHNOLOGY/CONCEPTS EPRESENTED IN THIS DEMO/LAB MAY NOT REPRESENT FULL FEATURE FUNCTIONALITY AND MAY NOT WORK THE WAY A FINAL VERSION MAY WORK. WE ALSO MAY NOT RELEASE A FINAL VERSION OF SUCH FEATURES OR CONCEPTS. YOUR EXPERIENCE WITH USING SUCH FEATURES AND FUNCITONALITY IN A PHYSICALENVIRONMENT MAY ALSO BE DIFFERENT.

**FEEDBACK**. If you give feedback about the technology features,functionality and/or concepts described in this demo/lab to Microsoft,you give to Microsoft, without charge, the right to use, share and commercialize your feedback in any way and for any purpose. You also give to third parties, without charge, any patent rights needed for their products, technologies and services to use or interface with any specific parts of a Microsoft software or service that includes the feedback. You will not give feedback that is subject to a license that requires Microsoft to license its software or documentation to third parties because we include your feedback in them. These rights survive this agreement.

MICROSOFT CORPORATION HEREBY DISCLAIMS ALL WARRANTIES AND CONDITIONS WITH REGARD TO THE DEMO/LAB, INCLUDING ALL WARRANTIES AND CONDITIONS OF MERCHANTABILITY,WHETHER EXPRESS, IMPLIED OR STATUTORY, FITNESS FOR A PARTICULAR PURPOSE, TITLE AND NON-INFRINGEMENT. MICROSOFT DOES NOT MAKE ANY ASSURANCES OR REPRESENTATIONS WITH REGARD TO THE ACCURACY OF THE RESULTS, OUTPUT THAT DERIVES FROM USE OF DEMO/ LAB, OR SUITABILITY OF THE INFORMATION
CONTAINED IN THE DEMO/LAB FOR ANY PURPOSE.

**DISCLAIMER**
This demo/lab contains only a portion of new features and enhancements in Microsoft Power BI. Some of the features might change in future releases of the product. In this demo/lab, you will learn about some,but not all, new features.
