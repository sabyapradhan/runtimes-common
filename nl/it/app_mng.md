---

copyright:
  years: 2015, 2018
lastupdated: "2018-10-09"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}

# Gestione di applicazioni Liberty e Node.js
{: #app_management}


La Gestione applicazioni è un insieme di programmi di utilità per lo sviluppo e il debug che è possibile abilitare per le tue applicazioni Liberty su {{site.data.keyword.Bluemix}}.
{:shortdesc}

## Programmi di utilità di Gestione applicazioni
{: #Utilities}

### Programmi di utilità Liberty
* [proxy](#proxy)
* [noproxy](#noproxy)
* [devconsole](#devconsole)
* [hc](#hc)
* [shell](#shell)
* [debug](#debug)
* [jmx](#jmx)
* [localjmx](#localjmx)

### Programmi di utilità Node.js (obsoleto)

**Nota di obsolescenza**: i programmi di utilità di Gestione applicazioni sono obsoleti per le applicazioni Node.js. Alcuni programmi di utilità che erano disponibili sia per le applicazioni Node.JS che per quelle Liberty sono ora disponibili solo per le applicazioni Liberty. Questi programmi di utilità sono obsoleti per le applicazioni Node.js.

* [proxy](#proxy)
* [noproxy](#noproxy)
* [devconsole](#devconsole)
* [hc](#hc)
* [shell](#shell)
* [inspector](#inspector)
* [trace](#trace)

## Come configurare Gestione applicazioni
{: #configure}

Per abilitare il programma di utilità Gestione applicazioni, imposta il valore della variabile di ambiente *BLUEMIX_APP_MGMT_ENABLE* sul programma di utilità o sull'elenco dei programmi di utilità che vuoi abilitare, quindi riprepara la tua applicazione. Puoi abilitare più programmi di utilità separandoli con un **+**.

Ad esempio, per abilitare i programmi di utilità *hc*, *debug* e *trace*, immetti il seguente comando:

```
ibmcloud cf set-env myApp BLUEMIX_APP_MGMT_ENABLE hc+debug+trace
```
{: codeblock}

Prepara di nuovo la tua applicazione dopo aver impostato la variabile di ambiente:

```
ibmcloud cf restage myApp
```
{: codeblock}

Se non vuoi che i programmi di utilità Gestione applicazioni siano installati con la tua applicazione, imposta
la variabile di ambiente *BLUEMIX_APP_MGMT_INSTALL* su 'false' e prepara di nuovo la tua applicazione.

Ad esempio, immetti i seguenti comandi per preparare la tua applicazione senza il programma di utilità Gestione applicazioni:

```
ibmcloud cf set-env myApp BLUEMIX_APP_MGMT_INSTALL false
ibmcloud cf restage myApp
```
{: codeblock}

## Limitazioni
{: #restrictions}
* Le modifiche che apporti alla tua applicazione utilizzando Gestione applicazioni sono transitorie e vanno perse dopo che esci da questa modalità. Questa modalità serve solo per un uso di sviluppo temporaneo e non è prevista per l'utilizzo in un ambiente di produzione per via delle prestazioni.
* Per le applicazioni Node.js, la maggior parte dei programmi di utilità di Gestione applicazioni non funziona se hai impostato il tuo comando **start** nel file `manifest.yml` o con l'opzione `-c` nella riga di comando. Questi metodi sono delle sostituzioni del pacchetto di build e rappresentano l'antimodello per l'avvio delle applicazioni Node.js. Per dei risultati ottimali, imposta il comando **start** nel file `package.json` o `Procfile`.


### Programmi di utilità Liberty
{: #liberty_utilities}

#### jmx
{: #jmx}

Il programma di utilità *jmx* abilita il connettore JMX REST per consentire a un client JMX remoto di gestire l'applicazione utilizzando le credenziali utente di {{site.data.keyword.Bluemix_notm}}.

Puoi monitorare più istanze di un'applicazione utilizzando JMX, ma richiede una connessione JMX separata per ogni istanza. Il valore predefinito è di monitorare l'istanza 0. Per monitorare l'istanza 1, puoi utilizzare il seguente frammento di codice:

```
ibmcloud cf ssh -i 1 -N -T -L 5000:127.0.0.1:5001
```
{: codeblock}

Per ulteriori informazioni sulla configurazione di un connettore JMX, vedi [Configuring secure JMX connection to the Liberty profile ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www-01.ibm.com/support/knowledgecenter/was_beta_liberty/com.ibm.websphere.wlp.nd.multiplatform.doc/ae/twlp_admin_restconnector.html){:new_window}.

**Importante**: il programma di utilità *jmx* non avvia *proxy*.

#### localjmx
{: #localjmx}

Il programma di utilità *localjmx* abilita la funzione [localConnector-1.0 ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](http://www.ibm.com/support/knowledgecenter/SSEQTP_liberty/com.ibm.websphere.wlp.doc/ae/rwlp_feature_localConnector-1.0.html){:new_window} Liberty. Combinata con l'inoltro alla porta locale, *localjmx* agisce come un modo alternativo per consentire a un client JMX remoto di gestire l'applicazione.


**Prima di iniziare**: *localjmx* ti richiede di installare JConsole.

Il programma di utilità *localjmx* si applica solo alle applicazioni in esecuzione in una cella Diego. Per utilizzare *localjmx*, stabilisci prima l'inoltro alla porta utilizzando il comando `ibmcloud cf ssh`. Ad esempio:

```
ibmcloud cf ssh -N -T -L 5000:127.0.0.1:5000 <appName>
```
{: codeblock}

Successivamente, per il collegamento alla JConsole, scegli **Processo remoto**, specifica `127.0.0.1:5000` e utilizza una connessione non sicura.

#### proxy (obsoleto per Node.js)
{: #proxy}

Il programma di utilità *proxy* fornisce la gestione dell'applicazione minima tra la tua applicazione e {{site.data.keyword.Bluemix_notm}}.

Quando abilitato, il pacchetto di build avvia un agent proxy ubicato tra il runtime e il contenitore della tua applicazione.  Il programma di utilità *proxy* gestisce tutte le richieste ricevute dall'applicazione. In base al tipo di richiesta, effettua un'azione di Gestione applicazione o inoltra la richiesta alla tua applicazione. Utilizzando *proxy*, il tuo contenitore dell'applicazione continua a funzionare, anche se l'applicazione si arresta in modo anomalo. L'agent proxy consente inoltre aggiornamenti di file incrementali, che abilitano la modalità *Live Edit* per le applicazioni Node.js.

Alcuni programmi di utilità Gestione applicazione ti richiedono di utilizzare il programma di utilità *proxy* con la tua applicazione e possono avviare *proxy* automaticamente.

**Nota di obsolescenza**: per Node.js, la versione {{site.data.keyword.Bluemix_notm}} di Cloud Foundry viene eseguita in celle Diego. La funzione *noproxy* è per celle non Diego.

#### noproxy (obsoleto per Node.js)
{: #noproxy}

Il programma di utilità *noproxy* disabilita il programma di utilità *proxy* quando viene automaticamente avviato da un altro programma di utilità.  Con Diego, il proxy non è necessario perché Diego fornisce la funzionalità *ssh* direttamente alla tua applicazione e configura l'inoltro alla porta.

Il programma di utilità *noproxy* si applica solo alle applicazioni in esecuzione in una cella Diego.

**Nota di obsolescenza**: il programma di utilità *noproxy* disabilita *proxy*. Poiché *proxy* è obsoleto per Node.js e non funziona più, *noproxy* non è necessario.

#### devconsole (obsoleto per Node.js)
{: #devconsole}

Gli utenti possono riavviare, arrestare o sospendere le proprie applicazioni con il programma di utilità della console di sviluppo (*devconsole*). Gli utenti possono anche abilitare o accedere ai programmi di utilità shell e inspector utilizzando *devconsole*.  Puoi utilizzare il seguente URL per accedere a *devconsole*:
```
  https://<yourappname>.mybluemix.net/bluemix-debug/manage
```
{: codeblock}

Per Node versione 6.3.0 o superiore, la console di sviluppo fornisce un pulsante di riavvio per la tua applicazione e l'accesso al programma di utilità *shell*.  Per ulteriori informazioni, vedi la discussione su *inspector*.

**Importante**: il programma di utilità *devconsole* avvia *proxy*.

**Nota di obsolescenza**: invece di utilizzare il programma di utilità *devconsole* per Node.js, utilizza la console {{site.data.keyword.Bluemix_notm}}. Dalla console, vai alla pagina **Runtime** dell'applicazione. Dalla pagina **Runtime**, puoi arrestare, avviare, rinominare ed eliminare l'applicazione. Puoi inoltre accedere alla shell e ad altre informazioni.

#### hc (obsoleto per Node.js)
{: #hc}

L'agent Health Center (*hc*) abilita il monitoraggio della tua applicazione mediante il client Health Center.  Per Node.js, l'agent *hc* è disponibile solo con le versioni di runtime di Node.js incluse con il pacchetto di build IBM SDK for Node.js.  Consulta [Aggiornamenti più recenti al pacchetto di build sdk-for-nodejs](/docs/runtimes/nodejs/updates.html) per la serie corrente dei runtime.

Quando l'agent Health Center è abilitato, puoi analizzare le prestazioni delle tue applicazioni Liberty e Node.js utilizzando gli strumenti IBM Monitoring and Diagnostic. Per ulteriori informazioni, vedi [How to analyze the performance of Liberty Java or Node.js apps in {{site.data.keyword.Bluemix_notm}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://developer.ibm.com/bluemix/2015/07/03/how-to-analyze-performance-in-bluemix/){:new_window}.

**Importante:** il programma di utilità *hc* avvia *proxy*.

**Utilizzo di *hc* con *noproxy* **

Il programma di utilità *hc* può essere utilizzato insieme a *noproxy*. Per utilizzare Health Center con *noproxy*, stabilisci prima l'inoltro alla porta utilizzando il comando `ibmcloud cf ssh`. Ad esempio:

```
ibmcloud cf ssh -N -T -L 1883:127.0.0.1:1883 <appName>
```
{: codeblock}

Successivamente, per il collegamento al client Health Center, utilizza [MQTT connection ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](http://www.ibm.com/support/knowledgecenter/SS3KLZ/com.ibm.java.diagnostics.healthcenter.doc/topics/connectingtojvm.html){: new_window} e specifica l'host come `127.0.0.1` e la porta come `1883`.

**Nota di obsolescenza** invece di utilizzare l'agent Health Center per Node.js, utilizza la console {{site.data.keyword.Bluemix_notm}}. Dalla console, vai alla pagina **Runtime** dell'applicazione, dove puoi accedere all'utilizzo della CPU, all'utilizzo della memoria e allo spazio su disco. La pagina presenta anche una scheda dove puoi accedere a qualsiasi variabile di ambiente.

#### shell (obsoleto per Node.js)
{: #shell}

Il programma di utilità *shell* abilita una shell basata su Web.  Puoi accedere a *shell* dal programma di utilità *devconsole* o con il seguente URL:

```
  https://<yourappname>.mybluemix.net/bluemix-debug/shell
```
{: codeblock}

Dopo aver effettuato l'accesso al programma di utilità *shell* viene visualizzata una finestra di terminale con l'accesso shell nella tua applicazione. Puoi effettuare tutte le attività supportate in una normale shell, ad esempio modificare i file, controllare l'utilizzo della memoria o eseguire i comandi di diagnostica.

**Importante:** anche il programma di utilità *shell* avvia *proxy*.

Diego fornisce una shell interattiva tramite il comando `ibmcloud cf ssh`, per cui il programma di utilità *shell* è utile solo alle applicazioni in esecuzione su un DEA.
{: .tip}

**Nota di obsolescenza:** per Node.js, sebbene il comando `cf ssh` continui a funzionare, puoi accedere alla shell anche dalla console {{site.data.keyword.Bluemix_notm}}. Dalla console, vai alla pagina **Runtime** dell'applicazione, dove puoi trovare una scheda per visualizzare una shell nella pagina web.

### Programmi di utilità Node.js (obsoleto)
{: #node_utilities}

#### inspector (obsoleto)
{: #inspector}

Il programma di utilità inspector può essere utilizzato per creare profili di utilizzo CPU, aggiungere punti di interruzione ed eseguire il debug del codice, il tutto mentre la tua applicazione è in esecuzione su {{site.data.keyword.cloud_notm}}.  Per le versioni di Node.js precedenti alla 6.3.0, inspector abilita l'interfaccia del programma di debug Node inspector.  Per ulteriori informazioni su Node inspector, vedi il readme per [node-inspector on GitHub ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/node-inspector/node-inspector){: new_window}.  Per Node.js versioni 6.3.0 e successive, *inspector* utilizza [V8 Inspector Integration for Node.js ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://nodejs.org/dist/latest-v6.x/docs/api/debugger.html#debugger_v8_inspector_integration_for_node_js){:new_window}.

##### Per le versioni di Node.js successive alla 6.3.0
Quando avvii la modalità di debug, *proxy* viene automaticamente abilitato, anche se utilizzi una versione di Node.js che non include *proxy*. Le versioni di Node.js successive alla 6.3.0 non includono *proxy.* Se utilizzi il programma di utilità *inspector* con versioni di Node.js successive alla 6.3.0, puoi disattivare *proxy* utilizzando *noproxy.*

Invece di utilizzare *proxy* per accedere all'interfaccia *inspector*, utilizza la funzionalità Developer Tools del browser web Google Chrome.  

Abilita l'accesso all'URL con l'inoltro della porta locale con il seguente comando:
```
ibmcloud cf ssh -N -T -L 9229:127.0.0.1:9229 <appName>
```
{: codeblock}

Richiama il log di avvio dell'applicazione utilizzando il seguente comando:
```
ibmcloud cf logs <appName> --recent
```
{: codeblock}

Se il programma di utilità *inspector* è attivo, il log contiene dei messaggi simili ai seguenti:
```
 ... You will need a SSH tunnel for port 9229 to be able to use the Chrome DevTools to remotely debug your app
 ... Starting app with 'node --inspect=9229  app.js '
```
{: codeblock}

Utilizza una versione aggiornata del browser web Google Chrome per passare a `chrome://inspect`.
Da questo URL, vedi la tua applicazione elencata insieme a un link ai tuoi file dell'applicazione come ad esempio `file://home/vcap/app/app.js`., quindi seleziona **inspect** per accedere all'interfaccia di inspect.

##### Per le versioni di Node.js antecedenti alla 6.3.0
Se utilizzi *proxy,* puoi accedere all'interfaccia *inspector* all'indirizzo `https://myApp.mybluemix.net/bluemix-debug/inspector`.

Se non utilizzi il programma di utilità *proxy*, abilita l'accesso all'URL dell'applicazione utilizzando l'inoltro alla porta locale con il seguente comando:

```
ibmcloud cf ssh -N -T -L 8790:127.0.0.1:8790 <appName>
```
{: codeblock}

Quindi, accedi a inspector dall'URL, `http://127.0.0.1:8790`.

**Nota di obsolescenza:** puoi ancora utilizzare la funzione *inspector* modificando leggermente la tua configurazione. Nel file `manifest.yml` della tua applicazione, imposta il comando start su 'node --inspect=9229 app.js'. Puoi quindi attenerti alle istruzioni per le versioni di Node.js successive alla 6.3.0. Tuttavia, i log che normalmente indicano quando l'inspector è attivo non sono visibili, ma sta funzionando correttamente.


#### trace (obsoleto)
{: #trace}

Il programma di utilità trace ti consente di impostare in modo dinamico i livelli di traccia se la tua applicazione utilizza i moduli di registrazione log4js, ibmbluemix o bunyan.

Passa alla pagina **Dettagli istanza** nella console web {{site.data.keyword.cloud_notm}} e seleziona **Azioni** per visualizzare la IU.

Nota: versioni supportate per le dipendenze:

* log4js:(0.6.0 - 0.6.24)
* bunyan: (1.0.0, 1.0.1, 1.1.0 - 1.1.3, 1.2.0 - 1.2.3, 1.3.0 - 1.3.5)
* ibmbluemix: (1.0.0-20140707-1250)-(1.0.0-20150409-1328)


Il programma di utilità trace non è disponibile quando l'applicazione si avvia utilizzando l'opzione `-b buildpack`.

Il programma di utilità trace non avvia *proxy*.
