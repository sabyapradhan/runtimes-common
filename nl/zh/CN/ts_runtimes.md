---

copyright:
  years: 2015, 2018
lastupdated: "2018-11-09"

---

{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:new_window: target="_blank"}


# 运行时的故障诊断
{: #runtimes}

使用 [{{site.data.keyword.Bluemix}} 运行时](index.html)的时候，可能会遇到问题。在许多情况下，只需执行几个简单的步骤即可解决这些问题。
{:shortdesc}

* [一般故障诊断](#ts_all)
* [Liberty for Java](#ts_liberty)
* [SDK for Node.js](#ts_nodejs)
* [ASP.NET 核心](#ts_dotnet)
* [PHP](#ts_php)
* [Python](#ts_python)

## 一般故障诊断
{: #ts_all}

### 推送应用程序时使用过时的 buildpack
{: #ts_loading_bp}

您可能在推送应用程序时无法使用最新的 buildpack 组件。可以使用具有内置机制的 buildpack 来阻止装入过时的组件，或者可以在推送或重新编译打包应用程序之前，删除应用程序高速缓存目录中的内容。

在更新 buildpack 后推送或重新编译打包应用程序时，不会自动装入最新的 buildpack 组件。结果应用程序就从高速缓存使用过时的 buildpack 组件。自上次推送应用程序以来已应用到 buildpack 的更新未实施。
{: tsSymptoms}

某些 buildpack 未配置为从因特网自动下载所有更新的组件以确保您始终使用最新的版本。
{: tsCauses}

您可以使用具有内置机制来避免装入过时组件的 buildpack，例如以下 buildpack：
{: tsResolve}

  * [Cloud Foundry Java buildpack ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/cloudfoundry/java-buildpack){: new_window}. 此 buildpack 具有内置机制来确保使用 buildpack 的最新版本。有关此机制如何工作的更多信息，请参阅 [extending-caches.md ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/cloudfoundry/java-buildpack/blob/master/docs/extending-caches.md){: new_window}。
  * [Cloud Foundry Node.js buildpack ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/cloudfoundry/nodejs-buildpack){: new_window}。此 buildpack 通过使用环境变量来提供类似功能。要使 Node.js buildpack 每次都能从因特网下载节点模块，请在 {{site.data.keyword.Bluemix_notm}} 命令行界面中键入以下命令： 	

  ```
  set NODE_MODULES_CACHE=false
  ```

如果您使用的 buildpack 不提供自动装入最新组件的机制，那么您可以手动删除高速缓存目录中的内容，并重新推送应用程序。请使用以下步骤：

 1. 检出空 buildpack 的分支，例如 https://github.com/ryandotsmith/null-buildpack。有关如何检出分支的信息，请参阅 [Git Basics - Getting a Git Repository ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](http://www.git-scm.com/book/en/v2/Git-Basics-Getting-a-Git-Repository){: new_window}。  
 2. 将以下行添加到 `null-buildpack/bin/compile` 文件，并落实更改。有关如何落实更改的信息，请参阅 [Git Basics - Recording Changes to the Repository ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](http://www.git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository){: new_window}。
  ```
  rm -rfv $2/*
  ```
 3. 通过使用以下命令，使用修改用于删除高速缓存的空 buildpack 推送应用程序。完成此步骤后，会删除应用程序高速缓存目录中的所有内容。
  ```
  ibmcloud cf push appname -p app_path -b <modified_null_buildpack>
  ```
 4. 通过使用以下命令，使用想要使用的最新 buildpack 推送应用程序：
  ```
  ibmcloud cf push appname -p app_path -b <latest_buildpack>
  ```

### 应用程序继续重新启动
{: #ts_apprestart}

应用程序继续崩溃并重新启动。
{: tsSymptoms}

应用程序容器生命周期（例如，重新部署和关闭）可能会影响应用程序功能。  
{: tsCauses}

确定是应用程序容器生命周期中的哪个步骤导致应用程序部署错误。了解有关 [Cloud Foundry 应用程序生命周期](https://docs.cloudfoundry.org/devguide/deploy-apps/app-lifecycle.html#evacuation)的更多信息。
{: tsResolve}

### “实例详细信息”页面上的“操作”按钮已禁用（不推荐）
  {: #ts_actionsbutton}

  “实例详细信息”页面上的“操作”按钮已禁用。
  {: tsSymptoms}

  发生此问题的原因如下：
  {: tsCauses}

   * 应用程序不是使用嵌入式 Liberty buildpack 部署的。
   * 应用程序是使用 Liberty buildpack 早期版本部署的。

  如果该问题是由 Liberty buildpack 早期版本导致的，请在 {{site.data.keyword.Bluemix_notm}} 中重新部署应用程序。否则，您可以向支持团队提供以下客户机应用程序日志文件：
  {: tsResolve}

    * logs/messages.log
    * logs/stdout.log
    * logs/stderr.log


### 打开跟踪或转储窗口时需要凭证（不推荐）
  {: #ts_username}

  打开跟踪或转储窗口时需要用户名和密码。
  {: tsSymptoms}

  由于登录会话超时，发生此问题。
  {: tsCauses}

  重新输入用户名和密码。
  {: tsResolve}


### 正在执行跟踪或转储操作时发生错误（不推荐）
  {: #ts_target}

  正在执行跟踪或转储操作时显示错误消息。该消息指示应用程序的目标实例未处于运行状态：
  {: tsSymptoms}

  ```
  实例 0：已成功设置跟踪规范
  实例 2：已成功设置跟踪规范
  实例 1：应用程序 abcd 的目标实例 1 未处于运行中状态。
  实例 3：已成功设置跟踪规范
  实例 4：已成功设置跟踪规范
  ```

  发生此问题的原因如下：
  {: tsCauses}

    * 跟踪或转储管理功能仅用于正在运行的应用程序实例。无法对已停止、正在启动或已崩溃的应用程序实例使用跟踪或转储操作。
    * 打开跟踪或转储对话框时，应用程序实例的状态正在更改。

  关闭窗口，然后重新打开该窗口。
  {: tsResolve}


### 实例具有不同的 traceSpecification 配置（不推荐）
  {: #ts_different_config}

  对于一个应用程序的不同实例，您可能会看到不同的 traceSpecification 配置。
  {: tsSymptoms}

  发生此行为的原因如下：
  {: tsCauses}

    * 您先前更改了一个或多个实例的配置。如果更改一个实例的 traceSpecification 配置，该更改并不会应用到同一应用程序的其他实例。例如，您的应用程序使用 log4j，并且此应用程序有 2 个实例。您可以将实例 0 的日志级别从参考更改为调试，但实例 1 的日志级别仍为参考。

  无需执行任何操作。这是正常的情况。
  {: tsResolve}


### 超出磁盘配额
  {: #ts_diskquota}

  您可能会在应用程序日志中看到超过磁盘配额的消息。

  您在应用程序日志中看到`超过磁盘配额`错误消息。
  {: tsSymptoms}

  此问题由以下其中一个原因导致：
  {: tsCauses}

    * 转储文件是使用运行中的应用程序实例生成的，并且这些文件用完了分配的磁盘配额。缺省情况下，一个应用程序实例的磁盘配额是 1 GB。您可以通过单击**仪表板>应用程序>应用程序运行时**来检查磁盘使用情况。以下示例显示某个应用程序的两个实例的运行时信息，包括磁盘使用情况：
      ```
      实例  状态	  CPU	 内存使用情况  磁盘使用情况

  	0		运行中	1.0%	344.8MB/512MB	236.8MB/1GB
  	2		运行中	2.3%	361.2MB/512MB	235.7MB/1GB
      ```
    * 磁盘配额受当前组织配额限制。

  使用以下其中一种方法：
  {: tsResolve}

    * 下载转储文件后将其删除。
    * 通过将以下条目包含在部署清单中，使用更大的磁盘配额重新部署应用程序：
    ```
      disk_quota: 2048
      ```

## Liberty for Java
{: #ts_liberty}

### 应用程序无法开始接受连接
{: #health_check_timeout}

Liberty 应用程序启动失败，错误为“_无法开始接受连接_”。例如，日志中的错误可能看起来如下所示：
{: tsSymptoms}

```
   2016-11-14T14:44:58.45+0000 [API/0]      OUT App instance exited with guid 21ac63eb-9595-428a-94c7-b0b02aaf77cc payload: {"cc_partition"=>"default", "droplet"=>"21ac63eb-9595-428a-94c7-b0b02aaf77cc", "version"=>"b2772438-92de-4d47-b680-ea772ac2288a", "instance"=>"f4799c8c89214bbd8067883c3ffe23e0", "index"=>0, "reason"=>"CRASHED", "exit_status"=>255, "exit_description"=>"failed to accept connections within health check timeout", "crash_timestamp"=>1479134698}
   2016-11-14T14:45:07.50+0000 [DEA/4]      ERR Instance (index 0) failed to start accepting connections
```
{: #codeblock}

{{site.data.keyword.Bluemix_notm}} 会对应用程序执行运行状况检查，以查看其是否已成功启动。运行状况检查会测试应用程序是否在分配给应用程序的端口上进行侦听。此检查的缺省超时为 60 秒，一些应用程序可能会需要超过 60 秒的时间才能启动。应用程序启动时间可能较长的原因有很多种。例如，绑定服务（如 [Monitoring and Analytics](/docs/services/monana/index.html#gettingstartedtemplate) 或 [New Relic](/docs/runtimes/liberty/monitoring/newRelic.html)）或者[启用调试器](/docs/runtimes/common/app_mng.html#debug)都会增加启动时间。应用程序还可能会执行初始化步骤，这些步骤可能需要较长时间才能完成。
{: tsCauses}

首先，检查日志以查看是否有任何明显的错误可能导致 Liberty 应用程序失败。如果找不到明显的错误，请尝试以下操作：
{: tsResolve}

#### 增加运行状况检查超时

* 使用 `ibmcloud cf push` 命令部署应用程序时，可以使用 `-t` 选项来指定较长的应用程序启动超时。例如：

        ```
	ibmcloud cf push myApp -t 180
        ```
	{: #codeblock}

* 还可以在 manifest.yml 文件中指定运行状况检查超时。例如：

        ---
           ...
           timeout: 180
        {: #codeblock}

#### 禁用 appstate 功能

appstate 功能与 {{site.data.keyword.Bluemix_notm}} 运行状况检查进程集成在一起，可确保在 Liberty 应用程序开始接收 HTTP 请求之前先将其完全初始化。应用程序完全初始化后，appstate 功能就不再起作用了。此功能的副作用是某些应用程序可能需要较长时间才能启动。要禁用 appstate 功能，可以在应用程序上设置以下环境属性，然后重新编译打包应用程序：

```
ibmcloud cf set-env myApp JBP_CONFIG_LIBERTY "app_state: false"
```
{: #codeblock}

#### 考虑重构应用程序

如果应用程序初始化需要较长时间，可能需要重构应用程序来进行延迟和/或异步初始化。

#### 使用“no-route”选项进行部署

1. 使用“--no-route”选项来部署应用程序。这会禁用端口运行状况检查。例如：

        ```
	ibmcloud cf push myApp –no-route
        ```
	{: #codeblock}

2. 应用程序初始化后，将路径映射到应用程序。例如：

        ibmcloud cf map-route myApp mybluemix.net
        {: #codeblock}

### IBM 网关发生 SSL 错误
{: #ssl_handshake_failure}


在日志中可以看到以下错误，应用程序可能无法启动：
{: tsSymptoms}

```
    2016-11-03T12:32:44.82-0200 [App/0]      ERR java.security.cert.CertPathValidatorException: Certificate chaining error
    2016-11-03T12:32:44.83-0200 [App/0]      ERR [ERROR   ] CWPKI0022E: SSL HANDSHAKE FAILURE:  A signer with SubjectDN CN=*.gateway.prd.na.ca.ibmserviceengage.com, O=International Business Machines Corp., L=Armonk, ST=New York, C=US was sent from the target host.  The signer might need to be added to local trust store /home/vcap/app/wlp/usr/servers/defaultServer/resources/security/key.jks, located in SSL configuration alias defaultSSLConfig.  The extended error message from the SSL handshake exception is: PKIX path building failed: java.security.cert.CertPathBuilderException: PKIXCertPathBuilderImpl could not build a valid CertPath.; internal cause is:
    2016-11-03T12:32:44.83-0200 [App/0]      ERR java.security.cert.CertPathValidatorException: The certificate issued by CN=DigiCert Global Root CA, OU=www.digicert.com, O=DigiCert Inc, C=US is not trusted; internal cause is:
    2016-11-03T12:32:44.83-0200 [App/0]      ERR java.security.cert.CertPathValidatorException: Certificate chaining error
```
{: codeblock}

将 Monitoring & Analytics 服务绑定到 Liberty 应用程序后，如果将 Liberty 应用程序部署为服务器目录或打包服务器，而该目录或服务器中包含用于配置 Liberty ssl-1.0 功能的 server.xml，那么可能会产生这些错误。将 Monitoring & Analytics 服务绑定到 Liberty 应用程序会导致运行时通过安全连接来连接到服务。该安全连接是使用缺省 SSL 设置建立的。由于在 Liberty 的 server.xml 中已指定缺省 SSL 设置，因此所配置的信任库可能不会信任 Monitoring & Analytics 服务所使用的证书。
{: tsCauses}

使用以下某个选项来修改配置，以使用 JVM 的信任库。在进行更改之后，一定要重新编译打包应用程序。
{: tsResolve}

#### 更新 Liberty 的 server.xml

更新 server.xml 以使用 JVM 的 cacerts 文件作为信任库。将以下内容添加到 server.xml 中：

        <ssl id="defaultSSLConfig" trustStoreRef="defaultTrustStore"/>
        <keyStore id="defaultTrustStoretore" location="${java.home}/lib/security/cacerts"/>
        {: codeblock}

#### 更新配置的信任库

修改所配置的信任库，以信任 DigitCert 根 CA。
  1. 从 https://www.digicert.com/CACerts/DigiCertGlobalRootCA.crt 下载 DigiCert 根 CA。
  2. 假设 resources/security/key.jks 用作信任库，请使用 Java 的 keytool 实用程序将 CA 导入到密钥中：

            keytool -importcert --storepass <keyStorePassword> -keystore &lt;path&gt;/resources/security/key.jks -file DigiCertGlobalRootCA.crt
            {: codeblock}


## SDK for Node.js
{: #ts_nodejs}

### 应用程序无法启动：一般故障诊断

对于 {{site.data.keyword.runtime_nodejs_notm}} buildpack V3.23 或更高版本，请尝试将供应依赖关系作为故障诊断的第一步。供应依赖关系表示将依赖关系与应用程序一起打包在相同的源文件中。这可以解决假定依赖关系与应用程序位于相同基本目录时发生的各种错误。

1. 从应用程序根目录，通过运行以下命令安装依赖关系。

   ```bash
npm install
```
   {: codeblock}
1. 确保 `.cfignore` 文件不包含以下行：

   ```
   node_modules/
   ```

现在，使用 `ibmcloud cf push` 命令部署应用程序时，不是将依赖关系下载到单独的位置，而是将依赖关系复制到与其余应用程序相同的目录中。

### 应用程序启动失败，错误为“设备上没有剩余空间”
{: #no_space_left_on_device}


Node.js 应用程序启动失败，错误为“设备上没有剩余空间”。例如，日志中的错误可能看起来如下所示：
{: tsSymptoms}

```
   2017-01-16T14:25:14.61-0500 [CELL/0]     ERR tar: ./app/node_modules/pm2/node_modules/cron/node_modules/moment-timezone/LICENSE: Cannot write: No space left on device

```
{: codeblock}

Node.js 应用程序使用的 NPM 版本如果早于 V3，下载依赖项所消耗的空间就会更多。
{: tsCauses}

修改应用程序的 package.json 文件，以使用 NPM V3 或更高版本。
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

### 应用程序因内存限制而重新启动
{: #oom}

Node.js 不知道有多少内存可用于应用程序，因此在内存耗尽前垃圾回收器可能不会运行。

```
2017-09-01T11:00:42.19-0400 [APP/PROC/WEB/0]OUT Exit status 137
2017-09-01T11:00:42.23-0400 [CELL/0]     OUT Exit status 0
2017-09-01T11:00:42.27-0400 [CELL/0]     OUT Destroying container
2017-09-01T11:00:42.34-0400 [API/0]      OUT Process has crashed with type: "web"
2017-09-01T11:00:42.36-0400 [API/0]      OUT App instance exited with guid eecfba3b-430c-4a6b-b71f-ac72816fe152 payload: {"instance"=>"77dbb981-16d0-3a05-3235-9a4b", "index"=>0, "reason"=>"CRASHED", "exit_description"=>"2 error(s) occurred:\n\n* 2 error(s) occurred:\n\n* Exited with status 137 (out of memory)\n* cancelled\n* cancelled", "crash_count"=>1, "crash_timestamp"=>1504278042244633291, "version"=>"6497b5b5-67d4-4c5a-b1af-362e522a029d"}
2017-09-01T11:00:43.35-0400 [CELL/0]     OUT Successfully destroyed container
```
{: codeblock}

可能的解决方案是在 package.json 文件中应用程序的 start 命令上设置 `--max_old_space_size` 选项。此选项代表应用程序内存占用量的一部分，因此应设置为小于应用程序可用内存总量的值。有关此主题的更深入讨论，请阅读[大内存峰值和 Heroku](https://github.com/nodejs/node/issues/3370)。
```
"scripts": {
	 		 "start": "node --max_old_space_size=800 server.js"
  }
```
{: codeblock}

## ASP.NET 核心
{: #ts_dotnet}

应用程序部署失败，消息为：`API/0App 实例已退出 ... 有效内容：{... "reason"=>"CRASHED", "exit_status"=>-1, ...}`.
{: tsSymptoms}

如果在推送 ASP.net 应用程序时收到类似消息，那么很可能是因为应用程序超过内存或磁盘限额限制。可以增加应用程序的内存或磁盘空间限额。
{: tsCauses}

应用程序部署失败，消息为：`未能压缩 Droplet：信号：管道中断`或`设备上没有剩余空间`。我可以如何修复此问题？
{: tsSymptoms}

从包含大量 NuGet 数据包依赖项的源代码推送的项目，有时可能会在启用 NuGet 数据包高速缓存时导致此错误。可以将 `CACHE_NUGET_PACKAGES` 环境变量设置为 `false` 以禁用高速缓存。有关更多信息，请参阅有关如何[禁用 NuGet 数据包](/docs/runtimes/dotnet/disablingNuGet.md)的指示信息。
{: tsCauses}

### 有用的链接
* [NuGet](https://docs.nuget.org/Consume/Overview){: new_window}
* [ASP.NET 核心概述](http://docs.asp.net/en/latest/conceptual-overview/aspnet.html){: new_window}

## PHP
{: #ts_PHP}

### 来自 PHP buildpack 的注意消息
{: #ts_phplog}

您可能会在日志中看到包含“注意”的消息。通过更改日志记录级别，可以停止记录这些消息。

使用 PHP buildpack 将应用程序推送到 {{site.data.keyword.Bluemix_notm}} 时，可能会看到包含`注意`的消息：
{: tsSymptoms}

```
• 2015-01-26T15:00:59.70+0100 [App/0] 错误 [26-Jan-2015 14:00:59] 注意：[pool www] 当 FPM 未以 root 用户身份运行时，会忽略“user”伪指令
• 2015-01-26T15:01:00.63+0100 [App/0] 错误 [26-Jan-2015 14:00:59] 注意：[pool www] 当 FPM 未以 root 用户身份运行时，会忽略“user”伪指令
• 2015-01-26T15:01:00.63+0100 [App/0] 错误 [26-Jan-2015 14:00:59] 注意：fpm 正在运行，pid 93
• 2015-01-26T15:01:00.63+0100 [App/0] 错误 [26-Jan-2015 14:00:59] 注意：准备就绪，可处理连接
```
在 PHP buildpack 中，error_log 参数用于定义日志记录级别。缺省情况下，`error_log` 参数的值为 **stderr notice**。以下示例显示 Cloud Foundry 所提供 PHP buildpack 的 `nginx-defaults.conf` 文件中的缺省日志记录级别配置。有关更多信息，请参阅 [cloudfoundry/php-buildpack ![外部链接图标](../../icons/launch-glyph.svg " 外部链接图标")](https://github.com/cloudfoundry/php-buildpack/blob/ff71ea41d00c1226d339e83cf2c7d6dda6c590ef/defaults/config/nginx/1.5.x/nginx-defaults.conf){: new_window}。
{: tsCauses}

```
daemon off;
error_log stderr notice;
pid @{HOME}/nginx/logs/nginx.pid;
```

`注意`消息仅供参考，可能并不表示有问题。您可以通过在 buildpack 的 nginx-defaults.conf 文件中将日志记录级别从 `stderr notice` 更改为 `stderr error` 来停止记录这些消息。例如：
{: tsResolve}

```
daemon off;
error_log stderr error;
pid @{HOME}/nginx/logs/nginx.pid;
```
有关如何更改缺省日志记录配置的更多信息，请参阅 [error_log ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](http://nginx.org/en/docs/ngx_core_module.html#error_log){: new_window}。
	




## Python
{: #ts_python}

### 无法将第三方 Python 库导入 {{site.data.keyword.Bluemix_notm}}
{: #ts_importpylib}

您可能无法将第三方 Python 库导入 {{site.data.keyword.Bluemix_notm}}。要解决该问题，请将配置文件添加到 Python 应用程序的根目录中。

当您尝试导入第三方 Python 库（如 `web.py` 库）时，`ibmcloud cf push` 命令会失败。
{: tsSymptoms}

Python 应用程序缺少配置信息。
{: tsCauses}

将 `requirements.txt` 文件和 `Procfile` 文件添加到 Python 应用程序的根目录中。以下信息假定您要导入 `web.py` 库：
{: tsResolve}

 1. 将 `requirements.txt` 文件添加到 Python 应用程序的根目录中。

 `requirements.txt` 文件指定 Python 应用程序所需的库数据包以及数据包版本。以下示例显示的是 `requirements.txt` 文件的内容，其中 `web.py==0.37` 指示要下载的 `web.py` 库版本为 0.37，而 `wsgiref==0.1.2` 指示 web.py 库所需的 Web 服务器网关接口版本为 0.1.2。 
	 ```
	 web.py==0.37
     wsgiref==0.1.2
	 ```
	 有关如何配置 `requirements.txt` 文件的更多信息，请参阅 [Requirements files](https://pip.readthedocs.org/en/1.1/requirements.html)。



 2. 将 `Procfile` 文件添加到 Python 应用程序的根目录中。`Procfile` 文件中必须包含 Python 应用程序的启动命令。在以下命令中，*yourappname* 是 Python 应用程序的名称，*PORT* 是 Python 应用程序在接收应用程序用户请求时必须使用的端口号。*$PORT* 为可选项。如果不在启动命令中指定 PORT，那么会使用应用程序内 `VCAP_APP_PORT` 环境变量下的端口号。
	```
	web: python <yourappname>.py $PORT
	```

现在，您可以将第三方 Python 库导入 {{site.data.keyword.Bluemix_notm}}。
