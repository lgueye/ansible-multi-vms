Vagrant.configure('2') do |config|

  config.vm.provider :digital_ocean do |provider, override|
    override.ssh.private_key_path = '~/.ssh/louis.gueye@figarocms.fr_rsa'
    override.vm.box = 'digital_ocean'
    override.vm.box_url = 'https://github.com/smdahlen/vagrant-digitalocean/raw/master/box/digital_ocean.box'

    provider.client_id = ENV.fetch('DO_CLIENT_ID') { abort "add DO_CLIENT_ID" }
    provider.api_key = ENV.fetch('DO_API_KEY') { abort "add DO_API_KEY" }
  end
end