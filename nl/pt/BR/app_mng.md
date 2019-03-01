---

copyright:
  years: 2015, 2018
lastupdated: "2018-10-09"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}

# Gerenciando apps Liberty e Node.js
{: #app_management}


O App Management é um conjunto de utilitários de desenvolvimento e depuração que podem ser ativados para os aplicativos Liberty no {{site.data.keyword.Bluemix}}.
{:shortdesc}

## Utilitários do App Management
{: #Utilities}

### Utilitários do Liberty
* [proxy](#proxy)
* [noproxy](#noproxy)
* [devconsole](#devconsole)
* [hc](#hc)
* [shell](#shell)
* [depuração](#debug)
* [jmx](#jmx)
* [localjmx](#localjmx)

### Utilitários Node.js (descontinuados)

**Nota de descontinuação**: os utilitários do App Management foram descontinuados para os aplicativos Node.js. Alguns utilitários que estavam disponíveis para os aplicativos Node.JS e Liberty agora estão disponíveis apenas para os aplicativos Liberty. Esses utilitários foram descontinuados para os aplicativos Node.js.

* [proxy](#proxy)
* [noproxy](#noproxy)
* [devconsole](#devconsole)
* [hc](#hc)
* [shell](#shell)
* [inspetor](#inspector)
* [rastreio](#trace)

## Como configurar o App Management
{: #configure}

Para ativar os utilitários do App Management, configure o valor da variável de ambiente *BLUEMIX_APP_MGMT_ENABLE* para o utilitário ou lista de utilitários que você deseja ativar e, em seguida, remonte seu aplicativo. É possível ativar múltiplos utilitários separando-os com um **+**.

Por exemplo, para ativar os utilitários *hc*, de *depuração* e de *rastreio*, execute o comando a seguir:

```
ibmcloud cf set-env myApp BLUEMIX_APP_MGMT_ENABLE hc + debug + trace
```
{: codeblock}

Remonte o seu aplicativo após você configurar a variável de ambiente:

```
ibmcloud cf restage myApp
```
{: codeblock}

Se não deseja que os utilitários do App Management sejam instalados com seu aplicativo, configure a variável de ambiente *BLUEMIX_APP_MGMT_INSTALL* para 'false' e remonte seu aplicativo.

Por exemplo, execute os seguintes comandos para montar seu aplicativo sem utilitários do App Management:

```
ibmcloud cf set-env myApp BLUEMIX_APP_MGMT_INSTALL false
ibmcloud cf restage myApp
```
{: codeblock}

## Restrictions
{: #restrictions}
* As mudanças feitas em seu aplicativo usando App Management são temporárias e são perdidas após você sair deste modo. Esse modo é apenas para uso de desenvolvimento provisório e não deve ser usado como um ambiente de produção devido ao desempenho.
* Para aplicativos Node.js, a maioria dos utilitários do App Management não funcionará se você configurar o comando **start** no arquivo `manifest.yml` ou com a opção `-c` na linha de comandos. Esses métodos são substituições de buildpack e são antipadrões para iniciar aplicativos Node.js. Para obter melhores resultados, configure o comando **start** no arquivo `package.json` ou `Procfile`.


### Utilitários do Liberty
{: #liberty_utilities}

#### jmx
{: #jmx}

O utilitário *jmx* ativa o Conector de REST do JMX para permitir que um cliente JMX remoto gerencie o aplicativo usando credenciais do usuário do {{site.data.keyword.Bluemix_notm}}.

É possível monitorar múltiplas instâncias de um aplicativo usando JMX, mas isso requer uma conexão JMX separada para cada instância. O padrão é monitorar a instância 0. Para monitorar a instância 1, é possível usar o fragmento de código a seguir:

```
ibmcloud cf ssh -i 1 -N -T -L 5000:127.0.0.1:5001
```
{: codeblock}

Para obter mais informações sobre como configurar um conector JMX, veja [Configurando a conexão JMX segura com o perfil Liberty ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www-01.ibm.com/support/knowledgecenter/was_beta_liberty/com.ibm.websphere.wlp.nd.multiplatform.doc/ae/twlp_admin_restconnector.html){:new_window}.

**Importante**: o utilitário *jmx* não inicia o *proxy*.

#### localjmx
{: #localjmx}

O utilitário *localjmx* ativa o recurso Liberty [localConnector-1.0 ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](http://www.ibm.com/support/knowledgecenter/SSEQTP_liberty/com.ibm.websphere.wlp.doc/ae/rwlp_feature_localConnector-1.0.html){:new_window}. Combinado com o encaminhamento de porta local, o *localjmx* age como uma maneira alternativa de permitir que um cliente JMX remoto gerencie o aplicativo.


**Antes de iniciar**: *localjmx* requer que você instale o JConsole.

O utilitário *localjmx* só é aplicável a aplicativos em execução em uma célula do Diego. Para usar *localjmx*, estabeleça primeiro o encaminhamento de porta usando o comando `ibmcloud cf ssh`. Por exemplo:

```
ibmcloud cf ssh -N -T -L 5000:127.0.0.1:5000 <appName>
```
{: codeblock}

Em seguida, para se conectar ao JConsole, escolha **Processo remoto**, especifique `127.0.0.1:5000` e use uma conexão não segura.

#### proxy (descontinuado para o Node.js)
{: #proxy}

O utilitário *proxy* fornece gerenciamento de aplicativo mínimo entre o seu aplicativo e {{site.data.keyword.Bluemix_notm}}.

Quando ativado, o buildpack inicia um agente de proxy que está localizado entre o tempo de execução e o contêiner do aplicativo.  O utilitário *proxy* manipula todas as solicitações recebidas pelo aplicativo. Com base no tipo de solicitação, uma ação do App Management é tomada ou a solicitação é encaminhada para seu aplicativo. Usando o *proxy*, seu contêiner de aplicativo continuará em tempo real mesmo se o aplicativo travar. O agente de proxy também permite atualizações incrementais de arquivo, o que ativa o modo de *Edição em tempo real* para aplicativos Node.js.

Alguns utilitários do App Management requerem o uso do utilitário de *proxy* com seu aplicativo e podem iniciar o *proxy* automaticamente.

**Nota de descontinuação**: para o Node.js, a versão do {{site.data.keyword.Bluemix_notm}} do Cloud Foundry executa em células do Diego. O recurso *noproxy* é para células não do Diego.

#### noproxy (descontinuado para o Node.js)
{: #noproxy}

O utilitário *noproxy* desativa o utilitário *proxy* quando ele é iniciado automaticamente por outro utilitário.  Com o Diego, o proxy não é necessário porque o Diego fornece a capacidade de executar *ssh* diretamente em seu aplicativo e configurar o encaminhamento de porta.

O utilitário *noproxy* se aplica apenas a aplicativos que são executados em uma célula do Diego.

**Nota de descontinuação**: o utilitário *noproxy* desativa o *proxy*. Como o *proxy* foi descontinuado para o Node.js e não funciona mais, o *noproxy* não é necessário.

#### devconsole (descontinuado para o Node.js)
{: #devconsole}

Os usuários podem reiniciar, parar ou suspender seus aplicativos com o utilitário do console de desenvolvimento (*devconsole*). Os usuários também podem ativar ou acessar os utilitários shell e inspector usando o *devconsole*.  É possível usar a URL a seguir para acessar o *devconsole*:s
```
  https://<yourappname>.mybluemix.net/bluemix-debug/manage
```
{: codeblock}

Para o Node versão 6.3.0 ou mais recente, o console de desenvolvimento fornece um botão de reinicialização para o seu aplicativo e acesso ao utilitário *shell*.  Consulte a discussão do *inspetor* para obter mais informações.

**Importante**: o utilitário *devconsole* inicia o *proxy*.

**Nota de descontinuação**: em vez de usar o utilitário *devconsole* para o Node.js, use o console do {{site.data.keyword.Bluemix_notm}}. No console, navegue até a página **Tempo de execução** do aplicativo. Na página **Tempo de execução**, é possível parar, iniciar, renomear e excluir o aplicativo. Também é possível acessar o shell e outras informações.

#### hc (descontinuado para o Node.js)
{: #hc}

O agente do Health Center (*hc*) permite que o seu aplicativo seja monitorado pelo cliente do Health Center.  Para Node.js, o agente *hc* está disponível apenas com as versões de tempo de runtime do Node.js incluído com o IBM SDK para o buildpack Node.js.  Veja [Atualizações mais recentes para o buildpack sdk-for-nodejs](/docs/runtimes/nodejs/updates.html) para obter o conjunto atual de tempos de execução.

Quando você tiver o agente do Health Center ativado, será possível analisar o desempenho de seus aplicativos Liberty e Node.js usando o IBM Monitoring and Diagnostic Tools. Para obter mais informações, veja [Como analisar o desempenho de apps Liberty Java ou Node.js no {{site.data.keyword.Bluemix_notm}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://developer.ibm.com/bluemix/2015/07/03/how-to-analyze-performance-in-bluemix/){:new_window}.

**Importante:** o utilitário *hc* inicia o *proxy*.

**Usando *hc* com *noproxy* **

O utilitário *hc* pode ser usado em conjunção com *noproxy*. Para usar o Health Center com *noproxy*, primeiro estabeleça o encaminhamento de porta usando o comando `ibmcloud cf ssh`. Por exemplo:

```
ibmcloud cf ssh -N -T -L 1883:127.0.0.1:1883 <appName>
```
{: codeblock}

Em seguida, para se conectar ao cliente Health Center, use uma [conexão MQTT ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](http://www.ibm.com/support/knowledgecenter/SS3KLZ/com.ibm.java.diagnostics.healthcenter.doc/topics/connectingtojvm.html){: new_window} e especifique o host como `127.0.0.1` e a porta como `1883`.

**Nota de descontinuação:** em vez de usar o agente do Health Center para o Node.js, use o console do {{site.data.keyword.Bluemix_notm}}. No console, navegue até a página **Tempo de execução** do aplicativo, na qual é possível acessar o uso de CPU, o uso de memória e o espaço em disco. A página também tem uma guia na qual é possível acessar qualquer variável de ambiente.

#### shell (descontinuado para o Node.js)
{: #shell}

O utilitário *shell* ativa o shell baseado na web.  É possível acessar o *shell* do utilitário *devconsole* ou com a URL a seguir:

```
  https://<yourappname>.mybluemix.net/bluemix-debug/shell
```
{: codeblock}

Depois de acessar o utilitário *shell*, uma janela do terminal é exibida com acesso de shell em seu aplicativo. É possível fazer tudo o que for suportado em um shell regular, como editar arquivos, verificar o uso de memória ou executar comandos de diagnóstico.

**Importante:** o utilitário *shell* também inicia o *proxy*.

O Diego fornece um shell interativo por meio do comando ` bmcloud cf ssh`, portanto, o utilitário *shell* é útil apenas para aplicativos em execução em um DEA.
{: .tip}

**Nota de descontinuação:** para o Node.js, enquanto o comando `cf ssh` continua a funcionar, também é possível acessar o shell por meio do console do {{site.data.keyword.Bluemix_notm}}. No console, navegue até a página **Tempo de execução** do aplicativo, na qual é possível localizar uma guia para puxar um shell na página da web.

### Utilitários Node.js (descontinuados)
{: #node_utilities}

#### inspector (descontinuado)
{: #inspector}

O utilitário inspector pode ser usado para criar perfis de uso da CPU, incluir pontos de interrupção e depurar código, tudo enquanto seu aplicativo é executado no {{site.data.keyword.cloud_notm}}.  Para versões do Node.js antes da 6.3.0, o inspector ativa a interface do depurador do Node Inspector.  Para obter mais informações sobre o Node Inspector, consulte o leia-me para [node-inspector no GitHub ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/node-inspector/node-inspector){: new_window}.  Para o Node.js versões 6.3.0 e mais recentes, o utilitário *inspector* usa o [V8 Inspector Integration for Node.js ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://nodejs.org/dist/latest-v6.x/docs/api/debugger.html#debugger_v8_inspector_integration_for_node_js){:new_window}.

##### Para as versões do Node.js após a 6.3.0
Ao iniciar o modo de depuração, o *proxy* é automaticamente ativado, mesmo quando você usa uma versão do
Node.js que não inclui o *proxy*. Versões do Node.js após a 6.3.0 não incluem *proxy.* Se você usar o
utilitário *inspector* com versões do Node.js após a 6.3.0, será possível desligar o *proxy*
novamente usando o *noproxy.*

Em vez de usar o *proxy* para acessar a interface do *inspector*, use o recurso
Ferramentas do desenvolvedor do navegador da web do Google Chrome.  

Ative o acesso à URL com o encaminhamento de porta local com o comando a seguir:
```
ibmcloud cf ssh -N -T -L 9229:127.0.0.1:9229 <appName>
```
{: codeblock}

Obtenha o log de inicialização para o aplicativo usando o comando a seguir.
```
ibmcloud cf logs < appName> -- recentes
```
{: codeblock}

Se o utilitário *inspector* estiver ativo, o log conterá mensagens semelhantes ao seguinte:
```
 ...You will need a SSH tunnel for port 9229 to be able to use the Chrome DevTools to remotely debug your app... Starting app with 'node --inspect=9229  app.js '
```
{: codeblock}

Use uma versão atualizada do navegador da web do Google Chrome para procurar `chrome://inspect`.
Nessa URL, você vê seu aplicativo listado juntamente com um link para seus arquivos do aplicativo como
`file://home/vcap/app/app.js` e, em seguida, selecione **inspect** para acessar a interface
do inspect.

##### Para as versões do Node.js antes da 6.3.0
Se você usar o *proxy,* será possível acessar a interface do *inspector* em
`https://myApp.mybluemix.net/bluemix-debug/inspector`.

Se você não usar o utilitário *proxy*, ative o acesso à URL do aplicativo usando o encaminhamento de
porta local com o comando a seguir:

```
ibmcloud cf ssh -N -T -L 8790:127.0.0.1:8790 <appName>
```
{: codeblock}

Em seguida, acesse o inspector da URL, `http://127.0.0.1:8790`.

**Nota de descontinuação:** ainda é possível usar o recurso *inspector* mudando ligeiramente a sua configuração. No arquivo `manifest.yml` do aplicativo, configure o comando inicial para 'node -- inspect=9229 app.js'. Em seguida, é possível seguir as instruções para as versões do Node.js após a 6.3.0. No entanto, os logs que normalmente indicam quando o inspector está ativo não são visíveis, mas funcionam corretamente.


#### rastreio (descontinuado)
{: #trace}

O utilitário de rastreio permite que você configure dinamicamente os níveis de rastreio se o seu aplicativo estiver usando
log4js, ibmbluemix ou módulos de criação de log bunyan.

Navegue para a página **Detalhes da instância** no {{site.data.keyword.cloud_notm}} console
da web e selecione **Ações** para ver a UI.

Nota: versões de dependência suportadas:

* log4js:(0.6.0 - 0.6.24)
* bunyan: (1.0.0, 1.0.1, 1.1.0 - 1.1.3, 1.2.0 - 1.2.3, 1.3.0 - 1.3.5)
* ibmbluemix: (1.0.0-20140707-1250)-(1.0.0-20150409-1328)


O utilitário de rastreio não está disponível quando o aplicativo é iniciado usando a opção `-b buildpack`.

O utilitário de rastreio não inicia o *proxy*.
