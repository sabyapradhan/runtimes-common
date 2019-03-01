---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-23"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}


# 使用 Proxy
{: #working_with_proxy}

在某些環境（例如 [{{site.data.keyword.Bluemix_notm}} Dedicated](/docs/dedicated/index.html#dedicated) 及
[{{site.data.keyword.Bluemix_notm}} Local](/docs/local/index.html#local)中，配置的 Proxy 可能會影響應用程式在編譯打包及運行期間的行為。

您可以使用下列環境變數，以配置應用程式來使用 Proxy：
  * [http_proxy ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.cloudfoundry.org/buildpacks/proxy-usage.html){: new_window}
  * [https_proxy ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.cloudfoundry.org/buildpacks/proxy-usage.html){: new_window}
  * [no_proxy ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](http://www.gnu.org/software/wget/manual/html_node/Proxies.html){: new_window}

您可以使用 *bluemix app env-set* 或透過 *manifest.yml* 檔案來設定這些環境變數。視 Proxy 環境變數的配置方式而定，如果您的應用程式在編譯打包期間從網際網路下載資源，則可能會使用 Proxy 來下載您的資源。比方說，如果您在 `http_proxy` 設為 `yourProxyURL` 的環境中有 Nodejs 應用程式，而且想要讓 `npm` 從網際網路（**而不是 Proxy**）下載模組。若要下載而不使用 Proxy，請將 `no_proxy` 設為 `npmjs.org`。

**附註**：您可能想要在運行期間於編譯打包之後使用 Proxy。運行環境的 Proxy 使用完全取決於應用程式，而不受建置套件或 Proxy 環境變數所影響。

## Java 應用程式
{: #java_apps}

若為 [Liberty for Java](/docs/runtimes/liberty/index.html) 及 [java_buildpack](/docs/runtimes/tomcat/index.html) 應用程式，可以透過 **JAVA_OPTS** 環境變數將 Proxy 設定傳遞給運行環境。例如，您可以發出指令，然後重新編譯打包您的應用程式：
```
   ibmcloud app env-set myApp JAVA_OPTS "-Dhttp.proxyHost=yourProxyURL -Dhttp.proxyPort=yourProxyPort"
```
{: codeblock}

您的應用程式接著會使用在運行環境指定的 Proxy 設定。如需 Java Proxy 選項的相關資訊，請參閱 [Java 網路及 Proxy ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.oracle.com/javase/8/docs/technotes/guides/net/proxies.html){: new_window}。
