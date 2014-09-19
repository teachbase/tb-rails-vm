Vagrant.configure("2") do |config|
  config.vm.box       = "precise64"
  config.vm.box_url   = "http://files.vagrantup.com/precise64.box"

  config.vm.provider "virtualbox" do |v|
  	v.memory = 1024
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "priv/ansible/teachbase_dev.yml"
    ansible.inventory_path = "priv/ansible/dev_hosts"
    ansible.verbose = 'v'
  end

  config.vm.network "forwarded_port", guest: 3000, host: 3001
  config.vm.network "private_network", ip: "192.168.50.111"

  config.vm.synced_folder ".", "/vagrant", :disabled => true
  config.vm.synced_folder "src/", "/webapps/teachbase", create: true, id: "vagrant-root",
    owner: "vagrant",
    group: "www-data",
    mount_options: ["dmode=775,fmode=664"]
  config.vm.synced_folder "../pieces/pieces-rails/", "/webapps/teachbase/pieces-rails", create: true,
    id: "vagrant-root",
    owner: "vagrant",
    group: "www-data",
    mount_options: ["dmode=775,fmode=664"]

  config.vm.synced_folder "../pieces/pieces-rails", "/webapps/teachbase/pieces-rails", create: true, id: "vagrant-root",
    owner: "vagrant",
    group: "www-data",
    mount_options: ["dmode=775,fmode=664"]

end