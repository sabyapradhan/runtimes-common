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


# Fehlerbehebung für Laufzeiten
{: #runtimes}

Bei der Verwendung von [{{site.data.keyword.Bluemix}}-Laufzeiten](index.html) können möglicherweise Probleme auftreten. In vielen Fällen können Sie diese Probleme durch einige einfache Schritte beheben.
{:shortdesc}

* [Allgemeine Fehlerbehebung](#ts_all)
* [Liberty for Java](#ts_liberty)
* [SDK for Node.js](#ts_nodejs)
* [ASP.NET Core](#ts_dotnet)
* [PHP](#ts_php)
* [Python](#ts_python)

## Allgemeine Fehlerbehebung
{: #ts_all}

### Verwendung eines veralteten Buildpacks bei Push-Übertragung einer App
{: #ts_loading_bp}

Sie können möglicherweise nicht die neuesten Buildpackkomponenten verwenden, wenn Sie eine App durch eine Push-Operation übertragen. Sie können Buildpacks verwenden, die integrierte Mechanismen besitzen, die das Laden veralteter Komponenten verhindern, oder Sie können den Inhalt des Cacheverzeichnisses Ihrer App löschen, bevor Sie für die App eine Push-Operation oder ein erneutes Staging durchführen.

Wenn Sie eine Push-Operation oder ein erneutes Staging für eine App durchführen, nachdem das Buildpack aktualisiert wurde, werden die neuesten Buildpackkomponenten nicht automatisch geladen. Infolgedessen verwendet Ihre App die veralteten Buildpackkomponenten aus dem Cache. Aktualisierungen, die auf das Buildpack seit der letzten Push-Operation der App angewendet wurden, werden nicht implementiert.
{: tsSymptoms}

Einige Buildpacks sind nicht dazu konfiguriert, automatisch alle aktualisierten Komponenten aus dem Internet herunterzuladen, um sicherzustellen, dass Sie immer die neueste Version verwenden.
{: tsCauses}

Sie können Buildpacks, die integrierte Mechanismen besitzen, durch die das Laden veralteter Komponenten verhindert wird, wie zum Beispiel die folgenden verwenden:
{: tsResolve}

  * [Cloud Foundry-Java-Buildpack ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/cloudfoundry/java-buildpack){: new_window}. Dieses Buildpack hat einen integrierte Mechanismus, der sicherstellt, dass die neueste Version des Buildpacks verwendet wird. Weitere Informationen zur Funktionsweise dieses Mechanismus finden Sie im Abschnitt über das [Erweitern von Caches ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/cloudfoundry/java-buildpack/blob/master/docs/extending-caches.md){: new_window}.
  * [Cloud Foundry-Node.js-Buildpack ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/cloudfoundry/nodejs-buildpack){: new_window}. Dieses Buildpack stellt ähnliche Funktionalität durch die Verwendung von Umgebungsvariablen bereit. Um das Node.js-Buildpack so einzurichten, dass es jedes Mal Knotenmodule aus dem Internet herunterlädt, geben Sie den folgenden Befehl in die {{site.data.keyword.Bluemix_notm}}-Befehlszeilenschnittstelle ein: 	

  ```
  set NODE_MODULES_CACHE=false
  ```

Wenn das von Ihnen verwendete Buildpack keinen Mechanismus zum automatischen Laden der neuesten Komponenten bereitstellt, können Sie den Inhalt im Cacheverzeichnis manuell löschen und Ihre App erneut durch eine Push-Operation übertragen. Führen Sie die folgenden Schritte aus:

 1. Checken Sie einen Zweig eines Null-Buildpacks aus. Beispiel: https://github.com/ryandotsmith/null-buildpack. Informationen dazu, wie Sie einen Zweig (Branch) auschecken, finden Sie unter [Git Basics - Getting a Git Repository ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://www.git-scm.com/book/en/v2/Git-Basics-Getting-a-Git-Repository){: new_window}.  
 2. Fügen Sie in der Datei `null-buildpack/bin/compile` die folgende Zeile hinzu und schreiben Sie die Änderungen fest. Informationen dazu, wie Sie Änderungen festschreiben finden Sie unter [Git Basics - Recording Changes to the Repository ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://www.git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository){: new_window}.
  ```
  rm -rfv $2/*
  ```
 3. Führen Sie mit dem folgenden Befehl eine Push-Operation für Ihre App mit dem geänderten Null-Buildpack durch, um den Cacheinhalt zu löschen. Wenn Sie diesen Schritt ausgeführt haben, wurde der gesamte Inhalt im Cacheverzeichnis Ihrer App gelöscht.
  ```
  ibmcloud cf push appname -p app_path -b <modified_null_buildpack>
  ```
 4. Führen Sie mit dem folgenden Befehl eine Push-Operation für Ihre App mit dem neuesten Buildpack aus, das Sie verwenden möchten:
  ```
  ibmcloud cf push appname -p app_path -b <latest_buildpack>
  ```

### Anwendung wird fortgesetzt neu gestartet
{: #ts_apprestart}

Die Anwendung stürzt weiterhin ab und wird erneut gestartet.
{: tsSymptoms}

Der Lebenszyklus des Anwendungscontainers, wie Evakuierung und Beendigung, kann sich auf Ihre Anwendungsfunktionalität auswirken.  
{: tsCauses}

Ermitteln Sie, welcher Schritt des Anwendungscontainerlebenszyklus Fehler in der Anwendungsbereitstellung verursacht. Informieren Sie sich eingehender über den [Lebenszyklus von Cloud Foundry-Anwendungen](https://docs.cloudfoundry.org/devguide/deploy-apps/app-lifecycle.html#evacuation).
{: tsResolve}

### Die Schaltfläche 'Aktionen' auf der Seite 'Instanzdetails' ist inaktiviert (veraltet)
  {: #ts_actionsbutton}

  Die Schaltfläche 'Aktionen' auf der Seite 'Instanzdetails' ist inaktiviert.
  {: tsSymptoms}

  Dieses Problem kann folgende Ursachen haben:
  {: tsCauses}

   * Die App wurde nicht mit dem eingebetteten Liberty-Buildpack bereitgestellt.
   * Die App wurde mit einer älteren Version des Liberty-Buildpacks bereitgestellt.

  Wenn das Problem durch eine ältere Version des Liberty-Buildpacks verursacht wird, stellen Sie die App erneut in {{site.data.keyword.Bluemix_notm}} bereit. Andernfalls können Sie dem Support-Team die folgenden Protokolldateien für die Clientanwendung zur Verfügung stellen:
  {: tsResolve}

    * logs/messages.log
    * logs/stdout.log
    * logs/stderr.log


### Berechtigungsnachweise sind zum Öffnen eines Trace- oder Speicherauszugsfensters erforderlich (veraltet)
  {: #ts_username}

  Ein Benutzername und das zugehörige Kennwort sind erforderlich, um das Trace- bzw. Speicherauszugsfenster zu öffnen.
  {: tsSymptoms}

  Dieses Problem wird durch eine Zeitlimitüberschreitung der Anmeldesitzung verursacht.
  {: tsCauses}

  Geben Sie den Benutzernamen und das Kennwort erneut ein.
  {: tsResolve}


### Beim Ausführen von Trace- oder Speicherauszugsoperationen tritt ein Fehler auf (veraltet)
  {: #ts_target}

  Während der Ausführung von Trace- oder Speicherauszugsoperationen wird eine Fehlernachricht angezeigt. Diese Nachricht gibt an, dass eine Zielinstanz für eine App nicht im aktiven Status ist:
  {: tsSymptoms}

  ```
  Instance 0: Trace specification is set successfully
  Instance 2: Trace specification is set successfully
  Instance 1: Target instance 1 for app abcd is not in the running state.
  Instance 3: Trace specification is set successfully
  Instance 4: Trace specification is set successfully
  ```

  Dieses Problem kann folgende Ursachen haben:
  {: tsCauses}

    * Die Trace- bzw. Speicherauszugsmanagementfunktionen sind nur für App-Instanzen vorgesehen, die ausgeführt werden. Trace- oder Speicherauszugsoperationen können nicht für App-Instanzen verwendet werden, die gestoppt sind, gestartet werden oder abgestürzt sind.
    * Der Status der App-Instanz ändert sich gerade, wenn der Trace- oder Speicherauszugsdialog geöffnet wird.

  Schließen Sie das Fenster und öffnen Sie es erneut.
  {: tsResolve}


### Instanzen haben unterschiedliche Konfigurationen der Tracespezifikation (veraltet)
  {: #ts_different_config}

  Für verschiedene Instanzen einer App werden möglicherweise unterschiedliche Konfigurationen der Tracespezifikation (traceSpecification) angezeigt.
  {: tsSymptoms}

  Dieses Verhalten hat folgende Ursachen:
  {: tsCauses}

    * Sie haben zuvor die Konfiguration für eine oder mehrere Instanzen geändert. Wenn Sie die Konfiguration der Tracespezifikation für eine Instanz ändern, gilt die Änderung nicht für andere Instanzen derselben App. Ihre App verwendet zum Beispiel 'log4j' und Sie haben zwei Instanzen für diese App. Sie können die Protokollebene von Instanz 0 von 'info' in 'debug' ändern, jedoch bleibt die Protokollebene von Instanz 1 bei 'info'.

  Keine Aktion erforderlich. Es handelt sich um das erwartete Verhalten.
  {: tsResolve}


### Datenträgerkontingent überschritten
  {: #ts_diskquota}

  In Ihrem App-Protokoll wird möglicherweise angegeben, dass Ihr Datenträgerkontingent überschritten ist.

  Sie finden die Fehlernachricht `Disk quota exceeded` (Datenträgerkontingent überschritten) im Protokoll Ihrer App.
  {: tsSymptoms}

  Dieses Problem hat eine der folgenden Ursachen:
  {: tsCauses}

    * Die Speicherauszugsdateien werden mit den aktiven App-Instanzen generiert und die Dateien belegen das gesamte zugeordnete Datenträgerkontingent. Standardmäßig beträgt das Datenträgerkontingent für eine App-Instanz 1 GB. Sie können Ihre Plattenbelegung prüfen, indem Sie auf **Dashboard > Anwendung > Anwendungslaufzeit** klicken. Das folgende Beispiel zeigt die Laufzeitinformationen, einschließlich Plattenbelegung, für zwei Instanzen einer App:
      ```
      Instanz	Status	CPU	Speicherbelegung Plattenbelegung

  	0		Aktiv	1.0%	344.8MB/512MB	236.8MB/1GB
  	2		Aktiv	2.3%	361.2MB/512MB	235.7MB/1GB
      ```
    * Das Datenträgerkontingent wird durch das aktuelle Organisationskontingent beschränkt.

  Verwenden Sie eine der folgenden Methoden:
  {: tsResolve}

    * Löschen Sie Speicherauszugsdateien, nachdem sie heruntergeladen wurden.
    * Stellen Sie die App erneut mit einem größeren Datenträgerkontingent bereit, indem Sie den folgenden Eintrag in das Bereitstellungsmanifest einfügen:
      ```
      disk_quota: 2048
      ```

## Liberty for Java
{: #ts_liberty}

### Anwendung startet das Akzeptieren von Verbindungen nicht
{: #health_check_timeout}

Eine Liberty-Anwendung wird mit einem Fehler "_Failed to start accepting connections_" nicht gestartet. Der Fehler könnte in den Protokollen zum Beispiel wie folgt aussehen:
{: tsSymptoms}

```
   2016-11-14T14:44:58.45+0000 [API/0]      OUT App instance exited with guid 21ac63eb-9595-428a-94c7-b0b02aaf77cc payload: {"cc_partition"=>"default", "droplet"=>"21ac63eb-9595-428a-94c7-b0b02aaf77cc", "version"=>"b2772438-92de-4d47-b680-ea772ac2288a", "instance"=>"f4799c8c89214bbd8067883c3ffe23e0", "index"=>0, "reason"=>"CRASHED", "exit_status"=>255, "exit_description"=>"failed to accept connections within health check timeout", "crash_timestamp"=>1479134698}
   2016-11-14T14:45:07.50+0000 [DEA/4]      ERR Instance (index 0) failed to start accepting connections
```
{: codeblock}

{{site.data.keyword.Bluemix_notm}} führt eine Statusprüfung der Anwendung durch, um zu ermitteln, ob sie erfolgreich gestartet wurde. Die Statusprüfung stellt fest, ob die Anwendung über den Port empfangsbereit ist, der der Anwendung zugeordnet ist. Das Standardzeitlimit für diese Prüfung beträgt 60 Sekunden und einige Anwendungen benötigen zum Starten möglicherweise mehr als 60 Sekunden.  Die Anwendung kann aus mehreren Gründen mehr Zeit für den Start benötigen. Zum Beispiel verlängert das Binden von Services wie [New Relic](/docs/runtimes/liberty/monitoring/newRelic.html) die Startzeit. Die Anwendung kann außerdem auch Initialisierungsschritte durchführen, deren Abschluss einige Zeit dauert.
{: tsCauses}

Untersuchen Sie zuerst die Protokolle auf offensichtliche Fehler, die zu dem Fehler der Liberty-Anwendung geführt haben könnten. Falls keine offensichtlichen Fehler zu erkennen sind, versuchen Sie Folgendes:
{: tsResolve}

#### Zeitlimit der Statusprüfung erhöhen

* Wenn Sie die Anwendung mit dem Befehl `ibmcloud cf push` bereitstellen, geben Sie ein längeres Anwendungsstartzeitlimit mithilfe der Option `-t` an. Beispiel:

        ```
	ibmcloud cf push myApp -t 180
        ```
	{: codeblock}

* Das Zeitlimit für die Statusprüfung kann auch in der Datei 'manifest.yml' angegeben werden. Beispiel:

        ---
           ...
           timeout: 180
        {: codeblock}

#### Feature 'appstate' inaktivieren

Das Feature 'appstate' wird in den Prozess der {{site.data.keyword.Bluemix_notm}}-Statusprüfung integriert, um sicherzustellen, dass die Liberty-Anwendung vollständig initialisiert wird, bevor die Anwendung HTTP-Anforderungen empfangen kann. Sobald die Anwendung vollständig initialisiert ist, hat das Feature 'appstate' keine weitere Wirkung mehr.  Der Nebeneffekt dieses Features besteht darin, dass einige Anwendungen möglicherweise mehr Zeit für den Start benötigen. Zur Inaktivierung des Features 'appstate' legen Sie die folgende Umgebungseigenschaft für Ihre Anwendung fest und führen ein erneutes Staging der Anwendung durch:

```
ibmcloud cf set-env myApp JBP_CONFIG_LIBERTY "app_state: false"
```
{: codeblock}

#### Refactoring der Anwendung durchführen

Wenn Ihre Anwendung eine lange Zeit für die Initialisierung benötigt, müssen Sie die Anwendung möglicherweise refaktorieren, sodass eine verzögerte ('lazy') und/oder asynchrone Initialisierung ausgeführt wird.

#### Mit der Option "no-route" bereitstellen

1. Stellen Sie Ihre Anwendung mit der Option "--no-route" bereit. Dadurch wird die Portstatusprüfung inaktiviert. Beispiel:

        ```
	ibmcloud cf push myApp –no-route
        ```
	{: codeblock}

2. Sobald die Anwendung initialisiert ist, ordnen Sie der Anwendung eine Route zu. Beispiel:

        ibmcloud cf map-route myApp mybluemix.net
        {: codeblock}

### SSL-Fehler mit dem IBM Gateway
{: #ssl_handshake_failure}


Die folgenden Fehler werden in den Protokollen angegeben und die Anwendung wird möglicherweise nicht gestartet:
{: tsSymptoms}

```
    2016-11-03T12:32:44.82-0200 [App/0]      ERR java.security.cert.CertPathValidatorException: Certificate chaining error
    2016-11-03T12:32:44.83-0200 [App/0]      ERR [ERROR   ] CWPKI0022E: SSL HANDSHAKE FAILURE:  A signer with SubjectDN CN=*.gateway.prd.na.ca.ibmserviceengage.com, O=International Business Machines Corp., L=Armonk, ST=New York, C=US was sent from the target host.  The signer might need to be added to local trust store /home/vcap/app/wlp/usr/servers/defaultServer/resources/security/key.jks, located in SSL configuration alias defaultSSLConfig.  The extended error message from the SSL handshake exception is: PKIX path building failed: java.security.cert.CertPathBuilderException: PKIXCertPathBuilderImpl could not build a valid CertPath.; internal cause is:
    2016-11-03T12:32:44.83-0200 [App/0]      ERR java.security.cert.CertPathValidatorException: The certificate issued by CN=DigiCert Global Root CA, OU=www.digicert.com, O=DigiCert Inc, C=US is not trusted; internal cause is:
    2016-11-03T12:32:44.83-0200 [App/0]      ERR java.security.cert.CertPathValidatorException: Certificate chaining error
```
{: codeblock}

Die Fehler können generiert werden, wenn ein sicherer Service an eine Liberty-Anwendung gebunden wird und die Liberty-Anwendung als Serververzeichnis oder paketierter Server bereitgestellt wurde, der eine Datei 'server.xml' enthält, in der das Feature 'Liberty ssl-1.0' konfiguriert wird. Das Binden des sicheren Service an die Liberty-Anwendung veranlasst die Laufzeit, die Verbindung zu dem Service über eine sichere Verbindung herzustellen. Diese sichere Verbindung wird mit den SSL-Standardeinstellungen hergestellt. Da in der Liberty-Datei 'server.xml' die SSL-Standardeinstellungen angegeben sind, vertraut der konfigurierte Truststore dem Zertifikat, das vom sicheren Service verwendet wird, möglicherweise nicht.
{: tsCauses}

Ändern Sie die Konfiguration, sodass der Truststore der JVM mit einer der folgenden Optionen verwendet wird.  Stellen Sie sicher, dass Sie nach der Änderung ein erneutes Staging Ihrer Anwendung durchführen.
{: tsResolve}

#### Liberty-Datei 'server.xml' aktualisieren

Aktualisieren Sie die Datei 'server.xml', um die Datei 'cacerts' der JVM als Truststore zu verwenden. Fügen Sie die folgenden Informationen Ihrer Datei 'server.xml' hinzu:

        <ssl id="defaultSSLConfig" trustStoreRef="defaultTrustStore"/>
        <keyStore id="defaultTrustStoretore" location="${java.home}/lib/security/cacerts"/>
        {: codeblock}

#### Konfigurierten Truststore aktualisierten

Ändern Sie den konfigurierten Truststore, sodass der DigitCert-Stammzertifizierungsstelle (DigitCert-Root-CA) vertraut wird.
  1. Laden Sie die DigiCert-Root-CA unter https://www.digicert.com/CACerts/DigiCertGlobalRootCA.crt herunter.
  2. Importieren Sie die CA mit dem Java-Dienstprogramm 'keytool' in den Schlüssel. Das folgende Beispiel geht davon aus, dass 'resources/security/key.jks' als Truststore verwendet wird:

            keytool -importcert --storepass <keyStorePassword> -keystore &lt;path&gt;/resources/security/key.jks -file DigiCertGlobalRootCA.crt
            {: codeblock}


## SDK for Node.js
{: #ts_nodejs}

### Anwendung kann nicht gestartet werden: Allgemeine Fehlerbehebung

Versuchen Sie bei {{site.data.keyword.runtime_nodejs_notm}} Buildpack V3.23 oder einer späteren Version als ersten Fehlerbehebungsschritt, Ihre Abhängigkeiten in dieselben Quellendateien zu paketieren wie Ihre Anwendung. Dadurch können unterschiedliche Fehler gelöst werden, die möglicherweise auftreten, wenn Abhängigkeiten annehmen, dass sie sich im selben Basisverzeichnis befinden wie die Anwendung.

1. Installieren Sie in Ihrem Anwendungsstammverzeichnis die Abhängigkeiten, indem Sie den folgenden Befehl ausführen.

   ```bash
   npm install
   ```
   {: codeblock}
1. Stellen Sie sicher, dass die Datei mit der Endung `.cfignore ` die folgende Zeile nicht enthält:

   ```
   node_modules/
   ```

Wenn Sie jetzt Ihre Anwendung mit dem Befehl `ibmcloud cf push` bereitstellen anstatt Ihre Abhängigkeiten an eine separate Position herunterzuladen, werden die Abhängigkeiten in dasselbe Verzeichnis wie der Rest der Anwendung kopiert.

### Anwendungsstart schlägt mit Fehler "No space left on device" fehl
{: #no_space_left_on_device}


Der Start einer Node.js-Anwendung schlägt mit dem Fehler "No space left on device" fehl. Der Fehler könnte in den Protokollen zum Beispiel wie folgt aussehen:
{: tsSymptoms}

```
   2017-01-16T14:25:14.61-0500 [CELL/0]     ERR tar: ./app/node_modules/pm2/node_modules/cron/node_modules/moment-timezone/LICENSE: Cannot write: No space left on device

```
{: codeblock}

Node.js-Anwendungen mit NPM-Versionen vor Version 3 belegen mehr Speicherplatz beim Herunterladen von Abhängigkeiten.
{: tsCauses}

Ändern Sie die Datei 'package.json' für Ihre Anwendung so, dass NPM Version 3 oder höher verwendet wird.
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

### Anwendung wird wegen Speicherbeschränkungen erneut gestartet
{: #oom}

Node.js erkennt nicht, wie viel Speicherplatz für die Anwendung verfügbar ist, sodass der Garbage-Collector möglicherweise nicht ausgeführt wird, bevor der Speicherplatz erschöpft ist.

```
2017-09-01T11:00:42.19-0400 [APP/PROC/WEB/0]OUT Exit status 137
2017-09-01T11:00:42.23-0400 [CELL/0]     OUT Exit status 0
2017-09-01T11:00:42.27-0400 [CELL/0]     OUT Destroying container
2017-09-01T11:00:42.34-0400 [API/0]      OUT Process has crashed with type: "web"
2017-09-01T11:00:42.36-0400 [API/0]      OUT App instance exited with guid eecfba3b-430c-4a6b-b71f-ac72816fe152 payload: {"instance"=>"77dbb981-16d0-3a05-3235-9a4b", "index"=>0, "reason"=>"CRASHED", "exit_description"=>"2 error(s) occurred:\n\n* 2 error(s) occurred:\n\n* Exited with status 137 (out of memory)\n* cancelled\n* cancelled", "crash_count"=>1, "crash_timestamp"=>1504278042244633291, "version"=>"6497b5b5-67d4-4c5a-b1af-362e522a029d"}
2017-09-01T11:00:43.35-0400 [CELL/0]     OUT Successfully destroyed container
```
{: codeblock}

Eine mögliche Lösung besteht darin, die Option `--max_old_space_size` im Startbefehl der Anwendung in der Datei 'package.json' festzulegen. Diese Option stellt einen Teil des Speicherplatzbedarfs der Anwendung dar und sollte auf einen Wert gesetzt werden, der kleiner als der gesamte für die Anwendung verfügbare Speicherplatz ist. Eine eingehendere Erörterung dieses Themas finden Sie unter [Large memory spikes and Heroku](https://github.com/nodejs/node/issues/3370) (Hohe Speicherbelegungsspitzen und Heroku).
```
  "scripts": {
    "start": "node --max_old_space_size=800 server.js"
  }
```
{: codeblock}

## ASP.NET Core
{: #ts_dotnet}

Die Bereitstellung der Anwendung schlägt mit folgender Nachricht fehl: `API/0App instance exited ... payload: {... "reason"=>"CRASHED", "exit_status"=>-1, ...}`.
{: tsSymptoms}

Wenn Sie eine ähnliche Nachricht bei der Push-Operation Ihrer ASP.net-Anwendung empfangen, ist dies am wahrscheinlichsten auf darauf zurückzuführen, dass Ihre Anwendung entweder das Hauptspeicherplatzlimit oder das Datenträgerkontingentlimit überschreitet.  Erhöhen Sie die Kontingente für den Hauptspeicherplatz oder Plattenspeicherplatz für Ihre Anwendung.
{: tsCauses}

Die Bereitstellung der Anwendung schlägt mit folgender Nachricht fehl: `Failed to compress droplet: signal: broken pipe` oder `No space left on device`.  Wie lässt sich dieser Fehler beheben?
{: tsSymptoms}

Projekte, die durch eine Push-Operation aus Quellcode übertragen werden, der eine hohe Anzahl von NuGet-Paketabhängigkeiten enthält, können manchmal diesen Fehler verursachen, wenn der NuGet-Paketcache aktiviert ist.  Setzen Sie die Umgebungsvariable `CACHE_NUGET_PACKAGES` auf den Wert `false`, um den Cache zu inaktivieren. Weitere Informationen finden Sie in den Anweisungen zur [Inaktivierung des NuGet-Paketcache](/docs/runtimes/dotnet/disablingNuGet.md).
{: tsCauses}

### Nützliche Links
* [NuGet](https://docs.nuget.org/Consume/Overview){: new_window}
* [Übersicht über ASP.NET Core](http://docs.asp.net/en/latest/conceptual-overview/aspnet.html){: new_window}

## PHP
{: #ts_PHP}

### NOTICE-Nachrichten aus dem PHP-Buildpack
{: #ts_phplog}

Sie finden in den Protokollen möglicherweise Nachrichten, die die Zeichenfolge NOTICE enthalten. Sie können die Protokollierung für solche Nachrichten durch eine Änderung der Protokollierungsebene stoppen.

Wenn Sie eine App durch eine Push-Operation an {{site.data.keyword.Bluemix_notm}} unter Verwendung eines PHP-Buildpacks übertragen, werden möglicherweise Nachrichten mit `NOTICE` angezeigt:
{: tsSymptoms}

```
• 2015-01-26T15:00:59.70+0100 [App/0] ERR [26-Jan-2015 14:00:59] NOTICE: [pool www] 'user' directive is ignored when FPM is not running as root
• 2015-01-26T15:01:00.63+0100 [App/0] ERR [26-Jan-2015 14:00:59] NOTICE: [pool www] 'user' directive is ignored when FPM is not running as root
• 2015-01-26T15:01:00.63+0100 [App/0] ERR [26-Jan-2015 14:00:59] NOTICE: fpm is running, pid 93
• 2015-01-26T15:01:00.63+0100 [App/0] ERR [26-Jan-2015 14:00:59] NOTICE: ready to handle connections
```
Im PHP-Buildpack wird die Protokollierungsebene durch den Parameter 'error_log' definiert. Der Parameter `error_log` hat standardmäßig den Wert **stderr notice**. Das folgende Beispiel zeigt die Konfiguration der Standardprotokollierungsebene in der Datei `nginx-defaults.conf` des PHP-Buildpacks, das von Cloud Foundry bereitgestellt wird. Weitere Informationen finden Sie unter [cloudfoundry/php-buildpack ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/cloudfoundry/php-buildpack/blob/ff71ea41d00c1226d339e83cf2c7d6dda6c590ef/defaults/config/nginx/1.5.x/nginx-defaults.conf){: new_window}.
{: tsCauses}

```
daemon off;
error_log stderr notice;
pid @{HOME}/nginx/logs/nginx.pid;
```

Die `NOTICE`-Nachrichten dienen der Information und weisen möglicherweise nicht auf ein Problem hin. Sie können die Protokollierung dieser Nachrichten stoppen, indem Sie die Protokollierungsebene in der Datei 'nginx-defaults.conf' Ihres Buildpacks von `stderr notice` in `stderr error` ändern. Beispiel: 	
{: tsResolve}

```
daemon off;
error_log stderr error;
pid @{HOME}/nginx/logs/nginx.pid;
```
Weitere Informationen zur Änderung der Standardkonfiguration für die Protokollierung finden Sie unter [error_log ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://nginx.org/en/docs/ngx_core_module.html#error_log){: new_window}.


## Python
{: #ts_python}

### Eine Python-Bibliothek eines anderen Anbieters kann nicht in {{site.data.keyword.Bluemix_notm}} importiert werden
{: #ts_importpylib}

Es möglich, dass Sie eine Python-Bibliothek eines anderen Anbieters nicht in {{site.data.keyword.Bluemix_notm}} importieren können. Zur Lösung des Problems fügen Sie im Stammverzeichnis Ihrer Python-App Konfigurationsdateien hinzu.

Wenn Sie versuchen, eine Python-Bibliothek eines anderen Anbieters wie die Bibliothek `web.py` zu importieren, schlägt der Befehl `ibmcloud cf push` fehl.
{: tsSymptoms}

Die Konfigurationsinformationen für die Python-App fehlen.
{: tsCauses}

Fügen Sie eine Datei `requirements.txt` und eine Datei `Procfile` im Stammverzeichnis Ihrer Python-App hinzu. Die folgenden Informationen gehen davon aus, dass Sie die Bibliothek `web.py` importieren:
{: tsResolve}

 1. Fügen Sie im Stammverzeichnis Ihrer Python-App eine Datei `requirements.txt` hinzu.

 In der Datei `requirements.txt` werden die Bibliothekspakete, die für Ihre Python-App erforderlich sind, und die Version der Pakete angegeben. Das folgende Beispiel zeigt den Inhalt der Datei `requirements.txt`. Dabei gibt `web.py==0.37` an, dass die Version 0.37 der Bibliothek `web.py` heruntergeladen wird, und `wsgiref==0.1.2` gibt an, dass für die Bibliothek `web.py` die Version 0.1.2 der Web-Server-Gateway-Schnittstelle erforderlich ist.
	 ```
	 web.py==0.37
     wsgiref==0.1.2
	 ```
	 Weitere Informationen zur Konfiguration der Datei `requirements.txt` finden Sie im Abschnitt zu [Requirements-Dateien](https://pip.readthedocs.org/en/1.1/requirements.html).

 2. Fügen Sie im Stammverzeichnis Ihrer Python-App eine Datei `Procfile` hinzu.
 Die Datei `Procfile` muss den Startbefehl für Ihre Python-App enthalten. Im folgenden Befehl ist *yourappname* der Name Ihrer Python-App und *PORT* ist die Portnummer, die Ihre Python-App verwenden muss, um Anforderungen von Benutzern der App zu empfangen. *$PORT* ist optional. Wenn Sie PORT im Startbefehl nicht angeben, wird die Portnummer unter der Umgebungsvariablen `VCAP_APP_PORT` innerhalb der App verwendet.
	```
	web: python <yourappname>.py $PORT
	```

Sie können jetzt die Python-Bibliothek des anderen Anbieters in {{site.data.keyword.Bluemix_notm}} importieren.
