app_name = 'limber'

Vagrant.configure('2') do |config|

  config.vm.box = 'ubuntu-13.04'

  config.hostmanager.enabled=true
  config.hostmanager.manage_host= true
  config.vm.provision :hostmanager

  config.vm.provider :lxc do |lxc|
    lxc.vm.box = 'ubuntu-13.04'
    #lxc.vm.box_url = 'http://bit.ly/vagrant-lxc-raring64-2013-09-28-'
    lxc.customize 'cgroup.memory.limit_in_bytes', '512M'
  end

  roles = ['data', 'appserver', 'search', 'proxy']
  app_servers_count = 2
  hosts = {}
  roles.each do |role|
    hostname = "#{app_name}-#{role}"
    hosts[role] = {'hosts' => [{'name' => hostname}]}
    if role == 'appserver'
      hosts[role] = {'hosts' => []}
      app_servers_count.times.map do |i|
        hosts[role]['hosts'].push ({'name' => "#{hostname}-#{i}"})
      end
    end
  end

  hosts.each_pair do |role, value|
    value['hosts'].each do |host|
      hostname = host['name']
      config.vm.define hostname do |cfg|
        cfg.vm.hostname = hostname
        # shell provisioner
        cfg.vm.provision "shell", inline: 'apt-get update ; apt-get upgrade -y; apt-get autoclean'
        cfg.vm.provision "shell", inline: 'apt-get install -y python python-apt python-pycurl'
        # ansible provisioner
        cfg.vm.provision 'ansible' do |ansible|
          ansible.inventory_file = "ansible_hosts"
          ansible.playbook = "ansible/#{role}.yml"
        end
      end
    end
  end

end
