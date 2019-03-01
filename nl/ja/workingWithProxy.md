---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-23"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}


# プロキシーの処理
{: #working_with_proxy}

[{{site.data.keyword.Bluemix_notm}} Dedicated](/docs/dedicated/index.html#dedicated) や
[{{site.data.keyword.Bluemix_notm}} Local](/docs/local/index.html#local) などの一部の環境では、
ステージングおよび実行時のアプリケーションの動作に影響するプロキシーを構成できます。

以下の環境変数を使用して、プロキシーで動作するようにアプリケーションを構成できます。
  * [http_proxy ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.cloudfoundry.org/buildpacks/proxy-usage.html){: new_window}
  * [https_proxy ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.cloudfoundry.org/buildpacks/proxy-usage.html){: new_window}
  * [no_proxy ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](http://www.gnu.org/software/wget/manual/html_node/Proxies.html){: new_window}

これらの環境変数は、*bluemix app env-set* を使用して、または *manifest.yml* ファイルで設定できます。  プロキシー環境変数の構成方法によっては、アプリケーションがステージング中にインターネットからリソースをダウンロードする場合に、リソースがプロキシーを使用してダウンロードされることがあります。 例えば、`http_proxy` が `yourProxyURL` に設定された環境に Nodejs アプリケーションがあって、**プロキシーではなく**、`npm` でインターネットからモジュールをダウンロードできるようにしたい場合があります。  プロキシーを使用しないでダウンロードするには、`no_proxy` を `npmjs.org` に設定します。

**注**: ステージングの後、実行時にアプリケーションがプロキシーを使用するようにする場合があります。  実行時のプロキシーの使用は、完全にアプリケーションに依存するもので、ビルドパックおよびプロキシー環境変数の影響は受けません。

## Java アプリケーション
{: #java_apps}

[Liberty for Java](/docs/runtimes/liberty/index.html) および [java_buildpack](/docs/runtimes/tomcat/index.html) アプリケーションでは、**JAVA_OPTS** 環境変数でプロキシー設定をランタイムに渡すことができます。  例えば、以下のコマンドを実行した後、アプリケーションを再ステージングすることができます。
```
   ibmcloud app env-set myApp JAVA_OPTS "-Dhttp.proxyHost=yourProxyURL -Dhttp.proxyPort=yourProxyPort"
```
{: codeblock}

これにより、アプリケーションは、指定されたプロキシー設定を実行時に使用します。 Java プロキシー・オプションについて詳しくは、[「Java Networking and Proxies」![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.oracle.com/javase/8/docs/technotes/guides/net/proxies.html){: new_window} を参照してください。
