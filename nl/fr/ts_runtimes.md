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


# Traitement des incidents liés aux contextes d'exécution
{: #runtimes}

Vous pouvez rencontrer des problèmes lorsque vous utilisez des contextes d'exécution [{{site.data.keyword.Bluemix}}](index.html). Dans de nombreux cas, ces problèmes peuvent être résolus en quelques opérations simples.
{:shortdesc}

* [Traitement des incidents d'ordre général](#ts_all)
* [Liberty for Java](#ts_liberty)
* [SDK for Node.js](#ts_nodejs)
* [ASP.NET Core](#ts_dotnet)
* [PHP](#ts_php)
* [Python](#ts_python)

## Traitement des incidents d'ordre général
{: #ts_all}

### Pack de construction obsolète utilisé lorsqu'une application est envoyée par commande push
{: #ts_loading_bp}

Il est possible que vous ne puissiez pas utiliser les derniers composants du pack de construction lorsque vous envoyez une application par commande push. Vous pouvez utiliser des packs de construction disposant de mécanismes intégrés pour empêcher le chargement de composants obsolètes ou supprimer le contenu du répertoire cache de votre application avant de l'envoyer par commande push ou de la reconstituer.

Lorsque vous envoyez une application par commande push ou que vous la reconstituez une fois le pack de construction mis à jour, les composants les plus récents du pack de construction ne sont pas automatiquement chargés. Par conséquent, votre application utilise les composants obsolètes du pack de construction à partir du cache. Les mises à jour qui ont été appliquées au pack de construction depuis le dernier envoi de l'application par commande push ne sont pas implémentées.
{: tsSymptoms}

Certains packs de construction ne sont pas configurés pour télécharger automatiquement depuis Internet tous les composants mis à jour pour faire en sorte que vous utilisiez toujours la version la plus récente.
{: tsCauses}

Vous pouvez utiliser des packs de construction disposant de mécanismes intégrés pour éviter de charger des composants obsolètes, par exemple :
{: tsResolve}

  * [Cloud Foundry Java buildpack ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/cloudfoundry/java-buildpack){: new_window}. Ce pack de construction comporte un mécanisme intégré qui permet de s'assurer d'utiliser la version la plus récente. Pour plus d'informations sur le fonctionnement de ce mécanisme, voir [extending-caches.md ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/cloudfoundry/java-buildpack/blob/master/docs/extending-caches.md){: new_window}.
  * [Cloud Foundry Node.js buildpack ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/cloudfoundry/nodejs-buildpack){: new_window}. Ce pack de construction a une fonctionnalité similaire qui utilise des variables d'environnement. Pour permettre au pack de construction Node.js de télécharger des modules de noeud sur internet à chaque fois, entrez la commande suivante dans l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}} : 	

  ```
  set NODE_MODULES_CACHE=false
  ```

Si le pack de construction que vous utilisez ne dispose pas d'un mécanisme permettant de charger automatiquement les composants les plus récents, vous pouvez supprimer manuellement le contenu du répertoire cache et envoyer à nouveau votre application par commande push. Utilisez la procédure suivante :

 1. Réservez une branche d'un pack de construction null, par exemple https://github.com/ryandotsmith/null-buildpack. Pour plus d'informations sur la réservation d'une branche, voir [Git Basics - Getting a Git Repository ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://www.git-scm.com/book/en/v2/Git-Basics-Getting-a-Git-Repository){: new_window}.  
 2. Ajoutez la ligne suivante au fichier `null-buildpack/bin/compile` et validez les modifications. Pour plus d'informations sur la validation des modifications, voir [Git Basics - Recording Changes to the Repository ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://www.git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository){: new_window}.
  ```
  rm -rfv $2/*
  ```
 3. Envoyez votre application par commande push avec le pack de construction null modifié pour supprimer le cache à l'aide de la commande suivante. Une fois cette étape réalisée, l'intégralité du contenu du répertoire cache de votre application est supprimée.
  ```
  ibmcloud cf push appname -p app_path -b <modified_null_buildpack>
  ```
 4. Envoyez votre application par commande push avec le pack de construction le plus récent que vous souhaitez utiliser à l'aide de la commande suivante :
  ```
  ibmcloud cf push appname -p app_path -b <latest_buildpack>
  ```

### L'application continue de redémarrer
{: #ts_apprestart}

L'application ne cesse de tomber en panne puis redémarrer.
{: tsSymptoms}

Le cycle de vie du conteneur d'application, comme l'évacuation et l'arrêt, peut avoir un impact sur la fonctionnalité de votre application.  
{: tsCauses}

Identifiez les étapes du cycle de vie du conteneur d'application qui cause des erreurs dans le déploiement d'application. Plus d'informations sur le [cycle de vie de l'application Cloud Foundry](https://docs.cloudfoundry.org/devguide/deploy-apps/app-lifecycle.html#evacuation).
{: tsResolve}

### Le bouton Actions de la page Détails de l'instance est désactivé(obsolète)
  {: #ts_actionsbutton}

  Le bouton Actions de la page Détails de l'instance est désactivé.
  {: tsSymptoms}

  Ce problème se produit pour les raisons suivantes :
  {: tsCauses}

   * L'application n'est pas déployée avec le pack de construction Liberty imbriqué.
   * L'application a été déployée avec une ancienne version du pack de construction Liberty.

  Si l'application a été déployée avec une ancienne version du pack de construction Liberty, redéployez l'application dans {{site.data.keyword.Bluemix_notm}}. Sinon, vous pouvez transmettre les fichiers journaux d'application client suivants à l'équipe de support :
  {: tsResolve}

    * logs/messages.log
    * logs/stdout.log
    * logs/stderr.log


### Les données d'identification sont requises pour ouvrir une fenêtre de trace ou de vidage (obsolète)
  {: #ts_username}

  Un nom d'utilisateur et un mot de passe sont requis pour ouvrir la fenêtre de trace et de vidage.
  {: tsSymptoms}

  Ce problème se produit à cause du dépassement de délai d'attente de la session de connexion.
  {: tsCauses}

  Entrez de nouveau le nom d'utilisateur et le mot de passe.
  {: tsResolve}


### Une erreur se produit lorsque des opérations de trace ou de vidage son en cours (obsolète)
  {: #ts_target}

  Un message d'erreur s'affiche lorsque des opérations de trace ou de vidage sont en cours. Ce message indique qu'une instance cible d'une application n'est pas à l'état en cours :
  {: tsSymptoms}

  ```
  Instance 0: Trace specification is set successfully
  Instance 2: Trace specification is set successfully
  Instance 1: Target instance 1 for app abcd is not in the running state.
  Instance 3: Trace specification is set successfully
  Instance 4: Trace specification is set successfully
  ```

  Ce problème se produit pour les raisons suivantes :
  {: tsCauses}

    * Les fonctionnalités de trace ou de vidage s'appliquent uniquement aux instances d'application en cours d'exécution. Les opérations de trace ou de vidage ne peuvent pas être utilisées sur des instances d'application arrêtées, en cours de démarrage ou en panne.
    * Le statut de l'instance d'application change lorsque la boîte de dialogue de trace ou de vidage est ouverte.

  Fermez la fenêtre puis rouvrez-la.
  {: tsResolve}


### Les instances possèdent des configurations traceSpecification différentes (obsolète)
  {: #ts_different_config}

  Il peut exister différentes configurations traceSpecification pour différentes instances d'une seule application.
  {: tsSymptoms}

  Ce comportement se produit pour les raisons suivantes :
  {: tsCauses}

    * Vous avez changé la configuration pour une ou plusieurs instances précédemment. Si vous modifiez la configuration traceSpecification pour une instance, cette modification ne s'applique pas aux autres instances de la même application. Par exemple, votre application utilise log4j et il existe deux instances pour cette application. Vous pouvez remplacer le niveau de journalisation info de l'instance 0 par debug ; toutefois, le niveau de journalisation de l'instance 1 reste info.

  Aucune action requise. Ce comportement est normal.
  {: tsResolve}


### Quota de disque dépassé
  {: #ts_diskquota}

  Vous pouvez constater, dans votre journal d'application, que le quota de disque est dépassé.

  Le message d'erreur `Disk quota exceeded` figure dans le journal de votre application.
  {: tsSymptoms}

  Ce problème se produit pour l'une des raisons suivantes :
  {: tsCauses}

    * Les fichiers de vidage sont générés avec les instances d'application en cours d'exécution ; le quota de disque alloué est dépassé. Par défaut, le quota de disque pour une instance d'application est de 1 Go. Vous pouvez vérifier l'utilisation de votre disque en cliquant sur **Tableau de bord > Application > Contexte d'exécution de l'application**. L'exemple suivant montre les informations de contexte d'exécution, notamment l'utilisation du disque, pour deux instances d'une application :
      ```
      Instance	State	CPU	Memory Usage	Disk Usage

  	0		Running	1.0%	344.8MB/512MB	236.8MB/1GB
  	2		Running	2.3%	361.2MB/512MB	235.7MB/1GB
      ```
    * Le quota de disque est limité par le quota de l'organisation.

  Utilisez l'une des méthodes suivantes :
  {: tsResolve}

    * Supprimez les fichiers de vidage après les avoir téléchargés.
    * Redéployez l'application avec un quota de disque plus important en insérant l'entrée suivante dans le manifeste de déploiement :
      ```
      disk_quota: 2048
      ```

## Liberty for Java
{: #ts_liberty}

### L'application ne démarre pas lors de l'acceptation de connexions
{: #health_check_timeout}

Une application Liberty ne démarre pas et affiche une erreur "_Ne peut pas commencer à accepter des connexions._". Par exemple, l'erreur dans les journaux peut être similaire à l'exemple suivant :
{: tsSymptoms}

```
   2016-11-14T14:44:58.45+0000 [API/0]      OUT App instance exited with guid 21ac63eb-9595-428a-94c7-b0b02aaf77cc payload: {"cc_partition"=>"default", "droplet"=>"21ac63eb-9595-428a-94c7-b0b02aaf77cc", "version"=>"b2772438-92de-4d47-b680-ea772ac2288a", "instance"=>"f4799c8c89214bbd8067883c3ffe23e0", "index"=>0, "reason"=>"CRASHED", "exit_status"=>255, "exit_description"=>"failed to accept connections within health check timeout", "crash_timestamp"=>1479134698}
   2016-11-14T14:45:07.50+0000 [DEA/4]      ERR Instance (index 0) failed to start accepting connections
```
{: codeblock}

{{site.data.keyword.Bluemix_notm}} effectue un diagnostic d'intégrité sur l'application afin de voir si elle a correctement démarré. Le diagnostic d'intégrité vérifie si l'application écoute sur port qui lui a été affecté. Le délai d'attente par défaut pour cette vérification est de 60 secondes et il est possible que le démarrage de certaines applications prenne davantage de temps.  Il existe différentes raisons pour lesquelles l'application peut avoir besoin de plus de temps pour démarrer. Ainsi, la liaison de services tels que [New Relic](/docs/runtimes/liberty/monitoring/newRelic.html) va augmenter le temps de démarrage. Il est également possible que l'application effectue une procédure d'initialisation nécessitant davantage de temps.
{: tsCauses}

Commencez par rechercher dans les journaux des erreurs manifestes qui auraient pu causer l'échec de l'application Liberty. Si aucune erreur manifeste n'est trouvée, essayez la procédure suivante :
{: tsResolve}

#### Augmenter le dépassement du délai d'attente de vérification de santé

* Lors du déploiement de l'application à l'aide de la commande `ibmcloud cf push`, indiquez un délai d'attente de démarrage de l'application plus long via l'option `-t`. Par exemple :

        ```
	ibmcloud cf push myApp -t 180
        ```
	{: codeblock}

* Le dépassement du délai d'attente de vérification de santé peut également être spécifié dans le fichier manifest.yml. Par exemple :

        ---
           ...
           timeout: 180
        {: codeblock}

#### Désactiver la fonction appstate

La fonction appstate est intégrée au processus de diagnostic d'intégrité {{site.data.keyword.Bluemix_notm}} pour vérifier que l'application Liberty est entièrement initialisée de sorte que l'application puisse recevoir des demandes HTTP. Une fois l'application initialisée, la fonction appstate n'a plus d'effet.  Effet secondaire de cette fonction, certaines applications peuvent prendre plus de temps à démarrer. Pour désactiver la fonction appstate, définissez la propriété d'environnement suivante sur votre application puis reconstituez l'application en préproduction.

```
ibmcloud cf set-env myApp JBP_CONFIG_LIBERTY "app_state: false"
```
{: codeblock}

#### Prendre en considération la restructuration de l'application

Si l'initialisation de votre application prend du temps, vous devrez peut-être la restructurer pour effectuer une initialisation différée et/ou asynchrone.

#### Déployer avec l'option "no-route"

1. Déployez votre application avec l'option "--no-route". Cela désactivera le diagnostic d'intégrité de port. Par exemple :

        ```
	ibmcloud cf push myApp –no-route
        ```
	{: codeblock}

2. Une fois l'application initialisée, mappez une route à l'application. Par exemple :

        ibmcloud cf map-route myApp mybluemix.net
        {: codeblock}

### Erreurs SSL avec la passerelle d'IBM
{: #ssl_handshake_failure}


Les erreurs suivantes sont visibles dans les journaux et l'application risque de ne pas démarrer :
{: tsSymptoms}

```
    2016-11-03T12:32:44.82-0200 [App/0]      ERR java.security.cert.CertPathValidatorException: Certificate chaining error
    2016-11-03T12:32:44.83-0200 [App/0]      ERR [ERROR   ] CWPKI0022E: SSL HANDSHAKE FAILURE:  A signer with SubjectDN CN=*.gateway.prd.na.ca.ibmserviceengage.com, O=International Business Machines Corp., L=Armonk, ST=New York, C=US was sent from the target host.  The signer might need to be added to local trust store /home/vcap/app/wlp/usr/servers/defaultServer/resources/security/key.jks, located in SSL configuration alias defaultSSLConfig.  The extended error message from the SSL handshake exception is: PKIX path building failed: java.security.cert.CertPathBuilderException: PKIXCertPathBuilderImpl could not build a valid CertPath.; internal cause is:
    2016-11-03T12:32:44.83-0200 [App/0]      ERR java.security.cert.CertPathValidatorException: The certificate issued by CN=DigiCert Global Root CA, OU=www.digicert.com, O=DigiCert Inc, C=US is not trusted; internal cause is:
    2016-11-03T12:32:44.83-0200 [App/0]      ERR java.security.cert.CertPathValidatorException: Certificate chaining error
```
{: codeblock}

Les erreurs peuvent être générées quand un service sécurisé est lié à une application Liberty et que l'application Liberty est déployée en tant que répertoire de serveur ou package de serveur contenant le fichier server.xml qui configure la fonction Liberty ssl-1.0. La liaison du service sécurisé à l'application Liberty entraîne la connexion de l'environnement d'exécution au service via une connexion sécurisée. Cette connexion sécurisée est établie via les paramètres SSL par défaut. Comme les paramètres SSL par défaut sont spécifiés dans le fichier server.xml de Liberty, il est possible que le magasin de clés de confiance n'approuve pas le certificat utilisé par le service sécurisé.
{: tsCauses}

Modifiez la configuration de façon à utiliser le magasin de clés de confiance de la machine JVM avec l'une des options suivantes.  Veillez à reconstituer en préproduction votre application après avoir effectué le changement.
{: tsResolve}

#### Mettre à jour le fichier Liberty server.xml

Mettez à jour le fichier server.xml afin d'utiliser le fichier cacerts de la machine JVM comme magasin de clés de confiance. Ajoutez les éléments suivants à votre fichier server.xml :

        <ssl id="defaultSSLConfig" trustStoreRef="defaultTrustStore"/>
        <keyStore id="defaultTrustStoretore" location="${java.home}/lib/security/cacerts"/>
        {: codeblock}

#### Mettre à jour le magasin de clés de confiance configuré

Modifiez le magasin de clés de confiance configuré afin d'approuver l'autorité de certification DigitCert ROOT CA.
  1. Téléchargez l'autorité de certification DigiCert Root CA depuis https://www.digicert.com/CACerts/DigiCertGlobalRootCA.crt.
  2. En considérant que le fichier resources/security/key.jks est utilisé comme magasin de clés de confiance, importez l'autorité de certification dans la clé à l'aide de l'utilitaire de clé Java :

            keytool -importcert --storepass <keyStorePassword> -keystore &lt;path&gt;/resources/security/key.jks -file DigiCertGlobalRootCA.crt
            {: codeblock}


## SDK for Node.js
{: #ts_nodejs}

### L'application ne démarre pas : Traitement général des incidents

Pour le pack de construction {{site.data.keyword.runtime_nodejs_notm}} version 3.23 ou ultérieure, essayer de fournir vos dépendances constitue la première étape d'identification et de résolution de problème. En d'autres termes, vous packagez les dépendances dans les mêmes fichiers source que votre application. Cela permet de résoudre diverses erreurs pouvant se produire quand des dépendances considèrent qu'elles se trouvent dans le même répertoire que l'application.

1. Depuis le répertoire racine de votre application, installez les dépendances en exécutant la commande suivante.

   ```bash
   npm install
   ```
   {: codeblock}
1. Assurez-vous que le fichier `.cfignore` ne comporte pas les lignes suivantes :

   ```
   node_modules/
   ```

A présent, lorsque vous déployez votre application avec la commande `ibmcloud cf push`, les dépendances sont copiées dans le même répertoire que le reste de l'application, au lie d'un emplacement distinct.

### L'application ne démarre pas et affiche une erreur de type "Plus d'espace sur l'unité"
{: #no_space_left_on_device}


Une application Node.js ne démarre pas et affiche une erreur de type "Plus d'espace sur l'unité". Par exemple, l'erreur dans les journaux peut être similaire à l'exemple suivant :
{: tsSymptoms}

```
   2017-01-16T14:25:14.61-0500 [CELL/0]     ERR tar: ./app/node_modules/pm2/node_modules/cron/node_modules/moment-timezone/LICENSE: Cannot write: No space left on device

```
{: codeblock}

Les applications Node.js utilisant des versions NPM antérieures à la version 3 consomment plus d'espace pour le téléchargement des dépendances.
{: tsCauses}

Modifiez le fichier package.json de votre application afin d'utiliser une version 3 ou ultérieure de NPM.
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

### L'application redémarre en raison de contraintes de mémoire
{: #oom}

Node.js ne connaît pas la quantité de mémoire disponible pour l'application. C'est pourquoi le récupérateur de place risque de ne pas s'exécuter avant que la mémoire ne soit épuisée.

```
2017-09-01T11:00:42.19-0400 [APP/PROC/WEB/0]OUT Exit status 137
2017-09-01T11:00:42.23-0400 [CELL/0]     OUT Exit status 0
2017-09-01T11:00:42.27-0400 [CELL/0]     OUT Destroying container
2017-09-01T11:00:42.34-0400 [API/0]      OUT Process has crashed with type: "web"
2017-09-01T11:00:42.36-0400 [API/0]      OUT App instance exited with guid eecfba3b-430c-4a6b-b71f-ac72816fe152 payload: {"instance"=>"77dbb981-16d0-3a05-3235-9a4b", "index"=>0, "reason"=>"CRASHED", "exit_description"=>"2 error(s) occurred:\n\n* 2 error(s) occurred:\n\n* Exited with status 137 (out of memory)\n* cancelled\n* cancelled", "crash_count"=>1, "crash_timestamp"=>1504278042244633291, "version"=>"6497b5b5-67d4-4c5a-b1af-362e522a029d"}
2017-09-01T11:00:43.35-0400 [CELL/0]     OUT Successfully destroyed container
```
{: codeblock}

Une solution possible consiste à définir l'option `--max_old_space_size` sur la commande de démarrage de l'application dans le fichier package.json. Cette option représente une partie de l'empreinte de mémoire de l'application et doit être définie sur une valeur inférieure à celle de la mémoire totale disponible pour l'application. Voir [Large memory spikes and Heroku](https://github.com/nodejs/node/issues/3370) pour une discussion plus approfondie de cette rubrique.
```
  "scripts": {
    "start": "node --max_old_space_size=800 server.js"
  }
```
{: codeblock}

## ASP.NET Core
{: #ts_dotnet}

Echec de déploiement de l'application avec le message `API/0App instance exited ... payload: {... "reason"=>"CRASHED", "exit_status"=>-1, ...}`.
{: tsSymptoms}

Si vous recevez un message similaire lorsque vous envoyez votre application ASP.net par commande push, il est probable que votre application dépassez les limites de quota de mémoire ou d'espace disque.  Augmentez les quotas pour la mémoire et l'espace disque de votre application.
{: tsCauses}

Echec du déploiement de l'application avec le message `Failed to compress droplet: signal: broken pipe` ou `No space left on device`.  Comment corriger le problème ?
{: tsSymptoms}

Les projets envoyés par commande push depuis le code source contenant un grand nombre de dépendances de package NuGet peuvent parfois provoquer cette erreur quand la mise en cache du package NuGet est activée.  Définissez la variable d'environnement `CACHE_NUGET_PACKAGES` sur `false` pour désactiver le cache. Voir les instructions sur la [désactivation du cache de packages NuGet](/docs/runtimes/dotnet/disablingNuGet.md) pour plus d'informations.
{: tsCauses}

### Liens utiles
* [NuGet](https://docs.nuget.org/Consume/Overview){: new_window}
* [Présentation d'ASP.NET Core](http://docs.asp.net/en/latest/conceptual-overview/aspnet.html){: new_window}

## PHP
{: #ts_PHP}

### Messages NOTICE du pack de construction PHP
{: #ts_phplog}

Des messages contenant le terme NOTICE peuvent apparaître dans les journaux. Vous pouvez arrêter la journalisation de ces messages en changeant le niveau de journalisation.

Lorsque vous envoyez par commande push une application dans {{site.data.keyword.Bluemix_notm}} à l'aide d'un pack de construction PHP, des messages contenant le terme `NOTICE` peuvent s'afficher :
{: tsSymptoms}

```
• 2015-01-26T15:00:59.70+0100 [App/0] ERR [26-Jan-2015 14:00:59] NOTICE: [pool www] 'user' directive is ignored when FPM is not running as root
• 2015-01-26T15:01:00.63+0100 [App/0] ERR [26-Jan-2015 14:00:59] NOTICE: [pool www] 'user' directive is ignored when FPM is not running as root
• 2015-01-26T15:01:00.63+0100 [App/0] ERR [26-Jan-2015 14:00:59] NOTICE: fpm is running, pid 93
• 2015-01-26T15:01:00.63+0100 [App/0] ERR [26-Jan-2015 14:00:59] NOTICE: ready to handle connections
```
Dans le pack de construction PHP, le paramètre error_log définit le niveau de journalisation. Par défaut, la valeur du paramètre `error_log` est **stderr notice**. L'exemple ci-dessous illustre la configuration du niveau de journalisation par défaut dans le fichier `nginx-defaults.conf` du pack de construction PHP fourni par Cloud Foundry. Pour plus d'informations, voir [cloudfoundry/php-buildpack ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/cloudfoundry/php-buildpack/blob/ff71ea41d00c1226d339e83cf2c7d6dda6c590ef/defaults/config/nginx/1.5.x/nginx-defaults.conf){: new_window}.
{: tsCauses}

```
daemon off;
error_log stderr notice;
pid @{HOME}/nginx/logs/nginx.pid;
```

Les messages `NOTICE` sont des messages d'information et n'indiquent pas nécessairement un problème. Vous pouvez arrêter la journalisation de ces messages en remplaçant le niveau de journalisation `stderr notice` par `stderr error` dans le fichier nginx-defaults.conf de votre pack de construction. Par exemple : 	
{: tsResolve}

```
daemon off;
error_log stderr error;
pid @{HOME}/nginx/logs/nginx.pid;
```
Pour plus d'informations sur la modification de la configuration de journalisation par défaut, voir [error_log ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://nginx.org/en/docs/ngx_core_module.html#error_log){: new_window}.


## Python
{: #ts_python}

### Impossible d'importer une bibliothèque Python tierce dans {{site.data.keyword.Bluemix_notm}}
{: #ts_importpylib}

Il se peut que vous ne puissiez pas importer une bibliothèque Python tierce dans {{site.data.keyword.Bluemix_notm}}. Pour résoudre le problème, ajoutez des fichiers de configuration au répertoire racine de votre application Python.

Lorsque vous tentez d'importer une bibliothèque Python tierce, comme la bibliothèque `web.py`, la commande `ibmcloud cf push` échoue.
{: tsSymptoms}

Les informations de configuration pour l'application Python sont manquantes.
{: tsCauses}

Ajoutez un fichier `requirements.txt` et un fichier `Procfile` au répertoire racine de votre application Python. Les informations suivantes supposent que vous importiez la bibliothèque `web.py` :
{: tsResolve}

 1. Ajoutez un fichier `requirements.txt` au répertoire racine de votre application Python.

 Le fichier `requirements.txt` spécifie les packages de bibliothèque requis pour votre application Python ainsi que la version des packages. L'exemple ci-après illustre le contenu du fichier `requirements.txt`, où `web.py==0.37` indique que la version de la bibliothèque `web.py` qui sera téléchargée est la version 0.37 et `wsgiref==0.1.2` indique que la version de l'interface Web de Secure Gateway requise par la bibliothèque web.py est la version 0.1.2.
	 ```
	 web.py==0.37
     wsgiref==0.1.2
	 ```
	 Pour plus d'informations sur la configuration du fichier `requirements.txt`, voir [Requirements files](https://pip.readthedocs.org/en/1.1/requirements.html).

 2. Ajoutez un fichier `Procfile` au répertoire racine de votre application Python.
 Le fichier `Procfile` doit contenir la commande de démarrage de votre application Python. Dans la commande suivante, *nom_de_votre_app* est le nom de votre application Python et *PORT* est le numéro de port que votre application Python doit utiliser pour recevoir les demandes des utilisateurs de l'application. *$PORT* est facultatif. Si vous ne spécifiez pas PORT dans la commande de démarrage, le numéro de port qui figure dans la variable d'environnement `VCAP_APP_PORT` dans l'application est utilisé.
	```
	web: python <yourappname>.py $PORT
	```

Vous pouvez à présent importer la bibliothèque Python tierce dans {{site.data.keyword.Bluemix_notm}}.
