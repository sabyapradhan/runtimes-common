---

copyright:
  years: 2016, 2018
lastupdated: "2018-11-20"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}

# Buildpack 支持声明
{: #buildpack_support_statement}


## 内置 IBM buildpack
{: #built-in_ibm_buildpacks}

对于 [Liberty for Java](/docs/runtimes/liberty/index.html)、[SDK for Node.js](/docs/runtimes/nodejs/index.html) 和 [ASP.NET Core](/docs/runtimes/dotnet/index.html)，IBM 将支持两个版本（n 和 n-1），例如 IBM Liberty Buildpack V3.12 和 IBM Liberty Buildpack V3.11。每个 buildpack 将根据需要提供并支持其相应运行时的一个或多个主版本。通常，每月会使用可用的运行时最新次版本对 buildpack 刷新一次。

对于 [{{site.data.keyword.Bluemix_notm}} Runtime for Swift](/docs/runtimes/swift/index.html)，IBM 会为与 [Swift.org](http://swift.org) 上提供的最新版 Swift 匹配的 buildpack 提供支持。对 buildpack 的更新会与 Swift 的已发布最新可用版本同步。

与 {{site.data.keyword.Bluemix_notm}} 上当前支持的任何内置 IBM Buildpack 版本相关的问题都可以报告，但这些问题将根据最新版本进行验证。如果发现缺陷，IBM 将在运行时的未来级别以及相应的 buildpack 中提供修订。IBM 不会对更低的主版本和次版本（N-1 和 n-1）提供修订。IBM 不支持社区运行时，即便在使用 IBM buildpack（例如，将 Open JDK 与 Liberty buildpack 配合使用）时也不例外。这些社区运行时与内置的社区 buildpack 遵循相同的支持策略，如以下部分所述。

## 内置社区 buildpack
{: #built-in_community_buildpacks}

Cloud Foundry 社区提供了以下内置社区 Buildpack：

* [Java](/docs/runtimes/tomcat/index.html)
* [Node.js](https://github.com/cloudfoundry/nodejs-buildpack)
* [PHP](/docs/runtimes/php/index.html)
* [Ruby](/docs/runtimes/ruby/index.html)
* [Python](/docs/runtimes/python/index.html)
* [Go](/docs/runtimes/go/index.html)

在将 {{site.data.keyword.Bluemix_notm}} 升级到新版本的 Cloud Foundry 时，会对这些 buildpack 进行更新。有关在 {{site.data.keyword.Bluemix_notm}} 上使用这些运行时遇到的问题，可以报告给 IBM，我们会帮助您确定 {{site.data.keyword.Bluemix_notm}} 是否是问题的根源。对于与 {{site.data.keyword.Bluemix_notm}} 相关的问题，IBM 会提供修订，而对于 buildpack 或运行时本身的缺陷，IBM 会帮助将其报告给相应的社区。IBM 不会对这些 buildpack 和运行时提供修订。

## 外部 buildpack
{: #external_buildpacks}

对于外部 buildpack，IBM 不会提供支持。您可能需要联系 Cloud Foundry 社区来获取支持。

## 第三方服务
{: #third-party}

通过 buildpack，您可以在应用程序内使用一些非 IBM 的第三方服务，例如 Dynatrace 或 New Relic。对于第三方服务，IBM 不提供支持。有关在 IBM Cloud 中使用第三方服务的信息，请参阅最新 [IBM Cloud 服务描述 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www-03.ibm.com/software/sla/sladb.nsf/sla/bm) 中的_云服务使用_。在使用第三方服务之前，请查阅服务提供商提供的许可信息。
