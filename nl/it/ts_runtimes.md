---

copyright:
  years: 2015, 2018
lastupdated: "2018-12-05"

---

{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:new_window: target="_blank"}


# Risoluzione dei problemi relativi ai runtime
{: #runtimes}

Potresti riscontrare dei problemi quando utilizzi i runtime [{{site.data.keyword.Bluemix}}](index.html). In molti casi, puoi eseguire un ripristino da questi problemi seguendo pochi semplici passi.
{:shortdesc}

* [Risoluzione di problemi generali](#ts_all)
* [Liberty for Java](#ts_liberty)
* [SDK for Node.js](#ts_nodejs)
* [ASP.NET Core](#ts_dotnet)
* [PHP](#ts_php)
* [Python](#ts_python)

## Risoluzione di problemi generali
{: #ts_all}

### Pacchetto di build obsoleto utilizzato quando viene eseguito il push di un'applicazione
{: #ts_loading_bp}

Potresti non essere in grado di utilizzare i componenti di pacchetti di build più recenti quando esegui il push di un'applicazione. Puoi utilizzare i pacchetti di build che hanno dei meccanismi integrati per impedire il caricamento di componenti obsoleti oppure puoi eliminare il contenuto nella directory cache della tua applicazione prima di eseguire il push o di preparare di nuovo l'applicazione.

Quando esegui il push o una ripreparazione di un'applicazione dopo l'aggiornamento del pacchetto di build, i componenti del pacchetto di build più recenti non vengono caricati automaticamente. Di conseguenza, la tua applicazione utilizza i componenti del pacchetto di build obsoleti dalla cache. Gli aggiornamenti applicati al pacchetto di build dall'ultima volta che avevi eseguito il push dell'applicazione non vengono implementati.
{: tsSymptoms}

Alcuni pacchetti di build non sono configurati per scaricare automaticamente tutti i componenti aggiornati da Internet per garantire che utilizzi sempre la versione più recente.
{: tsCauses}

Puoi utilizzare i pacchetti di build che hanno dei meccanismi integrati per evitare di caricare componenti obsoleti, ad esempio i seguenti pacchetti di build:
{: tsResolve}

  * [Pacchetto di build Cloud Foundry Java ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/cloudfoundry/java-buildpack){: new_window}. Questo pacchetto di build ha un meccanismo integrato per garantire che venga utilizzata la versione più recente del pacchetto di build. Per ulteriori informazioni sulla modalità di funzionamento di questo meccanismo, consulta [extending-caches.md ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/cloudfoundry/java-buildpack/blob/master/docs/extending-caches.md){: new_window}.
  * [Pacchetto di build Cloud Foundry Node.js ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/cloudfoundry/nodejs-buildpack){: new_window}. Questo pacchetto di build fornisce una funzionalità utilizzando le variabili di ambiente. Per abilitare il pacchetto di build Node.js a scaricare i moduli del nodo da Internet ogni volta, digita il seguente comando nell'interfaccia riga di comando {{site.data.keyword.Bluemix_notm}}: 	

  ```
  set NODE_MODULES_CACHE=false
  ```

Se il pacchetto di build che stai utilizzando non fornisce un meccanismo per caricare i componenti più recenti automaticamente, puoi eliminare manualmente il contenuto nella directory della cache ed eseguire nuovamente il push della tua applicazione. Utilizza la seguente procedura:

 1. Estrai mediante checkout un ramo di un pacchetto di build null, ad esempio https://github.com/ryandotsmith/null-buildpack. Per informazioni su come estrarre mediante checkout un ramo, consulta [Git Basics - Getting a Git Repository ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](http://www.git-scm.com/book/en/v2/Git-Basics-Getting-a-Git-Repository){: new_window}.  
 2. Aggiungi la seguente riga al file `null-buildpack/bin/compile` ed esegui il commit delle modifiche. Per informazioni su come eseguire il commit delle modifiche, consulta [Git Basics - Recording Changes to the Repository ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](http://www.git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository){: new_window}.
  ```
  rm -rfv $2/*
  ```
 3. Esegui il push della tua applicazione con il pacchetto di build null che è stato modificato per eliminare la cache utilizzando il seguente comando. Dopo che hai completato questo passo, tutto il contenuto nella directory cache della tua applicazione viene eliminato.
  ```
  ibmcloud cf push appname -p app_path -b <modified_null_buildpack>
  ```
 4. Esegui il push della tua applicazione con il pacchetto di build più recente che vuoi utilizzare servendoti del seguente comando:
  ```
  ibmcloud cf push appname -p app_path -b <latest_buildpack>
  ```

### L'applicazione continua a riavviarsi
{: #ts_apprestart}

L'applicazione continua ad arrestarsi in modo anomalo e a riavviarsi.
{: tsSymptoms}

Il ciclo di vita del contenitore dell'applicazione, come l'evacuazione e l'arresto, può influire sulla funzionalità della tua applicazione.  
{: tsCauses}

Identifica quale passo del ciclo di vita del contenitore dell'applicazione sta causando gli errori nella distribuzione dell'applicazione. Accedi ad ulteriori informazioni sul [ciclo di vita dell'applicazione Cloud Foundry](https://docs.cloudfoundry.org/devguide/deploy-apps/app-lifecycle.html#evacuation).
{: tsResolve}

### Il pulsante Azioni nella pagina Dettagli istanza è disabilitato (obsoleto)
  {: #ts_actionsbutton}

  Il pulsante Azioni nella pagina Dettagli istanza è disabilitato.
  {: tsSymptoms}

  Questo problema si verifica per i seguenti motivi:
  {: tsCauses}

   * L'applicazione non viene distribuita con il pacchetto di build Liberty integrato.
   * L'applicazione è stata distribuita con una versione precedente del pacchetto di build Liberty.

  Se il problema è causato da una versione precedente del pacchetto di build Liberty, ridistribuisci l'applicazione in {{site.data.keyword.Bluemix_notm}}. In caso contrario, puoi fornire i seguenti file di log dell'applicazione client al team di supporto:
  {: tsResolve}

    * logs/messages.log
    * logs/stdout.log
    * logs/stderr.log


### Le credenziali sono necessarie per aprire una finestra di traccia o di dump (obsoleto)
  {: #ts_username}

  Sono richiesti un nome utente e una password per aprire la finestra di traccia o di dump.
  {: tsSymptoms}

  Questo problema si verifica a causa del timeout della sessione di accesso.
  {: tsCauses}

  Immetti nuovamente nome utente e password.
  {: tsResolve}


### Si verifica un errore quando le operazioni di traccia o di dump sono in esecuzione (obsoleto)
  {: #ts_target}

  Viene visualizzato un messaggio di errore mentre le operazioni di traccia o di dump sono in esecuzione. Il messaggio indica che un'istanza di destinazione per un'applicazione non è nello stato in esecuzione:
  {: tsSymptoms}

  ```
  Instance 0: Trace specification is set successfully
  Instance 2: Trace specification is set successfully
  Instance 1: Target instance 1 for app abcd is not in the running state.
  Instance 3: Trace specification is set successfully
  Instance 4: Trace specification is set successfully
  ```

  Questo problema si verifica per i seguenti motivi:
  {: tsCauses}

    * Le funzionalità di gestione di dump o di traccia sono solo per le istanze dell'applicazione che sono in esecuzione. Le operazioni di traccia o di dump non possono essere utilizzate su istanze dell'applicazione che sono arrestate, in fase di avvio o arrestate in modo anomalo
    * Lo stato dell'istanza dell'applicazione sta cambiando quando la finestra di dialogo di traccia o di dump è aperta.

  Chiudi la finestra e quindi riaprila.
  {: tsResolve}


### Le istanze hanno diverse configurazioni traceSpecification (obsoleto)
  {: #ts_different_config}

  Per le diverse istanze di un'applicazione, potresti vedere delle configurazioni traceSpecification differenti.
  {: tsSymptoms}

  Questa modalità di funzionamento si verifica per i seguenti motivi:
  {: tsCauses}

    * Hai modificato la configurazione per una o più istanze precedentemente. Se modifichi la configurazione traceSpecification per un'istanza, la modifica non si applica ad altre istanze della stessa applicazione. Ad esempio, la tua applicazione utilizza log4j e hai 2 istanze per questa applicazione. Puoi modificare il livello di log dell'istanza 0 da info a debug ma il livello di log dell'istanza 1 continua a essere info.

  Non è necessaria alcuna azione. Questa modalità di funzionamento è prevista.
  {: tsResolve}


### Quota disco superata
  {: #ts_diskquota}

  Nel log dell'applicazione, potresti notare che la quota del disco è stata superata.

  Nel log della tua applicazione vedi il messaggio di errore `Disk quota exceeded`.
  {: tsSymptoms}

  Questo problema si verifica per uno dei seguenti motivi:
  {: tsCauses}

    * I file di dump vengono generati con le istanze dell'applicazione in esecuzione e i file utilizzano la quota di disco assegnata. Per impostazione predefinita, la quota del disco per un'istanza dell'applicazione è 1 GB. Puoi controllare l'utilizzo del disco facendo clic su **Dashboard> Applicazione> Runtime applicazione**. Il seguente esempio mostra le informazioni di runtime, incluso l'utilizzo di disco, per due istanze di un'applicazione:
      ```
      Instance	State	CPU	Memory Usage	Disk Usage

  	0		Running	1.0%	344.8MB/512MB	236.8MB/1GB
  	2		Running	2.3%	361.2MB/512MB	235.7MB/1GB
      ```
    * La quota del disco è limitata dalla quota dell'organizzazione corrente.

  Utilizza uno dei seguenti metodi:
  {: tsResolve}

    * Elimina i file di dump dopo che sono stati scaricati.
    * Ridistribuisci l'applicazione con una quota di disco più grande includendo la seguente voce nel manifest di distribuzione:
      ```
      disk_quota: 2048
      ```

## Liberty for Java
{: #ts_liberty}

### L'applicazione non riesce ad iniziare ad accettare le connessioni
{: #health_check_timeout}

L'avvio di un'applicazione Liberty non riesce con un errore "_Failed to start accepting connections_". Ad esempio, l'errore nei log può essere simile al seguente:
{: tsSymptoms}

```
   2016-11-14T14:44:58.45+0000 [API/0]      OUT App instance exited with guid 21ac63eb-9595-428a-94c7-b0b02aaf77cc payload: {"cc_partition"=>"default", "droplet"=>"21ac63eb-9595-428a-94c7-b0b02aaf77cc", "version"=>"b2772438-92de-4d47-b680-ea772ac2288a", "instance"=>"f4799c8c89214bbd8067883c3ffe23e0", "index"=>0, "reason"=>"CRASHED", "exit_status"=>255, "exit_description"=>"failed to accept connections within health check timeout", "crash_timestamp"=>1479134698} 2016-11-14T14:45:07.50+0000 [DEA/4]      ERR Instance (index 0) failed to start accepting connections
```
{: codeblock}

{{site.data.keyword.Bluemix_notm}} esegue un controllo di integrità sull'applicazione per controllare se è stata avviata correttamente. Il controllo di integrità verifica se l'applicazione è in ascolto sulla porta assegnata all'applicazione. Il timeout predefinito per questo controllo è di 60 secondi e alcune applicazioni possono impiegare più di 60 secondi ad avviarsi.  Esistono molti motivi per cui l'applicazione può impiegare molto tempo ad avviarsi. Ad esempio, l'associazione mediante bind di servizi quali [New Relic](/docs/runtimes/liberty/monitoring/newRelic.html) aumenterà il tempo di avvio. L'applicazione potrebbe inoltre eseguire i passi di inizializzazione che possono impiegare molto tempo per il completamento.
{: tsCauses}

Per prima cosa, esamina i log per ogni errore ovvio che può aver causato il malfunzionamento dell'applicazione Liberty. Se non è stato trovato alcun errore ovvio tenta quanto segue:
{: tsResolve}

#### Aumenta il timeout del controllo di integrità

* Quando distribuisci l'applicazione utilizzando il comando ` ibmcloud cf push `, specifica un timeout di avvio dell'applicazione più lungo utilizzando l'opzione `-t`. Ad esempio:

        ```
	ibmcloud cf push myApp -t 180
        ```
	{: codeblock}

* Il timeout del controllo di integrità può inoltre essere specificato nel file manifest.yml. Ad esempio:

        ---
           ...
           timeout: 180
        {: codeblock}

#### Disabilita la funzione appstate

La funzione appstate si integra con il processo di verifica dell'integrità di {{site.data.keyword.Bluemix_notm}} per garantire che l'applicazione Liberty venga inizializzata completamente prima di poter ricevere richieste HTTP. Come l'applicazione è completamente inizializzata, la funzione appstate non ha più effetto.  L'effetto collaterale di questa funzione è che alcune applicazioni potrebbero richiedere più tempo per avviarsi. Per disabilitare la funzione appstate, imposta la seguente proprietà di ambiente sulla tua applicazione e ripreparala:

```
ibmcloud cf set-env myApp JBP_CONFIG_LIBERTY "app_state: false"
```
{: codeblock}

#### Considera di rieseguire il factoring dell'applicazione

Se la tua applicazione impiega troppo tempo ad inizializzarsi, potresti dover rieseguire il factoring dell'applicazione per effettuare un'inizializzazione asincrona e/o lenta.

#### Distribuisci con l'opzione "no-route"

1. Distribuisci la tua applicazione con l'opzione "--no-route". Questo disabiliterà il controllo di integrità della porta. Ad esempio:

        ```
	ibmcloud cf push myApp –no-route
        ```
	{: codeblock}

2. Dopo aver inizializzato l'applicazione, associa una rotta all'applicazione. Ad esempio:

        ibmcloud cf map-route myApp mybluemix.net
        {: codeblock}

### Errori SSL con il gateway di IBM
{: #ssl_handshake_failure}


I seguenti errori sono visualizzati nei log e l'applicazione potrebbe non avviarsi:
{: tsSymptoms}

```
    2016-11-03T12:32:44.82-0200 [App/0]      ERR java.security.cert.CertPathValidatorException: Certificate chaining error 2016-11-03T12:32:44.83-0200 [App/0]      ERR [ERROR   ] CWPKI0022E: SSL HANDSHAKE FAILURE:  A signer with SubjectDN CN=*.gateway.prd.na.ca.ibmserviceengage.com, O=International Business Machines Corp., L=Armonk, ST=New York, C=US was sent from the target host.  The signer might need to be added to local trust store /home/vcap/app/wlp/usr/servers/defaultServer/resources/security/key.jks, located in SSL configuration alias defaultSSLConfig.  The extended error message from the SSL handshake exception is: PKIX path building failed: java.security.cert.CertPathBuilderException: PKIXCertPathBuilderImpl could not build a valid CertPath.; internal cause is: 2016-11-03T12:32:44.83-0200 [App/0]      ERR java.security.cert.CertPathValidatorException: The certificate issued by CN=DigiCert Global Root CA, OU=www.digicert.com, O=DigiCert Inc, C=US is not trusted; internal cause is: 2016-11-03T12:32:44.83-0200 [App/0]      ERR java.security.cert.CertPathValidatorException: Certificate chaining error
```
{: codeblock}

Gli errori possono essere stati generati quando un servizio sicuro viene associato mediante bind a un'applicazione Liberty e tale applicazione è stata distribuita come una directory del server o un server compresso che contiene il file server.xml che configura la funzione Liberty ssl-1.0. L'associazione mediante bind del servizio sicuro all'applicazione Liberty fa in modo che il runtime si connetta al servizio su una connessione sicura. Tale connessione sicura viene stabilita utilizzando le impostazioni SSL predefinite. Poiché, le impostazioni SSL predefinite vengono specificate nel file server.xml di Liberty, il truststore configurato potrebbe non ritenere attendibile il certificato utilizzato dal servizio sicuro.
{: tsCauses}

Modifica la configurazione per utilizzare il truststore di JVM con una delle seguenti opzioni.  Assicurati di ripreparare la tua applicazione dopo aver effettuato la modifica.
{: tsResolve}

#### Aggiorna il server.xml di Liberty

Aggiorna il server.xml in modo che utilizzi il file cacerts di JVM come truststore. Aggiungi quanto segue al tuo file server.xml:

        <ssl id="defaultSSLConfig" trustStoreRef="defaultTrustStore"/>
        <keyStore id="defaultTrustStoretore" location="${java.home}/lib/security/cacerts"/>
        {: codeblock}

#### Aggiorna il truststore configurato

Modifica il truststore configurato per rendere attendibile la CA RADICE DigitCert.
  1. Scarica la CA radice da https://www.digicert.com/CACerts/DigiCertGlobalRootCA.crt.
  2. Supponendo che viene utilizzato resources/security/key.jks come truststore, importa la CA nella chiave utilizzando il programma di utilità dello strumento chiave di Java.

            keytool -importcert --storepass <keyStorePassword> -keystore &lt;path&gt;/resources/security/key.jks -file DigiCertGlobalRootCA.crt
            {: codeblock}


## SDK for Node.js
{: #ts_nodejs}

### Impossibile avviare l'applicazione: risoluzione dei problemi generale

Per il pacchetto di build {{site.data.keyword.runtime_nodejs_notm}} V3.23 o successive, prova a eseguire il vendoring delle tue dipendenze come primo passo per la risoluzione dei problemi. Eseguire il vendoring delle dipendenze significa impacchettare le dipendenze negli stessi file di origine della tua applicazione. Tale operazione può risolvere diversi errori che possono verificarsi quando le dipendenze presumono che ti trovi nella stessa directory di base dell'applicazione.

1. Dalla tua directory root dell'applicazione, installa le dipendenze eseguendo il comando di seguito indicato.

   ```bash
   npm install
   ```
   {: codeblock}
1. Assicurati che il tuo file `.cfignore` non includa la seguente riga:

   ```
   node_modules/
   ```

Ora, quando distribuisci la tua applicazione con il comando `ibmcloud cf push`, invece di essere scaricate in una ubicazione separata, le tue dipendenze vengono copiate nella stessa directory del resto dell'applicazione.

### Impossibile avviare l'applicazione con un errore di spazio insufficiente sul dispositivo
{: #no_space_left_on_device}


Impossibile avviare un'applicazione Node.js con un errore di spazio insufficiente sul dispositivo. Ad esempio, l'errore nei log può essere simile al seguente:
{: tsSymptoms}

```
   2017-01-16T14:25:14.61-0500 [CELL/0]     ERR tar: ./app/node_modules/pm2/node_modules/cron/node_modules/moment-timezone/LICENSE: Cannot write: No space left on device

```
{: codeblock}

Le applicazioni Node.js che utilizzano le versioni di NPM precedenti alla 3 utilizzano più spazio per lo scaricamento delle dipendenze.
{: tsCauses}

Modifica il file package.json della tua applicazione per utilizzare la versione 3 di NPM o una superiore.
{: tsResolve}

```
{
  "name": "myapp",
  "description": "this is my app",
  "version": "0.1",
  "engines": {
     "node": "4.2.4",
     "npm": "3.10.10"
  }
}
```
{: codeblock}

### L'applicazione viene riavviata a causa di vincoli di memoria
{: #oom}

Node.js non conosce quanta memoria è disponibile per l'applicazione, per cui il raccoglitore dei dati inutilizzati potrebbe non essere stato eseguito prima dell'esaurimento della memoria.

```
2017-09-01T11:00:42.19-0400 [APP/PROC/WEB/0]OUT Exit status 137
2017-09-01T11:00:42.23-0400 [CELL/0]     OUT Exit status 0
2017-09-01T11:00:42.27-0400 [CELL/0]     OUT Destroying container
2017-09-01T11:00:42.34-0400 [API/0]      OUT Process has crashed with type: "web"
2017-09-01T11:00:42.36-0400 [API/0]      OUT App instance exited with guid eecfba3b-430c-4a6b-b71f-ac72816fe152 payload: {"instance"=>"77dbb981-16d0-3a05-3235-9a4b", "index"=>0, "reason"=>"CRASHED", "exit_description"=>"2 error(s) occurred:\n\n* 2 error(s) occurred:\n\n* Exited with status 137 (out of memory)\n* cancelled\n* cancelled", "crash_count"=>1, "crash_timestamp"=>1504278042244633291, "version"=>"6497b5b5-67d4-4c5a-b1af-362e522a029d"}
2017-09-01T11:00:43.35-0400 [CELL/0]     OUT Successfully destroyed container
```
{: codeblock}

Una possibile soluzione è di impostare l'opzione `--max_old_space_size` nel comando di avvio dell'applicazione nel file package.json. Questa opzione rappresenta parte del footprint della memoria dell'applicazione e dovrebbe essere impostata su un valore inferiore al totale di memoria disponibile per l'applicazione. Per ulteriori informazioni, consulta [Large memory spikes and Heroku](https://github.com/nodejs/node/issues/3370) per una discussione più dettagliata su questo argomento.
```
  "scripts": {
    "start": "node --max_old_space_size=800 server.js"
  }
```
{: codeblock}

## ASP.NET Core
{: #ts_dotnet}

La distribuzione dell'applicazione non riesce con il messaggio: `API/0App instance exited ... payload: {... "reason"=>"CRASHED", "exit_status"=>-1, ...}`.
{: tsSymptoms}

Se ricevi un messaggio simile quando esegui il push della tua applicazione ASP.net, molto probabilmente la tua applicazione ha superato i limiti di quota di memoria o del disco.  Aumenta le quote per la memoria o lo spazio su disco per la tua applicazione.
{: tsCauses}

La distribuzione dell'applicazione non riesce con il messaggio: `Failed to compress droplet: signal: broken pipe` o `No space left on device`.  In che modo è possibile correggere l'errore?
{: tsSymptoms}

I progetti di cui viene eseguito il push dal codice sorgente che contiene un ampio numero di dipendenze del pacchetto NuGet possono a volte causare questo errore quando la cache dei pacchetti NuGet è abilitata.  Imposta la variabile di ambiente `CACHE_NUGET_PACKAGES` su `false` per disabilitare la cache. Per ulteriori informazioni, consulta le istruzioni su come [Disabilitare il pacchetto NuGet](/docs/runtimes/dotnet/disablingNuGet.md).
{: tsCauses}

### Link utili
* [NuGet](https://docs.nuget.org/Consume/Overview){: new_window}
* [Panoramica ASP.NET Core](http://docs.asp.net/en/latest/conceptual-overview/aspnet.html){: new_window}

## PHP
{: #ts_PHP}

### Messaggi NOTICE dal pacchetto di build PHP
{: #ts_phplog}

Potresti visualizzare dei messaggi di log che contengono NOTICE. Puoi interrompere la registrazione di questi messaggi modificando il livello di registrazione.

Quando esegui il push di un'applicazione a {{site.data.keyword.Bluemix_notm}} utilizzando un pacchetto di build PHP, potresti vedere dei messaggi che contengono `NOTICE`:
{: tsSymptoms}

```
• 2015-01-26T15:00:59.70+0100 [App/0] ERR [26-Jan-2015 14:00:59] NOTICE: [pool www] 'user' directive is ignored when FPM is not running as root
• 2015-01-26T15:01:00.63+0100 [App/0] ERR [26-Jan-2015 14:00:59] NOTICE: [pool www] 'user' directive is ignored when FPM is not running as root
• 2015-01-26T15:01:00.63+0100 [App/0] ERR [26-Jan-2015 14:00:59] NOTICE: fpm is running, pid 93
• 2015-01-26T15:01:00.63+0100 [App/0] ERR [26-Jan-2015 14:00:59] NOTICE: ready to handle connections
```
Nel pacchetto di build PHP, il parametro error_log definisce il livello di registrazione. Per impostazione predefinita, il valore del parametro `error_log` è **stderr notice**. Il seguente esempio mostra la configurazione del livello di registrazione predefinita nel file `nginx-defaults.conf` del pacchetto di build PHP fornito da Cloud Foundry. Per ulteriori informazioni, consulta [cloudfoundry/php-buildpack ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/cloudfoundry/php-buildpack/blob/ff71ea41d00c1226d339e83cf2c7d6dda6c590ef/defaults/config/nginx/1.5.x/nginx-defaults.conf){: new_window}.
{: tsCauses}

```
daemon off;
error_log stderr notice;
pid @{HOME}/nginx/logs/nginx.pid;
```

I messaggi ` NOTICE ` sono informativi e potrebbero non indicare un problema. Puoi arrestare la registrazione di questi messaggi modificando il livello di registrazione da `stderr notice` a `stderr error` nel file nginx-defaults.conf del tuo pacchetto di build. Ad esempio: 	
{: tsResolve}

```
daemon off;
error_log stderr error;
pid @{HOME}/nginx/logs/nginx.pid;
```
Per ulteriori informazioni su come modificare la configurazione di registrazione predefinita, consulta [error_log ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](http://nginx.org/en/docs/ngx_core_module.html#error_log){: new_window}.


## Python
{: #ts_python}

### Impossibile importare una libreria Python di terze parti in {{site.data.keyword.Bluemix_notm}}
{: #ts_importpylib}

Potresti non riuscire a importare una libreria Python di terze parti in {{site.data.keyword.Bluemix_notm}}. Per risolvere il problema, aggiungi i file di configurazione alla directory root della tua applicazione python.

Quando tenti di importare una libreria Python di terze parti, come ad esempio la libreria `web.py`, il comando `ibmcloud cf push` ha esito negativo.
{: tsSymptoms}

Mancano le informazioni di configurazione per l'applicazione Python.
{: tsCauses}

Aggiungi un file `requirements.txt` e un file `Procfile` alla directory root della tua applicazione Python. Le seguenti informazioni presumono che
si stia importando la libreria `web.py`:
{: tsResolve}

 1. Aggiungi un file `requirements.txt` alla directory root della tua applicazione Python.

 Il file `requirements.txt` specifica i pacchetti della libreria richiesti per la tua applicazione Python e la versione dei pacchetti. Il seguente esempio mostra il contenuto del file `requirements.txt`, dove `web.py==0.37` indica che la versione della libreria `web.py` che verrà scaricata è la 0.37 e `wsgiref==0.1.2` indica che la versione dell'interfaccia gateway del server Web richiesta dalla libreria web.py è la 0.1.2.
	 ```
	 web.py==0.37
     wsgiref==0.1.2
	 ```
	 Per ulteriori informazioni su come configurare il file `requirements.txt`, consulta [Requirements files](https://pip.readthedocs.org/en/1.1/requirements.html).

 2. Aggiungi un file `Procfile` alla directory root della tua applicazione Python.
 Il file `Procfile` deve contenere il comando di avvio per la tua applicazione Python. Nel seguente comando, *yourappname* è il nome della tua applicazione Python e *PORT* è il numero porta che la tua applicazione Python deve utilizzare per ricevere le richieste dagli utenti dell'applicazione. *$PORT* è facoltativo. Se non specifichi PORT nel comando di avvio, viene utilizzato il numero porta nella variabile di ambiente `VCAP_APP_PORT` che si trova nell'applicazione.
	```
	web: python <yourappname>.py $PORT
	```

Puoi ora importare la libreria Python di terze parti in {{site.data.keyword.Bluemix_notm}}.
