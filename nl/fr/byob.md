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

# Utilisation de packs de construction de communauté
{: #using_buildpacks}

Si vous ne trouvez pas, dans le catalogue {{site.data.keyword.Bluemix}}, de module de démarrage qui offre le contexte d'exécution dont vous avez besoin, vous pouvez utiliser un pack de construction externe dans {{site.data.keyword.Bluemix_notm}}. Vous pouvez spécifier un pack de construction personnalisé, compatible Cloud Foundry, lorsque vous déployez votre application via la commande `ibmcloud cf push`.
{:shortdesc}

Des packs de construction externes sont fournis par la communauté Cloud Foundry ; vous pouvez les utiliser comme vos propres packs de construction. Avant de déployer votre application sur {{site.data.keyword.Bluemix_notm}}, veillez à installer l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}.

**Remarque :** Les packs de construction externes ne sont pas fournis par IBM. Contactez la communauté Cloud Foundry pour une prise en charge.

## Packs de construction de communauté intégrés

Dans {{site.data.keyword.Bluemix_notm}}, vous pouvez utiliser des packs de construction intégrés fournis par la communauté Cloud Foundry. Pour afficher ces packs, exécutez la commande `ibmcloud cf buildpacks` :

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


Dans le cas d'un contexte d'exécution ou d'une infrastructure identique, les packs de construction créés par IBM sont prioritaires sur ceux de la communauté. Si vous souhaitez utiliser un pack de construction de la communauté à la place de celui créé par IBM, vous devez spécifier ce pack de construction en utilisant l'option `-b` avec la commande `ibmcloud cf push`.

Vous pouvez, par exemple, utiliser le pack de construction de la communauté pour les applications Web Java™ en utilisant la commande suivante.

```
ibmcloud cf push app_name -b java_buildpack -p app_path
```
{:pre}

Vous pouvez également utiliser le pack de construction de la communauté pour l'application Node.js en utilisant la commande suivante.

```
ibmcloud cf push app_name -b nodejs_buildpack -p app_path
```
{: codeblock}

Dans le cas d'un contexte d'exécution ou d'une infrastructure non pris en charge par des packs de construction créés par IBM mais par des packs de construction intégrés de la communauté, vous n'avez pas besoin d'utiliser l'option `-b` avec la commande `ibmcloud cf push`. Ainsi, pour des applications Ruby, il n'existe pas de pack de construction créé par IBM. Vous pouvez utiliser le pack intégré de la communauté en entrant la commande suivante.

```
ibmcloud cf push app_name -p app_path
```
{: codeblock}

## Packs de construction externes

Vous pouvez utiliser des packs de construction externes ou personnalisés dans {{site.data.keyword.Bluemix_notm}}. Vous devez spécifier l'URL du pack de construction avec l'option `-b`, ainsi que la pile avec l'option `-s` pour la commande `ibmcloud cf push`. Par exemple, pour utiliser un pack de construction de communauté externe, exécutez la commande suivante.

```
ibmcloud cf push app_name -p app_path -b https://github.com/cloudfoundry/staticfile-buildpack.git
```
{: codeblock}

Si vous ne souhaitez pas utiliser le pack intégré de la communauté pour des applications Ruby, vous pouvez utiliser un pack externe en entrant la commande suivante.

```
ibmcloud cf push app_name -p app_path -b https://github.com/cloudfoundry/ruby-buildpack.git
```
{: codeblock}

Vous pouvez également utiliser un pack de construction personnalisé pour votre application. Vous pouvez, par exemple, utiliser un pack de construction PHP open source qui est fourni par la communauté Cloud Foundry. Lorsque vous déployez votre application PHP sur {{site.data.keyword.Bluemix_notm}}, entrez la commande suivante pour spécifier l'URL du référentiel Git pour le pack de construction.

```
ibmcloud cf push app_name -p app_path -b https://github.com/cloudfoundry/php-buildpack.git
```
{:pre}

Vous pouvez également éditer le fichier `manifest.yml` de votre projet pour ajouter la ligne `buildpack`.

```
buildpack: https://github.com/cloudfoundry/python-buildpack.git
```
{: codeblock}


## Spécification de la version de pack de construction Java

Pour spécifier une version de pack de construction Java, utilisez la commande `ibmcloud cf set-env`. Entrez, par exemple, la commande suivante pour définir la version Java sur 1.7.0.

```
ibmcloud cf set-env app_name JBP_CONFIG_OPEN_JDK_JRE &apos;{jre: { version: 1.7.0_+ }}&apos;
```
{: codeblock}

Puis reconstituez en préproduction votre application afin que les changements prennent effet.

```
ibmcloud cf restage app_name
```
{: codeblock}

## Utilisation du fichier `manifest.yml`

Vous pouvez ajouter la variable d'environnement et la valeur à spécifier directement dans le fichier `manifest.yml`. Voir [Variables d'environnement](https://docs.cloudfoundry.org/devguide/deploy-apps/manifest.html#env-block).
