---

copyright:
  years: 2017, 2018
lastupdated: "2018-09-21"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}

# Verfügbare Buildpacks
{: #available_buildpacks}

* [Liberty for Java](/docs/runtimes/liberty/getting-started.html)
* [SDK for Node.js](/docs/runtimes/nodejs/getting-started.html)
* [ASP.NET Core](/docs/runtimes/dotnet/getting-started.html)
* [Runtime for Swift](/docs/runtimes/swift/getting-started.html)
* [XPages](/docs/starters/xpages/index.html)
* [Go](/docs/runtimes/go/getting-started.html)
* [PHP](/docs/runtimes/php/getting-started.html)
* [Python](/docs/runtimes/python/getting-started.html)
* [Ruby](/docs/runtimes/ruby/getting-started.html)
* [Tomcat](/docs/runtimes/tomcat/getting-started.html)

## Informationen zu Buildpacks
{: #about_buildpacks}

Cloud Foundry-Buildpacks stellen die Laufzeitunterstützung für Anwendungen in der Cloud Foundry-Umgebung zur Verfügung. Wenn Sie eine Anwendung in {{site.data.keyword.cloud}} bereitstellen, wird ein Buildpack gestartet, der Ihren Anwendungstyp unterstützt. {{site.data.keyword.cloud_notm}} stellt Cloud Foundry-Buildpack-Unterstützung für Java EE, Node.js, ASP.Net, Swift und andere Anwendungstypen zur Verfügung.
Mit den im Lieferumfang von {{site.data.keyword.cloud_notm}} enthaltenen Buildpacks können Sie Anwendungen bereitstellen und an Services binden.

*  Cloud Foundry

    Cloud Foundry ist eine Open-Source-Plattform für die Automatisierung des Anwendungslebenszyklus.  {{site.data.keyword.Bluemix}} baut als Service auf der Cloud Foundry-Plattform auf. Weitere Informationen finden Sie in der [Cloud Foundry-Dokumentation](https://www.cloudfoundry.org/learn/).

*  Cloud Foundry-Anwendung

   Eine Cloud Foundry-Anwendung oder Cloud Foundry-App ist jede Anwendung, die von einem der Buildpacks instanziiert wird, die mit {{site.data.keyword.Bluemix_notm}} zur Verfügung gestellt werden.

*  Buildpack

   Ein Buildpack ist ein typischerweise sprachspezifisches Paket oder eine Software, das bzw. die von {{site.data.keyword.Bluemix_notm}} zur Verfügung gestellt wird. Wenn eine Anwendung für {{site.data.keyword.Bluemix_notm}} bereitgestellt wird, wird ein entsprechendes Buildpack für die Anwendung ausgewählt. Das Buildpack stellt die Laufzeitumgebung für die Anwendung bereit.  Sie können die Gruppe der von {{site.data.keyword.Bluemix_notm}} bereitgestellten Buildpacks anzeigen, indem Sie den Befehl `ibmcloud cf buildpacks` in der Befehlszeile absetzen.

*  Laufzeit

   Eine Laufzeit ist die Gruppe von Softwarekomponenten, die die Ausführungsumgebung für eine Anwendung zur Verfügung stellen.  Die Benennungen *Laufzeit* und *Buildpack* werden manchmal austauschbar verwendet.  Wenn ein Buildpack mit der Bereitstellung einer Anwendung abgeschlossen hat, wird die Laufzeitumgebung eingerichtet.

*  Boilerplate

   Eine Boilerplate ist eine einfache Anwendung, die für eine bestimmte Laufzeit entworfen wurde.  Die Boilerplates bieten Vorlagen oder Beispiele der sprach- und laufzeitspezifischen Anwendungstypen.  Sie können Boilerplates am Anfang der Entwicklung ausgereifterer Anwendungen als Startcode verwenden.  {{site.data.keyword.Bluemix_notm}} stellt folgende Boilerplates bereit:
   * *Hello World-Boilerplates*: Einfache Anwendungen, die in der Regel eine sprach- und laufzeitspezifische Hello World-Anwendung bereitstellen.
   * *Boilerplates mit Services*: Diese veranschaulichen, wie eine Laufzeit mit einem Service verwendet wird.
   * *Boilerplates mit Frameworks*: Einfache sprach- und laufzeitspezifische Anwendungen, die bestimmte Sprachframeworks wie Python Flask oder Ruby Sinatra nutzen.

*  Service

   Ein Service ist eine von {{site.data.keyword.Bluemix_notm}} bereitgestellte Funktion, die mit einer Anwendung gekoppelt werden kann.
