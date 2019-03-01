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

# Utilizzo dei pacchetti di build della community
{: #using_buildpacks}

Se non riesci a trovare uno starter nel catalogo {{site.data.keyword.Bluemix}} che fornisca il runtime che desideri, puoi portare un pacchetto di build esterno in {{site.data.keyword.Bluemix_notm}}. Puoi specificare un pacchetto di build personalizzato e compatibile con Cloud Foundry quando distribuisci la tua applicazione utilizzando il comando `ibmcloud cf push`.
{:shortdesc}

La community Cloud Foundry fornisce dei pacchetti di build esterni che puoi utilizzare come tuoi pacchetti di build. Prima di distribuire la tua applicazione a {{site.data.keyword.Bluemix_notm}}, assicurati di installare la CLI {{site.data.keyword.Bluemix_notm}}.

**Nota:** dei pacchetti di build esterni non sono forniti da IBM. Per assistenza, contatta la community Cloud Foundry.

## Pacchetti di build della community integrati

In {{site.data.keyword.Bluemix_notm}}, puoi utilizzare i pacchetti di build integrati forniti dalla community Cloud Foundry. Per visualizzare i pacchetti di build della community integrati, esegui il comando `ibmcloud cf buildpacks`:

```
ibmcloud cf buildpacks
Getting buildpacks...

buildpack      position   enabled   locked   filename
...
java_buildpack     7      true      false    buildpack_java_v2.0.2.zip
ruby_buildpack     8      true      false    buildpack_ruby_v46-245-g2fc4ad8.zip
nodejs_buildpack   9      true      false    buildpack_nodejs_v8-177-g2b0a5cf.zip
```
{:screen}


Per lo stesso runtime o framework, i pacchetti di build creati da IBM hanno la precedenza su quelli della community. Se vuoi utilizzare il pacchetto di build della community per sovrascrivere il pacchetto di build creato da IBM, devi specificare il pacchetto di build utilizzando l'opzione `-b` con il comando `ibmcloud cf push`.

Ad esempio, puoi utilizzare il pacchetto di build della community per le applicazioni web Java™ utilizzando il seguente comando:

```
ibmcloud cf push app_name -b java_buildpack -p app_path
```
{:pre}

Puoi anche utilizzare il pacchetto di build della community per l'applicazione Node.js con il seguente comando:

```
ibmcloud cf push app_name -b nodejs_buildpack -p app_path
```
{: codeblock}

Per un runtime o un framework non supportati dai pacchetti di build creati da IBM ma supportati dai pacchetti di build della community integrati, non devi necessariamente utilizzare l'opzione `-b` con il comando `ibmcloud cf push`. Ad esempio, per le applicazioni Ruby, non ci sono pacchetti di build creati da IBM. Puoi utilizzare il pacchetto di build della community integrato immettendo il seguente comando:

```
ibmcloud cf push app_name -p app_path
```
{: codeblock}

## Pacchetti di build esterni

Puoi utilizzare pacchetti di build esterni o personalizzati in {{site.data.keyword.Bluemix_notm}}. Devi specificare l'URL del pacchetto di build con l'opzione `-b` e specificare lo stack con l'opzione `-s` nel comando `ibmcloud cf push`. Ad esempio, per utilizzare un pacchetto di build della community esterno per i file statici, esegui il seguente comando:

```
ibmcloud cf push app_name -p app_path -b https://github.com/cloudfoundry/staticfile-buildpack.git
```
{: codeblock}

Se non vuoi utilizzare il pacchetto di build della community integrato per le applicazioni Ruby, puoi utilizzare un pacchetto di build esterno immettendo il seguente comando.

```
ibmcloud cf push app_name -p app_path -b https://github.com/cloudfoundry/ruby-buildpack.git
```
{: codeblock}

Puoi anche utilizzare un pacchetto di build personalizzato per la tua applicazione. Ad esempio, puoi utilizzare un pacchetto di build PHP open source fornito dalla community di Cloud Foundry. Quando distribuisci la tua applicazione PHP a {{site.data.keyword.Bluemix_notm}}, immetti il seguente comando per specificare l'URL del repository Git del pacchetto di build.

```
ibmcloud cf push app_name -p app_path -b https://github.com/cloudfoundry/php-buildpack.git
```
{:pre}

Puoi anche modificare il file `manifest.yml` nel tuo progetto per aggiungere una riga `buildpack`.

```
buildpack: https://github.com/cloudfoundry/python-buildpack.git
```
{: codeblock}


## Specifica della versione del pacchetto di build Java

Per specificare una versione del pacchetto di build Java, utilizza il comando `ibmcloud cf set - env`. Ad esempio, immetti il seguente comando per impostare la versione Java su 1.7.0.

```
ibmcloud cf set-env app_name JBP_CONFIG_OPEN_JDK_JRE &apos;{jre: { version: 1.7.0_+ }}&apos;
```
{: codeblock}

Riprepara quindi la tua applicazione per rendere effettiva la modifica.

```
ibmcloud cf restage app_name
```
{: codeblock}

## Utilizza il file `manifest.yml`.

Puoi aggiungere la variabile di ambiente e il valore che vuoi specificare direttamente nel file `manifest.yml`. Consulta [Environment variables](https://docs.cloudfoundry.org/devguide/deploy-apps/manifest.html#env-block).
