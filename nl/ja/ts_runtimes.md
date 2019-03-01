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


# ランタイムに関するトラブルシューティング
{: #runtimes}

[{{site.data.keyword.Bluemix}} ランタイム](index.html)の使用時に問題が発生することがあります。 多くの場合、いくつかの簡単なステップを実行することで、これらの問題から復旧することが可能です。
{:shortdesc}

* [一般的なトラブルシューティング](#ts_all)
* [Liberty for Java](#ts_liberty)
* [SDK for Node.js](#ts_nodejs)
* [ASP.NET Core](#ts_dotnet)
* [PHP](#ts_php)
* [Python](#ts_python)

## 一般的なトラブルシューティング
{: #ts_all}

### アプリケーションがプッシュされたときに使用できないビルドパックが使用される
{: #ts_loading_bp}

アプリケーションをプッシュしたときに、最新のビルドパック・コンポーネントを使用できない場合があります。 使用できないコンポーネントがロードされないように組み込みメカニズムを備えたビルドパックを使用するか、アプリケーションのプッシュや再ステージを行う前にアプリケーションのキャッシュ・ディレクトリー内のコンテンツを削除します。

ビルドパックが更新されてからアプリケーションをプッシュまたは再ステージすると、最新のビルドパック・コンポーネントのロードが自動的に行われません。 その結果、アプリケーションは使用できないビルドパック・コンポーネントをキャッシュから使用することになります。 アプリケーションを最後にプッシュしてからビルドパックに適用された更新は実装されていません。
{: tsSymptoms}

ビルドパックによっては、すべての更新コンポーネントをインターネットから自動的にダウンロードして、いつでも最新バージョンを使用できるように構成されていないものもあります。
{: tsCauses}

組み込みメカニズムを備えたビルドパックを使用して、使用できないコンポーネントをロードしないようにすることができます。例えば、以下のビルドパックを使用できます。
{: tsResolve}

  * [Cloud Foundry Java ビルドパック ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/cloudfoundry/java-buildpack){: new_window}。 このビルドパックには、最新バージョンのビルドパックが使用されるように組み込みメカニズムが装備されています。 このメカニズムによる処理方法について詳しくは、[extending-caches.md ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/cloudfoundry/java-buildpack/blob/master/docs/extending-caches.md){: new_window} を参照してください。
  * [Cloud Foundry Node.js ビルドパック ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/cloudfoundry/nodejs-buildpack){: new_window}。 このビルドパックは、環境変数を使用して同様の機能を提供します。 この Node.js ビルドパックを有効にして、毎回インターネットからノード・モジュールをダウンロードするには、{{site.data.keyword.Bluemix_notm}} コマンド・ライン・インターフェースに次のコマンドを入力します。 	

  ```
  set NODE_MODULES_CACHE=false
  ```

使用中のビルドパックが最新のコンポーネントを自動的にロードするメカニズムを提供していない場合は、手動でキャッシュ・ディレクトリー内のコンテンツを削除し、アプリケーションを再度プッシュします。 以下のステップを使用します。

 1. ヌル・ビルドパックのブランチ (例えば https://github.com/ryandotsmith/null-buildpack) をチェックアウトします。 ブランチをチェックアウトする方法については、[Git Basics - Getting a Git Repository ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](http://www.git-scm.com/book/en/v2/Git-Basics-Getting-a-Git-Repository){: new_window} を参照してください。  
 2. `null-buildpack/bin/compile` ファイルに以下の行を追加して変更をコミットします。 変更をコミットする方法については、[Git Basics - Recording Changes to the Repository ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](http://www.git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository){: new_window} を参照してください。
  ```
  rm -rfv $2/*
  ```
 3. 以下のコマンドを使用して、キャッシュを削除するように変更されたヌル・ビルドパックでアプリケーションをプッシュします。 このステップを完了すると、アプリケーションのキャッシュ・ディレクトリー内にあるすべてのコンテンツが削除されます。
  ```
  ibmcloud cf push appname -p app_path -b <modified_null_buildpack>
  ```
 4. 以下のコマンドを使用して、希望する最新のビルドパックでアプリケーションをプッシュします。
  ```
  ibmcloud cf push appname -p app_path -b <latest_buildpack>
  ```

### アプリケーションが再始動を続ける
{: #ts_apprestart}

アプリケーションが異常終了と再始動を続けます。
{: tsSymptoms}

アプリケーション・コンテナーのライフサイクル (退避やシャットダウンなど) は、アプリケーション機能に影響を与える可能性があります。  
{: tsCauses}

アプリケーション・コンテナー・ライフサイクルの中で、アプリケーション・デプロイメントのエラーを引き起こしているステップを特定します。 [Cloud Foundry のアプリケーション・ライフサイクル](https://docs.cloudfoundry.org/devguide/deploy-apps/app-lifecycle.html#evacuation)の詳細を参照してください。
{: tsResolve}

### 「インスタンスの詳細」ページの「アクション」ボタンが使用不可になっている (非推奨)
  {: #ts_actionsbutton}

  「インスタンスの詳細」ページの「アクション」ボタンが使用不可になっています。
  {: tsSymptoms}

  この問題は、以下の理由で発生します。
  {: tsCauses}

   * アプリケーションが組み込みの Liberty ビルドパックでデプロイされていない。
   * アプリケーションが古いバージョンの Liberty ビルドパックでデプロイされている。

  問題の原因が古いバージョンの Liberty ビルドパックである場合は、{{site.data.keyword.Bluemix_notm}} でアプリケーションを再デプロイしてください。 それ以外の場合は、以下のクライアント・アプリケーションのログ・ファイルをサポート・チームにご提供ください。
  {: tsResolve}

    * logs/messages.log
    * logs/stdout.log
    * logs/stderr.log


### トレース・ウィンドウまたはダンプ・ウィンドウを開くために資格情報が必要である (非推奨)
  {: #ts_username}

  トレース・ウィンドウまたはダンプ・ウィンドウを開くために、ユーザー名とパスワードが必要です。
  {: tsSymptoms}

  この問題は、ログイン・セッションのタイムアウトが原因で発生します。
  {: tsCauses}

  ユーザー名とパスワードを再入力してください。
  {: tsResolve}


### トレース操作またはダンプ操作の実行中にエラーが発生する (非推奨)
  {: #ts_target}

  トレース操作またはダンプ操作の実行中にエラー・メッセージが表示されます。 そのメッセージは、アプリケーションのターゲット・インスタンスが実行状態にないことを示しています。
  {: tsSymptoms}

  ```
  Instance 0: Trace specification is set successfully
  Instance 2: Trace specification is set successfully
  Instance 1: Target instance 1 for app abcd is not in the running state.
  Instance 3: Trace specification is set successfully
  Instance 4: Trace specification is set successfully
  ```

  この問題は、以下の理由で発生します。
  {: tsCauses}

    * トレースまたはダンプ管理の機能は、実行中のアプリケーション・インスタンスのみを対象としている。 停止中、開始中、または異常終了状態のアプリケーション・インスタンスには、トレースまたはダンプ操作は使用できません。
    * トレースまたはダンプ・ダイアログが開く時点でアプリケーション・インスタンスの状況が移行中である。

  ウィンドウを閉じてから、再度開いてください。
  {: tsResolve}


### インスタンスによって traceSpecification 構成が異なる (非推奨)
  {: #ts_different_config}

  1 つのアプリケーションに異なるインスタンスがある場合に、異なる traceSpecification 構成が表示されることがあります。
  {: tsSymptoms}

  この動作は、以下の理由で発生します。
  {: tsCauses}

    * 1 つ以上のインスタンスの構成を以前に変更した。 あるインスタンスの traceSpecification 構成を変更した場合、その変更は同じアプリケーションの他のインスタンスには適用されません。 例えば、アプリケーションで log4j を使用していて、このアプリケーションのインスタンスが 2 つあるものとします。 インスタンス 0 のログ・レベルを info から debug に変更できますが、インスタンス 1 のログ・レベルは info のままになります。

  アクションは不要です。 これは予期された動作です。
  {: tsResolve}


### ディスク割り当て量の超過
  {: #ts_diskquota}

  アプリケーション・ログでディスク割り当て量の超過に気付くことがあります。

  `「ディスク割り当て量の超過 (Disk quota exceeded)」`エラー・メッセージがアプリケーションのログに示されます。
  {: tsSymptoms}

  この問題は、以下のいずれかの理由で発生します。
  {: tsCauses}

    * ダンプ・ファイルは実行中のアプリケーション・インスタンスで生成され、割り振られたディスク割り当て量を使い尽くします。 デフォルトでは、1 つのアプリケーション・インスタンスあたりのディスク割り当て量は 1 GB です。 ディスク使用量を確認するには、**「ダッシュボード」>「アプリケーション」>「アプリ・ランタイム」**とクリックします。 以下の例では、アプリケーションの 2 つのインスタンスについての、ディスク使用量を含むランタイム情報を示しています。
      ```
      Instance	State	CPU	Memory Usage	Disk Usage

  	0		Running	1.0%	344.8MB/512MB	236.8MB/1GB
  	2		Running	2.3%	361.2MB/512MB	235.7MB/1GB
      ```
    * ディスク割り当て量は、現在の組織の割り当て量によって制限されます。

  以下のいずれかの方法を使用します。
  {: tsResolve}

    * ダンプ・ファイルをダウンロード後に削除します。
    * デプロイメント・マニフェストに以下の入力内容を含めて、ディスク割り当て量をもっと大きくしてアプリケーションを再デプロイします。
      ```
      disk_quota: 2048
      ```

## Liberty for Java
{: #ts_liberty}

### アプリケーションが接続を受け入れて開始できない
{: #health_check_timeout}

Liberty アプリケーションを開始できず、「_接続を受け入れて開始できませんでした (Failed to start accepting connections)_」エラーが出されます。 例えば、ログ内のエラーは、次のようになっています。
{: tsSymptoms}

```
   2016-11-14T14:44:58.45+0000 [API/0]      OUT App instance exited with guid 21ac63eb-9595-428a-94c7-b0b02aaf77cc payload: {"cc_partition"=>"default", "droplet"=>"21ac63eb-9595-428a-94c7-b0b02aaf77cc", "version"=>"b2772438-92de-4d47-b680-ea772ac2288a", "instance"=>"f4799c8c89214bbd8067883c3ffe23e0", "index"=>0, "reason"=>"CRASHED", "exit_status"=>255, "exit_description"=>"failed to accept connections within health check timeout", "crash_timestamp"=>1479134698}
   2016-11-14T14:45:07.50+0000 [DEA/4]      ERR Instance (index 0) failed to start accepting connections
```
{: codeblock}

{{site.data.keyword.Bluemix_notm}} は、アプリケーション上でヘルス・チェックを実行して、アプリケーションが正常に開始されたかどうかを確認します。 このヘルス・チェックでは、アプリケーションがそのアプリケーションに割り当てられたポート上で listen しているかどうかをテストします。 このチェックのデフォルトのタイムアウトは 60 秒ですが、アプリケーションによっては開始に 60 秒より長くかかることもあります。  アプリケーションの開始に時間がかかる理由はさまざまです。 例えば、[New Relic](/docs/runtimes/liberty/monitoring/newRelic.html) などのサービスをバインドすると、始動時間が長くなります。また、そういったアプリケーションが、終了までに時間のかかる初期化ステップを実行している可能性もあります。
{: tsCauses}

最初に、ログを調べて Liberty アプリケーションが失敗する原因になった可能性のある明らかなエラーがないか確認します。 明らかなエラーが見つからない場合は、以下を試みてください。
{: tsResolve}

#### ヘルス・チェック・タイムアウトを増やす

* `ibmcloud cf push` コマンドを使用してアプリケーションをデプロイする際に、`-t` オプションを使用して、アプリケーションの開始タイムアウトをより長い値に指定します。 例えば、以下のようにします。

        ```
	ibmcloud cf push myApp -t 180
        ```
	{: codeblock}

* ヘルス・チェックのタイムアウトは、manifest.yml ファイルに指定することもできます。 例えば、以下のようにします。

        ---
           ...
           timeout: 180
        {: codeblock}

#### appstate フィーチャーを使用不可にする

appstate フィーチャーは、{{site.data.keyword.Bluemix_notm}} ヘルス・チェック処理と統合されて、Liberty アプリケーションが HTTP 要求を受信する前に完全に初期化されるようにします。 アプリケーションの完全な初期化が完了すると、appstate フィーチャーの効力はなくなります。  このフィーチャーの副次作用は、一部のアプリケーションの始動に長い時間がかかる場合があることです。 appstate フィーチャーを使用不可にするには、アプリケーションで以下の環境プロパティーを設定し、アプリケーションを再ステージします。

```
ibmcloud cf set-env myApp JBP_CONFIG_LIBERTY "app_state: false"
```
{: codeblock}

#### アプリケーションのリファクタリングを検討する

アプリケーションの初期化に長時間かかる場合、アプリケーションをリファクタリングして、遅延または非同期 (あるいは、その両方での) 初期化を行うことが必要になる可能性があります。

#### 「no-route」オプションを指定してデプロイする

1. 「--no-route」オプションを指定してアプリケーションをデプロイします。 これにより、ポートのヘルス・チェックが使用不可になります。 例えば、以下のようにします。

        ```
	ibmcloud cf push myApp –no-route
        ```
	{: codeblock}

2. アプリケーションが初期化された時点で、ルートをアプリケーションにマップします。 例えば、以下のようにします。

        ibmcloud cf map-route myApp mybluemix.net
        {: codeblock}

### IBM のゲートウェイでの SSL エラー
{: #ssl_handshake_failure}


ログに次のようなエラーが表示され、アプリケーションの開始に失敗することがあります。
{: tsSymptoms}

```
    2016-11-03T12:32:44.82-0200 [App/0]      ERR java.security.cert.CertPathValidatorException: Certificate chaining error
    2016-11-03T12:32:44.83-0200 [App/0]      ERR [ERROR   ] CWPKI0022E: SSL HANDSHAKE FAILURE:  A signer with SubjectDN CN=*.gateway.prd.na.ca.ibmserviceengage.com, O=International Business Machines Corp., L=Armonk, ST=New York, C=US was sent from the target host.  The signer might need to be added to local trust store /home/vcap/app/wlp/usr/servers/defaultServer/resources/security/key.jks, located in SSL configuration alias defaultSSLConfig.  The extended error message from the SSL handshake exception is: PKIX path building failed: java.security.cert.CertPathBuilderException: PKIXCertPathBuilderImpl could not build a valid CertPath.; internal cause is:
    2016-11-03T12:32:44.83-0200 [App/0]      ERR java.security.cert.CertPathValidatorException: The certificate issued by CN=DigiCert Global Root CA, OU=www.digicert.com, O=DigiCert Inc, C=US is not trusted; internal cause is:
    2016-11-03T12:32:44.83-0200 [App/0]      ERR java.security.cert.CertPathValidatorException: Certificate chaining error
```
{: codeblock}

これらのエラーは、セキュア・サービスが Liberty アプリケーションにバインドされ、その Liberty アプリケーションが、Liberty ssl-1.0 フィーチャーを構成する server.xml を含むサーバー・ディレクトリーまたはパッケージされたサーバーとしてデプロイされたときに生成される可能性があります。セキュア・サービスを Liberty アプリケーションにバインドすると、ランタイムはセキュア接続を介してサービスに接続するようになります。このセキュア接続はデフォルトの SSL 設定を使用して確立されます。 デフォルトの SSL 設定は Liberty の server.xml に指定されているため、構成済みトラストストアが、セキュア・サービスによって使用されている証明書を信頼しない場合があります。
{: tsCauses}

以下のいずれかのオプションを指定して JVM のトラストストアを使用するように、構成を変更します。  変更を行った後、アプリケーションの再ステージングを忘れずに行ってください。
{: tsResolve}

#### Liberty の server.xml を更新します

server.xml を更新し、トラストストアとして JVM の cacerts ファイルを使用するようにします。 以下を server.xml に追加します。

        <ssl id="defaultSSLConfig" trustStoreRef="defaultTrustStore"/>
        <keyStore id="defaultTrustStoretore" location="${java.home}/lib/security/cacerts"/>
        {: codeblock}

#### 構成済みトラストストアを更新します

DigitCert ROOT CA を信頼するように、構成済みトラストストアを変更します。
  1. https://www.digicert.com/CACerts/DigiCertGlobalRootCA.crt から DigiCert Root CA をダウンロードします。
  2. トラストストアとして resources/security/key.jks が使用されると想定し、Java の keytool ユーティリティーを使用して CA をこの鍵にインポートします。

            keytool -importcert --storepass <keyStorePassword> -keystore &lt;path&gt;/resources/security/key.jks -file DigiCertGlobalRootCA.crt
            {: codeblock}


## SDK for Node.js
{: #ts_nodejs}

### アプリケーションが開始に失敗する: 一般的なトラブルシューティング

{{site.data.keyword.runtime_nodejs_notm}} ビルドパック V3.23 以降の場合、トラブルシューティングの最初のステップとして、依存関係をベンダリングしてみてください。 依存関係をベンダリングするとは、アプリケーションと同じソース・ファイル内に依存関係をパッケージすることです。 これによって、依存関係がアプリケーションと同じベース・ディレクトリー内にあると想定している場合に発生する可能性のあるさまざまなエラーを解決できます。

1. アプリケーションのルート・ディレクトリーから、以下のコマンドを実行して依存関係をインストールします。

   ```bash
   npm install
   ```
   {: codeblock}
1. `.cfignore` ファイルに以下の行が含まれていないことを確認します。

   ```
   node_modules/
   ```

これで、別の場所に依存関係をダウンロードする代わりに、`ibmcloud cf push` コマンドを使用してアプリケーションをデプロイすると、依存関係はアプリケーションの残りと同じディレクトリーにコピーされるようになります。

### 「No space left on device」エラーでアプリケーションが開始に失敗する
{: #no_space_left_on_device}


「No space left on device」エラーで、Node.js アプリケーションが開始に失敗します。 例えば、ログ内のエラーは、次のようになっています。
{: tsSymptoms}

```
   2017-01-16T14:25:14.61-0500 [CELL/0]     ERR tar: ./app/node_modules/pm2/node_modules/cron/node_modules/moment-timezone/LICENSE: Cannot write: No space left on device

```
{: codeblock}

NPM バージョン 3 より前のバージョンを使用している Node.js アプリケーションでは、依存関係のダウンロード時により多くのスペースを使用します。
{: tsCauses}

NPM バージョン 3 以上を使用するように、アプリケーションの package.json ファイルを変更してください。
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

### メモリーの制約が原因でアプリケーションが再始動する
{: #oom}

Node.js は、アプリケーションが使用可能なメモリー量を認識しないため、メモリーが使い尽くされる前にガーベッジ・コレクターが稼働しない可能性があります。

```
2017-09-01T11:00:42.19-0400 [APP/PROC/WEB/0]OUT Exit status 137
2017-09-01T11:00:42.23-0400 [CELL/0]     OUT Exit status 0
2017-09-01T11:00:42.27-0400 [CELL/0]     OUT Destroying container
2017-09-01T11:00:42.34-0400 [API/0]      OUT Process has crashed with type: "web"
2017-09-01T11:00:42.36-0400 [API/0]      OUT App instance exited with guid eecfba3b-430c-4a6b-b71f-ac72816fe152 payload: {"instance"=>"77dbb981-16d0-3a05-3235-9a4b", "index"=>0, "reason"=>"CRASHED", "exit_description"=>"2 error(s) occurred:\n\n* 2 error(s) occurred:\n\n* Exited with status 137 (out of memory)\n* cancelled\n* cancelled", "crash_count"=>1, "crash_timestamp"=>1504278042244633291, "version"=>"6497b5b5-67d4-4c5a-b1af-362e522a029d"}
2017-09-01T11:00:43.35-0400 [CELL/0]     OUT Successfully destroyed container
```
{: codeblock}

考えられる解決策の 1 つは、package.json ファイルでアプリケーションの開始コマンドに `--max_old_space_size` オプションを設定することです。 このオプションは、アプリケーションのメモリー占有スペースの一部を表し、アプリケーションが使用可能な合計メモリーより少ない値に設定する必要があります。 このトピックについての詳しい説明は、[Large memory spikes and Heroku](https://github.com/nodejs/node/issues/3370) をご覧ください。
```
  "scripts": {
    "start": "node --max_old_space_size=800 server.js"
  }
```
{: codeblock}

## ASP.NET Core
{: #ts_dotnet}

アプリケーションがデプロイに失敗し、メッセージ `API/0App instance exited ... payload: {... "reason"=>"CRASHED", "exit_status"=>-1, ...}` が出されます。
{: tsSymptoms}

ASP.net アプリケーションのプッシュ時にも同様のメッセージを受け取っている場合、最も可能性が高い原因は、ご使用のアプリケーションがメモリーまたはディスクの割り当て量制限を超えていることです。  アプリケーションのメモリーまたはディスク・スペースの割り当て量を増加してください。
{: tsCauses}

アプリケーションがデプロイに失敗し、「`Failed to compress droplet: signal: broken pipe`」または「`No space left on device`」というメッセージが出されます。  これを修正する方法を教えてください。
{: tsSymptoms}

多数の NuGet パッケージ依存関係が含まれているソース・コードからプッシュされるプロジェクトでは、NuGet パッケージのキャッシュが有効になっている場合に、このエラーが発生することがあります。  `CACHE_NUGET_PACKAGES` 環境変数を `false` に設定して、キャッシュを無効にしてください。 詳しくは、[NuGet パッケージの無効化](/docs/runtimes/dotnet/disablingNuGet.md)の方法についての説明を参照してください。
{: tsCauses}

### 役に立つリンク
* [NuGet](https://docs.nuget.org/Consume/Overview){: new_window}
* [ASP.NET Core Overview](http://docs.asp.net/en/latest/conceptual-overview/aspnet.html){: new_window}

## PHP
{: #ts_PHP}

### PHP ビルドパックからの NOTICE メッセージ
{: #ts_phplog}

ログからの NOTICE を含むメッセージが表示されることがあります。 このようなメッセージのロギングは、ロギング・レベルを変更すれば止めることができます。

PHP ビルドパックを使用してアプリケーションを {{site.data.keyword.Bluemix_notm}} にプッシュすると、以下のような `NOTICE` を含むメッセージが表示されることがあります。
{: tsSymptoms}

```
• 2015-01-26T15:00:59.70+0100 [App/0] ERR [26-Jan-2015 14:00:59] NOTICE: [pool www] 'user' directive is ignored when FPM is not running as root
• 2015-01-26T15:01:00.63+0100 [App/0] ERR [26-Jan-2015 14:00:59] NOTICE: [pool www] 'user' directive is ignored when FPM is not running as root
• 2015-01-26T15:01:00.63+0100 [App/0] ERR [26-Jan-2015 14:00:59] NOTICE: fpm is running, pid 93
• 2015-01-26T15:01:00.63+0100 [App/0] ERR [26-Jan-2015 14:00:59] NOTICE: ready to handle connections
```
PHP ビルドパックでは、error_log パラメーターはロギング・レベルを定義します。 デフォルトでは、`error_log` パラメーターの値は **stderr notice** です。 次の例は、Cloud Foundry が提供する PHP ビルドパックの `nginx-defaults.conf` ファイルに含まれる、デフォルトのロギング・レベル構成を示しています。 詳しくは、[cloudfoundry/php-buildpack ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/cloudfoundry/php-buildpack/blob/ff71ea41d00c1226d339e83cf2c7d6dda6c590ef/defaults/config/nginx/1.5.x/nginx-defaults.conf){: new_window} を参照してください。
{: tsCauses}

```
daemon off;
error_log stderr notice;
pid @{HOME}/nginx/logs/nginx.pid;
```

`NOTICE` メッセージは通知用であり、問題を示していない場合があります。 これらのメッセージのロギングは、ビルドパックの nginx-defaults.conf ファイルに含まれるロギング・レベルを `stderr notice` から `stderr error` に変更することで停止できます。 例えば、以下のようにします。 	
{: tsResolve}

```
daemon off;
error_log stderr error;
pid @{HOME}/nginx/logs/nginx.pid;
```
デフォルトのロギング構成を変更する方法について詳しくは、[error_log ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](http://nginx.org/en/docs/ngx_core_module.html#error_log){: new_window} を参照してください。


## Python
{: #ts_python}

### サード・パーティーの Python ライブラリーを {{site.data.keyword.Bluemix_notm}} にインポートできない
{: #ts_importpylib}

サード・パーティーの Python ライブラリーを {{site.data.keyword.Bluemix_notm}} にインポートできない場合があります。 問題を解決するには、構成ファイルを python アプリケーションのルート・ディレクトリーに追加します。

サード・パーティーの Python ライブラリー (例えば `web.py` ライブラリーなど) をインポートしようとすると、`ibmcloud cf push` コマンドが失敗します。
{: tsSymptoms}

Python アプリケーションの構成情報が欠落しています。
{: tsCauses}

`requirements.txt` ファイルと `Procfile` ファイルを Python アプリケーションのルート・ディレクトリーに追加します。 以下の情報は、`web.py` ライブラリーをインポートしていると仮定しています。
{: tsResolve}

 1. `requirements.txt` ファイルを Python アプリケーションのルート・ディレクトリーに追加します。

 `requirements.txt` ファイルには、Python アプリケーションに必要なライブラリー・パッケージとそのパッケージのバージョンが指定されています。 以下の例には、`requirements.txt` ファイルの内容が示されています。ここで、`web.py==0.37` は、ダウンロードされる `web.py` ライブラリーのバージョンが 0.37 であることを示しています。`wsgiref==0.1.2` は、web.py ライブラリーに必要な Web サーバー・ゲートウェイ・インターフェースのバージョンが 0.1.2 であることを示しています。
	 ```
	 web.py==0.37
     wsgiref==0.1.2
	 ```
	 `requirements.txt` ファイルの構成方法について詳しくは、「[Requirements files](https://pip.readthedocs.org/en/1.1/requirements.html)」を参照してください。

 2. `Procfile` ファイルを Python アプリケーションのルート・ディレクトリーに追加します。
 `Procfile` ファイルには、Python アプリケーションの開始コマンドを含めてください。 以下のコマンドでは、*yourappname* が Python アプリケーションの名前で、*PORT* は、アプリケーションのユーザーから要求を受信するために Python アプリケーションが使用しなければならないポート番号です。 *$PORT* はオプションです。 開始コマンドに PORT を指定しない場合は、アプリケーション内部にある `VCAP_APP_PORT` 環境変数下のポート番号が使用されます。
	```
	web: python <yourappname>.py $PORT
	```

これでサード・パーティーの Python ライブラリーを {{site.data.keyword.Bluemix_notm}} にインポートできます。
