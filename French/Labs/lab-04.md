# Sommaire

- Structure du document	
- Introduction	
- Infrastructure en médaillon dans les bases de données KQL	
    - Tâche 1 : créer des tables Bronze	
    - Tâche 2 : charger les tables Bronze à l’aide d’un pipeline de données	
    - Tâche 3 : transformer des tables en couche Argent	
    - Tâche 4 : créer une couche Or avec des vues matérialisées	
Lakehouse Fabric et Disponibilité OneLake	
    - Tâche 5 : créer une lakehouse	
    - Tâche 6 : raccourci vers les tables de base de données KQL	
- Résumé	
- Références	

 
# Structure du document

Le labo comprend des étapes à suivre par l’utilisateur, ainsi que des captures d’écran associées qui fournissent une aide visuelle. Dans chaque capture d’écran, des sections sont mises en évidence avec des encadrés orange afin de souligner la ou les zones sur laquelle/lesquelles l’utilisateur doit se concentrer.

# Introduction

Dans ce labo, vous allez créer une infrastructure en médaillon à l’aide de l’approche de couches Bronze, Argent et Or pour gérer les données à leurs différentes phases de développement et
d’utilisation dans l’analyse. Vous allez ensuite connecter les données de votre base de données KQL dans une lakehouse pour montrer à quelle vitesse vous pouvez partager vos données en temps réel avec les membres de votre organisation qui souhaitent les utiliser à des fins de reporting Power BI.

À la fin de ce labo, vous saurez:

- comment créer des tables de base de données KQL avec le Langage de requête Kusto ;

- comment charger des données dans des bases KQL avec des pipelines Data Factory ;

- comment créer des vues matérialisées dans des bases de données KQL ;

- comment créer une lakehouse et utiliser des raccourcis dans la base de données KQL.


# Infrastructure en médaillon dans les bases de données KQL

## Tâche 1 : créer des tables Bronze

1. Ouvrez l’**espace de travail Fabric** pour le cours, puis le jeu de requêtes KQL que vous avez créé dans le dernier labo, à savoir **Create tables**.

2. Dans ce jeu de requêtes KQL, renommons l’onglet d’origine « eh_Fabrikam » que nous avons ici pour le remplacer par « Create External Tables », car cela facilitera l’organisation et la
compréhension de ce jeu de requêtes.
 
3. Créons maintenant un onglet en cliquant sur l’icône « + » et nommons-le « Bronze Layer ».

4. Dans ce nouvel onglet, collez et mettez en surbrillance le code suivant, puis sélectionnez « Exécuter » pour créer quatre tables qui serviront de couche Bronze de l’infrastructure en médaillon.

```
//BRONZE LAYER
.execute database script <|

.create table [Address] (AddressID:int,AddressLine1:string,AddressLine2:string,City: string, StateProvince:string, CountryRegion:string, PostalCode: string, rowguid: guid, ModifiedDate:datetime)
.create table [Customer](CustomerID:int, NameStyle: string, Title: string, FirstName: string, MiddleName: string, LastName: string,Suffix:string, CompanyName: string, SalesPerson: string, EmailAddress: string, Phone: string, ModifiedDate: datetime)
.create table [SalesOrderHeader](SalesOrderID: int, OrderDate: datetime, DueDate: datetime, ShipDate: datetime, ShipToAddressID: int, BillToAddressID: int, SubTotal: decimal, TaxAmt: decimal, Freight: decimal, TotalDue: decimal, ModifiedDate: datetime)
.create table [SalesOrderDetail](SalesOrderID: int, SalesOrderDetailID: int, OrderQty: int, ProductID: int, UnitPrice: decimal , UnitPriceDiscount: decimal,LineTotal: decimal, ModifiedDate: datetime)
```

5. Une fois ce code exécuté, vous devriez immédiatement voir quatre tables créées dans votre Explorateur d’objets de base de données :

    - Address
    - Customer
    - SalesOrderDetail
    - SalesOrderHeader

6. Développez la **table Address** en cliquant sur l’icône « > » en regard du nom.

7. Le schéma (noms de colonne et types de données) de la table s’affiche alors. Un élément qu’il serait utile d’ajouter à cette table sur la base de données KQL serait une colonne masquée pour le temps d’ingestion qui sera utilisée plus tard dans l’architecture en médaillon. Ajoutons-la maintenant.Copiez-collez le script ci-dessous pour modifier les tables que vous venez de créer en ajoutant une colonne de temps d’ingestion :

``` 
//adds a hidden field showing ingestion time
.execute database script <|
.alter table Address policy ingestiontime true
.alter table Customer policy ingestiontime true
.alter table SalesOrderHeader policy ingestiontime true
.alter table SalesOrderDetail policy ingestiontime true
```

8. Les quatre nouvelles tables sont des tables vides avec leur schéma défini. À présent, vous avez besoin d’un moyen de charger correctement ces tables. Revenez à votre espace de travail **RTI_username**.

## Tâche 2 : charger les tables Bronze à l’aide d’un pipeline de données

1. Depuis l’espace de travail, sélectionnez l’option **« + Nouvel élément »** pour afficher le volet de sélection. Ensuite, recherchez et sélectionnez l’option nommée **Pipeline de données**.

2. Nommez le nouveau pipeline **Load KQL Database Bronze Layer**.

3. Sélectionnez **Créer**.

4. Lorsque le menu du pipeline s’affiche, cliquez sur la vignette **Assistant Copier des données**.

5. Pour commencer, vous devez créer une connexion à la base de données source à partir de laquelle vous souhaitez extraire les données. Cliquez sur le bouton **Base de données Azure SQL** sous
« Nouvelles sources ». Si vous ne le voyez pas immédiatement, vous pouvez filtrer les sources à l’aide de la barre de recherche en haut. Nous allons nous connecter à la même base de données Azure SQL externe que celle du labo précédent, mais à des tables différentes.
 
6. Vous devez saisir les détails de connexion de la base de données. Continuez à l’aide des informations dans votre environnement ou comme ci-dessous.

    - fabrikamdemo.database.windows.net
    - fabrikamdb
    - demouser
    - fabrikam@123456

7. Cliquez sur **Suivant** après avoir tout renseigné.

8. Dans la liste des tables disponibles, sélectionnez les suivantes:

    - SalesLT.Address
    - SalesLT.Customer
    - SalesLT.SalesOrderDetail
    - SalesLT.SalesOrderHeader
 

9. Cliquez sur **Suivant**.

10.	Vous devez maintenant configurer votre destination pour déterminer l’endroit où vous souhaitez que le pipeline envoie les données. Recherchez le **Hub de données OneLake**, puis sélectionner votre base de données KQL **eh_Fabrikam**.

11.	Si vous êtes invité à vous connecter, utilisez simplement les informations d’identification qui sont fournies sur la page Détails de l’environnement.

12.	Cliquez sur la table **SalesLT.Address** si elle n’est pas encore sélectionnée, puis sur la liste déroulante en regard de l’option **Table**. Sélectionnez l’option de table **Address**.
 
13.	Une vue d’ensemble des **Mappages de colonnes** s’affiche alors. Ainsi, vous pouvez visualiser tous les champs provenant de la base de données source que vous envoyez à votre base de données KQL. Vous pouvez supprimer des champs spécifiques si vous ne souhaitez pas qu’ils soient mappés à partir de la source.

14.	Procédez de même qu’aux étapes 11 et 12 pour les tables **SalesLT.Customer, SaleLT.SalesOrderDetail et SalesLT.SalesOrderHeader**. Aucun mappage de colonnes ne doit être effectué, donc il suffit de mettre en correspondance les noms de table. Après avoir correctement mappé toutes les tables, cliquez sur Suivant.

15.	La dernière page utilisant l’Assistant Copier des données est une page de vue d’ensemble
permettant de vérifier tous les paramètres que vous avez sélectionnés. Assurez-vous que vos nombres de tables source et de destination sont identiques.

16.	Cliquez sur **Enregistrer + Exécuter**.

17.	Quelques instants après, une fenêtre volante comprenant un **réglage** s’affiche. L’Assistant Copier des données que nous venons de terminer a créé une liste des tables à itérer et à charger dans les tables KQL. Cliquez simplement sur le bouton **OK** pour exécuter le pipeline tel qu’il est actuellement configuré à partir de l’Assistant Copier des données.
 
18.	Laissez le pipeline s’exécuter et au bout d’environ une minute, le déplacement des données devrait être terminé. Une fois que vous avez constaté que toutes les activités du pipeline
affichent le statut **Opération réussie**, vous avez transféré les données.

19.	Vérifions l’une de nos tables et les données. Revenez au jeu de requêtes KQL que nous avons utilisé, appelé **Create Tables** et assurez-vous que vous êtes dans l’onglet **Bronze Layer**, puis exécutez le script suivant

```
//Query the Bronze layer Customer table 

Customer
| take 100
```

20.	Des données similaires à celles de l’image ci-dessous devraient s’afficher, mais elles peuvent ne pas être exactes
 
## Tâche 3 : transformer des tables en couche Argent

1. Maintenant que les tables Bronze sont chargées, nous allons créer un onglet dans notre jeu de requêtes KQL appelé « Silver Layer ».
 
2. Exécutez le script KQL suivant dans l’onglet « Silver Layer » pour créer quatre tables qui serviront de couche Argent de l’infrastructure en médaillon :

```
//SILVER LAYER

.execute database script <|

.create table [SilverAddress] (AddressID:int,AddressLine1:string,AddressLine2:string,City: string, StateProvince:string, CountryRegion:string, PostalCode: string, rowguid: guid, ModifiedDate:datetime, IngestionDate: datetime)

.create table [SilverCustomer](CustomerID:int, NameStyle: string, Title: string, FirstName: string, MiddleName: string, LastName: string,Suffix:string, CompanyName: string, SalesPerson: string, EmailAddress: string, Phone: string, ModifiedDate: datetime, IngestionDate: datetime)

.create table [SilverSalesOrderHeader](SalesOrderID: int, OrderDate: datetime, DueDate: datetime, ShipDate: datetime, ShipToAddressID: int, BillToAddressID: int, SubTotal: decimal, TaxAmt: decimal, Freight: decimal, TotalDue: decimal, ModifiedDate: datetime, DaysShipped: long, IngestionDate: datetime)

.create table [SilverSalesOrderDetail](SalesOrderID: int, SalesOrderDetailID: int, OrderQty: int, ProductID: int, UnitPrice: decimal, UnitPriceDiscount: decimal,LineTotal: decimal, ModifiedDate: datetime, IngestionDate: datetime)
```

3. Exécutez ce script en mettant en surbrillance le nouveau script et en cliquant sur **Exécuter**.
 
4. Une fois ce script exécuté, quatre nouvelles tables sont ajoutées au menu Tables de base de données KQL.

5. Maintenant que les tables ont été créées, vous devez y charger des données. Vous allez créer une stratégie de mise à jour pour transformer les données et les déplacer lorsqu’elles sont ingérées dans la couche Bronze. Copiez-collez le script suivant, puis cliquez sur **Exécuter** pour exécuter le code.

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

6. Bien que vous verrez des résultats de l’exécution de la requête, la meilleure preuve que votre requête est terminée est qu’un nouveau dossier extensible s’affiche dans votre volet Objets de base de données. Cliquez sur l’icône > en regard du **dossier Fonctions**. Ces fonctions permettent aux données chargées dans la couche Bronze de la base de données KQL d’être ensuite mises en miroir, transformées et chargées dans la couche Argent.
 
7. Simulons maintenant ce processus : vous allez réexécuter le pipeline que vous avez créé précédemment dans ce labo. Revenez maintenant au pipeline **Load KQL Database**.

8. Cliquez simplement sur le bouton **Exécuter** dans le **ruban Accueil** pour réexécuter le pipeline et charger les données dans la couche Bronze où elles sont ensuite transformées par les fonctions que vous avez créées et chargées dans les tables Argent.
 
9. Cliquez sur **OK** dans ce menu volant pour exécuter le pipeline avec les mêmes réglages que précédemment.
 
10.	Encore une fois, attendez environ une minute que le pipeline termine son chargement, puis lorsque tous les éléments de votre menu Sortie indiquent **Opération réussie**, passez à l’étape suivante.

11.	Une fois le pipeline de données terminé, validez les résultats dans la base de données KQL. Revenez au jeu de données KQL **Create Tables** et accédez à l’onglet **Silver Layer**.

12.	Sur une nouvelle ligne, interrogez la table SilverAddress en écrivant la requête suivante et en exécutant le code :

```
SilverAddress
| take 100
```

13.	Notez dans vos résultats que votre table **SilverAddress** comporte une colonne supplémentaire, à savoir **IngestionDate**, qui n’est pas physiquement présente dans la table **Address**.

## Tâche 4 : créer une couche Or avec des vues matérialisées

Maintenant que vous disposez de votre couche de données transformée dans la couche Argent, vous pouvez commencer à effectuer des analyses avec des données fiables, validées et enrichies dans un état Power BI, un jeu de données RTI ou simplement en créant des requêtes KQL. Cependant, vous pouvez parfois estimer qu’il est nécessaire d’agréger vos données pour les rendre plus utilisables par les utilisateurs finaux. Voyons comment y parvenir dans une base de données KQL.

1. Si ce n’est pas déjà fait, ouvrez votre jeu de requêtes KQL **Create Tables** et créez un onglet appelé « Gold Layer ».

2. Collez le code suivant dans le jeu de requêtes pour créer une vue matérialisée :

```
//GOLD LAYER
// use materialized views to view the latest changes in the SilverAddress table
.create materialized-view with (backfill=true) GoldAddress on table SilverAddress
{
SilverAddress
| summarize arg_max(IngestionDate, *) by AddressID
}
```

3. Une fois le code collé, mettez le code en surbrillance et exécutez-le en cliquant sur le bouton **Exécuter**.

4. Vous voyez alors une sortie dans les résultats de votre requête détaillant des informations sur la création de cette vue matérialisée.
 
5. Vous allez également voir qu’un autre dossier a été créé dans l’Explorateur d’objets de la base de données KQL. Développez le dossier **Materialized View** où se trouve votre vue **GoldAddress**.
 
6. Dans votre fenêtre de requête, exécutez le code suivant pour interroger la nouvelle vue matérialisée :

```
GoldAddress
| take 1000
```

7. Cette requête renvoie la ligne avec la dernière valeur **IngestionDate** pour chaque valeur **AddressID** unique dans la table SilverAddress.

8. À présent, collez et exécutez les requêtes suivantes pour créer d’autres vues matérialisées de la couche Or pour les autres tables :

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
9. Vous devriez maintenant disposer de six vues matérialisées dans votre base de données KQL.

10.	Vous venez de créer avec succès une infrastructure en médaillon dans une base de données KQL. Bien que ces données soient facilement utilisables, vous aurez des utilisateurs qui n’ont jamais utilisé Kusto et qui préféreront accéder aux données de ces tables par un autre moyen. Dans la tâche suivante, vous allez créer une lakehouse. Ensuite, à l’aide de la fonctionnalité Disponibilité de OneLake, que nous avons activée dans le labo 01, rendez certaines tables de notre base de données KQL accessibles au moyen de la lakehouse grâce à des raccourcis
 
# Lakehouse Fabric et Disponibilité OneLake

## Tâche 5 : créer une lakehouse

1. Revenez à l’espace de travail **RTI_username**.

2. Cliquez sur l’option **+ Nouvel élément** et sélectionnez **Lakehouse** dans la liste des options disponibles.

3. Nommez la lakehouse **lh_Fabrikam**, puis cliquez sur **Créer**. N’activez pas la fonctionnalité d’évaluation des schémas Lakehouse

## Tâche 6 : raccourci vers les tables de base de données KQL

Dans l’interface utilisateur Lakehouse, plusieurs options vous permettent d’importer des données diffusées en continu dans la lakehouse elle-même. Une option susmentionnée dans le cours consiste à charger des données directement dans la lakehouse à partir de l’Event Hub au lieu de la base de données KQL à l’aide d’un Eventstream. Comme vous avez déjà décidé que les bases de données KQL permettaient de répondre à des objectifs et besoins spécifiques, vous ne souhaitez pas copier à nouveau ces données. Importons plutôt les données de la base de données KQL dont nous disposons déjà à l’aide d’un **raccourci**, afin que les utilisateurs plus familiarisés avec cette expérience puissent avoir accès aux données que nous utilisons dans la base de données KQL

1. Dans le menu, cliquez sur la vignette **Nouveau raccourci**.
 
2. Sélectionnez l’option **Microsoft OneLake** sous **Sources internes**.

3. Dans le menu, sélectionnez la base de données KQL **eh_Fabrikam** pour importer les tables de cet espace de stockage dans la lakehouse sans dupliquer ni copier les données.
 
4. Cliquez sur **Suivant** en bas du menu.

5. Ouvrez les tables dans **eh_Fabrikam** en cliquant sur l’icône >, puis sélectionnez les tables suivantes à importer :

    - Clicks
    - Impressions
    - InternetSales

6. Ces tables peuvent être très utiles à tous les utilisateurs susceptibles d’exploiter des notebooks dans Fabric. Ces données peuvent être utilisées dans le cadre d’expériences de science des données, pour effectuer l’apprentissage d’un modèle qui prédit le lien susceptible d’intéresser les utilisateurs.

7. Cliquez sur **Suivant**.
 
8. Un dernier écran de validation s’affiche. Une fois satisfait de votre sélection, cliquez sur le bouton **Créer** en bas de l’écran.
 
9. Vous voyez alors que toutes les tables que vous avez sélectionnées dans la base de données KQL s’affichent dans la lakehouse.

10. Cliquez sur la table **Clicks**.
 
11.	Vous pouvez voir qu’un échantillon des enregistrements de cette table s’affiche dans votre interface utilisateur.

    > **Remarque** : l’affichage des données dans OneLake peut prendre quelques heures (https://learn.microsoft.com/en-us/fabric/real-time-intelligence/event-house-onelake- availability)

# Résumé

Dans ce labo, les utilisateurs ont créé une infrastructure en médaillon dans une base de données KQL (Langage de requête Kusto). Les utilisateurs ont ingéré des données brutes dans la couche Bronze de l’architecture en médaillon à l’aide d’un pipeline de données. Ils ont transformé ces données et les ont chargées dans la couche Argent pour un traitement et un affinement ultérieurs. Enfin, les utilisateurs ont agrégé et optimisé les données à des fins d’analyse dans la couche Or à l’aide de vues matérialisées.

Après avoir créé l’infrastructure en médaillon, les utilisateurs ont lié les données de la base de données KQL à une lakehouse à l’aide de raccourcis Microsoft Fabric. Cette intégration a facilité l’accès aux données et leur analyse dans les deux environnements. À la fin du labo, les utilisateurs ont vérifié l’association des données et effectué des requêtes de base pour s’assurer du bon fonctionnement de l’infrastructure.

# Références

Fabric Real-Time Intelligence in a Day (RTIIAD) vous présente certaines des fonctions clés de Microsoft Fabric. Dans le menu du service, la section Aide (?) comporte des liens vers d’excellentes ressources.
 
Voici quelques autres ressources qui vous aideront lors de vos prochaines étapes avec Microsoft Fabric :

- Consultez le billet de blog pour lire l’intégralité de l’  [texannonce de la GA de Microsof t Fabrict](https://aka.ms/Fabric-Hero-Blog-Ignite23) 

- Explorez Fabric grâce à la [visite guidée](https://aka.ms/Fabric-GuidedTour)

- Inscrivez-vous pour bénéficier d’un [essai gratuit de Microsof t Fabric](https://aka.ms/try-fabric)

- Rendez-vous sur le [site web Microsoft Fabric](https://aka.ms/microsoft-fabric)

- Acquérez de nouvelles compétences en explorant les [modules d’apprentissage Fabric](https://aka.ms/learn-fabric)

- Explorez la [documentation technique Fabric](https://aka.ms/fabric-docs)

- Lisez le [livre électronique gratuit sur la prise en main de Fabric](https://aka.ms/fabric-get-started-ebook)

- Rejoignez la [communauté Fabric](https://aka.ms/fabric-community) pour publier vos questions, partager vos commentaires et apprendre des autres

Lisez les blogs d’annonces plus détaillés sur l’expérience Fabric:

- [Blog Expérience Data Factory dans Fabric](https://aka.ms/Fabric-Data-Factory-Blog)

- [Blog Expérience Synapse Data Engineering dans Fabric](https://aka.ms/Fabric-DE-Blog)

- [Blog Expérience Synapse Data Science dans Fabric](https://aka.ms/Fabric-DS-Blog)

- [Blog Expérience Synapse Data Warehousing dans Fabric ](https://aka.ms/Fabric-DW-Blog)

- [Blog Expérience Real-Time Intelligence dans Fabric](https://blog.fabric.microsoft.com/en-us/blog/category/real-time-intelligence)

- [Blog Annonce Power BI](https://aka.ms/Fabric-PBI-Blog)

- [Blog Expérience Data Activator dans Fabric](https://aka.ms/Fabric-DA-Blog)

- [Blog Administration et gouvernance dans Fabric](https://aka.ms/Fabric-Admin-Gov-Blog)

- [Blog OneLake dans Fabric](https://aka.ms/Fabric-OneLake-Blog)

- [Blog Intégration de Dataverse et Microsoft Fabric](https://aka.ms/Dataverse-Fabric-Blog)


© 2024 Microsoft Corporation. Tous droits réservés.
En effectuant cette démonstration/ce labo, vous acceptez les conditions suivantes :

La technologie/fonctionnalité décrite dans cette démonstration/ce labo est fournie par Microsoft Corporation en vue d’obtenir vos commentaires et de vous fournir une expérience d’apprentissage. Vous pouvez utiliser cette démonstration/ce labo uniquement pour évaluer ces technologies et fonctionnalités, et pour fournir des commentaires à Microsoft. Vous ne pouvez pas l’utiliser à d’autres fins. Vous ne pouvez pas modifier, copier, distribuer, transmettre, afficher, effectuer, reproduire, publier, accorder une licence, créer des œuvres dérivées, transférer ou vendre tout ou une partie de cette démonstration/ce labo.

LA COPIE OU LA REPRODUCTION DE CETTE DÉMONSTRATION/CE LABO (OU DE TOUTE PARTIE DE CEUX-CI) SUR TOUT AUTRE SERVEUR OU AUTRE EMPLACEMENT EN VUE D’UNE AUTRE REPRODUCTION OU REDISTRIBUTION EST EXPRESSÉMENT INTERDITE.

CETTE DÉMONSTRATION/CE LABO FOURNIT CERTAINES FONCTIONNALITÉS DE PRODUIT/TECHNOLOGIES LOGICIELLES,NOTAMMENT D’ÉVENTUELS NOUVEAUX CONCEPTS ET FONCTIONNALITÉS, DANS UN ENVIRONNEMENT
SIMULÉ SANS CONFIGURATION NI INSTALLATION COMPLEXES AUX FINS DÉCRITES CI-DESSUS. LES TECHNOLOGIES/CONCEPTS REPRÉSENTÉS DANS CETTE DÉMONSTRATION/CE LABO PEUVENT NE PAS REPRÉSENTER LES FONCTIONNALITÉS COMPLÈTES ET PEUVENT NE PAS FONCTIONNER DE LA MÊME MANIÈRE QUE DANS UNE VERSION FINALE. IL EST ÉGALEMENT
POSSIBLE QUE NOUS NE PUBLIIONS PAS DE VERSION FINALE DE CES FONCTIONNALITÉS OU CONCEPTS. VOTRE EXPÉRIENCE D’UTILISATION DE CES FONCTIONNALITÉS DANS UN ENVIRONNEMENT PHYSIQUE PEUT ÉGALEMENT ÊTRE DIFFÉRENTE.

**COMMENTAIRES**. Si vous envoyez des commentaires sur les fonctionnalités, technologies et/ou concepts décrit(e)s dans ces labos/cette démonstration à Microsoft, vous accordez à Microsoft, sans frais, le droit d’utiliser, de partager et de commercialiser vos commentaires de quelque manière et à quelque fin que ce soit. Vous accordez également à des tiers, sans frais, les droits de brevet nécessaires pour leurs produits, technologies et services en vue de l’utilisation ou de l’interface avec des parties spécifiques d’un logiciel ou service Microsoft incluant les commentaires. Vous n’enverrez pas de commentaires soumis à une licence exigeant que.

Microsoft accorde une licence pour son logiciel ou sa documentation à des tiers du fait que nous y incluons vos commentaires. Ces droits survivent à ce contrat.
MICROSOFT CORPORATION DÉCLINE TOUTES LES GARANTIES ET CONDITIONS EN CE QUI CONCERNE CETTE DÉMONSTRATION/CE LABO, Y COMPRIS TOUTES LES GARANTIES ET CONDITIONS DE
QUALITÉ MARCHANDE,
QU’ELLES SOIENT EXPLICITES, IMPLICITES OU LÉGALES, D’ADÉQUATION À UN USAGE PARTICULIER, DE TITRE ET D’ABSENCE DE
CONTREFAÇON. MICROSOFT N’OFFRE AUCUNE GARANTIE OU REPRÉSENTATION EN CE QUI
CONCERNE LA PRÉCISION DES RÉSULTATS, LA CONSÉQUENCE QUI DÉCOULE DE L’UTILISATION DE CETTE DÉMONSTRATION/CE LABO, OU L’ADÉQUATION DES INFORMATIONS CONTENUES DANS CETTE DÉMONSTRATION/CE LABO À QUELQUE FIN QUE CE SOIT.

# CLAUSE D’EXCLUSION DE RESPONSABILITÉ

Cette démonstration/Ce labo comporte seulement une partie des nouvelles fonctionnalités et améliorations disponibles dans Microsoft Power BI. Certaines fonctionnalités sont susceptibles de changer dans les versions ultérieures du produit. Dans ce labo/cette démonstration, vous
allez découvrir comment utiliser certaines nouvelles fonctionnalités, mais pas toutes.
