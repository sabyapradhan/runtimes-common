---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-23"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}


# Trabalhando com um proxy
{: #working_with_proxy}

Em alguns ambientes como [{{site.data.keyword.Bluemix_notm}}
Dedicado](/docs/dedicated/index.html#dedicated) e [{{site.data.keyword.Bluemix_notm}} Local](/docs/local/index.html#local), um proxy pode
ser configurado, o que afeta o comportamento de seu aplicativo durante a preparação e o tempo de execução.

É possível configurar seu aplicativo para trabalhar com o proxy usando as seguintes variáveis de ambiente:
  * [http_proxy ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://docs.cloudfoundry.org/buildpacks/proxy-usage.html){: new_window}
  * [https_proxy ![ícone de link externo](../../icons/launch-glyph.svg "ícone de link externo")](https://docs.cloudfoundry.org/buildpacks/proxy-usage.html){: new_window}
  * [no_proxy ![ícone de link externo](../../icons/launch-glyph.svg "ícone de link externo")](http://www.gnu.org/software/wget/manual/html_node/Proxies.html){: new_window}

É possível configurar essas variáveis de ambiente usando *bluemix app env-set* ou por meio do arquivo *manifest.yml*.  Dependendo da configuração das variáveis de ambiente do proxy, se o seu aplicativo fizer download de recursos da Internet durante a
preparação, o download dos recursos poderá ser feito usando o proxy. Por exemplo, se você tiver um aplicativo Nodejs em um ambiente
com `http_proxy` configurado como `yourProxyURL` e quiser permitir que o
`npm` faça download dos módulos da Internet **, mas não do proxy**.  Para fazer download sem
usar o proxy, configure `no_proxy` para `npmjs.org`.

**Nota**: você pode querer que seu aplicativo use o proxy durante o tempo de execução, depois da preparação.  O uso do proxy no tempo de execução é totalmente dependente do aplicativo e não é afetado pelo buildpack ou pelas variáveis de
ambiente do proxy.

## aplicativos Java
{: #java_apps}

Para os aplicativos [Liberty for Java](/docs/runtimes/liberty/index.html) e
[java_buildpack](/docs/runtimes/tomcat/index.html), as configurações de proxy podem ser passadas para o
tempo de execução por meio da variável de ambiente **JAVA_OPTS**.  Por exemplo, é possível emitir o comando e,
então, remontar seu aplicativo:
```
   ibmcloud app env-set myApp JAVA_OPTS "-Dhttp.proxyHost=yourProxyURL -Dhttp.proxyPort=yourProxyPort"
```
{: codeblock}

Seu aplicativo usará, então, as configurações de proxy especificadas no tempo de execução. Veja [Redes e proxies Java ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://docs.oracle.com/javase/8/docs/technotes/guides/net/proxies.html){: new_window} para obter mais informações sobre as opções de proxy Java.
