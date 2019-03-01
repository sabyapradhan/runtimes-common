---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-23"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}


# 프록시 작업
{: #working_with_proxy}

[{{site.data.keyword.Bluemix_notm}} Dedicated](/docs/dedicated/index.html#dedicated) 및
[{{site.data.keyword.Bluemix_notm}} Local](/docs/local/index.html#local) 등의 일부 환경에서는
스테이징 및 런타임 중에 애플리케이션의 작동에 영향을 주는 프록시의 구성이 가능합니다.

다음의 환경 변수를 사용하여 프록시에 대해 작업할 수 있도록 애플리케이션을 구성할 수 있습니다.
  * [http_proxy ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.cloudfoundry.org/buildpacks/proxy-usage.html){: new_window}
  * [https_proxy ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.cloudfoundry.org/buildpacks/proxy-usage.html){: new_window}
  * [no_proxy ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://www.gnu.org/software/wget/manual/html_node/Proxies.html){: new_window}

*bluemix app env-set*를 사용하거나 *manifest.yml* 파일을 통해 이러한 환경 변수를 설정할 수 있습니다.  프록시 환경 변수를 구성하는 방법에 따라, 애플리케이션이 스테이징 중에 인터넷에서 리소스를 다운로드하는 경우 프록시를 사용한 리소스의 다운로드가 가능합니다. 예를 들어, `http_proxy`가 `yourProxyURL`로 설정된 환경에 Nodejs 애플리케이션이 있으며 `npm`이 **프록시가 아닌** 인터넷에서 모듈을 다운로드할 수 있도록 허용하고자 한다고 가정합니다.  프록시를 사용하지 않고 다운로드하려면 `no_proxy`를 `npmjs.org`로 설정하십시오.

**참고**: 스테이징 이후 런타임 중에 애플리케이션이 프록시를 사용하기를 원할 수 있습니다.  런타임 프록시 사용은 애플리케이션에 전적으로 의존하며, 빌드팩 또는 프록시 환경 변수의 영향을 받지 않습니다.

## Java 애플리케이션
{: #java_apps}

[Liberty for Java](/docs/runtimes/liberty/index.html) 및 [java_buildpack](/docs/runtimes/tomcat/index.html) 애플리케이션의 경우, 프록시 설정은 **JAVA_OPTS** 환경 변수를 통해 런타임에 전달될 수 있습니다.  예를 들어, 명령을 실행한 후에 애플리케이션을 다시 스테이징할 수 있습니다.
```
   ibmcloud app env-set myApp JAVA_OPTS "-Dhttp.proxyHost=yourProxyURL -Dhttp.proxyPort=yourProxyPort"
```
{: codeblock}

그리고 애플리케이션은 런타임에 지정된 프록시 설정을 사용합니다. Java 프록시 옵션에 대한 자세한 정보는 [Java 네트워킹 및 프록시 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.oracle.com/javase/8/docs/technotes/guides/net/proxies.html){: new_window}를 참조하십시오.
