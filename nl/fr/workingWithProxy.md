---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-23"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}


# Utilisation d'un proxy
{: #working_with_proxy}

Dans certains environnements tels que [{{site.data.keyword.Bluemix_notm}} Dedicated](/docs/dedicated/index.html#dedicated) et
[{{site.data.keyword.Bluemix_notm}} Local](/docs/local/index.html#local), un proxy peut être configuré, ce qui affecte le
comportement de votre application tant en phase de préproduction qu'à l'exécution.

Vous pouvez configurer votre application pour la faire fonctionner avec le proxy en utilisant les variables d'environnement suivantes :
  * [http_proxy ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://docs.cloudfoundry.org/buildpacks/proxy-usage.html){: new_window}
  * [https_proxy ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://docs.cloudfoundry.org/buildpacks/proxy-usage.html){: new_window}
  * [no_proxy ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://www.gnu.org/software/wget/manual/html_node/Proxies.html){: new_window}

Vous pouvez définir ces variables d'environnement à l'aide de *bluemix app env-set* ou du fichier *manifest.yml*.  Selon la manière dont vous configurez les variables d'environnement du proxy, si votre application doit télécharger des ressources de l'Internet pendant la phase de préproduction, il est possible qu'elle le fasse en utilisant le proxy. Par exemple, si vous avez une application Nodejs dans un environnement avec la variable `http_proxy` pointant sur `yourProxyURL` et que vous voulez autoriser `npm` à télécharger les modules de l'Internet **mais pas le proxy**.  Pour télécharger sans utiliser le proxy, réglez `no_proxy` sur `npmjs.org`.

**Remarque **: Peut-être souhaitez-vous que votre application utilise le proxy en phase d'exécution, après sa phase de préproduction.  L'utilisation du proxy à l'exécution est entièrement dépendante de l'application et n'est en rien affectée par le pack de construction ou par les variables d'environnement du proxy.

## Applications Java
{: #java_apps}

Pour les applications [Liberty for Java](/docs/runtimes/liberty/index.html) et [java_buildpack](/docs/runtimes/tomcat/index.html), les paramètres de proxy peuvent être passés à l'exécution via la variable d'environnement **JAVA_OPTS**.  Par exemple, vous pouvez émettre la commande suivante, puis remettre votre application en préproduction :
```
   ibmcloud app env-set myApp JAVA_OPTS "-Dhttp.proxyHost=yourProxyURL -Dhttp.proxyPort=yourProxyPort"
```
{: codeblock}

A l'exécution, votre application utilisera les paramètres de proxy spécifiés. Pour plus d'informations sur les options de proxy Java, consultez [Java Networking and Proxies ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://docs.oracle.com/javase/8/docs/technotes/guides/net/proxies.html){: new_window}.
