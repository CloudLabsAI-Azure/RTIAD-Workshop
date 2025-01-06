# Microsoft Fabric Real-Time Intelligence in a Day Lab 4

- Contents 
- Document Structure
- Introduction
- Medallion Framework within KQL Databases
    - Task 1: Create Bronze Tables
    - Task 2: Load Broze Tables using a Data Pipline
    - Task 3: Transform Tables in Silver Layer
    - Task 4: Create Gold Layer with Materialized Views
- Fabric Lakehouse and OneLake Availability
    - Task 5: Create Lakehouse
    - Task 6: Shortcut to KQL Database Tables
- Summary
- References

# Document Structure 

The lab includes steps for the user to follow along with associated
screenshots that provide visual aid. In each screenshot, sections are
highlighted with orange boxes to indicate the area(s) user should
focus on.

# Introduction 

In this lab, you will create a Medallion Framework using the Bronze,
Silver, Gold layer approach to handling data in it's different stages
of development and use it in analytics. You will then connect the data
from your KQL Database into a Lakehouse to showcase how quickly you
can share your real-time data with those in your organization who want
to use it for Power BI reporting.

By the end of this lab, you will have learned:

- Creating KQL Database Tables with the Kusto Query Language

- Loading data into KQL Databases with Data Factory Pipelines

- Creating Materialized Views in KQL Databases

- Creating a Lakehouse and Using Shortcuts to the KQL Database

# Medallion Framework within KQL Databases

## Task 1: Create Bronze Tables 

1. Open the **Fabric workspace** for the course and open the KQL
Queryset you created in the last lab, **Create Tables**.

    ![](images/media/image5.png)

2. Within this KQL Queryset lets rename the original tab that we have
here from "eh_Fabrikam" to "Create External Tables" making it easier
to organize and understand what we have in this Queryset.

    ![](images/media/image6.png)

3. Now let's create a new tab by selecting the "+" icon and name the
new tab "Bronze Layer"

    ![](images/media/image7.png)

4. Within this new tab, paste and highlight the following code and
select "Run" to create four new tables that will serve as your
Bronze Layer of the Medallion Framework.

    ![](images/media/image8.emf)

    ![](images/media/image9.png)

5. Once that executes you should immediately see four new tables
created within your Database Object Explorer.

    - Address

    - Customer

    - SalesOrderDetail

    - SalesOrderHeader

      ![](images/media/image10.png)

6. Expand the **Address table** by clicking on the "**"** icon next
to the name.

    ![](images/media/image11.png)

7. This shows you the schema (column names and data types) for the
table. One thing that will be helpful to add to this table on the
KQL Database would be a hidden column for the ingestion time that
will be used later in the Medallion architecture. Let's add that
now. Copy and paste the script below to alter the tables you just
created by adding an ingestion time column.

    ![](images/media/image12.emf)

    ![](images/media/image13.png)

8. The four new tables are blank tables with their schema defined. Now
you need a way to properly load these tables. Navigate back to your
workspace **RTI_username**.

## Task 2: Load Broze Tables using a Data Pipeline

1. From the workspace select the **"+ New Item"** option to bring up
the selection pane. Then find and select the option called **Data
pipeline**.

    ![](images/media/image14.png)

2. Give the new pipeline the name, **Load KQL Database Bronze Layer**.

    ![](images/media/image15.png)

3. Click on **Create**.

4. When the pipeline menu appears, click on the **Copy data assistant**
option.

    ![](images/media/image16.png)

5. To begin, you will need to create a connection to the source
database from where you wish to extract the data. Click on the
**Azure SQL database** option under "New sources". If you do not see
it immediately you can use the search bar at the top to filter
sources. We will be connecting to the same external Azure SQL
database from the prior lab but connecting to different tables.

    ![](images/media/image17.png)

6. You will need to input the connection details of the database.
Follow using the information in your environment or as below.

    - fabrikamdemo.database.windows.net

    - fabrikamdb

    - demouser

    - fabrikam@123456

7. Click **Next** when everything has been filled out.

8. From the list of available tables, select the following:

    - SalesLT.Address

    - SalesLT.Customer

    - SalesLT.SalesOrderDetail

    - SalesLT.SalesOrderHeader

    ![](images/media/image18.png)

9. Click on **Next**.

10. You will now be required to set up your destination to determine
where you want the pipeline to send the data to. Find the **OneLake
data hub** and then select your KQL database, **eh_Fabrikam**.

    ![](images/media/image19.png)

11. If you are prompted to sign-in, simply use the credentials provided
in the environment details page

12. Click on the **SalesLT.Address** table if it is not already selected
and then click on the dropdown next to the **Table** option. Click
on the **Address** table option.

    ![](images/media/image20.png)

13. You will now see an overview of the **Column mappings**. This will
allow you to visualize all the fields coming from the source
database that you are sending to your KQL Database. You have an
option to remove specific fields if you do not want them to map over
from the source.

    ![](images/media/image21.png)

14. Follow the same steps as Step 11-12 for the tables
**SalesLT.Customer**, **SaleLT.SalesOrderDetail**, and
**SalesLT.SalesOrderHeader**. No column mappings will need to be
performed so simply match up the table names. Once all tables have
been appropriately mapped, click on **Next**.

15. The final page using the Copy Data Assistant is an overview page to
verify all the settings you've selected. Ensure that your source
number of tables and destination number of tables are the same.

    ![](images/media/image22.png)

16. Click on **Save + Run**.

17. After a few moments a flyout window will appear that includes a
**Parameter**. The copy assistant wizard which we just completed,
created a list of the tables to iterate though and load into the kql
tables. Simply click on the **OK** button to run the pipeline as it
is currently configured from the Copy Data Assistant.

    ![](images/media/image23.png)

18. Let the pipeline run and after approximately one minute, the data
movement should be complete. Once you see that all activities within
the pipeline have **Succeeded**, you will have transferred the
data.

    ![](images/media/image24.png)

19. Let's go and check one of our tables and verify the data. Navigate
back to the KQL Queryset we have been using called **Create Tables**
and ensure you are in the **Bronze Layer** tab and run the following
script

    ![](images/media/image25.emf)

    ![](images/media/image26.png)

20. You should see some data like the image below, but it may not be
exact

    ![](images/media/image27.png)

## Task 3: Transform Tables in Silver Layer

1. Now that the Bronze tables are loaded we will create a new tab
within our KQL Queryset called "Silver Layer".
![](images/media/image28.png)

2. Run the following KQL script within the "Silver Layer" tab to create
four new tables that will serve as the Silver Layer of the Medallion
Framework.

    ![](images/media/image29.emf)

3. Run that script by highlighting the new script and clicking **Run**.

    ![](images/media/image30.png)

4. Once that script is executed, you will see four new tables added
into the KQL Database tables menu.

    ![](images/media/image31.png)

5. Now that the tables have been created, you need to load data into
them. You will create an update policy to transform the data and
move it when it is ingested into the bronze layer. Copy and paste
the following script and then **Run** the code.

    ![](images/media/image32.emf)

6. While you will see results of the query execution, the best evidence
that your query completed is that you will see a new expandable
folder in your Database objects pane. Click on the **icon** next
to the **Functions folder**. These functions will allow the data
loaded into the Bronze layer of the KQL Database to then be
mirrored, transformed and loaded into the Silver layer.

    ![](images/media/image33.png)

7. Now let's simulate this process, you will run the pipeline you
created earlier in this lab again. Navigate back to the **Load KQL
Database** pipeline now.

    ![](images/media/image34.png)

8. Simply click the **Run** button within the **Home ribbon** to
execute the pipeline again and load the data to the Bronze layer
where it will then be transformed by the functions you created and
loaded into the Silver tables.

    ![](images/media/image35.png)

9. Click **OK** on this flyout to run the pipeline with the same
parameters as previously.

    ![](images/media/image36.png)

10. Again, wait approximately one minute for the pipeline to complete
its load and when all items in your Output menu indicate
**Succeeded** move on to the next step.

    ![](images/media/image37.png)

11. Once the data pipeline has been completed, go and validate the
results in the KQL Database. Return to the **Create Tables** KQL
Queryset and navigate to the **Silver Layer** tab.

12. On a new line, query the SilverAddress table by writing out the
following query and executing the code.

    ![](images/media/image38.emf)

![A blue rectangle with red and black text Description automatically
generated](images/media/image39.png)

13. Notice in your results, your **SilverAddress** table has an
additional column, **IngestionDate**, that is not physically present
on the **Address** table.

    ![](images/media/image40.png)

## Task 4: Create Gold Layer with Materialized Views

Now that you have your transformed layer of data within the Silver Layer
you can start to perform analytics with trusted, validated, and enriched
data within a Power BI report, RTI Dataset, or just by simply authoring
some KQL queries. However, there may be times when you think it
necessary to aggregate your data to make it more consumable by end
users. Let's see how this is accomplished within a KQL database.

1. If it is not already, open your **Create Tables** KQL Queryset and
create a new tab calle "Gold Layer".

    ![](images/media/image41.png)

2. Paste in the queryset the following code for creating a materialized
view.

    ![](images/media/image42.emf)

3. Once the code has been pasted, highlight the code and execute it by
clicking on the **Run** button.

    ![](images/media/image43.png)

4. You will see an output in your query results detailing information
on how this materialized view was created.

    ![](images/media/image44.png)

5. You will also see another folder was created in the KQL Database
object explorer. Expand the **Materialized View** folder and you
will find your **GoldAddress** view within.

    ![](images/media/image45.png)

6. In your query window, run the following code to query the new
materialized view.

    ![](images/media/image46.emf)

    ![](images/media/image47.png)

7. This query will return the row with the latest **IngestionDate** for
each unique **AddressID** in the **SilverAddress** table.

8. Now paste and run the following queries to build more Gold layer
materialized views for the other tables.

    ![](images/media/image48.emf)

9. You should now have six materialized views within your KQL Database.

    ![](images/media/image49.png)

10. You have now successfully built a Medallion Framework within a KQL
Database. While this data is easily consumable, you will have users
that have never worked with Kusto and would prefer to access the
data from these tables through another means. In the next task you
will be creating a Lakehouse. Then, using the Onelake Availability
feature, which we enabled in Lab 01, make some of the tables in our
KQL Database accessible through the Lakehouse using shortcuts

# Fabric Lakehouse and OneLake Availability

## Task 5: Create Lakehouse

1. Return to the **RTI_username** workspace.

2. Click the **+ New Item** option and then select **Lakehouse** from
the list of available options.

    ![](images/media/image50.png)

3. Give the Lakehouse the name, **lh_Fabrikam** and then click on
**Create**. Do not enable the preview feature of Lakehouse schemas

    ![](images/media/image51.png)

## Task 6: Shortcut to KQL Database Tables

Within the Lakehouse user interface, you have a couple options for how
you could bring streaming data into the Lakehouse itself. One option
mentioned earlier in the class is using an Eventstream to load data
directly into the Lakehouse from the Event Hub instead of the KQL
Database. Since you have already decided that KQL Databases were to be
utilized to meet specific goals and requirements, you don't want to copy
that data again. Instead let's use a **Shortcut** to bring that data
from the KQL Database that we already have so users more familiar with
this experience can have access to the data we have been using in the
KQL Database

1. Choose the option from the menu that says **New shortcut**.

    ![](images/media/image52.png)

2. Select the option **Microsoft OneLake** under the **Internal
sources**.

    ![](images/media/image53.png)

3. Within the menu, select the **eh_Fabrikam** KQL Database to bring
tables from that storage into the Lakehouse without duplicating or
copying the data.

    ![](images/media/image54.png)

4. Click **Next** at the bottom of the menu.

5. Open the tables within the **eh_Fabrikam** by clicking on the **
icon** and then select the following tables to bring over.

    - Clicks

    - Impressions

    - InternetSales

      ![](images/media/image55.png)

6. These tables could be very useful to any users who may be leveraging
notebooks within Fabric. This data could be used in data science
experiments to train a model that predicts what link users might
likely be interested in.

7. Click **Next**.

8. One last validation screen will appear. Once you are satisfied with
your selection, click on the **Create** button at the bottom of the
screen.

    ![](images/media/image56.png)

9. You will now see all the tables you selected from the KQL Database
have appeared within the Lakehouse.

    ![](images/media/image57.png)

10. Click on the table called **Clicks**.

    ![](images/media/image58.png)

11. You can see a sample of the records from that table have appeared
within your user interface.

    **Note: It can take up to a few hours for the data to appear in
OneLake (https://learn.microsoft.com/en-us/fabric real-time-intelligence/event-house-onelake-availability)**

# Summary

In this lab, users created a Medallion Framework within a Kusto Query
Language (KQL) database. Users ingested raw data into the Bronze Layer
of the medallion architecture using a data pipeline. They transformed
this data and loaded it into the Silver Layer for further processing
and refinement. Finally, users aggregated and optimized the data for
analytics in the Gold Layer using Materialized Views.

After building the medallion framework, users employed Microsoft
Fabric shortcuts to link the data from the KQL database to a
Lakehouse. This integration allowed seamless access and analysis of
data across both environments. The lab concluded with users verifying
the data linkage and performing basic queries to ensure the
framework's functionality.

# References

Fabric Real-time Intelligence in a Day (RTIIAD) introduces you to some of the key functions available in Microsoft Fabric.

In the menu of the service, the Help (?) section has links to some great resources.


Here are a few more resources that will help you with your next steps with Microsoft Fabric.

- See blog post to read the full [Microsoft Fabric GA announcement](https://aka.ms/Fabric-Hero-Blog-Ignite23)
- Explore Fabric through the [Guided Tour](https://aka.ms/Fabric-GuidedTour)
- Sign up for the [Microsoft Fabric free trial](https://aka.ms/try-fabric)
- Visit the [Microsoft Fabric website](https://aka.ms/microsoft-fabric)
- Learn new skills by exploring the [Fabric Learning modules](https://aka.ms/learn-fabric)
- Explore the [Fabric technical documentation](https://aka.ms/fabric-docs)
- Read the [free e-book on getting started with Fabric](https://aka.ms/fabric-get-started-ebook)
- Join the [Fabric community](https://aka.ms/fabric-community) to post your questions, share your feedback, and learn from others

Read the more in-depth Fabric experience announcement blogs:

- [Data Factory experience in Fabric blog](https://aka.ms/Fabric-Data-Factory-Blog)
- [Synapse Data Engineering experience in Fabric blog](https://aka.ms/Fabric-DE-Blog)
- [Synapse Data Science experience in Fabric blog](https://aka.ms/Fabric-DS-Blog)
- [Synapse Data Warehousing experience in Fabric blog](https://aka.ms/Fabric-DW-Blog)
- [Real-](https://aka.ms/Fabric-RTA-Blog)[Time Intelligence experience in Fabric blog](https://blog.fabric.microsoft.com/en-us/blog/category/real-time-intelligence)[](https://aka.ms/Fabric-RTA-Blog)
- [Power BI announcement blog](https://aka.ms/Fabric-PBI-Blog)
- [Data Activator experience in Fabric blog](https://aka.ms/Fabric-DA-Blog)
- [Administration and governance in Fabric blog](https://aka.ms/Fabric-Admin-Gov-Blog)
- [OneLake in Fabric blog](https://aka.ms/Fabric-OneLake-Blog)
- [Dataverse and Microsoft Fabric integration blog](https://aka.ms/Dataverse-Fabric-Blog)

© 2024 Microsoft Corporation. All rights reserved.

By using this demo/lab, you agree to the following terms:

The technology/functionality described in this demo/lab is provided by Microsoft Corporation for purposes of obtaining your feedback and to provide you with a learning experience. You may only use the demo/lab to evaluate such technology features and functionality and provide feedback to Microsoft. You may not use it for any other purpose. You may not modify, copy, distribute, transmit, display, perform, reproduce, publish, license, create derivative works from, transfer, or sell this demo/lab or any portion thereof.

COPYING OR REPRODUCTION OF THE DEMO/LAB (OR ANY PORTION OF IT) TO ANY OTHER SERVER OR LOCATION FOR FURTHER REPRODUCTION OR REDISTRIBUTION IS EXPRESSLY PROHIBITED.

THIS DEMO/LAB PROVIDES CERTAIN SOFTWARE TECHNOLOGY/PRODUCT FEATURES AND FUNCTIONALITY, INCLUDING POTENTIAL NEW FEATURES AND CONCEPTS, IN A SIMULATED ENVIRONMENT WITHOUT COMPLEX SET-UP OR INSTALLATION FOR THE PURPOSE DESCRIBED ABOVE. THE TECHNOLOGY/CONCEPTS REPRESENTED IN THIS DEMO/LAB MAY NOT REPRESENT FULL FEATURE FUNCTIONALITY AND MAY NOT WORK THE WAY A FINAL VERSION MAY WORK. WE ALSO MAY NOT RELEASE A FINAL VERSION OF SUCH FEATURES OR CONCEPTS. YOUR EXPERIENCE WITH USING SUCH FEATURES AND FUNCITONALITY IN A PHYSICAL ENVIRONMENT MAY ALSO BE DIFFERENT.

**FEEDBACK**. If you give feedback about the technology features, functionality and/or concepts described in this demo/lab to Microsoft, you give to Microsoft, without charge, the right to use, share and commercialize your feedback in any way and for any purpose. You also give to third parties, without charge, any patent rights needed for their products, technologies and services to use or interface with any specific parts of a Microsoft software or service that includes the feedback. You will not give feedback that is subject to a license that requires Microsoft to license its software or documentation to third parties because we include your feedback in them. These rights survive this agreement.

MICROSOFT CORPORATION HEREBY DISCLAIMS ALL WARRANTIES AND CONDITIONS WITH REGARD TO THE DEMO/LAB, INCLUDING ALL WARRANTIES AND CONDITIONS OF MERCHANTABILITY, WHETHER EXPRESS, IMPLIED OR STATUTORY, FITNESS FOR A PARTICULAR PURPOSE, TITLE AND NON-INFRINGEMENT. MICROSOFT DOES NOT MAKE ANY ASSURANCES OR REPRESENTATIONS WITH REGARD TO THE ACCURACY OF THE RESULTS, OUTPUT THAT DERIVES FROM USE OF DEMO/ LAB, OR SUITABILITY OF THE INFORMATION CONTAINED IN THE DEMO/LAB FOR ANY PURPOSE.

**DISCLAIMER**

This demo/lab contains only a portion of new features and enhancements in Microsoft Power BI. Some of the features might change in future releases of the product. In this demo/lab, you will learn about some, but not all, new features.