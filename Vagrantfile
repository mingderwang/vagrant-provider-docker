ENV['VAGRANT_DEFAULT_PROVIDER'] ||= 'docker'

Vagrant.configure("2") do |config|
 
  config.vm.provider "docker" do |d|
 
    d.image = "phusion/baseimage:0.9.15" 
    #d.ports = ["22:22"]
    d.has_ssh = true
 
    # Is this necessary if EXPOSE is used in Dockerfile?
    d.expose = ["22"]
    d.remains_running = true
    d.volumes = ["/shared"]
    d.cmd = ["/sbin/my_init", "--enable-insecure-key"]
 
  end
 
  config.vm.network :private_network, ip: "10.0.1.129"
  config.vm.hostname = "gru"
  #config.vm.network "forwarded_port", guest: 2222, host: 22
  config.vm.synced_folder "~/Documents/shared", "/shared"

  config.vm.provision "chef_solo" do |chef|
    chef.add_recipe "apache"
  end 
end
