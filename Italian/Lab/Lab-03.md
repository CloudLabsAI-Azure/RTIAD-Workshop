# Sommario 
- Struttura del documento
- Introduzione
- Eventstream Fabric
   - Attività 1. Creazione di un Eventstream
   - Attività 2. Trasformazione dell'Eventstream
   - Attività 3. Divisione dell'Eventstream e caricamento in due destinazioni
- Aggiunta di ulteriori dati al database KQL
   - Attività 4. Convalida delle tabelle dei dati degli eventi
   - Attività 5. Creazione di collegamenti al database KQL per le tabelle delle dimensioni
- Riepilogo
- Riferimenti

# Struttura del documento 
Il lab include i passaggi che l'utente deve seguire con gli screenshot associati che forniscono un aiuto visivo. In ogni screenshot vi sono sezioni evidenziate con riquadri arancioni che indicano le aree su cui l'utente deve concentrarsi. 

# Introduzione 
In questo lab si procederà a creare un altro Eventstream per inserire dati aggiuntivi nell'Eventhouse esistente. Vedremo come includere le trasformazioni all'interno dell'Eventstream per controllare quali dati aggiungere al database KQL
In questo lab si apprenderà quanto segue: 
- Come elaborare e trasformare un Eventstream
- Come scrivere query KQL per unire i dati da un database esterno
- Come usare KQL per eseguire query sui dati e visualizzarli all'interno di Power BI

# Eventstream Fabric
## Attività 1. Creazione di un Eventstream 

1.	Aprire **l'area di lavoro Fabric** usata per il corso odierno.

2.	Ci sono ulteriori dati in streaming che è possibile inserire in relazione al punto vendita di 
e-commerce. Per questo Eventstream, tuttavia, abbiamo intenzione di trasformare i dati prima di caricarli nell'Eventhouse. Anziché accedere all'**"hub in tempo reale"** possiamo creare un nuovo Eventstream direttamente dall'area di lavoro. Dal menu a discesa **+ Nuovo elemento** creare un nuovo **Eventstream**.

3.	Assegnare al nuovo Eventstream il nome **es_Fabrikam_ClickEvents**, selezionare l'opzione **"Funzionalità avanzate"**, quindi fare clic su **Crea**.
 
4.	Nella barra multifunzione Home fare clic sul menu a discesa **Aggiungi origine**, quindi selezionare **Origini esterne**.
 
5.	Analogamente al lab precedente, ci collegheremo a un Hub eventi di Azure in cui i dati vengono trasmessi in streaming da un notebook Python. Fare clic su **"Connetti"** per il riquadro **"Hub eventi di Azure"**. Se **"Hub eventi di Azure"** non è visibile nella sezione consigliata, selezionare **"Visualizzare tutte le origini"** per individuarlo.

6.	Creare una **Nuova connessione**.
 
7.	Dalla pagina dei dettagli dell'ambiente copiare e incollare tutte le impostazioni di connessione necessarie nei campi appropriati.

Spazio dei nomi hub eventi: **rtiadhub{username}**

Hub eventi: **rta-iad-clicks**

Nome chiave di accesso condivisa: **rti-reader**

Chiave di accesso condivisa: **fornita da Dettagli ambiente**
 
8.	Una volta compilate tutte le proprietà, fare clic su **Connetti**.

9.	Nella configurazione dell'origine dati Hub eventi di Azure potrebbe essere necessario modificare il **Gruppo di consumer** dell'hub eventi per essere certi di ottenere l'accesso a un punto di accesso univoco al flusso di dati. Per questo workshop è possibile lasciare il valore "$Default", come mostrato di seguito
 
10.	Fare clic su **Avanti**.

11.	Nella finestra di revisione e connessione, verificare che tutto sia configurato correttamente e fare clic su **Aggiungi**.
 
12.	Una volta configurato il flusso, sarà possibile visualizzare un'anteprima dei dati provenienti dall'hub eventi.
 
13.	Esaminare i dati ricevuti. Esistono due tipi di eventi registrati dal sito Web di e-commerce: clic e impressioni.
- **IMPRESSION**: un evento impressione viene registrato ogni volta che un annuncio pubblicitario o un'inserzione di prodotti viene visualizzata da un utente. Le impressioni misurano il numero di volte in cui un elemento (annuncio o prodotto) viene visualizzato, indipendentemente dal fatto che vi sia o meno un'interazione.
- **CLICK**: un evento clic viene registrato quando un utente interagisce con un elemento facendo clic su di esso. Di solito ciò indica un livello di engagement più elevato rispetto a un'impressione.

Oltre agli eventi clic e impressione registrati, vi sono dettagli sul prodotto oggetto del clic o dell'impressione, da quale dispositivo e browser è stata caricata la pagina Web, quale indirizzo IP ha effettuato l'accesso alla pagina e quanto tempo ha impiegato la pagina a caricarsi.

## Attività 2. Trasformazione dell'Eventstream
1.	A questo punto si trasformerà questo flusso di dati prima di inserirlo nel database KQL in un modo che possa essere facilmente compreso dagli analisti che desiderano ricavare informazioni da questi dati. All'interno del canvas dell'Eventstream, fare clic sul menu a discesa dell'oggetto **Trasforma eventi**.

2.	Dall'elenco delle operazioni disponibili, selezionare l'opzione **Gestisci campi**.
 
3.	Sulla nuova icona visualizzata, denominata **ManageFields**, fare clic sull**'icona della matita** per selezionare i campi da aggiungere al flusso dall'origine.
 
4.	Nel riquadro a comparsa visualizzato fare clic sul pulsante **Aggiungi tutti i campi**.

5.	Dall'elenco dei campi selezionare quello denominato **PartitionId** e fare clic sui puntini di sospensione (…) che compaiono quando si passa il mouse sopra il campo

6.	Scegliere l'opzione **Rimuovi** per rimuovere il campo. Per questo flusso di dati proveniente dall'hub eventi, il partizionamento non viene usato; questa colonna non presenta quindi alcuna utilità e per questo viene rimossa.
 
7.	Rimuovere tutti i seguenti campi che non sono necessari per questo flusso.
    - userAgent
    - page_loading_seconds
    - EventProcessedUtcTime
    - EventEnqueredUtcTime

    Dovrebbero rimanere i seguenti campi, come mostrato nell'immagine di seguito.
  
8.	Passare il mouse sul campo eventDate e, quando sul lato destro della finestra appaiono dei puntini di sospensione (…), fare clic su di essi.
 
9.	Scegliere l'opzione **Modifica**.
 
10.	Fare clic sull**'interruttore Modifica tipo** per modificare il tipo di dati di questo campo. Il tipo originale è String, è necessario modificare il **Tipo convertito** in **DateTime**. Fare clic su **Salva** al termine dell'operazione.

## Attività 3. Divisione dell'Eventstream e caricamento in due destinazioni
1.	Benché sia possibile caricare questo flusso di dati in un database KQL per l'analisi, potrebbe essere necessario un altro modo per consumare questi dati per differenziare gli eventi CLICK dagli eventi IMPRESSION. Aggiungere un'altra attività di trasformazione all'interfaccia utente passando il mouse sulla fine della trasformazione **ManageFields**

2.	Scegliere la trasformazione **Filtro** dall'elenco delle operazioni disponibili.

3.	Fare clic sull'**icona della matita** sulla nuova trasformazione, **Filter**.

4.	Nel riquadro a comparsa visualizzato sul lato destro della schermata personalizzare le condizioni del filtro per riflettere un modo per restituire solo valori CLICK usando le impostazioni sottostanti. È importante notare che la trasformazione Filter rileva la distinzione tra maiuscole e minuscole
    - **Nome dell'operazione**: Clicks
    - **Selezionare un campo in base al quale filtrare**: eventType
    - **Mantieni eventi quando il valore** uguale a CLICK **(Importante! Questo è un campo che supporta la distinzione tra maiuscole e minuscole, quindi assicurarsi di inserire solo lettere maiuscole per questo esempio)**
 
5.	Scegliere l'opzione **Salva** per mantenere le modifiche.

6.	Fare nuovamente clic sul **pulsante Aggiorna** per verificare che i dati siano stati filtrati per eventType CLICK.
 
7.	Queste potrebbero essere le uniche righe che si desidera inviare a una tabella, ma un'altra opzione è quella di creare due flussi separati per instradare informazioni diverse a due o più tabelle. Dalla barra multifunzione **Home** dell'Eventstream fare clic sul menu a discesa **Trasforma eventi**, quindi selezionare **Filtro**.
 
8.	Un nuovo oggetto denominato **Filter (il nome potrebbe variare)** verrà visualizzato nel canvas. Sarà necessario connettere il flusso **ManageFields** alla nuova trasformazione di filtro. Per creare la connessione, trascinare una linea dal punto verde di una trasformazione all'altra.

9.	Fare clic sull'**icona della matita** per **Filter** per modificarne le impostazioni.

10.	Nel riquadro a comparsa visualizzato sul lato destro della schermata personalizzare le condizioni del filtro per riflettere un modo per restituire solo valori IMPRESSION usando i valori sottostanti. Ricordare che la trasformazione Filter rileva la distinzione tra maiuscole e minuscole
    - **Nome dell'operazione**: Impressions
    - **Selezionare un campo in base al quale filtrare**: eventType
    - **Mantieni eventi quando il valore** uguale a IMPRESSION (Importante! Questo è un campo che supporta la distinzione tra maiuscole e minuscole, quindi assicurarsi di inserire solo lettere maiuscole per questo esempio)
 
11.	Scegliere l'opzione **Salva** per mantenere le modifiche.

12.	Prima di caricare i dati nelle nuove tabelle all'interno del database KQL, possiamo rimuovere le colonne aggiuntive che non sono necessarie. In questo caso, per il flusso di dati filtrato per i record "CLICK" non abbiamo più bisogno della colonna "eventType" poiché ogni riga contiene lo stesso valore. Per il flusso di dati "IMPRESSION" possiamo rimuovere la colonna "eventType" per gli stessi motivi sopra menzionati, come pure la colonna "referrer" poiché è vuota per ogni riga in questa tabella.

13.	Fare clic sull'icona + dopo l'operazione di filtro **Clicks**.
 
14.	Nel menu a discesa selezionare "Gestisci campi"

15.	Fare clic sull'**icona della matita** per selezionare i campi da aggiungere/rimuovere nel flusso

16.	Rinominare l'operazione in "Manage_Clicks". Selezionare "Aggiungi tutti i campi", quindi rimuovere "eventType". Al termine, fare clic su **Salva**.

17.	Ora procederemo ad aggiungere un'altra trasformazione "Gestisci campi" connessa al filtro "Impressions" come mostrato di seguito

18.	Fare clic sull'**icona della matita** per selezionare i campi da aggiungere/rimuovere nel flusso

19.	Rinominare l'operazione in "Manage_Impressions". Selezionare quindi "Aggiungi tutti i campi" e rimuovere "eventType" e "referrer". La trasformazione "Gestisci campi" dovrebbe essere simile a quella illustrata nell'immagine seguente:

20.	Una volta ripuliti i dati per i flussi per ciascun tipo di evento, occorre caricare ogni flusso in una nuova tabella nel database KQL. Fare clic sull'icona + dopo l'operazione di gestione dei campi **Manage_Clicks**.

21.	Nell'elenco a discesa visualizzato, andare a **Destinazioni** e selezionare **Eventhouse**.

22.	Fare clic sull'**icona della matita** per la destinazione Eventhouse.

23.	Per questa destinazione configurare le seguenti proprietà.
    - **Nome destinazione**: dbo-Clicks
    - **Area di lavoro**: RTI_username
    - **Eventhouse**: eh_Fabrikam
    - **Database KQL**: eh_Fabrikam
    - **KQL Destination table**: creare una nuova tabella denominata **Clicks**

24.	Fare clic su **Salva** nella parte inferiore del riquadro a comparsa.

25.	Eseguire la stessa operazione per la tabella Impressions con le informazioni configurate come di seguito.

26.	Salvare le modifiche.

27.	Questo Eventstream è ora pronto per iniziare lo streaming. Fare clic su **Pubblica** per avviare il flusso.

28.	Ora che l'Eventstream è in esecuzione, l'interfaccia utente dell'Eventstream dovrebbe cambiare leggermente, a indicare che si stanno trasmettendo i dati dall'hub eventi, che si sta trasformando e suddividendo tale flusso di dati e lo si sta caricando in due tabelle separate del database KQL.

# Aggiunta di ulteriori dati al database KQL
## Attività 4. Convalida delle tabelle dei dati degli eventi

1.	Tornare all'area di lavoro **RTI_username**.

2.	Aprire il database KQL **eh_Fabrikam**. 

3.	Con l'Eventstream in esecuzione dovrebbero apparire due nuove tabelle nella pagina Panoramica del database KQL. Dopo aver lasciato in esecuzione l'Eventstream per qualche istante, le **Tabelle principali** nel database KQL verranno visualizzate nella pagina Panoramica e mostreranno quanti dati vi sono archiviati. 

4.	Fare clic sulla tabella **Impressions**. Questa tabella riceve circa 1,5 milioni di record ogni 24 ore. Poiché le impressioni sono molte più dei clic, questa sarà la tabella più grande ai fini di questa lezione.

## Attività 5. Creazione di collegamenti al database KQL per le tabelle delle dimensioni
Fino a questo punto si è lavorato con i dati in streaming, ma mancano ancora alcuni elementi critici per poter ricavare informazioni dai dati importati. In questa attività importeremo dei dati da un database SQL di Azure esterno che fungerà da tabelle delle dimensioni all'interno del database KQL. Ciò consentirà di descrivere meglio i dati che stiamo attualmente trasmettendo in streaming. Ad esempio, tutte le tabelle contengono un ID prodotto che è un campo numerico, ma sarebbe preferibile poter visualizzare il nome del prodotto. I dati necessari per supportare questa modifica si trovano attualmente in un database SQL di Azure esterno. Vediamo quanto è semplice stabilire connessioni ad alcune di queste tabelle delle dimensioni.

1.	Nel database **eh_Fabrikam** fare clic sul menu a discesa **New related item**. Selezionare l'opzione KQL Queryset.

2.	Assegnare al set di query KQL il nome **Create Tables**, quindi fare clic sul pulsante Crea.

3.	Si aprirà l'hub dei dati OneLake e l'unica opzione disponibile per la selezione sarà il database KQL "**eh_Fabrikam**". Selezionare questo database e fare clic su "**Connetti**".

4.	Nella nuova interfaccia fare clic una volta all'interno della finestra di query ed evidenziare tutto il testo usando la combinazione di tasti **CTRL+A**. Una volta evidenziato tutto il testo, eliminarlo.
 
5.	Nella finestra di query vuota immettere il seguente script KQL. Questo script creerà una connessione a un database SQL di Azure esterno e lo renderà disponibile nel database KQL come **collegamento**. Un **collegamento** è associato in modalità di sola lettura, rendendo possibile la visualizzazione e l'esecuzione di query insieme ai dati in streaming inseriti nel database KQL.

    ```
    .execute database script <|
    //External tables - shortcuts
    // connect to operational Database with external table Product
    .create external table products (ProductID: int, ProductNumber: string,  Name: string) 
    kind=sql
    table=[SalesLT.Product]
    ( 
    h@'Server= fabrikamdemo.database.windows.net,1433;Initial Catalog=fabrikamdb;User  Id=demouser;Password=fabrikam@123456'
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

6.	Fare clic sul pulsante **Run** per eseguire lo script.

7.	Nella finestra Esplora database sarà ora visibile una nuova cartella denominata **Shortcuts**, all'interno della quale sono presenti due tabelle aggiuntive collegate a questo database KQL. Queste tabelle esistono all'interno di un database SQL di Azure, ma tramite lo script eseguito, ora sono collegate a questo database KQL per essere unite alle tabelle InternetSales e degli eventi.

8.	Una volta aggiunte qualità dimensionali al database, è possibile rispondere alle domande e fornire maggiore contesto ai consumer dei report nonché eseguire query su queste tabelle per ottenere informazioni dettagliate su tutta l'attività. Eseguire la seguente query KQL per visualizzare alcune delle informazioni richieste.

    ```
    InternetSales
    | join kind=inner
    (external_table("products")) on ($left.ProductKey == $right.ProductID)
    | summarize SalesPerProduct=sum(SalesAmount) by Name
    | project Name, SalesPerProduct
    ```

9.	Ora nei risultati della query saranno visibili i valori di ogni singolo prodotto venduto dalla società.

10.	Con la query evidenziata fare clic sul pulsante **Power BI** nella barra degli strumenti.

11.	In questo modo si ha l'opportunità di creare un report Power BI usando i dati presenti nel database KQL. È possibile soffermarsi brevemente sull'argomento, ma non è ancora necessario creare un report da questi dati. Fare clic sul **pulsante X** nell'angolo in alto a destra quando si è pronti per procedere.

12.	Tornare al database KQL **eh_Fabrikam**.

13.	Fare clic sull'opzione **Shortcuts** nel riquadro di spostamento **eh_Fabrikam**. Verranno mostrati tutti i collegamenti creati per questo database KQL. È opportuno notare che questi collegamenti sono considerati tabelle esterne classiche di Esplora dati di Azure che usano la sintassi delle tabelle esterne di Azure SQL e sono strutturati in modo diverso rispetto ai collegamenti OneLake, ADLS o S3 che sono anche supportati nel database KQL all'interno di Fabric.

# Riepilogo
In questo lab si è creato un altro flusso di dati, successivamente trasformato usando l'interfaccia utente dell'Eventstream in Fabric. Caricando i dati in due tabelle separate è possibile monitorare tutti i clic e le impressioni all'interno del sistema di e-commerce per scopi di marketing, pubblicità e analisi. È stato inoltre creato un collegamento a un database SQL di Azure esterno usando la funzionalità relativa alle tabelle esterne del set di query KQL. Ora si hanno a disposizione alcune dimensioni per comprendere meglio il contesto delle vendite e dei clic all'interno del database KQL.

# Riferimenti 
Fabric Real-time Intelligence in a Day (RTIIAD) presenta alcune delle funzioni chiave disponibili in Microsoft Fabric. 

Nel menu di servizio, la sezione Guida (?) include collegamenti ad alcune risorse utili. 
 
Di seguito sono riportate ulteriori risorse utili che consentiranno di progredire nell'uso di Microsoft Fabric. 
- Fare riferimento al post del blog per leggere l'[annuncio completo sulla disponibilità generale di Microsoft Fabric](https://www.microsoft.com/en-us/microsoft-fabric/blog/2023/11/15/prepare-your-data-for-ai-innovation-with-microsoft-fabric-now-generally-available/)

- Esplorare Fabric tramite la [Presentazione guidata](https://guidedtour.microsoft.com/en-us/guidedtour/microsoft-fabric/microsoft-fabric/1/1)

- Iscriversi alla [versione di valutazione gratuita di Microsoft Fabric](https://www.microsoft.com/en-us/microsoft-fabric/getting-started)

- Visitare il [sito Web di Microsoft Fabric](https://www.microsoft.com/en-in/microsoft-fabric)

- Acquisire nuove competenze esplorando i [moduli di apprendimento di Fabric](https://learn.microsoft.com/en-us/training/browse/?products=fabric&resource_type=module)

- Consultare la [documentazione tecnica di Fabric](https://learn.microsoft.com/en-us/fabric/)

- Leggere l'[e-book gratuito introduttivo a Fabric](https://info.microsoft.com/ww-landing-unlocking-transformative-data-value-with-microsoft-fabric.html)

- Unirsi alla [community di Fabric](https://community.fabric.microsoft.com/) per pubblicare domande, condividere feedback e imparare dagli altri

Leggere i blog di annunci più approfonditi sull'esperienza Fabric:

- [Blog sull'esperienza Data Factory in Fabric](https://blog.fabric.microsoft.com/en-us/blog/introducing-data-factory-in-microsoft-fabric/)

- [Blog sull'esperienza Synapse Data Engineering in Fabric](https://blog.fabric.microsoft.com/en-us/blog/introducing-synapse-data-engineering-in-microsoft-fabric/)

- [Blog sull'esperienza Synapse Data Science in Fabric](https://blog.fabric.microsoft.com/en-us/blog/introducing-synapse-data-science-in-microsoft-fabric/)

- [Blog sull'esperienza Synapse Data Warehousing in Fabric](https://blog.fabric.microsoft.com/en-us/blog/introducing-synapse-data-warehouse-in-microsoft-fabric/)

- [Blog sull'esperienza Real-Time Intelligence in Fabric](https://blog.fabric.microsoft.com/en-us/blog/category/real-time-intelligence)
 
- [Blog di annunci di Power BI](https://powerbi.microsoft.com/en-us/blog/empower-power-bi-users-with-microsoft-fabric-and-copilot/)

- [Blog sull'esperienza Data Activator in Fabric](https://blog.fabric.microsoft.com/en-us/blog/driving-actions-from-your-data-with-data-activator/)

- [Blog su amministrazione e governance in Fabric](https://blog.fabric.microsoft.com/en-us/blog/administration-security-and-governance-in-microsoft-fabric/)

- [Blog su OneLake in Fabric](https://blog.fabric.microsoft.com/en-us/blog/microsoft-onelake-in-fabric-the-onedrive-for-data/)

- [Blog sull'integrazione di Dataverse e Microsoft Fabric](https://www.microsoft.com/en-us/dynamics-365/blog/it-professional/2023/05/24/new-dataverse-enhancements-and-ai-powered-productivity-with-microsoft-365-copilot/)
 
© 2024 Microsoft Corporation. Tutti i diritti sono riservati. 
L'uso della demo/del lab implica l'accettazione delle seguenti condizioni:

La tecnologia/le funzionalità descritte nella demo/nel lab sono fornite da Microsoft Corporation allo scopo di ottenere feedback dall'utente e offrire un'esperienza di apprendimento. L'utilizzo della demo/del lab è consentito solo per la valutazione delle caratteristiche e delle funzionalità di tale tecnologia e per l'invio di feedback a Microsoft. L'utilizzo per qualsiasi altro scopo non
è consentito. È vietato modificare, copiare, distribuire, trasmettere, visualizzare, eseguire, riprodurre, pubblicare, concedere in licenza, usare per la creazione di lavori derivati, trasferire o vendere questa demo/questo lab o parte di essi.

SONO ESPLICITAMENTE PROIBITE LA COPIA E LA RIPRODUZIONE DELLA DEMO/DEL LAB (O DI QUALSIASI PARTE DI ESSI) IN QUALSIASI ALTRO SERVER O IN QUALSIASI ALTRA POSIZIONE PER ULTERIORE RIPRODUZIONE O RIDISTRIBUZIONE.

QUESTA DEMO/QUESTO LAB RENDONO DISPONIBILI TECNOLOGIE SOFTWARE/FUNZIONALITÀ DI PRODOTTO SPECIFICHE, INCLUSI NUOVI CONCETTI E NUOVE FUNZIONALITÀ POTENZIALI, IN UN AMBIENTE SIMULATO, CON UN'INSTALLAZIONE E UNA CONFIGURAZIONE PRIVE DI
COMPLESSITÀ, PER GLI SCOPI DESCRITTI IN PRECEDENZA. LA TECNOLOGIA/I CONCETTI RAPPRESENTATI IN QUESTA DEMO/IN QUESTO LAB POTREBBERO NON CONTENERE LE
FUNZIONALITÀ COMPLETE E IL LORO FUNZIONAMENTO POTREBBE NON ESSERE LO STESSO DELLA VERSIONE FINALE. È ANCHE POSSIBILE CHE UNA VERSIONE FINALE DI TALI FUNZIONALITÀ O CONCETTI NON VENGA RILASCIATA. L'ESPERIENZA D'USO DI TALI CARATTERISTICHE E
FUNZIONALITÀ PUÒ INOLTRE RISULTARE DIVERSA IN UN AMBIENTE FISICO.

**FEEDBACK**. L'invio a Microsoft di feedback sulle caratteristiche, sulle funzionalità e/o sui concetti della tecnologia descritti in questa demo/questo lab implica la concessione a Microsoft, a titolo gratuito, del diritto di usare, condividere e commercializzare tale feedback in qualsiasi modo e per qualsiasi scopo. Implica anche la concessione a titolo gratuito a terze parti del diritto di utilizzo di eventuali brevetti necessari per i loro prodotti, le loro tecnologie e i loro servizi al fine di usare
o interfacciarsi ai componenti software o ai servizi Microsoft specifici che includono il feedback. L'utente si impegna a non inviare feedback la cui inclusione all'interno di software o documentazione Microsoft imponga a Microsoft di concedere in licenza a terze parti tale software
o documentazione. Questi diritti sussisteranno anche dopo la scadenza del presente contratto.

CON LA PRESENTE MICROSOFT CORPORATION NON RICONOSCE ALCUNA GARANZIA
E CONDIZIONE RELATIVAMENTE ALLA DEMO/AL LAB, INCLUSE TUTTE LE GARANZIE E CONDIZIONI DI COMMERCIABILITÀ, DI FATTO ESPRESSE, IMPLICITE O PRESCRITTE DALLA LEGGE,
ADEGUATEZZA PER UNO SCOPO SPECIFICO, TITOLARITÀ E NON VIOLABILITÀ. MICROSOFT NON OFFRE GARANZIE O RAPPRESENTAZIONI IN RELAZIONE ALL'ACCURATEZZA DEI RISULTATI E DELL'OUTPUT DERIVANTI DALL'USO DELLA DEMO/DEL LAB O ALL'ADEGUATEZZA DELLE
INFORMAZIONI CONTENUTE NELLA DEMO/NEL LAB PER QUALSIASI SCOPO.
 
**CLAUSOLA DI RESPONSABILITÀ**
Questa demo/questo lab contiene solo una parte delle nuove funzionalità e dei miglioramenti in Microsoft Power BI. Alcune funzionalità potrebbero cambiare nelle versioni future del prodotto. In questa demo/in questo lab si apprendono alcune delle nuove funzionalità, ma non tutte.
