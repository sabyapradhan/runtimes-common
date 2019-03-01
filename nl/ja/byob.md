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

# コミュニティー・ビルドパックの使用
{: #using_buildpacks}

使用したいランタイムを提供するスターターが {{site.data.keyword.Bluemix}} カタログにない場合、外部ビルドパックを {{site.data.keyword.Bluemix_notm}} に持ち込むことができます。 `ibmcloud cf push` コマンドを使用することで、アプリケーションのデプロイ時に Cloud Foundry 対応のカスタム・ビルドパックを指定できます。
{:shortdesc}

外部ビルドパックは、ユーザーが独自のビルドパックとして使用できるよう、Cloud Foundry コミュニティーによって提供されます。 アプリケーションを {{site.data.keyword.Bluemix_notm}} にデプロイする前に、{{site.data.keyword.Bluemix_notm}} コマンド・ライン・インターフェースを必ずインストールしてください。

**注:** 外部ビルドパックは IBM によって提供されるものではありません。 サポートについては、Cloud Foundry コミュニティーにお問い合わせください。

## 組み込みのコミュニティー・ビルドパック

{{site.data.keyword.Bluemix_notm}} では、Cloud Foundry コミュニティーによって提供される組み込みビルドパックを使用できます。 組み込みのコミュニティー・ビルドパックを表示するには、以下のように `ibmcloud cf buildpacks` コマンドを実行します。

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


同じランタイムまたはフレームワークでは、IBM 製ビルドパックがコミュニティー・ビルドパックより優先されます。 コミュニティー・ビルドパックを使用して IBM 製ビルドパックを上書きしたい場合、`ibmcloud cf push` コマンドで `-b` オプションを使用してビルドパックを指定する必要があります。

例えば、以下のコマンドを使用して、Java™ Web アプリケーションのコミュニティー・ビルドパックを使用できます。

```
ibmcloud cf push app_name -b java_buildpack -p app_path
```
{:pre}

また、以下のコマンドを使用して、Node.js アプリケーションのコミュニティー・ビルドパックを使用することもできます。

```
ibmcloud cf push app_name -b nodejs_buildpack -p app_path
```
{: codeblock}

IBM 製ビルドパックではサポートされないが組み込みコミュニティー・ビルドパックではサポートされるランタイムまたはフレームワークの場合、`ibmcloud cf push` コマンドで `-b` オプションを使用する必要はありません。 例えば、Ruby アプリケーションの場合、IBM 製のビルドパックはありません。 組み込みコミュニティー・ビルドパックは、次のコマンドを入力することによって使用できます。

```
ibmcloud cf push app_name -p app_path
```
{: codeblock}

## 外部ビルドパック

{{site.data.keyword.Bluemix_notm}} では、外部ビルドパックやカスタム・ビルドパックを使用できます。 `ibmcloud cf push` コマンドの `-b` オプションでビルドパックの URL を指定し、`-s` オプションでスタックを指定する必要があります。 例えば、静的ファイル用の外部コミュニティー・ビルドパックを使用するには、以下のコマンドを実行します。

```
ibmcloud cf push app_name -p app_path -b https://github.com/cloudfoundry/staticfile-buildpack.git
```
{: codeblock}

Ruby アプリケーションに組み込みのコミュニティー・ビルドパックを使用したくない場合は、以下のコマンドを入力して、外部ビルドパックを使用できます。

```
ibmcloud cf push app_name -p app_path -b https://github.com/cloudfoundry/ruby-buildpack.git
```
{: codeblock}

アプリケーション用にカスタム・ビルドパックを使用することもできます。 例えば、Cloud Foundry コミュニティーにより提供されるオープン・ソースの PHP ビルドパックを使用できます。 PHP アプリケーションを {{site.data.keyword.Bluemix_notm}} にデプロイする際に、次のコマンドを入力してビルドパックの Git リポジトリー URL を指定します。

```
ibmcloud cf push app_name -p app_path -b https://github.com/cloudfoundry/php-buildpack.git
```
{:pre}

プロジェクトの `manifest.yml` ファイルを編集して、`buildpack` 行を追加することもできます。

```
buildpack: https://github.com/cloudfoundry/python-buildpack.git
```
{: codeblock}


## Java ビルドパックのバージョンの指定

Java ビルドパックのバージョンを指定するには、`ibmcloud cf set-env` コマンドを使用します。 例えば、Java バージョンを 1.7.0 に設定するには、次のコマンドを入力します。

```
ibmcloud cf set-env app_name JBP_CONFIG_OPEN_JDK_JRE &apos;{jre: { version: 1.7.0_+ }}&apos;
```
{: codeblock}

その後、アプリケーションを再ステージングして、変更を有効にします。

```
ibmcloud cf restage app_name
```
{: codeblock}

## `manifest.yml` ファイルの使用

指定したい環境変数と値を直接 `manifest.yml` ファイルに追加することができます。 『[環境変数](https://docs.cloudfoundry.org/devguide/deploy-apps/manifest.html#env-block)』を参照してください。
