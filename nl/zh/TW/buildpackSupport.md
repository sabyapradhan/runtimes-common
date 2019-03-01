---

copyright:
  years: 2016, 2018
lastupdated: "2018-11-20"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}

# 建置套件支援聲明
{: #buildpack_support_statement}


## 內建 IBM 建置套件
{: #built-in_ibm_buildpacks}

若為 [Liberty for Java](/docs/runtimes/liberty/index.html)、[SDK for Node.js](/docs/runtimes/nodejs/index.html) 及 [ASP.NET Core](/docs/runtimes/dotnet/index.html)，IBM 將支援兩個版本（n 及 n - 1），例如 IBM Liberty Buildpack 3.12 版及 IBM Liberty Buildpack 3.11 版。每個建置套件都會視情況提供及支援其對應運行環境的一個以上主要版本。建置套件一般一個月會以可用運行環境的最新次要版本重新整理一次。

若為 [{{site.data.keyword.Bluemix_notm}} Runtime for Swift](/docs/runtimes/swift/index.html)，IBM 支援符合 [Swift.org](http://swift.org) 上所提供之最新 Swift 版本的建置套件。建置套件的更新將會與 Swift 的最新可用發行版本同步。

可以針對 {{site.data.keyword.Bluemix_notm}} 上目前所支援之任何內建「IBM 建置套件」版本提報問題，不過將會針對最新版本進行驗證。如果發現問題，IBM 將提供運行環境及對應建置套件之未來層次的修正程式。IBM 不會提供較舊的主要版本及次要版本（N-1、n-1）的修正程式。IBM 將不支援社群運行環境，即使使用 IBM 建置套件時也是一樣（例如，搭配使用 Open JDK 與 Liberty 建置套件）。這些社群運行環境所遵循的支援原則與內建社群建置套件相同（如下節所述）。

## 內建社群建置套件
{: #built-in_community_buildpacks}

「Cloud Foundry 社群」提供下列內建「社群建置套件」：

* [Java](/docs/runtimes/tomcat/index.html)
* [Node.js](https://github.com/cloudfoundry/nodejs-buildpack)
* [PHP](/docs/runtimes/php/index.html)
* [Ruby](/docs/runtimes/ruby/index.html)
* [Python](/docs/runtimes/python/index.html)
* [Go](/docs/runtimes/go/index.html)

將 {{site.data.keyword.Bluemix_notm}} 升級至新版的 Cloud Foundry 時，將會更新這些建置套件。可以將 {{site.data.keyword.Bluemix_notm}} 上這些運行環境的問題提報給 IBM，我們將協助判斷問題來源是否為 {{site.data.keyword.Bluemix_notm}}。若為 {{site.data.keyword.Bluemix_notm}} 相關問題，IBM 將提供修正程式；不過，若為建置套件或運行環境本身的問題，IBM 會協助提報給適當的社群。IBM 不會提供這些建置套件及運行環境的修正程式。

## 外部建置套件
{: #external_buildpacks}

對於外部建置套件，IBM 不提供支援。您需要聯絡「Cloud Foundry 社群」，以獲得支援。

## 協力廠商服務
{: #third-party}

建置套件可讓您在應用程式內使用一些非 IBM 的協力廠商服務（例如 Dynatrace 或 New Relic）。IBM 不提供對協力廠商服務的支援。如需在 IBM Cloud 中使用協力廠商服務的相關資訊，請參閱最新 [IBM Cloud 服務說明 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www-03.ibm.com/software/sla/sladb.nsf/sla/bm) 中的_雲端服務使用_。在使用協力廠商服務之前，請參閱服務提供者的授權資訊。
