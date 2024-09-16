## Links das Documentações 

Para provisionar o "Provider" do VirtualBox:
- Link: https://docs.oracle.com/en/virtualization/virtualbox/6.0/user/vboxmanage-modifyvm.html

Para provisionar VM Windows com Vagrantfile:
- Link: https://developer.hashicorp.com/vagrant/docs/vagrantfile/winrm_settings

Para alterar o espaço em disco após a VM ser criada:
- Link: https://docs.oracle.com/en/virtualization/virtualbox/6.0/user/vboxmanage-modifymedium.html

Para instalação e configuração do Jenkins:
- Link: https://www.jenkins.io/doc/book/installing/windows/

Para Download do JDK versão 17 LTS:
- Link: https://adoptium.net/temurin/releases/?os=windows&version=17

Para Download do Jenkins versão LTS Windows:
- Link: https://www.jenkins.io/download/

Para instalar e configurar IIS:
- Link: https://www.youtube.com/watch?v=gK_bLdu-AzI

Para Download do Git:
- Link: https://git-scm.com/downloads

Para Download do SonarQube
- Link: https://www.sonarsource.com/products/sonarqube/downloads/

Para Download do SonarScanner
- Link: https://docs.sonarsource.com/sonarqube/9.9/analyzing-source-code/scanners/sonarscanner/

Para Documentação do Sonarlint
- Link: https://docs.sonarsource.com/sonarlint/vs-code/

Para Instalação do Sonarlint no VCcode
- Link: https://www.youtube.com/watch?v=m8sAdYCIWhY&ab_channel=Sonar

Para Pipeline com Powershell
- Link: https://www.jenkins.io/blog/2017/07/26/powershell-pipeline/

## Criar do zero uma Box do windows
1. Baixar a ISO do Windows
2. Criar VM e instalar a ISO
3. Alterar nome da VM para VagrantPC ou Vagrant
4. Criar usuario e senha vagrant
5. No terminal, navegue até o diretório onde a máquina virtual foi criada.
6. Opcional: Instalar WinRm e Atualizar o PowerShell para futura configuração do Ansible

7. Execute o seguinte comando para criar a Box do Vagrant:
- Precisa estar com a VM desligada. Caso contrário o comando irá forçar o desligamento.
- `vagrant package --base nome_da_sua_vm_no_virtualbox`
- Isso criará um arquivo package.box no diretório atual.
- Ex.: `vagrant package --base Vagrant`
- A exportação da VM é um pouco demorada (Depndendo da configuração do PC).
- Logo após exportar, será realizada a compressão do arquivo que levará um tempo também.
- O Arquivo final terá em média 5GB a 6GB mais ou menos.

8. Adicione a box à sua biblioteca do Vagrant:
- Execute o seguinte comando para adicionar a “box” à sua biblioteca do Vagrant:
- `vagrant box add windowsserver2019 package.box`
- Isso adicionará a box do Windows 10 à sua biblioteca LOCAL do Vagrant com o nome windowsserver2019.

9. Crie um Vagrantfile para iniciar a máquina virtual:
- `vagrant init windowsserver2019`

## Etapas de Configuração e Instalação Manual do Jenkins

### Instalação do Java JDK
 - Essa instalação não será no padrão windows com "next, next, next"
    
    - Inicie o arquivo JDK 
        - Siga até "Custom Setup" par alterar o local e variável de ambiente
            - Opção "Set or override JAVA_HOME variable" 
            - "Will be installed on local hard drive"
            - clicar em "JDK with Hostspot" e "Brownse"
                - Alterar o caminho padrão C:\Program Files\Eclipse Adoptium\jdk-11.0.24.8-hotspot\
                - Para C:\tools\jdk-11.0.24.8-hotspot\
         - Next até o final

         - Configuração das Variáveis de Ambiente
            - Configurações Avançadas do Sistema
            - Avançado > Variáveis de Ambiente 
                - Localize o JAVA_HOME
                - Edite o "valor" para C:\tools\jdk-11.0.24.8-hotspot\ 
                   
                - Ainda em JAVA_HOME
                    - Altere o "Path"    
                    - C:\tools\jdk-11.0.24.8-hotspot\bin
                    - %JAVA_HOME%\bin

- Copie e cole no terminal para conferir o retorno das variáveis
```bash
    java -version
```
 
```bash
    echo %JAVA_VERSION%
```

```bash
    echo %PATH%
```

### Local Security Policy (Necessário para permissões do Jenkins)
- Local Policies
    - User Rights Assignment
    - Log on as a services
        - Add User or Group
        - administrator
            - Check Names
            - Apply                              
### Instalação do Jenkins
- Será instalado no C:\tools\Jenkins\
    - Run service as local or domain user:
        - Adicionar usuário e senha
        - Porta Padrão 8080
    - Conferir o caminho
        - C:\tools\jdk-11.0.24.8-hotspot\
    - Algumas configurações ainda serão realizadas, por isso o serviço ainda não será iniciado. Para isso:
        - Clicar em Start Services 
        - Entire feature will be unavailable
    
    - Opção 1: Copiar o arquivo jenkins.xml e substituir em C:\tools\Jenkins\

    - Opção 2: Configuração manual do arquivo jenkins.xml 
        - Abrir o local onde o Jenkins foi instalado
            - C:\tools\Jenkins\
            - Abrir o arquivo jenkins.xml com algum editor de texto
            - Substituir na linha 34 (env name)
            - env name="JENKINS_HOME" value="%LocalAppData%\Jenkins\.jenkins"/
            - env name="JENKINS_HOME" value="C:\data\jenkins_home"/

            - Substituir na linha 39 (executable)
            - C:\tools\jdk-11.0.24.8-hotspot\bin\java.exe
            - %JAVA_HOME%\bin\java.exe

            - Substituir na linha 40 (arguments)
            - -Xrs -Xms3g -Xmx3g -Djava.awt.headless=true -Djava.net.preferIPv4Stack=true -Djava.io.tmpdir=C:\tools\jenkins\tmp\ -Dorg.apache.commons.jelly.tags.fmt.timeZone=America/Sao_Paulo -Duser.timezone=America/Sao_Paulo -Dhudson.lifecycle=hudson.lifecycle.WindowsServiceLifecycle -jar "C:\tools\jenkins\jenkins.war" --httpPort=8080 --webroot="C:\tools\Jenkins\war" --pluginroot="C:\tools\Jenkins\plugins"

            - Substituir na linha 58 (pidfile)
            - C:\tools\Jenkins\jenkins.pid

            - criar pasta tmp em C:\tools\Jenkins\
            - Reiniciar a VM / Servidor

    - Iniciar o serviço do Jenkins
        - Services
        - Start Jenkins

    - Abra o navegador
    - acesse http://localhost:8080/
    - Siga as instruções de configuração inicial para alterar senha e instalar os plugins

### Habilitar Jenkins para acesso Externo

- Abrir o Gerenciamento do Firewall
- Inbound Rules / Regras de Entrada
- Nova Regra
- Criar regra para porta 8080

### Instalação do Git

- Pode ser instalado em C:\tools\Git
- Editor padrão deve ser alterado para Notepad++
- Após alterar o local e editor, basta seguir com "next, next"

### Configurar no Jenkins o GIT, JDK e POWERSHELL

- Abra o Jenkins navegador

1. Configuração do Git 
- Acesse: 
    - Gerenciar Jenkins
    - Tools
    - Git Installations
    - Path to Git Executable (Adicione o caminho completo do bin onde foi instalado o git)
    - C:\tools\Git\bin\git.exe

2. Acesse a pasta do Git
    - Dê permissão ao usuário que foi configurado para o Jenkins em sua instalação
    - Neste caso, vagrant

3. Configuração do JDK  
- Acesse: 
    - Gerenciar Jenkins
    - Tools
    - JDK instalações
    - Nome > JAVA_HOME
    - JAVA_HOME > C:\tools\jdk-11.0.24.8-hotspot\   (Local de instalação do JDK)

4. Aplicar e Salvar

5. Os arquivos utilizados nas builds dos pipelines serão armazenadas neste diretório:
    - C:\data\jenkins_home\workspace\

6. Instalar PowerShell
    - Gerenciar Jenkins
    - Plugins
    - Extensões Disponíveis
    - Pesquisar por PowerShell

## Instalação e Configuração do IIS

1. Acesse o Server Manager
    - Add roles and features
    - Installation Type > Role-Based...
    - Server Selection > ...server pool
    - Server Roles > Web Server (IIS) > Add features
    - Features > Click Next
    - Web Server Role (IIS) > Click Next
        - Role Services > Click Next
    - Confirmation > Install
    - Acesse o IP do servidor para conferir

2. Configurar o Local Padrão do Site
    - Acesse o Server Manager
    - Tools (Menu superior direito)
    - Internet Information Services (IIS) Manager
    - Start Page > Nome_Local > Sites
    - Default Web Site
        - Propriedades > Manage Website > Advanced Settings
            - Mostra o local padrão em "Physical Path"
        - Default Document > Mostra o tipo de arquivo padrão index aceito pelo web-service exposto
    - Diretório padrão > C:\inetpub\wwwroot

## Instalação e Configuração do SonarQube e SonarScanner    

1. Fazer o download do Sonarqube Community
    - Extrair, copiar e colar na pasta:
        - C:\tools\sonarqube-10.6.0.92116
    - Acessar pelo Terminal:
        - C:\tools\sonarqube-10.6.0.92116\bin\windows-x86-64
        - Executar o arquivo StartSonar.bat
    - Acessar no navegdor http://localhost:9000/

2. Instalar o SonarService
    - Acessar pelo Terminal como Administrador:
        - C:\tools\sonarqube-10.6.0.92116\bin\windows-x86-64
        - Executar o arquivo SonarService.bat
        - SonarService.bat install
    - Acesse Gerenciador de Serviços
        - Abra os Seriços
        - Inicie o Processo do SonarQube

2. Fazer o download do SonarScanner
    - Extrair, copiar e colar na pasta:
        - C:\tools\sonar-scanner

3. Instalar SonarScanner no Jenkins
    - Abrir o Jenkins no navegador
    - Gerenciar Jenkins
    - Plugins
    - Extensões Disponíveis
    - Pesquisar por SonarQube Scanner  
    - Foi adidionado -Dhudson.model.UpdateCenter.pluginDownloadReadTimeoutSeconds=120 ao arquivo jenkins.xml, em "arguments" para poder instalar o scanner.

5. Habilitar Sonarqube para acesso Externo

- Abrir o Gerenciamento do Firewall
- Inbound Rules / Regras de Entrada
- Nova Regra
- Criar regra para porta 9000   

## Utilização local do SonarScanner e configuração do sonar-scanner.properties

Antes de disparar o sonnar-scanner precisará ser realizada algumas configurações:

1. sonar-scanner.properties:
`C:\Local_da_Instalação\sonar-scanner\conf`

Arquivo Original:
```bash
# Configure here general information about the environment, such as the server connection details for example
# No information about specific project should appear here

#----- SonarQube server URL (default to SonarCloud)
#sonar.host.url=https://mycompany.com/sonarqube

#sonar.scanner.proxyHost=myproxy.mycompany.com
#sonar.scanner.proxyPort=8002
```

Adicionar ao final as configurações do servidor (Senha e Login podem ser passadas como variaveis de ambiente):
```bash
# Configure here general information about the environment, such as the server connection details for example
# No information about specific project should appear here

#----- SonarQube server URL (default to SonarCloud)
#sonar.host.url=https://mycompany.com/sonarqube

#sonar.scanner.proxyHost=myproxy.mycompany.com
#sonar.scanner.proxyPort=8002

#Default SonarQube server
sonar.host.url=http://localhost:9000/
sonar.login=seu_login
sonar.password=seu_password
```

2. Permissões no projeto no SonarQube
- Criar e acessar o projeto
    - Project Settings
    - Permissions

3. Para facilitar, pode ser criado um arquivo .bat na pasta bin do sonar-scanner com o mesmo código gerado no Sonarqube, basta alterar o `-D"sonar.sources=."` com o caminho em que o seu código está, neste caso, está apontando para o workspace do Jenkins. Lembrando sempre de alterar a \ para /.

4. Além do código gerado pelo projeto do SonarQube, deve ser adicionado um parâmetro após o sonar.sources, que é o `-D"sonar.projectBaseDir=C:/data/jenkins_home/workspace/jenkins_file/"`, pois o Sonar-Scanner trabalha com outro diretório padrão, por isso deve ser passado o diretório de onde será analisado o código.

Por exemplo, inicialmente no projeto será gerado um código como esse:
```bash
sonar-scanner.bat -D"sonar.projectKey=app" -D"sonar.sources=." -D"sonar.host.url=http://localhost:9000" -D"sonar.token=SEU_TOKEN"
```
Depois ele deverá ser adaptado dessa forma para poder criar o arquivo bat, por exemplo.
```bash
sonar-scanner.bat -D"sonar.projectKey=app" -D"sonar.sources=C:/data/jenkins_home/workspace/jenkins_file/" -D"sonar.projectBaseDir=C:/data/jenkins_home/workspace/jenkins_file/" -D"sonar.host.url=http://localhost:9000" -D"sonar.token=SEU_TOKEN"
```

4. Adicionar em Path nas váriáveis de ambiente, o caminho do bin de onde foi instalado o sonnar-scanner: `C:\Local_de_instalação\sonar-scanner\bin`

### Sonarlint - Extensão VScode
    
1. Nas extensões, procurar por Sonarlint e instalar

2. Adicionar ao Sonarqube e configurar com o QualityGate do projeto
 - Na barra lateral aparecerá um ícone com a logo do Sonarlint
 - Clique nele e depois em "Add SonarQube Connection" 
 - irá pedir a URL 
 - Gerar Token
 - Abrirá o navegador solicitando a permissão
 - Salve a Conexão.

3. Connected Mode
 - Crie o projeto no SonarQube 
 - Crie o QualityGate
 - Acesse o ícone do SonarLint
 - Faça o bind da pasta do projeto no Sonarqube com a pasta do projeto local.








