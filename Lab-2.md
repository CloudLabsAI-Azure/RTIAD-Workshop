# Microsoft Fabric - Real-Time Intelligence in a Day

![](./media/lab2ms.png)

# Contents  

- Document Structure  .
- Introduction  
- Fabric Eventstreams  
  - Task 1: Create an Eventstream
  - Task 2: Setup Eventstream Destination 
- Kusto Query Language (KQL)  
  - Task 3: Authoring Kusto Database Queries  
  - Task 4: Using T-SQL Queries Against a KQL Database
- KQL Queryset  
  - Task 5: Create a KQL Queryset 
- References

## Document Structure

The lab includes steps for the user to follow along with associated screenshots that provide visual aid. In each screenshot, sections are highlighted with orange boxes to indicate the area(s) user should focus on.

## Introduction 

 In this lab, you will experience a way to handle a continuous stream of real-time data. You will use a Fabric Real-Time Intelligence object
 called an Eventstream to ingest this data to the Eventhouse you created in the last lab and write some basic KQL queries.

 By the end of this lab, you will have learned:

-   How to create an Eventstream

-   Load Real-time data into a KQL Database

-   Write basic Kusto Query Language Queries

##  Fabric Real-Time Hub

### Task 1: Create an Event Stream Source 

1.  Open the **Fabric workspace** you created in the last lab. From here we can see the Eventhouse we created.

    ![A screenshot of a computer](./media/image5.png)

2.  Navigate to the "Real-Time hub" where currently we do not see any treams of data. That will change shortly.

    ![A screenshot of a computer](./media/image6.png)

3.  Select the "Add source" green button which should be in the upper right corner.

    ![A screenshot of a search box Description automatically generated](./media/add-source-button.png)

4.  A window will open that will allow you to select a source for our stream data. As we discussed before, there are many fantastic options to choose from but for this class we will select the option "Azure Event Hubs".
    
    ![A screenshot of a computer Description automatically generated](./media/image8.png)

5.  You are now required to create a connection to the Azure Event Hub. Click on the **New connection** text since you do not currently have a connection.
    
    ![A screenshot of a computer](./media/image9.png)

6.  From your environment details page, copy and paste all the necessary connection settings into the appropriate fields. For these labs we are connecting to an Event Hub which has streaming data being sent from a python notebook. This notebook is creating fake sales transactions at rate of around 3,100 transactions per hour.
    
      Event Hub namespace:
    
      ```
      rtiadhub<inject key="DeploymentID" enableCopy="false"></inject>
      ```
      
     Event Hub:
    
      ```
     rti-iad-fabrikam
      ```
      
     Shared Access Key Name:
    
      ```
      rti-reader
      ```

      Shared Access Key: 
      ```    
      <inject key="rti-iad-fabrikam Primary Key" enableCopy="false"/>
      ```
      
      ```
      rtiadhub<inject key="DeploymentID" enableCopy="false"/>
      ```

    >**Note**: You can also copy the shared access primary key from environment tab 

8.  Once all properties have been filled out click on **Connect**.
    
    ![A screenshot of a computer Description automatically generated](./media/image10.png)

9.  In the configuration of the Azure Event Hub data source, you will need to modify the **Consumer group** of the Event Hub to ensure that you gain access to a unique access point to the stream of data. Within your **Environment details** find the property that lists what your consumer group name will be and place that into the field box. It will appear something like "**$Default**".
    
    ![A screenshot of a computer](./media/choose_1.png)

  >**Note**: Choose "**$Default**" as Consumer group

10.  Before we finalize this data source and Eventstream, let's go ahead and rename our Eventstream to something more useful. In the "Stream details\" section on the right select the pencil icon next to the "Eventstream name" and let's call our Eventstream **es_Fabrikam_InternetSales**"
    
     ![A screenshot of a computer](./media/image12.png)

11. Now we can click on **Next**, which will take us to a final overview page.
    
    ![A screenshot of a computer](./media/image13.png)

>**Note**: The Event Source connection stream may fail with a session timeout error, but you should still see the Event Source listed as created once you refresh .

12. In this overview screen, Verify the contents look correct and click **Connect**.
    
    ![A screenshot of a computer Description automatically generated](./media/connect-to-source2.png)

13. Once the Eventstream and Eventstream source are created select the option "**Open Eventstream**"
    
    ![A screenshot of a computer](./media/image15.png)

15. This will take you to the Eventstream user interface. Here is where you will see able to connect with a source stream of data, bring it
    into this Fabric item, and then stream it to a new destination like a Lakehouse or KQL Database.

16. It may take a few moments for your Source to be **Active** but after waiting a few moments, click on the middle icon with the name of
    your Eventstream on it and then click on **Refresh** if you do not see a preview of the data.
   
    >**Note: If you receive a "Warning" status around and audit policy, that is fine. The stream will still function**

    ![A screenshot of a computer Description automatically generated](./media/image16.png)

17. You should now see a sample of the data within the bottom window.
    
    ![A screenshot of a computer](./media/image17.png)

18. This will show you a preview of the data that is being received from the Azure Event Hub. If you slide your bottom horizontal scroll bar all the way to the right-side of your preview, you will be able to see the time that the data has been received in the Event Hub in twocolumns called, **EventProcessedUtcTime** and**EventEnqueuedUtcTime**. This should reflect the current date/time in UTC format.

    ![A screenshot of a computer Description automatically generated](./media/image18.png)

### Task 2: Setup Eventstream Destination

1.  Click on the tile within the canvas area labeled "Switch to edit mode to Transform event of add destination
    
    ![A screenshot of a computer](./media/image19.png)

2.  Within the Eventstream user interface, click on the **Transform vents or add destination** option to open the drop-down menu.

    ![A screenshot of a computer Description automatically generated](./media/image20.png)

3.  View the list of available operations that can be made to the stream.

    ![A screenshot of a phone Description automatically generated](./media/operations.png)

4.  Look below the operations and you will find the **Destinations** select the option that says **Eventhouse**.

    ![A screenshot of a computer Description automatically generated](./media/select-eventhouse.png)

5.  A new menu will open on the right-hand side of the screen. The first thing you need to modify for the destination is the **data ingestion mode**. The two options are **Direct Ingestion** and **Event processing before ingestion.** Because we are not going to transform anything in our Eventstream and directly load this information in a KQL database table ensure that you have selected the **Direct Ingestion** option.

    ![A white background with black text Description automatically generated](./media/select-direct-ingestion.png)

6.  Modify the remainder of the settings with the following details below.

-   Destination name -- **eh-kql-db-Fabrikam**

-   Workspace -- **RTI_username**

-   Eventhouse --**eh_Fabrikam**

-   KQL Database -- **eh_Fabrikam**

    ![A screenshot of a computer](./media/creating-eventstream-destination.png)

7.  Click on Save.

8.  With the Eventstream configured, click on the **Publish** button to save this Eventstream and begin your ingestion.

    ![A screenshot of a computer](./media/image25.png)

9.  Choose the **Configure** option within the **Destination** to  correctly map the stream to a table in the KQL Database.

    ![A screenshot of a chat Description automatically generated](./media/image26.png)

10. Click on the **+ New table** option underneath the **eh_Fabrikam** database.

    ![A screenshot of a computer](./media/image27.png)

11. Give the new table the name, **InternetSales** and then click on the checkmark.

    ![A screenshot of a computer](./media/image28.png)

12. You may need to update your **"Data connection name"** to meet requirements. Let us rename it to **"eh_Fabrikam_es_InternetSales".** Then we can click on **Next**.
    
    ![A screenshot of a computer](./media/image29.png)

13. After a few moments of searching for events, the user interface should allow you to see that sample data was found. Click on **Finish** at the bottom of the screen.

    ![A screenshot of a computer Description automatically generated](./media/image30.png)

14. After this you will be shown a summary. Once you have all green checkmarks, click **close** to move forward.

15. Once you see the user interface showing the mappings from the source to the Eventstream to the destination, you have correctly configured
    and started a stream of data into your KQL Database.

    ![A screenshot of a chat](./media/image31.png)

16. Incase if you do not see any data in the Data preview of your KQL Database destination, follow the below steps -

    i. Go to your destination KQL database
    
    ![A screenshot of a chat](./media/KQL-database-workaround1.png)

    ii. Click on **Get data (1)** and go to **Eventstream (2)** and select **Existing Eventstream (3)**.

    ![A screenshot of a chat](./media/KQL-database-workaround2.png)

    iii. Select the InternetSales table and give the give the following configurations -

       Workspace - **RTI_username**
    
       Eventstream Name - **es_Fabrikam_InternetSales**
    
       Data connectioin name - **give any name**

      Then click on **Next**, **Finish** and **Close** in the next tabs.
    
    ![A screenshot of a chat](./media/KQL-database-workaround3.png)

    iv. Now you should be able to see a new destination is created with the Data preview visible in it. The user interface showing the source and the destination with the data preview should look like as shown below,

     ![A screenshot of a chat](./media/KQL-database-workaround4.png)

# Kusto Query Language (KQL)

## Task 3: Authoring Kusto Database Queries

1.  Make your way back to your **RTI_username** workspace. You should see two new objects that have been created because of your
    Eventstream. The Eventstream itself as well as a Real-Time Intelligence Data stream.

    ![A screenshot of a computer](./media/image32.png)

2.  Open the **eh_Fabrikam** KQL Database item.

    ![A screenshot of a computer](./media/image33.png)

3.  Within this experience, you can get an overview of the current structure, size, and use of the KQL Database. Because the Eventstream is sending data to this KQL Database consistently you will notice the amount of storage will increase over time.

    ![A screenshot of a computer Description automatically generated](./media/image34.png)

4.  Click on the **refresh icon** in the top-right corner of the screen.

    ![A screenshot of a phone](./media/image35.png)

5.  The size of the database should have grown. Your numbers here will not be exact in their reflection with screenshots in the remainder
    of the lab as depending on how long you take to complete the content you will have received less or more records than other members of
    the class. This is completely fine and will not affect your ability to follow along whatsoever.

    ![A black text on a white background](./media/image36.png)

6.  Within your System details on the left-hand side of the screen, click on the table within your KQL Database called **InternetSales**.

    ![A screenshot of a computer](./media/image37.png)

7.  This menu will give you metadata details about the table you have created and any actively streaming data with your Eventstream.
    Again, the size of the table and the number of rows within the table are going to vary from student to student and will not affect your
    end results of this or any lab. A few additional items to call out on this menu include:

-   **Schema** -- This includes details about the column name and the data types of the column that can be queried with KQL.

-   **Mappings** -- This will list all the sources of data and in what format the data is being received. You can have various sources in
    various formats all mapping data to the same table on a KQL  Database.

-   **OneLake availability** -- This is currently active because of the  property we enabled at the higher level in the Eventhouse.

    ![A screenshot of a computer](./media/image38.png)

8.  Click on **Explore your data** in the top-right corner.

    ![A screenshot of a computer](./media/image39.png)

9.  This will open an interface in which you can author KQL queries against your database tables. There are a few pre-scripted queries
    that are already authored but need some slight customization. There are also two links to Microsoft documentation that can be helpful
    when learning KQL or also looking at SQL to KQL conversions which will be discussed later throughout this class.

    ![A screenshot of a computer Description automatically generated](./media/image40.png)

10. Click on **Line 8** and where the query says, **YOUR_TABLE_HERE**, replace that with the table name, **InternetSales**.

    ![A screenshot of a computer Description automatically generated](./media/image41.png)

11. Highlight **Line 8 and 9** and click on the **Run** button in the top-left corner of the window.

    ![A screenshot of a computer](./media/image42.png)

12. The query uses the **take** operator to bring back a specified number of rows. When the query runs it will pull data from the InternetSales table and bring back whatever number of rows you have plugged into the query. For this example, 100 rows will return. The specific rows returned cannot be determined with this operator and the results of your query will vary from the result of others.

    ![A screenshot of a computer Description automatically generated](./media/image43.png)

13. Click on **Line 12** and where the query says, **YOUR_TABLE_HERE**, replace that with the table name, **InternetSales**.

    ![A screen shot of a computer Description automatically generated](./media/image44.png)

14. Highlight **Line 12 and 13** and click on the **Run** button in the top-left corner of the window.
   
    ![A screenshot of a computer Description automatically generated](./media/image45.png)

15. This query uses the count operator. This query will return an aggregated number of records that exist at the time of the query execution on the KQL Database table. Feel free to run this query a few more times and you should notice that the number of records increases after every few seconds.

    ![A screenshot of a computer Description automatically generated](./media/image46.png)

16. Repeat the previous steps for the final query that is automatically created for you on **Line 16** and run the query again.

    ![A screenshot of a computer](./media/image47.png)

17. This query will give you the number of records that have been ingested into your table within an hour window. The overall distribution of these records for the data you are currently ingesting will be approximately 4,100 per hour. There will be slight variations within that numbers of transactions per hour though and this query will detail if less or more records were ingested in each
window.

### Task 4: Using T-SQL Queries Against a KQL Database

You may be working with the Kusto Query Language for the first time.While this language is intuitive and easy to learn for simple queries, you may want to return the results of a more complex queries than you are currently capable of. Several helpful tools have been included within the KQL Queryset capabilities including converting SQL queries to KQL queries and simply authoring T-SQL queries within the KQL Queryset. Let's explore!

1.  You need to create a query that returns the number of each product that has been sold. This is something you can quickly do with T-SQL.Within the query window, you can translate your SQL queries into KQL to better understand how to author KQL queries in the future. Start with writing the following command.
    
    ```
        --
    explain
    ```
    
        
    ![](./media/image49.png)

1. The comment line "--" followed by the keyword "explain" will allow you to now create a SQL query and return a result with the KQL query that could be used to achieve a similar query and result. Below input the following query to explain what the KQL query would look like:

   ```
       --
    explain
    SELECT COUNT(OrderQuantity) AS CountOfProducts
            , ProductKey
    FROM InternetSales
    GROUP BY ProductKey
    
    ```

  ![A screenshot of a computer Description automatically generated](./media/image51.png)


3. This is a simple SQL query that will retrieve results from the InternetSales table to return two columns, the product key and a count of the number of orders. Because there is an aggregated column and a non-aggregated column, you must use a GROUP BY to return results for each individual product. Run the entire query beginning with the "--" to the end of the T-SQL query.

   ![A screenshot of a chat Description automatically generated](./media/image52.png)

4. The output of the explain query should be a single record with the translated KQL query as the result. Click on the **caret icon (>)** to expand the results and allow for easier translation.

   ![A screenshot of a computer Description automatically generated](./media/image53.png)

5. Click on the query pane highlighted below in orange. This will allow you to select translated KQL query and copy it. Paste this query in
   the KQL Queryset we have been using

   ![A screenshot of a computer](./media/image54.png)

6. With the results in your query pane, highlight and run the query to retrieve the results. The **summarize** operator will produce a  table that aggregates the content of the input table while determining how to group each record with the **by Product Key** and the **project** operator will select the columns to include, rename, or drop while inserting new compute columns.

    ![A screenshot of a computer](./media/image55.png)

7. Feel free to explore a complete list of SQL to KQL cheat sheet operations at the top of your queryset for additional capabilities and conversions.

    ![A close-up of a message Description automatically generated](./media/image56.png)

8. Instead of using KQL, another alternative to querying the results of the KQL Database within Fabric is to write and run a T-SQL query.
    Highlight the original SQL statement that was used to translate the kQL query and run only that.

    ![A screenshot of a computer code](./media/image57.png)

9. This will also yield perfectly valid results without having to convert to KQL beforehand.

    ![A screenshot of a computer Description automatically generated](./media/image58.png)

# KQL Queryset

## Task 5: Create a KQL Queryset

1.  While most of the queries within this window were automatically created from the user interface, there may be times in the future where you wish to save the results of a query to access later in your exploration of KQL and working with your real-time data. Within the **Explore your data** window, you have several additional options for how to accomplish something like this.

2.  Click on the option **Save as KQL queryset**.

    ![A screenshot of a computer](./media/image59.png)

3.  Within the window that appears input the name of the object you're creating as **First KQL Query**. Click on **Create**.

    ![A screenshot of a computer Description automatically generated](./media/image60.png)

4.  A OneLake data hub menu may appear allowing you to connect to other KQL databases available in your organization. If this does appear, select the "**Cancel"** option in the bottom right corner

5.  You will see a new user interface appear when the KQL queryset opens. A new object has been created within your workspace that allows you to quickly go back to this queryset that you have authored and continue to add more queries, pin results to a dashboard, or build a Power BI report. Additional capabilities
within this interface include working with Copilot, should that be available to your organization, and exporting results to a CSV file.

    ![A screenshot of a computer](./media/image61.png)

6.  Return to your **RTI_username** workspace. You should have the following objects present

    ![A screenshot of a computer Description automatically generated](./media/image62.png)

## Summary

In this lab, you began by setting up a connection to an Event Hub that has a running stream of data and used an Eventstream to take that data and ingest it into a KQL Database. Once the data was ingested, you were able to author several KQL queries and look at the functionality of using T-SQL to help learn KQL syntax or simply return results with SQL statements.

## References 

Fabric Real-time Intelligence in a Day (RTIIAD) introduces you to some of the key functions available in Microsoft Fabric.

In the menu of the service, the Help (?) section has links to some great resources.


![](./media/image64.jpg)

Here are a few more resources that will help you with your next steps with Microsoft Fabric.

-   See blog post to read the full [Microsoft Fabric GA announcement](https://aka.ms/Fabric-Hero-Blog-Ignite23)
-   Explore Fabric through the [Guided  Tour](https://aka.ms/Fabric-GuidedTour)
-   Sign up for the [Microsoft Fabric free trial](https://aka.ms/try-fabric)
-   Visit the [Microsoft Fabric  website](https://aka.ms/microsoft-fabric)
-   Learn new skills by exploring the [Fabric Learning  modules](https://aka.ms/learn-fabric)
-   Explore the [Fabric technical  documentation](https://aka.ms/fabric-docs)
-   Read the [free e-book on getting started with  fabric](https://aka.ms/fabric-get-started-ebook)
-   Join the [Fabric community](https://aka.ms/fabric-community) to post  your questions, share your feedback, and learn from others

Read the more in-depth Fabric experience announcement blogs:

-   [Data Factory experience in Fabric blog](https://aka.ms/Fabric-Data-Factory-Blog)
-   [Synapse Data Engineering experience in Fabric  blog](https://aka.ms/Fabric-DE-Blog)
-   [Synapse Data Science experience in Fabric  blog](https://aka.ms/Fabric-DS-Blog)
-   [Synapse Data Warehousing experience in Fabric  blog](https://aka.ms/Fabric-DW-Blog)
-   [Real-](https://aka.ms/Fabric-RTA-Blog)[Time Intelligence  experience in Fabric blog](https://blog.fabric.microsoft.com/en-us/blog/category/real-time-intelligence)]
-   [Power BI announcement  blog](https://aka.ms/Fabric-PBI-Blog)
-   [Data Activator experience in Fabric  blog](https://aka.ms/Fabric-DA-Blog)
-   [Administration and governance in Fabric  blog](https://aka.ms/Fabric-Admin-Gov-Blog)
-   [OneLake in Fabric  blog](https://aka.ms/Fabric-OneLake-Blog)
-   [Dataverse and Microsoft Fabric integration  blog](https://aka.ms/Dataverse-Fabric-Blog)


 © 2024 Microsoft Corporation. All rights reserved.

 By using this demo/lab, you agree to the following terms:

 The technology/functionality described in this demo/lab is provided by Microsoft Corporation for purposes of obtaining your feedback and to
 provide you with a learning experience. You may only use the demo/lab to evaluate such technology features and functionality and provide
 feedback to Microsoft. You may not use it for any other purpose. You may not modify, copy, distribute, transmit, display, perform,
 reproduce, publish, license, create derivative works from, transfer, or sell this demo/lab or any portion thereof.

 COPYING OR REPRODUCTION OF THE DEMO/LAB (OR ANY PORTION OF IT) TO ANY OTHER SERVER OR LOCATION FOR FURTHER REPRODUCTION OR REDISTRIBUTION IS EXPRESSLY PROHIBITED.

 THIS DEMO/LAB PROVIDES CERTAIN SOFTWARE TECHNOLOGY/PRODUCT FEATURES AND FUNCTIONALITY, INCLUDING POTENTIAL NEW FEATURES AND CONCEPTS, IN A
 SIMULATED ENVIRONMENT WITHOUT COMPLEX SET-UP OR INSTALLATION FOR THE PURPOSE DESCRIBED ABOVE. THE TECHNOLOGY/CONCEPTS REPRESENTED IN THIS DEMO/LAB MAY NOT
 REPRESENT FULL FEATURE FUNCTIONALITY AND MAY NOT WORK THE WAY A FINAL VERSION MAY WORK. WE ALSO MAY NOT RELEASE A FINAL VERSION OF SUCH FEATURES OR CONCEPTS. 
 YOUR EXPERIENCE WITH USING SUCH FEATURES AND FUNCITONALITY IN A PHYSICALENVIRONMENT MAY ALSO BE DIFFERENT.

 **FEEDBACK**. If you give feedback about the technology features,functionality and/or concepts described in this demo/lab to Microsoft,you give to Microsoft, 
 without charge, the right to use, share and commercialize your feedback in any way and for any purpose. You also give to third parties, without charge, any 
 patent rights needed for their products, technologies and services to use or interface with any specific parts of a Microsoft software or service that 
 includes the feedback. You will not give feedback that is subject to a license that requires Microsoft to license its software or documentation to third
 parties because we include your feedback in them. These rights survive this agreement.

 MICROSOFT CORPORATION HEREBY DISCLAIMS ALL WARRANTIES AND CONDITIONS WITH REGARD TO THE DEMO/LAB, INCLUDING ALL WARRANTIES AND CONDITIONS OF MERCHANTABILITY, 
 WHETHER EXPRESS, IMPLIED OR STATUTORY, FITNESS FOR A PARTICULAR PURPOSE, TITLE AND NON-INFRINGEMENT. MICROSOFT DOES NOT MAKE ANY ASSURANCES OR
 REPRESENTATIONS WITH REGARD TO THE ACCURACY OF THE RESULTS, OUTPUT THAT DERIVES FROM USE OF DEMO/ LAB, OR SUITABILITY OF THE INFORMATION
 CONTAINED IN THE DEMO/LAB FOR ANY PURPOSE.

 **DISCLAIMER**
 
 This demo/lab contains only a portion of new features and enhancements in Microsoft Power BI. Some of the features might change in future
 releases of the product. In this demo/lab, you will learn about some, but not all, new features.
