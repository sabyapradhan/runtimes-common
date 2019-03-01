---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-23"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}


# 使用代理
{: #working_with_proxy}

在某些环境（例如，[{{site.data.keyword.Bluemix_notm}} Dedicated](/docs/dedicated/index.html#dedicated) 和 [{{site.data.keyword.Bluemix_notm}} Local](/docs/local/index.html#local)中，代理的配置可能会影响应用程序在编译打包和运行时期间的行为。

可以通过使用以下环境变量，将应用程序配置为使用相应代理：
  * [http_proxy ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://docs.cloudfoundry.org/buildpacks/proxy-usage.html){: new_window}
  * [https_proxy ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://docs.cloudfoundry.org/buildpacks/proxy-usage.html){: new_window}
  * [no_proxy ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](http://www.gnu.org/software/wget/manual/html_node/Proxies.html){: new_window}

可以使用 *bluemix app env-set* 或通过 *manifest.yml* 文件来设置这些环境变量。如果应用程序在编译打包期间从因特网下载资源，那么资源可能会使用代理进行下载，具体取决于代理环境变量的配置方式。例如，如果在 `http_proxy` 设置为 `yourProxyURL` 的环境中有 Nodejs 应用程序，并且您希望允许 `npm` 从因特网下载模块，**而不从代理**下载模块。要在不使用代理的情况下进行下载，请将 `no_proxy` 设置为 `npmjs.org`。

**注**：您可能希望应用程序在编译打包后的运行时期间使用代理。运行时代理的使用完全取决于应用程序，并且不受 buildpack 或代理环境变量的影响。

## Java 应用程序
{: #java_apps}

对于 [Liberty for Java](/docs/runtimes/liberty/index.html) 和 [java_buildpack](/docs/runtimes/tomcat/index.html) 应用程序，可以通过 **JAVA_OPTS** 环境变量将代理设置传递到运行时。例如，可以发出以下命令，然后重新编译打包应用程序：
```
   ibmcloud app env-set myApp JAVA_OPTS "-Dhttp.proxyHost=yourProxyURL -Dhttp.proxyPort=yourProxyPort"
```
{: codeblock}

随后，应用程序会在运行时使用指定的代理设置。有关 Java 代理选项的更多信息，请参阅 [Java Networking and Proxies ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://docs.oracle.com/javase/8/docs/technotes/guides/net/proxies.html){: new_window}。
