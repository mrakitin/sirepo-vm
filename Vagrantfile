Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-22.04"
  config.vm.box_check_update = true

  config.vm.network "private_network", ip: "192.168.56.0"
  config.vm.network "forwarded_port", guest: 8000, host: 8000, host_ip: "127.0.0.1"  # httpd port
  config.vm.network "forwarded_port", guest: 8080, host: 8080, host_ip: "127.0.0.1"  # nginx port for /
  config.vm.network "forwarded_port", guest: 8081, host: 8081, host_ip: "127.0.0.1"  # nginx port for /sirepo
  config.vm.network "forwarded_port", guest: 7000, host: 7000, host_ip: "127.0.0.1"  # uwsgi port

  config.vm.synced_folder ".", "/repo"

  config.vm.provider "virtualbox" do |vb|
    vb.name = "sirepo-raydata-vm"
    vb.gui = false
    # vb.memory = "4096"
    # vb.cpus = 4
  end

  config.ssh.forward_agent = true
  config.ssh.forward_x11 = true

  $script_root = "/bin/bash --login /vagrant/setup-under-root.sh"
  config.vm.provision "shell", privileged: true, inline: $script_root

  # Reboot between the sudo and regular user steps to pick up the new 'docker' group.
  #   https://developer.hashicorp.com/vagrant/docs/provisioning/shell#reboot
  config.vm.provision "shell", reboot: true

  # $script_vagrant = "/bin/bash --login /vagrant/setup-under-vagrant.sh"
  # config.vm.provision "shell", privileged: false, inline: $script_vagrant

end