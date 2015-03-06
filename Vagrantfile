# Use rbconfig to determine if we're on a windows host or not.
require 'rbconfig'
is_windows = (RbConfig::CONFIG['host_os'] =~ /mswin|mingw|cygwin/)

Vagrant.configure("2") do |config|
  config.vm.box       = "precise64"
  config.vm.box_url   = "http://files.vagrantup.com/precise64.box"

  if is_windows
    # Share folder only under windows
    config.vm.synced_folder "priv/ansible", "/ansible"
    
    # Provisioning configuration for shell script.
    config.vm.provision "shell" do |sh|
      sh.path = "priv/win.sh"
      sh.args = "teachbase_dev.yml dev_hosts"
    end
  else
    config.vm.provision "ansible" do |ansible|
      ENV["ANSIBLE_CONFIG"] = "priv/ansible/ansible.cfg"
      ansible.playbook = "priv/ansible/teachbase_dev.yml"
      ansible.inventory_path = "priv/ansible/dev_hosts"
      ansible.tags = ENV["TAGS"]
      ansible.skip_tags = ENV["SKIP_TAGS"]
      ansible.verbose = 'v'
      ansible.limit = "all"
    end
  end

  config.vm.network :forwarded_port, host: 2201, guest: 22 
  config.vm.network "forwarded_port", guest: 3000, host: 3001
  config.vm.network "forwarded_port", guest: 80, host: 8080

  config.vm.network "private_network", ip: "192.168.50.111"

  config.vm.synced_folder ".", "/vagrant", :disabled => true
  config.vm.synced_folder "src/", "/webapps/teachbase", create: true, id: "vagrant-root",
    owner: "vagrant",
    group: "www-data",
    mount_options: ["dmode=775,fmode=777"]
end