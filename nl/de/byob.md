---

copyright:
  years: 2015, 2018
lastupdated: "2018-06-27"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Community-Buildpacks verwenden
{: #using_buildpacks}

Wenn Sie einen Starter, der die gewünschte Laufzeit bereitstellt, im {{site.data.keyword.Bluemix}}-Katalog nicht finden können, können Sie ein externes Buildpack in {{site.data.keyword.Bluemix_notm}} integrieren. Sie können ein angepasstes, mit Cloud Foundry kompatibles Buildpack angeben, wenn Sie Ihre Anwendung mit dem Befehl `ibmcloud cf push` bereitstellen.
{:shortdesc}

Externe Buildpacks werden durch die Cloud Foundry-Community bereitgestellt und können als eigene Buildpacks verwendet werden. Stellen Sie vor der Bereitstellung Ihrer Anwendung in {{site.data.keyword.Bluemix_notm}} sicher, dass Sie die {{site.data.keyword.Bluemix_notm}}-Befehlszeilenschnittstelle installieren.

**Hinweis:** Externe Buildpacks werden nicht von IBM zur Verfügung gestellt. Wenden Sie sich an die Cloud Foundry-Community, um Unterstützung zu erhalten.

## Integrierte Community-Buildpacks

In {{site.data.keyword.Bluemix_notm}} können Sie integrierte Buildpacks verwenden, die von der Cloud Foundry-Community zur Verfügung gestellt werden. Zum Anzeigen der integrierten Community-Buildpacks führen Sie den Befehl `ibmcloud cf buildpacks` aus:

```
ibmcloud cf buildpacks
Abrufen von Buildpacks...

Buildpack      Position   aktiviert   gesperrt   Dateiname
...
java_buildpack     7      true      false    buildpack_java_v2.0.2.zip
ruby_buildpack     8      true      false    buildpack_ruby_v46-245-g2fc4ad8.zip
nodejs_buildpack   9      true      false    buildpack_nodejs_v8-177-g2b0a5cf.zip
```
{:screen}


Für dieselbe Laufzeit bzw. dasselbe Framework haben die von IBM erstellten Buildpacks Vorrang vor den Community-Buildpacks. Wenn Sie das Community-Buildpack verwenden möchten, um das von IBM erstellte Buildpack zu überschreiben, müssen Sie das Buildpack mit der Option `-b` im Befehl `ibmcloud cf push` angeben.

Beispiel: Sie können das Community-Buildpack für Java™-Webanwendungen mithilfe des folgenden Befehls verwenden.

```
ibmcloud cf push app_name -b java_buildpack -p app_path
```
{:pre}

Sie können das Community-Buildpack mit dem folgenden Befehl auch für die Node.js-Anwendung verwenden.

```
ibmcloud cf push app_name -b nodejs_buildpack -p app_path
```
{: codeblock}

Für eine Laufzeit bzw. ein Framework, die bzw. das nicht von durch IBM erstellte Buildpacks, sondern von integrierten Community-Buildpacks unterstützt wird, müssen Sie die Option `-b` im Befehl `ibmcloud cf push` nicht verwenden. Für Ruby-Anwendungen sind beispielsweise keine von IBM erstellten Buildpacks vorhanden. Sie können das integrierte Community-Buildpack verwenden, indem Sie den folgenden Befehl eingeben.

```
ibmcloud cf push app_name -p app_path
```
{: codeblock}

## Externe Buildpacks

Sie können externe oder angepasste Buildpacks in {{site.data.keyword.Bluemix_notm}} verwenden. Sie müssen die URL des Buildpacks mit der Option `-b` und den Stack mit der Option `-s` im Befehl `ibmcloud cf push` angeben. Wenn Sie zum Beispiel ein externes Community-Buildpack für statische Dateien verwenden wollen, führen Sie den folgenden Befehl aus.

```
ibmcloud cf push app_name -p app_path -b https://github.com/cloudfoundry/staticfile-buildpack.git
```
{: codeblock}

Wenn Sie das integrierte Community-Buildpack für Ruby-Anwendungen nicht verwenden wollen, können Sie ein externes Buildpack verwenden, indem Sie den folgenden Befehl eingeben.

```
ibmcloud cf push app_name -p app_path -b https://github.com/cloudfoundry/ruby-buildpack.git
```
{: codeblock}

Sie können auch ein angepasstes Buildpack für Ihre Anwendung verwenden. Sie können zum Beispiel ein quelloffenes PHP-Buildpack verwenden, das von der Cloud Foundry-Community zur Verfügung gestellt wird. Wenn Sie Ihre PHP-Anwendung in {{site.data.keyword.Bluemix_notm}} bereitstellen, geben Sie den folgenden Befehl ein, um die Git-Repository-URL des Buildpacks anzugeben.

```
ibmcloud cf push app_name -p app_path -b https://github.com/cloudfoundry/php-buildpack.git
```
{:pre}

Sie können außerdem die Datei `manifest.yml` in Ihrem Projekt bearbeiten, um eine Zeile für `buildpack` hinzufügen.

```
buildpack: https://github.com/cloudfoundry/python-buildpack.git
```
{: codeblock}


## Java-Buildpack-Version angeben

Zur Angabe einer Java-Buildpack-Version verwenden Sie den Befehl `ibmcloud cf set-env`. Geben Sie zum Beispiel den folgenden Befehl ein, um die Java-Version auf 1.7.0 zu setzen.

```
ibmcloud cf set-env app_name JBP_CONFIG_OPEN_JDK_JRE &apos;{jre: { version: 1.7.0_+ }}&apos;
```
{: codeblock}

Führen Sie anschließend ein erneutes Staging Ihrer Anwendung durch, um die Änderungen in Kraft zu setzen.

```
ibmcloud cf restage app_name
```
{: codeblock}

## Datei `manifest.yml` verwenden

Sie können die Umgebungsvariable direkt in der Datei `manifest.yml` angeben und auf den gewünschten Wert setzen. Siehe [Umgebungsvariablen](https://docs.cloudfoundry.org/devguide/deploy-apps/manifest.html#env-block).
