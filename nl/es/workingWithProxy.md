---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-23"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}


# Cómo trabajar con un proxy
{: #working_with_proxy}

En algunos entornos como [{{site.data.keyword.Bluemix_notm}} Dedicado](/docs/dedicated/index.html#dedicated)
[{{site.data.keyword.Bluemix_notm}} Local](/docs/local/index.html#local) es posible que se haya configurado un proxy que afecte
el comportamiento de su aplicación durante la transferencia y la ejecución.

Puede configurar la aplicación para que funcione con el proxy utilizando las siguientes variables de entorno:
  * [http_proxy ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.cloudfoundry.org/buildpacks/proxy-usage.html){: new_window}
  * [https_proxy ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.cloudfoundry.org/buildpacks/proxy-usage.html){: new_window}
  * [no_proxy ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](http://www.gnu.org/software/wget/manual/html_node/Proxies.html){: new_window}

Puede establecer estas variables de entorno utilizando *bluemix app env-set* o a través del archivo
*manifest.yml*.  Dependiendo de cómo configure las variables de entorno de proxy, si la aplicación de descarga recursos de Internet durante la transferencia, los recursos se pueden descargar utilizando el proxy. Por ejemplo, si tiene una aplicación de Nodejs en su entorno con `http_proxy` establecido en `yourProxyURL` y quiere permitir que `npm` descargue módulos desde Internet **pero no el proxy**.  Para descargar sin utilizar el proxy, establezca `no_proxy` en `npmjs.org`.

**Nota**: Puede que quiera que su aplicación utilice el proxy durante el tiempo de ejecución después de la transferencia.  El tiempo de ejecución del proxy es totalmente dependiente de la aplicación y no está afectada por el paquete de compilación o las variables de entorno del proxy.

## Aplicaciones Java
{: #java_apps}

Para las aplicaciones de [Liberty for Java](/docs/runtimes/liberty/index.html) y el [java_buildpack](/docs/runtimes/tomcat/index.html), los valores del proxy pueden pasarse al entorno de ejecución a través de la variable de entorno **JAVA_OPTS**.  Por ejemplo, puede emitir el mandato y luego transferir la aplicación:
```
   ibmcloud app env-set myApp JAVA_OPTS "-Dhttp.proxyHost=yourProxyURL -Dhttp.proxyPort=yourProxyPort"
```
{: codeblock}

La aplicación utiliza los valores de proxy especificados en el tiempo de ejecución. Consulte [Redes y proxies Java ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.oracle.com/javase/8/docs/technotes/guides/net/proxies.html){: new_window} para obtener más información sobre las opciones de proxy Java.
