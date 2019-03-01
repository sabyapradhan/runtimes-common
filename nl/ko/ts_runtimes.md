---

copyright:
  years: 2015, 2018
lastupdated: "2018-12-05"

---

{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:new_window: target="_blank"}


# 런타임 문제점 해결
{: #runtimes}

[{{site.data.keyword.Bluemix}} 런타임](index.html)을 사용할 때 문제점이 발생할 수 있습니다. 대부분 몇 가지 간단한 단계를 수행하여 이러한 문제점에서 복구할 수 있습니다.
{:shortdesc}

* [일반 문제점 해결](#ts_all)
* [Liberty for Java](#ts_liberty)
* [SDK for Node.js](#ts_nodejs)
* [ASP.NET Core](#ts_dotnet)
* [PHP](#ts_php)
* [Python](#ts_python)

## 일반 문제점 해결
{: #ts_all}

### 앱을 푸시할 때 더 이상 사용되지 않는 빌드팩이 사용됨
{: #ts_loading_bp}

앱을 푸시할 때 최신 빌드팩 컴포넌트를 사용하지 못할 수 있습니다. 더 이상 사용되지 않는 컴포넌트가 로드되지 않도록 기본 제공되는 메커니즘이 있는 빌드팩을 사용하거나, 앱을 푸시하거나 다시 스테이징하기 전에 앱의 캐시 디렉토리에서 컨텐츠를 삭제할 수 있습니다.

빌드팩을 업데이트한 후 앱을 푸시하거나 다시 스테이징할 때 최신 빌드팩 컴포넌트가 자동으로 로드되지 않습니다. 결과적으로 앱은 캐시에서 더 이상 사용되지 않는 빌드팩 컴포넌트를 사용합니다. 마지막으로 앱을 푸시한 이후 빌드팩에 적용한 업데이트는 구현되지 않습니다.
{: tsSymptoms}

일부 빌드팩은 인터넷에서 업데이트된 모든 컴포넌트를 자동으로 다운로드하여 항상 최신 버전을 사용하도록 구성되지 않습니다.
{: tsCauses}

더 이상 사용되지 않는 컴포넌트가 로드되지 않도록 기본 제공 메커니즘이 있는 빌드팩을 사용할 수 있습니다. 예를 들어 다음 빌드팩을 사용합니다.
{: tsResolve}

  * [Cloud Foundry Java 빌드팩 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/cloudfoundry/java-buildpack){: new_window}. 최신 버전의 빌드팩을 사용하도록 이 빌드팩에는 기본 제공 메커니즘이 포함되어 있습니다. 이 메커니즘이 작동하는 방식에 대한 자세한 정보는 [extending-caches.md ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/cloudfoundry/java-buildpack/blob/master/docs/extending-caches.md){: new_window}를 참조하십시오.
  * [Cloud Foundry Node.js 빌드팩 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/cloudfoundry/nodejs-buildpack){: new_window}. 이 빌드팩은 환경 변수를 사용하여 유사한 기능을 제공합니다. Node.js 빌드팩이 항상 인터넷에서 노드 모듈을 다운로드할 수 있도록 하려면 {{site.data.keyword.Bluemix_notm}} 명령행 인터페이스에서 다음 명령을 입력하십시오. 	

  ```
  set NODE_MODULES_CACHE=false
  ```

사용 중인 빌드팩이 최신 컴포넌트를 자동으로 로드하는 메커니즘을 제공하지 않는 경우 캐시 디렉토리에서 해당 컨텐츠를 수동으로 삭제하고 앱을 다시 푸시할 수 있습니다. 다음 단계를 수행하십시오.

 1. 널 빌드팩의 분기를 체크아웃합니다. 예: https://github.com/ryandotsmith/null-buildpack 분기를 체크아웃하는 방법에 대한 자세한 정보는 [Git Basics - Git 저장소 가져오기 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://www.git-scm.com/book/en/v2/Git-Basics-Getting-a-Git-Repository){: new_window}를 참조하십시오.  
 2. `null-buildpack/bin/compile` 파일에 다음 행을 추가하고 변경사항을 커미트합니다. 변경사항을 커미트하는 방법에 대한 자세한 정보는 [Git Basics - 저장소에 변경사항 기록 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://www.git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository){: new_window}을 참조하십시오.
  ```
  rm -rfv $2/*
  ```
 3. 다음 명령을 사용하여 캐시를 삭제하기 위해 수정한 널 빌드팩으로 앱을 푸시합니다. 이 단계를 완료하고 나면 앱의 캐시 디렉토리에 있는 모든 컨텐츠가 삭제됩니다.
  ```
  ibmcloud cf push appname -p app_path -b <modified_null_buildpack>
  ```
 4. 다음 명령을 사용하여 사용할 최신 빌드팩으로 앱을 푸시합니다.
  ```
  ibmcloud cf push appname -p app_path -b <latest_buildpack>
  ```

### 애플리케이션이 계속 다시 시작됨
{: #ts_apprestart}

애플리케이션이 계속해서 충돌하고 다시 시작됩니다.
{: tsSymptoms}

애플리케이션 컨테이너 라이프사이클(예: 버리기 및 시스템 종료)이 애플리케이션 기능에 영향을 줄 수 있습니다.  
{: tsCauses}

애플리케이션 배치 시 오류를 일으키는 애플리케이션 컨테이너 라이프사이클 단계를 식별합니다. [Cloud Foundry 애플리케이션 라이프사이클](https://docs.cloudfoundry.org/devguide/deploy-apps/app-lifecycle.html#evacuation)에 대해 자세히 알아봅니다.
{: tsResolve}

### 인스턴스 세부사항 페이지에서 조치 단추가 사용 안함 상태임(더 이상 사용되지 않음)
  {: #ts_actionsbutton}

  인스턴스 세부사항 페이지의 조치 단추가 사용 안함으로 설정되어 있습니다.
  {: tsSymptoms}

  이 문제점은 다음과 같은 이유로 발생합니다.
  {: tsCauses}

   * 앱이 임베디드 Liberty 빌드팩을 사용하여 배치되지 않았습니다.
   * 앱이 이전 버전의 Liberty 빌드팩을 사용하여 배치되었습니다.

  이전 버전의 Liberty 빌드팩으로 인해 문제점이 발생한 경우 {{site.data.keyword.Bluemix_notm}}에 앱을 다시 배치하십시오. 그렇지 않은 경우 다음과 같은 클라이언트 애플리케이션 로그 파일을 지원 팀에 제공하십시오.
  {: tsResolve}

    * logs/messages.log
    * logs/stdout.log
    * logs/stderr.log


### 추적 또는 덤프 창을 열려면 인증 정보가 필요함(더 이상 사용되지 않음)
  {: #ts_username}

  추적 또는 덤프 창을 열려면 사용자 이름과 비밀번호가 필요합니다.
  {: tsSymptoms}

  이 문제점은 로그인 세션 제한시간 초과로 인해 발생합니다.
  {: tsCauses}

  사용자 이름과 비밀번호를 다시 입력하십시오.
  {: tsResolve}


### 추적 또는 덤프 오퍼레이션이 실행되는 중에 오류가 발생함(더 이상 사용되지 않음)
  {: #ts_target}

  추적 또는 덤프 오퍼레이션이 실행되는 중에 오류 메시지가 표시됩니다. 이 메시지는 앱의 대상 인스턴스가 실행 중 상태가 아님을 나타냅니다.
  {: tsSymptoms}

  ```
  Instance 0: Trace specification is set successfully
  Instance 2: Trace specification is set successfully
  Instance 1: Target instance 1 for app abcd is not in the running state.
  Instance 3: Trace specification is set successfully
  Instance 4: Trace specification is set successfully
  ```

  이 문제점은 다음과 같은 이유로 발생합니다.
  {: tsCauses}

    * 추적 또는 덤프 관리 기능은 실행 중인 앱 인스턴스에만 사용할 수 있습니다. 추적 또는 덤프 오퍼레이션은 중지되거나 시작 중이거나 충돌된 앱 인스턴스에서는 사용할 수 없습니다.
    * 추적 또는 덤프 대화 상자가 열리면 앱 인스턴스의 상태가 변경됩니다.

  창을 닫은 다음 다시 여십시오.
  {: tsResolve}


### 인스턴스에 다른 traceSpecification 구성이 있음(더 이상 사용되지 않음)
  {: #ts_different_config}

  한 앱의 서로 다른 인스턴스에 대해 다른 traceSpecification 구성이 표시될 수 있습니다.
  {: tsSymptoms}

  이 작동은 다음과 같은 이유로 발생합니다.
  {: tsCauses}

    * 이전에 하나 이상의 인스턴스에 대해 구성을 변경했습니다. 한 인스턴스의 traceSpecification 구성을 변경하는 경우 변경사항은 동일한 앱의 다른 인스턴스에는 적용되지 않습니다. 예를 들어 앱에서 log4j를 사용하고 이 앱에 대한 인스턴스가 2개 있습니다. 인스턴스 0의 로그 레벨을 정보에서 디버그로 변경할 수 있지만 인스턴스 1의 로그 레벨은 여전히 정보입니다.

  조치가 필요하지 않습니다. 이는 예상된 동작입니다.
  {: tsResolve}


### 디스크 할당량이 초과됨
  {: #ts_diskquota}

  앱 로그에 디스크 할당량이 초과되었다고 표시될 수 있습니다.

  앱 로그에 `Disk quota exceeded` 오류 메시지가 표시됩니다.
  {: tsSymptoms}

  이 문제점은 다음과 같은 이유 중 하나로 발생합니다.
  {: tsCauses}

    * 덤프 파일은 실행 중인 앱 인스턴스를 사용하여 생성되며 이 파일은 할당된 디스크 할당량까지 사용됩니다. 기본적으로 앱 인스턴스 1개의 디스크 할당량은 1GB입니다. **대시보드 > 애플리케이션 > 앱 런타임**을 클릭하면 디스크 사용량을 확인할 수 있습니다. 다음 예에서는 두 개의 앱 인스턴스에 대한 런타임 정보(디스크 사용량 포함)를 보여 줍니다.
      ```
      인스턴스 상태	CPU	메모리 사용량 디스크 사용량

  	0		실행 중	1.0%	344.8MB/512MB	236.8MB/1GB
  	2		실행 중	2.3%	361.2MB/512MB	235.7MB/1GB
      ```
    * 디스크 할당량은 현재 조직 할당량으로 제한됩니다.

  다음 방법 중 하나를 사용하십시오.
  {: tsResolve}

    * 덤프 파일을 다운로드한 후 삭제하십시오.
    * 배치 Manifest에 다음 항목을 포함시켜 더 큰 디스크 할당량 값으로 앱을 다시 배치하십시오.
      ```
      disk_quota: 2048
      ```

## Liberty for Java
{: #ts_liberty}

### 애플리케이션 연결 수락 실패
{: #health_check_timeout}

Liberty 애플리케이션 시작에 실패하고 "_Failed to start accepting connections_" 오류가 발생합니다. 예를 들면, 로그에 다음과 같은 오류가 표시됩니다.
{: tsSymptoms}

```
   2016-11-14T14:44:58.45+0000 [API/0]      OUT App instance exited with guid 21ac63eb-9595-428a-94c7-b0b02aaf77cc payload: {"cc_partition"=>"default", "droplet"=>"21ac63eb-9595-428a-94c7-b0b02aaf77cc", "version"=>"b2772438-92de-4d47-b680-ea772ac2288a", "instance"=>"f4799c8c89214bbd8067883c3ffe23e0", "index"=>0, "reason"=>"CRASHED", "exit_status"=>255, "exit_description"=>"failed to accept connections within health check timeout", "crash_timestamp"=>1479134698}
   2016-11-14T14:45:07.50+0000 [DEA/4]      ERR Instance (index 0) failed to start accepting connections
```
{: codeblock}

{{site.data.keyword.Bluemix_notm}}에서는 애플리케이션이 정상적으로 시작되었는지 확인하기 위해 애플리케이션에 대한 상태 검사를 수행합니다. 상태 검사에서는 애플리케이션이 애플리케이션에 지정된 포트에서 청취 중인지 여부를 테스트합니다. 이 검사의 기본 제한시간은 60초이며 일부 애플리케이션은 시작하는 데 60초 넘게 걸릴 수 있습니다.  애플리케이션을 시작하는 데 오래 걸리는 이유는 여러 가지가 있습니다. 예를 들면, [New Relic](/docs/runtimes/liberty/monitoring/newRelic.html)과 같은 바인딩 서비스는 시작 시간을 늘립니다. 또한 애플리케이션에서 완료하는 데 시간이 오래 걸리는 초기화 단계를 수행할 수도 있습니다.
{: tsCauses}

먼저 로그를 조사하여 Liberty 애플리케이션 실패를 일으킨 명백한 오류가 있는지 확인하십시오. 명백한 오류를 찾을 수 없는 경우에는 다음을 시도하십시오.
{: tsResolve}

#### 상태 검사 제한시간 늘리기

* `ibmcloud cf push` 명령을 사용하여 애플리케이션을 배치할 경우 `-t` 옵션을 사용해 애플리케이션 시작 제한시간을 길게 지정하십시오. 예를 들어, 다음과 같습니다.

        ```
	ibmcloud cf push myApp -t 180
        ```
	{: codeblock}

* manifest.yml 파일에서 상태 검사 제한시간을 지정할 수도 있습니다. 예를 들어, 다음과 같습니다.

        ---
           ...
           timeout: 180
        {: codeblock}

#### appstate 기능 사용 안함

appstate 기능은 {{site.data.keyword.Bluemix_notm}} 상태 검사 프로세스와 통합되어 Liberty 애플리케이션이 완전히 초기화되어야 HTTP 요청을 수신할 수 있도록 합니다. 애플리케이션이 완전히 초기화되면 appstate 기능이 적용되지 않습니다.  이 기능에는 일부 애플리케이션을 시작하는 데 시간이 오래 걸린다는 부작용이 있습니다. appstate 기능을 사용 안함으로 설정하려면 애플리케이션에서 다음 환경 특성을 설정하고 애플리케이션을 다시 스테이징하십시오.

```
ibmcloud cf set-env myApp JBP_CONFIG_LIBERTY "app_state: false"
```
{: codeblock}

#### 애플리케이션 리팩토링

애플리케이션을 초기화하는 데 시간이 오래 걸리는 경우 애플리케이션을 리팩토링하여 지연 및/또는 비동기 초기화를 수행해야 합니다.

#### "no-route" 옵션을 지정하여 배치

1. "--no-route" 옵션을 사용하여 애플리케이션을 배치하십시오. 그러면 포트 상태 검사를 사용하지 않습니다. 예를 들어, 다음과 같습니다.

        ```
	ibmcloud cf push myApp –no-route
        ```
	{: codeblock}

2. 애플리케이션이 초기화되면 라우트를 애플리케이션에 맵핑하십시오. 예를 들어, 다음과 같습니다.

        ibmcloud cf map-route myApp mybluemix.net
        {: codeblock}

### IBM 게이트웨이에서 SSL 오류 발생
{: #ssl_handshake_failure}


로그에 다음 오류가 표시되고 애플리케이션을 시작할 수 없습니다.
{: tsSymptoms}

```
    2016-11-03T12:32:44.82-0200 [App/0]      ERR java.security.cert.CertPathValidatorException: Certificate chaining error
    2016-11-03T12:32:44.83-0200 [App/0]      ERR [ERROR   ] CWPKI0022E: SSL HANDSHAKE FAILURE:  A signer with SubjectDN CN=*.gateway.prd.na.ca.ibmserviceengage.com, O=International Business Machines Corp., L=Armonk, ST=New York, C=US was sent from the target host.  The signer might need to be added to local trust store /home/vcap/app/wlp/usr/servers/defaultServer/resources/security/key.jks, located in SSL configuration alias defaultSSLConfig.  The extended error message from the SSL handshake exception is: PKIX path building failed: java.security.cert.CertPathBuilderException: PKIXCertPathBuilderImpl could not build a valid CertPath.; internal cause is:
    2016-11-03T12:32:44.83-0200 [App/0]      ERR java.security.cert.CertPathValidatorException: The certificate issued by CN=DigiCert Global Root CA, OU=www.digicert.com, O=DigiCert Inc, C=US is not trusted; internal cause is:
    2016-11-03T12:32:44.83-0200 [App/0]      ERR java.security.cert.CertPathValidatorException: Certificate chaining error
```
{: codeblock}

보안 서비스가 Liberty 애플리케이션에 바인드되었으며 이 Liberty 애플리케이션이 Liberty ssl-1.0 기능을 구성하는 server.xml을 포함하는 패키지된 서버 또는 서버 디렉토리로 배치된 경우 오류가 생성될 수 있습니다. 보안 서비스를 Liberty 애플리케이션에 바인드하면 런타임이 보안 연결을 통해 서비스에 연결합니다. 해당 보안 연결은 기본 SSL 설정을 사용해 설정됩니다. 기본 SSL 설정은 Liberty의 server.xml에서 지정되므로 구성된 신뢰 저장소에서 보안 서비스가 사용하는 인증서를 신뢰하지 않을 수 있습니다.
{: tsCauses}

다음 옵션 중 하나를 지정하여 JVM의 신뢰 저장소를 사용하도록 구성을 수정하십시오.  변경 후 애플리케이션을 다시 스테이징해야 합니다.
{: tsResolve}

#### Liberty의 server.xml 업데이트

JVM의 cacerts 파일을 신뢰 저장소로 사용하도록 server.xml을 업데이트하십시오. 다음을 server.xml에 추가하십시오.

        <ssl id="defaultSSLConfig" trustStoreRef="defaultTrustStore"/>
        <keyStore id="defaultTrustStoretore" location="${java.home}/lib/security/cacerts"/>
        {: codeblock}

#### 구성된 신뢰 저장소 업데이트

DigitCert ROOT CA를 신뢰하도록 구성된 신뢰 저장소를 수정하십시오.
  1. https://www.digicert.com/CACerts/DigiCertGlobalRootCA.crt 사이트에서 DigiCert Root CA를 다운로드하십시오.
  2. resources/security/key.jks를 신뢰 저장소로 사용할 경우 Java의 keytool 유틸리티를 사용하여 CA를 키로 가져오십시오.

            keytool -importcert --storepass <keyStorePassword> -keystore &lt;path&gt;/resources/security/key.jks -file DigiCertGlobalRootCA.crt
            {: codeblock}


## SDK for Node.js
{: #ts_nodejs}

### 애플리케이션 시작 실패: 일반적인 문제점 해결

{{site.data.keyword.runtime_nodejs_notm}} 빌드팩 V3.23 이상의 경우 첫 번째 단계로 종속성 벤더링을 시도하십시오. 종속성 벤더링은 애플리케이션과 동일한 소스 파일에서 종속성을 패키지하는 것을 의미합니다. 이를 통해 종속성이 애플리케이션과 동일한 기본 디렉토리에 있다고 가정할 때 발생할 수 있는 다양한 오류를 해결할 수 있습니다.

1. 애플리케이션 루트 디렉토리에서 다음 명령을 실행하여 종속성을 설치하십시오.

   ```bash
npm install
   ```
   {: codeblock}
1. `.cfignore` 파일에 다음 행이 포함되어 있지 않은지 확인하십시오.

   ```
   node_modules/
   ```

이제 `ibmcloud cf push` 명령을 사용하여 애플리케이션을 배치하는 경우 종속성을 별도의 위치에 다운로드하는 대신 애플리케이션의 나머지 항목과 동일한 디렉토리에 복사됩니다.

### 애플리케이션 시작이 실패하며 "No space left on device" 오류 발생
{: #no_space_left_on_device}


Node.js 애플리케이션이 시작에 실패하며 "No space left on device" 오류가 나타납니다. 예를 들면, 로그에 다음과 같은 오류가 표시됩니다.
{: tsSymptoms}

```
   2017-01-16T14:25:14.61-0500 [CELL/0]     ERR tar: ./app/node_modules/pm2/node_modules/cron/node_modules/moment-timezone/LICENSE: Cannot write: No space left on device

```
{: codeblock}

버전 3 이전의 NPM 버전을 사용하는 Node.js 애플리케이션은 종속 항목 다운로드에 추가 영역을 사용합니다.
{: tsCauses}

애플리케이션의 package.json 파일을 수정하여 NPM 버전 3 이상을 사용하십시오.
{: tsResolve}

```
{
  "name": "myapp",
  "description": "this is my app",
  "version": "0.1",
  "engines": {
     "node": "4.2.4",
     "npm": "3.10.10"
  }
}
```
{: codeblock}

### 메모리 제한조건으로 인해 애플리케이션 다시 시작
{: #oom}

Node.js에서는 애플리케이션에 사용 가능한 메모리의 양을 알 수 없기 때문에 메모리가 모두 사용되기 전에 가비지 콜렉터가 실행되지 않을 수 있습니다.

```
2017-09-01T11:00:42.19-0400 [APP/PROC/WEB/0]OUT Exit status 137
2017-09-01T11:00:42.23-0400 [CELL/0]     OUT Exit status 0
2017-09-01T11:00:42.27-0400 [CELL/0]     OUT Destroying container
2017-09-01T11:00:42.34-0400 [API/0]      OUT Process has crashed with type: "web"
2017-09-01T11:00:42.36-0400 [API/0]      OUT App instance exited with guid eecfba3b-430c-4a6b-b71f-ac72816fe152 payload: {"instance"=>"77dbb981-16d0-3a05-3235-9a4b", "index"=>0, "reason"=>"CRASHED", "exit_description"=>"2 error(s) occurred:\n\n* 2 error(s) occurred:\n\n* Exited with status 137 (out of memory)\n* cancelled\n* cancelled", "crash_count"=>1, "crash_timestamp"=>1504278042244633291, "version"=>"6497b5b5-67d4-4c5a-b1af-362e522a029d"}
2017-09-01T11:00:43.35-0400 [CELL/0]     OUT Successfully destroyed container
```
{: codeblock}

가능한 솔루션은 package.json 파일의 애플리케이션의 시작 솔루션에 `--max_old_space_size` 옵션을 설정하는 것입니다. 이 옵션은 애플리케이션 메모리 공간의 부분을 표시하고 애플리케이션에 사용 가능한 전체 메모리 미만으로 값을 설정해야 합니다. 이 주제에 대한 심도 깊은 토론을 보려면 [Large memory spikes and Heroku](https://github.com/nodejs/node/issues/3370)를 읽어 보십시오.
```
  "scripts": {
    "start": "node --max_old_space_size=800 server.js"
  }
```
{: codeblock}

## ASP.NET Core
{: #ts_dotnet}

애플리케이션 배치에 실패하고 다음 메시지가 표시됩니다. `API/0App instance exited ... payload: {... "reason"=>"CRASHED", "exit_status"=>-1, ...}`.
{: tsSymptoms}

ASP.net 애플리케이션을 푸시할 때 이와 비슷한 메시지를 받는 경우 이는 애플리케이션이 메모리나 디스크 할당량 한계를 초과했기 때문일 가능성이 매우 높습니다.  애플리케이의 메모리 또는 디스크 공간 할당량을 늘리십시오.
{: tsCauses}

내 애플리케이션이 배치에 실패하며 다음 메시지가 표시됩니다. `Failed to compress droplet: signal: broken pipe` 또는 `No space left on device`.  이 문제의 해결 방법은 무엇입니까?
{: tsSymptoms}

많은 수의 NuGet 패키지 종속 항목이 포함된 소스 코드에서 푸시된 프로젝트로 인해 NuGet 패키지 캐시를 사용할 때 종종 이 오류가 발생합니다.  캐시를 사용 안함으로 설정하려면 `CACHE_NUGET_PACKAGES` 환경 변수를 `false`로 설정하십시오. 자세한 정보는 [NuGet 패키지를 사용 안함](/docs/runtimes/dotnet/disablingNuGet.md)으로 설정하는 방법에 대한 지시사항을 참조하십시오.
{: tsCauses}

### 유용한 링크
* [NuGet](https://docs.nuget.org/Consume/Overview){: new_window}
* [ASP.NET Core 개요](http://docs.asp.net/en/latest/conceptual-overview/aspnet.html){: new_window}

## PHP
{: #ts_PHP}

### PHP 빌드팩의 NOTICE 메시지
{: #ts_phplog}

로그에 NOTICE가 포함된 메시지가 표시될 수 있습니다. 로깅 레벨을 변경하여 이러한 메시지의 로깅을 중지할 수 있습니다.

PHP 빌드팩을 사용하여 앱을 {{site.data.keyword.Bluemix_notm}}에 푸시하는 경우 다음과 같이 `NOTICE`가 포함된 메시지가 표시될 수 있습니다.
{: tsSymptoms}

```
• 2015-01-26T15:00:59.70+0100 [App/0] ERR [26-Jan-2015 14:00:59] NOTICE: [pool www] 'user' directive is ignored when FPM is not running as root
• 2015-01-26T15:01:00.63+0100 [App/0] ERR [26-Jan-2015 14:00:59] NOTICE: [pool www] 'user' directive is ignored when FPM is not running as root
• 2015-01-26T15:01:00.63+0100 [App/0] ERR [26-Jan-2015 14:00:59] NOTICE: fpm is running, pid 93
• 2015-01-26T15:01:00.63+0100 [App/0] ERR [26-Jan-2015 14:00:59] NOTICE: ready to handle connections
```
PHP 빌드팩에서 error_log 매개변수는 로깅 레벨을 정의합니다. 기본적으로 `error_log` 매개변수의 값은 **stderr notice**입니다. 다음 예는 Cloud Foundry에서 제공하는 PHP 빌드팩의 `nginx-defaults.conf` 파일에 있는 기본 로깅 레벨 구성을 보여 줍니다. 자세한 정보는 [cloudfoundry/php-buildpack ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/cloudfoundry/php-buildpack/blob/ff71ea41d00c1226d339e83cf2c7d6dda6c590ef/defaults/config/nginx/1.5.x/nginx-defaults.conf){: new_window}을 참조하십시오.
{: tsCauses}

```
daemon off;
error_log stderr notice;
pid @{HOME}/nginx/logs/nginx.pid;
```

`NOTICE` 메시지는 정보용이며 문제점을 표시하지 않을 수 있습니다. 빌드팩의 nginx-defaults.conf 파일에서 로깅 레벨을 `stderr notice`에서 `stderr error`로 변경하여 이러한 메시지 로깅을 중지할 수 있습니다. 예를 들어, 다음과 같습니다. 	
{: tsResolve}

```
daemon off;
error_log stderr error;
pid @{HOME}/nginx/logs/nginx.pid;
```
기본 로깅 구성을 변경하는 방법에 대한 자세한 정보는 [error_log ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://nginx.org/en/docs/ngx_core_module.html#error_log){: new_window}을 참조하십시오.


## Python
{: #ts_python}

### 서드파티 Python 라이브러리를 {{site.data.keyword.Bluemix_notm}}로 가져올 수 없음
{: #ts_importpylib}

서드파티 Python 라이브러리를 {{site.data.keyword.Bluemix_notm}}로 가져오지 못할 수 있습니다. 이 문제점을 해결하려면 Python 앱의 루트 디렉토리에 구성 파일을 추가하십시오.

`web.py` 라이브러리와 같은 서드파티 Python 라이브러리를 가져올 때 `ibmcloud cf push` 명령이 실패합니다.
{: tsSymptoms}

Python 앱의 구성 정보가 누락되었습니다.
{: tsCauses}

`requirements.txt` 파일 및 `Procfile` 파일을 Python 앱의 루트 디렉토리에 추가하십시오. 다음 정보는 `web.py` 라이브러리를 가져온다고 가정합니다.
{: tsResolve}

 1. `requirements.txt` 파일을 Python 앱의 루트 디렉토리에 추가하십시오.

 `requirements.txt` 파일은 Python 앱에 필요한 라이브러리 패키지와 패키지 버전을 지정합니다. 다음 예에서는 `requirements.txt` 파일의 컨텐츠를 보여줍니다. 여기서 `web.py==0.37`은 다운로드할 `web.py` 라이브러리의 버전이 0.37임을 나타내고, `wsgiref==0.1.2`는 web.py 라이브러리에 필요한 웹 서버 게이트웨이 인터페이스가 0.1.2임을 나타냅니다.
	 ```
	 web.py==0.37
     wsgiref==0.1.2
	 ```
	 `requirements.txt` 파일을 구성하는 방법에 대한 자세한 정보는 [Requirements files](https://pip.readthedocs.org/en/1.1/requirements.html)를 참조하십시오.

 2. `Procfile` 파일을 Python 앱의 루트 디렉토리에 추가하십시오.
 `Procfile` 파일에는 Python 앱에 대한 시작 명령이 포함되어야 합니다. 다음 명령에서 *yourappname*은 Python 앱의 이름이고 *PORT*는 앱 사용자로부터 요청을 받기 위해 Python 앱이 사용해야 하는 포트 번호입니다. *$PORT* 는 선택사항입니다. 시작 명령에서 PORT를 지정하지 않으면 앱에 있는 `VCAP_APP_PORT` 환경 변수의 포트 번호가 사용됩니다.
	```
	web: python <yourappname>.py $PORT
	```

이제 서드파티 Python 라이브러리를 {{site.data.keyword.Bluemix_notm}}로 가져올 수 있습니다.
