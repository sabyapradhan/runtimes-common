---

copyright:
  years: 2015, 2018
lastupdated: "2018-10-09"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}

# Gestion des applications Liberty et Node.js
{: #app_management}


App Management est un ensemble d'utilitaires de développement et de débogage qui peuvent être activés pour les applications Liberty dans {{site.data.keyword.Bluemix}}.
{:shortdesc}

## Utilitaires App Management
{: #Utilities}

### Utilitaires Liberty
* [proxy](#proxy)
* [noproxy](#noproxy)
* [devconsole](#devconsole)
* [hc](#hc)
* [shell](#shell)
* [debug](#debug)
* [jmx](#jmx)
* [localjmx](#localjmx)

### Utilitaires Node.js (obsolètes)

**Remarque sur l'obsolescence** : Les utilitaires App Management sont dépréciés pour les applications Node.js. Certains utilitaires auparavant disponibles pour les applications Node.JS et Liberty le sont désormais uniquement pour les applications Liberty. Ils sont dépréciés pour les applications Node.js.

* [proxy](#proxy)
* [noproxy](#noproxy)
* [devconsole](#devconsole)
* [hc](#hc)
* [shell](#shell)
* [inspector](#inspector)
* [trace](#trace)

## Configuration d'App Management
{: #configure}

Pour activer les utilitaires de gestion des applications (App Management), faites pointer la variable d'environnement *BLUEMIX_APP_MGMT_ENABLE* sur l'utilitaire ou la liste d'utilitaires que vous souhaitez activer, puis reconstituez votre application. Vous pouvez activer plusieurs utilitaires en les séparant par un **+**.

Par exemple, pour activer les utilitaires *hc*, *debug* et *trace*, exécutez la commande suivante :

```
ibmcloud cf set-env myApp BLUEMIX_APP_MGMT_ENABLE hc+debug+trace
```
{: codeblock}

Reconstituez votre application après avoir défini la variable d'environnement :

```
ibmcloud cf restage myApp
```
{: codeblock}

Si vous ne souhaitez pas que les utilitaires App Management soient installés avec votre application, mettez à 'false' la variable d'environnement *BLUEMIX_APP_MGMT_INSTALL* et reconstituez votre application.

Par exemple, exécutez les commandes suivantes pour passer votre application en préproduction sans les utilitaires App Management :

```
ibmcloud cf set-env myApp BLUEMIX_APP_MGMT_INSTALL false
ibmcloud cf restage myApp
```
{: codeblock}

## Restrictions
{: #restrictions}
* Les changements que vous apportez à votre application avec les utilitaires App Management sont transitoires. Ils sont perdus lorsque vous quittez ce mode. Ce mode ne doit être utilisé que temporairement, en phase de développement. Pour des raisons de performances, il n'est pas prévu pour une utilisation en environnement de production.
* Pour des applications Node.js, la plupart des utilitaires de gestion d'application ne fonctionnent pas si vous définissez votre commande **start** dans le fichier `manifest.yml` ou avec l'option `-c` sur la ligne de commande. Ces méthodes sont des dérogations aux packs de construction et représentent des anti-modèles pour le démarrage d'applications Node.js. Pour de meilleurs résultats, définissez la commande **start** dans le fichier `package.json` ou dans `Procfile`.


### Utilitaires Liberty
{: #liberty_utilities}

#### jmx
{: #jmx}

L'utilitaire *jmx* active le connecteur REST JMX afin de permettre à un client JMX distant de gérer l'application en utilisant les données d'identification {{site.data.keyword.Bluemix_notm}}.

Avec JMX, vous pouvez surveiller plusieurs instances d'une application, mais il vous faut dans ce cas une connexion JMX par instance. Par défaut, c'est l'instance 0 qui est surveillée. Pour surveiller l'instance 1, vous pouvez utiliser le fragment de code suivant :

```
ibmcloud cf ssh -i 1 -N -T -L 5000:127.0.0.1:5001
```
{: codeblock}

Pour plus d'informations sur la configuration d'un connecteur JMX, voir [Configuring secure JMX connection to the Liberty profile ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www-01.ibm.com/support/knowledgecenter/was_beta_liberty/com.ibm.websphere.wlp.nd.multiplatform.doc/ae/twlp_admin_restconnector.html){:new_window}.

**Important :** L'utilitaire *jmx* ne démarre pas *proxy*.

#### localjmx
{: #localjmx}

L'utilitaire *localjmx* active la fonction Liberty [localConnector-1.0 ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://www.ibm.com/support/knowledgecenter/SSEQTP_liberty/com.ibm.websphere.wlp.doc/ae/rwlp_feature_localConnector-1.0.html){:new_window}. Combiné au réacheminement de port local, *localjmx* agit comme un autre moyen de permettre à un client JMX distant de gérer l'application.


**Avant de commencer** : *localjmx* nécessite l'installation de JConsole.

L'utilitaire *localjmx* est applicable uniquement pour les applications qui s'exécutent dans une cellule Diego. Pour utiliser *localjmx*, établissez d'abord l'acheminement de port à l'aide de la commande `ibmcloud cf ssh`. Par exemple :

```
ibmcloud cf ssh -N -T -L 5000:127.0.0.1:5000 <appName>
```
{: codeblock}

Ensuite, pour vous connecter avec JConsole, choisissez **Remote Process** (processus distant), spécifiez `127.0.0.1:5000` et utilisez une connexion non sûre.

#### proxy (obsolète pour Node.js)
{: #proxy}

L'utilitaire *proxy* fournit l'outillage minimum de gestion de la communication entre votre application et {{site.data.keyword.Bluemix_notm}}.

Lorsque cet utilitaire est activé, le pack de construction démarre un agent proxy qui se trouve entre le contexte d'exécution de votre application et le conteneur.  L'utilitaire *proxy* s'occupe de toutes les demandes que l'application reçoit. Selon le type de demande, il exécute une action de gestion d'application ou transfère la demande à votre application. Avec l'utilitaire *proxy*, le conteneur de votre application continue à vivre même en cas de plantage de celle-ci. L'agent proxy autorise aussi les mises à jour incrémentielles des fichiers, ce qui active le mode *Live Edit* pour les applications Node.js.

Certains utilitaires App Management imposent l'utilisation de l'utilitaire *proxy* avec votre application, auquel cas ils le démarrent automatiquement**.

**Remarque sur l'obsolescence** : Pour Node.js, la version {{site.data.keyword.Bluemix_notm}} de Cloud Foundry s'exécute dans Diego Cells. La fonction *noproxy* est destinée aux versions non-Diego Cells.

#### noproxy (obsolète pour Node.js)
{: #noproxy}

L'utilitaire *noproxy* désactive l'utilitaire *proxy* lorsque celui-ci est démarré automatiquement par un autre utilitaire.  Le proxy n'est pas nécessaire avec Diego car ce dernier permet d'exécuter directement *ssh* sur votre application et de configurer l'acheminement de port.

L'utilitaire *noproxy* s'applique uniquement aux applications fonctionnant dans une cellule Diego.

**Remarque sur l'obsolescence** : L'utilitaire *noproxy* désactive *proxy*. *proxy* étant déprécia pour Node.js et ne fonctionnant plus, *noproxy* n'est pas nécessaire.

#### devconsole (obsolète pour Node.js)
{: #devconsole}

Les utilisateurs peuvent redémarrer, arrêter ou suspendre leurs applications avec l'utilitaire de console de développement (*devconsole*). Ils peuvent aussi activer les utilitaires shell et inspector ou y accéder en utilisant *devconsole*.  L'URL d'accès à *devconsole* est la suivante :
```
  https://<nom_votre_app>.mybluemix.net/bluemix-debug/manage
```
{: codeblock}

Dans le cas de Node version 6.3.0 ou ultérieure, la console de développement fournit un bouton de redémarrage de votre application ainsi qu'un accès à l'utilitaire *shell*.  Pour plus d'informations, consultez la section relative à *inspector*.

**Important :** L'utilitaire *devconsole* démarre *proxy*.

**Remarque sur l'obsolescence** : Plutôt que d'avoir recours à l'utilitaire *devconsole* pour Node.js, utilisez la console {{site.data.keyword.Bluemix_notm}}. Depuis la console, accédez à la page **Exécution** de l'application. Depuis la page **Exécution**, vous pouvez arrêter, démarrer, renommer et supprimer l'application. Vous pouvez également accéder à l'interpréteur de commandes ainsi qu'à d'autres informations.

#### hc (obsolète pour Node.js)
{: #hc}

L'agent Health Center (*hc*) permet à votre application d'être surveillée par le client Health Center.  Dans le cas de Node.js, l'agent *hc* n'est disponible que pour les versions de l'exécution Node.js incluses avec le pack de construction IBM SDK for Node.js.  Voir [Dernières mises à jour du pack de construction sdk-for-nodejs](/docs/runtimes/nodejs/updates.html) pour le jeu d'environnements d'exécution actuels.

Lorsque vous activez l'agent Health Center, vous pouvez l'utiliser pour analyser les performances de vos applications Liberty et Node.js à l'aide de l'ensemble d'outils IBM Monitoring and Diagnostic Tools. Pour plus d'informations, voir [How to analyze the performance of Liberty Java or Node.js apps in {{site.data.keyword.Bluemix_notm}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/bluemix/2015/07/03/how-to-analyze-performance-in-bluemix/){:new_window}.

**Important :** L'utilitaire *hc* démarre *proxy*.

**Utilisation de *hc* avec *noproxy* **

L'utilitaire *hc* peut être utilisé conjointement avec *noproxy*. Pour utiliser Health Center avec *noproxy*, établissez d'abord l'acheminement de port à l'aide de la commande `ibmcloud cf ssh`. Par exemple :

```
ibmcloud cf ssh -N -T -L 1883:127.0.0.1:1883 <appName>
```
{: codeblock}

Ensuite, pour vous connecter avec le client Health Center, utilisez une [connexion MQTT ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://www.ibm.com/support/knowledgecenter/SS3KLZ/com.ibm.java.diagnostics.healthcenter.doc/topics/connectingtojvm.html){: new_window} et spécifiez l'hôte `127.0.0.1` et le port `1883`.

**Remarque sur l'obsolescence** : Plutôt que d'utiliser l'agent Health Center pour Node.js, utilisez la console {{site.data.keyword.Bluemix_notm}}. Depuis la console, accédez à la page **Exécution** de l'application, où vous pourrez accéder à l'utilisation d'UC, l'utilisation de la mémoire et l'espace disque. La page comporte également un onglet permettant d'accéder aux variables d'environnement.

#### shell (obsolète pour Node.js)
{: #shell}

L'utilitaire *shell* active un shell basé sur le Web.  Vous pouvez accéder au *shell* depuis l'utilitaire *devconsole* ou avec l'URL suivante :

```
  https://<nom_votre_app>.mybluemix.net/bluemix-debug/shell
```
{: codeblock}

Une fois l'accès à l'utilitaire *shell* effectif, une fenêtre de terminal s'affiche avec un accès d'interpréteur de commande dans votre application. Vous pouvez effectuer toutes les opérations habituellement possibles dans un interpréteur de commande classique, par exemple éditer des fichiers, vérifier l'utilisation de la mémoire ou exécuter des commandes de diagnostic.

**Important :** L'utilitaire *shell* démarre aussi *proxy*.

Diego fournit un shell interactif via la commande `ibmcloud cf ssh`, de sorte que l'utilitaire *shell* n'est utile que pour les applications qui s'exécutent sur un agent DEA.
{: .tip}

**Remarque sur l'obsolescence** : Pour Node.js, si la commande `cf ssh` est toujours fonctionnelle, vous avez également la possibilité d'accéder à l'interpréteur de commandes depuis la console {{site.data.keyword.Bluemix_notm}}. Depuis la console, accédez à la page **Exécution** de l'application, où vous trouverez un onglet pour utiliser un interpréteur de commandes dans la page Web.

### Utilitaires Node.js (obsolètes)
{: #node_utilities}

#### inspector (obsolète)
{: #inspector}

L'utilitaire inspector peut servir à créer des profils d'utilisation d'UC, à ajouter des points d'arrêt et à déboguer le code, et tout cela pendant que votre application continue à fonctionner sur {{site.data.keyword.cloud_notm}}.  Pour les versions de Node.js antérieures à la 6.3.0, l'utilitaire active l'interface de débogage Node inspector.  Pour plus d'informations sur Node inspector, consultez le Readme pour [node-inspector sur GitHub ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/node-inspector/node-inspector){: new_window}.  Pour les versions 6.3.0 et supérieures de Node.js, l'utilitaire *inspector* utilise [V8 Inspector Integration for Node.js ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://nodejs.org/dist/latest-v6.x/docs/api/debugger.html#debugger_v8_inspector_integration_for_node_js){:new_window}.

##### Pour les versions de Node.js postérieures à la version 6.3.0
Lorsque vous démarrez le mode débogage, *proxy* est automatiquement activé, même si vous utilisez une version de Node.js qui n'inclut pas *proxy*. Les versions de Node.js postérieures à la 6.3.0 n'incluent pas *proxy*. Si vous utilisez l'utilitaire *inspector* avec une version de Node.js postérieure à la 6.3.0, vous pouvez désactiver à nouveau *proxy* en utilisant *noproxy.*

Au lieu d'utiliser *proxy* pour accéder à l'interface *inspector*, vous utilisez la capacité Developer Tools du navigateur web Google Chrome.  

Activez l'accès à l'URL avec le réacheminement de port local avec la commande suivante :
```
ibmcloud cf ssh -N -T -L 9229:127.0.0.1:9229 <appName>
```
{: codeblock}

Obtenez le journal de démarrage pour l'application en utilisant la commande suivante.
```
ibmcloud cf logs <appName> --recent
```
{: codeblock}

Si l'utilitaire *inspector* est actif, le journal contiendra des messages similaires aux suivants :
```
 ... You will need a SSH tunnel for port 9229 to be able to use the Chrome DevTools to remotely debug your app
 ... Starting app with 'node --inspect=9229  app.js '
```
{: codeblock}

Utilisez une version à jour du navigateur web Google Chrome pour accéder à `chrome://inspect`.
A partir de cette URL, vous verrez votre application listée avec un lien à ses fichiers tels que `file://home/vcap/app/app.js`. Sélectionnez **inspect** pour accéder à l'interface inspect.

##### Pour les versions de Node.js antérieures à la version 6.3.0
Si vous utilisez le *proxy,* vous pouvez accéder à l'interface *inspector* sur `https://myApp.mybluemix.net/bluemix-debug/inspector`.

Si vous n'utilisez pas l'utilitaire *proxy*, activez l'accès à l'URL de l'application en utilisant le réacheminement de port local avec la commande suivante :

```
ibmcloud cf ssh -N -T -L 8790:127.0.0.1:8790 <appName>
```
{: codeblock}

Accédez ensuite à l'inspecteur à partir de l'URL `http://127.0.0.1:8790`.

**Remarque sur l'obsolescence** : Vous pouvez toujours utiliser la fonction *inspector* en modifiant légèrement votre configuration. Dans le fichier `manifest.yml` de votre application, définissez la commande de démarrage sur 'node --inspect=9229 app.js'. Suivez ensuite les instructions pour les versions Node.js postérieures à la 6.3.0. Cependant, les journaux indiquant habituellement quand inspector est actif ne sont pas visibles, bien qu'il fonctionne correctement.


#### trace (obsolète)
{: #trace}

L'utilitaire trace vous permet de définir dynamiquement des niveaux de trace si votre application utilise les modules de journalisation log4js, ibmbluemix ou bunyan.

Naviguez jusqu'à la page **Détails de l'instance** dans la console web {{site.data.keyword.cloud_notm}} et sélectionnez **Actions** pour voir l'interface utilisateur.

Remarque : les versions de dépendance prises en charge sont :

* log4js : (0.6.0 à 0.6.24)
* bunyan : (1.0.0, 1.0.1, 1.1.0 à 1.1.3, 1.2.0 à 1.2.3, 1.3.0 à 1.3.5)
* ibmbluemix : (1.0.0-20140707-1250) à (1.0.0-20150409-1328)


L'utilitaire trace n'est pas disponible lorsque l'application est démarrée avec l'option `-b buildpack`.

L'utilitaire trace ne démarre pas *proxy*.
