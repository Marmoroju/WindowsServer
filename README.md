## Links das Documentações 

Para provisionar o "Provider" do VirtualBox:
link: https://docs.oracle.com/en/virtualization/virtualbox/6.0/user/vboxmanage-modifyvm.html

Para provisionar VM Windows com Vagrantfile:
link: https://developer.hashicorp.com/vagrant/docs/vagrantfile/winrm_settings

Para alterar o espaço em disco após a VM ser criada:
link: https://docs.oracle.com/en/virtualization/virtualbox/6.0/user/vboxmanage-modifymedium.html

Para instalação e configuração do Jenkins:
link: https://www.jenkins.io/doc/book/installing/windows/

Para instalar e configurar IIS:
Link: https://www.youtube.com/watch?v=gK_bLdu-AzI

Para Download do JDK versão 11:
Link: https://adoptium.net/temurin/releases/?os=windows&arch=x64&package=jdk&version=11

Para Download do Jenkins versão LTS Windows:
Link: https://www.jenkins.io/download/

Para Download do Git:
link: https://git-scm.com/downloads

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
                - Edite o "valor" para C:\tools\jdk-11.0.24.8-hotspot 
                    - precisa deixar sem a última barra
                - Ainda em JAVA_HOME
                    - Altere o "Path"    
                    - C:\tools\jdk-11.0.24.8-hotspot\bin
                    - %JAVA_HOME%\bin

- Abra o terminal para conferir o retorno das variáveis
```bash
    java -version
```

- digite: 
```bash
    echo %JAVA_VERSION%
```

- digite: 
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
            - C:\tools\jdk-11.0.24.8-hotspot\\bin\java.exe
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

### Configurar no Jenkins o Git e JDK

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
    







