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

# Using community buildpacks
{: #using_buildpacks}

If you can't find a starter in the {{site.data.keyword.Bluemix}} catalog that provides the runtime you want, you can bring an external buildpack to {{site.data.keyword.Bluemix_notm}}. You can specify a custom, Cloud Foundry-compatible buildpack when you deploy your application by using the `ibmcloud cf push` command.
{:shortdesc}

External buildpacks are provided by the Cloud Foundry community for you to use as your own buildpacks. Before you deploy your application to {{site.data.keyword.Bluemix_notm}}, make sure that you install the {{site.data.keyword.Bluemix_notm}} command line interface.

**Note:** External buildpacks are not provided by IBM. Contact the Cloud Foundry community for support.

## Built-in community buildpacks

In {{site.data.keyword.Bluemix_notm}}, you can use built-in buildpacks that are provided by the Cloud Foundry community. To see built-in community buildpacks, run the `ibmcloud cf buildpacks` command:

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


For the same runtime or framework, IBM-created buildpacks take precedence over the community ones. If you want to use the community buildpack to overwrite the IBM-created buildpack, you must specify the buildpack by using the `-b` option with the `ibmcloud cf push` command.

For example, you can use the community buildpack for Java™ web applications by using the following command.

```
ibmcloud cf push app_name -b java_buildpack -p app_path
```
{:pre}

You can also use the community buildpack for Node.js application with the following command.

```
ibmcloud cf push app_name -b nodejs_buildpack -p app_path
```
{: codeblock}

For a runtime or framework that is not supported by IBM-created buildpacks but is supported by built-in community buildpacks, you do not have to use the `-b` option with the `ibmcloud cf push` command. For example, for Ruby applications, there are no IBM-created buildpacks. You can use the built-in community buildpack by entering the following command.

```
ibmcloud cf push app_name -p app_path
```
{: codeblock}

## External buildpacks

You can use external or custom buildpacks in {{site.data.keyword.Bluemix_notm}}. You must specify the URL of the buildpack with the `-b` option, and specify the stack with the `-s` option on the `ibmcloud cf push` command. For example, to use an external community buildpack for static files, run the following command.

```
ibmcloud cf push app_name -p app_path -b https://github.com/cloudfoundry/staticfile-buildpack.git
```
{: codeblock}

If you don't want to use the built-in community buildpack for Ruby applications, you can use an external buildpack by entering the following command.

```
ibmcloud cf push app_name -p app_path -b https://github.com/cloudfoundry/ruby-buildpack.git
```
{: codeblock}

You can also use a custom buildpack for your application. For example, you can use an open source PHP buildpack that is provided by the Cloud Foundry community. When you deploy your PHP application to {{site.data.keyword.Bluemix_notm}}, enter the following command to specify the Git repository URL of the buildpack.

```
ibmcloud cf push app_name -p app_path -b https://github.com/cloudfoundry/php-buildpack.git
```
{:pre}

You can also edit the `manifest.yml` file in your project to add a `buildpack` line.

```
buildpack: https://github.com/cloudfoundry/python-buildpack.git
```
{: codeblock}


## Specifying the Java buildpack version

To specify a Java buildpack version, use the `ibmcloud cf set-env` command. For example, enter the following command to set the Java version to 1.7.0.

```
ibmcloud cf set-env app_name JBP_CONFIG_OPEN_JDK_JRE &apos;{jre: { version: 1.7.0_+ }}&apos;
```
{: codeblock}

Then, restage your applicationto make the change effective.

```
ibmcloud cf restage app_name
```
{: codeblock}

## Use the `manifest.yml` file.

You can add the environment variable and the value that you want to specify directly to the `manifest.yml` file. See [Environment variables](https://docs.cloudfoundry.org/devguide/deploy-apps/manifest.html#env-block).
