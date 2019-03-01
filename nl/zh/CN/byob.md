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

# 使用社区 buildpack
{: #using_buildpacks}

如果在 {{site.data.keyword.Bluemix}}“目录”中找不到提供所需运行时的入门模板，那么可以将外部 buildpack 添加到 {{site.data.keyword.Bluemix_notm}} 中。使用 `ibmcloud cf push` 命令部署应用程序时，可以指定与 Cloud Foundry 兼容的定制 buildpack。
{:shortdesc}

外部 buildpack 由 Cloud Foundry 社区提供，可用作您自己的 buildpack。在将应用程序部署到 {{site.data.keyword.Bluemix_notm}} 之前，请确保安装了 {{site.data.keyword.Bluemix_notm}} 命令行界面。

**注：**IBM 不提供外部 buildpack。请联系 Cloud Foundry 社区来获取支持。

## 内置社区 buildpack

在 {{site.data.keyword.Bluemix_notm}} 中，您可以使用 Cloud Foundry 社区提供的内置 buildpack。要查看内置的社区 buildpack，请运行 `ibmcloud cf buildpacks` 命令：

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


对于同一运行时或框架，IBM 创建的 buildpack 优先于社区 buildpack。如果想要使用社区 buildpack 覆盖 IBM 创建的 buildpack，必须使用带 `-b` 选项的 `ibmcloud cf push` 命令来指定该 buildpack。

例如，要对 Java™ Web 应用程序使用社区 buildpack，可以运行以下命令。

```
ibmcloud cf push app_name -b java_buildpack -p app_path
```
{:pre}

另外，要对 Node.js 应用程序使用社区 buildpack，可以运行以下命令。

```
ibmcloud cf push app_name -b nodejs_buildpack -p app_path
```
{: codeblock}

对于 IBM 创建的 buildpack 不支持但内置社区 buildpack 支持的运行时或框架，不必使用带 `-b` 选项的 `ibmcloud cf push` 命令。例如，对于 Ruby 应用程序，没有 IBM 创建的 buildpack。可以通过输入以下命令使用内置的社区 buildpack。

```
ibmcloud cf push app_name -p app_path
```
{: codeblock}

## 外部 buildpack

您可以在 {{site.data.keyword.Bluemix_notm}} 中使用外部或定制 buildpack。在 `ibmcloud cf push` 命令中，必须使用 `-b` 选项来指定 buildpack 的 URL，使用 `-s` 选项来指定堆栈。例如，要对静态文件使用外部的社区 buildpack，请运行以下命令。

```
ibmcloud cf push app_name -p app_path -b https://github.com/cloudfoundry/staticfile-buildpack.git
```
{: codeblock}

如果不想对 Ruby 应用程序使用内置的社区 buildpack，那么可以通过输入以下命令使用外部 buildpack。

```
ibmcloud cf push app_name -p app_path -b https://github.com/cloudfoundry/ruby-buildpack.git
```
{: codeblock}

您还可以将定制 buildpack 用于自己的应用程序。例如，可以使用 Cloud Foundry 社区提供的开放式源代码 PHP buildpack。在将 PHP 应用程序部署到 {{site.data.keyword.Bluemix_notm}} 时，请输入以下命令来指定 buildpack 的 Git 存储库 URL。

```
ibmcloud cf push app_name -p app_path -b https://github.com/cloudfoundry/php-buildpack.git
```
{:pre}

此外，还可以编辑项目中的 `manifest.yml` 文件来添加 `buildpack` 行。

```
buildpack: https://github.com/cloudfoundry/python-buildpack.git
```
{: codeblock}


## 指定 Java buildpack 版本

要指定 Java buildpack 版本，请使用 `ibmcloud cf set-env` 命令。例如，输入以下命令，将 Java 版本设置为 1.7.0。

```
ibmcloud cf set-env app_name JBP_CONFIG_OPEN_JDK_JRE &apos;{jre: { version: 1.7.0_+ }}&apos;
```
{: codeblock}

然后，重新编译打包应用程序以使更改生效。

```
ibmcloud cf restage app_name
```
{: codeblock}

## 使用 `manifest.yml` 文件

您可以将想要指定的环境变量和值直接添加到 `manifest.yml` 文件中。请参阅[环境变量](https://docs.cloudfoundry.org/devguide/deploy-apps/manifest.html#env-block)。
