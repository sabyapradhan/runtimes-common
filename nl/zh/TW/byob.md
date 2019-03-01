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

# 使用社群建置套件
{: #using_buildpacks}

如果您在 {{site.data.keyword.Bluemix}} 型錄中找不到提供您所要運行環境的入門範本，可以將外部建置套件帶到 {{site.data.keyword.Bluemix_notm}}。當您使用 `ibmcloud cf push` 指令部署應用程式時，可以指定自訂 Cloud Foundry 相容的建置套件。
{:shortdesc}

外部建置套件是由 Cloud Foundry 社群提供，可用來作為您自己的建置套件。將應用程式部署至 {{site.data.keyword.Bluemix_notm}} 之前，請確定您安裝了 {{site.data.keyword.Bluemix_notm}} 指令行介面。

**附註：**外部建置套件不是由 IBM 提供。請與 Cloud Foundry 社群聯絡，以獲得支援。

## 內建社群建置套件

在 {{site.data.keyword.Bluemix_notm}} 中，您可以使用 Cloud Foundry 社群所提供的內建建置套件。若要查看內建社群建置套件，請執行 `ibmcloud cf buildpacks` 指令：

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


對於相同的運行環境或架構，IBM 建立的建置套件優先於社群建置套件。如果您要使用社群建置套件來改寫 IBM 建立的建置套件，則必須搭配使用 `-b` 選項與 `ibmcloud cf push` 指令來指定建置套件。

例如，您可以使用下列指令來使用適用於 Java™ Web 應用程式的社群建置套件。

```
ibmcloud cf push app_name -b java_buildpack -p app_path
```
{:pre}

您也可以使用下列指令來使用適用於 Node.js 應用程式的社群建置套件。

```
ibmcloud cf push app_name -b nodejs_buildpack -p app_path
```
{: codeblock}

對於 IBM 建立的建置套件不支援但內建社群建置套件支援的運行環境或架構，您不需要搭配使用 `-b` 選項與 `ibmcloud cf push` 指令。例如，對於 Ruby 應用程式，沒有 IBM 建立的建置套件。您可以輸入下列指令來使用內建社群建置套件。

```
ibmcloud cf push app_name -p app_path
```
{: codeblock}

## 外部建置套件

您可以在 {{site.data.keyword.Bluemix_notm}} 中使用外部或自訂建置套件。您必須使用 `-b` 選項來指定建置套件的 URL，並在 `ibmcloud cf push` 指令上使用 `-s` 選項來指定堆疊。例如，若要針對靜態檔案使用外部社群建置套件，請執行下列指令。

```
ibmcloud cf push app_name -p app_path -b https://github.com/cloudfoundry/staticfile-buildpack.git
```
{: codeblock}

如果您不想針對 Ruby 應用程式使用內建社群建置套件，則可以輸入下列指令來使用外部建置套件。

```
ibmcloud cf push app_name -p app_path -b https://github.com/cloudfoundry/ruby-buildpack.git
```
{: codeblock}

您也可以針對應用程式使用自訂建置套件。例如，您可以使用 Cloud Foundry 社群所提供的開放程式碼 PHP 建置套件。將 PHP 應用程式部署至 {{site.data.keyword.Bluemix_notm}} 時，請輸入下列指令來指定建置套件的 Git 儲存庫 URL。

```
ibmcloud cf push app_name -p app_path -b https://github.com/cloudfoundry/php-buildpack.git
```
{:pre}

您也可以編輯專案中的 `manifest.yml` 檔案，以新增 `buildpack` 行。

```
buildpack: https://github.com/cloudfoundry/python-buildpack.git
```
{: codeblock}


## 指定 Java 建置套件版本

若要指定 Java 建置套件版本，請使用 `ibmcloud cf set-env` 指令。例如，輸入下列指令，以將 Java 版本設為 1.7.0。

```
ibmcloud cf set-env app_name JBP_CONFIG_OPEN_JDK_JRE &apos;{jre: { version: 1.7.0_+ }}&apos;
```
{: codeblock}

然後，重新編譯打包您的應用程式，讓變更生效。

```
ibmcloud cf restage app_name
```
{: codeblock}

## 使用 `manifest.yml` 檔案。

您可以將想要指定的環境變數及值直接新增至 `manifest.yml` 檔案。請參閱[環境變數](https://docs.cloudfoundry.org/devguide/deploy-apps/manifest.html#env-block)。
