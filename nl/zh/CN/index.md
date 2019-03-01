---

copyright:
  years: 2017, 2018
lastupdated: "2018-09-21"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}

# 可用 Buildpack
{: #available_buildpacks}

* [Liberty for Java](/docs/runtimes/liberty/getting-started.html)
* [SDK for Node.js](/docs/runtimes/nodejs/getting-started.html)
* [ASP.NET 核心](/docs/runtimes/dotnet/getting-started.html)
* [Runtime for Swift](/docs/runtimes/swift/getting-started.html)
* [XPages](/docs/starters/xpages/index.html)
* [Go](/docs/runtimes/go/getting-started.html)
* [PHP](/docs/runtimes/php/getting-started.html)
* [Python](/docs/runtimes/python/getting-started.html)
* [Ruby](/docs/runtimes/ruby/getting-started.html)
* [Tomcat](/docs/runtimes/tomcat/getting-started.html)

# 关于 Buildpack
{: #about_buildpacks}

Cloud Foundry buildpack 用于为 Cloud Foundry 环境中的应用程序提供运行时支持。将应用程序部署到 {{site.data.keyword.cloud}} 时，会启动支持该应用程序类型的 buildpack。{{site.data.keyword.cloud_notm}} 为 Java EE、Node.js、ASP.Net、Swift 和其他应用程序类型提供了 Cloud Foundry buildpack 支持。可以使用 {{site.data.keyword.cloud_notm}} 随附的 buildpack 来部署应用程序并将其绑定到服务。

*  Cloud Foundry

    Cloud Foundry 是一种开放式源代码平台，用于应用程序生命周期自动化。{{site.data.keyword.Bluemix}} 基于 Cloud Foundry 平台作为服务构建。请查看 [Cloud Foundry 文档](https://www.cloudfoundry.org/learn/)以了解更多信息。

*  Cloud Foundry 应用程序 (Cloud Foundry Application)

   Cloud Foundry 应用程序是指意图通过 {{site.data.keyword.Bluemix_notm}} 提供的其中一个 buildpack 来实例化的任何应用程序。

*  Buildpack

   buildpack 通常是 {{site.data.keyword.Bluemix_notm}} 提供的特定于语言的软件包。将应用程序部署到 {{site.data.keyword.Bluemix_notm}} 时，会选择与该应用程序相应的 buildpack。buildpack 为应用程序供应运行时环境。{{site.data.keyword.Bluemix_notm}} 提供了一组 buildpack。要查看它们，可以从命令行发出 `ibmcloud cf buildpacks` 命令。

*  运行时 (Runtime)

   运行时是一组软件组件，用于为应用程序提供执行环境。术语*运行时*和 *buildpack* 有时会互换使用。buildpack 完成应用程序部署后，会建立运行时环境。

*  样板 (Boilerplate)

   样板是一种简单的应用程序，专为特定运行时而设计。样板提供了特定于语言和运行时的应用程序类型的模板或样本。可以将样板用作入门模板代码，以开始开发更复杂的应用程序。{{site.data.keyword.Bluemix_notm}} 提供下列各项：
   * *Hello World 样板*：这是简单应用程序，通常提供特定于语言和运行时的“hello world”应用程序。
   * *具有服务的样板*：用于演示如何将运行时与服务配合使用。
   * *具有框架的样板*：这是特定于语言和运行时的简单应用程序，利用了特定语言框架，例如 Python Flask 或 Ruby Sinatra。

*  服务 (Service)

   服务是 {{site.data.keyword.Bluemix_notm}} 提供的一种工具，可以与应用程序结合使用。
