---

copyright:
  years: 2016, 2018
lastupdated: "2018-11-20"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}

# Unterstützung für Buildpacks
{: #buildpack_support_statement}


## Integrierte IBM Buildpacks
{: #built-in_ibm_buildpacks}

Für [Liberty for Java](/docs/runtimes/liberty/index.html), [SDK for Node.js](/docs/runtimes/nodejs/index.html) und [ASP.NET Core](/docs/runtimes/dotnet/index.html) unterstützt IBM zwei Versionen (n & n - 1), z. B. IBM Liberty Buildpack v3.12 & IBM Liberty Buildpack v3.11. Mit jedem Buildpack wird nach Bedarf mindestens eine Hauptversion der entsprechenden Laufzeit bereitgestellt und unterstützt. Buildpacks werden in der Regel einmal im Monat auf die neueste verfügbare Unterversion der Laufzeit aktualisiert.

Für die [{{site.data.keyword.Bluemix_notm}} Runtime for Swift](/docs/runtimes/swift/index.html) stellt IBM Unterstützung für das Buildpack bereit, das der neuesten Swift-Version auf [Swift.org](http://swift.org) entspricht. Updates für das Buildpack sind mit der neusten verfügbaren, freigegebenen Version von Swift synchron.

Probleme und Fehler können für jede Version des integrierten IBM Buildpacks gemeldet werden, die zurzeit in {{site.data.keyword.Bluemix_notm}} unterstützt wird; sie müssen jedoch gegenüber der neuesten Version überprüft werden. Wenn ein Fehler gefunden wird, stellt IBM in einer zukünftigen Version der Laufzeit und des entsprechenden Buildpacks eine Korrektur zur Verfügung. IBM stellt keine Korrekturen für niedrigere Haupt- und Unterversionen (N-1, n-1) zur Verfügung. IBM stellt keine Unterstützung für Community-Laufzeiten bereit, selbst wenn IBM Buildpacks verwendet werden. Ein Beispiel ist die Verwendung von Open JDK mit dem Liberty-Buildpack. Für diese Community-Laufzeiten gilt dieselbe Unterstützungsrichtlinie wie für die integrierten Community-Buildpacks, wie dies im folgenden Abschnitt beschrieben wird.

## Integrierte Community-Buildpacks
{: #built-in_community_buildpacks}

Die folgenden integrierten Community-Buildpacks werden von der Cloud Foundry-Community zur Verfügung gestellt:

* [Java](/docs/runtimes/tomcat/index.html)
* [Node.js](https://github.com/cloudfoundry/nodejs-buildpack)
* [PHP](/docs/runtimes/php/index.html)
* [Ruby](/docs/runtimes/ruby/index.html)
* [Python](/docs/runtimes/python/index.html)
* [Go](/docs/runtimes/go/index.html)

Diese Buildpacks werden aktualisiert, wenn {{site.data.keyword.Bluemix_notm}} auf eine neue Version von Cloud Foundry aktualisiert wird. Probleme oder Fehler bei diesen Laufzeiten unter {{site.data.keyword.Bluemix_notm}} können IBM gemeldet werden. IBM hilft Ihnen, zu ermitteln, ob {{site.data.keyword.Bluemix_notm}} die Ursache des Problems ist. Für Probleme, die mit {{site.data.keyword.Bluemix_notm}} in Zusammenhang stehen, stellt IBM ein Fix zur Verfügung. Betrifft der Fehler jedoch das Buildpack oder die Laufzeit selbst, ist IBM Ihnen bei der Meldung an die entsprechende Community behilflich. Für diese Buildpacks und Laufzeiten stellt IBM keine Korrekturen zur Verfügung.

## Externe Buildpacks
{: #external_buildpacks}

Für externe Buildpacks stellt IBM keine Unterstützung bereit. Sie müssen sich möglicherweise an die Cloud Foundry-Community wenden, wenn Sie Unterstützung benötigen.

## Services anderer Anbieter
{: #third-party}

Durch die Buildpacks können Sie einige Services, die nicht von IBM, sondern von anderen Anbietern zur Verfügung gestellt werden, wie zum Beispiel Dynatrace oder New Relic, in Ihren Anwendungen nutzen. IBM stellt keine Unterstützung für Services anderer Anbieter bereit. Informationen zur Verwendung von Services anderer Anbieter in IBM Cloud finden Sie im Abschnitt zur Verwendung von Cloud-Services (_Cloud Service Use_) in der aktuellen [IBM Cloud-Servicebeschreibung ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www-03.ibm.com/software/sla/sladb.nsf/sla/bm). Bevor Sie einen Service eines anderen Anbieters verwenden, machen Sie sich mit den Lizenzinformationen des Serviceanbieters vertraut.
