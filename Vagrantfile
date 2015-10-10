Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/trusty64"
  # config.vm.network "forwarded_port", guest: 80, host: 8080
  # config.vm.network "private_network", type: "dhcp" 
  # config.vm.network "public_network"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #

  config.vm.provision "shell", inline: <<-SHELL
     sudo apt-get update
     sudo apt-get install -y glusterfs-server
     sudo cp /vagrant/hosts /etc/hosts
     gluster peer probe tor0
  SHELL

  config.vm.define "tor0" do |tor0|
     tor0.vm.hostname = "tor0"
     tor0.vm.network "private_network", ip: "172.16.172.254"
     tor0.vm.provision "shell", inline: <<-SHELL
	apt-get install -y dnsmasq tftpd
	cp /vagrant/dnsmasq-tor.cfg /etc/dnsmasq.d/
	service dnsmasq restart
        cp /vagrant/xinetd-tftpd /etc/xinetd.d/tftpd
       	service xinetd restart 
	mkdir /tftpboot
	chmod 766 /tftpboot
	chown nobody /tftpboot
	cp /vagrant/undionly.kpxe /tftpboot/
	cp /vagrant/boot/* /tftpboot/
     SHELL
  end

  config.vm.define "tor1" do |tor1|
     tor1.vm.hostname = "tor1"
     tor1.vm.network "private_network", ip: "172.16.172.253"
     
     tor1.vm.provision "shell", inline: <<-SHELL
	apt-get install -y dnsmasq tftpd
	cp /vagrant/dnsmasq-tor.cfg /etc/dnsmasq.d/
	service dnsmasq restart
        cp /vagrant/xinetd-tftpd /etc/xinetd.d/tftpd
       	service xinetd restart 
	mkdir /tftpboot
	chmod 766 /tftpboot
	chown nobody /tftpboot
	cp /vagrant/undionly.kpxe /tftpboot/
	cp /vagrant/boot/* /tftpboot/
     SHELL
  end
end
