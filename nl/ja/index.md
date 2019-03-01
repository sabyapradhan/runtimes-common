---

copyright:
  years: 2017, 2018
lastupdated: "2018-09-21"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}

# 使用可能なビルドパック
{: #available_buildpacks}

* [Liberty for Java](/docs/runtimes/liberty/getting-started.html)
* [SDK for Node.js](/docs/runtimes/nodejs/getting-started.html)
* [ASP.NET core](/docs/runtimes/dotnet/getting-started.html)
* [Runtime for Swift](/docs/runtimes/swift/getting-started.html)
* [XPages](/docs/starters/xpages/index.html)
* [Go](/docs/runtimes/go/getting-started.html)
* [PHP](/docs/runtimes/php/getting-started.html)
* [Python](/docs/runtimes/python/getting-started.html)
* [Ruby](/docs/runtimes/ruby/getting-started.html)
* [Tomcat](/docs/runtimes/tomcat/getting-started.html)

## ビルドパックについて
{: #about_buildpacks}

Cloud Foundry ビルドパックは、Cloud Foundry 環境のアプリケーションに対するランタイム・サポートを提供します。 アプリケーションを {{site.data.keyword.cloud}} にデプロイすると、該当のアプリケーション・タイプをサポートするビルドパックが開始されます。 {{site.data.keyword.cloud_notm}} は、Java EE、Node.js、ASP.Net、Swift、およびその他のアプリケーション・タイプに対する Cloud Foundry ビルドパック・サポートを提供します。
{{site.data.keyword.cloud_notm}} に含まれるビルドパックを使用して、アプリケーションをデプロイし、サービスにバインドすることができます。

*  Cloud Foundry (Cloud Foundry)

    Cloud Foundry は、アプリケーション・ライフサイクル自動化用のオープン・ソースのプラットフォームである。  {{site.data.keyword.Bluemix}} は、Platform as a Service の Cloud Foundry をベースに構築されている。 詳しくは、[Cloud Foundry の資料](https://www.cloudfoundry.org/learn/)を参照。

*  Cloud Foundry アプリケーション (Cloud Foundry Application)

   Cloud Foundry アプリケーション (アプリ) とは、{{site.data.keyword.Bluemix_notm}} によって提供されているビルドパックの 1 つでインスタンス化されるようになっているアプリケーションのことである。

*  ビルドパック (Buildpack)

   ビルドパックは通常、{{site.data.keyword.Bluemix_notm}} によって提供されるソフトウェアの言語固有のパッケージである。 アプリケーションが {{site.data.keyword.Bluemix_notm}} にデプロイされると、そのアプリケーションに適したビルドパックが選択される。 ビルドパックは、アプリケーションのランタイム環境をプロビジョンする。  コマンド・ラインから `ibmcloud cf buildpacks` コマンドを発行して、{{site.data.keyword.Bluemix_notm}} で提供されている一連のビルドパックを表示できる。

*  ランタイム (Runtime)

   ランタイムとは、アプリケーションの実行環境を提供する一連のソフトウェア・コンポーネントである。  用語「*ランタイム (runtime)*」と用語「*ビルドパック (buildpack)*」は、同じ意味で使用されることがある。  ビルドパックでアプリケーションのデプロイが完了すると、ランタイム環境が設定される。

*  ボイラープレート (Boilerplate)

   ボイラープレートとは、特定のランタイム向けに設計されたシンプルなアプリケーションである。  ボイラープレートは、言語およびランタイム固有のアプリケーション・タイプのテンプレートまたはサンプルを提供する。  ボイラープレートをスターター・コードとして使用し、そこから、より高度なアプリケーションの開発を開始できる。  {{site.data.keyword.Bluemix_notm}} では、以下が用意されている。
   * *Hello world ボイラープレート*: 通常、言語およびランタイム固有の「hello world」アプリケーションを提供するシンプルなアプリケーション。
   * *サービスでのボイラープレート*: サービスでランタイムを使用する方法を示す。
   * *フレームワークでのボイラープレート*: 特定の言語フレームワーク (Python Flask や Ruby Sinatra など) を利用するシンプルな言語およびランタイム固有のアプリケーション。

*  サービス (Service)

   サービスは、{{site.data.keyword.Bluemix_notm}} 提供の機能であり、アプリケーションと結合することができる。
