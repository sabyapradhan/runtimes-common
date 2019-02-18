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


# Troubleshooting for runtimes
{: #runtimes}

You might experience problems when you use [{{site.data.keyword.Bluemix}} runtimes](index.html). In many cases, you can recover from these problems by following a few easy steps.
{:shortdesc}

* [General troubleshooting](#ts_all)
* [Liberty for Java](#ts_liberty)
* [SDK for Node.js](#ts_nodejs)
* [ASP.NET Core](#ts_dotnet)
* [PHP](#ts_php)
* [Python](#ts_python)

## General Troubleshooting
{: #ts_all}

### Obsolete buildpack used when an app is pushed
{: #ts_loading_bp}

You might not be able to use the latest buildpack components when you push an app. You can use buildpacks that have built-in mechanisms to prevent loading obsolete components, or you can delete the contents in your app's cache directory before you push or restage the app.

When you push or restage an app after the buildpack is updated, the latest buildpack components aren't loaded automatically. As a result, your app uses the obsolete buildpack components from the cache. Updates that were applied to the buildpack since the last time you pushed the app aren't implemented.
{: tsSymptoms}

Some buildpacks aren't configured to automatically download all updated components from the internet to ensure that you always use the latest version.
{: tsCauses}

You can use buildpacks that have built-in mechanisms to avoid loading obsolete components, for example, the following buildpacks:
{: tsResolve}

  * [Cloud Foundry Java buildpack ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/cloudfoundry/java-buildpack){: new_window}. This buildpack has a built-in mechanism to ensure that the latest version of the buildpack is used. For more information about how this mechanism works, see [extending-caches.md ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/cloudfoundry/java-buildpack/blob/master/docs/extending-caches.md){: new_window}.
  * [Cloud Foundry Node.js buildpack ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/cloudfoundry/nodejs-buildpack){: new_window}. This buildpack provides similar functionality by using environment variables. To enable the Node.js buildpack to download node modules from the internet every time, type the following command in the {{site.data.keyword.Bluemix_notm}} command line interface: 	

  ```
  set NODE_MODULES_CACHE=false
  ```

If the buildpack that you are using doesn't provide a mechanism to load the latest components automatically, you can manually delete the contents in the cache directory and push your app again. Use the following steps:

 1. Check out a branch of a null buildpack, for example, https://github.com/ryandotsmith/null-buildpack. For information about how to check out a branch, see [Git Basics - Getting a Git Repository ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://www.git-scm.com/book/en/v2/Git-Basics-Getting-a-Git-Repository){: new_window}.  
 2. Add the following line to the `null-buildpack/bin/compile` file and commit the changes. For information about how to commit changes, see [Git Basics - Recording Changes to the Repository ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://www.git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository){: new_window}.
  ```
  rm -rfv $2/*
  ```
 3. Push your app with the null buildpack that was modified to delete the cache by using the following command. After you complete this step, all contents in the cache directory of your app are deleted.
  ```
  ibmcloud cf push appname -p app_path -b <modified_null_buildpack>
  ```
 4. Push your app with the latest buildpack that you want to use by using the following command:
  ```
  ibmcloud cf push appname -p app_path -b <latest_buildpack>
  ```

### The application continues to restart
{: #ts_apprestart}

The application continues to crash and restart.
{: tsSymptoms}

The application container lifecycle, such as evacuation and shut down, can impact your application functionality.  
{: tsCauses}

Identify which step of the application container lifecycle is causing errors in application deployment. Learn more about the [Cloud Foundry application lifecycle](https://docs.cloudfoundry.org/devguide/deploy-apps/app-lifecycle.html#evacuation).
{: tsResolve}

### The Actions button on the Instance Details page is disabled (Deprecated)
  {: #ts_actionsbutton}

  The Actions button on the Instance Details page is disabled.
  {: tsSymptoms}

  This problem occurs for the following reasons:
  {: tsCauses}

   * The app isn't deployed with the embedded Liberty buildpack.
   * The app was deployed with an early version of Liberty buildpack.

  If the problem is caused by an early version of Liberty buildpack, redeploy the app in {{site.data.keyword.Bluemix_notm}}. Otherwise, you can provide the following client application log files to the support team:
  {: tsResolve}

    * logs/messages.log
    * logs/stdout.log
    * logs/stderr.log


### Credentials are required to open a trace or dump window (Deprecated)
  {: #ts_username}

  A user name and password are required to open the trace or dump window.
  {: tsSymptoms}

  This problem occurs because of the login session timeout.
  {: tsCauses}

  Enter the user name and password again.
  {: tsResolve}


### Error occurs when trace or dump operations are running (Deprecated)
  {: #ts_target}

  An error message is displayed while the trace or dump operations are running. The message indicates that a target instance for an app isn't in the running state:
  {: tsSymptoms}

  ```
  Instance 0: Trace specification is set successfully
  Instance 2: Trace specification is set successfully
  Instance 1: Target instance 1 for app abcd is not in the running state.
  Instance 3: Trace specification is set successfully
  Instance 4: Trace specification is set successfully
  ```

  This problem occurs for the following reasons:
  {: tsCauses}

    * The trace or dump management capabilities are only for app instances that are running. Trace or dump operations can't be used on app instances that are stopped, starting, or crashed.
    * The status of the app instance is changing when the trace or dump dialog is opened.

  Close the window, then open it again.
  {: tsResolve}


### Instances have different traceSpecification configurations (Deprecated)
  {: #ts_different_config}

  For different instances of one app, you might see different traceSpecification configurations.
  {: tsSymptoms}

  This behavior occurs for the following reasons:
  {: tsCauses}

    * You changed the configuration for one or more instances previously. If you change the traceSpecification configuration for one instance, the change doesn't apply to other instances of the same app. For example, your app uses log4j and you have 2 instances for this app. You can change the log level of instance 0 from info to debug, but the log level of instance 1 remains as info.

  No action is required. This behavior is expected.
  {: tsResolve}


### Disk quota exceeded
  {: #ts_diskquota}

  You might see, in your app log, that your disk quota is exceeded.

  You see the `Disk quota exceeded` error message in the log of your app.
  {: tsSymptoms}

  This problem occurs for one of the following reasons:
  {: tsCauses}

    * The dump files are generated with the running app instances, and the files use up the allocated disk quota. By default, the disk quota for one app instance is 1 GB. You can check your disk usage by clicking **Dashboard > Application > App Runtime**. The following example shows the runtime information, including disk usage, for two instances of an app:
      ```
      Instance	State	CPU	Memory Usage	Disk Usage

  	0		Running	1.0%	344.8MB/512MB	236.8MB/1GB
  	2		Running	2.3%	361.2MB/512MB	235.7MB/1GB
      ```
    * The disk quota is limited by the current organization quota.

  Use one of the following methods:
  {: tsResolve}

    * Delete dump files after they are downloaded.
    * Redeploy the app with a bigger disk quota by including the following entry in the deployment manifest:
      ```
      disk_quota: 2048
      ```

## Liberty for Java
{: #ts_liberty}

### Application fails to start accepting connections
{: #health_check_timeout}

A Liberty application fails to start with a "_Failed to start accepting connections_" error. For example, the error in the logs might look like the following:
{: tsSymptoms}

```
   2016-11-14T14:44:58.45+0000 [API/0]      OUT App instance exited with guid 21ac63eb-9595-428a-94c7-b0b02aaf77cc payload: {"cc_partition"=>"default", "droplet"=>"21ac63eb-9595-428a-94c7-b0b02aaf77cc", "version"=>"b2772438-92de-4d47-b680-ea772ac2288a", "instance"=>"f4799c8c89214bbd8067883c3ffe23e0", "index"=>0, "reason"=>"CRASHED", "exit_status"=>255, "exit_description"=>"failed to accept connections within health check timeout", "crash_timestamp"=>1479134698}
   2016-11-14T14:45:07.50+0000 [DEA/4]      ERR Instance (index 0) failed to start accepting connections
```
{: codeblock}

{{site.data.keyword.Bluemix_notm}} performs a health check on the application to see if it has successfully started. The health check tests if the application is listening on the port assigned to the application. The default timeout for this check is 60 seconds and some applications might take longer than 60 seconds to start.  There are a number of reasons why the application might take longer to start. For example, binding services such as [New Relic](/docs/runtimes/liberty/monitoring/newRelic.html) will increase the start-up time. The application might also perform initialization steps that may take a long time to finish.
{: tsCauses}

First, examine the logs for any obvious errors that might have caused the Liberty application to fail. If no obvious errors are found then try the following:
{: tsResolve}

#### Increase the health check timeout

* When deploying the application using the `ibmcloud cf push` command specify a longer application start timeout using the `-t` option. For example:

        ```
	ibmcloud cf push myApp -t 180
        ```
	{: codeblock}

* The health check timeout can also be specified in the manifest.yml file. For example:

        ---
           ...
           timeout: 180
        {: codeblock}

#### Disable the appstate feature

The appstate feature integrates with the {{site.data.keyword.Bluemix_notm}} health check process to ensure the Liberty application is fully initialized before the application can receive HTTP requests. Once the application is fully initialized the appstate feature has no more effect.  The side effect of this feature is that some applications might take longer to start up. To disable the appstate feature, set the following environment property on your application and restage the application:

```
ibmcloud cf set-env myApp JBP_CONFIG_LIBERTY "app_state: false"
```
{: codeblock}

#### Consider re-factoring the application

If your application takes a long time to initialize, you might have to re-factor the application to do lazy and/or asynchronous initialization.

#### Deploy with the "no-route" option

1. Deploy your application with "--no-route" option. This will disable the port health check. For example:

        ```
	ibmcloud cf push myApp –no-route
        ```
	{: codeblock}

2. Once the application is initialized, map a route to the application. For example:

        ibmcloud cf map-route myApp mybluemix.net
        {: codeblock}

### SSL errors with IBM's gateway
{: #ssl_handshake_failure}


The following errors are visible in the logs and the application may fail to start:
{: tsSymptoms}

```
    2016-11-03T12:32:44.82-0200 [App/0]      ERR java.security.cert.CertPathValidatorException: Certificate chaining error
    2016-11-03T12:32:44.83-0200 [App/0]      ERR [ERROR   ] CWPKI0022E: SSL HANDSHAKE FAILURE:  A signer with SubjectDN CN=*.gateway.prd.na.ca.ibmserviceengage.com, O=International Business Machines Corp., L=Armonk, ST=New York, C=US was sent from the target host.  The signer might need to be added to local trust store /home/vcap/app/wlp/usr/servers/defaultServer/resources/security/key.jks, located in SSL configuration alias defaultSSLConfig.  The extended error message from the SSL handshake exception is: PKIX path building failed: java.security.cert.CertPathBuilderException: PKIXCertPathBuilderImpl could not build a valid CertPath.; internal cause is:
    2016-11-03T12:32:44.83-0200 [App/0]      ERR java.security.cert.CertPathValidatorException: The certificate issued by CN=DigiCert Global Root CA, OU=www.digicert.com, O=DigiCert Inc, C=US is not trusted; internal cause is:
    2016-11-03T12:32:44.83-0200 [App/0]      ERR java.security.cert.CertPathValidatorException: Certificate chaining error
```
{: codeblock}

The errors can be generated when a secure service is bound to a Liberty application and the Liberty application was deployed as a server directory or packaged server that contains server.xml that configures the Liberty ssl-1.0 feature. Binding the secure service to the Liberty application causes the runtime to connect to the service over a secure connection. That secure connection is established using the default SSL settings. Since, the default SSL settings are specified in the Liberty's server.xml, the configured trust store may not trust the certificate used by the secure service.
{: tsCauses}

Modify configuration to use the JVM's trust store with one of the options that follow.  Be sure to restage your application after making the change.
{: tsResolve}

#### Update Liberty's server.xml

Update the server.xml to use JVM's cacerts file as the trust store. Add the following to your server.xml:

        <ssl id="defaultSSLConfig" trustStoreRef="defaultTrustStore"/>
        <keyStore id="defaultTrustStoretore" location="${java.home}/lib/security/cacerts"/>
        {: codeblock}

#### Update the configured trust store

Modify the configured trust store to trust the DigitCert ROOT CA.
  1. Download the DigiCert Root CA from https://www.digicert.com/CACerts/DigiCertGlobalRootCA.crt.
  2. Assuming the resources/security/key.jks is used as the trust store, import the CA into the key using the Java's keytool utility:

            keytool -importcert --storepass <keyStorePassword> -keystore &lt;path&gt;/resources/security/key.jks -file DigiCertGlobalRootCA.crt
            {: codeblock}


## SDK for Node.js
{: #ts_nodejs}

### Application fails to start: General troubleshooting

For the {{site.data.keyword.runtime_nodejs_notm}} buildpack V3.23 or later, try vendoring your dependencies as a first troubleshooting step. Vendoring dependencies means that you package the dependencies in the same source files as your application. This can resolve various errors that can occur when dependencies assume that they are in the same base directory as the application.

1. From your application root directory, install dependencies by running the following command.

   ```bash
   npm install
   ```
   {: codeblock}
1. Ensure that your `.cfignore` file does not include the following line:

   ```
   node_modules/
   ```

Now when you deploy your application with the  `ibmcloud cf push` command, instead of downloading your dependencies to a separate location, the dependencies are copied into the same directory as the rest of the application.

### Application fails to start with a "No space left on device" error
{: #no_space_left_on_device}


A Node.js application fails to start with a "No space left on device" error. For example, the error in the logs might look like the following:
{: tsSymptoms}

```
   2017-01-16T14:25:14.61-0500 [CELL/0]     ERR tar: ./app/node_modules/pm2/node_modules/cron/node_modules/moment-timezone/LICENSE: Cannot write: No space left on device

```
{: codeblock}

Node.js applications using NPM versions prior to version 3 consume more space downloading dependencies.
{: tsCauses}

Modify the package.json file for your application to use an NPM version 3 or greater.
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

### Application restarts due to memory constraints
{: #oom}

Node.js does not know how much memory is available to the application, so the garbage collector might not run before memory is exhausted.

```
2017-09-01T11:00:42.19-0400 [APP/PROC/WEB/0]OUT Exit status 137
2017-09-01T11:00:42.23-0400 [CELL/0]     OUT Exit status 0
2017-09-01T11:00:42.27-0400 [CELL/0]     OUT Destroying container
2017-09-01T11:00:42.34-0400 [API/0]      OUT Process has crashed with type: "web"
2017-09-01T11:00:42.36-0400 [API/0]      OUT App instance exited with guid eecfba3b-430c-4a6b-b71f-ac72816fe152 payload: {"instance"=>"77dbb981-16d0-3a05-3235-9a4b", "index"=>0, "reason"=>"CRASHED", "exit_description"=>"2 error(s) occurred:\n\n* 2 error(s) occurred:\n\n* Exited with status 137 (out of memory)\n* cancelled\n* cancelled", "crash_count"=>1, "crash_timestamp"=>1504278042244633291, "version"=>"6497b5b5-67d4-4c5a-b1af-362e522a029d"}
2017-09-01T11:00:43.35-0400 [CELL/0]     OUT Successfully destroyed container
```
{: codeblock}

A possible solution is to set the `--max_old_space_size` option on the application's start command in the package.json file. This option represents part of the application's memory footprint and should be set to a value less than the total memory available to the application. Read about [Large memory spikes and Heroku](https://github.com/nodejs/node/issues/3370) for a more in-depth discussion of this topic.
```
  "scripts": {
    "start": "node --max_old_space_size=800 server.js"
  }
```
{: codeblock}

## ASP.NET Core
{: #ts_dotnet}

The application fails to deploy with the message: `API/0App instance exited ... payload: {... "reason"=>"CRASHED", "exit_status"=>-1, ...}`.
{: tsSymptoms}

If you receive a similar message when you push your ASP.net application, it is most likely because your application exceeds either the memory or disk quota limits.  Increase the quotas for memory or disk space for your application.
{: tsCauses}

The application fails to deploy with the message: `Failed to compress droplet: signal: broken pipe` or `No space left on device`.  How can I fix this?
{: tsSymptoms}

Projects pushed from source code that contains a large number of NuGet package dependencies can sometimes cause this error when the NuGet package cache is enabled.  Set the `CACHE_NUGET_PACKAGES` environment variable to `false` to disable the cache. See the instructions about how to [Disable the NuGet package](/docs/runtimes/dotnet/disablingNuGet.md) for more information.
{: tsCauses}

### Helpful links
* [NuGet](https://docs.nuget.org/Consume/Overview){: new_window}
* [ASP.NET Core Overview](http://docs.asp.net/en/latest/conceptual-overview/aspnet.html){: new_window}

## PHP
{: #ts_PHP}

### NOTICE messages from the PHP buildpack
{: #ts_phplog}

You might see messages that contain NOTICE from the logs. You can stop the logging of these messages by changing the logging level.

When you push an app to {{site.data.keyword.Bluemix_notm}} by using a PHP buildpack, you might see messages that contain `NOTICE`:
{: tsSymptoms}

```
• 2015-01-26T15:00:59.70+0100 [App/0] ERR [26-Jan-2015 14:00:59] NOTICE: [pool www] 'user' directive is ignored when FPM is not running as root
• 2015-01-26T15:01:00.63+0100 [App/0] ERR [26-Jan-2015 14:00:59] NOTICE: [pool www] 'user' directive is ignored when FPM is not running as root
• 2015-01-26T15:01:00.63+0100 [App/0] ERR [26-Jan-2015 14:00:59] NOTICE: fpm is running, pid 93
• 2015-01-26T15:01:00.63+0100 [App/0] ERR [26-Jan-2015 14:00:59] NOTICE: ready to handle connections
```
In the PHP buildpack, the error_log parameter defines the logging level. By default, the value of the `error_log` parameter is **stderr notice**. The following example shows the default logging level configuration in the `nginx-defaults.conf` file of the PHP buildpack that Cloud Foundry provides. For more information, see [cloudfoundry/php-buildpack ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/cloudfoundry/php-buildpack/blob/ff71ea41d00c1226d339e83cf2c7d6dda6c590ef/defaults/config/nginx/1.5.x/nginx-defaults.conf){: new_window}.
{: tsCauses}

```
daemon off;
error_log stderr notice;
pid @{HOME}/nginx/logs/nginx.pid;
```

The `NOTICE` messages are for information and might not indicate a problem. You can stop the logging of these messages by changing the logging level from `stderr notice` to `stderr error` in the nginx-defaults.conf file of your buildpack. For example: 	
{: tsResolve}

```
daemon off;
error_log stderr error;
pid @{HOME}/nginx/logs/nginx.pid;
```
For more information about how to change the default logging configuration, see [error_log ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://nginx.org/en/docs/ngx_core_module.html#error_log){: new_window}.


## Python
{: #ts_python}

### Can't import a third-party Python library into {{site.data.keyword.Bluemix_notm}}
{: #ts_importpylib}

You might not be able to import a third-party Python library into {{site.data.keyword.Bluemix_notm}}. To resolve the problem, add configuration files to the root directory of your python app.

When you try to import a third-party Python library, such as the `web.py` library, the `ibmcloud cf push` command fails.
{: tsSymptoms}

Configuration information for the Python app is missing.
{: tsCauses}

Add a `requirements.txt` file and a `Procfile` file to the root directory of your Python app. The following information assumes that you are importing the `web.py` library:
{: tsResolve}

 1. Add a `requirements.txt` file to the root directory of your Python app.

 The `requirements.txt` file specifies the library packages that are required for your Python app and the version of the packages. The following example shows the content of the `requirements.txt` file, where `web.py==0.37` indicates the version of the `web.py` library that will be downloaded is 0.37, and `wsgiref==0.1.2` indicates the version of the web server gateway interface that is required by the web.py library is 0.1.2.
	 ```
	 web.py==0.37
     wsgiref==0.1.2
	 ```
	 For more information about how to configure the `requirements.txt` file, see [Requirements files](https://pip.readthedocs.org/en/1.1/requirements.html).

 2. Add a `Procfile` file to the root directory of your Python app.
 The `Procfile` file must contain the start command for your Python app. In the following command, *yourappname* is the name of your Python app, and *PORT* is the port number that your Python app must use to receive requests from users of the app. *$PORT* is optional. If you don't specify PORT in the start command, the port number under the `VCAP_APP_PORT` environment variable that is inside the app is used.
	```
	web: python <yourappname>.py $PORT
	```

You can now import the third-party Python library into {{site.data.keyword.Bluemix_notm}}.
