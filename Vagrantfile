require 'rbconfig'

host = RbConfig::CONFIG['host_os']
is_windows = (host =~ /mswin|mingw|cygwin/)

Vagrant.configure("2") do |config|
  config.vm.box       = "ubuntu/trusty64"

  ## OS Tune

  # Give VM 1/4 system memory & access to all cpu cores on the host
  if host =~ /darwin/
    cpus = `sysctl -n hw.ncpu`.to_i
    # sysctl returns Bytes and we need to convert to MB
    mem = `sysctl -n hw.memsize`.to_i / 1024 / 1024 / 3
  elsif host =~ /linux/
    cpus = `nproc`.to_i
    # meminfo shows KB and we need to convert to MB
    mem = `grep 'MemTotal' /proc/meminfo | sed -e 's/MemTotal://' -e 's/ kB//'`.to_i / 1024 / 3
  else # sorry Windows folks, I can't help you
    cpus = 2
    mem = 1024
  end

  config.vm.provider "virtualbox" do |v|
    v.memory = ENV['MEMORY'] || mem
    v.cpus = ENV['CPU'] || cpus
  end

  ## Provision 

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

  config.vm.network "forwarded_port", host: 2211, guest: 22 
  config.vm.network "forwarded_port", guest: 3000, host: 3001
  config.vm.network "forwarded_port", guest: 4000, host: 4001
  config.vm.network "forwarded_port", guest: 9200, host: 9200
  config.vm.network "forwarded_port", guest: 5601, host: 5601
  config.vm.network "forwarded_port", guest: 25672, host: 25672
  config.vm.network "forwarded_port", guest: 15672, host: 15672
  config.vm.network "forwarded_port", guest: 8888, host: 8888
  config.vm.network "forwarded_port", guest: 8086, host: 8086
  config.vm.network "forwarded_port", guest: 8083, host: 8083
  config.vm.network "forwarded_port", guest: 9092, host: 9092
  config.vm.network "private_network", ip: "192.168.50.111"

  config.vm.synced_folder ".", "/vagrant", disabled: true
  config.vm.synced_folder "src/", "/webapps/teachbase", 
    nfs: true,
    create: true,
    id: "vagrant-root"
end
