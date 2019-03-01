---

copyright:
  years: 2015, 2018
lastupdated: "2018-10-09"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}

# Liberty アプリケーションおよび Node.js アプリケーションの管理
{: #app_management}


「アプリ管理」は、開発用およびデバッグ用のユーティリティーの集合であり、{{site.data.keyword.Bluemix}} 上の Liberty アプリケーション用に使用可能にすることができます。
{:shortdesc}

## 「アプリ管理」ユーティリティー
{: #Utilities}

### Liberty 用のユーティリティー
* [proxy](#proxy)
* [noproxy](#noproxy)
* [devconsole](#devconsole)
* [hc](#hc)
* [shell](#shell)
* [debug](#debug)
* [jmx](#jmx)
* [localjmx](#localjmx)

### Node.js ユーティリティー (非推奨)

**非推奨についての注記:** 「アプリ管理」ユーティリティーは Node.js アプリケーションには非推奨です。 Node.JS アプリケーションと Liberty アプリケーションの両方に使用可能だったいくつかのユーティリティーは、Liberty アプリケーション用にのみ使用可能になりました。 以下のユーティリティーは Node.js アプリケーションには非推奨です。

* [proxy](#proxy)
* [noproxy](#noproxy)
* [devconsole](#devconsole)
* [hc](#hc)
* [shell](#shell)
* [inspector](#inspector)
* [trace](#trace)

## 「アプリ管理」の構成方法
{: #configure}

「アプリ管理」ユーティリティーを使用可能にするには、*BLUEMIX_APP_MGMT_ENABLE* 環境変数の値に、使用可能にするユーティリティーまたはそのリストを設定し、アプリケーションを再ステージングします。 複数のユーティリティーを使用可能にするには、**+** で区切ります。

例えば、*hc*、*debug*、*trace* のユーティリティーを使用可能にするには、以下のコマンドを実行します。

```
ibmcloud cf set-env myApp BLUEMIX_APP_MGMT_ENABLE hc+debug+trace
```
{: codeblock}

環境変数を設定したら、その後アプリケーションを再ステージングしてください。

```
ibmcloud cf restage myApp
```
{: codeblock}

アプリケーションとともに「アプリ管理」ユーティリティーがインストールされることのないようにする場合は、*BLUEMIX_APP_MGMT_INSTALL* 環境変数を「false」に設定して、アプリケーションを再ステージングします。

例えば、以下のコマンドを実行すると、「アプリ管理」ユーティリティーなしでアプリケーションをステージングします。

```
ibmcloud cf set-env myApp BLUEMIX_APP_MGMT_INSTALL false
ibmcloud cf restage myApp
```
{: codeblock}

## 制約事項
{: #restrictions}
* 「アプリ管理」を使用してアプリケーションに対して行った変更は一時的なものであり、このモードを終了すると失われます。 このモードは、一時的に開発目的のみで使用され、パフォーマンス上の理由から、実稼働環境として使用することは意図されていません。
* Node.js アプリケーションの場合、ほとんどの「アプリ管理」ユーティリティーは、**start** コマンドが `manifest.yml` ファイルで設定されているか、コマンド・ラインの `-c` オプションで設定されていると機能しません。 このような手法はビルドパックのオーバーライドであり、Node.js アプリケーションの開始に関するアンチパターンです。 最良の結果を得るには、**開始**コマンドは `package.json` ファイルか `Procfile` に設定してください。


### Liberty 用のユーティリティー
{: #liberty_utilities}

#### jmx
{: #jmx}

*jmx* ユーティリティーは、JMX REST Connector を使用可能にして、リモート JMX クライアントが {{site.data.keyword.Bluemix_notm}} ユーザー資格情報を使用してアプリケーションを管理できるようにします。

JMX を使用してアプリケーションの複数インスタンスをモニター可能ですが、それには、インスタンスごとに別個の JMX 接続が必要です。 デフォルトでは、インスタンス 0 をモニターします。インスタンス 1 をモニターするには、以下のコード・スニペットを使用できます。

```
ibmcloud cf ssh -i 1 -N -T -L 5000:127.0.0.1:5001
```
{: codeblock}

JMX コネクターの構成について詳しくは、[Configuring secure JMX connection to the Liberty profile ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www-01.ibm.com/support/knowledgecenter/was_beta_liberty/com.ibm.websphere.wlp.nd.multiplatform.doc/ae/twlp_admin_restconnector.html){:new_window} を参照してください。

**重要**: *jmx* ユーティリティーは *proxy* を開始しません。

#### localjmx
{: #localjmx}

*localjmx* ユーティリティーは、[localConnector-1.0 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](http://www.ibm.com/support/knowledgecenter/SSEQTP_liberty/com.ibm.websphere.wlp.doc/ae/rwlp_feature_localConnector-1.0.html){:new_window} Liberty フィーチャーを有効にします。 *localjmx* は、ローカル・ポート転送と組み合わせることで、リモート JMX クライアントによるアプリケーションの管理を可能にする代替方法として機能します。


**始める前に**: *localjmx* は、JConsole のインストールを必要とします。

*localjmx* ユーティリティーは、Diego セルで実行中のアプリケーションにのみ適用できます。 *localjmx* を使用するには、まず最初に `ibmcloud cf ssh` コマンドを使用してポート転送を設定します。 例えば、次のように指定します。

```
ibmcloud cf ssh -N -T -L 5000:127.0.0.1:5000 <appName>
```
{: codeblock}

次に、JConsole と接続するために、**「リモート・プロセス」**を選択して `127.0.0.1:5000` を指定し、非セキュアな接続を使用します。

#### proxy (Node.js には非推奨)
{: #proxy}

*proxy* ユーティリティーでは、アプリケーションと {{site.data.keyword.Bluemix_notm}} 間の最低限のアプリケーション管理が行われます。

有効にすると、ビルドパックは、アプリケーションのランタイムとコンテナーの間に配置されたプロキシー・エージェントを開始します。  *proxy* ユーティリティーは、アプリケーションが受信したすべての要求を処理します。 要求のタイプに基づいて、「アプリ管理」アクションを実行するか、要求をアプリケーションに転送します。 *proxy* を使用することにより、アプリケーションが異常終了しても、アプリケーション・コンテナーは機能し続けます。 また、プロキシー・エージェントにより、増分ファイル更新が可能になり、Node.js アプリケーションに*ライブ編集* モードを使用できます。

一部の「アプリ管理」ユーティリティーでは、アプリケーションと一緒に *proxy* ユーティリティーを使用する必要があり、*proxy* を自動的に開始することができます。

**非推奨についての注記:** Node.js 用には、{{site.data.keyword.Bluemix_notm}} 版の Cloud Foundry が Diego セルで実行されます。 *noproxy* フィーチャーは非 Diego セル用です。

#### noproxy (Node.js には非推奨)
{: #noproxy}

*noproxy* ユーティリティーは、*proxy* ユーティリティーが別のユーティリティーによって自動的に開始された場合に、それを使用不可にします。  Diego はアプリケーションに直接 *ssh* 接続し、ポート転送をセットアップする機能を備えているため、Diego ではプロキシーは必要ありません。

*noproxy* ユーティリティーは、Diego セルで実行されるアプリケーションにのみ適用されます。

**非推奨についての注記:** *noproxy* ユーティリティーは *proxy* を無効にします。 *proxy* は Node.js には非推奨であり、もう機能しなくなったため、*noproxy* は必要ありません。

#### devconsole (Node.js には非推奨)
{: #devconsole}

ユーザーは、(*devconsole*) 開発コンソール・ユーティリティーで、アプリケーションの再始動、停止、または中断を行えます。 また、ユーザーは、*devconsole* を使用して shell および inspector ユーティリティーを使用可能にしたり、それらにアクセスしたりすることもできます。  *devconsole* へのアクセスには、以下の URL を使用できます。
```
  https://<yourappname>.mybluemix.net/bluemix-debug/manage
```
{: codeblock}

Node バージョン 6.3.0 以上では、開発コンソールで、アプリケーションの再始動ボタンが用意されており、また *shell* ユーティリティーにアクセスできます。  詳しくは、*inspector* の説明を参照してください。

**重要**: *devconsole* ユーティリティーは *proxy* を開始します。

**非推奨についての注記:** Node.js 用に *devconsole* ユーティリティーを使用する代わりに、{{site.data.keyword.Bluemix_notm}} コンソールを使用してください。 コンソールでアプリケーションの**「ランタイム」**ページに移動します。 **「ランタイム」**ページで、アプリケーションの停止、開始、名前変更、および削除を行うことができます。 また、シェルおよびその他の情報にアクセスすることもできます。

#### hc (Node.js には非推奨)
{: #hc}

(*hc*) ヘルス・センター・エージェントにより、アプリケーションをヘルス・センター・クライアントでモニターできます。  Node.js の場合、*hc* エージェントは、IBM SDK for Node.js ビルドパックに含まれる Node.js ランタイム・バージョンでのみ使用可能です。  現行のランタイム・セットについては、[sdk-for-nodejs ビルドパックの最新更新](/docs/runtimes/nodejs/updates.html)を参照してください。

ヘルス・センター・エージェントを使用可能にしたら、IBM Monitoring and Diagnostic Tools を使用して Liberty および Node.js のアプリケーションのパフォーマンスを分析できます。 詳細情報については、[How to analyze the performance of Liberty Java or Node.js apps in {{site.data.keyword.Bluemix_notm}} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://developer.ibm.com/bluemix/2015/07/03/how-to-analyze-performance-in-bluemix/){:new_window} を参照してください。

**重要:** *hc* ユーティリティーは *proxy* を開始します。

***noproxy* と併せた *hc* の使用**

*hc* ユーティリティーは *noproxy* と組み合わせて使用できます。 ヘルス・センターを *noproxy* と一緒に使用するには、まず最初に `ibmcloud cf ssh` コマンドを使用してポート転送を設定します。 例えば、次のように指定します。

```
ibmcloud cf ssh -N -T -L 1883:127.0.0.1:1883 <appName>
```
{: codeblock}

次に、ヘルス・センター・クライアントと接続するため、[MQTT 接続 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](http://www.ibm.com/support/knowledgecenter/SS3KLZ/com.ibm.java.diagnostics.healthcenter.doc/topics/connectingtojvm.html){: new_window} を使用し、ホストを `127.0.0.1`、ポートを `1883` に指定します。

**非推奨についての注記:** Node.js 用にヘルス・センター・エージェントを使用する代わりに、{{site.data.keyword.Bluemix_notm}} コンソールを使用してください。 コンソールでアプリケーションの**「ランタイム」**ページに移動し、このページで、CPU 使用量、メモリー使用量、およびディスク・スペースにアクセスできます。 このページには、どの環境変数にもアクセスできるタブもあります。

#### shell (Node.js には非推奨)
{: #shell}

*shell* ユーティリティーは、Web ベースのシェルを使用可能にします。  *shell* へのアクセスは、*devconsole* ユーティリティーから、または以下の URL を使用して行えます。

```
  https://<yourappname>.mybluemix.net/bluemix-debug/shell
```
{: codeblock}

*shell* ユーティリティーにアクセスすると、アプリケーションに shell アクセスした端末ウィンドウが表示されます。 ファイルの編集、メモリー使用量の確認、診断コマンドの実行など、通常のシェルでサポートされるすべての操作を実行できます。

**重要:** *shell* ユーティリティーは *proxy* も開始します。

Diego は `ibmcloud cf ssh` コマンドを介した対話式シェルを提供します。したがって、*shell* ユーティリティーは、DEA で実行中のアプリケーションにのみ有用です。
{: .tip}

**非推奨についての注記:** Node.js 用に `cf ssh` コマンドは引き続き機能しますが、{{site.data.keyword.Bluemix_notm}} コンソールからシェルにアクセスすることもできます。 コンソールでアプリケーションの**「ランタイム」**ページに移動すると、Web ページ内でシェルをプルできるタブがあります。

### Node.js ユーティリティー (非推奨)
{: #node_utilities}

#### inspector (非推奨)
{: #inspector}

inspector ユーティリティーを使用すると、アプリケーションを {{site.data.keyword.cloud_notm}} で実行している間に、CPU 使用プロファイル、ブレークポイント、およびデバッグ・コードを作成できます。  6.3.0 より前の Node.js バージョンの場合、inspector は、Node インスペクター・デバッガー・インターフェースを使用可能にします。  Node インスペクターについて詳しくは、[GitHub で node-inspector ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/node-inspector/node-inspector){: new_window} の README を参照してください。  Node.js バージョン 6.3.0 以上の場合、*inspector* ユーティリティーは、[V8 Inspector Integration for Node.js ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://nodejs.org/dist/latest-v6.x/docs/api/debugger.html#debugger_v8_inspector_integration_for_node_js){:new_window} を使用します。

##### バージョン 6.3.0 以降の Node.js の場合
デバッグ・モードを開始すると、*proxy* を含まない Node.js バージョンを使用している場合でも、*proxy* は自動的に使用可能になります。 6.3.0 より後の Node.js バージョンには、*proxy* が含まれません。 6.3.0 より後の Node.js バージョンと一緒に *inspector* ユーティリティーを使用する場合は、*noproxy* を使用して *proxy* を再びオフにできます。

*proxy* を使用して *inspector* インターフェースにアクセスする代わりに、Google Chrome Web ブラウザーの Developer Tools 機能を使用します。  

以下のコマンドにより、ローカル・ポート転送で URL へのアクセスを使用可能にします。
```
ibmcloud cf ssh -N -T -L 9229:127.0.0.1:9229 <appName>
```
{: codeblock}

以下のコマンドを使用して、アプリケーションの開始ログを取得します。
```
ibmcloud cf logs <appName> --recent
```
{: codeblock}

*inspector* ユーティリティーがアクティブの場合、以下のようなメッセージがログに含まれます。
```
 ... You will need a SSH tunnel for port 9229 to be able to use the Chrome DevTools to remotely debug your app
 ... Starting app with 'node --inspect=9229  app.js '
```
{: codeblock}

最新版の Google Chrome Web ブラウザーを使用して `chrome://inspect` を表示します。
この URL から、ご使用のアプリケーションが `file://home/vcap/app/app.js` などのアプリケーション・ファイルへのリンクとともにリストされます。そこで、**「inspect」**を選択して inspect インターフェースにアクセスします。

##### バージョン 6.3.0 より前の Node.js の場合
*proxy* を使用する場合、`https://myApp.mybluemix.net/bluemix-debug/inspector` で *inspector* インターフェースにアクセスできます。

*proxy* ユーティリティーを使用しない場合は、以下のコマンドでローカル・ポート転送を使用してアプリケーション URL へのアクセスを使用可能にします。

```
ibmcloud cf ssh -N -T -L 8790:127.0.0.1:8790 <appName>
```
{: codeblock}

その後、URL `http://127.0.0.1:8790` から inspector にアクセスします。

**非推奨についての注記:** セットアップを少し変更すれば、*inspector* フィーチャーを引き続き使用できます。 アプリケーション `manifest.yml` ファイル内で、start コマンドを 'node --inspect=9229 app.js' に設定します。 その後は、6.3.0 以降のバージョンの Node.js 用の手順に従うことができます。 ただし、通常はインスペクターがアクティブになるとそれがログに示されるのですが、正しく機能してはいるものの、そういったログは表示されません。


#### trace (非推奨)
{: #trace}

trace ユーティリティーは、アプリケーションが log4js、ibmbluemix、または bunyan のロギング・モジュールを使用している場合のトレース・レベルを動的に設定できるようにします。

{{site.data.keyword.cloud_notm}} Web コンソールで**「インスタンスの詳細」**ページに移動し、**「アクション」**を選択して UI を表示します。

注: サポートされる依存関係バージョンは、以下のとおりです。

* log4js: (0.6.0 から 0.6.24)
* bunyan: (1.0.0、1.0.1、1.1.0 から 1.1.3、1.2.0 から 1.2.3、1.3.0 から 1.3.5)
* ibmbluemix: (1.0.0-20140707-1250) から (1.0.0-20150409-1328)


trace ユーティリティーは、アプリケーションが `-b buildpack` オプションを使用して開始した場合は使用できません。

trace ユーティリティーは *proxy* を開始しません。
