# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|

  config.vm.base_mac = ""
  private_net = "172.16.1"
  private_dns1 = "#{private_net}.251"
  private_dns2 = "#{private_net}.252"

  config.vm.define :dc1 do |dc1_config|
    dc1_config.vm.box = "generic/alpine317"
    dc1_config.vm.network "private_network", ip: private_net+".251"

    dc1_config.vm.provider "virtualbox" do |dc1_vb|
      dc1_vb.name = "dc1"
    end
    dc1_config.vm.provision "shell", inline: <<-SHELL
      echo "#{private_net}.251	dc1.ad.lab.home	dc1" >> /etc/hosts
      echo "#{private_net}.252	dc2.ad.lab.home	dc2" >> /etc/hosts
    SHELL
  end

  config.vm.define :dc2 do |dc2_config|
    dc2_config.vm.box = "generic/alpine317"
    dc2_config.vm.network "private_network", ip: private_net+".252"

    dc2_config.vm.provider "virtualbox" do |dc2_vb|
      dc2_vb.name = "dc2"
    end
    dc2_config.vm.provision "shell", inline: <<-SHELL
      echo "#{private_net}.251	dc1.ad.lab.home	dc1" >> /etc/hosts
      echo "#{private_net}.252	dc2.ad.lab.home	dc2" >> /etc/hosts
    SHELL
  end

  config.vm.define :rh7, autostart: false, primary: false do |rh7_config|
    rh7_config.vm.box = "centos/7"
    rh7_config.vm.network "private_network", ip: private_net+".247"
    rh7_config.vm.synced_folder ".", "/vagrant", type: "rsync", disabled: true
    rh7_config.vm.provider "virtualbox" do |rh7_vb|
      rh7_vb.name = "rh7"
      rh7_vb.memory = "256"
    end
    rh7_config.vm.provision "shell", inline: <<-SHELL
      echo "#{private_net}.251	dc1.ad.lab.home	dc1" >> /etc/hosts
      echo "#{private_net}.252	dc2.ad.lab.home	dc2" >> /etc/hosts
      natdev=`ip -o addr show up primary scope global | grep 10.0.2.15 | awk '{print $2}'`
      natif=`nmcli dev show ${natdev} | grep GENERAL.CONNECTION: | awk '{for (i=2; i<NF; i++) printf $i " "; print $NF}'`
      nmcli con mod "${natif}" ipv4.addresses 10.0.2.15/24 
      nmcli con mod "${natif}" ipv4.gateway 10.0.2.2
      nmcli con mod "${natif}" ipv4.dns "#{private_dns1} #{private_dns2} 10.0.2.3"
      nmcli con mod "${natif}" ipv4.dns-search "ad.lab.home lab.home"
      nmcli con mod "${natif}" ipv4.method manual
      nmcli con down "${natif}"; nmcli con up "${natif}"
      sed -i "s/^#UseDNS yes/UseDNS no/" /etc/ssh/sshd_config
      systemctl restart sshd
    SHELL
  end

  config.vm.define :rh8, autostart: false, primary: true do |rh8_config|
    rh8_config.vm.box = "almalinux/8"
    rh8_config.vm.network "private_network", ip: private_net+".248"
    rh8_config.vm.synced_folder ".", "/vagrant", type: "rsync", disabled: true
    rh8_config.vm.provider "virtualbox" do |rh8_vb|
      rh8_vb.name = "rh8"
      rh8_vb.memory = "1024"
    end
    rh8_config.vm.provision "shell", inline: <<-SHELL
      echo "#{private_net}.251	dc1.ad.lab.home	dc1" >> /etc/hosts
      echo "#{private_net}.252	dc2.ad.lab.home	dc2" >> /etc/hosts
      natdev=`ip -o addr show up primary scope global | grep 10.0.2.15 | awk '{print $2}'`
      natif=`nmcli dev show ${natdev} | grep GENERAL.CONNECTION: | awk '{for (i=2; i<NF; i++) printf $i " "; print $NF}'`
      nmcli con mod "${natif}" ipv4.addresses 10.0.2.15/24 
      nmcli con mod "${natif}" ipv4.gateway 10.0.2.2
      nmcli con mod "${natif}" ipv4.dns "#{private_dns1} #{private_dns2} 10.0.2.3"
      nmcli con mod "${natif}" ipv4.dns-search "ad.lab.home lab.home"
      nmcli con mod "${natif}" ipv4.method manual
      nmcli con down "${natif}"; nmcli con up "${natif}"
      sed -i "s/^#UseDNS yes/UseDNS no/" /etc/ssh/sshd_config
      systemctl restart sshd
    SHELL
  end

  config.vm.define :rh9, autostart: false, primary: false do |rh9_config|
    rh9_config.vm.box = "almalinux/9"
    rh9_config.vm.network "private_network", ip: private_net+".249"
    rh9_config.vm.synced_folder ".", "/vagrant", type: "rsync", disabled: true
    rh9_config.vm.provider "virtualbox" do |rh9_vb|
      rh9_vb.name = "rh9"
      rh9_vb.memory = "1024"
    end
    rh9_config.vm.provision "shell", inline: <<-SHELL
      echo "#{private_net}.251	dc1.ad.lab.home	dc1" >> /etc/hosts
      echo "#{private_net}.252	dc2.ad.lab.home	dc2" >> /etc/hosts
      natdev=`ip -o addr show up primary scope global | grep 10.0.2.15 | awk '{print $2}'`
      natif=`nmcli dev show ${natdev} | grep GENERAL.CONNECTION: | awk '{for (i=2; i<NF; i++) printf $i " "; print $NF}'`
      nmcli con mod "${natif}" ipv4.addresses 10.0.2.15/24
      nmcli con mod "${natif}" ipv4.gateway 10.0.2.2
      nmcli con mod "${natif}" ipv4.dns "#{private_dns1} #{private_dns2} 10.0.2.3"
      nmcli con mod "${natif}" ipv4.dns-search "ad.lab.home lab.home"
      nmcli con mod "${natif}" ipv4.method manual
      nmcli con down "${natif}"; nmcli con up "${natif}"
      sed -i "s/^#UseDNS yes/UseDNS no/" /etc/ssh/sshd_config
      systemctl restart sshd
    SHELL
  end

  config.vm.define :deb10, autostart: false, primary: false do |deb10_config|
    deb10_config.vm.box = "debian/buster64"
    deb10_config.vm.network "private_network", ip: private_net+".231"
    deb10_config.vm.synced_folder ".", "/vagrant", type: "rsync", disabled: true
    deb10_config.vm.provider "virtualbox" do |deb10_vb|
      deb10_vb.name = "deb10"
      deb10_vb.memory = "1024"
    end
    deb10_config.vm.provision "shell", inline: <<-SHELL
      echo "#{private_net}.251	dc1.ad.lab.home	dc1" >> /etc/hosts
      echo "#{private_net}.252	dc2.ad.lab.home	dc2" >> /etc/hosts
      echo "domain lab.home" > /etc/resolv.conf
      echo "search lab.home ad.lab.home" >> /etc/resolv.conf
      echo "nameserver #{private_dns1}" >> /etc/resolv.conf
      echo "nameserver #{private_dns2}" >> /etc/resolv.conf
      echo "nameserver 10.0.2.3" >> /etc/resolv.conf
    SHELL
  end

  config.vm.define :deb11, autostart: false, primary: false do |deb11_config|
    deb11_config.vm.box = "debian/bullseye64"
    deb11_config.vm.network "private_network", ip: private_net+".232"
    deb11_config.vm.synced_folder ".", "/vagrant", type: "rsync", disabled: true
    deb11_config.vm.provider "virtualbox" do |deb11_vb|
      deb11_vb.name = "deb11"
      deb11_vb.memory = "1024"
    end
    deb11_config.vm.provision "shell", inline: <<-SHELL
      echo "#{private_net}.251	dc1.ad.lab.home	dc1" >> /etc/hosts
      echo "#{private_net}.252	dc2.ad.lab.home	dc2" >> /etc/hosts
      echo "domain lab.home" > /etc/resolv.conf
      echo "search lab.home ad.lab.home" >> /etc/resolv.conf
      echo "nameserver #{private_dns1}" >> /etc/resolv.conf
      echo "nameserver #{private_dns2}" >> /etc/resolv.conf
      echo "nameserver 10.0.2.3" >> /etc/resolv.conf
    SHELL
  end

  config.vm.define :ubu18, autostart: false, primary: false do |ubu18_config|
    ubu18_config.vm.box = "bento/ubuntu-18.04"
    ubu18_config.vm.network "private_network", ip: private_net+".236"
    ubu18_config.vm.synced_folder ".", "/vagrant", type: "rsync", disabled: true
    ubu18_config.vm.provider "virtualbox" do |ubu18_vb|
      ubu18_vb.name = "ubu18"
      ubu18_vb.memory = "1024"
    end
    ubu18_config.vm.provision "shell", inline: <<-SHELL
      systemctl stop systemd-resolved; systemctl disable systemd-resolved; rm -f /etc/resolv.conf
      echo "#{private_net}.251	dc1.ad.lab.home	dc1" >> /etc/hosts
      echo "#{private_net}.252	dc2.ad.lab.home	dc2" >> /etc/hosts
      echo "domain lab.home" > /etc/resolv.conf
      echo "search lab.home ad.lab.home" >> /etc/resolv.conf
      echo "nameserver #{private_dns1}" >> /etc/resolv.conf
      echo "nameserver #{private_dns2}" >> /etc/resolv.conf
      echo "nameserver 10.0.2.3" >> /etc/resolv.conf
    SHELL
  end

  config.vm.define :ubu20, autostart: false, primary: false do |ubu20_config|
    ubu20_config.vm.box = "bento/ubuntu-20.04"
    ubu20_config.vm.network "private_network", ip: private_net+".237"
    ubu20_config.vm.synced_folder ".", "/vagrant", type: "rsync", disabled: true
    ubu20_config.vm.provider "virtualbox" do |ubu20_vb|
      ubu20_vb.name = "ubu20"
      ubu20_vb.memory = "1024"
    end
    ubu20_config.vm.provision "shell", inline: <<-SHELL
      systemctl stop systemd-resolved; systemctl disable systemd-resolved; rm -f /etc/resolv.conf
      echo "#{private_net}.251	dc1.ad.lab.home	dc1" >> /etc/hosts
      echo "#{private_net}.252	dc2.ad.lab.home	dc2" >> /etc/hosts
      echo "domain lab.home" > /etc/resolv.conf
      echo "search lab.home ad.lab.home" >> /etc/resolv.conf
      echo "nameserver #{private_dns1}" >> /etc/resolv.conf
      echo "nameserver #{private_dns2}" >> /etc/resolv.conf
      echo "nameserver 10.0.2.3" >> /etc/resolv.conf
    SHELL
  end

  config.vm.define :ubu22, autostart: false, primary: false do |ubu22_config|
    ubu22_config.vm.box = "bento/ubuntu-22.04"
    ubu22_config.vm.network "private_network", ip: private_net+".238"
    ubu22_config.vm.synced_folder ".", "/vagrant", type: "rsync", disabled: true
    ubu22_config.vm.provider "virtualbox" do |ubu22_vb|
      ubu22_vb.name = "ubu22"
      ubu22_vb.memory = "1024"
    end
    ubu22_config.vm.provision "shell", inline: <<-SHELL
      systemctl stop systemd-resolved; systemctl disable systemd-resolved; rm -f /etc/resolv.conf
      echo "#{private_net}.251	dc1.ad.lab.home	dc1" >> /etc/hosts
      echo "#{private_net}.252	dc2.ad.lab.home	dc2" >> /etc/hosts
      echo "domain lab.home" > /etc/resolv.conf
      echo "search lab.home ad.lab.home" >> /etc/resolv.conf
      echo "nameserver #{private_dns1}" >> /etc/resolv.conf
      echo "nameserver #{private_dns2}" >> /etc/resolv.conf
      echo "nameserver 10.0.2.3" >> /etc/resolv.conf
    SHELL
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "playbook.yml"
    ansible.groups = {
       "linux" => ["rh7","rh8","rh9","deb10","deb11","ubu18","ubu20","ubu22"],
       "linux:vars" => { "hostdomain" => "lab.home" }
    }
    ansible.host_vars = {
       "rh7" => { "hostprivrole" => "priv_rh7_admins" },
       "rh8" => { "hostprivrole" => "priv_rh8_admins" },
       "deb10" => { "hostprivrole" => "priv_deb10_admins" },
       "deb11" => { "hostprivrole" => "priv_deb11_admins" },
    }
  end
  config.vm.synced_folder ".", "/home/vagrant/sync", type: "rsync", disabled: true

end
