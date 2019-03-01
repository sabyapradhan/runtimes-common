---

copyright:
  years: 2015, 2018
lastupdated: "2018-12-05"

---

{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:new_window: target="_blank"}


# Resolución de problemas de tiempos de ejecución
{: #runtimes}

Puede tener problemas al utilizar los [tiempos de ejecución de {{site.data.keyword.Bluemix}}](index.html). En muchos de los casos, puede solucionar estos problemas siguiendo unos sencillos pasos.
{:shortdesc}

* [Resolución de problemas generales](#ts_all)
* [Liberty for Java](#ts_liberty)
* [SDK for Node.js](#ts_nodejs)
* [ASP.NET Core](#ts_dotnet)
* [PHP](#ts_php)
* [Python](#ts_python)

## Resolución de problemas generales
{: #ts_all}

### Se utiliza un paquete de compilación obsoleto cuando se envía una app por push
{: #ts_loading_bp}

Es posible que no pueda utilizar los últimos componentes de paquete de compilación al enviar por push una app. Puede utilizar paquetes de compilación que tengan mecanismos incorporados para evitar que se carguen componentes obsoletos, o puede suprimir el contenido del directorio de memoria caché de la app antes de enviar por push o volver a transferir la app.

Cuando se envía por push o se vuelve a transferir una app después de que se actualice el paquete de compilación, los componentes del paquete de compilación no se cargan automáticamente. Como resultado, la app utiliza los componentes obsoletos del paquete de compilación de la memoria caché. Las actualizaciones que se hayan aplicado al paquete de compilación desde la última vez que haya enviado por push la app no se implementan.
{: tsSymptoms}

Algunos paquetes de compilación no están configurados para descargar automáticamente todos los componentes actualizados de Internet para asegurarse de utilizar siempre la última versión.
{: tsCauses}

Puede utilizar paquetes de compilación que tengan mecanismos incorporados para evitar que se carguen componentes obsoletos, como por ejemplo los siguientes paquetes de compilación:
{: tsResolve}

  * [Paquete de compilación Cloud Foundry Java ![icono de enlace externo](../../icons/launch-glyph.svg "icono de enlace externo")](https://github.com/cloudfoundry/java-buildpack){: new_window}. Este paquete de compilación tiene un mecanismo incorporado para garantizar que se utiliza la versión más reciente del paquete de compilación. Para obtener más información sobre el funcionamiento de este mecanismo, consulte [extending-caches.md ![icono de enlace externo](../icons/launch-glyph.svg "icono de enlace externo")](https://github.com/cloudfoundry/java-buildpack/blob/master/docs/extending-caches.md){: new_window}.
  * [Paquete de compilación Cloud Foundry Node.js ![icono de enlace externo](../../icons/launch-glyph.svg "icono de enlace externo")](https://github.com/cloudfoundry/nodejs-buildpack){: new_window}. Este paquete de compilación ofrece una funcionalidad similar utilizando variables de entorno. Para habilitar el paquete de compilación Node.js para descargar módulos de nodo de Internet cada vez, escriba el siguiente mandato en la interfaz de línea de mandatos de {{site.data.keyword.Bluemix_notm}}: 	

  ```
  set NODE_MODULES_CACHE=false
  ```

Si el paquete de compilación que utiliza no ofrece un mecanismo para cargar los últimos componentes automáticamente, puede suprimir manualmente el contenido del directorio de memoria caché y volver a enviar por push la app. Siga los siguientes pasos:

 1. Extraiga una rama de un paquete de compilación nulo como, por ejemplo, https://github.com/ryandotsmith/null-buildpack. Para obtener información sobre cómo extraer una rama, consulte [Conceptos básicos de Git - Obtención de un repositorio Git ![icono de enlace externo](../../icons/launch-glyph.svg "icono de enlace externo")](http://www.git-scm.com/book/en/v2/Git-Basics-Getting-a-Git-Repository){: new_window}.  
 2. Añada la siguiente línea al archivo `null-buildpack/bin/compile` y confirme los cambios. Para obtener información sobre cómo confirmar cambios, consulte [Conceptos básicos de Git - Grabación de cambios en el repositorio ![icono de enlace externo](../../icons/launch-glyph.svg "icono de enlace externo")](http://www.git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository){: new_window}.
  ```
  rm -rfv $2/*
  ```
 3. Envíe por push la app con el paquete de compilación nulo que ha sido modificado para suprimir la memoria caché utilizando el siguiente mandato. Después de completar este paso, se suprimirá todo el contenido del directorio de memoria caché de su app.
  ```
  ibmcloud cf push appname -p app_path -b <modified_null_buildpack>
  ```
 4. Envíe por push la app con el último paquete de compilación que desee utilizar mediante el mandato siguiente:
  ```
  ibmcloud cf push appname -p app_path -b <latest_buildpack>
  ```

### La aplicación continúa reiniciándose
{: #ts_apprestart}

La aplicación continúa bloqueándose y reiniciándose.
{: tsSymptoms}

El ciclo de vida del contenedor de aplicaciones, como por ejemplo la evacuación y la conclusión, puede afectar a la funcionalidad de la aplicación.  
{: tsCauses}

Identifique qué paso del ciclo de vida del contenedor de aplicaciones está causando errores en el despliegue de la aplicación. Obtenga más información sobre el [ciclo de vida de la aplicación Cloud Foundry](https://docs.cloudfoundry.org/devguide/deploy-apps/app-lifecycle.html#evacuation).
{: tsResolve}

### El botón Acciones de la página Detalles de la instancia está inhabilitado (en desuso)
  {: #ts_actionsbutton}

  El botón Acciones de la página Detalles de la instancia está inhabilitado.
  {: tsSymptoms}

  Este problema se produce por las siguientes razones:
  {: tsCauses}

   * La app no se ha desplegado con el paquete de compilación Liberty integrado.
   * La app se ha desplegado con una versión anterior del paquete de compilación de Liberty.

  Si el problema se debe a una versión anterior del paquete de compilación de Liberty, vuelva a desplegar la app en {{site.data.keyword.Bluemix_notm}}. De lo contrario, puede proporcionar los siguientes archivos de registro de la aplicación del cliente al equipo de soporte:
  {: tsResolve}

    * logs/messages.log
    * logs/stdout.log
    * logs/stderr.log


### Se necesitan credenciales para abrir la ventana de rastreo o de volcado (en desuso)
  {: #ts_username}

  Se necesita un nombre de usuario y una contraseña para abrir la ventana de rastreo o de volcado.
  {: tsSymptoms}

  Este problema se produce debido al tiempo de espera excedido de inicio de sesión.
  {: tsCauses}

  Escriba de nuevo el nombre de usuario y la contraseña.
  {: tsResolve}


### Los errores se producen al ejecutar operaciones de rastreo o de volcado (en desuso)
  {: #ts_target}

  Se muestra un mensaje de error mientras se están ejecutando las operaciones de rastreo o volcado. El mensaje indica que una instancia de destino de una app no está en ejecución:
  {: tsSymptoms}

  ```
  Instancia 0: la especificación de rastreo se ha establecido correctamente
Instancia 2: la especificación de rastreo se ha establecido correctamente
Instancia 1: la instancia de destino 1 para la app abcd no se encuentra en el estado de ejecución.
  Instancia 3: la especificación de rastreo se ha establecido correctamente
Instancia 4: la especificación de rastreo se ha establecido correctamente
  ```

  Este problema se produce por las siguientes razones:
  {: tsCauses}

    * Las funciones de gestión de rastreo o de volcado son sólo para las instancias de apps que se están ejecutando. No se pueden utilizar operaciones de rastreo o de volcado en instancias de apps que se han detenido, se están iniciando o se han interrumpido.
    * El estado de la instancia de app cambia cuando se abre el diálogo de rastreo o de volcado.

  Cierre la ventana y vuélvala a abrir.
  {: tsResolve}


### Las instancias tienen distintas configuraciones de traceSpecification (en desuso)
  {: #ts_different_config}

  Para distintas instancias de una app, es posible que vea distintas configuraciones de traceSpecification.
  {: tsSymptoms}

  Este comportamiento se produce por las siguientes razones:
  {: tsCauses}

    * Ha cambiado anteriormente la configuración de una o varias de las instancias. Si cambia la configuración de traceSpecification para una instancia, el cambio no se aplicará a otras instancias de la misma app. Por ejemplo, la app utiliza log4j y tiene 2 instancias para esta app. Puede cambiar el nivel de registro de la instancia 0 desde info para depurar, pero el nivel de registro de la instancia 1 permanece como info.

  No se requiere ninguna acción. Este comportamiento es el esperado.
  {: tsResolve}


### Cuota de disco excedida
  {: #ts_diskquota}

  Es posible que en el registro de app vea que se ha superado la cuota de disco.

  Verá el mensaje de error `Cuota de disco excedida` en el registro de la app.
  {: tsSymptoms}

  Este problema se produce por las siguientes razones:
  {: tsCauses}

    * Los archivos de volcado se generan con las instancias de la app que se está ejecutando y los archivos utilizan gran parte de la cuota de disco asignada. De forma predeterminada, la cuota de disco para una instancia de app es 1 GB. Puede comprobar el uso de disco
pulsando **Panel de control>Aplicación>Tiempo de ejecución de app**. En el siguiente ejemplo se muestra la información de tiempo de ejecución,
incluido el uso de disco, para dos instancias de una app:
      ```
      Instance	State	CPU	Memory Usage	Disk Usage

  	0		Running	1.0%	344.8MB/512MB	236.8MB/1GB
	2		Running	2.3%	361.2MB/512MB	235.7MB/1GB
      ```
    * La cuota de disco está limitada por la cuota actual de la organización.

  Utilice uno de los siguientes métodos:
  {: tsResolve}

    * Suprima los archivos de volcado después de descargarlos.
    * Vuelva a desplegar la app con una cuota de disco mayor, incluyendo la entrada siguiente en el manifiesto de despliegue:
      ```
      disk_quota: 2048
      ```

## Liberty for Java
{: #ts_liberty}

### La aplicación no comienza a aceptar conexiones
{: #health_check_timeout}

Una aplicación Liberty no se inicia con el error "_No se ha podido empezar a aceptar conexiones_". Por ejemplo, el error puede parecerse al siguiente en los registros:
{: tsSymptoms}

```
   2016-11-14T14:44:58.45+0000 [API/0]      OUT App instance exited with guid 21ac63eb-9595-428a-94c7-b0b02aaf77cc payload: {"cc_partition"=>"default", "droplet"=>"21ac63eb-9595-428a-94c7-b0b02aaf77cc", "version"=>"b2772438-92de-4d47-b680-ea772ac2288a", "instance"=>"f4799c8c89214bbd8067883c3ffe23e0", "index"=>0, "reason"=>"CRASHED", "exit_status"=>255, "exit_description"=>"failed to accept connections within health check timeout", "crash_timestamp"=>1479134698} 2016-11-14T14:45:07.50+0000 [DEA/4]      ERR Instance (index 0) failed to start accepting connections
```
{: codeblock}

{{site.data.keyword.Bluemix_notm}} realiza una comprobación del estado de la aplicación para ver si se ha iniciado correctamente. La comprobación de estado prueba si la aplicación está escuchando en el puerto que se le ha asignado. El tiempo de espera predeterminado de esta comprobación es de 60 segundos y algunas aplicaciones pueden tardar más de 60 segundos en iniciarse.  Existen varias razones por las que la aplicación puede tardar más en iniciarse. Por ejemplo, el enlace de servicios como [New Relic](/docs/runtimes/liberty/monitoring/newRelic.html) incrementará el tiempo de inicio. También es posible que la aplicación realice pasos de inicialización que tarden en finalizar.
{: tsCauses}

En primer lugar, examine los registros en busca de errores obvios que puedan haber hecho que la aplicación Liberty falle. Si no se encuentran errores evidentes, intente lo siguiente:
{: tsResolve}

#### Aumente el tiempo de espera de comprobación de estado

* Cuando despliegue la aplicación con el mandato `ibmcloud cf push`, aumente el tiempo de espera de inicio de la aplicación con la opción `-t`. Por ejemplo:

        ```
	ibmcloud cf push myApp -t 180
        ```
	{: codeblock}

* El tiempo de espera de comprobación de estado también se especifica en el archivo manifest.yml. Por ejemplo:

        ---
           ...
           timeout: 180
        {: codeblock}

#### Inhabilite la característica appstate

La característica appstate
se integra con el proceso de comprobación de estado de {{site.data.keyword.Bluemix_notm}} para asegurarse de que la aplicación Liberty se inicialice
completamente para que la aplicación pueda recibir solicitudes HTTP. Una vez que la aplicación se haya inicializado por completo, la característica appstate deja de tener efecto.  El efecto secundario de esta característica es que algunas aplicaciones pueden tardar más en iniciarse. Para inhabilitar la característica appstate, establezca la siguiente propiedad del entorno en la aplicación y vuelva a transferir la aplicación:

```
ibmcloud cf set-env myApp JBP_CONFIG_LIBERTY "app_state: false"
```
{: codeblock}

#### Considere la posibilidad de aplicar una refactorización a la aplicación

Si la aplicación tarda mucho en inicializarse, es posible que tenga que aplicar una refactorización a la aplicación para aplicar una inicialización lenta o asíncrona.

#### Realice el despliegue con la opción "no-route"

1. Despliegue la aplicación con la opción "--no-route". Esto inhabilitará la comprobación de estado del puerto. Por ejemplo:

        ```
	ibmcloud cf push myApp –no-route
        ```
	{: codeblock}

2. Una vez que la aplicación se inicializa, correlacione una ruta con la aplicación. Por ejemplo:

        ibmcloud cf map-route myApp mybluemix.net
        {: codeblock}

### Errores de SSL con la pasarela de IBM
{: #ssl_handshake_failure}


Verá los siguientes errores en los registros y la aplicación no se iniciará:
{: tsSymptoms}

```
    2016-11-03T12:32:44.82-0200 [App/0]      ERR java.security.cert.CertPathValidatorException: Certificate chaining error 2016-11-03T12:32:44.83-0200 [App/0]      ERR [ERROR   ] CWPKI0022E: SSL HANDSHAKE FAILURE:  A signer with SubjectDN CN=*.gateway.prd.na.ca.ibmserviceengage.com, O=International Business Machines Corp., L=Armonk, ST=New York, C=US was sent from the target host.  The signer might need to be added to local trust store /home/vcap/app/wlp/usr/servers/defaultServer/resources/security/key.jks, located in SSL configuration alias defaultSSLConfig.  The extended error message from the SSL handshake exception is: PKIX path building failed: java.security.cert.CertPathBuilderException: PKIXCertPathBuilderImpl could not build a valid CertPath.; internal cause is: 2016-11-03T12:32:44.83-0200 [App/0]      ERR java.security.cert.CertPathValidatorException: The certificate issued by CN=DigiCert Global Root CA, OU=www.digicert.com, O=DigiCert Inc, C=US is not trusted; internal cause is: 2016-11-03T12:32:44.83-0200 [App/0]      ERR java.security.cert.CertPathValidatorException: Certificate chaining error
```
{: codeblock}

Pueden generarse estos errores cuando un servicio seguro se enlaza con una aplicación Liberty y la aplicación Liberty se ha desplegado como un directorio de servidor o servidor empaquetado que contiene el archivo server.xml que configura la característica Liberty ssl-1.0. Enlazar el servicio seguro con la aplicación Liberty hace que el tiempo de ejecución se conecte al servicio sobre una conexión segura. Esta conexión segura se establece utilizando los valores SSL predeterminados. Puesto que los valores SSL predeterminados se especifican en el archivo server.xml de Liberty, es posible que el almacén de confianza configurado no confíe en el certificado que utiliza el servicio seguro.
{: tsCauses}

Modifique la configuración para que utilice el almacén de confianza de JVM con una de las opciones siguientes.  No olvide volver a transferir la aplicación después de realizar el cambio.
{: tsResolve}

#### Actualice el archivo server.xml de Liberty

Actualice el archivo server.xml para que utilice el archivo cacerts de JVM como almacén de confianza. Añada lo siguiente al archivo server.xml:

        <ssl id="defaultSSLConfig" trustStoreRef="defaultTrustStore"/>
        <keyStore id="defaultTrustStoretore" location="${java.home}/lib/security/cacerts"/>
        {: codeblock}

#### Actualice el almacén de confianza configurado

Modifique el almacén de confianza configurado para que confíe en la entidad emisora de certificados raíz de DigitCert.
  1. Descargue la entidad emisora de certificados raíz de DigiCert de https://www.digicert.com/CACerts/DigiCertGlobalRootCA.crt.
  2. Suponiendo que utilice resources/security/key.jks como almacén de confianza, importe la entidad emisora de certificados en la clave utilizado el programa de utilidad de herramientas de claves de Java:

            keytool -importcert --storepass <keyStorePassword> -keystore &lt;path&gt;/resources/security/key.jks -file DigiCertGlobalRootCA.crt
            {: codeblock}


## SDK for Node.js
{: #ts_nodejs}

### La aplicación no se puede iniciar: resolución de problemas generales

Para el paquete de compilación de {{site.data.keyword.runtime_nodejs_notm}} V3.23 o posterior, intente vendorizar las dependencias como primer paso de la resolución de problemas. Vendorizar dependencias significa empaquetar las dependencias en los mismos archivos de origen que la aplicación. Esto puede resolver varios errores que se pueden producir cuando las dependencias presuponen que están en el mismo directorio base que la aplicación.

1. Desde el directorio raíz de la aplicación, instale las dependencias con el mandato siguiente.

   ```bash
   npm install
   ```
   {: codeblock}
1. Asegúrese de que el archivo `.cfignore` no incluya la línea siguiente:

   ```
   node_modules/
   ```

Ahora, cuando despliegue la aplicación con el mandato `ibmcloud cf push`, en lugar de descargar las dependencias en una ubicación distinta, las dependencias se copian en el mismo directorio que el resto de la aplicación.

### La aplicación no se inicia con el error "No queda espacio en el dispositivo"
{: #no_space_left_on_device}


Una aplicación Node.js no se inicia con el error "No queda espacio en el dispositivo". Por ejemplo, el error puede parecerse al siguiente en los registros:
{: tsSymptoms}

```
   2017-01-16T14:25:14.61-0500 [CELL/0]     ERR tar: ./app/node_modules/pm2/node_modules/cron/node_modules/moment-timezone/LICENSE: No se puede escribir: No queda espacio en el dispositivo

```
{: codeblock}

Las aplicaciones Node.js que utilizan versiones de NPM anteriores a la versión 3 consumen más espacio al descargar dependencias.
{: tsCauses}

Modifique el archivo package.json de la aplicación de modo que utilice NPM versión 3 o posterior.
{: tsResolve}

```
{
  "name": "myapp",
  "description": "this is my app",
  "version": "0.1",
  "engines": {
     "node": "4.2.4",
     "npm": "3.10.10"
  }
}
```
{: codeblock}

### La aplicación se reinicia debido a restricciones de memoria
{: #oom}

Node.js no sabe cuánta memoria está disponible para la aplicación, de modo que el recopilador de basura no puede ejecutarse antes de que se agote la memoria.

```
2017-09-01T11:00:42.19-0400 [APP/PROC/WEB/0]OUT Exit status 137
2017-09-01T11:00:42.23-0400 [CELL/0]     OUT Exit status 0
2017-09-01T11:00:42.27-0400 [CELL/0]     OUT Destroying container
2017-09-01T11:00:42.34-0400 [API/0]      OUT Process has crashed with type: "web"
2017-09-01T11:00:42.36-0400 [API/0]      OUT App instance exited with guid eecfba3b-430c-4a6b-b71f-ac72816fe152 payload: {"instance"=>"77dbb981-16d0-3a05-3235-9a4b", "index"=>0, "reason"=>"CRASHED", "exit_description"=>"2 error(s) occurred:\n\n* 2 error(s) occurred:\n\n* Exited with status 137 (out of memory)\n* cancelled\n* cancelled", "crash_count"=>1, "crash_timestamp"=>1504278042244633291, "version"=>"6497b5b5-67d4-4c5a-b1af-362e522a029d"}
2017-09-01T11:00:43.35-0400 [CELL/0]     OUT Successfully destroyed container
```
{: codeblock}

Una posible solución es establecer la opción `--max_old_space_size` en el mandato de inicio de la aplicación en el archivo package.json. Esta opción representa parte de la ocupación de memoria de la aplicación y debe establecerse en un valor inferior a la memoria total disponible para la aplicación. Más información sobre [Grandes picos de memoria y Heroku](https://github.com/nodejs/node/issues/3370) para un debate más profundo de este tema.
```
  "scripts": {
    "start": "node --max_old_space_size=800 server.js"
  }
```
{: codeblock}

## ASP.NET Core
{: #ts_dotnet}

Mi aplicación no se despliega y aparece el mensaje: `API/0App instance exited ... payload: {... "reason"=>"CRASHED", "exit_status"=>-1, ...}`.
{: tsSymptoms}

Si recibe un mensaje parecido cuando envía la aplicación ASP.net, lo más probable es que sea porque la aplicación supera los límites de cuota de memoria o de disco.  Aumente las cuotas de memoria o de espacio de disco para la aplicación.
{: tsCauses}

Mi aplicación no se puede desplegar con el mensaje: `Failed to compress droplet: signal: broken pipe` o `No space left on device`.  ¿Cómo lo puedo solucionar?
{: tsSymptoms}

Los proyectos enviados por push desde código fuente que contiene un gran número de dependencias de paquetes NuGet pueden generar este error cuando la memoria caché de paquetes NuGet está habilitada.  Establezca la variable de entorno `CACHE_NUGET_PACKAGES` en `false` para inhabilitar la memoria caché. Para obtener más información, consulte las instrucciones sobre cómo [Inhabilitar el paquete NuGet](/docs/runtimes/dotnet/disablingNuGet.md).
{: tsCauses}

### Enlaces útiles
* [NuGet](https://docs.nuget.org/Consume/Overview){: new_window}
* [Visión general de ASP.NET Core](http://docs.asp.net/en/latest/conceptual-overview/aspnet.html){: new_window}

## PHP
{: #ts_PHP}

### Mensajes de AVISO del paquete de compilación PHP
{: #ts_phplog}

Puede que vea mensajes que contengan AVISOS procedentes de los registros. Puede detener el registro de estos mensajes cambiando el nivel de registro.

Al enviar por push una app a {{site.data.keyword.Bluemix_notm}} utilizando un paquete de compilación PHP, es posible que vea mensajes que contengan `AVISOS`:
{: tsSymptoms}

```
• 2015-01-26T15:00:59.70+0100 [App/0] ERR [26-Jan-2015 14:00:59] NOTICE: [pool www] 'user' directive is ignored when FPM is not running as root
• 2015-01-26T15:01:00.63+0100 [App/0] ERR [26-Jan-2015 14:00:59] NOTICE: [pool www] 'user' directive is ignored when FPM is not running as root
• 2015-01-26T15:01:00.63+0100 [App/0] ERR [26-Jan-2015 14:00:59] NOTICE: fpm is running, pid 93
• 2015-01-26T15:01:00.63+0100 [App/0] ERR [26-Jan-2015 14:00:59] NOTICE: ready to handle connections
```
En el paquete de compilación PHP, el parámetro error_log define el nivel de registro. De forma predeterminada, el valor del parámetro `error_log` es **stderr notice**. El ejemplo siguiente muestra la configuración del nivel de registro predeterminado del archivo `nginx-defaults.conf` del paquete de compilación PHP que proporciona Cloud Foundry. Para obtener más información, consulte [cloudfoundry/php-buildpack ![icono de enlace externo](../../icons/launch-glyph.svg "icono de enlace externo")](https://github.com/cloudfoundry/php-buildpack/blob/ff71ea41d00c1226d339e83cf2c7d6dda6c590ef/defaults/config/nginx/1.5.x/nginx-defaults.conf){: new_window}.
{: tsCauses}

```
daemon off;
error_log stderr notice;
pid @{HOME}/nginx/logs/nginx.pid;
```

Los mensajes de `AVISO` son informativos y pueden no indicar un problema. Puede detener el registro de estos mensajes cambiando el nivel de registro de `stderr notice` a `stderr error`
en el archivo nginx-defaults.conf del paquete de compilación. Por ejemplo: 	
{: tsResolve}

```
daemon off;
error_log stderr error;
pid @{HOME}/nginx/logs/nginx.pid;
```
Para obtener más información sobre cómo cambiar la configuración de registro, consulte [error_log ![icono de enlace externo](../../icons/launch-glyph.svg "icono de enlace externo")](http://nginx.org/en/docs/ngx_core_module.html#error_log){: new_window}.


## Python
{: #ts_python}

### No se puede importar una biblioteca Python de terceros en {{site.data.keyword.Bluemix_notm}}
{: #ts_importpylib}

Es posible que no pueda importar una biblioteca Python de terceros en {{site.data.keyword.Bluemix_notm}}. Para resolver el problema, añada archivos de configuración al directorio raíz de su app python.

Cuando intenta importar una biblioteca Python de terceros, como por ejemplo la biblioteca `web.py`, el mandato `ibmcloud cf push` falla.
{: tsSymptoms}

Falta información de configuración para la app Python.
{: tsCauses}

Añada un archivo `requirements.txt` y un archivo `Procfile` al directorio raíz de la app Python. La información siguiente da por supuesto que está importando una biblioteca `web.py`:
{: tsResolve}

 1. Añada un archivo `requirements.txt` al directorio raíz de la app Python.

 El archivo `requirements.txt` especifica los paquetes de la biblioteca que la app Python requiere y la versión de los paquetes. El ejemplo siguiente muestra el contenido del archivo `requirements.txt`, donde `web.py==0.37` indica que la versión de la biblioteca `web.py` que se descargará es la 0.37, y `wsgiref==0.1.2` indica que la versión de la interfaz de la pasarela del servidor web que la biblioteca web.py requiere es la 0.1.2.
	 ```
	 web.py==0.37
     wsgiref==0.1.2
	 ```
	 Para obtener más información sobre cómo configurar el archivo `requirements.txt`, consulte [Archivos de requisitos](https://pip.readthedocs.org/en/1.1/requirements.html).

 2. Añada un archivo `Procfile` al directorio raíz de la app Python.
 El archivo `Procfile` debe contener el mandato de inicio de la app Python. En el mandato siguiente, *nombreapp* es el nombre de la app Python, y *PUERTO* es el número de puerto que la app Python debe utilizar para recibir solicitudes de los usuarios de la app. *$PORT* es opcional. Si no especifica PORT en el mandato start, se utilizará el número de puerto bajo la variable de entorno `VCAP_APP_PORT` de la app.
	```
	web: python <nombreapp>.py $PORT
	```

Ahora puede importar una biblioteca Python de terceros en {{site.data.keyword.Bluemix_notm}}.
