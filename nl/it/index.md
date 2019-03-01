---

copyright:
  years: 2017, 2018
lastupdated: "2018-09-21"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}

# Pacchetti di build disponibili
{: #available_buildpacks}

* [Liberty for Java](/docs/runtimes/liberty/getting-started.html)
* [SDK for Node.js](/docs/runtimes/nodejs/getting-started.html)
* [ASP.NET core](/docs/runtimes/dotnet/getting-started.html)
* [Runtime for Swift](/docs/runtimes/swift/getting-started.html)
* [XPages](/docs/starters/xpages/index.html)
* [Go](/docs/runtimes/go/getting-started.html)
* [PHP](/docs/runtimes/php/getting-started.html)
* [Python](/docs/runtimes/python/getting-started.html)
* [Ruby](/docs/runtimes/ruby/getting-started.html)
* [Tomcat](/docs/runtimes/tomcat/getting-started.html)

## Informazioni sui pacchetti di build
{: #about_buildpacks}

I pacchetti di build Cloud Foundry forniscono il supporto di runtime per le applicazioni nell'ambiente Cloud Foundry. Quando distribuisci un'applicazione a {{site.data.keyword.cloud}}, avvia un pacchetto di build che supporta il tuo tipo di applicazione. {{site.data.keyword.cloud_notm}} fornisce il supporto per il pacchetto di build Cloud Foundry per Java EE, Node.js, ASP.Net, Swif e altri tipi di applicazione.
Puoi utilizzare i pacchetti di build inclusi con {{site.data.keyword.cloud_notm}} per distribuire le applicazioni e associarle ai servizi.

*  Cloud Foundry

    Cloud Foundry è una piattaforma open source per l'automazione del ciclo di vita dell'applicazione.  {{site.data.keyword.Bluemix}} è creato con la PaaS (Platform as a Service) Cloud Foundry. Per ulteriori informazioni, controlla la [documentazione Cloud Foundry](https://www.cloudfoundry.org/learn/).

*  Applicazione Cloud Foundry

   Un'applicazione cloud foundry, è una qualsiasi applicazione progettata per essere istanziata da uno dei pacchetti di build forniti da {{site.data.keyword.Bluemix_notm}}.

*  Pacchetto di build

   Un pacchetto di build è un pacchetto normalmente specifico per il linguaggio software fornito da {{site.data.keyword.Bluemix_notm}}. Quando un'applicazione viene distribuita a {{site.data.keyword.Bluemix_notm}} viene scelto un pacchetto di build appropriato per l'applicazione. Il pacchetto di build fornisce l'ambiente di runtime per l'applicazione.  Puoi visualizzare i pacchetti di build forniti da {{site.data.keyword.Bluemix_notm}} immettendo il comando `ibmcloud cf buildpacks` dalla riga di comando.

*  Runtime

   Un runtime è una serie di componenti software che forniscono l'ambiente di esecuzione di un'applicazione.  I termini *runtime* e *pacchetto di build* sono alcune volte utilizzati in modo interscambiabile.  Quando un pacchetto di build ha completato la distribuzione di un'applicazione viene stabilito l'ambiente di runtime.

*  Contenitore tipo

   Un contenitore tipo è un'applicazione semplice progettata per un runtime in particolare.  I contenitori tipo forniscono i modelli o gli esempi dei tipi di applicazione specifici per runtime e linguaggio.  Puoi utilizzare un contenitore tipo come codice starter per iniziare lo sviluppo di più applicazioni sofisticate.  {{site.data.keyword.Bluemix_notm}} fornisce:
   * *Contenitori tipo Hello world*: applicazioni semplici che normalmente forniscono un'applicazione 'hello world' specifica per linguaggio e runtime.
   * *Contenitori tipo con servizi*: dimostrano come utilizzare un runtime con un servizio.
   * *Contenitori tipo con framework*: applicazioni semplici specifiche per linguaggio e runtime che si avvalgono di un framework di linguaggio particolare come Python Flask o Ruby Sinatra.

*  Servizio

   Un servizio è una funzione fornita da {{site.data.keyword.Bluemix_notm}} che può essere associata a un'applicazione.
