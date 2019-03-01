---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-23"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}


# Utilizzo di un proxy
{: #working_with_proxy}

In alcuni ambienti come [{{site.data.keyword.Bluemix_notm}} Dedicato](/docs/dedicated/index.html#dedicated) e
[{{site.data.keyword.Bluemix_notm}} Locale](/docs/local/index.html#local) un proxy può essere configurato in modo che influisca sul comportamento della tua applicazione durante le fasi di preparazione e runtime.

Puoi configurare la tua applicazione per lavorare con il proxy utilizzando le seguenti variabili di ambiente:
  * [http_proxy ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://docs.cloudfoundry.org/buildpacks/proxy-usage.html){: new_window}
  * [https_proxy ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://docs.cloudfoundry.org/buildpacks/proxy-usage.html){: new_window}
  * [no_proxy ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](http://www.gnu.org/software/wget/manual/html_node/Proxies.html){: new_window}

Puoi impostare queste variabili utilizzando *bluemix app env-set* o con il file *manifest.yml*.  A seconda di come configuri le tue variabili di ambiente del proxy, se la tua applicazione scarica le risorse da internet durante la preparazione, le tue risorse possono essere scaricate utilizzando il proxy. Ad esempio, se hai un'applicazione Nodejs in un ambiente con `http_proxy` impostato su `yourProxyURL` e vuoi consentire a `npm` di scaricare i moduli da internet **ma non con il proxy**.  Per scaricare utilizzando i proxy, imposta `no_proxy` su `npmjs.org`.

**Nota**: potresti desiderare che la tua applicazione utilizzi il proxy durante il runtime, dopo la preparazione.  L'utilizzo del proxy di runtime è completamente dipendente dall'applicazione e non viene influenzato dal pacchetto di build o dalle variabili di ambiente del proxy.

## Applicazioni Java
{: #java_apps}

Per le applicazioni [Liberty for Java](/docs/runtimes/liberty/index.html) e [java_buildpack](/docs/runtimes/tomcat/index.html), le impostazioni del proxy possono essere passate al runtime tramite la variabile di ambiente **JAVA_OPTS**.  Ad esempio puoi immettere il comando e quindi ripreparare la tua applicazione:
```
   ibmcloud app env-set myApp JAVA_OPTS "-Dhttp.proxyHost=yourProxyURL -Dhttp.proxyPort=yourProxyPort"
```
{: codeblock}

La tua applicazione quindi utilizza le impostazioni del proxy specificate al runtime. Consulta [Java Networking and Proxies ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://docs.oracle.com/javase/8/docs/technotes/guides/net/proxies.html){: new_window} per ulteriori informazioni sulle opzioni del proxy Java.
