Vagrant.configure("2") do |config|

config.vm.define "master" do |config|
		config.vm.box = "digital_ocean"
		config.ssh.private_key_path = "~/.ssh/id_rsa"
			config.vm.provider :digital_ocean do |provider|
				provider.token = "token"
				provider.image = "centOS-7-x64"
				provider.region = "nyc3"
				provider.size = "64gb"
				config.vm.provision "shell", inline: <<-SHELL
    
				#!/bin/bash
				
				echo "----------------------------"
				echo "----- INSTALANDO NANO ------"
				echo "----------------------------"
				yum -y install nano
				echo "--------------------------------"
				echo "----- CONFIGURANDO O HOST ------"
				echo "--------------------------------"
				echo "NETWORKING=yes 
				GATEWAY=192.168.1.1" > /etc/sysconfig/network
				sed -i "92s/^/dummyuser ALL=(ALL) NOPASSWD:ALL/" /etc/sudoers
				echo "----- Desabilitando o Selinux ------"
				setenforce 0
				echo "Disable IPv6
				net.ipv6.conf.all.disable_ipv6 = 1
				net.ipv6.conf.default.disable_ipv6 = 1
				net.ipv6.conf.lo.disable_ipv6 = 1" >> /etc/sysctl.conf
				echo "NETWORKING_IPV6=no
				IPV6INIT=no" >> /etc/sysconfig/network-scripts/ifcfg-eth0
				systemctl disable firewalld
				systemctl status firewalld
				sysctl vm.overcommit_memory=1
				echo "vm.overcommit_memory = 1" >> /etc/sysctl.conf
				sysctl vm.swappiness=0
				echo "vm_swappiness = 0" >> /etc/sysctl.conf
				echo "echo never > /sys/kernel/mm/redhat_transparent_hugepage/defrag"  >> /etc/rc.local
				echo "echo never > /sys/kernel/mm/transparent_hugepage/defrag"  >> /etc/rc.local
				echo "echo never > /sys/kernel/mm/transparent_hugepage/enabled"  >> /etc/rc.local
				cat /sys/kernel/mm/transparent_hugepage/defrag
				/etc/init.d/network restart
				echo "--------------------------------------"
				echo "----- DOWNLOAD CLOUDERA MANAGER ------"
				echo "--------------------------------------"
				wget https://archive.cloudera.com/cm5/redhat/7/x86_64/cm/cloudera-manager.repo
				cp cloudera-manager.repo /etc/yum.repos.d/
				sudo yum -y install oracle-j2sdk1.7
				sudo yum -y install cloudera-manager-daemons cloudera-manager-server
				sudo yum -y install cloudera-manager-server-db-2
				sudo service cloudera-scm-server-db start
				sudo service cloudera-scm-server start
				echo "--------------------------------"
				echo "----- CLOUDERA MANAGER UP ------"
				echo "--------------------------------"
				exit
			SHELL
		end
	end
end
