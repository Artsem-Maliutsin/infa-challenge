# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
 (1..4).each do |i|
  if i <= 3
    config.vm.define "worker-#{i}" do |node|
      node.vm.box = "bento/ubuntu-20.04"
      node.vm.provision :docker
      node.vm.provision :docker_compose
      node.vm.network "private_network", ip: "192.168.70.#{i+1}"
      config.vm.provider :virtualbox do |vb|
        vb.customize ["modifyvm", :id, "--memory", "512"]
      end
      config.vm.provision "shell" do |s|
       ssh_pub_key = File.readlines("#{Dir.home}/.ssh/id_rsa.pub").first.strip
       s.inline = <<-SHELL
         echo #{ssh_pub_key} >> /home/vagrant/.ssh/authorized_keys
       SHELL
      end
    end
  else
    config.vm.define "proxy-#{i}" do |node|
      node.vm.box = "bento/ubuntu-20.04"
      node.vm.provision :docker 
      node.vm.provision :docker_compose
      node.vm.network "private_network", ip: "192.168.70.#{i+1}"
      config.vm.provider :virtualbox do |vb|
        vb.customize ["modifyvm", :id, "--memory", "512"]
      end
      config.vm.provision "shell" do |s|
       ssh_pub_key = File.readlines("#{Dir.home}/.ssh/id_rsa.pub").first.strip
       s.inline = <<-SHELL
         echo #{ssh_pub_key} >> /home/vagrant/.ssh/authorized_keys
       SHELL
      end
    end
  end
 end

end
