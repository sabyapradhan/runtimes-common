---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-23"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}


# Mit einem Proxy arbeiten
{: #working_with_proxy}

In einigen Umgebungen wie [{{site.data.keyword.Bluemix_notm}} Dedicated](/docs/dedicated/index.html#dedicated) und
[{{site.data.keyword.Bluemix_notm}} Local](/docs/local/index.html#local) kann ein Proxy konfiguriert werden. Dieser verändert das Verhalten Ihrer Anwendung während des Stagings und der Laufzeit.

Mithilfe der folgenden Umgebungsvariablen können Sie Ihre Anwendung für die Arbeit mit einem Proxy konfigurieren:
  * [http_proxy ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.cloudfoundry.org/buildpacks/proxy-usage.html){: new_window}
  * [https_proxy ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.cloudfoundry.org/buildpacks/proxy-usage.html){: new_window}
  * [no_proxy ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://www.gnu.org/software/wget/manual/html_node/Proxies.html){: new_window}

Diese Umgebungsvariablen können Sie unter Verwendung von *bluemix app env-set* oder über die Datei *manifest.yml* festlegen.  Je nach der Konfiguration der Umgebungsvariablen für den Proxy können Ihre Ressourcen über den Proxy heruntergeladen werden, wenn Ihre Anwendung während des Stagings Ressourcen aus dem Internet herunterlädt. Beispiel: Eine Node.js-Anwendung befindet sich in einer Umgebung, in der `http_proxy` auf `yourProxyURL` gesetzt ist. Sie möchten zulassen, dass `npm` Module aus dem Internet herunterlädt, **jedoch nicht der Proxy**.  Für den Download ohne Verwendung des Proxys setzen Sie `no_proxy` auf `npmjs.org`.

**Hinweis**: Möglicherweise empfiehlt es sich, dass Ihre Anwendung den Proxy während der Laufzeit nach dem Staging verwendet.  Der Laufzeitproxy ist vollständig von der Anwendung abhängig; er ist weder vom Buildpack noch von den Umgebungsvariablen für den Proxy betroffen.

## Java-Anwendungen
{: #java_apps}

Für [Liberty for Java](/docs/runtimes/liberty/index.html) und die [java_buildpack](/docs/runtimes/tomcat/index.html)-Anwendungen können die Proxy-Einstellungen über die Umgebungsvariable **JAVA_OPTS** an die Laufzeit übergeben werden.  Beispielsweise können Sie den Befehl absetzen und anschließend ein erneutes Staging für Ihre Anwendung durchführen:
```
   ibmcloud app env-set myApp JAVA_OPTS "-Dhttp.proxyHost=yourProxyURL -Dhttp.proxyPort=yourProxyPort"
```
{: codeblock}

Ihre Anwendung verwendet dann zur Laufzeit die angegebenen Proxy-Einstellungen. Weitere Informationen zu den Java-Proxy-Optionen finden Sie in [Java-Netzbetrieb und Proxys ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.oracle.com/javase/8/docs/technotes/guides/net/proxies.html){: new_window}.
