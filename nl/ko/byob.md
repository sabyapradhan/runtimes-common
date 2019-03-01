---

copyright:
  years: 2015, 2018
lastupdated: "2018-06-27"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# 커뮤니티 빌드팩 사용
{: #using_buildpacks}

원하는 런타임을 제공하는 스타터가 {{site.data.keyword.Bluemix}} 카탈로그에 없는 경우, 외부 빌드팩을 {{site.data.keyword.Bluemix_notm}}로 가져올 수 있습니다. `ibmcloud cf push` 명령을 사용하여 애플리케이션을 배치할 때 사용자 정의 Cloud Foundry 호환 빌드팩을 지정할 수 있습니다.
{:shortdesc}

사용자 자신의 빌드팩으로 사용할 수 있도록 Cloud Foundry 커뮤니티에서 외부 빌드팩을 제공합니다. 애플리케이션을 {{site.data.keyword.Bluemix_notm}}에 배치하기 전에 {{site.data.keyword.Bluemix_notm}} 명령행 인터페이스를 설치해야 합니다.

**참고:** IBM에서는 외부 빌드팩을 제공하지 않습니다. 지원을 받으려면 Cloud Foundry 커뮤니티에 문의하십시오.

## 기본 제공 커뮤니티 빌드팩

{{site.data.keyword.Bluemix_notm}}에서는 Cloud Foundry 커뮤니티에서 제공하는 기본 제공 빌드팩을 사용할 수 있습니다. 기본 제공 커뮤니티 빌드팩을 보려면 다음과 같이 `ibmcloud cf buildpacks` 명령을 실행하십시오.

```
ibmcloud cf buildpacks
Getting buildpacks...

buildpack      position   enabled   locked   filename
...
java_buildpack     7      true      false    buildpack_java_v2.0.2.zip
ruby_buildpack     8      true      false    buildpack_ruby_v46-245-g2fc4ad8.zip
nodejs_buildpack   9      true      false    buildpack_nodejs_v8-177-g2b0a5cf.zip
```
{:screen}


동일한 런타임 또는 프레임워크의 경우, IBM에서 작성된 빌드팩이 커뮤니티 빌드팩에 우선합니다. 커뮤니티 빌드팩을 사용하여 IBM에서 작성된 빌드팩을 겹쳐쓰려면 `ibmcloud cf push` 명령에서 `-b` 옵션을 사용하여 빌드팩을 지정하십시오.

예를 들어 다음 명령을 사용하여 Java™ 웹 애플리케이션용 커뮤니티 빌드팩을 사용할 수 있습니다.

```
ibmcloud cf push app_name -b java_buildpack -p app_path
```
{:pre}

다음 명령을 사용하여 Node.js 애플리케이션용 커뮤니티 빌드팩을 사용할 수도 있습니다.

```
ibmcloud cf push app_name -b nodejs_buildpack -p app_path
```
{: codeblock}

IBM에서 작성된 빌드팩에서는 지원되지 않지만 기본 제공 커뮤니티 빌드팩에서는 지원되는 런타임 또는 프레임워크의 경우 `ibmcloud cf push` 명령에 `-b` 옵션을 사용할 필요가 없습니다. 예를 들어 Ruby 애플리케이션의 경우 IBM에서 작성된 빌드팩이 없습니다. 다음 명령을 입력하여 기본 제공 커뮤니티 빌드팩을 사용할 수 있습니다.

```
ibmcloud cf push app_name -p app_path
```
{: codeblock}

## 외부 빌드팩

{{site.data.keyword.Bluemix_notm}}에서는 외부 빌드팩 또는 사용자 정의 빌드팩을 사용할 수 있습니다. `ibmcloud cf push` 명령에서 `-b` 옵션을 사용하여 빌드팩 URL을 지정해야 하고 `-s` 옵션을 사용하여 스택을 지정해야 합니다. 예를 들어 정적 파일에 대한 외부 커뮤니티 빌드팩을 사용하려면 다음 명령을 실행하십시오.

```
ibmcloud cf push app_name -p app_path -b https://github.com/cloudfoundry/staticfile-buildpack.git
```
{: codeblock}

Ruby 애플리케이션에 기본 제공 빌드팩을 사용하지 않을 경우 다음 명령을 입력하여 외부 빌드팩을 사용할 수 있습니다.

```
ibmcloud cf push app_name -p app_path -b https://github.com/cloudfoundry/ruby-buildpack.git
```
{: codeblock}

애플리케이션에 사용자 정의 빌드팩을 사용할 수도 있습니다. 예를 들어 Cloud Foundry 커뮤니티에서 제공하는 오픈 소스 PHP 빌드팩을 사용할 수 있습니다. PHP 애플리케이션을 {{site.data.keyword.Bluemix_notm}}에 배치하는 경우 다음 명령을 입력하여 빌드팩의 Git 저장소 URL을 지정하십시오.

```
ibmcloud cf push app_name -p app_path -b https://github.com/cloudfoundry/php-buildpack.git
```
{:pre}

프로젝트의 `manifest.yml` 파일을 편집하여 `buildpack` 행을 추가할 수도 있습니다.

```
buildpack: https://github.com/cloudfoundry/python-buildpack.git
```
{: codeblock}


## Java 빌드팩 버전 지정

Java 빌드팩 버전을 지정하려면 `ibmcloud cf set-env` 명령을 사용하십시오. 예를 들어 다음 명령을 입력하여 Java 버전을 1.7.0으로 설정하십시오.

```
ibmcloud cf set-env app_name JBP_CONFIG_OPEN_JDK_JRE &apos;{jre: { version: 1.7.0_+ }}&apos;
```
{: codeblock}

그런 다음 애플리케이션을 다시 스테이징하면 변경사항이 적용됩니다.

```
ibmcloud cf restage app_name
```
{: codeblock}

## `manifest.yml` 파일을 사용하십시오.

지정하려는 환경 변수와 값을 `manifest.yml` 파일에 직접 추가할 수 있습니다. [환경 변수](https://docs.cloudfoundry.org/devguide/deploy-apps/manifest.html#env-block)를 참조하십시오.
