# Microsoft Fabric Real-Time Intelligence in a Day Labo 2
 
# Sommaire
- Structure du document	
- Introduction	
- Hub en temps réel Fabric	
- Tâche 1 : créer une source de flux d’événements	
- Tâche 2 : configurer la destination EventStream	
- Langage de requête Kusto (KQL)	
- Tâche 3 : créer des requêtes de base de données Kusto	
- Tâche 4 : utiliser des requêtes T-SQL sur une base de données KQL	
- Jeu de requêtes KQL	
- Tâche 5 : Utiliser un jeu de requêtes KQL	
- Résumé	
- Références	
 
# Structure du document

Le labo comprend des étapes à suivre par l’utilisateur, ainsi que des captures d’écran associées qui fournissent une aide visuelle. Dans chaque capture d’écran, des sections sont mises en évidence avec des encadrés orange afin de souligner la ou les zones sur laquelle/lesquelles l’utilisateur doit se concentrer.

# Introduction
Dans ce labo, vous allez découvrir un moyen de gérer un flux continu de données en temps réel. Vous allez ingérer ces données dans l’Eventhouse que vous avez créée dans le dernier labo et écrire des requêtes KQL de base à l’aide d’un objet Fabric Real-Time Intelligence appelé Eventstream.
À la fin de ce labo, vous saurez :
- comment créer un Eventstream ;
- comment charger des données en temps réel dans une base de données KQL ;
- comment rédiger des requêtes de base KQL (Langage de requête Kusto).

# Hub en temps réel Fabric
## Tâche 1 : créer une source de flux d’événements

1.	Ouvrez l’**espace de travail Fabric** que vous avez créé dans le dernier labo. Ensuite, nous pouvons voir l’Eventhouse que nous avons créée.

2.	Accédez au Hub en temps réel en cliquant sur le bouton **En temps réel** à gauche. Même si nous ne voyons pas de flux de données, cela changera prochainement.
 
3.	Cliquez sur le bouton vert **+ Connecter la source de données** qui devrait se trouver dans le coin supérieur droit.
 
4.	Une fenêtre s’ouvre alors pour vous permettre de sélectionner une source pour nos données de flux. Comme nous l’avons vu précédemment, de nombreuses options fantastiques existent, mais pour ce cours, nous allons sélectionner l’option « Azure Event Hubs ». Si « Azure Event Hubs » n’est pas visible aisément, cliquez sur les **sources Microsoft** dans la partie supérieure pour filtrer les options que vous voyez.

5.	Vous devez à présent créer une connexion au Azure Event Hub. Cliquez sur **Nouvelle connexion**, car vous n’avez actuellement aucune connexion.

6.	Sur la page des détails de votre environnement, copiez-collez tous les paramètres de connexion nécessaires dans les champs appropriés. Pour ces labos, nous nous connectons à un Event Hub dans lequel des données diffusées en continu sont envoyées depuis un notebook Python. Ce bloc-notes
crée de fausses transactions de vente à un rythme d’environ 3 100 transactions par heure. Espace de noms du 

- Event Hub : **rtiadhub{userid} – fourni par cloudlabs**
- Event Hub : **rti-iad-fabrikam**
- Nom de la clé d’accès partagé : **rti-reader**
- Clé d’accès partagé : **Disponible dans l’onglet Détails de l’environnement**

7.	Une fois toutes les propriétés renseignées, cliquez sur **Connecter**.

8.	Dans la configuration de la source de données Azure Event Hubs, vous aurez peut-être besoin de modifier le **Groupe de consommateurs** du Event Hub pour veiller à avoir accès à un point d’accès unique au flux de données. Pour cet atelier, vous pouvez laisser la valeur « $Default » comme indiqué ci-après
 
9.	Avant de finaliser cette source de données et cet Eventstream, redéfinissons le nom de notre Eventstream sur quelque chose de plus utile. Dans la section « Détails du flux » à droite, cliquez sur l’icône représentant un crayon en regard du champ « Nom de l’Eventstream » et nommons-le
« **es_Fabrikam_InternetSales** ».

10.	Nous pouvons maintenant cliquer sur **Suivant** pour être redirigés vers une dernière page de vue d’ensemble.

11.	Sur cet écran de vue d’ensemble, vérifiez que le contenu semble correct et cliquez sur **Créer la source**.

**>Remarque : vos coordonnées seront différentes de celles que vous voyez dans la capture d’écran**
 
12.	Une fois l’Eventstream et sa source créés, cliquez sur le bouton « **Ouvrir EventStream** »

13.	Vous êtes alors redirigé vers l’interface utilisateur EventStream. C’est ici que vous verrez votre flux de données source circuler dans notre eventstream et nous pouvons également ajouter des événements de transformation.

14.	L’**Activation** de votre source peut prendre quelques instants. Ensuite, cliquez sur l’icône centrale avec le nom de votre Eventstream, puis sur Actualiser si vous ne voyez pas d’aperçu des données.

**Remarque : si vous recevez un statut « Avertissement » autour d’une stratégie d’audit, ce n’est pas grave. Le flux fonctionne toujours.**
 
15.	Vous devriez maintenant voir un échantillon des données dans la fenêtre inférieure.
 
16.	Il s’agit d’un aperçu des données reçues du Azure Event Hub. Si vous faites glisser votre barre de défilement horizontale inférieure complètement à droite de votre aperçu, vous pouvez voir l’heure de réception des données dans l’Event Hub dans deux colonnes nommées **EventProcessedUtcTime** et **EventEnqueuedUtcTime**. Elles devraient refléter la date/l’heure actuelles au format UTC.

## Tâche 2 : configurer la destination EventStream

1.	Cliquez sur la vignette dans la zone du canevas intitulée « Passer en mode Édition pour transformer l’événement ou ajouter une destination ».

2.	Dans l’interface utilisateur EventStream, cliquez sur le bouton **Transformer des événements ou ajouter une destination** pour ouvrir le menu déroulant.
 
3.	Affichez la liste des opérations disponibles qui peuvent être intégrées au flux.

4.	Examinez la section **Opérations** ci-dessous et vous trouverez les **Destinations** ; sélectionnez l’option **Eventhouse**.

5.	Un nouveau menu s’ouvre alors sur le côté droit de l’écran. Vous devez d’abord modifier
le**mode d’ingestion des données** pour la destination. Les deux options sont **Ingestion directe** et **Traitement de l'événement avant ingestion**" **as per screenshot**. Comme nous n’allons rien transformer dans notre Eventstream et charger directement ces informations dans une table de base de données KQL, veillez à sélectionner l’option **Ingestion directe**.
 
6. Modifiez le reste des paramètres avec les détails suivants :

  - Nom de la destination : eh-kql-db-fabrikam
  - Espace de travail : RTI_username
  - Eventhouse – eh_Fabrikam
  - Base de données KQL : eh_Fabrikam
 
7.	Cliquez sur Enregistrer.

8. Une fois l’Eventstream configuré, cliquez sur le bouton **Publier** pour enregistrer cet Eventstream et commencer votre ingestion.

9.	Si vous remarquez que la source **AzureEventHub** est devenue inactive, définissez le bouton bascule sur l’état « Actif » et choisissez l’option « Maintenant » lorsque la boîte de dialogue s’ouvre
 
10.	Cliquez sur le bouton **Configurer** dans la section **Destination** pour mapper correctement le flux à une table dans la base de données KQL.
 
11.	Cliquez sur le bouton **+ Nouvelle table** sous la base de données **eh_Fabrikam**.

12.	Nommez la nouvelle table **InternetSales**, puis cliquez sur la coche.

13.	Vous devrez peut-être mettre à jour le **« Nom de connexion de données »** pour répondre aux
exigences. Redéfinissons son nom sur **« eh_Fabrikam_es_InternetSales »**. Ensuite, nous pouvons cliquer sur Suivant.
 
14.	Après quelques instants de recherche d’événements, l’interface utilisateur devrait vous permettre de voir que des exemples de données ont été trouvés. Cliquez sur **Terminer** en bas de l’écran.
 
15.	Ensuite, un résumé s’affiche. Une fois que vous avez toutes les coches vertes, cliquez sur
**Fermer** pour avancer.

16.	Une fois que l’interface utilisateur affiche les mappages de la source à l’Eventstream et à la destination, vous avez correctement configuré et démarré un flux de données dans votre base de données KQL.
  
# Langage de requête Kusto (KQL)
## Tâche 3 : créer des requêtes de base de données Kusto

1. Retournez dans votre espace de travail **RTI_username**. Vous devriez voir le nouvel Eventstream que vous venez de créer, ainsi que tous les éléments d’Evenhouse.

2.	Ouvrez l’élément de base de données KQL **eh_Fabrikam**.

3.	Dans le cadre de cette expérience, vous pouvez bénéficier d’une vue d’ensemble de la structure, de la taille et de l’utilisation actuelles de la base de données KQL. Comme l’Eventstream envoie constamment des données à cette base de données KQL, notez que la quantité de stockage augmente au fil du temps.
 
4.	Cliquez sur l’**icône Actualiser** en haut de l’écran à droite.

5.	La taille de la base de données devrait avoir augmenté. La valeur que vous voyez peut ne pas être exacte par rapport aux captures d’écran dans le reste du labo. Selon le temps que vous aurez mis à compléter le contenu, vous aurez reçu moins ou plus d’enregistrements que les autres membres de la classe. Cela ne pose aucun problème et n’affectera pas du tout votre capacité à suivre ce labo.

6.	Dans la zone de navigation de la base de données sur le côté gauche de l’écran, cliquez sur la table de votre base de données KQL appelée **InternetSales** et vous verrez une vue d’ensemble de la table
 
7.	Cette vue d’ensemble vous donne des détails sur les métadonnées relatives à la table que vous avez créée et toutes les données diffusées activement en continu avec votre Eventstream. Encore une fois, la taille et le nombre de lignes de la table varient d’un étudiant à l’autre et n’affectent pas vos résultats finaux de ce labo ou de tout autre. Voici quelques options supplémentaires de ce menu :

  - **Suivi des données d’activité** : indique le nombre de lignes ingérées, l’heure à laquelle elles ont été générées pour la dernière fois et l’intervalle d’affichage
  - **Aperçu des données** : affiche un aperçu des résultats de l’ingestion de la table.
  - **Schema Insights** : cette option comprend des détails sur le nom de la colonne et les types de données de la colonne qui peuvent être interrogés avec KQL. Affiche également les 10 premières valeurs de la colonne sélectionnée
  - **Détails de la table** : affiche la taille comprimée et originale de la table, la disponibilité de OneLake, le nombre de lignes dans les tables et d’autres détails
 
8.	Revenez à la vue de la base de données et cliquez sur **Explorer vos données** dans le coin supérieur droit.

9.	Ceci ouvrira le jeu de requêtes KQL par défaut qui a été créé avec l’Eventhouse. Quelques requêtes pré-scriptées sont déjà créées, mais nécessitent une légère personnalisation. En outre, deux liens vers la documentation Microsoft peuvent être utiles lorsque vous découvrez KQL ou examinez des conversions SQL vers KQL, lesquelles seront abordées plus loin dans ce cours.
 
10.	Cliquez sur la **Ligne 8** et, là où la requête indique **YOUR_TABLE_HERE**, remplacez ce texte par le nom de la table, à savoir **InternetSales**.

11.	Mettez en surbrillance les **Lignes 8 et 9**, puis cliquez sur le bouton **Exécuter** dans le coin supérieur gauche de la fenêtre.
 
12.	La requête renvoie un nombre spécifié de lignes à l’aide de l’opérateur **take**. Lorsque la requête s’exécute, elle extrait les données de la table InternetSales et renvoie le nombre de lignes que
vous avez connecté à la requête. Dans cet exemple, 100 lignes ne seront retournées que, comme une clause WHERE en SQL. Les lignes spécifiques renvoyées ne peuvent pas être déterminées avec cet opérateur et les résultats de votre requête différeront des résultats des autres.

13.	Cliquez sur la **Ligne 12** et, là où la requête indique **YOUR_TABLE_HERE**, remplacez ce texte par le nom de la table, à savoir **InternetSales**.

14.	Mettez en surbrillance les **Lignes 12 et 13**, puis cliquez sur le bouton **Exécuter** dans le coin supérieur gauche de la fenêtre.

15.	Cette requête utilise l’opérateur count. Cette requête renvoie un nombre agrégé d’enregistrements qui existent au moment de l’exécution de la requête sur la table de base de données KQL. N’hésitez pas à exécuter cette requête plusieurs fois et vous devriez remarquer que le nombre d’enregistrements augmente toutes les quelques secondes.

16.	Répétez les étapes précédentes pour la dernière requête créée automatiquement pour vous sur la **Ligne 16/17** et réexécutez la requête.

17.	Cette requête vous indique le nombre d’enregistrements ingérés dans votre table dans un
intervalle d’une heure. La distribution globale de ces enregistrements pour les données que vous êtes en train d’ingérer est d’environ 4 100 par heure. Il y aura cependant de légères variations dans ce nombre de transactions par heure et cette requête détaillera si moins ou plus
d’enregistrements ont été ingérés dans chaque intervalle.

## Tâche 4 : utiliser des requêtes T-SQL sur une base de données KQL

Vous utilisez peut-être le Langage de requête Kusto pour la première fois. Bien que ce langage soit intuitif et facile à apprendre pour les requêtes simples, vous souhaiterez peut-être renvoyer les
résultats d’une requête plus complexe que celles que vous êtes actuellement capable de créer. Plusieurs outils utiles ont été inclus dans les fonctionnalités de jeu de requêtes KQL, notamment la conversion de requêtes SQL en requêtes KQL et la création simple de requêtes T-SQL dans le jeu de requêtes KQL. Explorons.

1.	Vous devez créer une requête qui renvoie chaque produit avec le nombre de fois qu’il a été vendu. Vous pouvez y parvenir rapidement avec T-SQL. Dans la fenêtre de requête, vous pouvez convertir vos requêtes SQL en requêtes KQL pour mieux comprendre comment créer des requêtes KQL à l’avenir. Commencez par écrire la commande suivante :
**(Remarque : double-cliquez sur l’objet ci-dessous afin de pouvoir copier le texte.)**

2.	La ligne de commentaire « -- » suivie du mot clé « explain » vous permet maintenant de créer une requête SQL et de renvoyer un résultat avec la requête KQL permettant d’obtenir une requête et un résultat similaires. En dessous, saisissez la requête suivante pour expliquer à quoi ressemblerait la requête KQL :
 
3.	Il s’agit d’une requête SQL simple qui récupère les résultats de la table InternetSales pour renvoyer deux colonnes : la clé de produit et un décompte du nombre de commandes. Comme il existe une colonne agrégée et une colonne non agrégée, vous devez renvoyer des résultats pour chaque produit individuel à l’aide d’une requête GROUP BY. Exécutez toute la requête en
commençant par la ligne de commentaire « -- » jusqu’à la fin de la requête T-SQL.

4.	La sortie de la requête explain devrait être un enregistrement unique avec la requête KQL traduite comme résultat. Cliquez sur le signe supérieur (>) pour développer les résultats et faciliter la traduction.
 
5.	Cliquez sur le volet de requête mis en surbrillance en orange ci-dessous. Ainsi, vous pouvez
sélectionner la requête KQL traduite et la copier. Collez cette requête dans le jeu de requêtes KQL que nous avons utilisé
 
6.	Une fois les résultats dans votre volet de requête, mettez en surbrillance la requête et exécutez- la pour récupérer les résultats. L’opérateur summarize produit une table qui agrège le contenu de la table d’entrée tout en déterminant comment regrouper chaque enregistrement avec la mention by Product Key et l’opérateur project sélectionne les colonnes à inclure, renommer ou supprimer tout en insérant de nouvelles colonnes de calcul.
7.	N’hésitez pas à explorer une liste des opérations de l’aide-mémoire SQL vers KQL en haut de votre jeu de requêtes pour des fonctionnalités et conversions supplémentaires.
 
8.	Au lieu d’utiliser KQL, une alternative à l’interrogation des résultats de la base de données KQL dans Fabric consiste à écrire et exécuter une requête T-SQL. Mettez en surbrillance l’instruction SQL d’origine ayant permis de traduire la requête KQL et exécutez uniquement celle-ci.
 
 
9.	Cela donnera également des résultats parfaitement valides sans avoir à convertir en KQL au préalable.

Jeu de requêtes KQL
Tâche 5 : Utiliser un jeu de requêtes KQL
1.	Bien que la plupart des requêtes de cette fenêtre aient été créées automatiquement à partir de l’interface utilisateur, il se peut qu’à l’avenir vous souhaitiez créer vos propres requêtes KQL en partant de zéro. Pour ce faire, vous devez utiliser la fonctionnalité d’onglets située en haut de
l’écran. Il convient également de noter que ce jeu de requêtes effectue automatiquement des sauvegardes périodiques.

2.	Vous remarquerez qu’en haut du jeu de requêtes, le nom par défaut de notre première page est le même que celui de notre base de données.
 
3.	Nous allons renommer cet onglet en cliquant sur l’icône en forme de crayon, appelons-le
« Ma première requête KQL ».
 
 
4.	À l’avenir, si nous souhaitons isoler notre code, nous pouvons simplement créer des onglets supplémentaires en cliquant sur l’icône « + ».



5.	Revenez à votre espace de travail RTI_username. Les objets suivants devraient être présents


Résumé
Dans ce labo, vous avez commencé par configurer une connexion à un Event Hub comportant un flux de données en cours d’exécution, puis vous avez extrait ces données et les avez ingérées dans une base de données KQL à l’aide d’un Eventstream. Une fois les données ingérées, vous avez pu créer plusieurs requêtes KQL et examiner le fonctionnement de T-SQL pour découvrir la syntaxe KQL ou
simplement renvoyer des résultats avec des instructions SQL.
 
Références
Fabric Real-Time Intelligence in a Day (RTIIAD) vous présente certaines des fonctions clés de Microsoft Fabric.
Dans le menu du service, la section Aide (?) comporte des liens vers d’excellentes ressources.

Voici quelques autres ressources qui vous aideront lors de vos prochaines étapes avec Microsoft Fabric :
•	Consultez le billet de blog pour lire l’intégralité de l’ annonce de la GA de Microsof t Fabric
•	Explorez Fabric grâce à la visite guidée
•	Inscrivez-vous pour bénéficier d’un essai gratuit de Microsof t Fabric
•	Rendez-vous sur le site web Microsoft Fabric
•	Acquérez de nouvelles compétences en explorant les modules d’apprentissage Fabric
•	Explorez la documentation technique Fabric
•	Lisez le livre électronique gratuit sur la prise en main de Fabric
•	Rejoignez la communauté Fabric pour publier vos questions, partager vos commentaires et apprendre des autres
Lisez les blogs d’annonces plus détaillés sur l’expérience Fabric :
•	Blog Expérience Data Factory dans Fabric
•	Blog Expérience Synapse Data Engineering dans Fabric
•	Blog Expérience Synapse Data Science dans Fabric
•	Blog Expérience Synapse Data Warehousing dans Fabric
•	Blog Expérience Real-Time Intelligence dans Fabric
•	Blog Annonce Power BI
•	Blog Expérience Data Activator dans Fabric
•	Blog Administration et gouvernance dans Fabric
•	Blog OneLake dans Fabric
•	Blog Intégration de Dataverse et Microsof t Fabric
 
© 2024 Microsoft Corporation. Tous droits réservés.
En effectuant cette démonstration/ce labo, vous acceptez les conditions suivantes :
La technologie/fonctionnalité décrite dans cette démonstration/ce labo est fournie par Microsoft Corporation en vue d’obtenir vos commentaires et de vous fournir une expérience d’apprentissage.
Vous pouvez utiliser cette démonstration/ce labo uniquement pour évaluer ces technologies et fonctionnalités, et pour fournir des commentaires à Microsoft. Vous ne pouvez pas l’utiliser à d’autres fins. Vous ne pouvez pas modifier, copier, distribuer, transmettre, afficher, effectuer, reproduire, publier, accorder une licence, créer des œuvres dérivées, transférer ou vendre tout ou une partie de cette démonstration/ce labo.
LA COPIE OU LA REPRODUCTION DE CETTE DÉMONSTRATION/CE LABO (OU DE TOUTE PARTIE DE CEUX-CI) SUR TOUT AUTRE SERVEUR OU AUTRE EMPLACEMENT EN VUE D’UNE AUTRE
REPRODUCTION OU REDISTRIBUTION EST EXPRESSÉMENT INTERDITE.
CETTE DÉMONSTRATION/CE LABO FOURNIT CERTAINES FONCTIONNALITÉS DE PRODUIT/ TECHNOLOGIES LOGICIELLES, NOTAMMENT D’ÉVENTUELS NOUVEAUX CONCEPTS ET FONCTIONNALITÉS, DANS UN ENVIRONNEMENT SIMULÉ SANS CONFIGURATION NI INSTALLATION COMPLEXES AUX FINS DÉCRITES CI-DESSUS. LES TECHNOLOGIES/CONCEPTS REPRÉSENTÉS DANS CETTE DÉMONSTRATION/CE LABO PEUVENT NE PAS REPRÉSENTER LES FONCTIONNALITÉS COMPLÈTES ET PEUVENT NE PAS FONCTIONNER DE LA MÊME MANIÈRE QUE DANS UNE VERSION FINALE. IL EST ÉGALEMENT POSSIBLE QUE NOUS NE PUBLIIONS PAS DE VERSION FINALE DE CES FONCTIONNALITÉS OU CONCEPTS. VOTRE EXPÉRIENCE D’UTILISATION DE CES FONCTIONNALITÉS DANS UN ENVIRONNEMENT PHYSIQUE PEUT ÉGALEMENT ÊTRE DIFFÉRENTE.
COMMENTAIRES. Si vous envoyez des commentaires sur les fonctionnalités, technologies et/ou concepts décrit(e)s dans ces labos/cette démonstration à Microsoft, vous accordez à Microsoft, sans frais, le droit d’utiliser, de partager et de commercialiser vos commentaires de quelque manière et
à quelque fin que ce soit. Vous accordez également à des tiers, sans frais, les droits de brevet nécessaires pour leurs produits, technologies et services en vue de l’utilisation ou de l’interface avec des parties spécifiques d’un logiciel ou service Microsoft incluant les commentaires. Vous n’enverrez pas de commentaires soumis à une licence exigeant que Microsoft accorde une licence pour son logiciel ou sa documentation à des tiers du fait que nous y incluons vos commentaires. Ces droits survivent à ce contrat.
MICROSOFT CORPORATION DÉCLINE TOUTES LES GARANTIES ET CONDITIONS EN CE
QUI CONCERNE CETTE DÉMONSTRATION/CE LABO, Y COMPRIS TOUTES LES GARANTIES ET CONDITIONS DE QUALITÉ MARCHANDE, QU’ELLES SOIENT EXPLICITES, IMPLICITES
OU LÉGALES, D’ADÉQUATION À UN USAGE PARTICULIER, DE TITRE ET D’ABSENCE DE
CONTREFAÇON. MICROSOFT N’OFFRE AUCUNE GARANTIE OU REPRÉSENTATION EN CE QUI CONCERNE LA PRÉCISION DES RÉSULTATS, LA CONSÉQUENCE QUI DÉCOULE DE L’UTILISATION DE CETTE DÉMONSTRATION/CE LABO, OU L’ADÉQUATION DES INFORMATIONS CONTENUES DANS CETTE DÉMONSTRATION/CE LABO À QUELQUE FIN QUE CE SOIT.
CLAUSE D’EXCLUSION DE RESPONSABILITÉ
Cette démonstration/Ce labo comporte seulement une partie des nouvelles fonctionnalités et améliorations disponibles dans Microsoft Power BI. Certaines fonctionnalités sont susceptibles de changer dans les versions ultérieures du produit. Dans ce labo/cette démonstration, vous
allez découvrir comment utiliser certaines nouvelles fonctionnalités, mais pas toutes.
