# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  config.hostmanager.enabled = true

  config.vm.box = "ubuntu/xenial64"

  config.vm.define "control", primary: true do |h|
    h.vm.network "private_network", ip: "192.168.135.10"
    h.vm.synced_folder "./work", "/work"
    h.vm.provision :shell, :inline => <<'EOF'
if [ ! -f "/home/vagrant/.ssh/id_rsa" ]; then
  ssh-keygen -t rsa -N "" -f /home/vagrant/.ssh/id_rsa
fi
cp /home/vagrant/.ssh/id_rsa.pub /vagrant/control.pub

cat << 'SSHEOF' > /home/vagrant/.ssh/config
Host *
  StrictHostKeyChecking no
  UserKnownHostsFile=/dev/null
SSHEOF

chown -R vagrant:vagrant /home/vagrant/.ssh/
EOF
  end

  config.vm.define "lb01" do |h|
    h.vm.network "private_network", ip: "192.168.135.101"
    h.vm.provision :shell, inline: 'cat /vagrant/control.pub >> /home/vagrant/.ssh/authorized_keys'
  end

  config.vm.define "app01" do |h|
    h.vm.network "private_network", ip: "192.168.135.111"
    h.vm.provision :shell, inline: 'cat /vagrant/control.pub >> /home/vagrant/.ssh/authorized_keys'
  end

  config.vm.define "app02" do |h|
    h.vm.network "private_network", ip: "192.168.135.112"
    h.vm.provision :shell, inline: 'cat /vagrant/control.pub >> /home/vagrant/.ssh/authorized_keys'
  end

  config.vm.define "db01" do |h|
    h.vm.network "private_network", ip: "192.168.135.121"
    h.vm.provision :shell, inline: 'cat /vagrant/control.pub >> /home/vagrant/.ssh/authorized_keys'
  end

end
