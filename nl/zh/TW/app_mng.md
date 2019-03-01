---

copyright:
  years: 2015, 2018
lastupdated: "2018-10-09"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}

# 管理 Liberty 及 Node.js 應用程式
{: #app_management}


「應用程式管理」是一組開發及除錯公用程式，可以針對您在 {{site.data.keyword.Bluemix}} 上的 Liberty 應用程式啟用。
{:shortdesc}

## 應用程式管理公用程式
{: #Utilities}

### Liberty 公用程式
* [proxy](#proxy)
* [noproxy](#noproxy)
* [devconsole](#devconsole)
* [hc](#hc)
* [shell](#shell)
* [debug](#debug)
* [jmx](#jmx)
* [localjmx](#localjmx)

### Node.js 公用程式（已淘汰）

**淘汰附註**：Node.js 應用程式的應用程式管理公用程式已淘汰。同時適用於 Node.js 和 Liberty 應用程式的部分公用程式，現在只適用於 Liberty 應用程式。Node.js 應用程式的這些公用程式已淘汰。

* [proxy](#proxy)
* [noproxy](#noproxy)
* [devconsole](#devconsole)
* [hc](#hc)
* [shell](#shell)
* [inspector](#inspector)
* [trace](#trace)

## 如何配置應用程式管理
{: #configure}

若要啟用「應用程式管理」公用程式，請將 *BLUEMIX_APP_MGMT_ENABLE* 環境變數的值設為您要啟用之公用程式或公用程式的清單，然後重新編譯打包應用程式。您可以透過使用 **+** 來分隔多個公用程式，以啟用它們。

例如，若要啟用 *hc*、*debug* 及 *trace* 公用程式，請執行下列指令：

```
ibmcloud cf set-env myApp BLUEMIX_APP_MGMT_ENABLE hc+debug+trace
```
{: codeblock}

設定環境變數之後，請重新編譯打包應用程式：

```
ibmcloud cf restage myApp
```
{: codeblock}

如果不想和應用程式一起安裝「應用程式管理」公用程式，請將 *BLUEMIX_APP_MGMT_INSTALL* 環境變數設為 'false'，然後重新編譯打包應用程式。

例如，執行下列指令來編譯打包應用程式，而不包含「應用程式管理」公用程式：

```
ibmcloud cf set-env myApp BLUEMIX_APP_MGMT_INSTALL false
ibmcloud cf restage myApp
```
{: codeblock}

## 限制
{: #restrictions}
* 使用「應用程式管理」對應用程式進行的變更是暫時性變更，會在您結束此模式之後遺失。由於效能之故，此模式僅供暫時性開發使用，而不用來作為正式作業環境。
* 對於 Node.js 應用程式，如果您在 `manifest.yml` 檔案中設定 **start** 指令，或在指令行上使用 `-c` 選項，則大部分的「應用程式管理」公用程式無法運作。那些方法是建置套件置換項目，而且是啟動 Node.js 應用程式的反面模式 (anti-pattern)。若要獲得最佳的結果，請在 `package.json` 檔案或 `Procfile` 中設定 **start** 指令。


### Liberty 公用程式
{: #liberty_utilities}

#### jmx
{: #jmx}

*jmx* 公用程式可使用 {{site.data.keyword.Bluemix_notm}} 使用者認證，讓「JMX REST 連接器」容許遠端 JMX 用戶端管理應用程式。

您可使用 JMX 來監視應用程式的多個實例，但每個實例都需要個別的 JMX 連線。預設值為監視實例 0。若要監視實例 1，您可以使用下列程式碼 Snippet：

```
ibmcloud cf ssh -i 1 -N -T -L 5000:127.0.0.1:5001
```
{: codeblock}

如需配置 JMX 連接器的相關資訊，請參閱[配置 Liberty 設定檔的安全 JMX 連線 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www-01.ibm.com/support/knowledgecenter/was_beta_liberty/com.ibm.websphere.wlp.nd.multiplatform.doc/ae/twlp_admin_restconnector.html){:new_window}。

**重要事項**：*jmx* 公用程式不會啟動 *proxy*。

#### localjmx
{: #localjmx}

*localjmx* 公用程式會啟用 [localConnector-1.0 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](http://www.ibm.com/support/knowledgecenter/SSEQTP_liberty/com.ibm.websphere.wlp.doc/ae/rwlp_feature_localConnector-1.0.html){:new_window} Liberty 特性。*localjmx* 與本端埠轉遞合併使用時，可啟用容許遠端 JMX 用戶端管理應用程式的替代方式。


**開始之前**：*localjmx* 需要您安裝 JConsole。

*localjmx* 公用程式僅適用於 Diego Cell 上執行的應用程式。若要使用 *localjmx*，請先使用 `ibmcloud cf ssh` 指令來建立埠轉遞。例如：

```
ibmcloud cf ssh -N -T -L 5000:127.0.0.1:5000 <appName>
```
{: codeblock}

接下來，若要使用 JConsole 進行連線，請選擇**遠端處理程序**，指定 `127.0.0.1:5000`，然後使用不安全連線。

#### proxy（針對 Node.js 已淘汰）
{: #proxy}

*proxy* 公用程式提供您的應用程式與 {{site.data.keyword.Bluemix_notm}} 之間的最低應用程式管理。

若已啟用，建置套件會啟動位在應用程式運行環境與容器之間的 proxy 代理程式。*proxy* 公用程式會處理應用程式接收的所有要求。根據要求類型，它會採取「應用程式管理」動作，或將要求轉遞給應用程式。藉由使用 *proxy*，即使應用程式當機，您的應用程式容器還是會繼續運作。proxy 代理程式也容許進行漸進式檔案更新，以啟用 Node.js 應用程式的*即時編輯* 模式。

有些「應用程式管理」公用程式需要您使用 *proxy* 公用程式與您的應用程式搭配，並且可以自動啟動 *proxy*。

**淘汰附註**：針對 Node.js，{{{site.data.keyword.Bluemix_notm}} 版本的 Cloud Foundry 會在 Diego Cell 中執行。*noproxy* 特性為針對非 Diego Cell。

#### noproxy（針對 Node.js 已淘汰）
{: #noproxy}

*noproxy* 可停用由其他公用程式自動啟動的 *proxy* 公用程式。有了 Diego，就不需要 proxy，因為 Diego 提供了直接透過 *ssh* 連上應用程式並設定埠轉遞的功能。

*noproxy* 公用程式只適用於 Diego Cell 中執行的應用程式。

**淘汰附註**：*noproxy* 公用程式會停用 *proxy*。由於 *proxy* 已針對 Node.js 淘汰且不再運作，因此不需要 *noproxy*。

#### devconsole（針對 Node.js 已淘汰）
{: #devconsole}

使用者可以使用 (*devconsole*) 開發主控台公用程式來重新啟動、停止或暫停其應用程式。使用者也可以使用 *devconsole* 啟用或存取 shell 及 inspector 公用程式。您可以使用下列 URL 來取存 *devconsole*：
```
  https://<yourappname>.mybluemix.net/bluemix-debug/manage
```
{: codeblock}

若為 6.3.0 版或更新版本的 Node，開發主控台提供了應用程式的重新啟動按鈕，以及對 *shell* 公用程式的存取。如需相關資訊，請參閱 *inspector* 討論。

**重要事項**：*devconsole* 公用程式會啟動 *proxy*。

**淘汰附註**：針對 Node.js 請使用 {{site.data.keyword.Bluemix_notm}} 主控台，而不要使用 *devconsole* 公用程式。從主控台，導覽至應用程式**運行環境**頁面。從**運行環境**頁面，您可以停止、啟動、重新命名及刪除應用程式。此外，您也可以存取 Shell 及其他資訊。

#### hc（針對 Node.js 已淘汰）
{: #hc}

「(*hc*) 性能檢測中心」代理程式可讓「性能檢測中心」用戶端監視您的應用程式。對於 Node.js，*hc* 代理程式只可用於 IBM SDK for Node.js 建置套件隨附的 Node.js 運行環境版本。如需現行運行環境集，請參閱 [sdk-for-nodejs 建置套件的最新更新](/docs/runtimes/nodejs/updates.html)。

已啟用「性能檢測中心」代理程式後，您可以使用 IBM Monitoring and Diagnostic Tools 來分析 Liberty 及 Node.js 應用程式的效能。如需相關資訊，請參閱[如何分析 {{site.data.keyword.Bluemix_notm}} 中 Liberty Java or Node.js 的效能 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://developer.ibm.com/bluemix/2015/07/03/how-to-analyze-performance-in-bluemix/){:new_window}。

**重要事項：***hc* 公用程式會啟動 *proxy*。

**搭配使用 *hc* 與 *noproxy***

*hc* 公用程式可以與 *noproxy* 一起使用。若要搭配使用「性能檢測中心」與 *noproxy*，請先使用 `ibmcloud cf ssh` 指令來建立埠轉遞。例如：

```
ibmcloud cf ssh -N -T -L 1883:127.0.0.1:1883 <appName>
```
{: codeblock}

接下來，若要使用「性能檢測中心」用戶端進行連線，請使用 [MQTT 連線 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](http://www.ibm.com/support/knowledgecenter/SS3KLZ/com.ibm.java.diagnostics.healthcenter.doc/topics/connectingtojvm.html){: new_window}，然後將主機指定為 `127.0.0.1`，並將埠指定為 `1883`。

**淘汰附註**：針對 Node.js 請使用 {{site.data.keyword.Bluemix_notm}} 主控台，而不要使用 Health Center 代理程式。從主控台，導覽至應用程式**運行環境**頁面，您可以在該處存取 CPU 使用率、記憶體用量及磁碟空間。頁面也有一個標籤，您可以在標籤上存取任何環境變數。

#### shell（針對 Node.js 已淘汰）
{: #shell}

*shell* 公用程式可啟用 Web 型 Shell。您可以從 *devconsole* 公用程式或使用下列 URL 來存取 *shell*：

```
  https://<yourappname>.mybluemix.net/bluemix-debug/shell
```
{: codeblock}

在您存取 *shell* 公用程式之後，即會顯示終端機視窗，並透過 Shell 連入應用程式。您可以執行一般 Shell 中所支援的所有作業，例如編輯檔案、檢查記憶體用量，或執行診斷指令。

**重要事項：***shell* 公用程式也會啟動 *proxy*。

Diego 透過 `ibmcloud cf ssh` 指令提供互動式 Shell，因此 *shell* 公用程式只適用於在 DEA 上執行的應用程式。
{: .tip}

**淘汰附註**：針對 Node.js，雖然 `cf ssh` 指令仍能運作，您也可以從 {{site.data.keyword.Bluemix_notm}} 主控台存取 Shell。從主控台，導覽至應用程式**運行環境**頁面，您可以在該處找到標籤，以在網頁裡顯示 Shell。

### Node.js 公用程式（已淘汰）
{: #node_utilities}

#### inspector（已淘汰）
{: #inspector}

inspector 公用程式可用來建立 CPU 用量設定檔、新增岔斷點，以及對程式碼進行除錯，所有作業都是應用程式在 {{site.data.keyword.cloud_notm}} 上執行時進行。若為 Node.js 6.3.0 版以前的版本，inspector 會啟用 Node inspector 除錯器介面。如需 Node inspector 的相關資訊，請參閱 [GitHub 上的 node-inspector ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/node-inspector/node-inspector){: new_window} 的 Readme。若為 Node.js 6.3.0 版及更新版本，*inspector* 公用程式會使用 [Node.js 的第 8 版 Inspector 整合 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://nodejs.org/dist/latest-v6.x/docs/api/debugger.html#debugger_v8_inspector_integration_for_node_js){:new_window}。

##### 若為 Node.js 6.3.0 之後的版本
當您啟動除錯模式時，即使使用不包含 *proxy* 的 Node.js 版本，也會自動啟用 *proxy*。Node.js 6.3.0 之後的版本未包含 *proxy*。如果您搭配使用 *inspector* 與 Node.js 6.3.0 之後的版本，則可以使用 *noproxy* 再次關閉 *proxy*。

您可以使用 Google Chrome Web 瀏覽器的「開發者工具」功能，而不是使用 *proxy* 來存取 *inspector* 介面。  

請使用下列指令，透過本端埠轉遞來啟用 URL 的存取：
```
ibmcloud cf ssh -N -T -L 9229:127.0.0.1:9229 <appName>
```
{: codeblock}

使用下列指令來取得應用程式的啟動日誌。
```
ibmcloud cf logs <appName> --recent
```
{: codeblock}

如果 *inspector* 公用程式為作用中，則日誌會包含類似下列內容的訊息：
```
 ... You will need a SSH tunnel for port 9229 to be able to use the Chrome DevTools to remotely debug your app
 ... Starting app with 'node --inspect=9229  app.js '
```
{: codeblock}

請使用最新版本的 Google Chrome Web 瀏覽器，以瀏覽至 `chrome://inspect`。
從此 URL 中，您會看到您的應用程式與應用程式檔案的鏈結列在一起（例如 `file://home/vcap/app/app.js`），然後選取**檢查**來存取檢查介面。

##### 若為 Node.js 6.3.0 之前的版本
如果您使用 *proxy*，則可以在 `https://myApp.mybluemix.net/bluemix-debug/inspector` 存取 *inspector* 介面。

如果您未使用 *proxy* 公用程式，請使用下列指令，透過使用本端埠轉遞來啟用應用程式 URL 的存取：

```
ibmcloud cf ssh -N -T -L 8790:127.0.0.1:8790 <appName>
```
{: codeblock}

然後，從 URL `http://127.0.0.1:8790` 存取 inspector。

**淘汰附註**：藉由稍微變更設定，您仍可以使用 *inspector* 特性。在應用程式 `manifest.yml` 檔案中，將啟動指令設為 'node --inspect=9229 app.js'。然後，您可以遵循 Node.js 6.3.0 之後版本的指示。不過，無法看到正常會指出檢查程式在作用中的日誌，但它運作正確。


#### trace（已淘汰）
{: #trace}

trace 公用程式可讓您在應用程式使用 log4js、ibmbluemix 或 bunyan 記載模組時動態設定追蹤層次。

請導覽至 {{site.data.keyword.cloud_notm}} Web 主控台的**實例詳細資料**頁面，然後選取**動作**來查看使用者介面。

附註：支援的相依關係版本：

* log4js：(0.6.0 - 0.6.24)
* bunyan：（1.0.0、1.0.1、1.1.0 - 1.1.3、1.2.0 - 1.2.3、1.3.0 - 1.3.5）
* ibmbluemix：(1.0.0-20140707-1250)-(1.0.0-20150409-1328)


使用 `-b buildpack` 選項啟動應用程式時，無法使用 trace 公用程式。

trace 公用程式不會啟動 *proxy*。
