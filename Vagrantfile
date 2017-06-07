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
				
				hostname="master1.cloud.com"
				
				echo "----------------------------"
				echo "----- INSTALANDO NANO ------"
				echo "----------------------------"
				yum -y install nano
				echo "--------------------------------"
				echo "----- CONFIGURANDO O HOST ------"
				echo "--------------------------------"
				echo "NETWORKING=yes
				HOSTNAME="$hostname" 
				GATEWAY=192.168.1.1" > /etc/sysconfig/network
				sed -i "8s/^/::/" /etc/hosts
				sed -i "11s/^/$hostname master/" /etc/hosts
				sed -i "15s/^/::/" /etc/cloud/templates/hosts.redhat.tmpl
				sed -i "18s/^/$hostname master/" /etc/cloud/templates/hosts.redhat.tmpl
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
				hostname $hostname
				echo "$hostname" > /etc/hostname
				/etc/init.d/network restart
				echo "--------------------------------------"
				echo "----- DOWNLOAD CLOUDERA MANAGER ------"
				echo "--------------------------------------"
				wget https://archive.cloudera.com/cm5/redhat/7/x86_64/cm/cloudera-manager.repo
				cp cloudera-manager.repo /etc/yum.repos.d/
				sudo yum -y install oracle-j2sdk1.7
				sudo yum -y install cloudera-manager-daemons cloudera-manager-server
				sudo yum --nogpgcheck localinstall cloudera-manager-daemons-*.rpm
				sudo yum --nogpgcheck localinstall cloudera-manager-server-*.rpm
				sudo yum -y install cloudera-manager-server-db-2
				sudo service cloudera-scm-server-db start
				sudo yum -y install cloudera-manager-agent cloudera-manager-daemons
				sudo yum --nogpgcheck localinstall cloudera-manager-agent-package.*.x86_64.rpm cloudera-manager-daemons
				wget https://archive.cloudera.com/cdh5/one-click-install/redhat/7/x86_64/cloudera-cdh-5-0.x86_64.rpm?_ga=2.183832100.346032946.1495453787-1339592384.1495453787
				mv cloudera-cdh-5-0.x86_64.rpm?_ga=2.183832100.346032946.1495453787-1339592384.1495453787 cloudera-cdh-5-0.x86_64.rpm
				sudo yum -y --nogpgcheck localinstall cloudera-cdh-5-0.x86_64.rpm
				sudo yum clean all
				sudo yum -y install avro-tools crunch flume-ng hadoop-hdfs-fuse hadoop-hdfs-nfs3 hadoop-httpfs hadoop-kms hbase-solr hive-hbase hive-webhcat hue-beeswax hue-hbase hue-impala hue-pig hue-plugins hue-rdbms hue-search hue-spark hue-sqoop hue-zookeeper impala impala-shell kite llama mahout oozie pig pig-udf-datafu search sentry solr-mapreduce spark-core spark-master spark-worker spark-history-server spark-python sqoop sqoop2 whirr
				sudo service cloudera-scm-server start
				sudo service cloudera-scm-agent start
				echo "--------------------------------"
				echo "----- CLOUDERA MANAGER UP ------"
				echo "--------------------------------"
				exit
			SHELL
		end
	end
end
