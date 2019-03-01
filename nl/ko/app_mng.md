---

copyright:
  years: 2015, 2018
lastupdated: "2018-10-09"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}

# Liberty 및 Node.js 앱 관리
{: #app_management}


App Management는 {{site.data.keyword.Bluemix}}에서 Liberty 애플리케이션에 사용될 수 있는 개발 및 디버깅 유틸리티 세트입니다.
{:shortdesc}

## App Management 유틸리티
{: #Utilities}

### Liberty 유틸리티
* [proxy](#proxy)
* [noproxy](#noproxy)
* [devconsole](#devconsole)
* [hc](#hc)
* [shell](#shell)
* [debug](#debug)
* [jmx](#jmx)
* [localjmx](#localjmx)

### Node.js 유틸리티(더 이상 사용되지 않음)

**지원 중단 참고**: App Management 유틸리티는 Node.js 애플리케이션에 더 이상 사용되지 않습니다. Node.JS 및 Liberty 애플리케이션 모두에 사용 가능했던 일부 유틸리티는 이제 Liberty 애플리케이션에만 사용 가능합니다. 이 유틸리티는 Node.js 애플리케이션에 더 이상 사용되지 않습니다.

* [proxy](#proxy)
* [noproxy](#noproxy)
* [devconsole](#devconsole)
* [hc](#hc)
* [shell](#shell)
* [inspector](#inspector)
* [trace](#trace)

## App Management를 구성하는 방법
{: #configure}

App Management 유틸리티를 사용하려면 *BLUEMIX_APP_MGMT_ENABLE* 환경 변수의 값을 사용하고자 하는 유틸리티 또는 유틸리티의 목록으로 설정한 후에 애플리케이션을 다시 스테이징하십시오. 유틸리티를 **+**로 구분하여 다수의 유틸리티를 사용할 수 있습니다.

예를 들어, *hc*, *debug* 및 *trace* 유틸리티를 사용하려면 다음 명령을 실행하십시오.

```
ibmcloud cf set-env myApp BLUEMIX_APP_MGMT_ENABLE hc+debug+trace
```
{: codeblock}

환경 변수를 설정한 후에 애플리케이션을 다시 스테이징하십시오.

```
ibmcloud cf restage myApp
```
{: codeblock}

App Management 유틸리티를 사용자 애플리케이션에 설치하지 않으려면 *BLUEMIX_APP_MGMT_INSTALL* 환경 변수를 'false'로 설정하고 애플리케이션을 다시 스테이징하십시오.

예를 들어, App Management 유틸리티 없이 애플리케이션을 스테이징하려면 다음 명령을 실행하십시오.

```
ibmcloud cf set-env myApp BLUEMIX_APP_MGMT_INSTALL false
ibmcloud cf restage myApp
```
{: codeblock}

## 제한사항
{: #restrictions}
* App Management를 사용하여 애플리케이션에 대해 작성된 변경사항은 일시적이며 이 모드를 종료하면 유실됩니다. 이 모드는 임시 개발 전용이며, 성능 문제로 인해 프로덕션 환경으로서 사용되는 용도는 아닙니다.
* Node.js 애플리케이션의 경우, 대부분의 App Management 유틸리티는 `manifest.yml` 파일에 **start** 명령을 설정하거나 명령행에서 `-c` 옵션을 사용하는 경우에는 작동하지 않습니다. 이러한 메소드는 빌드팩 대체이며, Node.js 애플리케이션 시작에 대한 반대 패턴입니다. 최상의 결과를 얻으려면 `package.json` 파일 또는 `Procfile`에서 **시작** 명령을 설정하십시오.


### Liberty 유틸리티
{: #liberty_utilities}

#### jmx
{: #jmx}

*jmx* 유틸리티는 JMX REST 커넥터를 사용하여 원격 JMX 클라이언트가 {{site.data.keyword.Bluemix_notm}} 사용자 인증 정보를 사용하여 애플리케이션을 관리할 수 있도록 허용합니다.

JMX를 사용하여 애플리케이션의 다중 인스턴스를 모니터할 수 있지만, 이를 위해서는 각 인스턴스에 대한 별도의 JMX 연결이 필요합니다. 기본값은 인스턴스 0을 모니터하는 것입니다. 인스턴스 1을 모니터하려면 다음 코드 스니펫을 사용할 수 있습니다.

```
ibmcloud cf ssh -i 1 -N -T -L 5000:127.0.0.1:5001
```
{: codeblock}

JMX 커넥터 구성에 대한 자세한 정보는 [Liberty 프로파일에 대한 보안 JMX 연결 구성 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www-01.ibm.com/support/knowledgecenter/was_beta_liberty/com.ibm.websphere.wlp.nd.multiplatform.doc/ae/twlp_admin_restconnector.html){:new_window}을 참조하십시오.

**중요사항**: *jmx* 유틸리티는 *proxy*를 시작하지 않습니다.

#### localjmx
{: #localjmx}

*localjmx* 유틸리티는 [localConnector-1.0 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://www.ibm.com/support/knowledgecenter/SSEQTP_liberty/com.ibm.websphere.wlp.doc/ae/rwlp_feature_localConnector-1.0.html){:new_window} Liberty 기능을 사용합니다. 로컬 포트 전달과 결합하여 *localjmx*는 원격 JMX 클라이언트가 애플리케이션을 관리할 수 있도록 허용하는 대체 방안으로서의 역할을 합니다.


**시작하기 전에**: *localjmx*를 사용하려면 JConsole을 설치해야 합니다.

*localjmx* 유틸리티는 Diego 셀에서 실행 중인 애플리케이션에만 적용됩니다. *localjmx*를 사용하려면 우선 `ibmcloud cf ssh` 명령을 사용하여 포트 전달을 설정하십시오. 예를 들어, 다음과 같습니다.

```
ibmcloud cf ssh -N -T -L 5000:127.0.0.1:5000 <appName>
```
{: codeblock}

다음으로, JConsole과 연결하려면 **원격 프로세스**를 선택하고 `127.0.0.1:5000`을 지정한 후에 비보안 연결을 사용하십시오.

#### proxy(Node.js에 더 이상 사용되지 않음)
{: #proxy}

*proxy* 유틸리티는 애플리케이션 및 {{site.data.keyword.Bluemix_notm}} 간에 최소한의 애플리케이션 관리를 제공합니다.

이를 사용하는 경우, 빌드팩은 애플리케이션의 런타임 및 컨테이너 사이에 위치하는 프록시 에이전트를 시작합니다.  *proxy* 유틸리티는 애플리케이션이 수신하는 모든 요청을 처리합니다. 요청의 유형에 따라, 이는 App Management 조치를 취하거나 해당 요청을 애플리케이션에 전달합니다. *proxy*를 사용함으로써 애플리케이션 컨테이너는 애플리케이션 장애 시에도 계속해서 작동됩니다. 프록시 에이전트는 또한 Node.js 애플리케이션에 대해 *Live Edit* 모드를 사용하는 점진적 파일 업데이트도 허용합니다.

일부 App Management 유틸리티는 애플리케이션에서 *proxy* 유틸리티를 사용하도록 사용자에게 요구하며, *proxy*를 자동으로 시작할 수 있습니다.

**지원 중단 참고**: Node.js의 경우 Cloud Foundry의 {{site.data.keyword.Bluemix_notm}} 버전은 Diego Cells에서 실행됩니다. *noproxy*는 비Diego 셀을 위한 기능입니다.

#### noproxy(Node.js에 더 이상 사용되지 않음)
{: #noproxy}

다른 유틸리티에 의해 자동으로 시작되는 경우, *noproxy* 유틸리티는 *proxy* 유틸리티를 사용하지 않습니다.  Diego가 애플리케이션에 대해 직접 *ssh*를 실행하고 포트 전달을 설정하는 기능을 제공하므로, Diego에서는 프록시가 필요하지 않습니다.

*noproxy* 유틸리티는 Diego 셀에서 실행되는 애플리케이션에만 적용됩니다.

**지원 중단 참고**: *noproxy* 유틸리티는 *proxy*를 사용 안함으로 설정합니다. *proxy*는 Node.js에 더 이상 사용되지 않고 작동하지 않으므로 *noproxy*는 필요하지 않습니다.

#### devconsole(Node.js에 더 이상 사용되지 않음)
{: #devconsole}

사용자는 (*devconsole*) 개발 콘솔 유틸리티를 사용하여 애플리케이션을 다시 시작, 중지 또는 일시중단할 수 있습니다. 사용자는 또한 *devconsole*을 이용하여 shell 및 inspector 유틸리티를 사용하거나 이에 액세스할 수도 있습니다.  다음 URL을 사용하여 다음에 액세스할 수 있습니다. *devconsole*:s
```
  https://<yourappname>.mybluemix.net/bluemix-debug/manage
```
{: codeblock}

Node 버전 6.3.0 이상의 경우, 개발 콘솔은 애플리케이션에 대한 다시 시작 단추와 *shell* 유틸리티에 대한 액세스를 제공합니다.  자세한 정보는 *검사기* 토론을 참조하십시오.

**중요사항**: *devconsole* 유틸리티는 *proxy*를 시작합니다.

**지원 중단 참고**: Node.js를 위한 *devconsole* 유틸리티를 사용하는 대신 {{site.data.keyword.Bluemix_notm}} 콘솔을 사용하십시오. 콘솔에서 애플리케이션 **런타임** 페이지로 이동하십시오. **런타임** 페이지에서 애플리케이션에 대한 중지, 시작, 이름 바꾸기 및 삭제를 수행할 수 있습니다. 또한 쉘 및 기타 정보에 액세스할 수 있습니다.

#### hc(Node.js에 더 이상 사용되지 않음)
{: #hc}

(*hc*) Health Center 에이전트는 애플리케이션이 Health Center 클라이언트에 의해 모니터링될 수 있도록 합니다.  Node.js의 경우, *hc* 에이전트는 IBM SDK for Node.js 빌드팩에 포함된 Node.js 런타임 버전에서만 사용 가능합니다.  런타임의 현재 설정은 [sdk-for-nodejs 빌드팩의 최신 업데이트](/docs/runtimes/nodejs/updates.html)를 참조하십시오.

Health Center 에이전트가 사용되는 경우, IBM Monitoring and Diagnostic Tools를 사용하여 Liberty 및 Node.js 애플리케이션의 성능을 분석할 수 있습니다. 자세한 정보는 [{{site.data.keyword.Bluemix_notm}}에서 Liberty Java 또는 Node.js 앱의 성능을 분석하는 방법 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://developer.ibm.com/bluemix/2015/07/03/how-to-analyze-performance-in-bluemix/){:new_window}을 참조하십시오.

**중요사항:** *hc* 유틸리티는 *proxy*를 시작합니다.

***noproxy*와 함께 *hc* 사용**

*hc* 유틸리티를 *noproxy*와 함께 사용할 수 있습니다. Health Center를 *noproxy*와 함께 사용하려면 우선 `ibmcloud cf ssh` 명령을 사용하여 포트 전달을 설정하십시오. 예를 들어, 다음과 같습니다.

```
ibmcloud cf ssh -N -T -L 1883:127.0.0.1:1883 <appName>
```
{: codeblock}

다음으로, Health Center 클라이언트와 연결하려면 [MQTT 연결 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://www.ibm.com/support/knowledgecenter/SS3KLZ/com.ibm.java.diagnostics.healthcenter.doc/topics/connectingtojvm.html){: new_window}을 사용하여 호스트를 `127.0.0.1`로 지정하고 포트를 `1883`으로 지정하십시오.

**지원 중단 참고:** Node.js를 위한 Health Center 에이전트를 사용하는 대신 {{site.data.keyword.Bluemix_notm}} 콘솔을 사용하십시오. 콘솔에서 CPU 사용량, 메모리 사용량 및 디스크 공간에 액세스할 수 있는 애플리케이션 **런타임** 페이지로 이동하십시오. 페이지에는 환경 변수에 액세스할 수 있는 탭도 있습니다.

#### shell(Node.js에 더 이상 사용되지 않음)
{: #shell}

*shell* 유틸리티는 웹 기반 쉘을 사용합니다.  *devconsole* 유틸리티에서 또는 다음의 URL을 사용하여 *shell*에 액세스할 수 있습니다.

```
  https://<yourappname>.mybluemix.net/bluemix-debug/shell
```
{: codeblock}

*shell* 유틸리티에 액세스되면 터미널 창이 애플리케이션에 대한 쉘 액세스와 함께 표시됩니다. 파일 편집, 메모리 사용량 확인 또는 진단 명령 실행 등과 같은 일반적인 쉘에서 지원되는 모든 작업을 수행할 수 있습니다.

**중요사항:** *shell* 유틸리티는 *proxy*도 시작합니다.

Diego에서 `ibmcloud cf ssh` 명령을 통해 대화식 쉘을 제공하므로, *shell* 유틸리티는 DEA에서 실행 중인 애플리케이션에만 유용합니다.
{: .tip}

**지원 중단 참고:** Node.js의 경우 `cf ssh` 명령이 계속 작동되는 동안 {{site.data.keyword.Bluemix_notm}} 콘솔의 쉘에 액세스할 수도 있습니다. 콘솔의 웹 페이지에서 쉘을 풀업하는 탭을 찾을 수 있는 애플리케이션 **런타임** 페이지로 이동하십시오.

### Node.js 유틸리티(더 이상 사용되지 않음)
{: #node_utilities}

#### inspector(더 이상 사용되지 않음)
{: #inspector}

inspector 유틸리티를 사용하면 CPU 사용량 프로파일 작성, 중단점 추가, 코드 디버그를 수행할 수 있으며, 이 모두를 애플리케이션이 {{site.data.keyword.cloud_notm}}에서 실행 중인 동안에 수행할 수 있습니다.  6.3.0 이전의 Node.js 버전의 경우, inspector는 노드 검사기 디버거 인터페이스를 사용합니다.  노드 검사기에 대한 자세한 정보는 [GitHub의 node-inspector ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/node-inspector/node-inspector){: new_window}에 대한 Readme를 참조하십시오.  Node.js 버전 6.3.0 이상의 경우, *inspector* 유틸리티는 [V8 Inspector Integration for Node.js ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://nodejs.org/dist/latest-v6.x/docs/api/debugger.html#debugger_v8_inspector_integration_for_node_js){:new_window}를 사용합니다.

##### 6.3.0 이후의 Node.js 버전의 경우
디버깅 모드가 시작되면 *proxy*가 포함되지 않은 Node.js 버전을 사용 중인 경우에도 *proxy*가 자동으로 사용됩니다. 6.3.0 이후의 Node.js 버전에는 *proxy*가 포함되지 않습니다. 6.3.0 이후의 Node.js 버전에서 *inspector* 유틸리티를 사용하는 경우에는 *noproxy*를 사용하여 다시 *proxy*를 끌 수 있습니다.

*proxy*를 사용하여 *inspector* 인터페이스에 액세스하는 대신에 사용자는 Google Chrome 웹 브라우저의 Developer Tools 기능을 사용합니다.  

다음 명령으로 로컬 포트 전달을 사용하여 URL에 대한 액세스를 사용하십시오.
```
ibmcloud cf ssh -N -T -L 9229:127.0.0.1:9229 <appName>
```
{: codeblock}

다음 명령을 사용하여 애플리케이션에 대한 스타트업 로그를 가져오십시오.
```
ibmcloud cf logs <appName> --recent
```
{: codeblock}

*inspector* 유틸리티가 활성인 경우, 로그에는 다음과 유사한 메시지가 포함됩니다.
```
 ... You will need a SSH tunnel for port 9229 to be able to use the Chrome DevTools to remotely debug your app
 ... Starting app with 'node --inspect=9229  app.js '
```
{: codeblock}

Google Chrome 웹 브라우저의 최신 버전을 사용하여 `chrome://inspect`를 찾아보십시오.
이 URL에서 `file://home/vcap/app/app.js` 등의 애플리케이션 파일에 대한 링크와 함께 나열된 앱을 본 후에 **inspect**를 선택하여 inspect 인터페이스에 액세스하십시오.

##### 6.3.0 이전의 Node.js 버전의 경우
*proxy*를 사용하는 경우에는 `https://myApp.mybluemix.net/bluemix-debug/inspector`에서 *inspector* 인터페이스에 액세스할 수 있습니다.

*proxy* 유틸리티를 사용하지 않는 경우에는 다음 명령으로 로컬 포트 전달을 사용하여 애플리케이션 URL에 대한 액세스를 사용하십시오.

```
ibmcloud cf ssh -N -T -L 8790:127.0.0.1:8790 <appName>
```
{: codeblock}

그리고 URL `http://127.0.0.1:8790`에서 inspector에 액세스하십시오.

**지원 중단 참고:** 설정을 약간만 변경하여 *inspector* 기능을 계속해서 사용할 수 있습니다. 애플리케이션 `manifest.yml` 파일에서 시작 명령을 'node --inspect=9229 app.js'로 설정하십시오. 그런 다음 6.3.0 이후의 Node.js 버전에 대한 지시사항을 따를 수 있습니다. 그러나 일반적으로 검사기가 활성 상태임을 나타내는 로그는 표시되지 않지만 올바르게 작동합니다.


#### trace(더 이상 사용되지 않음)
{: #trace}

trace 유틸리티를 사용하면 애플리케이션이 log4js, ibmbluemix 또는 bunyan 로깅 모듈을 사용하는 경우에 사용자가 추적 레벨을 동적으로 설정할 수 있습니다.

{{site.data.keyword.cloud_notm}} 웹 콘솔의 **인스턴스 세부사항** 페이지로 이동하고 **조치**를 선택하여 UI를 보십시오.

참고: 지원되는 종속 버전은 다음과 같습니다.

* log4js:(0.6.0 - 0.6.24)
* bunyan: (1.0.0, 1.0.1, 1.1.0 - 1.1.3, 1.2.0 - 1.2.3, 1.3.0 - 1.3.5)
* ibmbluemix: (1.0.0-20140707-1250)-(1.0.0-20150409-1328)


애플리케이션이 `-b buildpack` 옵션을 사용하여 시작할 때는 trace 유틸리티를 사용할 수 없습니다.

trace 유틸리티는 *proxy*를 시작하지 않습니다.
