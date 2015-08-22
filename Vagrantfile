Vagrant.configure("2") do |config|
  config.vm.box_url = 'file://builds/vEOS_4.15.0F_virtualbox.box'

  config.vm.box = "vEOS_4.15.0F"
  #
  # XXX: These options are not supported until vbguest is avaiable
  #config.vm.hostname = "veos"
  config.vm.network :forwarded_port, guest: 22, host: 2224, id: 'ssh'

  # Configure a forwarded port to access eAPI on vEOS
  # https://username:password@localhost:8443/command-api
  config.vm.network :forwarded_port, guest: 443, host: 8443

  # https://github.com/dotless-de/vagrant-vbguest
  # Vagrant => 1.1: vagrant plugin install vagrant-vbguest
  config.vbguest.auto_update = false

  # The sample, below is preconfigured in the basebox
  # Enable eAPI in the EOS config
  config.vm.provision "shell", inline: <<-SHELL
    FastCli -p 15 -c "configure
    username vagrant privilege 15 role network-admin secret vagrant
    username eapi privilege 15 role network-admin secret eapi
    management api http-commands
      no shutdown
    end
    copy running-config startup-config"
  SHELL

  # setup ansible shell
  config.vm.provision "shell", inline: <<-SHELL
    useradd -d /persist/local/ansible -G eosadmin ansible
    echo password | sudo passwd --stdin ansible
    mkdir /persist/local/ansible/.ssh
    chmod 700 /persist/local/ansible/.ssh
    chown ansible:eosadmin /persist/local/ansible/.ssh
  SHELL
end
