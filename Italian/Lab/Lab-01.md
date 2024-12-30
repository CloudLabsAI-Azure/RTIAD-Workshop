
# Microsoft Fabric Real-Time Intelligence in a Day Lab 1

Versione: ottobre 2024
 
# Sommario

- Struttura del documento	

- Scenario/Esposizione del problema

- Introduzione	

- Licenza di Fabric
  
  - Attività 1. Abilitazione di una licenza di valutazione per Microsoft Fabric	

- Real-Time Intelligence e hub in tempo reale	

  - Attività 2. Elementi dell'esperienza Real-Time Intelligence	
  
  - Attività 3. Hub in tempo reale	

- Creazione di un'area di lavoro e di un Eventhouse	

  - Attività 4. Creazione di un'area di lavoro Fabric	
  
  - Attività 5. Creazione di un Eventhouse

- Riferimenti	

 
# Struttura del documento

Il lab include i passaggi che l'utente deve seguire con gli screenshot associati che forniscono un aiuto visivo. In ogni screenshot vi sono sezioni evidenziate con riquadri arancioni che indicano le aree su cui l'utente deve concentrarsi.

# Scenario/Esposizione del problema

Fabrikam è una società di e-commerce specializzata in un'ampia gamma di attrezzature e accessori per l'attività sportiva e l'escursionismo. La società vende i propri prodotti a clienti al dettaglio in tutto il mondo tramite la propria piattaforma online e prevede di ampliare la propria presenza in nuovi
mercati internazionali. È in atto una nuova iniziativa che prevede la fornitura di informazioni dettagliate in tempo reale da un sito di e-commerce per dare modo ai dirigenti di prendere decisioni tempestive sulla base di informazioni aggiornate.

In qualità di ingegnere analitico del team di vendita, si è investiti di una serie di responsabilità tra cui la raccolta, la pulizia e l'interpretazione di set di dati per risolvere problemi aziendali. Alcune delle attività previste sono la creazione e la gestione di pipeline di dati batch, lo sviluppo di visualizzazioni quali grafici e diagrammi, la creazione e l'ottimizzazione di modelli semantici e report completi e la presentazione dei risultati ai decision maker dell'organizzazione.

## Sfide attuali

- Necessità di gestire un flusso continuo di dati in tempo reale dal sito Web di e-commerce e, per farlo, occorre un'architettura solida e scalabile.

- Garantire analisi ed elaborazione dei dati in tempo reale per restare al passo con la natura frenetica delle vendite online.

- Gestire il volume e la velocità dei dati generati dalle interazioni degli utenti, dalle transazioni e dall'attività del sito Web.

- Integrare i dati in streaming in tempo reale con i dati storici per un'analisi completa.

- Usare l'architettura Medallion in un ambiente Eventhouse per strutturare in modo efficiente il flusso di dati.

- Sfruttare i dati Eventhouse all'interno di un lakehouse

- Si è interessati a sfruttare Microsoft Fabric per affrontare le sfide di cui sopra, usando Eventhouse, un database KQL ed Eventstream per creare una pipeline di elaborazione dati resiliente ed efficiente.
 
# Introduzione

Oggi si apprenderanno alcune funzionalità chiave di Microsoft Fabric. Questo è un workshop introduttivo che ha lo scopo di presentare le diverse esperienze di uso dei prodotti e i vari elementi disponibili in Fabric. Al termine di questo workshop si sarà appreso a usare Eventhouse, pipeline di dati, Eventstream, set di query KQL e un dashboard in tempo reale.

In questo lab si apprenderà quanto segue:
-	Come esaminare gli utenti tipo di Fabric
-	Come creare un'area di lavoro Fabric
-	Come creare un Eventhouse

# Licenza di Fabric

## Attività 1. Abilitazione di una licenza di valutazione per Microsoft Fabric

1.	Aprire il **browser Microsoft Edge** sul desktop e andare all'indirizzo
https://app.fabric.microsoft.com/. Si aprirà la pagina di accesso. **Nota**: se non si usa l'ambiente lab e si dispone di un account Power BI esistente, potrebbe essere opportuno usare il browser in modalità privata o in incognito.

2.	Immettere il **Nome utente** disponibile nella scheda **Variabili di ambiente** (accanto alla guida al lab) come **Posta elettronica** e fare clic su **Invia**.
 
3.	Si aprirà la schermata **Password**. Immettere la **Password** disponibile nella scheda **Variabili di ambiente** (accanto alla Guida al lab) fornita dall'istruttore.

4.	Fare clic su **Accedi e** seguire le istruzioni per accedere a Fabric.
 
 
5.	Si aprirà la **home page di Fabric**


Per lavorare con gli elementi di Fabric, sono necessarie una licenza di valutazione e un'area di lavoro con licenza di Fabric. Avviare la configurazione.

6.	Nell'angolo in alto a destra della schermata selezionare l'**icona utente**.

7.	Selezionare **Versione di valutazione gratuita**.
 
 
8.	Si apre la finestra di dialogo Esegui l'aggiornamento a una versione di valutazione gratuita di Microsoft Fabric. Selezionare **Attiva**.

9.	Si apre la finestra di dialogo Aggiornamento a Microsoft Fabric riuscito. Selezionare **Fabric Home Page**.

10.	Si aprirà nuovamente la **home page** di **Microsoft Fabric**.
 
# Real-Time Intelligence e hub in tempo reale

## Attività 2. Elementi dell'esperienza Real-Time Intelligence

1.	Fare clic sull'esperienza Real-Time Intelligence.

2.	Si verrà indirizzati alla **home page di Real-Time Intelligence**. Appariranno le categorie **Modelli di flussi di compiti, Elementi consigliati da creare e Altre informazioni su Real-Time Intelligence**. Nella categoria degli **Elementi consigliati** prendere nota di quanto segue:

     a.	**Casa eventi:** permette di creare un'area di lavoro di uno o più database KQL, che è possibile condividere tra progetti. Crea inoltre un database KQL all'interno 
        di Eventhouse.
     
     b.	**Set di query KQL:** permette di eseguire query sui dati per produrre tabelle e oggetti visivi condivisibili.
     
     c.	**Dashboard in tempo reale:** una raccolta di riquadri, facoltativamente organizzati in pagine, in cui ogni riquadro ha una query sottostante e una 
         rappresentazione visiva.
     
     d.	**Eventstream:** consente di acquisire, trasformare e instradare il flusso di eventi in tempo reale.
     
     e.	**Reflex:** permette di intraprendere automaticamente azioni quando vengono rilevati schemi o condizioni nei dati che cambiano.

 
## Attività 3. Hub in tempo reale

1.	Fare clic su **In tempo reale** nel riquadro di spostamento di Fabric sul lato sinistro della schermata.

2.	Si aprirà la finestra di dialogo **Hub in tempo reale**, in cui è possibile selezionare **Presentazione** o direttamente **Attività iniziali**.

 
3.	L'hub in tempo reale è la singola posizione da cui trasmettere in streaming i dati in movimento nell'intera organizzazione. Di questo hub viene automaticamente eseguito il provisioning per ogni tenant Microsoft Fabric. Permette di individuare, inserire, gestire e consumare facilmente dati in movimento provenienti da un'ampia gamma di origini.

4.	All'interno dell'hub in tempo reale si ha accesso a tre diverse tipologie di integrazione dei dati.

-	**Tutti i flussi di dati:** per gli Eventstream e i database KQL in esecuzione, tutti gli output degli Eventstream e le tabelle dei database KQL vengono automaticamente visualizzati nell'hub in tempo reale.

-	**Origini di streaming:** elenca tutte le risorse di streaming dei servizi Microsoft. Che si
tratti di hub eventi di Azure, hub IoT di Azure o altri servizi, è possibile inserire dati senza problemi nell'hub in tempo reale.

-	**Eventi Fabric:** gli eventi generati tramite artefatti Fabric ed origini esterne vengono resi disponibili in Fabric per supportare scenari guidati dagli eventi, come avvisi in tempo
reale e attivazione di azioni a valle. È possibile monitorare e reagire agli eventi, tra cui gli eventi degli elementi dell'area di lavoro Fabric e gli eventi di Archiviazione BLOB di Azure.

-	**Eventi di Azure:** questo elenco include gli eventi di sistema generati in Azure a cui è possibile accedere. È possibile monitorare un evento e impostare regole che invieranno notifiche o eseguiranno azioni quando vengono attivate.
 

5.	Nell'angolo in alto a destra dell'hub in tempo reale fare clic sul pulsante **+ Connettere un'origine dati**.

 
6.	Viene visualizzata una finestra che descrive nel dettaglio i flussi di dati attualmente disponibili per l'integrazione nell'hub in tempo reale. I flussi comprendono una combinazione di origini Azure e origini di streaming cloud esterne come Amazon Kinesis, Confluent Cloud Kafka e Google Cloud Pub/Sub. Sono disponibili anche alcuni dati di esempio da esplorare

7.	**Chiudere** la finestra Recupera eventi facendo clic sulla "X" nell'angolo in alto a destra.
 
# Creazione di un'area di lavoro e di un Eventhouse

## Attività 4. Creazione di un'area di lavoro Fabric

1.	Si procederà ora con la creazione di un'area di lavoro con licenza di Fabric. Selezionare **Aree di lavoro** nella barra di spostamento a sinistra.

2.	Selezionare **+ Nuova area di lavoro**.

3.	Si apre la finestra di dialogo **Crea un'area di lavoro** sul lato destro del browser.

4.	Nel campo **Nome** immettere **RTI_username**. Usare il nome utente fornito nei dettagli dell'ambiente.

**Nota:** il nome dell'area di lavoro deve essere univoco. Assicurarsi che sotto il campo Nome sia presente un segno di spunta verde e che sia indicato **"Questo nome è disponibile"**.

5.	Se lo si desidera, è possibile immettere una **Descrizione** per l'area di lavoro. Questo campo è facoltativo.
 
6.	Fare clic su **Avanzate** per espandere la sezione.

7.	In **Modalità licenza** assicurarsi che si sia selezionato **Versione di prova** (deve essere selezionato per impostazione predefinita).

8.	Selezionare **Applica** per creare una nuova area di lavoro.

**Nota:** se si apre la finestra di dialogo **Introduzione ai flussi di attività**, fare clic su **Chiudi**.

 
## Attività 5. Creazione di un Eventhouse

1.	Fare clic sulla casella **+ Nuovo** per aprire un nuovo riquadro contenente tutti gli elementi che è possibile creare in quest'area di lavoro Fabric.

2.	Selezionare **Casa eventi** dalla sezione **Archivia dati** del riquadro. Come indicato in precedenza,
un Eventhouse è in qualche modo assimilabile a un lakehouse, nel senso che è possibile archiviare dati, ma è incentrato sui dati in tempo reale.

3.	Nella finestra visualizzata, assegnare all'Eventhouse il nome **eh_Fabrikam**, quindi fare clic su **Crea**.
 
4.	È qui che, per il resto della formazione odierna, si trasmetteranno in streaming i dati provenienti da varie origini. Una volta creato l'elemento, verrà visualizzata una finestra che fornirà alcuni dettagli sull'Eventhouse. Fare clic sul pulsante **Get started.**

5.	Guardare una presentazione rapida dell'Eventhouse seguendo le descrizioni comandi verdi sullo schermo. La prima mostra che è stato creato un database Linguaggio di query Kusto (KQL) vuoto con l'Eventhouse.
 

6.	Seguire il resto delle descrizioni comandi sullo schermo che mostrano dove creare database
aggiuntivi, verificare l'archiviazione in OneLake dell'Eventhouse, controllare l'utilizzo delle risorse Fabric in minuti di calcolo e, infine, vedere quali azioni si sono verificate nell'Eventhouse.
 
7.	Nel riquadro di spostamento a sinistra dell'Eventhouse individuare il database KQL creato insieme all'Eventhouse e fare clic su di esso per visualizzarne i dettagli

8.	In tal modo sarà possibile avere ancora una scheda nel riquadro sinistro del browser per
visualizzare la panoramica dell'intero Eventhouse e una nuova scheda incentrata sulle proprietà del database KQL. Uno degli obiettivi da raggiungere in questo scenario è far sì che i dati trasmessi in streaming al database KQL siano accessibili tramite OneLake. Abilitando questa funzionalità, si rendono i dati in questo database KQL facilmente individuabili tramite collegamenti da usare in qualsiasi lakehouse. Individuare la sezione **Dettagli database** a destra e impostare su **Attivato**
l'opzione "Availability".

9.	Tornare all'area di lavoro **RTI_username** selezionandola dal lato sinistro del browser
 
 
10.	Se l'opzione **Flussi di compiti** occupa la maggior parte dello spazio, selezionare la doppia freccia verso l'alto sul lato destro per ridurla a icona

11.	Ora si hanno le basi per iniziare a inserire i dati in streaming in OneLake. Il passaggio successivo consiste nel creare un flusso di dati in grado di ricevere i dati in movimento.

In questo lab abbiamo esaminato l'interfaccia di Real-Time Intelligence e l'hub in tempo reale, creato un'area di lavoro Fabric e un Eventhouse dotato di un database KQL. Nel prossimo lab
inizieremo a esplorare le tecniche per l'inserimento dei dati dalle varie origini del patrimonio dati in OneLake ed eseguiremo alcune analisi di base con il Linguaggio di query Kusto (KQL).
 
# Riferimenti

Fabric Real-time Intelligence in a Day (RTIIAD) presenta alcune delle funzioni chiave disponibili in Microsoft Fabric.

Nel menu di servizio, la sezione Guida (?) include collegamenti ad alcune risorse utili.

Di seguito sono riportate ulteriori risorse utili che consentiranno di progredire nell'uso di Microsoft Fabric.

•	Fare riferimento al post del blog per leggere l'(annuncio completo sulla disponibilità generale di Microsoft Fabric)[https://www.microsoft.com/en-us/microsoft-fabric/blog/2023/11/15/prepare-your-data-for-ai-innovation-with-microsoft-fabric-now-generally-available/]

•	Esplorare Fabric tramite la (Presentazione guidata)[https://guidedtour.microsoft.com/en-us/guidedtour/microsoft-fabric/microsoft-fabric/1/1]

•	Iscriversi alla (versione di valutazione gratuita di Microsoft Fabric)[https://www.microsoft.com/en-us/microsoft-fabric/getting-started]

•	Visitare il (sito Web di Microsoft Fabric)[https://www.microsoft.com/en-in/microsoft-fabric]

•	Acquisire nuove competenze esplorando i (moduli di apprendimento di Fabric)[https://learn.microsoft.com/en-us/training/browse/?products=fabric&resource_type=module]

•	Consultare la (documentazione tecnica di Fabric)[https://learn.microsoft.com/en-us/fabric/]

•	Leggere l'(e-book gratuito introduttivo a Fabric)[https://info.microsoft.com/ww-landing-unlocking-transformative-data-value-with-microsoft-fabric.html]

•	Unirsi alla (community di Fabric)[https://community.fabric.microsoft.com/] per pubblicare domande, condividere feedback e imparare dagli altri

Leggere i blog di annunci più approfonditi sull'esperienza Fabric:

•	(Blog sull'esperienza Data Factory in Fabric)[https://blog.fabric.microsoft.com/en-us/blog/introducing-data-factory-in-microsoft-fabric/]

•	(Blog sull'esperienza Synapse Data Engineering in Fabric)[https://blog.fabric.microsoft.com/en-us/blog/introducing-synapse-data-engineering-in-microsoft-fabric/]

•	(Blog sull'esperienza Synapse Data Science in Fabric)[https://blog.fabric.microsoft.com/en-us/blog/introducing-synapse-data-science-in-microsoft-fabric/]

•	(Blog sull'esperienza Synapse Data Warehousing in Fabric)[https://blog.fabric.microsoft.com/en-us/blog/introducing-synapse-data-warehouse-in-microsoft-fabric/]

•	(Blog sull'esperienza Real-Time Intelligence in Fabric)[https://blog.fabric.microsoft.com/en-us/blog/category/real-time-intelligence]
 
•	[Blog di annunci di Power BI](https://powerbi.microsoft.com/en-us/blog/empower-power-bi-users-with-microsoft-fabric-and-copilot/)

•	[Blog sull'esperienza Data Activator in Fabric](https://blog.fabric.microsoft.com/en-us/blog/driving-actions-from-your-data-with-data-activator/)

•	[Blog su amministrazione e governance in Fabric](https://blog.fabric.microsoft.com/en-us/blog/administration-security-and-governance-in-microsoft-fabric/)

•	[Blog su OneLake in Fabric](https://blog.fabric.microsoft.com/en-us/blog/microsoft-onelake-in-fabric-the-onedrive-for-data/)

•	[Blog sull'integrazione di Dataverse e Microsoft Fabric](https://www.microsoft.com/en-us/dynamics-365/blog/it-professional/2023/05/24/new-dataverse-enhancements-and-ai-powered-productivity-with-microsoft-365-copilot/)


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

