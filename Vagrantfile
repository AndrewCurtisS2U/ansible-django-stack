# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "ubuntu/trusty64"

  config.vm.network :private_network, ip: "192.168.33.15"

  config.ssh.private_key_path = "~/.ssh/id_rsa"

  config.vm.provider :virtualbox do |vb|
    vb.customize ["modifyvm", :id, "--name", "MyCoolApp", "--memory", "512"]
  end

  config.vm.provider :rackspace do |rs|
    rs.username         = ENV['rax_username']
    rs.api_key          = ENV['rax_api_key']
    rs.rackspace_region = ENV['rax_reg']
    rs.flavor           = /512MB Standard/
    rs.image            = /Ubuntu 14.04/
    rs.public_key_path = "~/.ssh/id_rsa.pub"
  end

  # Shared folder from the host machine to the guest machine. Uncomment the line
  # below to enable it.
  #config.vm.synced_folder "../../../my-cool-app", "/webapps/mycoolapp/my-cool-app"

  config.vm.define :"staging.special2.us" do |stage|
    stage.vm.hostname = "staging.special2.us"

    # Ansible provisioner.
    stage.vm.provision "ansible" do |ansible|
      ansible.host_key_checking = false
      ansible.verbose = "v"
      ansible.vault_password_file = "vault_passwords.txt"
      ansible.playbook = "staging.yml"
    end

  end

  config.vm.define :"special2us_production" do |stage|
    stage.vm.hostname = "special2us.com"

    # Ansible provisioner.
    stage.vm.provision "ansible" do |ansible|
      ansible.host_key_checking = false
      ansible.verbose = "v"
      ansible.vault_password_file = "vault_passwords.txt"
      ansible.playbook = "production.yml"
    end

  end
end
