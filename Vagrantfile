# -*- mode: ruby -*-
# vi: set ft=ruby :

$install_tools = <<SCRIPT
#yum -y update
yum -y install vim zip unzip mc
yum -y install squid
echo "$(echo 'acl client src '$ADDRIP)" >> /etc/squid/squid.conf
echo 0 */1 * * * service squid reload >> /var/spool/cron/root
service squid start
echo "=============================="
echo 'Done'
echo "=============================="
SCRIPT

Vagrant.configure("2") do |config|
  
  config.vm.define "proxyVM" do |proxyVM|
    proxyVM.vm.box = "bento/centos-6.7"
    proxyVM.vm.hostname= "proxyVM"
    proxyVM.vm.provision :shell, :inline => $install_tools
    proxyVM.vm.synced_folder ".", "/vagrant", disabled: true
    proxyVM.vm.network "forwarded_port", guest: 22, host: 2022, id: "ssh", auto_correct: true
	proxyVM.vm.network "forwarded_port", guest: 3128, host: 31280, id: "proxy"
    proxyVM.vm.provider "virtualbox" do |vm|
       vm.name = "proxyVM"
       vm.customize [
          'modifyvm', :id,
          '--memory', '1024'
        ]
    end
  end
  
end