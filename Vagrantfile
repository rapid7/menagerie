# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure('2') do |config|
  config.vm.box = 'ubuntu-14.04-provisionerless'
  config.vm.box_url = 'https://cloud-images.ubuntu.com/vagrant/trusty/'\
    'current/trusty-server-cloudimg-amd64-vagrant-disk1.box'

  # config.vm.provider :virtualbox do |vb|
    # vb.memory = 2048
  # end

  config.omnibus.chef_version = :latest
  config.berkshelf.enabled = true
  config.berkshelf.berksfile_path = './cookbook/Berksfile'
  config.cache.scope = :box if Vagrant.has_plugin?('vagrant-cachier')

  config.vm.hostname = 'menagerie-0'
  # config.vm.network :private_network, :ip => "192.168.254.#{ i + 2 }"
  config.vm.network :forwarded_port, :host => 2381, :guest => 2381
  config.vm.network :forwarded_port, :host => 2379, :guest => 2379

  config.vm.provision :chef_solo do |chef|
    chef.json = {
      :etcd => {
        # :listen => "192.168.254.#{ i + 2 }"
        :listen => '0.0.0.0'
      },
      :menagerie => {
        :source => '/vagrant'
      }
    }
    chef.run_list = ['recipe[menagerie::default]']
  end
end
