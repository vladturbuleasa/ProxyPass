# -*- mode: ruby -*-
# vi: set ft=ruby :

$install_tools = <<SCRIPT
echo "$(echo 'GATEWAY=192.168.1.1' | cat - /etc/sysconfig/network)" > /etc/sysconfig/network
mv /etc/resolv.conf /etc/resolv.conf.old
touch /etc/resolv.conf
echo "193.231.252.1" >> /etc/resolv.conf
echo "213.154.124.1" >> /etc/resolv.conf
echo "8.8.8.8" >> /etc/resolv.conf
service network restart
ADDRIP=$(ifconfig eth1 | cut -d" " -f 12 | grep addr | cut -d":" -f2)
#yum -y update
yum -y install vim zip unzip mc
yum -y install squid
echo "$(echo 'acl client src '$ADDRIP)" >> /etc/squid/squid.conf
echo 0 */1 * * * service squid reload >> /var/spool/cron/root
service squid start
echo "=============================="
echo $ADDRIP
echo "=============================="
SCRIPT

Vagrant.configure("2") do |config|
  
  config.vm.define "proxyVM" do |proxyVM|
    proxyVM.vm.box = "bento/centos-6.7"
    proxyVM.vm.hostname= "proxyVM"
    proxyVM.vm.provision :shell, :inline => $install_tools
    proxyVM.vm.synced_folder ".", "/vagrant", disabled: true
    proxyVM.vm.network :public_network, bridge: "Local Area Connection"
    proxyVM.vm.provider "virtualbox" do |vm|
       vm.name = "proxyVM"
       vm.customize [
          'modifyvm', :id,
          '--memory', '1024'
        ]
    end
  end
  
end