---

copyright:
  years: 2016, 2018
lastupdated: "2018-11-20"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}

# ビルドパックのサポート・ステートメント
{: #buildpack_support_statement}


## 組み込みの IBM ビルドパック
{: #built-in_ibm_buildpacks}

[Liberty for Java](/docs/runtimes/liberty/index.html)、[SDK for Node.js](/docs/runtimes/nodejs/index.html)、および [ASP.NET Core](/docs/runtimes/dotnet/index.html) については、IBM は 2 つのバージョン (n および n - 1) (例えば、IBM Liberty Buildpack v3.12 および IBM Liberty Buildpack v3.11) をサポートします。 各ビルドパックは、必要に応じて、対応するランタイムの 1 つ以上のメジャー・バージョンを提供し、サポートします。 ビルドパックは通常、使用可能なランタイムの最新のマイナー・バージョンで、1 カ月に 1 回更新されます。

[{{site.data.keyword.Bluemix_notm}} Runtime for Swift](/docs/runtimes/swift/index.html) については、IBM は、[Swift.org](http://swift.org) で入手できる最新バージョンの Swift に適合したビルドパックのサポートを提供しています。 ビルドパックに対する更新は、入手可能な最新リリースの Swift バージョンと同期して行われます。

{{site.data.keyword.Bluemix_notm}} 上で現在サポートされているすべてのバージョンの組み込み IBM ビルドパックについて問題が報告される可能性がありますが、それらの問題は最新バージョンに対して検証されます。 欠陥が検出された場合、IBM は将来レベルのランタイムでのフィックスと、対応するビルドパックを提供します。 IBM は、前のメジャー・バージョンおよびマイナー・バージョン (N-1、n-1) 用のフィックスは提供しません。 IBM は、IBM ビルドパックを使用している場合でも (例えば、Liberty ビルドパックと共に Open JDK を使用している場合など)、コミュニティー・ランタイムをサポートすることはありません。 これらのコミュニティー・ランタイムは、次のセクションで説明されているように、組み込みのコミュニティー・ビルドパックと同じサポート・ポリシーに従います。

## 組み込みのコミュニティー・ビルドパック
{: #built-in_community_buildpacks}

以下の組み込みコミュニティー・ビルドパックが Cloud Foundry コミュニティーによって提供されています。

* [Java](/docs/runtimes/tomcat/index.html)
* [Node.js](https://github.com/cloudfoundry/nodejs-buildpack)
* [PHP](/docs/runtimes/php/index.html)
* [Ruby](/docs/runtimes/ruby/index.html)
* [Python](/docs/runtimes/python/index.html)
* [Go](/docs/runtimes/go/index.html)

これらのビルドパックに対する更新は、{{site.data.keyword.Bluemix_notm}} が Cloud Foundry の新バージョンにアップグレードされたときに行われます。 {{site.data.keyword.Bluemix_notm}} 上のこれらのランタイムでの問題を IBM に報告して、{{site.data.keyword.Bluemix_notm}} がその問題の原因かどうかを判別するための支援を受けることができます。 {{site.data.keyword.Bluemix_notm}} に関連する問題であれば IBM はフィックスを提供しますが、ビルドパックまたはランタイム自体の欠陥の場合、IBM は問題を該当のコミュニティーに報告することを支援します。 それらのビルドパックおよびランタイム用のフィックスを IBM が提供することはありません。

## 外部ビルドパック
{: #external_buildpacks}

IBM は外部ビルドパックのサポートを提供しません。 サポートについては Cloud Foundry コミュニティーに問い合わせる必要があります。

## サード・パーティー・サービス
{: #third-party}

ビルドパックにより、IBM 以外のいくつかのサード・パーティー・サービス (Dynatrace や New Relic など) をアプリケーション内で使用できます。 IBM は、サード・パーティー・サービスに関するサポートは提供しません。 IBM Cloud でサード・パーティー・サービスを使用する方法については、最新の [IBM Cloud サービス記述書 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www-03.ibm.com/software/sla/sladb.nsf/sla/bm) の「_クラウド・サービスの使用_ 」を参照してください。 サード・パーティー・サービスを使用する前に、サービス・プロバイダーのライセンス情報を調べてください。
