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

# Usando buildpacks da comunidade
{: #using_buildpacks}

Se não for possível localizar um iniciador no catálogo do {{site.data.keyword.Bluemix}} que forneça o tempo de execução desejado, será possível trazer um buildpack externo para o {{site.data.keyword.Bluemix_notm}}. É possível especificar um buildpack compatível com o Cloud Foundry customizado ao implementar o seu aplicativo usando o
comando `ibmcloud cf push`.
{:shortdesc}

São fornecidos buildpacks externos pela comunidade do Cloud Foundry para
utilização como seus próprios buildpacks. Antes de implementar o seu aplicativo no {{site.data.keyword.Bluemix_notm}},
certifique-se de instalar a interface da linha de comandos do {{site.data.keyword.Bluemix_notm}}.

**Nota:** os buildpacks externos não são fornecidos pela IBM. Entre em contato com a comunidade do Cloud Foundry para suporte.

## Buildpacks integrados da comunidade

No {{site.data.keyword.Bluemix_notm}},
é possível usar os buildpacks integrados que são fornecidos pela comunidade do
Cloud Foundry. Para ver os buildpacks integrados da comunidade, execute o comando `ibmcloud cf buildpacks`:

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


Para obter o mesmo tempo de execução ou estrutura, os buildpacks criados pela IBM têm precedência sobre os da comunidade. Se
você deseja usar o buildpack da comunidade para sobrescrever o buildpack criado pela IBM, deve-se especificar o buildpack usando a
opção `-b` com o comando `ibmcloud cf push`.

Por exemplo, é possível usar o buildpack da comunidade para aplicativos da web Java™ usando o comando a seguir.

```
ibmcloud cf push app_name -b java_buildpack -p app_path
```
{:pre}

Também é possível usar o buildpack da comunidade para o aplicativo Node.js com o comando a seguir.

```
ibmcloud cf push app_name -b nodejs_buildpack -p app_path
```
{: codeblock}

Para um tempo de execução ou estrutura que não é suportado pelos buildpacks criados pela IBM, mas é suportado por buildpacks
integrados da comunidade, não é necessário usar a opção `-b` com o comando `ibmcloud cf push`. Por
exemplo, para aplicativos Ruby, não há buildpacks criados pela IBM. É possível usar o buildpack integrado da comunidade inserindo o comando a seguir.

```
ibmcloud cf push app_name -p app_path
```
{: codeblock}

## Buildpacks externos

É possível usar buildpacks externos ou customizados no {{site.data.keyword.Bluemix_notm}}. Deve-se especificar a
URL do buildpack com a opção `-b` e especificar a pilha com a opção `-s` no comando `ibmcloud cf push`. Por exemplo, para usar um buildpack de comunidade externa para arquivos estáticos, execute o comando a seguir.

```
ibmcloud cf push app_name -p app_path -b https://github.com/cloudfoundry/staticfile-buildpack.git
```
{: codeblock}

Se você não desejar usar o buildpack integrado da comunidade para aplicativos Ruby, será possível usar um buildpack
externo inserindo o comando a seguir.

```
ibmcloud cf push app_name -p app_path -b https://github.com/cloudfoundry/ruby-buildpack.git
```
{: codeblock}

Também é
possível usar um buildpack customizado para seu aplicativo. Por exemplo, é possível usar um buildpack PHP de software livre que é
fornecido pela comunidade do Cloud Foundry. Ao implementar o seu aplicativo PHP no {{site.data.keyword.Bluemix_notm}}, insira o comando a seguir para especificar a URL do repositório Git do buildpack.

```
ibmcloud cf push app_name -p app_path -b https://github.com/cloudfoundry/php-buildpack.git
```
{:pre}

Também é possível editar o arquivo `manifest.yml` em seu projeto para incluir uma linha `buildpack`.

```
buildpack: https://github.com/cloudfoundry/python-buildpack.git
```
{: codeblock}


## Especificando a versão do buildpack Java

Para especificar uma versão do buildpack Java, use o comando `ibmcloud cf set-env`. Por exemplo, insira o comando a seguir para configurar a versão Java para 1.7.0.

```
ibmcloud cf set-env app_name JBP_CONFIG_OPEN_JDK_JRE &apos;{jre: { version: 1.7.0_+ }}&apos;
```
{: codeblock}

Em seguida, remonte o aplicativo para tornar a mudança efetiva.

```
ibmcloud cf restage app_name
```
{: codeblock}

## Use o arquivo  ` manifest.yml ` .

É possível incluir a variável de ambiente e o valor que você deseja especificar diretamente para o arquivo `manifest.yml`. Consulte  [ Variáveis de ambiente ](https://docs.cloudfoundry.org/devguide/deploy-apps/manifest.html#env-block).
