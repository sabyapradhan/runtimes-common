---

copyright:
  years: 2015, 2018
lastupdated: "2018-10-09"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}

# 管理 Liberty 和 Node.js 应用程序
{: #app_management}


“应用程序管理”是可以为 {{site.data.keyword.Bluemix}} 上的 Liberty 应用程序启用的一组开发和调试实用程序。
{:shortdesc}

## 应用程序管理实用程序
{: #Utilities}

### Liberty 实用程序
* [proxy](#proxy)
* [noproxy](#noproxy)
* [devconsole](#devconsole)
* [hc](#hc)
* [shell](#shell)
* [debug](#debug)
* [jmx](#jmx)
* [localjmx](#localjmx)

### Node.js 实用程序（不推荐）

**不推荐使用注释**：Node.js 应用程序不推荐使用“应用程序管理”实用程序。过去可同时用于 Node.js 和 Liberty 应用程序的某些实用程序现在仅可用于 Liberty 应用程序。Node.js 应用程序不推荐使用以下这些实用程序。

* [proxy](#proxy)
* [noproxy](#noproxy)
* [devconsole](#devconsole)
* [hc](#hc)
* [shell](#shell)
* [inspector](#inspector)
* [trace](#trace)

## 如何配置应用程序管理
{: #configure}

要启用“应用程序管理”实用程序，请将 *BLUEMIX_APP_MGMT_ENABLE* 环境变量的值设置为要启用的实用程序或实用程序列表，然后重新编译打包应用程序。可以启用多个实用程序，各实用程序之间用 **+** 分隔。

例如，要启用 *hc*、*debug* 和 *trace* 实用程序，请运行以下命令：

```
ibmcloud cf set-env myApp BLUEMIX_APP_MGMT_ENABLE hc+debug+trace
```
{: codeblock}

设置环境变量后，重新编译打包应用程序：

```
ibmcloud cf restage myApp
```
{: codeblock}

如果不希望“应用程序管理”实用程序随应用程序一起安装，请将 *BLUEMIX_APP_MGMT_INSTALL* 环境变量设置为“false”并重新编译打包应用程序。

例如，运行以下命令以在没有“应用程序管理”实用程序的情况下编译打包应用程序：

```
ibmcloud cf set-env myApp BLUEMIX_APP_MGMT_INSTALL false
ibmcloud cf restage myApp
```
{: codeblock}

## 限制
{: #restrictions}
* 使用“应用程序管理”对应用程序进行的更改是暂时性的，退出此方式后即会失效。此方式仅用于临时开发，出于性能考虑，不应该用作生产环境。
* 对于 Node.js 应用程序，如果在命令行上使用 `-c` 选项或在 `manifest.yml` 文件中设置 **start** 命令，那么大多数“应用程序管理”实用程序都不会正常运行。这些方法是 buildpack 覆盖，也是用于启动 Node.js 应用程序的反模式。为了获得最佳结果，请在 `package.json` 文件或 `Procfile` 中设置 **start** 命令。


### Liberty 实用程序
{: #liberty_utilities}

#### jmx
{: #jmx}

*jmx* 实用程序启用 JMX REST Connector 以允许远程 JMX 客户机使用 {{site.data.keyword.Bluemix_notm}} 用户凭证来管理应用程序。

可以使用 JMX 来监视应用程序的多个实例，但 JMX 需要为每个实例分别建立 JMX 连接。缺省情况是监视实例 0。要监视实例 1，可以使用以下代码片段：

```
ibmcloud cf ssh -i 1 -N -T -L 5000:127.0.0.1:5001
```
{: codeblock}

有关配置 JMX Connector 的更多信息，请参阅 [Configuring secure JMX connection to the Liberty profile ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www-01.ibm.com/support/knowledgecenter/was_beta_liberty/com.ibm.websphere.wlp.nd.multiplatform.doc/ae/twlp_admin_restconnector.html){:new_window}。

**重要信息**：*jmx* 实用程序不会启动 *proxy*。

#### localjmx
{: #localjmx}

*localjmx* 实用程序启用 [localConnector-1.0 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](http://www.ibm.com/support/knowledgecenter/SSEQTP_liberty/com.ibm.websphere.wlp.doc/ae/rwlp_feature_localConnector-1.0.html){:new_window} Liberty 功能。*localjmx* 与本地端口转发组合使用，可充当允许远程 JMX 客户机管理应用程序的备用方法。


**开始之前**：*localjmx* 需要安装 JConsole。

*localjmx* 实用程序仅适用于在 Diego 单元上运行的应用程序。要使用 *localjmx*，请先使用 `ibmcloud cf ssh` 命令来建立端口转发。例如：

```
ibmcloud cf ssh -N -T -L 5000:127.0.0.1:5000 <appName>
```
{: codeblock}

接下来，要与 JConsole 建立连接，请选择**远程进程**，指定 `127.0.0.1:5000`，并使用不安全的连接。

#### proxy（Node.js 不推荐使用）
{: #proxy}

*proxy* 实用程序可提供应用程序和 {{site.data.keyword.Bluemix_notm}} 之间最少的应用程序管理。

启用该程序后，buildpack 会启动位于应用程序的运行时与容器之间的代理。*proxy* 实用程序会处理应用程序收到的所有请求。它会根据请求的类型，执行“应用程序管理”操作或将请求转发给应用程序。通过使用 *proxy*，即使应用程序崩溃，应用程序容器也可继续运行。代理还支持增量文件更新，这将为 Node.js 应用程序启用*实时编辑*方式。

一些“应用程序管理”实用程序需要将 *proxy* 实用程序与应用程序一起使用，并可自动启动 *proxy*。

**不推荐使用注释**：对于 Node.js，{{{site.data.keyword.Bluemix_notm}} 版本的 Cloud Foundry 在 Diego Cell 中运行。*noproxy* 功能适用于非 Diego Cell。

#### noproxy（Node.js 不推荐使用）
{: #noproxy}

如果 *proxy* 实用程序已由其他实用程序自动启动，*noproxy* 实用程序可将其禁用。使用 Diego 时，proxy 不是必需的，因为 Diego 提供了直接通过 *ssh* 访问应用程序并设置端口转发的功能。

*noproxy* 实用程序仅适用于在 Diego Cell 中运行的应用程序。

**不推荐使用注释**：*noproxy* 实用程序会禁用 *proxy*。因为不推荐 Node.js 使用 *proxy* 且 proxy 已不再运行，所以不需要 *noproxy*。

#### devconsole（Node.js 不推荐使用）
{: #devconsole}

用户可以使用 (*devconsole*) 开发控制台实用程序来重新启动、停止或暂停其应用程序。用户还可以使用 *devconsole* 启用或访问 shell 和 inspector 实用程序。可以使用以下 URL 来访问 *devconsole*：
```
  https://<yourappname>.mybluemix.net/bluemix-debug/manage
```
{: codeblock}

对于 Node V6.3.0 或更高版本，开发控制台提供了用于重新启动应用程序的按钮，并提供了对 *shell* 实用程序的访问。有关更多信息，请参阅 *inspector* 讨论。

**重要信息**：*devconsole* 实用程序会启动 *proxy*。

**不推荐使用注释**：对 Node.js 不使用 *devconsole* 实用程序，而是使用 {{site.data.keyword.Bluemix_notm}} 控制台。从控制台导航至应用程序**运行时**页面。在**运行时**页面中，您可以停止、启动、重命名和删除应用程序。还可以访问 shell 和其他信息。

#### hc（Node.js 不推荐使用）
{: #hc}

(*hc*) Health Center 代理程序支持通过 Health Center 客户机来监视应用程序。对于 Node.js，*hc* 代理程序仅可用于 IBM SDK for Node.js buildpack 随附的 Node.js 运行时版本。有关当前运行时集的信息，请参阅 [sdk-for-nodejs buildpack 的最新更新](/docs/runtimes/nodejs/updates.html)。

启用 Health Center 代理程序后，可以使用 IBM Monitoring and Diagnostic Tools 来分析 Liberty 和 Node.js 应用程序的性能。有关更多信息，请参阅 [How to analyze the performance of Liberty Java or Node.js apps in {{site.data.keyword.Bluemix_notm}} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://developer.ibm.com/bluemix/2015/07/03/how-to-analyze-performance-in-bluemix/){:new_window}。

**重要信息**：*hc* 实用程序会启动 *proxy*。

**将 *hc* 与 *noproxy* 一起使用**

*hc* 实用程序可以与 *noproxy* 一起使用。要将 Health Center 与 *noproxy* 一起使用，请先使用 `ibmcloud cf ssh` 命令来建立端口转发。例如：

```
ibmcloud cf ssh -N -T -L 1883:127.0.0.1:1883 <appName>
```
{: codeblock}

接下来，要与 Health Center 客户机建立连接，请使用 [MQTT 连接 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](http://www.ibm.com/support/knowledgecenter/SS3KLZ/com.ibm.java.diagnostics.healthcenter.doc/topics/connectingtojvm.html){: new_window}，并将主机指定为 `127.0.0.1`，端口指定为 `1883`。

**不推荐使用注释：**对 Node.js 不使用 Health Center 代理程序，而是使用 {{site.data.keyword.Bluemix_notm}} 控制台。从控制台导航至您可以在其中访问 CPU 使用情况、内存使用情况和磁盘空间的应用程序**运行时**页面。该页面还有一个您可以访问任何环境变量的选项卡。

#### shell（Node.js 不推荐使用）
{: #shell}

*shell* 实用程序可启用基于 Web 的 shell。可以通过 *devconsole* 实用程序或使用以下 URL 来访问 *shell*：

```
  https://<yourappname>.mybluemix.net/bluemix-debug/shell
```
{: codeblock}

访问 *shell* 实用程序后，会显示一个终端窗口，支持通过 shell 访问应用程序。可以执行常规 shell 中支持的所有功能，例如编辑文件、检查内存使用情况或运行诊断命令。

**重要信息**：*shell* 实用程序还会启动 *proxy*。

Diego 通过 `ibmcloud cf ssh` 命令提供交互式 shell，因此 *shell* 实用程序仅对 DEA 上运行的应用程序有用。
{: .tip}

**不推荐使用注释：**对于 Node.js，`cf ssh` 命令继续运作的同时，您还可以从 {{site.data.keyword.Bluemix_notm}} 控制台访问 shell。从控制台导航至应用程序**运行时**页面，您可以在其中找到用于在 Web 页面中拉取 shell 的选项卡。

### Node.js 实用程序（不推荐）
{: #node_utilities}

#### inspector（不推荐）
{: #inspector}

inspector 实用程序可用于创建 CPU 使用情况概要文件，添加断点并调试代码，所有这些操作均于应用程序在 {{site.data.keyword.cloud_notm}} 上运行期间执行。对于低于 Node.js V6.3.0 的版本，inspector 会启用 Node Inspector 调试器界面。有关 Node Inspector 的更多信息，请参阅 [GitHub 上的 node-inspector ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/node-inspector/node-inspector){: new_window} 的自述文件。对于 Node.js V6.3.0 及更高版本，*inspector* 实用程序会使用 [Node.js 的 V8 Inspector 集成 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://nodejs.org/dist/latest-v6.x/docs/api/debugger.html#debugger_v8_inspector_integration_for_node_js){:new_window}。

##### 对于 6.3.0 之后的 Node.js 版本
启动调试方式后，会自动启用 *proxy*，即便使用的 Node.js 版本不包含 *proxy* 也是如此。高于 Node.js V6.3.0 的版本不包含 *proxy*。如果将 *inspector* 实用程序与高于 Node.js V6.3.0 的版本一起使用，可以通过 *noproxy* 重新禁用 *proxy*。

您可以不使用 *proxy* 来访问 *inspector* 界面，而改为使用 Google Chrome Web 浏览器的“开发者工具”功能。  

使用以下命令通过本地端口转发来启用对 URL 的访问：
```
ibmcloud cf ssh -N -T -L 9229:127.0.0.1:9229 <appName>
```
{: codeblock}

使用以下命令获取应用程序的启动日志。
```
ibmcloud cf logs <appName> --recent
```
{: codeblock}

如果 *inspector* 实用程序处于活动状态，那么日志会包含类似于以下内容的消息：
```
 ... You will need a SSH tunnel for port 9229 to be able to use the Chrome DevTools to remotely debug your app
 ... Starting app with 'node --inspect=9229  app.js '
```
{: codeblock}

使用最新版本的 Google Chrome Web 浏览器来浏览至 `chrome://inspect`。
通过此 URL，可以看到列出了应用程序，以及应用程序文件（例如 `file://home/vcap/app/app.js`）的链接，然后选择 **inspect** 来访问 inspect 界面。

##### 对于 6.3.0 之前的 Node.js 版本
如果使用了 *proxy*，那么可以通过以下网址访问 *inspector* 界面：`https://myApp.mybluemix.net/bluemix-debug/inspector`。

如果未使用 *proxy* 实用程序，请通过以下命令使用本地端口转发来启用对应用程序 URL 的访问：

```
ibmcloud cf ssh -N -T -L 8790:127.0.0.1:8790 <appName>
```
{: codeblock}

然后，通过 URL `http://127.0.0.1:8790` 来访问 inspector。

**不推荐使用注释：**您仍可以通过稍微更改设置使用 *inspector* 功能。在应用程序 `manifest.yml` 文件中，将启动命令设置为“node --inspect=9229 app.js”。然后遵循 6.3.0 之后的 Node.js 版本的指示信息。然而，通常指示检验器活动时间的日志不可见，但其正常运作。


#### trace（不推荐）
{: #trace}

trace 实用程序允许您动态设置跟踪级别（如果应用程序使用的是 log4js、ibmbluemix 或 bunyan 日志记录模块）。

浏览至 {{site.data.keyword.cloud_notm}} Web 控制台中的**实例详细信息**页面，然后选择**操作**来查看 UI。

注：支持的依赖项版本：

* log4js：(0.6.0 - 0.6.24)
* bunyan：(1.0.0, 1.0.1, 1.1.0 - 1.1.3, 1.2.0 - 1.2.3, 1.3.0 - 1.3.5)
* ibmbluemix：(1.0.0-20140707-1250)-(1.0.0-20150409-1328)


应用程序使用 `-b buildpack` 选项启动时，trace 实用程序不可用。

trace 实用程序不会启动 *proxy*。
