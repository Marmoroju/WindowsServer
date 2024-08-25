## Configuração geral do "Provider" do VirtualBox

link: https://docs.oracle.com/en/virtualization/virtualbox/6.0/user/vboxmanage-modifyvm.html

Como exemplo, para habilitar o adaptador de rede, nome no VirtualBox e capacidade de CPU e Memória e cópia bidirecional ao iniciar a VM pelo Vagrant:

```bash
vm_windows.vm.provider "virtualbox" do |v|
    v.name = "windowsserver2019"
    v.cpus = 2
    v.memory = 4096
        v.customize ["modifyvm", :id, "--hwvirtex", "on"]
        v.customize ["modifyvm", :id, "--nic2", "bridged", "--bridgeadapter2" , "Realtek PCIe GBE Family Controller", "--cableconnected2", "off"]
        v.customize ["modifyvm", :id, "--nicpromisc3", "allow-all"]
        v.customize ["modifyvm", :id, "--nictype3", "82540EM"]
        v.customize ["modifyvm", :id, "--clipboard", "bidirectional"]
end
```        



## Configuração de VM Windows com Vagrantfile

link: https://developer.hashicorp.com/vagrant/docs/vagrantfile/winrm_settings

Como configurar o usuário de acesso do Vagrant
Vagrant.configure("2") do |config|
config.vm.communicator = "winrm"
config.winrm.username = "vagrant"
config.winrm.password = "vagrant"

Alterar o espaço em disco após a VM ser criada
link: https://docs.oracle.com/en/virtualization/virtualbox/6.0/user/vboxmanage-modifymedium.html

## Configuração e Instalação do Jenkins
