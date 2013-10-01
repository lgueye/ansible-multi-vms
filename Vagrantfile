app_name = 'limber'

Vagrant.configure('2') do |config|

  config.vm.box = 'ubuntu-server-12.10'
  config.vm.box_url = 'http://goo.gl/wxdwM'
  config.vm.provision :hosts

  config.vm.provider :digital_ocean do |provider, override|
    override.ssh.private_key_path = '~/.ssh/louis.gueye@figarocms.fr_rsa'
    provider.client_id = ENV.fetch('DO_CLIENT_ID') { abort 'add DO_CLIENT_ID' }
    provider.api_key = ENV.fetch('DO_API_KEY') { abort 'add DO_API_KEY' }
    provider.image = 'Ubuntu 13.04 x64'
    provider.size = '1GB'
    provider.region = 'Amsterdam 1'
    provider.ansible.inventory_path = 'ansible_hosts'
  end

  roles = {
      'data' => {'hosts' => [{'name' => 'limberdata'}]},
      'appserver' => {'hosts' => [{'name' => 'limberappserver0'}, {'name' => 'limberappserver1'}]},
      'search' => {'hosts' => [{'name' => 'limbersearch'}]},
      'proxy' => {'hosts' => [{'name' => 'limberproxy'}]}
  }

  roles.each_pair { |role, value|
    value['hosts'].each { |host|
      hostname = host['name']

      config.vm.define hostname do |cfg|
        cfg.vm.hostname = hostname

        cfg.vm.provider :digital_ocean do |docean|
          docean.name = hostname
        end

        cfg.vm.provision 'ansible' do |ansible|
          ansible.playbook = "ansible/#{role}.yml"
        end

      end

    }
  }
end
