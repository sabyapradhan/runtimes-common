---

copyright:
  years: 2017, 2018
lastupdated: "2018-09-21"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}

# 사용 가능한 빌드팩
{: #available_buildpacks}

* [Liberty for Java](/docs/runtimes/liberty/getting-started.html)
* [SDK for Node.js](/docs/runtimes/nodejs/getting-started.html)
* [ASP.NET Core](/docs/runtimes/dotnet/getting-started.html)
* [Runtime for Swift](/docs/runtimes/swift/getting-started.html)
* [XPages](/docs/starters/xpages/index.html)
* [Go](/docs/runtimes/go/getting-started.html)
* [PHP](/docs/runtimes/php/getting-started.html)
* [Python](/docs/runtimes/python/getting-started.html)
* [Ruby](/docs/runtimes/ruby/getting-started.html)
* [Tomcat](/docs/runtimes/tomcat/getting-started.html)

## 빌드팩 정보
{: #about_buildpacks}

Cloud Foundry 빌드팩은 Cloud Foundry 환경의 애플리케이션에 대한 런타임 지원을 제공합니다. {{site.data.keyword.cloud}}에 애플리케이션을 배치하는 경우, 이는 애플리케이션 유형을 지원하는 빌드팩을 시작합니다. {{site.data.keyword.cloud_notm}}에서는 Java EE, Node.js, ASP.Net, Swift 및 기타 애플리케이션 유형에 대한 Cloud Foundry 빌드팩 지원을 제공합니다.
{{site.data.keyword.cloud_notm}}에 포함된 빌드팩을 사용하여 애플리케이션을 배치하고 이를 서비스에 바인드할 수 있습니다.

*  Cloud Foundry

    Cloud Foundry는 애플리케이션 라이프사이클 자동화를 위한 오픈 소스 플랫폼입니다.  {{site.data.keyword.Bluemix}}는 Cloud Foundry PaaS(Platform as a Service)를 기반으로 구축되었습니다. 자세한 내용은 [Cloud Foundry 문서](https://www.cloudfoundry.org/learn/)를 참조하십시오.

*  Cloud Foundry 애플리케이션

   Cloud Foundry 애플리케이션 또는 앱은 {{site.data.keyword.Bluemix_notm}}가 제공하는 빌드팩 중 하나가 인스턴스화하려고 하는 애플리케이션입니다.

*  빌드팩

   빌드팩은 일반적으로 {{site.data.keyword.Bluemix_notm}}에서 제공하는 소프트웨어의 언어별 패키지입니다. 애플리케이션이 {{site.data.keyword.Bluemix_notm}}에 배치되면 애플리케이션에 적합한 빌드팩이 선택됩니다. 빌드팩은 애플리케이션의 런타임 환경을 프로비저닝합니다.  명령행에서 `ibmcloud cf buildpacks` 명령을 실행하여 {{site.data.keyword.Bluemix_notm}}에서 제공하는 빌드팩 세트를 볼 수 있습니다.

*  런타임

   런타임은 애플리케이션에 대한 실행 환경을 제공하는 소프트웨어 컴포넌트 세트입니다.  용어 *런타임*과 *빌드팩*은 때때로 서로 바뀌어 사용됩니다.  빌드팩이 애플리케이션 배치를 완료하면 런타임 환경이 설정됩니다.

*  표준 유형

   표준 유형은 특정 런타임에 맞게 설계된 단순 애플리케이션입니다.  표준 유형은 언어 및 런타임별 애플리케이션 유형의 샘플 또는 템플리트를 제공합니다.  표준 유형을 스타터 코드로 사용하여 복잡한 애플리케이션 개발을 시작할 수 있습니다.  {{site.data.keyword.Bluemix_notm}}는 다음과 같은 표준 유형을 제공합니다.
   * *Hello world 표준 유형*: 일반적으로 언어 및 런타임 특정 'hello world' 애플리케이션을 제공하는 단순 애플리케이션입니다.
   * *서비스의 표준 유형*: 서비스에서 런타임을 사용하는 방법을 보여줍니다.
   * *프레임워크의 표준 유형*: Python Flask 또는 Ruby Sinatra 등의 특정 언어 프레임워크를 활용하는 단순 언어 및 런타임 특정 애플리케이션입니다.

*  서비스

   서비스는 애플리케이션과 연결할 수 있는 {{site.data.keyword.Bluemix_notm}} 제공 기능입니다.
