---

copyright:
  years: 2017, 2018
lastupdated: "2018-09-21"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}

# Available Buildpacks
{: #available_buildpacks}

* [Liberty for Java](/docs/runtimes/liberty/getting-started.html)
* [SDK for Node.js](/docs/runtimes/nodejs/getting-started.html)
* [ASP.NET core](/docs/runtimes/dotnet/getting-started.html)
* [Runtime for Swift](/docs/runtimes/swift/getting-started.html)
* [XPages](/docs/starters/xpages/index.html)
* [Go](/docs/runtimes/go/getting-started.html)
* [PHP](/docs/runtimes/php/getting-started.html)
* [Python](/docs/runtimes/python/getting-started.html)
* [Ruby](/docs/runtimes/ruby/getting-started.html)
* [Tomcat](/docs/runtimes/tomcat/getting-started.html)

## About the Buildpacks
{: #about_buildpacks}

Cloud Foundry buildpacks provide the runtime support for applications in the Cloud Foundry environment. When you deploy an application to {{site.data.keyword.cloud}}, it starts a buildpack that supports your application type. {{site.data.keyword.cloud_notm}} provides Cloud Foundry buildpack support for Java EE, Node.js, ASP.Net, Swift, and other application types.
You can use the buildpacks included with {{site.data.keyword.cloud_notm}} to deploy applications and bind them to services.

*  Cloud Foundry

    Cloud Foundry is an open source platform for application lifecycle automation.  {{site.data.keyword.Bluemix}} is built on top of the Cloud Foundry platform as a service. Check out the [Cloud Foundry documentation](https://www.cloudfoundry.org/learn/) to learn more.

*  Cloud Foundry Application

   A cloud foundry application or app, is any application intended to be instantiated by on one of the buildpacks provided by {{site.data.keyword.Bluemix_notm}}.

*  Buildpack

   A buildpack is a typically language specific package of software provided by {{site.data.keyword.Bluemix_notm}}. When an application is deployed to {{site.data.keyword.Bluemix_notm}} a buildpack appropriate for the application is chosen. The buildpack provisions the runtime environment for the application.  You can see the set of buildpacks provided by {{site.data.keyword.Bluemix_notm}} by issuing the `ibmcloud cf buildpacks` command from the command line.

*  Runtime

   A runtime is the set of software components which provide the exectuion environment for an application.  The terms *runtime* and *buildpack* are sometimes used interchangeably.  When a buildpack has completed deploying an application the runtime environment is established.

*  Boilerplate

   A boilerplate is a simple application designed for a particular runtime.  The boilerplates provide templates or samples of the language and runtime specific application types.  You can use a boilerplate as starter code to begin development of more sophisticated applications.  {{site.data.keyword.Bluemix_notm}} provides:
   * *Hello world boilerplates*: simple applications typically providing a language and runtime specific 'hello world' application.
   * *Boilerplates with services*: demonstrate how to use a runtime with a service.
   * *Boilerplates with frameworks*: simple language and runtime specific applications that take advantage of a particulary language framework such as Python Flask or Ruby Sinatra.

*  Service

   A service is a {{site.data.keyword.Bluemix_notm}} provided facility which can be coupled with an application.
