---

copyright:
  years: 2017, 2018
lastupdated: "2018-09-21"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}

# Buildpacks disponíveis
{: #available_buildpacks}

* [Liberty for Java](/docs/runtimes/liberty/getting-started.html)
* [SDK for Node.js](/docs/runtimes/nodejs/getting-started.html)
* [Núcleo do ASP.NET](/docs/runtimes/dotnet/getting-started.html)
* [Runtime for Swift](/docs/runtimes/swift/getting-started.html)
* [XPages](/docs/starters/xpages/index.html)
* [Go](/docs/runtimes/go/getting-started.html)
* [PHP](/docs/runtimes/php/getting-started.html)
* [Python](/docs/runtimes/python/getting-started.html)
* [Ruby](/docs/runtimes/ruby/getting-started.html)
* [Tomcat](/docs/runtimes/tomcat/getting-started.html)

## Sobre os buildpacks
{: #about_buildpacks}

Os buildpacks do Cloud Foundry fornecem o suporte de tempo de execução para aplicativos no ambiente do Cloud Foundry. Ao
implementar um aplicativo para {{site.data.keyword.cloud}}, ele inicia um buildpack que suporta seu tipo de aplicativo. O {{site.data.keyword.cloud_notm}} fornece suporte de buildpack do Cloud Foundry para Java EE, Node.js, ASP.Net, Swift e
outros tipos de aplicativos.
É possível usar os buildpacks incluídos com o {{site.data.keyword.cloud_notm}} para implementar aplicativos e ligá-los a
serviços.

*  Cloud Foundry

    Cloud Foundry é uma plataforma de software livre para automação de ciclo de vida do aplicativo.  O {{site.data.keyword.Bluemix}}
baseia-se na plataforma do Cloud Foundry como serviço. Consulte a documentação do [Cloud Foundry](https://www.cloudfoundry.org/learn/) para saber mais.

*  Aplicativo Cloud Foundry

   Um aplicativo ou app Cloud Foundry é qualquer aplicativo que deve ser instanciado por um dos buildpacks fornecidos pelo
{{site.data.keyword.Bluemix_notm}}.

*  Buildpack

   Um buildpack é geralmente um pacote de software específico da linguagem fornecido pelo {{site.data.keyword.Bluemix_notm}}. Quando um aplicativo é implementado no {{site.data.keyword.Bluemix_notm}}, um buildpack apropriado para o aplicativo é escolhido. O buildpack provisiona o ambiente de tempo de execução para o aplicativo.  É possível ver o conjunto de buildpacks fornecidos pelo {{site.data.keyword.Bluemix_notm}} emitindo o comando `ibmcloud cf buildpacks` da linha de comandos.

*  Tempo de execução

   Um tempo de execução é o conjunto de componentes de software que fornecem o ambiente de execução para um aplicativo.  Os termos *tempo de execução* e *buildpack* são, às vezes, usados de modo intercambiável.  Quando um buildpack termina a implementação de um aplicativo, o ambiente de tempo de execução é estabelecido.

*  Modelo

   Um modelo é um aplicativo simples projetado para um tempo de execução específico.  Os modelos fornecem modelos ou amostras da linguagem e tipos de aplicativo específicos do tempo de execução.  É possível usar um modelo como código de início para começar o desenvolvimento de aplicativos mais sofisticados.  O {{site.data.keyword.Bluemix_notm}} fornece:
   * *Textos padrão Hello world*: aplicativos simples que geralmente fornecem uma linguagem e o
aplicativo 'hello world' específico de tempo de execução.
   * *Textos padrão com serviços*: demonstram como usar um tempo de execução com um serviço.
   * *Textos padrão com estruturas*: linguagem simples e aplicativos específicos do tempo de execução
que aproveitam uma estrutura de linguagem especial, como Python Flask ou Ruby Sinatra.

*  Serviço

   Um serviço é um recurso fornecido pelo {{site.data.keyword.Bluemix_notm}}
que pode ser acoplado a um aplicativo.
