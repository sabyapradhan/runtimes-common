---

copyright:
  years: 2016, 2018
lastupdated: "2018-11-20"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}

# Declaração de suporte do buildpack
{: #buildpack_support_statement}


## Buildpacks integrados da IBM
{: #built-in_ibm_buildpacks}

Para o [Liberty for Java](/docs/runtimes/liberty/index.html), o
[SDK for Node.js](/docs/runtimes/nodejs/index.html) e o [ASP.NET
Core](/docs/runtimes/dotnet/index.html), a IBM suportará duas versões (n e n - 1), por exemplo, o IBM Liberty Buildpack v3.12 e IBM Liberty
Buildpack v3.11. Cada buildpack irá fornecer e suportar uma ou mais versões principais de seu tempo de execução correspondente, conforme apropriado. Os buildpacks serão atualizados normalmente uma vez por mês com a versão secundária mais recente do tempo de execução que está disponível.

Para o [{{site.data.keyword.Bluemix_notm}} Runtime for Swift](/docs/runtimes/swift/index.html), a IBM fornece suporte para o buildpack que corresponde à versão mais recente do Swift disponível em [Swift.org](http://swift.org). As atualizações para o buildpack estão em sincronia com a versão liberada mais recente disponível do Swift.

Problemas podem ser relatados com relação a qualquer versão do buildpack integrado da IBM que é atualmente suportada no {{site.data.keyword.Bluemix_notm}}, mas elas serão verificadas quanto à versão mais recente. Se um defeito for localizado, a IBM fornecerá uma correção em um nível futuro do tempo de execução e do buildpack correspondente. A IBM não fornecerá correções para versões principais e secundárias
anteriores (N-1, n-1). A IBM não fornecerá suporte para tempos de execução de comunidade mesmo ao usar buildpacks IBM, por exemplo, ao usar o Open JDK com o buildpack do Liberty. Esses tempos de execução da comunidade seguem a mesma política de suporte que os buildpacks integrados da comunidade, conforme
descrito na seção a seguir.

## Buildpacks integrados da comunidade
{: #built-in_community_buildpacks}

Os Buildpacks integrados da comunidade a seguir são fornecidos pelo Cloud Foundry Community:

* [Java](/docs/runtimes/tomcat/index.html)
* [Node.js](https://github.com/cloudfoundry/nodejs-buildpack)
* [PHP](/docs/runtimes/php/index.html)
* [Ruby](/docs/runtimes/ruby/index.html)
* [Python](/docs/runtimes/python/index.html)
* [Go](/docs/runtimes/go/index.html)

As atualizações para esses buildpacks serão realizadas quando o {{site.data.keyword.Bluemix_notm}} tiver upgrade
feito para uma nova versão do Cloud Foundry. Problemas com esses tempos de execução no {{site.data.keyword.Bluemix_notm}} podem ser relatados para a IBM e nós ajudaremos a determinar se o {{site.data.keyword.Bluemix_notm}} é a origem do problema. Para problemas relacionados ao {{site.data.keyword.Bluemix_notm}}, a IBM fornecerá uma correção; no entanto, para os defeitos no buildpack ou no próprio tempo de execução, a IBM ajudará a relatá-los à comunidade apropriada. A IBM
não fornecerá correções para esses buildpacks e tempos de execução.

## Buildpacks externos
{: #external_buildpacks}

Para buildpacks externos, não será fornecido suporte pela IBM. Talvez seja necessário entrar em contato com a comunidade do
Cloud Foundry para suporte.

## Serviços de terceiros
{: #third-party}

Os buildpacks permitem que você use alguns serviços não IBM, de terceiros, como o Dynatrace ou o New Relic, dentro de seus
aplicativos. A IBM não fornece suporte para serviços de terceiro. Para obter informações sobre o uso de serviços de terceiro no IBM
Cloud, consulte _Uso de serviço de nuvem_ na
[descrição do serviço IBM Cloud mais recente
![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo") ](https://www-03.ibm.com/software/sla/sladb.nsf/sla/bm). Antes de usar um serviço de terceiro, consulte as informações sobre licença do provedor de serviços.
