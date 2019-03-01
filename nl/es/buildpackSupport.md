---

copyright:
  years: 2016, 2018
lastupdated: "2018-11-20"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}

# Declaración de soporte de paquete de compilación
{: #buildpack_support_statement}


## Paquetes de compilación de IBM incorporados
{: #built-in_ibm_buildpacks}

Para [Liberty for Java](/docs/runtimes/liberty/index.html), [SDK for Node.js](/docs/runtimes/nodejs/index.html) y [ASP.NET Core](/docs/runtimes/dotnet/index.html), IBM soportará dos versiones (n y n - 1), por ejemplo, IBM Liberty Buildpack v3.12 e IBM Liberty Buildpack v3.11. Cada paquete de compilación proporcionará y dará soporte a una o más versiones principales de su tiempo de ejecución correspondiente según convenga. Los paquetes de compilación se renovarán normalmente una vez al mes con la versión menor más reciente del tiempo de ejecución disponible.

Para [{{site.data.keyword.Bluemix_notm}} Runtime for Swift](/docs/runtimes/swift/index.html), IBM proporciona soporte para el paquete de compilación que coincida con la versión más reciente de Swift disponible en [Swift.org](http://swift.org). Las actualizaciones para el paquete de compilación se sincronizan con la última versión publicada disponible de Swift.

Se pueden notificar problemas sobre cualquier versión del paquete de compilación de IBM integrado soportado actualmente en {{site.data.keyword.Bluemix_notm}}, pero se deben verificarán sobre la versión más reciente. Si se detecta un defecto, IBM proporcionará un arreglo en un futuro nivel del tiempo de ejecución y el paquete de compilación correspondiente. IBM no proporcionará arreglos para versiones principales y menores anteriores (N-1, n-1). IBM no proporcionará soporte para tiempos de ejecución de la comunidad incluso cuando se utilicen paquetes de compilación de IBM, por ejemplo, Open JDK con el paquete de compilación Liberty. Estos tiempos de ejecución de la comunidad siguen la misma política de soporte que los paquetes de compilación de la comunidad integrados, tal como se describe en la siguiente sección.

## Paquetes de compilación de la comunidad incorporados
{: #built-in_community_buildpacks}

Cloud Foundry Community proporciona los siguientes paquetes de compilación de la comunidad integrados:

* [Java](/docs/runtimes/tomcat/index.html)
* [Node.js](https://github.com/cloudfoundry/nodejs-buildpack)
* [PHP](/docs/runtimes/php/index.html)
* [Ruby](/docs/runtimes/ruby/index.html)
* [Python](/docs/runtimes/python/index.html)
* [Go](/docs/runtimes/go/index.html)

Las actualizaciones de estos paquetes de compilación se realizarán cuando se actualice {{site.data.keyword.Bluemix_notm}} a una versión nueva de Cloud Foundry. Se pueden notificar problemas con estos tiempos de ejecución en {{site.data.keyword.Bluemix_notm}} a IBM para obtener ayuda a la hora de determinar si {{site.data.keyword.Bluemix_notm}} es el origen del problema. En el caso de problemas relacionados con {{site.data.keyword.Bluemix_notm}}, IBM proporcionará un arreglo; sin embargo, para los defectos del propio paquete de compilación o tiempo de ejecución, IBM le ayudará a notificarlos a la comunidad pertinente. IBM no proporcionará arreglos para estos paquetes de compilación y tiempos de ejecución.

## Paquetes de compilación externos
{: #external_buildpacks}

IBM no ofrece soporte para paquetes de compilación externos. Es posible que se deba poner en contacto con la comunidad de Cloud Foundry para obtener ayuda.

## Servicios de terceros
{: #third-party}

Los paquetes de compilación le permiten utilizar algunos servicios de terceros que no son de IBM, como por ejemplo Dynatrace o New Relic, dentro de sus aplicaciones. IBM no proporciona soporte para servicios de terceros. Para obtener información sobre el uso de servicios de terceros en IBM Cloud, consulte _Uso de servicios en la nube_ en la última [descripción de servicio de IBM Cloud ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www-03.ibm.com/software/sla/sladb.nsf/sla/bm). Antes de utilizar un servicio de terceros, consulte la información sobre licencias desde el proveedor de servicios.
