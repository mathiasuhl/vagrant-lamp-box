# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant::Config.run do |config|
  config.vm.box = "precise32"
  config.vm.box_url = "http://files.vagrantup.com/precise32.box"

  config.vm.forward_port 80, 8080
  config.vm.forward_port 3306, 8889

  config.vm.share_folder "app", "/var/www", "app/", :create => true, :nfs => true
  config.vm.network :hostonly, "192.168.255.2"

  config.vm.provision :chef_solo do |chef|
    chef.cookbooks_path = ["cookbooks"]
    chef.add_recipe "apt"
    chef.add_recipe "git"
    chef.add_recipe "mysql::server"
    chef.add_recipe "oh_my_zsh"
    chef.add_recipe "imagemagick"
    chef.add_recipe "imagemagick::devel"
    chef.add_recipe "nodejs"
    chef.add_recipe "vim"
    chef.add_recipe "php"
    chef.add_recipe "apache2"


    chef.json.merge!({
      mysql: {
        client: {
          packages: [
            "mysql-client",
            "libmysqlclient-dev",
          ]
        },
        server_root_password: "123",
        server_repl_password: "123",
        server_debian_password: "123",
        allow_remote_root: true,
        port: 3306,
        bind_address: '0.0.0.0'
      },
      oh_my_zsh: {
        users: [{
          login: 'vagrant',
          theme: 'arrow',
          plugins: ['cake', 'git']
        }]
      },
      run_list:[
        "recipe[mysql::server]",
        "recipe[mysql::client]",
        "recipe[php::module_mysql]",
      ]
    })

  end
end
