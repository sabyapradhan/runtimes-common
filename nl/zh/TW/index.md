---

copyright:
  years: 2017, 2018
lastupdated: "2018-09-21"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}

# 可用的建置套件
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

# 關於建置套件
{: #about_buildpacks}

Cloud Foundry 建置套件會針對 Cloud Foundry 環境中的應用程式提供運行環境支援。當您將應用程式部署至 {{site.data.keyword.cloud}} 時，它會啟動支援您應用程式類型的建置套件。{{site.data.keyword.cloud_notm}} 提供了 Java EE、Node.js、ASP.Net、Swift 及其他應用程式類型的 Cloud Foundry 建置套件支援。
您可以使用 {{site.data.keyword.cloud_notm}} 隨附的建置套件，以部署應用程式並將其連結到服務。

*  Cloud Foundry

    Cloud Foundry 是適用於應用程式生命週期自動化的開放程式碼平台。{{site.data.keyword.Bluemix}} 以 Cloud Foundry 平台即服務 (PaaS) 為建置基礎。若要進一步瞭解，請參閱 [Cloud Foundry 文件](https://www.cloudfoundry.org/learn/)。

*  Cloud Foundry 應用程式 (Cloud Foundry Application)

   Cloud Foundry 應用程式是要由 {{site.data.keyword.Bluemix_notm}} 所提供的其中一個建置套件實例化的任何應用程式。

*  建置套件 (Buildpack)

   建置套件是 {{site.data.keyword.Bluemix_notm}} 所提供的一般語言特定軟體套件。將應用程式部署至 {{site.data.keyword.Bluemix_notm}} 時，會選擇應用程式適用的建置套件。建置套件會佈建應用程式的運行環境。從指令行發出 `ibmcloud cf buildpacks` 指令，即可看到 {{site.data.keyword.Bluemix_notm}} 所提供的建置套件集。

*  運行環境 (Runtime)

   運行環境是提供應用程式執行環境的軟體元件集。*運行環境* 及*建置套件* 術語有時可交換使用。建置套件完成部署應用程式時，會建立運行環境。

*  樣板 (Boilerplate)

   樣板是針對特定運行環境所設計的簡單應用程式。樣板提供語言及運行環境特定應用程式類型的範本或範例。您可以使用樣板作為入門範本程式碼，以開始開發更精密的應用程式。{{site.data.keyword.Bluemix_notm}} 提供：
   * *Hello world 樣板*：一般提供語言及運行環境特定 'hello world' 應用程式的簡單應用程式。
   * *樣板與服務*：示範如何搭配使用運行環境與服務。
   * *樣板與架構*：充分運用特定語言架構（例如 Python Flask 或 Ruby Sinatra）的簡單語言及運行環境特定應用程式。

*  服務 (Service)

   服務是由 {{site.data.keyword.Bluemix_notm}} 所提供且可與應用程式連結的機能。
