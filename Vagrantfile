Vagrant.configure("2") do |config|
    config.vm.boot_timeout = 60
    config.vm.define "Windows" do |vm_windows|
        vm_windows.vm.box = "StefanScherer/windows_2019"
        vm_windows.vm.hostname = "windowsserver2019"
        vm_windows.vm.network :forwarded_port, guest: 8080, host: 8080
        vm_windows.vm.network :forwarded_port, guest: 9000, host: 9000
        vm_windows.vm.provider "virtualbox" do |v|
            v.name = "windowsserver2019"
            v.cpus = 2
            v.memory = 4096
            v.customize ["modifyvm", :id, "--hwvirtex", "on"]
            v.customize ["modifyvm", :id, "--nic2", "bridged", "--bridgeadapter2" , "Realtek PCIe GBE Family Controller", "--cableconnected1", "on"]
            v.customize ["modifyvm", :id, "--nicpromisc3", "allow-all"]
            v.customize ["modifyvm", :id, "--nictype3", "82540EM"]
            v.customize ["modifyvm", :id, "--clipboard", "bidirectional"]
        end
    end    
end    
