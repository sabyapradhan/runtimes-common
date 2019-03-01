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


# Resolução de Problemas para Tempos de execução
{: #runtimes}

Você pode ter problemas ao usar os tempos de execução do [{{site.data.keyword.Bluemix}} runtimes](index.html). Em muitos casos, é possível recuperar-se desses problemas seguindo algumas etapas simples.
{:shortdesc}

* [ Resolução de Problemas Gerais ](#ts_all)
* [Liberty for Java](#ts_liberty)
* [SDK for Node.js](#ts_nodejs)
* [ASP.NET Core](#ts_dotnet)
* [PHP](#ts_php)
* [Python](#ts_python)

## Resolução de Problemas Gerais
{: #ts_all}

### Buildpack obsoleto usado quando um aplicativo é enviado por push
{: #ts_loading_bp}

É possível que você não consiga usar os componentes de buildpack mais recentes
ao enviar um app por push. É possível usar buildpacks que possuem mecanismos integrados
para evitar o carregamento de componentes obsoletos ou é possível excluir os conteúdos
no diretório de cache do app antes de enviar por push ou remontar o app.

Ao enviar por push ou remontar um app após a atualização do buildpack, os componentes de buildpack mais recentes não são carregados automaticamente. Como resultado, o seu aplicativo usa os componentes de buildpack obsoletos a partir do cache. As atualizações que foram aplicadas ao buildpack desde a última vez que o app foi enviado por push não são implementadas.
{: tsSymptoms}

Alguns buildpacks não são configurados para fazer download automaticamente de todos os componentes atualizados da Internet para assegurar que você sempre use a versão mais recente.
{: tsCauses}

É possível usar buildpacks que tenham mecanismos integrados para evitar o carregamento de componentes obsoletos, por exemplo, os buildpacks a seguir:
{: tsResolve}

  * [Buildpack Java do Cloud Foundry ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/cloudfoundry/java-buildpack){: new_window}. Esse buildpack tem um mecanismo integrado
para assegurar que a versão mais recente do buildpack seja usada. Para obter mais informações sobre como esse mecanismo funciona, veja [extending-caches.md ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/cloudfoundry/java-buildpack/blob/master/docs/extending-caches.md){: new_window}.
  * [Buildpack Node.js do Cloud Foundry ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/cloudfoundry/nodejs-buildpack){: new_window}. Esse buildpack fornece funcionalidade semelhante ao usar variáveis de ambiente. Para ativar o buildpack Node.js para fazer download de módulos de nó da Internet toda vez, digite o comando a seguir na interface da linha de comandos do {{site.data.keyword.Bluemix_notm}}: 	

  ```
  set NODE_MODULES_CACHE=false
  ```

Se o buildpack que você estiver usando não fornecer um mecanismo para carregar os componentes mais recentes automaticamente, será possível excluir manualmente o conteúdo no diretório de cache e enviar por push seu app novamente. Use as etapas a seguir:

 1. Efetue o check-out de uma ramificação de um buildpack nulo, por exemplo, https://github.com/ryandotsmith/null-buildpack. Para obter informações sobre como efetuar check-out de uma ramificação, veja [Git básico - Obtendo um repositório Git ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](http://www.git-scm.com/book/en/v2/Git-Basics-Getting-a-Git-Repository){: new_window}.  
 2. Inclua a linha a seguir no arquivo `null-buildpack/bin/compile`
e confirme as mudanças. Para obter informações sobre como confirmar as mudanças, veja [Git básico - Gravando mudanças no repositório ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](http://www.git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository){: new_window}.
  ```
  rm -rfv $2/*
  ```
 3. Envie seu app por push com o buildpack nulo que foi modificado para excluir
o cache usando o comando a seguir. Depois de concluir essa
etapa, todos os conteúdos no diretório de cache de seu app serão excluídos.
  ```
  ibmcloud cf push appname -p app_path -b <modified_null_buildpack>
  ```
 4. Envie seu app por push com o buildpack mais recente que você deseja usar
usando o comando a seguir:
  ```
  ibmcloud cf push appname -p app_path -b <latest_buildpack>
  ```

### O aplicativo continua a ser reiniciado
{: #ts_apprestart}

O aplicativo continua travando e reiniciado.
{: tsSymptoms}

O ciclo de vida do contêiner de aplicativo, como evacuação e encerramento, pode impactar a funcionalidade de seu aplicativo.  
{: tsCauses}

Identifique qual etapa do ciclo de vida do contêiner de aplicativo está causando erros na implementação do aplicativo. Saiba
mais sobre o [ ciclo de vida do
aplicativo Cloud Foundry](https://docs.cloudfoundry.org/devguide/deploy-apps/app-lifecycle.html#evacuation).
{: tsResolve}

### O botão Ações na página Detalhes da instância está desativado (descontinuado)
  {: #ts_actionsbutton}

  O botão Ações na página Detalhes da instância está desativado.
  {: tsSymptoms}

  Esse problema ocorre por causa dos motivos a seguir:
  {: tsCauses}

   * O app não é implementado com o buildpack Liberty integrado.
   * O app foi implementado com uma versão anterior do buildpack Liberty.

  Se o problema for causado por uma versão anterior do buildpack Liberty, reimplemente o app no {{site.data.keyword.Bluemix_notm}}. Caso contrário, é possível fornecer os
arquivos de log do aplicativo do cliente para a equipe de suporte:
  {: tsResolve}

    * logs/messages.log
    * logs/stdout.log
    * logs/stderr.log


### Credenciais são necessárias para abrir uma janela de rastreio ou de dump (descontinuado)
  {: #ts_username}

  Um nome de usuário e uma senha são necessários para abrir a janela de rastreio ou de dump.
  {: tsSymptoms}

  Esse problema ocorre por causa do tempo limite da sessão de login.
  {: tsCauses}

  Insira o nome do usuário e a senha novamente.
  {: tsResolve}


### Ocorre um erro quando as operações de rastreio ou de dump estão em execução (descontinuado)
  {: #ts_target}

  Uma mensagem de erro é exibida quando as operações de rastreio ou dump
estão em execução. A mensagem indica que uma instância de destino para um app não está no estado em execução:
  {: tsSymptoms}

  ```
  Instance 0: Trace specification is set successfully
  Instance 2: Trace specification is set successfully
  Instance 1: Target instance 1 for app abcd is not in the running state.
  Instance 3: Trace specification is set successfully
  Instance 4: Trace specification is set successfully
  ```

  Esse problema ocorre por causa dos motivos a seguir:
  {: tsCauses}

    * Os recursos de gerenciamento de rastreio ou de dump são apenas para instâncias de app que estão em execução. As operações de rastreio ou de dump não podem ser usadas em instâncias do app que estejam paradas, sendo iniciadas ou que tenham sido interrompidas.
    * O status da instância do app está mudando quando o diálogo de rastreio ou de dump é aberto.

  Feche a janela e, em seguida, abra-a novamente.
  {: tsResolve}


### As instâncias possuem configurações de traceSpecification diferentes (descontinuadas)
  {: #ts_different_config}

  Para instâncias diferentes de um app, é possível ver configurações traceSpecification diferentes.
  {: tsSymptoms}

  Esse comportamento ocorre por causa dos motivos a seguir:
  {: tsCauses}

    * Você mudou a configuração de uma ou mais instâncias anteriormente. Se você mudar a configuração de traceSpecification de uma instância, a mudança não se aplicará a outras instâncias do mesmo app. Por exemplo, seu app usa log4j e você tem 2 instâncias para esse app. É possível mudar o nível de log da instância 0 de informações para depuração, mas o nível de log da instância 1 permanecerá como informações.

  Não é necessária nenhuma ação. Este comportamento é esperado.
  {: tsResolve}


### Cota do disco exce
  {: #ts_diskquota}

  Você pode ver, em seu log de app, que sua cota do disco foi excedida.

  Você vê a mensagem de erro `Cota do disco excedida` no log de seu app.
  {: tsSymptoms}

  Esse problema ocorre por causa de um dos motivos a seguir:
  {: tsCauses}

    * Os arquivos de dump são gerados com as instâncias do app em execução e os arquivos usam até a cota de disco alocada. Por padrão, a cota de disco para uma instância de app é 1 GB. É possível verificar o uso de
seu disco clicando em **Painel>Aplicativo>Tempo de Execução do Aplicativo**. O exemplo a seguir mostra as informações de tempo de execução, incluindo uso do disco, de duas instâncias de um app:
      ```
      Uso de disco de uso da memória da	CPU	do estado da instância

  	0		Executando	1,0%	344,8MB/512MB	236,8MB/1GB
  	2		Executando	2,3%	361,2MB/512MB	235,7MB/1GB
      ```
    * A cota do disco é limitada pela cota da organização atual.

  Use um dos métodos a seguir:
  {: tsResolve}

    * Excluir arquivos de dump depois de eles serem transferidos por download.
    * Reimplementar o app com uma cota de disco maior, incluindo a entrada a seguir no manifest de implementação:
      ```
      disk_quota: 2048
      ```

## Liberty for Java
{: #ts_liberty}

### O aplicativo falha ao começar a aceitar conexões
{: #health_check_timeout}

Um aplicativo Liberty falha ao iniciar com um erro "_Falha ao iniciar a aceitação de conexões_". Por exemplo, o
erro nos logs pode ser semelhante ao seguinte:
{: tsSymptoms}

```
   2016-11-14T14:44:58.45+0000 [API/0]      OUT App instance exited with guid 21ac63eb-9595-428a-94c7-b0b02aaf77cc payload: {"cc_partition"=>"default", "droplet"=>"21ac63eb-9595-428a-94c7-b0b02aaf77cc", "version"=>"b2772438-92de-4d47-b680-ea772ac2288a", "instance"=>"f4799c8c89214bbd8067883c3ffe23e0", "index"=>0, "reason"=>"CRASHED", "exit_status"=>255, "exit_description"=>"failed to accept connections within health check timeout", "crash_timestamp"=>1479134698} 2016-11-14T14:45:07.50+0000 [DEA/4]      ERR Instance (index 0) failed to start accepting connections
```
{: codeblock}

O {{site.data.keyword.Bluemix_notm}} executa uma verificação de funcionamento no aplicativo para ver se ele foi iniciado com êxito. A verificação de funcionamento testa se o aplicativo está atendendo na porta designada ao aplicativo. O tempo limite padrão para essa verificação é de 60 segundos e alguns aplicativos podem demorar mais de 60 segundos para iniciar.  Há vários motivos pelos quais o aplicativo pode levar mais tempo para iniciar. Por exemplo, serviços de ligação, como [New Relic](/docs/runtimes/liberty/monitoring/newRelic.html), aumentará o tempo de inicialização. O aplicativo também pode executar etapas de inicialização que podem levar muito tempo para serem concluídas.
{: tsCauses}

Primeiro, examine nos logs se há quaisquer erros óbvios que possam causar falha do aplicativo
Liberty. Se não forem encontrados erros óbvios, tente o seguinte:
{: tsResolve}

#### Aumentar o tempo limite de verificação de funcionamento

* Ao implementar o aplicativo usando o comando `ibmcloud cf push`, especifique um tempo limite de início
do aplicativo mais longo usando a opção `-t`. Por exemplo:

        ```
	ibmcloud cf push myApp -t 180
        ```
	{: codeblock}

* O tempo limite de verificação de funcionamento também pode ser especificado no arquivo manifest.yml. Por exemplo:

        ---
           ...
           timeout: 180
        {: codeblock}

#### Desativar o recurso appstate

O recurso appstate é integrado ao processo de verificação de funcionamento do {{site.data.keyword.Bluemix_notm}} para
assegurar que o aplicativo Liberty seja totalmente inicializado antes que o aplicativo possa receber solicitações de HTTP. Quando o aplicativo estiver totalmente inicializado, o recurso appstate não terá mais efeito.  O efeito colateral deste recurso é que alguns aplicativos
podem levar mais tempo para inicializar. Para desativar o recurso appstate, configure a propriedade de
ambiente a seguir em seu aplicativo e remonte o aplicativo:

```
ibmcloud cf set-env myApp JBP_CONFIG_LIBERTY "app_state: false"
```
{: codeblock}

#### Considere a nova factoração do aplicativo

Se seu aplicativo levar muito tempo para inicializar, poderá
ser necessário refatorar o aplicativo para executar inicialização lenta e/ou assíncrona.

#### Implementar com a opção "no-route"

1. Implemente seu aplicativo com a opção "-- no-route". Isso desativará a verificação de funcionamento da porta. Por exemplo:

        ```
	ibmcloud cf push myApp -no-route
        ```
	{: codeblock}

2. Quando o aplicativo for inicializado, mapeie uma rota para o aplicativo. Por exemplo:

        ibmcloud cf map-route myApp mybluemix.net
        {: codeblock}

### Erros de SSL com gateway IBM
{: #ssl_handshake_failure}


Os erros a seguir são visíveis nos logs e o aplicativo poderá falhar ao iniciar:
{: tsSymptoms}

```
    2016-11-03T12:32:44.82-0200 [App/0]      ERR java.security.cert.CertPathValidatorException: Certificate chaining error 2016-11-03T12:32:44.83-0200 [App/0]      ERR [ERROR   ] CWPKI0022E: SSL HANDSHAKE FAILURE:  A signer with SubjectDN CN=*.gateway.prd.na.ca.ibmserviceengage.com, O=International Business Machines Corp., L=Armonk, ST=New York, C=US was sent from the target host.  O assinante poderá precisar ser incluído no armazenamento confiável local
/home/vcap/app/wlp/usr/servers/defaultServer/resources/security/key.jks,
localizado no alias de configuração SSL defaultSSLConfig. The extended error message from the SSL handshake exception is: PKIX path building failed: java.security.cert.CertPathBuilderException: PKIXCertPathBuilderImpl could not build a valid CertPath.; internal cause is: 2016-11-03T12:32:44.83-0200 [App/0]      ERR java.security.cert.CertPathValidatorException: The certificate issued by CN=DigiCert Global Root CA, OU=www.digicert.com, O=DigiCert Inc, C=US is not trusted; internal cause is: 2016-11-03T12:32:44.83-0200 [App/0]      ERR java.security.cert.CertPathValidatorException: Certificate chaining error
```
{: codeblock}

Os erros podem ser gerados quando um serviço seguro é ligado a um aplicativo Liberty, o qual foi implementado como um diretório do servidor ou servidor empacotado que contém server.xml que configura o recurso ssl-1.0 do Liberty. Ligar o serviço seguro ao aplicativo Liberty faz com que o tempo de execução se conecte ao serviço por meio de uma conexão segura. Esta conexão segura é estabelecida usando as configurações de SSL padrão. Como as configurações SSL padrão são especificadas no server.xml do Liberty, o armazenamento confiável configurado pode não confiar no certificado usado pelo serviço seguro.
{: tsCauses}

Modifique a configuração para usar o armazenamento confiável da JVM com uma das opções a seguir.  Certifique-se de remontar seu aplicativo depois de fazer a mudança.
{: tsResolve}

#### Atualizar o server.xml do Liberty

Atualize o server.xml para usar o arquivo cacerts da JVM como o armazenamento confiável. Inclua o seguinte no seu server.xml:

        <ssl id="defaultSSLConfig" trustStoreRef="defaultTrustStore"/>
        <keyStore id="defaultTrustStoretore" location="${java.home}/lib/security/cacerts"/>
        {: codeblock}

#### Atualizar o armazenamento confiável configurado

Modifique o armazenamento confiável configurado para confiar na autoridade de certificação raiz DigitCert.
  1. Faça download da autoridade de certificação raiz DigiCert de https://www.digicert.com/CACerts/DigiCertGlobalRootCA.crt.
  2. Supondo que resources/security/key.jks seja usado como o armazenamento confiável, importe a autoridade de certificação para a chave usando o utilitário keytool de Java:

            keytool -importcert --storepass <keyStorePassword> -keystore &lt;path&gt;/resources/security/key.jks -file DigiCertGlobalRootCA.crt
            {: codeblock}


## SDK for Node.js
{: #ts_nodejs}

### O aplicativo falha ao ser iniciado: resolução de problemas gerais

Para o buildpack V3.23 ou mais recente do {{site.data.keyword.runtime_nodejs_notm}}, tente enviar as suas dependências ao fornecedor como uma primeira etapa da resolução de problemas. O envio de dependências ao fornecedor significa que você empacota as dependências nos mesmos arquivos de origem do aplicativo. Isso pode resolver vários erros que podem ocorrer quando as dependências assumem que estão no mesmo diretório base que o aplicativo.

1. No diretório-raiz do aplicativo, instale as dependências executando o comando a seguir.

   ```bash
   npm install
   ```
   {: codeblock}
1. Assegure-se de que o arquivo `.cfignore` não inclua a linha a seguir:

   ```
   node_modules/
   ```

Agora, ao implementar o aplicativo com o comando `ibmcloud cf push`, em vez de as dependências serem transferidas por download para um local separado, elas são copiadas no mesmo diretório do restante do aplicativo.

### O aplicativo falha ao iniciar com um erro "Não há mais espaço no dispositivo"
{: #no_space_left_on_device}


Um aplicativo Node.js falha ao iniciar com um erro "Não há mais espaço no dispositivo". Por exemplo, o
erro nos logs pode ser semelhante ao seguinte:
{: tsSymptoms}

```
   2017-01-16T14:25:14.61-0500 [CELL/0]     ERR tar: ./app/node_modules/pm2/node_modules/cron/node_modules/moment-timezone/LICENSE: Cannot write: No space left on device

```
{: codeblock}

Os aplicativos Node.js que usam versões do NPM anteriores à versão 3 consomem mais espaço ao fazer download de dependências.
{: tsCauses}

Modifique o arquivo package.json de seu aplicativo para usar uma versão NPM 3 ou superior.
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

### O aplicativo reinicia devido a restrições de memória
{: #oom}

O Node.js não sabe quanta memória está disponível para o aplicativo, portanto o coletor de lixo pode não ser executado antes de a memória estar esgotada.

```
2017-09-01T11:00:42.19-0400 [APP/PROC/WEB/0]OUT Exit status 137
2017-09-01T11:00:42.23-0400 [CELL/0]     OUT Exit status 0
2017-09-01T11:00:42.27-0400 [CELL/0]     OUT Destroying container
2017-09-01T11:00:42.34-0400 [API/0]      OUT Process has crashed with type: "web"
2017-09-01T11:00:42.36-0400 [API/0]      OUT App instance exited with guid eecfba3b-430c-4a6b-b71f-ac72816fe152 payload: {"instance"=>"77dbb981-16d0-3a05-3235-9a4b", "index"=>0, "reason"=>"CRASHED", "exit_description"=>"2 error(s) occurred:\n\n* 2 error(s) occurred:\n\n* Exited with status 137 (out of memory)\n* cancelled\n* cancelled", "crash_count"=>1, "crash_timestamp"=>1504278042244633291, "version"=>"6497b5b5-67d4-4c5a-b1af-362e522a029d"}
2017-09-01T11:00:43.35-0400 [CELL/0]     OUT Successfully destroyed container
```
{: codeblock}

Uma solução possível é configurar a opção `--max_old_space_size` no comando inicial do aplicativo no arquivo package.json. Essa opção representa parte da área de cobertura da memória do aplicativo e deve ser configurada para um valor menor que a memória total disponível para o aplicativo. Leia sobre [Aumentos grandes de memória e Heroku](https://github.com/nodejs/node/issues/3370) para uma discussão mais detalhada deste tópico.
```
  "scripts": {
    "start": "node --max_old_space_size=800 server.js"
  }
```
{: codeblock}

## ASP.NET Core
{: #ts_dotnet}

O aplicativo falha ao implementar com a mensagem: `A instância do API/0App saiu... carga útil: {... "reason"=>"CRASHED", "exit_status"=>-1, ...}`.
{: tsSymptoms}

Se você receber uma mensagem semelhante quando enviar por push o seu aplicativo ASP.net, muito provavelmente isso ocorrerá
porque o seu aplicativo excede os limites de cota de memória ou de disco.  Aumente as cotas para a memória ou o espaço em disco para
o seu aplicativo.
{: tsCauses}

O aplicativo falha ao implementar com a mensagem: `Falha ao compactar droplet: sinal: canal
dividido` ou `Não há mais espaço no dispositivo`.  Como posso corrigir isso?
{: tsSymptoms}

Os projetos enviados por push do código-fonte que contêm um grande número de dependências de pacote NuGet podem, às
vezes, causar esse erro quando o cache de pacotes NuGet está ativado.  Configure a variável de ambiente `CACHE_NUGET_PACKAGES` como `false` para desativar o cache. Consulte as instruções sobre como [Desativar o pacote NuGet](/docs/runtimes/dotnet/disablingNuGet.md) para obter mais informações.
{: tsCauses}

### Links úteis
* [NuGet](https://docs.nuget.org/Consume/Overview){: new_window}
* [Visão geral do ASP.NET Core](http://docs.asp.net/en/latest/conceptual-overview/aspnet.html){: new_window}

## PHP
{: #ts_PHP}

### Mensagens NOTICE do buildpack do PHP
{: #ts_phplog}

Talvez você veja mensagens que contenham AVISO nos logs. É possível parar a criação de log dessas mensagens alterando o
nível de criação de log.

Ao enviar um app por push para o {{site.data.keyword.Bluemix_notm}} usando um buildpack PHP, será possível ver mensagens contendo `NOTICE`:
{: tsSymptoms}

```
• 2015-01-26T15:00:59.70+0100 [App/0] ERR [26-Jan-2015 14:00:59] NOTICE: [pool www] 'user' directive is ignored when FPM is not running as root
• 2015-01-26T15:01:00.63+0100 [App/0] ERR [26-Jan-2015 14:00:59] NOTICE: [pool www] 'user' directive is ignored when FPM is not running as root
• 2015-01-26T15:01:00.63+0100 [App/0] ERR [26-Jan-2015 14:00:59] NOTICE: fpm is running, pid 93
• 2015-01-26T15:01:00.63+0100 [App/0] ERR [26-Jan-2015 14:00:59] NOTICE: ready to handle connections
```
No buildpack PHP, o parâmetro error_log define o nível de criação de log. Por padrão, o valor do parâmetro `error_log`
é **stderr notice**. O exemplo a seguir mostra a configuração do nível de criação de log padrão no arquivo `nginx-defaults.conf` do buildpack PHP que é fornecido pelo Cloud Foundry. Para obter mais informações, veja [cloudfoundry/php-buildpack ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/cloudfoundry/php-buildpack/blob/ff71ea41d00c1226d339e83cf2c7d6dda6c590ef/defaults/config/nginx/1.5.x/nginx-defaults.conf){: new_window}.
{: tsCauses}

```
daemon off;
error_log stderr notice;
pid @{HOME}/nginx/logs/nginx.pid;
```

As mensagens `NOTICE` são para informações e podem não indicar um problema. É possível parar a criação de log dessas mensagens mudando o nível de criação de log de `stderr notice` para `stderr error` no arquivo nginx-defaults.conf de seu buildpack. Por exemplo: 	
{: tsResolve}

```
daemon off;
error_log stderr error;
pid @{HOME}/nginx/logs/nginx.pid;
```
Para obter mais informações sobre como mudar a configuração de criação de log padrão, veja [error_log ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](http://nginx.org/en/docs/ngx_core_module.html#error_log){: new_window}.


## Python
{: #ts_python}

### Não é possível importar uma biblioteca Python de terceiros para o {{site.data.keyword.Bluemix_notm}}
{: #ts_importpylib}

Pode ser que você não consiga importar uma biblioteca Python de terceiros
para o {{site.data.keyword.Bluemix_notm}}. Para resolver o problema, inclua arquivos de configuração no diretório-raiz de seu app python.

Ao tentar importar uma biblioteca Python de terceiro, como a biblioteca `web.py`, o comando
`ibmcloud cf push` falha.
{: tsSymptoms}

As informações de configuração para o app Python estão ausentes.
{: tsCauses}

Inclua um arquivo `requirements.txt` e um arquivo `Procfile` no diretório-raiz de seu app Python. As informações a seguir presumem que você
esteja importando a biblioteca `web.py`:
{: tsResolve}

 1. Inclua um arquivo `requirements.txt` no diretório-raiz de seu app Python.

 O arquivo `requirements.txt` especifica os pacotes de biblioteca necessários para o app Python e a versão dos pacotes. O exemplo a seguir mostra o conteúdo do arquivo `requirements.txt`, em que `web.py==0.37` indica que a versão da biblioteca `web.py` que será transferida por download é 0,37 e `wsgiref==0.1.2` indica que a versão da interface do gateway do servidor da web que é requerida pela biblioteca web.py é 0.1.2.
	 ```
	 web.py==0.37
     wsgiref==0.1.2
	 ```
	 Para obter mais informações sobre como configurar
o arquivo `requirements.txt`, consulte [Arquivos de requisitos](https://pip.readthedocs.org/en/1.1/requirements.html).

 2. Inclua um arquivo `Procfile` no diretório-raiz de seu app Python.
 O arquivo `Procfile` deve conter o comando inicial para seu app Python. No comando a seguir, *yourappname* é o nome de seu app Python e *PORT* é o número da porta que seu app Python deve usar para receber solicitações de usuários do app. *$PORT* é opcional. Se você não especificar PORT no comando inicial, o número da porta sob a variável de ambiente `VCAP_APP_PORT` que está dentro do app será usado.
	```
	web: python <yourappname>.py $PORT
	```

Agora é possível importar a biblioteca Python de terceiros para o {{site.data.keyword.Bluemix_notm}}.
