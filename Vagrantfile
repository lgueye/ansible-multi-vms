Vagrant.configure('2') do |config|

  config.vm.provider :digital_ocean do |provider, override|
    override.ssh.private_key_path = '~/.ssh/louis.gueye@figarocms.fr_rsa'
    override.vm.box = 'digital_ocean'
    override.vm.box_url = 'https://github.com/smdahlen/vagrant-digitalocean/raw/master/box/digital_ocean.box'

    provider.client_id = ENV.fetch('DO_CLIENT_ID') { abort "add DO_CLIENT_ID" }
    provider.api_key = ENV.fetch('DO_API_KEY') { abort "add DO_API_KEY" }
  end
end

app_name = 'limber'

Vagrant.configure("2") do |config|

  config.vm.box = 'ubuntu-server-12.10'
  config.vm.box_url = 'http://goo.gl/wxdwM'
  config.vm.provision :hosts

  config.vm.provider :digital_ocean do |provider, override|
    override.ssh.private_key_path = '~/.ssh/id_rsa'
    provider.client_id = ENV.fetch('DO_CLIENT_ID') { abort "add DO_CLIENT_ID" }
    provider.api_key = ENV.fetch('DO_API_KEY') { abort "add DO_API_KEY" }
    provider.image = 'Ubuntu 13.04 x64'
    provider.size = '1GB'
    provider.region = 'ams1'
  end

  # Database
  data_role = 'data'
  app_data_host = "#{app_name}#{data_role}"
  config.vm.define app_data_host do |mysql|
    mysql.vm.hostname = app_data_host
    mysql.vm.network :private_network, ip: '192.168.10.2'

    mysql.vm.provider :digital_ocean do |docean|
      #docean.customize ['modifyvm', :id, '--memory', '1024']
      docean.name = "#{mysql.vm.hostname}"
    end

    mysql.vm.provision 'ansible' do |ansible|
      ansible.playbook = "#{app_name}-#{data_role}.yml"
    end
  end

  #App servers
  appserver_role = 'appserver'
  app_appserver_host = "#{app_name}#{appserver_role}"
  appservers = []
  2.times.map { |i|
    appservers.push(
        {
            'hostname' => "#{app_appserver_host}#{i}",
            'ipaddress' => "192.168.10.#{ i + 10}"
        }
    )
  }

  appservers.each_index do |index|
    current_server = appservers[index]['hostname']
    config.vm.define current_server do |jetty|
      jetty.vm.hostname = current_server
      jetty.vm.network :private_network, ip: appservers[index]['ipaddress']

      jetty.vm.provider :digital_ocean do |docean|
        docean.name = "#{jetty.vm.hostname}"
      end

      jetty.vm.provision 'ansible' do |ansible|
        ansible.playbook =  "#{app_name}-#{appserver_role}.yml"
      end
    end
  end

  # Proxy
  proxy_role = 'proxy'
  app_proxy_host = "#{app_name}#{proxy_role}"
  config.vm.define app_proxy_host do |proxy|
    proxy.vm.hostname = app_proxy_host
    proxy.vm.network :private_network, ip: '192.168.10.5'

    proxy.vm.provider :digital_ocean do |docean|
      docean.name = "#{proxy.vm.hostname}"
    end

    proxy.vm.provision 'ansible' do |ansible|
      ansible.playbook =  "#{app_name}-#{proxy_role}.yml"
    end
  end

  # Search engine
  search_role = 'search'
  app_search_host = "#{app_name}#{search_role}"
  config.vm.define app_search_host do |es|
    es.vm.hostname = app_search_host
    es.vm.network :private_network, ip: '192.168.10.6'

    es.vm.provider :digital_ocean do |docean|
      docean.name = "#{es.vm.hostname}"
    end

    es.vm.provision 'ansible' do |ansible|
      ansible.playbook =  "#{app_name}::#{search_role}"
    end
  end

end
