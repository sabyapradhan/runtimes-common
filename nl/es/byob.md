---

copyright:
  years: 2015, 2018
lastupdated: "2018-06-27"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Utilización de compilación de la comunidad
{: #using_buildpacks}

Si no encuentra ningún iniciador en el catálogo de {{site.data.keyword.Bluemix}} que proporcione
el tiempo de ejecución que desea, puede incorporar un paquete de compilación externo
a {{site.data.keyword.Bluemix_notm}}. Puede especificar un paquete de compilación personalizado compatible con Cloud Foundry cuando despliegue la aplicación mediante el mandato `ibmcloud cf push`.
{:shortdesc}

La comunidad de Cloud Foundry proporciona paquetes de compilación externos que puede utilizar como paquetes de compilación propios. Antes de desplegar la aplicación en {{site.data.keyword.Bluemix_notm}}, asegúrese de instalar la interfaz de línea de mandatos de {{site.data.keyword.Bluemix_notm}}.

**Nota:** los paquetes de compilación externos no los proporciona IBM. Póngase en contacto con la comunidad de Cloud Foundry para obtener ayuda.

## Paquetes de compilación de la comunidad incorporados

En {{site.data.keyword.Bluemix_notm}},
puede utilizar paquetes de compilación incorporados que ofrece la comunidad de Cloud Foundry. Para ver los paquetes de compilación incorporados de la comunidad, ejecute el mandato `ibmcloud cf buildpacks`:

```
ibmcloud cf buildpacks
Getting buildpacks...

buildpack      position   enabled   locked   filename
...
java_buildpack     7      true      false    buildpack_java_v2.0.2.zip
ruby_buildpack     8      true      false    buildpack_ruby_v46-245-g2fc4ad8.zip
nodejs_buildpack   9      true      false    buildpack_nodejs_v8-177-g2b0a5cf.zip
```
{:screen}


Para el mismo tiempo de ejecución o infraestructura, los paquetes de compilación de IBM
prevalecen sobre los de la comunidad. Si desea que el paquete de compilación de la comunidad prevalezca sobre el que ha creado IBM, debe especificar el paquete de compilación con la opción `-b` del mandato `ibmcloud cf push`.

Por ejemplo, puede utilizar el paquete de compilación de la comunidad para aplicaciones web Java™ ejecutando el siguiente mandato:

```
ibmcloud cf push app_name -b java_buildpack -p app_path
```
{:pre}

También puede utilizar el paquete de compilación de la comunidad para aplicaciones Node.js con el siguiente mandato:

```
ibmcloud cf push app_name -b nodejs_buildpack -p app_path
```
{: codeblock}

Para un tiempo de ejecución o infraestructura que no reciba soporte de los paquetes de compilación creados por IBM pero sí de los integrados de la comunidad, no tiene que utilizar la opción `-b` con el mandato `ibmcloud cf push`. Por ejemplo, para aplicaciones Ruby, no hay paquetes de compilación creados por IBM. Puede utilizar el paquete de compilación integrado de la comunidad especificando el mandato siguiente:

```
ibmcloud cf push app_name -p app_path
```
{: codeblock}

## Paquetes de compilación externos

Puede utilizar paquetes de compilación externos o personalizados en {{site.data.keyword.Bluemix_notm}}. Debe especificar el URL del paquete de compilación con la opción `-b`, así como la pila con la opción `-s` en el mandato `ibmcloud cf push`. Por ejemplo, para utilizar un paquete de compilación de la comunidad externo para archivos estáticos, ejecute el siguiente mandato:

```
ibmcloud cf push app_name -p app_path -b https://github.com/cloudfoundry/staticfile-buildpack.git
```
{: codeblock}

Si no desea utilizar el paquete de compilación de la comunidad incorporado para aplicaciones Ruby, puede utilizar un paquete de compilación externo especificando el mandato siguiente:

```
ibmcloud cf push app_name -p app_path -b https://github.com/cloudfoundry/ruby-buildpack.git
```
{: codeblock}

También puede utilizar un paquete de compilación personalizado para la aplicación. Por ejemplo, puede utilizar un paquete de compilación PHP de código abierto proporcionado por la comunidad de Cloud Foundry. Al desplegar la aplicación PHP en {{site.data.keyword.Bluemix_notm}}, escriba el mandato siguiente para especificar el URL del repositorio Git del paquete de compilación:

```
ibmcloud cf push app_name -p app_path -b https://github.com/cloudfoundry/php-buildpack.git
```
{:pre}

También puede editar el archivo `manifest.yml` en el proyecto para añadir una línea `buildpack`.

```
buildpack: https://github.com/cloudfoundry/python-buildpack.git
```
{: codeblock}


## Especificación de la versión de paquete de compilación de Java

Para especificar una versión de paquete de compilación de Java, utilice el mandato `ibmcloud cf set-env`. Por ejemplo, especifique el mandato siguiente para establecer la versión de Java a 1.7.0:

```
ibmcloud cf set-env app_name JBP_CONFIG_OPEN_JDK_JRE &apos;{jre: { version: 1.7.0_+ }}&apos;
```
{: codeblock}

A continuación, vuelva a transferir la aplicación para que el cambio sea efectivo.

```
ibmcloud cf restage app_name
```
{: codeblock}

## Utilice el archivo `manifest.yml`.

Puede añadir la variable de entorno y el valor que desee especificar directamente en el archivo `manifest.yml`. Consulte [Variables de entorno](https://docs.cloudfoundry.org/devguide/deploy-apps/manifest.html#env-block).
