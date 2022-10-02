Vagrant.configure("2") do |config|
  config.vm.provision "shell", inline: "echo SCHOOL OF DEVOPS PROJECT IS ALREADY CREATING..."
  config.vm.define "vm1" do |vm1|
    vm1.vm.box = "ubuntu/focal64"
    vm1.vm.box_check_update = false
    vm1.vm.hostname = "vm1"
    vm1.vm.network "forwarded_port", guest: 8080, host: 8081, host_ip: "127.0.0.1"
    vm1.vm.network "forwarded_port", guest: 3000, host: 8083, host_ip: "127.0.0.1"
    vm1.vm.network "forwarded_port", guest: 3306, host: 8085, host_ip: "127.0.0.1"
    vm1.vm.network "private_network", ip: "192.168.56.3"
  # web.vm.network "public_network"
    vm1.vm.synced_folder "data", "/vagrant_data"
    vm1.vm.provider "virtualbox" do |vb|
      vb.memory = "1024"
    end
    vm1.vm.provision "shell", inline: <<-SHELL
      cd /vagrant_data
      sudo apt -y install git
      cd /vagrant_data/
      git clone https://gitlab.com/matiasnicolas.vargas/schoolofdevopsdocker.git
      cp /vagrant_data/Dockerfile_frontend ./schoolofdevopsdocker/rampupFrontend/Dockerfile
      cp /vagrant_data/Dockerfile_backend ./schoolofdevopsdocker/rampupBackend/Dockerfile
      cp /vagrant_data/.envfrontend ./schoolofdevopsdocker/rampupFrontend/.env
      cp /vagrant_data/.envbackend ./schoolofdevopsdocker/rampupFrontend/.env
    SHELL
    vm1.vm.provision "docker" do |d|
      d.build_image "/vagrant_data/schoolofdevopsdocker/rampupBackend",
      args: "-t backend:schooldevops"
      d.build_image "/vagrant_data/schoolofdevopsdocker/rampupFrontend",
      args: "-t frontend:schooldevops"
    end
    vm1.vm.provision "shell", inline: <<-SHELL
      docker swarm init --advertise-addr 192.168.56.3
      docker swarm join-token worker -q > /vagrant_data/worker.txt
    SHELL
 
  end
  config.vm.define "vm2" do |vm2|
    vm2.vm.box = "ubuntu/focal64"
    vm2.vm.box_check_update = false
    vm2.vm.hostname = "vm2"
    vm2.vm.network "forwarded_port", guest: 3030, host: 8082, host_ip: "127.0.0.1"
    vm2.vm.network "forwarded_port", guest: 3000, host: 8084, host_ip: "127.0.0.1"
    vm2.vm.network "forwarded_port", guest: 3306, host: 8086, host_ip: "127.0.0.1"
    vm2.vm.network "private_network", ip: "192.168.56.4"
  # web.vm.network "public_network"
    vm2.vm.synced_folder "data", "/vagrant_data"
    vm2.vm.provider "virtualbox" do |vb|
      vb.memory = "1024"
    end
    vm2.vm.provision "docker" do |d|
      d.build_image "/vagrant_data/schoolofdevopsdocker/rampupBackend",
      args: "-t backend:schooldevops"
      d.build_image "/vagrant_data/schoolofdevopsdocker/rampupFrontend",
      args: "-t frontend:schooldevops"
    end
  end
end

