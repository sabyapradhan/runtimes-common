---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-23"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}


# Working with a proxy
{: #working_with_proxy}

In some environments such as [{{site.data.keyword.Bluemix_notm}} Dedicated](/docs/dedicated/index.html#dedicated) and
[{{site.data.keyword.Bluemix_notm}} Local](/docs/local/index.html#local) a proxy may be configured which affects the
behavior of your application during staging and runtime.

You can configure your application to work with the proxy by using the following environment variables:
  * [http_proxy ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://docs.cloudfoundry.org/buildpacks/proxy-usage.html){: new_window}
  * [https_proxy ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://docs.cloudfoundry.org/buildpacks/proxy-usage.html){: new_window}
  * [no_proxy ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://www.gnu.org/software/wget/manual/html_node/Proxies.html){: new_window}

You can set these environment variables using *bluemix app env-set* or via the *manifest.yml* file.  Depending on how you configure your proxy environment variables, if your application downloads resources from the internet during staging, your resources might download using the proxy. For example, if you have a Nodejs application in an environment with `http_proxy` set to `yourProxyURL` and you want to allow `npm` to download modules from the internet **but not the proxy**.  To download without using the proxy, set `no_proxy` to `npmjs.org`.

**Note**: You might want your application to use the proxy during runtime, after staging.  Runtime proxy use is entirely dependent on the application and is not affected by either the buildpack or the proxy environment variables.

## Java applications
{: #java_apps}

For [Liberty for Java](/docs/runtimes/liberty/index.html) and the [java_buildpack](/docs/runtimes/tomcat/index.html) applications, the proxy settings can be passd to the runtime via the **JAVA_OPTS** environment variable.  For example you can issue the command and then restage your application:
```
   ibmcloud app env-set myApp JAVA_OPTS "-Dhttp.proxyHost=yourProxyURL -Dhttp.proxyPort=yourProxyPort"
```
{: codeblock}

Your application then uses the specified proxy settings at runtime. See [Java Networking and Proxies ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://docs.oracle.com/javase/8/docs/technotes/guides/net/proxies.html){: new_window} for more information about the Java proxy options.
