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


# 運行環境的疑難排解
{: #runtimes}

您在使用 [{{site.data.keyword.Bluemix}} 運行環境](index.html)時，可能會遇到問題。在許多情況下，您都可以遵循一些簡單的步驟來回復這些問題。
{:shortdesc}

* [一般疑難排解](#ts_all)
* [Liberty for Java](#ts_liberty)
* [SDK for Node.js](#ts_nodejs)
* [ASP.NET Core](#ts_dotnet)
* [PHP](#ts_php)
* [Python](#ts_python)

## 一般疑難排解
{: #ts_all}

### 推送應用程式時使用已作廢的建置套件
{: #ts_loading_bp}

推送應用程式時，您可能無法使用最新的建置套件元件。您可以使用具有內建機制的建置套件，以避免載入已作廢的元件，或者您可以刪除應用程式快取目錄中的內容，然後才推送或重新編譯打包應用程式。

當您在建置套件更新之後，推送或重新編譯打包應用程式時，不會自動載入最新的建置套件元件。因此，您的應用程式會使用快取中已作廢的建置套件元件。自您上次推送應用程式之後，套用至建置套件的更新都不會實作。
{: tsSymptoms}

部分建置套件未配置為自動從網際網路下載所有更新的元件，來確保您隨時使用最新的版本。
{: tsCauses}

您可以使用具有內建機制的建置套件來避免載入已作廢的元件，例如，下列建置套件：
{: tsResolve}

  * [Cloud Foundry Java 建置套件 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/cloudfoundry/java-buildpack){: new_window}。這個建置套件具有內建的機制，可以確保使用最新版本的建置套件。如需此機制運作方式的相關資訊，請參閱 [extending-caches.md ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/cloudfoundry/java-buildpack/blob/master/docs/extending-caches.md){: new_window}。
  * [Cloud Foundry Node.js 建置套件 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/cloudfoundry/nodejs-buildpack){: new_window}。這個建置套件的功能與使用環境變數類似。若要讓 Node.js 建置套件每次都從網際網路下載 node 模組，請在 {{site.data.keyword.Bluemix_notm}} 指令行介面中鍵入下列指令： 	

  ```
  set NODE_MODULES_CACHE=false
  ```

如果您使用的建置套件未提供自動載入最新元件的機制，您可以手動刪除快取目錄中的內容，然後重新推送應用程式。請使用下列步驟：

 1. 移出空值建置套件的分支，例如 `https://github.com/ryandotsmith/null-buildpack`。如需如何移出分支的相關資訊，請參閱 [Git Basics - Getting a Git Repository ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](http://www.git-scm.com/book/en/v2/Git-Basics-Getting-a-Git-Repository){: new_window}。  
 2. 將下列這一行新增到 `null-buildpack/bin/compile` 檔案並確定變更。如需如何確定變更的相關資訊，請參閱 [Git Basics - Recording Changes to the Repository ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](http://www.git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository){: new_window}。
  ```
  rm -rfv $2/*
  ```
 3. 使用下列指令，用已修改的空值建置套件推送應用程式，以刪除快取。完成此步驟之後，應用程式快取目錄中的所有內容都會刪除。
  ```
  ibmcloud cf push appname -p app_path -b <modified_null_buildpack>
  ```
 4. 使用下列指令，用您要使用的最新建置套件來推送應用程式：
  ```
  ibmcloud cf push appname -p app_path -b <latest_buildpack>
  ```

### 應用程式繼續重新啟動
{: #ts_apprestart}

應用程式會繼續損毀並重新啟動。
{: tsSymptoms}

應用程式容器生命週期（例如撤離和關閉）可能會影響應用程式功能。  
{: tsCauses}

識別應用程式容器生命週期的哪個步驟導致應用程式部署發生錯誤。進一步瞭解 [Cloud Foundry 應用程式生命週期](https://docs.cloudfoundry.org/devguide/deploy-apps/app-lifecycle.html#evacuation)。
{: tsResolve}

### 已停用「實例詳細資料」頁面上的「動作」按鈕（已淘汰）
  {: #ts_actionsbutton}

  已停用「實例詳細資料」頁面上的「動作」按鈕。
  {: tsSymptoms}

  發生此問題的原因如下：
  {: tsCauses}

   * 應用程式不是使用內嵌的 Liberty 建置套件所部署。
   * 應用程式是使用舊版 Liberty 建置套件所部署。

  如果問題是舊版 Liberty 建置套件所造成，請在 {{site.data.keyword.Bluemix_notm}} 中重新部署應用程式。否則，您可以將下列用戶端應用程式日誌檔提供給支援團隊：
  {: tsResolve}

    * logs/messages.log
    * logs/stdout.log
    * logs/stderr.log


### 需要認證才能開啟追蹤或傾出視窗（已淘汰）
  {: #ts_username}

  需要有使用者名稱和密碼，才能開啟追蹤或傾出視窗。
  {: tsSymptoms}

  此問題的發生原因是登入階段作業逾時。
  {: tsCauses}

  請重新輸入使用者名稱和密碼。
  {: tsResolve}


### 追蹤或傾出作業在執行時發生錯誤（已淘汰）
  {: #ts_target}

  當追蹤或傾出作業在執行時顯示錯誤訊息。此訊息指出應用程式的目標實例不是處於執行中狀態：
  {: tsSymptoms}

  ```
  Instance 0: Trace specification is set successfully
  Instance 2: Trace specification is set successfully
  Instance 1: Target instance 1 for app abcd is not in the running state.
  Instance 3: Trace specification is set successfully
  Instance 4: Trace specification is set successfully
  ```

  發生此問題的原因如下：
  {: tsCauses}

    * 追蹤或傾出管理功能僅適用於執行中的應用程式實例。在已停止、啟動中或已當機的應用程式實例上，無法執行追蹤或傾出作業。
    * 當追蹤或傾出對話框開啟時，應用程式實例的狀態為變更中。

  請關閉視窗，然後再重新開啟。
  {: tsResolve}


### 實例具有不同的 traceSpecification 配置（已淘汰）
  {: #ts_different_config}

  對於某個應用程式的不同實例，您可能會看到不同的 traceSpecification 配置。
  {: tsSymptoms}

  發生此行為的原因如下：
  {: tsCauses}

    * 您先前已變更一個以上實例的配置。如果您變更某個實例的 traceSpecification 配置，該變更並不適用於相同應用程式的其他實例。例如，您的應用程式使用 log4j，而這個應用程式有兩個實例。您可以將實例 0 的記載層次從 info 變更為 debug，但實例 1 的記載層次仍為 info。

  不需要執行任何動作。這是預期的行為。
  {: tsResolve}


### 已超出磁碟配額
  {: #ts_diskquota}

  您可能會在應用程式日誌中看到已超出磁碟配額。

  您在應用程式的日誌中看到`已超出磁碟配額`訊息。
  {: tsSymptoms}

  發生此問題的原因為下列其中一項：
  {: tsCauses}

    * 傾出檔案是由執行中的應用程式實例所產生，而這些檔案耗盡配置的磁碟配額。依預設，一個應用程式實例的磁碟配額為 1 GB。您可以按一下**儀表板 > 應用程式 > 應用程式運行環境**，來檢查您的磁碟用量。下列範例顯示一個應用程式的兩個實例的運行環境資訊，包括磁碟用量：
      ```
      Instance	State	CPU	Memory Usage	Disk Usage

  	0		Running	1.0%	344.8MB/512MB	236.8MB/1GB
  	2		Running	2.3%	361.2MB/512MB	235.7MB/1GB
      ```
    * 磁碟配額受限於現行組織配額。

  請使用下列其中一種方法：
  {: tsResolve}

    * 在下載傾出檔案之後刪除它們。
    * 在部署資訊清單中包括下列項目，以較大的磁碟配額來重新部署應用程式：
      ```
      disk_quota: 2048
      ```

## Liberty for Java
{: #ts_liberty}

### 應用程式無法開始接受連線
{: #health_check_timeout}

Liberty 應用程式無法啟動，錯誤為_無法開始接受連線_。例如，日誌中的錯誤可能類似下列內容：
{: tsSymptoms}

```
   2016-11-14T14:44:58.45+0000 [API/0]      OUT App instance exited with guid 21ac63eb-9595-428a-94c7-b0b02aaf77cc payload: {"cc_partition"=>"default", "droplet"=>"21ac63eb-9595-428a-94c7-b0b02aaf77cc", "version"=>"b2772438-92de-4d47-b680-ea772ac2288a", "instance"=>"f4799c8c89214bbd8067883c3ffe23e0", "index"=>0, "reason"=>"CRASHED", "exit_status"=>255, "exit_description"=>"failed to accept connections within health check timeout", "crash_timestamp"=>1479134698}
   2016-11-14T14:45:07.50+0000 [DEA/4]      ERR Instance (index 0) failed to start accepting connections
```
{: #codeblock}

{{site.data.keyword.Bluemix_notm}} 會對應用程式執行性能檢查，以查看它是否已順利啟動。性能檢查會測試應用程式是否正在接聽指派給應用程式的埠。此檢查的預設逾時是 60 秒，而部分應用程式可能需要超過 60 秒的時間才能啟動。應用程式需要較久的時間才能啟動的原因有許多。例如，連結服務（例如 [Monitoring and Analytics](/docs/services/monana/index.html#gettingstartedtemplate) 或 [New Relic](/docs/runtimes/liberty/monitoring/newRelic.html)）或[啟用除錯器](/docs/runtimes/common/app_mng.html#debug)便會增加啟動時間。應用程式也可能執行需要較久時間才能完成的起始設定步驟。
{: tsCauses}

請先檢查日誌，以尋找任何可能導致 Liberty 應用程式失敗的明顯錯誤。如果未發現任何明顯錯誤，請嘗試下列動作：
{: tsResolve}

#### 增加性能檢查逾時

* 使用 `ibmcloud cf push` 指令來部署應用程式時，請使用 `-t` 選項來指定較長的應用程式啟動逾時。例如：

        ```
	ibmcloud cf push myApp -t 180
        ```
	{: #codeblock}

* 性能檢查逾時也可以指定於 manifest.yml 檔案中。例如：

        ---
           ...
           timeout: 180
        {: #codeblock}

#### 停用 appstate 特性

appstate 特性會與 {{site.data.keyword.Bluemix_notm}} 性能檢查處理程序整合，以確定 Liberty 應用程式已完整起始設定，然後應用程式才能收到 HTTP 要求。完整起始設定應用程式之後，appstate 特性便不再有任何效果。此特性的負面影響是部分應用程式可能需要較長的時間才能啟動。若要停用 appstate 特性，請在應用程式上設定下列環境內容，並重新編譯打包應用程式：

```
ibmcloud cf set-env myApp JBP_CONFIG_LIBERTY "app_state: false"
```
{: #codeblock}

#### 考慮重構應用程式

如果您的應用程式需要很長的時間才能起始設定，您可能需要重構應用程式才能執行延遲及（或）非同步起始設定。

#### 使用 "no-route" 選項部署

1. 使用 "--no-route" 選項部署應用程式。這樣會停用埠性能檢查。例如：

        ```
	ibmcloud cf push myApp –no-route
        ```
	{: #codeblock}

2. 在起始設定應用程式之後，請將路徑對映至應用程式。例如：

        ibmcloud cf map-route myApp mybluemix.net
        {: #codeblock}

### IBM 閘道的 SSL 錯誤
{: #ssl_handshake_failure}


下列錯誤顯示於日誌中，而且應用程式可能無法啟動：
{: tsSymptoms}

```
    2016-11-03T12:32:44.82-0200 [App/0]      ERR java.security.cert.CertPathValidatorException: Certificate chaining error
    2016-11-03T12:32:44.83-0200 [App/0]      ERR [ERROR   ] CWPKI0022E: SSL HANDSHAKE FAILURE:  A signer with SubjectDN CN=*.gateway.prd.na.ca.ibmserviceengage.com, O=International Business Machines Corp., L=Armonk, ST=New York, C=US was sent from the target host.  The signer might need to be added to local trust store /home/vcap/app/wlp/usr/servers/defaultServer/resources/security/key.jks, located in SSL configuration alias defaultSSLConfig.  The extended error message from the SSL handshake exception is: PKIX path building failed: java.security.cert.CertPathBuilderException: PKIXCertPathBuilderImpl could not build a valid CertPath.; internal cause is:
    2016-11-03T12:32:44.83-0200 [App/0]      ERR java.security.cert.CertPathValidatorException: The certificate issued by CN=DigiCert Global Root CA, OU=www.digicert.com, O=DigiCert Inc, C=US is not trusted; internal cause is:
    2016-11-03T12:32:44.83-0200 [App/0]      ERR java.security.cert.CertPathValidatorException: Certificate chaining error
```
{: codeblock}

下列情況可能會產生錯誤：Monitoring & Analytics 服務已連結至 Liberty 應用程式，而且 Liberty 應用程式已部署為包含可配置 Liberty ssl-1.0 特性之 server.xml 的伺服器目錄或包裝伺服器。將 Monitoring & Analytics 服務連結至 Liberty 應用程式，可讓運行環境透過安全連線來連接至服務。該安全連線是使用預設 SSL 設定所建立。因為預設 SSL 設定指定於 Liberty 的 server.xml 中，所以已配置的信任儲存庫可能不信任 Monitoring & Analytics 服務所使用的憑證。
{: tsCauses}

修改配置，以搭配使用 JVM 的信任儲存庫與其中一個後面的選項。進行變更之後，請務必重新編譯打包應用程式。
{: tsResolve}

#### 更新 Liberty 的 server.xml

更新 server.xml，以使用 JVM 的 cacerts 檔案作為信任儲存庫。將下列內容新增至 server.xml：

        <ssl id="defaultSSLConfig" trustStoreRef="defaultTrustStore"/>
        <keyStore id="defaultTrustStoretore" location="${java.home}/lib/security/cacerts"/>
        {: codeblock}

#### 更新已配置的信任儲存庫

修改已配置的信任儲存庫，以信任「DigitCert 主要 CA」。
  1. 從 https://www.digicert.com/CACerts/DigiCertGlobalRootCA.crt 下載「DigiCert 主要 CA」。
  2. 假設使用 resources/security/key.jks 作為信任儲存庫，請使用 Java 的 keytool 公用程式將 CA 匯入至金鑰：

            keytool -importcert --storepass <keyStorePassword> -keystore &lt;path&gt;/resources/security/key.jks -file DigiCertGlobalRootCA.crt
            {: codeblock}


## SDK for Node.js
{: #ts_nodejs}

### 應用程式無法啟動：一般疑難排解

若為 {{site.data.keyword.runtime_nodejs_notm}} 建置套件 3.23 版或更新版本，請嘗試將您的相依關係供應為第一個疑難排解步驟。供應相依關係意思是您將相依關係包裝在與應用程式相同的原始檔中。這樣能夠解決在相依關係假設它們在與應用程式相同基礎目錄中時可能發生的各種錯誤。

1. 從您的應用程式根目錄，執行下列指令來安裝相依關係。

   ```bash
   npm install
   ```
   {: codeblock}
1. 確定您的 `.cfignore` 檔不包含以下這行：

   ```
   node_modules/
   ```

現在，當您使用 `ibmcloud cf push` 指令部署應用程式時，相依關係會複製到與其餘應用程式相同的目錄中，而不是下載到個別的位置。

### 應用程式無法啟動，錯誤為「裝置上沒有可用的空間」
{: #no_space_left_on_device}


Node.js 應用程式無法啟動，錯誤為「裝置上沒有可用的空間」。例如，日誌中的錯誤可能類似下列內容：
{: tsSymptoms}

```
   2017-01-16T14:25:14.61-0500 [CELL/0]     ERR tar: ./app/node_modules/pm2/node_modules/cron/node_modules/moment-timezone/LICENSE: Cannot write: No space left on device

```
{: codeblock}

下載相依關係時，使用第 3 版之前 NPM 版本的 Nodejs 應用程式會耗用更多空間。
{: tsCauses}

修改應用程式的 package.json 檔案，以使用 NPM 第 3 版或更高版本。
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

### 應用程式因記憶體限制而重新啟動 
{: #oom}

Node.js 不知道應用程式可以使用多少記憶體，因此記憶體回收器在記憶體耗盡之前可能未執行。

```
2017-09-01T11:00:42.19-0400 [APP/PROC/WEB/0]OUT Exit status 137
2017-09-01T11:00:42.23-0400 [CELL/0]     OUT Exit status 0
2017-09-01T11:00:42.27-0400 [CELL/0]     OUT Destroying container
2017-09-01T11:00:42.34-0400 [API/0]      OUT Process has crashed with type: "web"
2017-09-01T11:00:42.36-0400 [API/0]      OUT App instance exited with guid eecfba3b-430c-4a6b-b71f-ac72816fe152 payload: {"instance"=>"77dbb981-16d0-3a05-3235-9a4b", "index"=>0, "reason"=>"CRASHED", "exit_description"=>"2 error(s) occurred:\n\n* 2 error(s) occurred:\n\n* Exited with status 137 (out of memory)\n* cancelled\n* cancelled", "crash_count"=>1, "crash_timestamp"=>1504278042244633291, "version"=>"6497b5b5-67d4-4c5a-b1af-362e522a029d"}
2017-09-01T11:00:43.35-0400 [CELL/0]     OUT Successfully destroyed container
```
{: codeblock}

可能的解決方案是在 package.json 檔案的應用程式 start 指令上設定 `--max_old_space_size` 選項。此選項代表應用程式記憶體覆蓋區的一部分，而且應該設定為小於應用程式可用總記憶體的值。請閱讀 [Large memory spikes and Heroku](https://github.com/nodejs/node/issues/3370)，瞭解本主題的更深入討論。
```
  "scripts": {
    "start": "node --max_old_space_size=800 server.js"
  }
```
{: codeblock}

## ASP.NET Core
{: #ts_dotnet}

應用程式無法部署，訊息為：`API/0App instance exited ... payload: {... "reason"=>"CRASHED", "exit_status"=>-1, ...}`。
{: tsSymptoms}

如果您在推送 ASP.net 應用程式時收到類似的訊息，很可能是因為您的應用程式超出記憶體或磁碟配額限制。為您的應用程式，增加記憶體或磁碟空間的配額。
{: tsCauses}

應用程式無法部署，訊息為：`Failed to compress droplet: signal: broken pipe` 或 `No space left on device`。如何修正這個問題？
{: tsSymptoms}

若已啟用 NuGet 套件快取，則從包含大量 NuGet 套件相依關係的原始碼推送的專案有時可能會導致此錯誤。將 `CACHE_NUGET_PACKAGES` 環境變數設為 `false`，以停用快取。如需相關資訊，請參閱如何[停用 NuGet 套件](/docs/runtimes/dotnet/disablingNuGet.md)的指示。
{: tsCauses}

### 有用的鏈結
* [NuGet](https://docs.nuget.org/Consume/Overview){: new_window}
* [ASP.NET Core 概觀](http://docs.asp.net/en/latest/conceptual-overview/aspnet.html){: new_window}

## PHP
{: #ts_PHP}

### PHP 建置套件的 NOTICE 訊息
{: #ts_phplog}

您可能會在日誌中看到包含 NOTICE 的訊息。您可以變更記載層次，以停止記載這些訊息。

當您使用 PHP 建置套件將應用程式推送至 {{site.data.keyword.Bluemix_notm}} 時，可能會看到包含 `NOTICE` 的訊息：
{: tsSymptoms}

```
• 2015-01-26T15:00:59.70+0100 [App/0] ERR [26-Jan-2015 14:00:59] NOTICE: [pool www] 'user' directive is ignored when FPM is not running as root
• 2015-01-26T15:01:00.63+0100 [App/0] ERR [26-Jan-2015 14:00:59] NOTICE: [pool www] 'user' directive is ignored when FPM is not running as root
• 2015-01-26T15:01:00.63+0100 [App/0] ERR [26-Jan-2015 14:00:59] NOTICE: fpm is running, pid 93
• 2015-01-26T15:01:00.63+0100 [App/0] ERR [26-Jan-2015 14:00:59] NOTICE: ready to handle connections
```
在 PHP 建置套件中，error_log 參數會定義記載層次。依預設，`error_log` 參數的值為 **stderr notice**。下列範例顯示 Cloud Foundry 提供之 PHP 建置套件的 `nginx-defaults.conf` 檔案中的預設記載層次配置。如需相關資訊，請參閱 [cloudfoundry/php-buildpack ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/cloudfoundry/php-buildpack/blob/ff71ea41d00c1226d339e83cf2c7d6dda6c590ef/defaults/config/nginx/1.5.x/nginx-defaults.conf){: new_window}。
{: tsCauses}

```
daemon off;
error_log stderr notice;
pid @{HOME}/nginx/logs/nginx.pid;
```

`NOTICE` 訊息僅提供資訊，不一定表示有問題。若要停止記載這些訊息，您可以在建置套件的 nginx-defaults.conf 檔案中，將記載層次從 `stderr notice` 變更為 `stderr error`。例如：
{: tsResolve}

```
daemon off;
error_log stderr error;
pid @{HOME}/nginx/logs/nginx.pid;
```
如需如何變更預設記載配置的相關資訊，請參閱 [error_log ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](http://nginx.org/en/docs/ngx_core_module.html#error_log){: new_window}。
	




## Python
{: #ts_python}

### 無法將協力廠商的 Python 檔案庫匯入 {{site.data.keyword.Bluemix_notm}}
{: #ts_importpylib}

您可能無法將協力廠商 Python 程式庫匯入到 {{site.data.keyword.Bluemix_notm}}。若要解決此問題，請在 Python 應用程式的根目錄中新增配置檔。

當您嘗試匯入協力廠商 Python 程式庫（例如 `web.py` 程式庫）時，`ibmcloud cf push` 指令會失敗。
{: tsSymptoms}

Python 應用程式的配置資訊遺失。
{: tsCauses}

在 Python 應用程式的根目錄中，新增 `requirements.txt` 檔案和 `Procfile` 檔案。下列資訊是假設您要匯入 `web.py` 檔案庫：
{: tsResolve}

 1. 在 Python 應用程式的根目錄中，新增 `requirements.txt` 檔案。

 `requirements.txt` 檔案可指定 Python 應用程式所需的檔案庫套件，以及套件的版本。下列範例顯示 `requirements.txt` 檔案的內容，其中 `web.py==0.37` 表示將要下載的 `web.py` 程式庫版本為 0.37，而 `wsgiref==0.1.2` 表示 web.py 程式庫所需的 Web 伺服器閘道介面版本為 0.1.2。
	 ```
	 web.py==0.37
     wsgiref==0.1.2
	 ```
	 如需如何配置 `requirements.txt` 檔案的相關資訊，請參閱[需求檔案](https://pip.readthedocs.org/en/1.1/requirements.html)。


 2. 在 Python 應用程式的根目錄中，新增 `Procfile` 檔案。`Procfile` 檔案必須包含 Python 應用程式的啟動指令。在下列指令中，*yourappname* 是 Python 應用程式的名稱，而 *PORT* 是 Python 應用程式必須用來接收應用程式使用者要求的埠號。*$PORT* 是選用項目。如果您未在啟動指令中指定 PORT，則會使用應用程式內 `VCAP_APP_PORT` 環境變數下的埠號。
	```
	web: python <yourappname>.py $PORT
	```

您現在可以將協力廠商的 Python 檔案庫匯入 {{site.data.keyword.Bluemix_notm}}。
