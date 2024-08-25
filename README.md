## Configuração geral do "Provider" do VirtualBox

link: https://docs.oracle.com/en/virtualization/virtualbox/6.0/user/vboxmanage-modifyvm.html

## Configuração de VM Windows com Vagrantfile

link: https://developer.hashicorp.com/vagrant/docs/vagrantfile/winrm_settings

Como configurar o usuário de acesso do Vagrant
Vagrant.configure("2") do |config|
config.vm.communicator = "winrm"
config.winrm.username = "vagrant"
config.winrm.password = "vagrant"

Alterar o espaço em disco após a VM ser criada
link: https://docs.oracle.com/en/virtualization/virtualbox/6.0/user/vboxmanage-modifymedium.html

## Configuração e Instalação manual do Jenkins

link: https://www.jenkins.io/doc/book/installing/windows/

1. Instalação do Java JDK
    - Download do JDK versão 11
        - https://adoptium.net/temurin/releases/?os=windows&arch=x64&package=jdk&version=11
    - Essa instalação não será no padrão windows com "next, next, next"
    
    - Inicie o arquivo JDK 
        - Siga até "Custo Setup" par alterar o local e variável de ambiente
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

- Abra o terminal para conferir se as variáveis tiveram retorno
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

2. Local Security Policy (Necessário para permissões do Jenkins)
    - Local Policies
    - User Rights Assignment
    - Log on as a services
        - Add User or Group
        - administrator
            - Check Names
            - Aplly                              

3. Instalação do Jenkins
    - Download do Jenkins versão LTS Windows
        - https://www.jenkins.io/download/
    
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


