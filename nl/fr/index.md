---

copyright:
  years: 2017, 2018
lastupdated: "2018-09-21"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}

# Packs de construction disponibles
{: #available_buildpacks}

* [Liberty for Java](/docs/runtimes/liberty/getting-started.html)
* [SDK for Node.js](/docs/runtimes/nodejs/getting-started.html)
* [ASP.NET Core](/docs/runtimes/dotnet/getting-started.html)
* [Runtime for Swift](/docs/runtimes/swift/getting-started.html)
* [XPages](/docs/starters/xpages/index.html)
* [Go](/docs/runtimes/go/getting-started.html)
* [PHP](/docs/runtimes/php/getting-started.html)
* [Python](/docs/runtimes/python/getting-started.html)
* [Ruby](/docs/runtimes/ruby/getting-started.html)
* [Tomcat](/docs/runtimes/tomcat/getting-started.html)

## A propos des packs de construction
{: #about_buildpacks}

Les packs de construction Cloud Foundry fournissent le support d'exécution des applications déployées dans l'environnement Cloud Foundry. Lorsque vous déployez une application sur {{site.data.keyword.cloud}}, un pack de construction est démarré pour prendre en charge ce type d'application. {{site.data.keyword.cloud_notm}} fournit des packs de construction prenant en charge divers types d'applications, parmi lesquels Java EE, Node.js, ASP.Net et Swift.
Vous pouvez utiliser les packs de construction inclus avec {{site.data.keyword.cloud_notm}} pour déployer des applications et les lier à des services.

*  Cloud Foundry

    Cloud Foundry est une plateforme open source dédiée à l'automatisation du cycle de vie des applications.  {{site.data.keyword.Bluemix}} est construit en tant que service sur la plateforme Cloud Foundry. Jetez un oeil à la [documentation Cloud Foundry](https://www.cloudfoundry.org/learn/) pour en savoir plus.

*  Application Cloud Foundry

   Une application Cloud Foundry (souvent appelée "app" en anglais) est une application prévue pour être instanciée par l'un des packs de construction (buildpacks) fournis par {{site.data.keyword.Bluemix_notm}}.

*  Pack de construction

   Un pack de construction (buildpack en anglais) est un package, souvent spécifique d'un langage particulier, d'un logiciel fourni par {{site.data.keyword.Bluemix_notm}}. Lorsqu'une application est déployée sur {{site.data.keyword.Bluemix_notm}}, un pack de construction approprié à cette application est choisi. Le pack de construction fournit l'environnement d'exécution à l'application.  Vous pouvez afficher la liste des packs de construction fournis par {{site.data.keyword.Bluemix_notm}} en émettant la commande `ibmcloud cf buildpacks` depuis la ligne de commande.

*  Exécution

   Une exécution (en anglais, "runtime") est l'ensemble de composants logiciels qui fournissent l'environnement d'exécution d'une application.  Les termes *exécution* et *pack de construction* sont parfois utilisés de façon interchangeable.  Lorsqu'un pack de construction a fini de déployer une application, l'environnement d'exécution est établi.

*  Conteneur boilerplate

   Un conteneur boilerplate est une application simple conçue pour une exécution (runtime) particulière.  Les conteneurs boilerplate fournissent des modèles et des exemples (ou échantillons) de différents types d'applications, dans des langages et des exécutions spécifiques.  Vous pouvez les utiliser comme code de départ pour commencer à développer des applications plus sophistiquées.  {{site.data.keyword.Bluemix_notm}} fournit :
   * Des *conteneurs boilerplate Hello world*, dont chacun est une application ultra simple visant à offrir une implémentation d'application "hello world" dans un langage et une exécution spécifiques.
   * Des *conteneurs boilerplate avec services*, qui illustrent l'emploi d'une exécution avec un service.
   * Des *conteneurs boilerplate avec frameworks*, dont chacun est une application simple, conçue dans un langage et une exécution spécifiques, et tirant parti d'un framework de langage particulier tel que Python Flask ou Ruby Sinatra.

*  Service

   Un service est une fonctionnalité fournie par {{site.data.keyword.Bluemix_notm}} qui peut être couplée à une application.
