---

copyright:
  years: 2015, 2018
lastupdated: "2018-10-09"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}

# Gestión de apps de Liberty y Node.js
{: #app_management}


App Management es un conjunto de programas de utilidad de desarrollo y depuración que se pueden habilitar en las aplicaciones Liberty para {{site.data.keyword.Bluemix}}.
{:shortdesc}

## Utilidades de App Management
{: #Utilities}

### Utilidades de Liberty
* [proxy](#proxy)
* [noproxy](#noproxy)
* [devconsole](#devconsole)
* [hc](#hc)
* [shell](#shell)
* [debug](#debug)
* [jmx](#jmx)
* [localjmx](#localjmx)

### Programas de utilidad de Node.js (en desuso)

**Nota sobre desuso**: los programas de utilidad de App Management han quedado en desuso para las aplicaciones Node.js. Algunos programas de utilidad que estaban disponibles tanto para aplicaciones Node.JS como para aplicaciones Liberty, ahora solo están disponibles para las aplicaciones Liberty. Estos programas de utilidad han quedado en desuso para las aplicaciones Node.js.

* [proxy](#proxy)
* [noproxy](#noproxy)
* [devconsole](#devconsole)
* [hc](#hc)
* [shell](#shell)
* [inspector](#inspector)
* [trace](#trace)

## Cómo configurar App Management
{: #configure}

Para habilitar las utilidades de App Management, establecer el valor de la variable de entorno de *BLUEMIX_APP_MGMT_ENABLE* a la utilidad o lista de utilidades que quiere habilitar, después vuelva a transferir su aplicación. Puede habilitar múltiples utilidades al separar con un **+**.

Por ejemplo, para habilitar las utilidades *hc*, *debug* y *trace* ejecute el siguiente mandato:

```
ibmcloud cf set-env myApp BLUEMIX_APP_MGMT_ENABLE hc+debug+trace
```
{: codeblock}

Volver a transferir su aplicación después de establecer la variable del entorno:

```
ibmcloud cf restage myApp
```
{: codeblock}

Si no quiere que las utilidades de App Management se instalen en su aplicación, establezca el entorno de *BLUEMIX_APP_MGMT_INSTALL* como 'false' y vuelva a transferir su aplicación.

Por ejemplo, ejecute los siguientes mandatos para transferir su aplicación sin utilidades de App Management:

```
ibmcloud cf set-env myApp BLUEMIX_APP_MGMT_INSTALL false
ibmcloud cf restage myApp
```
{: codeblock}

## Restricciones
{: #restrictions}
* Los cambios que haga a su aplicación mediante App Management son transitorios y se pierden después de salir de este modo. Esta modalidad es solo para uso temporal de desarrollo y no está diseñada para utilizarse como entorno producción debido al rendimiento.
* Para las aplicaciones de Node.js, la mayoría de las utilidades de App Management no funcionan si establece su mandato de **inicio** en el archivo de `manifest.yml` o con la opción `-c` de la línea de mandatos. Dichos métodos son alteraciones temporales de paquetes de compilación y son antipatrones para iniciar aplicaciones Node.js. Para obtener los mejores resultados,
establezca el mandato **start** en el archivo de `package.json` o `Procfile`.


### Utilidades de Liberty
{: #liberty_utilities}

#### jmx
{: #jmx}

La utilidad *jmx* habilita el conector JMX REST para permitir que un cliente JMX remoto gestione la aplicación mediante las credenciales de usuario de {{site.data.keyword.Bluemix_notm}}.

Puede supervisar varias instancias de una aplicación mediante JMX, pero requiere una conexión JMX aparte para cada instancia. El valor predeterminado es supervisar la instancia 0. Para supervisar la instancia 1, puede utilizar el siguiente fragmento de código:

```
ibmcloud cf ssh -i 1 -N -T -L 5000:127.0.0.1:5001
```
{: codeblock}

Para obtener más información sobre la configuración de un conector JMX, consulte [Configuración de una conexión JMX segura con el perfil de Liberty ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www-01.ibm.com/support/knowledgecenter/was_beta_liberty/com.ibm.websphere.wlp.nd.multiplatform.doc/ae/twlp_admin_restconnector.html){:new_window}.

**Importante**: *jmx* utilidad no inicia el *proxy*.

#### localjmx
{: #localjmx}

El programa de utilidad *localjmx* habilita la característica de Liberty [localConnector-1.0 ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](http://www.ibm.com/support/knowledgecenter/SSEQTP_liberty/com.ibm.websphere.wlp.doc/ae/rwlp_feature_localConnector-1.0.html){:new_window}. Combinado con el reenvío de puerto local, *localjmx* actúa de una forma alternativa para permitir que un cliente JMX remoto gestione la aplicación.


**Antes de empezar**: *localjmx* requiere instalar JConsole.

El programa de utilidad *localjmx* sólo es aplicable en las aplicaciones que se ejecutan en una célula de Diego. Para utilizar *localjmx*, primero establezca el reenvío de puertos utilizando el mandato `ibmcloud cf ssh`. Por ejemplo:

```
ibmcloud cf ssh -N -T -L 5000:127.0.0.1:5000 <appName>
```
{: codeblock}

A continuación, para conectarse con JConsole, seleccione **Proceso remoto**, especifique `127.0.0.1:5000`, y utilice una conexión no segura.

#### proxy (en desuso para Node.js)
{: #proxy}

La utilidad *proxy* proporciona una gestión mínima de las aplicaciones entre su aplicación e {{site.data.keyword.Bluemix_notm}}.

Cuando esta opción está habilitada, el paquete de compilación inicia un agente de proxy que se encuentra entre el contenedor y el tiempo de ejecución de la aplicación.  La utilidad *proxy* gestiona todas las solicitudes que recibe la aplicación. Según el tipo de solicitud, realiza una acción de App Management, o reenvia la solicitud a su aplicación. Al utilizar *proxy*, el contenedor de la aplicación sigue en funcionamiento aunque la aplicación se cuelga. El agente de proxy también permite actualizaciones incrementales de archivos, que permite la modalidad
*Live Edit* para aplicaciones Node.js.

Algunas utilidades de App Management requieren que utilice la utilidad de *proxy* con su aplicación, y que pueda iniciar el *proxy* automáticamente.

**Nota sobre desuso:**: para Node.js, la versión de {{site.data.keyword.Bluemix_notm}} de Cloud Foundry se ejecuta en células Diego. La característica *noproxy* es para células que no san Diego.

#### noproxy (en desuso para Node.js)
{: #noproxy}

La utilidad *noproxy* inhabilita la utilidad *proxy* cuando se inicia automáticamente por otro programa.  Con Diego no se necesita el proxy, ya que Diego ofrece la posibilidad de ejecutar *ssh* directamente en la aplicación y configurar el reenvío de puertos.

La utilidad *noproxy* sólo se aplica a las aplicaciones que se ejecutan en una célula de Diego.

**Nota sobre desuso**: el programa de utilidad *noproxy* inhabilita *proxy*. Puesto que *proxy* ha quedado en desuso para Node.js y ya no funciona, no se necesita *noproxy*.

#### devconsole (en desuso para Node.js)
{: #devconsole}

Los usuarios pueden reiniciar, detener o suspender sus aplicaciones con la utilidad de consola de desarrollo (*devconsole*). Los usuarios también pueden habilitar o acceder las utilidades shell o inspector mediante *devconsole*.  Puede utilizar el siguiente URL para acceder a *devconsole*:
```
  https://<yourappname>.mybluemix.net/bluemix-debug/manage
```
{: codeblock}

Para la versión de Node 6.3.0 o superior, o posterior, la consola de desarrollo proporciona un botón de reinicio para la aplicación y el acceso al programa de utilidad *shell*.  Consulte el debate *inspector* para obtener más información.

**Importante**: la utilidad *devconsole* inicia el *proxy*.

**Nota sobre desuso**: en lugar de utilizar el programa de utilidad *devconsole* para Node.js, utilice la consola de {{site.data.keyword.Bluemix_notm}}. Desde la consola, vaya a la página **Tiempo de ejecución** de la aplicación. En la página **Tiempo de ejecución** puede detener, iniciar, renombrar y suprimir la aplicación. También puede acceder al shell y a otra información.

#### hc (en desuso para Node.js)
{: #hc}

El agente de (*hc*) Health Center permite que su aplicación se supervise con el cliente de Health Center.  Para Node.js, el agente *hc* solo está disponible con las versiones de ejecución de Node.js incluidas con el paquete de compilación IBM SDK for Node.js.  Consulte las [últimas actualizaciones del paquete de compilación sdk-for-nodejs](/docs/runtimes/nodejs/updates.html) para ver el conjunto actual de entornos de ejecución.

Con el agente de Health Center habilitado, podrá analizar el rendimiento de las applicaciones Liberty y Node.js mediante las herramientas de diagnóstico y supervisión de IBM. Para obtener más información consulte [Cómo analizar el rendimiento de las apps Liberty Java o Node.js en {{site.data.keyword.Bluemix_notm}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://developer.ibm.com/bluemix/2015/07/03/how-to-analyze-performance-in-bluemix/){:new_window}.

**Importante:** La utilidad *hc* inicia el *proxy*.

**Utilizando *hc* con *noproxy* **

La utilidad *hc* puede utilizarse junto con *noproxy*. Para utilizar Health Center con *noproxy*, primero establezca el reenvío de puertos con el mandato `ibmcloud cf ssh`. Por ejemplo:

```
ibmcloud cf ssh -N -T -L 1883:127.0.0.1:1883 <appName>
```
{: codeblock}

A continuación, para conectar con el cliente de Health Center, utilice una [conexión MQTT ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](http://www.ibm.com/support/knowledgecenter/SS3KLZ/com.ibm.java.diagnostics.healthcenter.doc/topics/connectingtojvm.html){: new_window} y especifique el host como `127.0.0.1` y el puerto como `1883`.

**Nota sobre desuso:** en lugar de utilizar el agente de Health Center para Node.js, utilice la consola de {{site.data.keyword.Bluemix_notm}}. Desde la consola, vaya a la página **Tiempo de ejecución** de la aplicación, donde puede acceder a uso de CPU, uso de memoria y espacio de disco. La página también tiene un separador en el que puede acceder a las variables de entorno.

#### shell (en desuso para Node.js)
{: #shell}

La utilidad *shell* permite un shell basado en web.  Puede acceder al *shell* desde la utilidad *devconsole* o con la siguiente URL:

```
  https://<yourappname>.mybluemix.net/bluemix-debug/shell
```
{: codeblock}

Después de acceder a la utilidad *shell*, se muestra una ventana de terminal con acceso shell a su aplicación. Puede hacer todo lo que está admitido en un shell normal, como por ejemplo editar archivos, comprobar el uso de la memoria, o ejecutar mandatos de diagnóstico.

**Importante:** La utilidad *shell* también inicia el *proxy*.

Diego proporciona un shell interactivo a través del mandato `ibmcloud cf ssh`, a que el programa de utilidad *shell* solo resulta útil para aplicaciones que se ejecutan en un DEA.
{: .tip}

**Nota sobre desuso:** para Node.js, aunque el mandato `cf ssh` sigue funcionando, también puede acceder al shell desde la consola de {{site.data.keyword.Bluemix_notm}}. Desde la consola, vaya a la página **Tiempo de ejecución** de la aplicación, donde encontrará un separador para extraer un shell en la página web.

### Programas de utilidad de Node.js (en desuso)
{: #node_utilities}

#### inspector (en desuso)
{: #inspector}

El programa de utilidad inspector puede utilizarse para crear perfiles de uso de CPU, añadir puntos de interrupción y depurar código, mientras la aplicación se ejecuta en {{site.data.keyword.cloud_notm}}.  Para las versiones de Node.js antes de 6.3.0, el inspector habilita la interfaz del depurador del inspector de nodos.  Para obtener más información sobre el inspector de nodos, consulte el Readme para [node-inspector en GitHub ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/node-inspector/node-inspector){: new_window}.  Para las versiones 6.3.0 y posteriores de Node.js, el *inspector* utiliza la utilidad [V8 Inspector Integration for Node.js ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://nodejs.org/dist/latest-v6.x/docs/api/debugger.html#debugger_v8_inspector_integration_for_node_js){:new_window}.

##### Para versiones de Node.js posteriores a la 6.3.0
Cuando se inicia la modalidad de depuración, *proxy* se habilita automáticamente, incluso si utiliza una versión de Node.js que no incluye *proxy*. Las versiones de Node.js después de 6.3.0 no incluyen el *proxy.* Si utiliza el programa de utilidad *inspector* con versiones de Node.js después de 6.3.0, puede desactivar *proxy* de nuevo utilizando *noproxy.*

En lugar de utilizar *proxy* para acceder a la interfaz de *inspector*, puede utilizar la función de Herramientas Developer del navegador web Google Chrome.  

Habilite el acceso al URL con el reenvio del puerto local mediante el siguiente mandato:
```
ibmcloud cf ssh -N -T -L 9229:127.0.0.1:9229 <appName>
```
{: codeblock}

Obtenga el registro de arranque para la aplicación utilizando el mandato siguiente.
```
ibmcloud cf logs <appName> --recent
```
{: codeblock}

Si el programa de utilidad *inspector* se activa, el registro contendrá mensajes similares a los siguientes:
```
 ... Necesitará un túnel SSH para el puerto 9229 para poder utilizar Chrome DevTools para depurar de forma remota su app
 ... Iniciando app con el nodo '-- inspect=9229 app.js'
```
{: codeblock}

Utilice una versión actualizada del navegador web de Google Chrome para ir a `chrome://inspect`.
En este URL, verá la app lista junto con un enlace a los archivos de su aplicación como `file://home/vcap/app/app.js`. y, a continuación, seleccione **inspect** para acceder a la interfaz inspeccionar.

##### Para versiones de Node.js anteriores a la 6.3.0
Si utiliza *proxy,* puede acceder a la interfaz de *inspector* en `https://myApp.mybluemix.net/bluemix-debug/inspector`.

Si no utiliza el programa de utilidad *proxy*, habilite el acceso a la aplicación mediante el URL de reenvío de puerto local con el mandato siguiente:

```
ibmcloud cf ssh -N -T -L 8790:127.0.0.1:8790 <appName>
```
{: codeblock}

A continuación, acceda al inspector desde el URL, `http://127.0.0.1:8790`.

**Nota sobre desuso:** todavía puede utilizar la característica *inspector* si cambia ligeramente la configuración. En el archivo `manifest.yml` de la aplicación, establezca el mandato de inicio en 'node --inspect=9229 app.js'. Luego puede seguir las instrucciones correspondientes a las versiones de Node.js posteriores a la 6.3.0. Sin embargo, los registros que normalmente indican cuándo el inspector está activo no están visibles, pero funciona correctamente.


#### trace (en desuso)
{: #trace}

La utilidad trace le permite establecer dinámicamente niveles de rastreo si la aplicación está utilizando log4js, ibmbluemix, o módulos de registro bunyan.

Vaya a la página **Detalles** en la consola web de {{site.data.keyword.cloud_notm}} y seleccione **Acciones** para ver la interfaz de usuario.

Nota: versiones soportadas de dependencia:

* log4js:(0.6.0 - 0.6.24)
* bunyan: (1.0.0, 1.0.1, 1.1.0 - 1.1.3, 1.2.0 - 1.2.3, 1.3.0 - 1.3.5)
* ibmbluemix: (1.0.0-20140707-1250)-(1.0.0-20150409-1328)


La utilidad trace no está disponible cuando la aplicación se inicia utilizando la opción `-b buildpack`.

La utilidad trace no inicia el *proxy*.
