---

copyright:
  years: 2016, 2018
lastupdated: "2018-11-20"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}

# Istruzione per il supporto del pacchetto di build
{: #buildpack_support_statement}


## Pacchetti di build IBM integrati
{: #built-in_ibm_buildpacks}

Per [Liberty for Java](/docs/runtimes/liberty/index.html), [SDK for Node.js](/docs/runtimes/nodejs/index.html) e [ASP.NET Core](/docs/runtimes/dotnet/index.html), IBM supporterà due versioni (n & n - 1), ad esempio, IBM Liberty Buildpack v3.12 & IBM Liberty Buildpack v3.11. Ogni pacchetto di build fornirà e supporterà una o più versioni principali del runtime corrispondente nel modo appropriato. I pacchetti di build verranno aggiornati generalmente una volta al mese con l'ultima versione secondaria del runtime disponibile.

Per [{{site.data.keyword.Bluemix_notm}} Runtime for Swift](/docs/runtimes/swift/index.html), IBM fornisce il supporto per il pacchetto di build corrispondente alla versione più recente di Swift disponibile in [Swift.org](http://swift.org). Gli aggiornamenti al pacchetto di build sono sincronizzati con la versione rilasciata disponibile più recente di Swift.

I problemi possono essere segnalati rispetto a qualsiasi versione del pacchetto di build IBM integrato attualmente supportato su {{site.data.keyword.Bluemix_notm}} ma la loro verifica viene eseguita rispetto alla versione più recente. Se viene riscontrato un difetto, IBM fornirà una correzione in un livello futuro del runtime e del pacchetto di build corrispondente. IBM non fornirà correzioni per le versioni principali e secondarie precedenti (N-1, n-1). IBM non fornirà supporto per i runtime della community anche quando utilizzano i pacchetti di build IBM, ad esempio Open JDK con il pacchetto di build Liberty. Questi runtime di community rispettano la stessa politica di supporto dei pacchetti di build di community integrati, come descritto nella seguente sezione.

## Pacchetti di build della community integrati
{: #built-in_community_buildpacks}

I seguenti pacchetti di build della community integrati sono forniti dalla community di Cloud Foundry:

* [Java](/docs/runtimes/tomcat/index.html)
* [Node.js](https://github.com/cloudfoundry/nodejs-buildpack)
* [PHP](/docs/runtimes/php/index.html)
* [Ruby](/docs/runtimes/ruby/index.html)
* [Python](/docs/runtimes/python/index.html)
* [Go](/docs/runtimes/go/index.html)

Si effettueranno aggiornamenti a questi pacchetti di build quando si esegue l'upgrade di {{site.data.keyword.Bluemix_notm}} a una nuova versione di Cloud Foundry. I problemi che si verificano con questi runtime su {{site.data.keyword.Bluemix_notm}} possono essere segnalati a IBM che ti aiuterà a determinare se {{site.data.keyword.Bluemix_notm}} è l'origine del problema. Per i problemi correlati a {{site.data.keyword.Bluemix_notm}}, IBM fornirà una correzione; tuttavia, per i difetti nel pacchetto di build o nel runtime stesso, IBM contribuirà a segnalarli alla community appropriata. IBM non fornirà correzioni per questi pacchetti di build e runtime.

## Pacchetti di build esterni
{: #external_buildpacks}

Per i pacchetti di build esterni, IBM non fornirà alcun supporto. Per ottenere assistenza, potresti dover contattare la community di Cloud Foundry.

## Servizi di terze parti
{: #third-party}

I pacchetti di build ti consentono di utilizzare alcuni servizi di terze parti non IBM, come Dynatrace o New Relic, nelle tue applicazioni. IBM non fornisce supporto per i servizi di terze parti. Per informazioni sull'utilizzo di servizi di terze parti in IBM Cloud, consulta _Cloud Service Use_ nella [descrizione del servizio IBM Cloud più recente ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www-03.ibm.com/software/sla/sladb.nsf/sla/bm). Prima di utilizzare un servizio di terze parti, consulta le informazioni sulla licenza dal provider di servizi.
