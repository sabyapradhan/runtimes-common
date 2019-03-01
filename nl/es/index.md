---

copyright:
  years: 2017, 2018
lastupdated: "2018-09-21"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}

# Paquetes de compilación disponibles
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

## Acerca de los paquetes de compilación
{: #about_buildpacks}

Los paquetes de compilación de Cloud Foundry proporcionan el soporte del tiempo de ejecución para las aplicaciones en el entorno de Cloud Foundry. Al desplegar una aplicación en {{site.data.keyword.cloud}}, se inicia un paquete que soporta el tipo de aplicación. {{site.data.keyword.cloud_notm}} proporciona soporte de paquetes de compilación de Cloud Foundry para Java EE, Node.js, ASP.Net, Swift, y otros tipos de aplicaciones.
Puede utilizar los paquetes de compilación incluidos con {{site.data.keyword.cloud_notm}} para desplegar aplicaciones y enlazarlas a los servicios.

*  Cloud Foundry

    Cloud Foundry es una plataforma de código abierto para la automatización del ciclo de vida de las aplicaciones.  {{site.data.keyword.Bluemix}} se instala sobre la plataforma Cloud Foundry como un servicio. Consulte la [Documentación de Cloud Foundry](https://www.cloudfoundry.org/learn/) para obtener más información.

*  Aplicación Cloud Foundry

   Una aplicación o app cloud foundry es una aplicación pensada para que cree instancias de ella uno de los paquetes de compilación que proporciona {{site.data.keyword.Bluemix_notm}}.

*  Paquete de compilación

   Un paquete de compilación es un paquete específico de un lenguaje de software suministrado por {{site.data.keyword.Bluemix_notm}}. Cuando se despliega una aplicación en {{site.data.keyword.Bluemix_notm}}, se elige un paquete de compilación adecuado para la aplicación. El paquete de compilación proporciona el entorno de ejecución
para la aplicación.  Puede ver el conjunto de paquetes de compilación proporcionados por {{site.data.keyword.Bluemix_notm}} emitiendo el mandato `ibmcloud cf buildpacks` desde la línea de mandatos.

*  Tiempo de ejecución

   El tiempo de ejecución es el conjunto de componentes que proporciona el entorno de ejecución para una aplicación.  A veces los términos *tiempo de ejecución* y *paquete de compilación* se utilizan indistintamente.  Cuando un paquete de compilación ha terminado de desplegar una aplicación, se establece el entorno de ejecución.

*  Contenedor modelo

   Un contenedor modelo es una aplicación sencilla diseñada para un determinado tiempo de ejecución.  Los contenedores modelo proporcionan plantillas o ejemplos de los tipos de aplicaciones específicos del lenguaje y del tiempo de ejecución.  Puede utilizar un contenedor modelo como código inicial para empezar a desarrollar aplicaciones más sofisticadas.  {{site.data.keyword.Bluemix_notm}} proporciona:
   * *Contenedores modelo Hello world*: aplicaciones sencillas que generalmente proporcionan una aplicación 'hello world' específica de un lenguaje y tiempo de ejecución.
   * *Contenedores de modelo con servicios*: demonstrar cómo utilizar un tiempo de ejecución con un servicio.
   * *Contenedores modelo con infraestructuras*: aplicaciones sencillas específicas de un lenguaje y tiempo de ejecución que aprovechan la infraestructura de un determinado lenguaje, como Python Flask o Ruby Sinatra.

*  Servicio

   Un servicio es un recurso proporcionado por {{site.data.keyword.Bluemix_notm}} que se puede acoplar a una aplicación.
